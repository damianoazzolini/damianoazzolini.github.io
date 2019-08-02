---
layout: post
title: Compile GMP with emscripten
date: 2019-07-17
categories: miscellaneus
---

GMP is one of the most known libraries to manipulate large primes.
It usually works with C and C++ code. However, it could be interesting
to compile it with emscripten in order to run it on a web page. 
In this brief guide i show you how i was able to compile it on
Ubuntu 16.04 x89_64. I've found few resoures on internet on how
to do it but none of them really worked for me... 

TL;DR: if you are impatient, just look at the [gist](https://gist.github.com/damianoazzolini/42ceb8e1473750dbf07b1ab0ed75a0a9) i've created.

## Instructions
First of all, download GMP from [the website](https://gmplib.org/).
You can use, for instance, `wget` from the command line in this way:

```
> wget https://gmplib.org/download/gmp/gmp-6.1.2.tar.lz 
```

In order to extract the `.tar.lz` file you need to install `lzip` with
`sudo apt install lzip`. Then, extract it with:

```
tar --lzip -xf gmp-6.1.2.tar.lz
```
Now, move into the folder with `cd gmp-6.1.2` and execute the classical
procedure `./configure`, `make`, `make check`, `sudo make install`. 
If you want to better understand how these three commands work, [read this post](https://thoughtbot.com/blog/the-magic-behind-configure-make-make-install).

If everything works fine, move on to install emscripten.
Clone it form the repository with `git clone https://github.com/emscripten-core/emsdk.git`.
Then move into the folder `emsdk` with `cd emsdk`. Then run:

```
> ./emsdk install latest 
> ./emsdk activate latest
> source ./emsdk_env.sh
```

Ok, now move into the folder where gmp was compiled and then type:
```
> ./configure ABI=64  "CFLAGS=-m64" "CXXFLAGS=-m64" "LDFLAGS=-m64"
> EMCONFIGURE_JS=1 emconfigure ./configure ABI=64 --disable-assembly CFLAGS=-m64 CXXFLAGS=-m64 LDFLAGS=-m64                 
> EMMAKEN_CFLAGS="-g" emmake  make -j 2
```
If you want to know what the flags mean, [look at emscripten documentation](https://emscripten.org/docs/tools_reference/emcc.html).

Apart from few warnings, everything should compile properly.
To test it, create a file called `test.c` with the content:

```
#include <stdio.h>
#include "gmp.h" // be careful with the position of gmp.h

 int main() {
   mpz_t a, b;
   mpz_init_set_str (a, "10", 10);    
   mpz_init_set_str (b, "10", 10);     
   mpz_add (a, a, b);             

  printf("%s + %s => %s\n", "10", "10", mpz_get_str (NULL, 10, a));
  return 0;
}
```

Two things to notice:
* I've included GMP with `#include "gmp.h"` because i've not added it to the path. In this case, `test.c` is in the same folder of `gmp.h` (the root folder of gmp).
* It is important to add `\n` at the end of a line in order to print it on thew console later.

Compile `test.c` with: 
```
> emcc -O2 --closure 0 test.c .libs/libgmp.a -o test.js -s ASM_JS=1 -g --llvm-lto 1 --bind
```
Also in this case, note that we are in the same folder where `.libs` is (the root folder of gmp).

Finally, run it with:
```
> node test.js
```

## Conclusions
Compile the library was no simple task and requires some fine tuning.
I'm still working on how to make the file interact also with the stdin, so for example, to be able to add a `scanf` statement on the `.c` file and make the console waiting for the input. I've found fer stuff on the web that try to solve this problem, however, this seems no to be directly supported and [requires some workarounds](https://emscripten.org/docs/api_reference/Filesystem-API.html#setting-up-standard-i-o-devices).

Hope this brief post helps someone.