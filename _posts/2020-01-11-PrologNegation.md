---
layout: post
title: Negation in Prolog
date: 2019-01-11
categories: prolog
---
In this post we will see how prolog handles negation. I will use SWI-Prolog as program for the examples.

# Negation
Let's start with a simple example that models the weather conditions:
```
weather(cloudy).
temperature(low).

snow:-
    weather(cloudy),
    temperature(low).
```
We have a small database with two facts representing the weather the temperature. Moreover, we have a predicate `snow/0` which is true if the weather is cloudy and the temperature is low. If we run `?- snow` we get `true`. As expected. 
Now we are interested to know if will rain. 
According to our experience, we know that if the weather is cloudy and the temperature is not low it will rain (if the temperature is low, the rain will be snow). How we can encode in our program `the weather is not low`?

Every prolog implementation offers the possibility to negate a goal with the predicate `not/1`, that usually can also be written as `\+/1` (`\+` should be used, according to [SWI](https://www.swi-prolog.org/pldoc/doc_for?object=not/1)). 
In the following examples we will use `not/1` for clarity.
So, our `rain/0` predicate will be: 
```
rain:-
    weather(cloudy),
    \+temperature(low).
```
With `?- rain` we get false, but why?

# Negation as Failure
In prolog, negation is implemented as *negation as failure*. 
This means that, if prolog encounters `not/1` (for example `not(g)`), it tries to prove the goal inside `not/1` (in this case `g`) and then, if the goal (`g`) fails, `not/1` will succeed and viceversa. 
Keep in mind that if the goal inside `not/1` has variables, such as `g(A)`, those variables will not be instantiated. This is intuitive because if `not(g(A))` succeeds, this means that `g(A)` fails and `A` should be instantiated to every term that makes `g/1` fail (which are likely to be infinite). 
For instance, `not(member(A,[a]))` should return in `A` all the elements that are not in the list `[a]`. 

## Implementation
An implementation of `not/1` can be:
```
not(Goal) :- call(Goal), !, fail.
not(Goal).
```
`call/1` calls `Goal` and if this succeeds the predicate fails through `fail/0`. Note that if `Goal` does not terminate, `not(Goal)` will not terminate as well.