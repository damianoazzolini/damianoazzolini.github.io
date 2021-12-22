---
layout: post
title: Prolog glossary
date: 2021-12-22
categories: prolog
---
In this new blog post, we will see the common terminology adopted in Prolog.
You will note that there are several words that can denote the same element.

# Glossary
The basic unit of information is the *atom*, a sequence of character used to
indicate an element of the domain. 
It is used to indicate constants (not number).
For example,
```
house.
a.
fly.
```
are atoms.

*Variables* in Prolog starts with uppercase letter and, differently form other programming
languages, once they are assigned to a value (in Prolog terminology, *bound*) their value cannot
be changed.

These are examples of variables:
```
Abc
House
Car
```

Another concept is the one of *term*, used to denote variables, constants and compound.
*Compound terms* (we will call these simply terms) are composed by an atom together with one or more arguments.
For example,
```
friend(a,b).
```
is a compound term. 
In this case, it can be interpreted as a relation called `friend` between `a` and `b` (i.e., `a` is friend with `b`).
However, note that not all compounds denote relations.
The name of the term is an atom and it is called *functor*.
The number of arguments is called *arity* and compound terms are compactly referred
with the notation `functor/arity`, so, for this example, `friend/2`.
A term is called *ground* if it does not contain variables.

A Prolog program is composed of a set of *rules*: a rule is composed of a
*head* and a *body* separated by the neck operator `:-`.
The head is an atom or a compound term.
The body can be a conjunction of terms (or empty).
These are examples of rules:
```
grass:- sun.
friend(a,b):- knows(a,b).
```
The intuitive meaning is that: if the body is true then the head is also true.
We can insert multiple terms in the body, by separating them with commas
```
a:- b,c.
```
So, a is true if both b and c are true. 
Comma stands for conjunction.
If the body is empty, the neck operator is omitted and the rule is called *fact*.

A *clause* is a rule or a fact.
Facts denote which is true in the domain of interest since, in Prolog, what is
not explicitly stated is considered false.

## Useful References
- [https://sicstus.sics.se/sicstus/docs/4.3.1/html/sicstus/Glossary.html](https://sicstus.sics.se/sicstus/docs/4.3.1/html/sicstus/Glossary.html)
- [https://www.swi-prolog.org/pldoc/man?section=glossary](https://www.swi-prolog.org/pldoc/man?section=glossary)