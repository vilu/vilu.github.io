---
title: Free monads
summary: Free monads what are they good for?
---

## Why free monads?

### The expression problem

Adding new types to Expr would force us to adjust all functions where we have defined in terms of Expr. 

```
data Expr = Val Int | Add Expr Expr

evaluate :: Expr -> Int
evaluate (Val x) = undefined
evaluate (Add e1 e2) = undefined
```

Philip Wadler called this the expression problem

"The goal is to define a data type by cases, where one can add new cases to the data type
and new functions over the data type, without recompiling existing code, and while retaining
static type safety."

Using Free monads is one way of solving this problem. Free monads allow you to define data types, functions and some monads avoiding the constraints of the original problem.



### Let's use them

So how should the datatype look of an expression look? If we explicitly use the constructors of the cases in the expression datatype we would not to solve our original problem because of the tight coupling between. Instead we parameterize over the constructor with "f".
```
data Expr f = In (f (Expr f)) 

data Val e = Val Int
type IntExpr = Expr Val

data Add e = Add e e
type AddExpr = Expr Add
```

We have now solved one part of the expression problem. By decoupling the constructors of the actual expressions from the `Expr` type we can now easily add new expressions without breaking our current program. However, this in isolation would be useless. One of the issues we now have is that we have isolated the expressions so much that we can't really use them together. To join them together we create yet another datatype from the coproduct of `Val` and `Add`. 

```
data (f :+: g) e = Inl (f e) | Inr (g e)

addExample :: Expr (Val :+: Add)
addExample = In (Inr (Add (In (Inl (Val 118))) (In (Inl (Val 1219)))))
```


### Bin

https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#type-operators

### Flavours?

- Free monads
- Freer monads
- Tagless final




### Sources

Data types a la carte : http://www.cs.ru.nl/~W.Swierstra/Publications/DataTypesALaCarte.pdf

http://okmij.org/ftp/Computation/free-monad.html
