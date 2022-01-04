In this new video, we will study an important Prolog concept: *unification*.

# Unification
We have already seen in a previous video that Prolog, to answer a query, scans the program (database) from the top to the bottom to find a clause that matches with the current goal.

Prolog uses a from of matching called *unification*.

Generally speaking, unification consists in making two terms syntactically equal by replacing variables with constants.
The involved terms must have the same functor and the same arity.

If the two terms are constants, these can *unify* only if they are equal.

The used operator to unify two terms is `=`.
Note that `=` is not used to perform arithmetic operations.

Initially, all variables are *unbound*. 
Once a variable is assigned to a value, it is named *bound*.
Once a variable is bound, we can change its value only through *backtracking* (that we will see in another video).

Consider these examples:
```Prolog
?- house = house.
true

?- house = abc.
false

?- edge(a,b) = edge(C,b).
C = a.

?- edge(a,d) = edge(C,B).
C = a,
B = d.

?- edge(C,B) = edge(a,d).
C = a,
B = d.

?- edge(A,f(g,h)) = edge(a,B).
A = a,
B = f(g,h).

?- edge(A,A) = edge(b,c).
false

?- edge(A,A) = edge(b,b).
A = b.

?- A = B, B = f.
A = B, B = f.

?- A = B, B = f, A = e.
false
```