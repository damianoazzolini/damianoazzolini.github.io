---
layout: post
title: Querying Prolog
date: 2021-12-24
categories: prolog
---
In this new blog post, we will see how to interact with Prolog programs.
<!-- In this new video, we will see how to interact with Prolog programs. -->

# Asking Queries
<!-- In a [previous post](https://damianoazzolini.github.io/prolog/2021/12/22/PrologGlossary.html) we have seen the basic Prolog terminology. -->
<!-- In particular, a Prolog program is a set of rules. -->
<!-- Recall that rules without a body are called facts. -->

Consider these two facts
```
train(milan,paris).
train(paris,madrid).
```

The first fact states that there is a train that connects `milan` and `paris`.
The second fact states that there is a train that connects `paris` and `madrid`.
How can we interact with this program?

First, we need to install a Prolog interpreter.
There are many free interpreters:
- [SWI-Prolog](https://www.swi-prolog.org/) that also has a web interface called [SWISH](https://swish.swi-prolog.org/)
- [XSB Prolog](http://xsb.sourceforge.net/)
- [tau-Prolog](http://tau-prolog.org/)
- [Scryer Prolog](https://github.com/mthom/scryer-prolog)
- [Trealla Prolog](https://github.com/infradig/trealla)
- [GNU Prolog](https://github.com/didoudiaz/gprolog)
- and many more

For these examples, they are all equivalent.
We will adopt SWI-Prolog, since it also has a web interface where you can easily reproduce these programs.

As first operation, we write the two `train/2` facts into a file called `train.pl`.
If you use a web interface, you can just write the facts in the web page.
Using SWI, we can open the interpreter from the command line with `swipl`.
Once the interpreter is opened, we can load our file with `[train].` (the final dot is important).
You should see
```
Welcome to SWI-Prolog (threaded, 64 bits, version 8.5.2-8-geb4ab47)
SWI-Prolog comes with ABSOLUTELY NO WARRANTY. This is free software.
Please run ?- license. for legal details.

For online help and background, visit https://www.swi-prolog.org
For built-in help, use ?- help(Topic). or ?- apropos(Word).

?- [train].
true.
?- 
```
The pair of symbols `?-` states that the interpreter is ready for a new command. 

We can now make a request to the interpreter (actually, this is our second request, since the first is the one used to load the file).
A request to the interpreter is also called *query*.

For example, ``is there a train between milan and paris?''
In code, this translates into:
```
?- train(milan,paris).
true.
```
So is `true` that there is a train between `milan` and `paris`. 
Another query: ``Is there a train between milan and madrid?''
```
?- train(milan,madrid).
false.
```
Is `false` that there is a train between `milan` and `madrid`: *closed world assumption*, what it is not explicitly stated is false.

Another request: ``is there a location that can be reached starting from milan?''
In code:
```
?- train(milan,X).
X = paris.
```

We introduced a variable (`X`) in our query, so the interpreter will look for all the terms that can *unify* with `X`.
In other words, we look for all the possible `X`s that make `train(milan,X)` true (in this case, a fact into our program).
Note that the symbol `=` is not used to perform arithmetic sum.

We add a third fact into the program: `train(milan,rome)`.
Our program now looks like this:
```
train(milan,paris).
train(paris,madrid).
train(milan,rome).
```

If we ask again `?- train(milan,X)` we get
```
?- train(milan,X).
X = paris
```

You can see that the interpreter is not ready, since it has not printed `.` after `paris`.
This means that there can be other possible solutions (or, in Prolog terminology, *choice point*).
To retrieve them, we can press `;`. 
The result will be:
```
?- train(milan,X).
X = paris ;
X = rome.
```
So, both `paris` and `rome` are two possible solutions.

We now swap the facts `train(milan,paris)` and `train(milan,rome)`, and the program looks like this:
```
train(milan,rome).
train(paris,madrid).
train(milan,paris).
```

If we ask again `?- train(milan,X)` we get
```
?- train(milan,X).
X = rome ;
X = paris.
```

What is the difference with the previous output? `rome` and `paris` are swapped.
Now `rome` comes before `paris` while previously `paris` was the first solution and `rome` the second.

What does this mean? That in Prolog *the order of the clauses (in this case rules) matters*.
This is an important feature because, for some programs, a different order of the clauses may result in a query that terminates or loops forever.

Consider this simple extension:
```
train(milan,rome).
train(paris,madrid).
train(milan,paris).

connected(A,B):- train(A,B).
```
With this new rule `connected(A,B)` is true if  `train(A,B)` is true.

We can ask some queries:
```
?- connected(A,B).
A = milan,
B = rome ;
A = paris,
B = madrid ;
A = milan,
B = paris.
```
We get a list of all the connected locations.

To exit the SWI-Prolog terminal, press `Ctrl+D` or write `halt.`
```
?- halt.
```

## Useful References
- https://www.swi-prolog.org/
- https://swish.swi-prolog.org/
- http://xsb.sourceforge.net/
- http://tau-prolog.org/
- https://github.com/mthom/scryer-prolog
- https://github.com/infradig/trealla
- https://github.com/didoudiaz/gprolog