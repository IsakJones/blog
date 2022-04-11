
---
layout: post
title:  "Anonymous Functions vs. Lambda Expressions"
date:   2022-03-24 18:20:49 +0100
categories: programming functional-programming
---
<!--end_of_excerpt--> 

In this blog post, I argue that there's a substantive difference between anonymous functions and lambda expressions. In particular, lambda expressions only contain one expression, and are a subset of anonymous functions, which may contain any number of expressions or statements.

## Lambda expressions

A lambda expression is quite a specific type of anonymous function. I would define an anonymous function as any function consisting of one single expression with at least one free variable evaluating to a new value or expression when the free variable is substituted with a value.

That was a mouthful. But lambda expressions are usually quite simple. 

To illustrate them, I'll use a few examples in Haskell (syntactically, `\x` or anything else following the backslash lists the free variables, and everything following the `->` arrow makes up the resulting expression).

```haskell
mapDouble :: Num a => [a] -> [a]
mapDouble = map (\x -> x * 2)
```

The above function doubles all elements of a list by mapping the lambda expression, which takes `x` as a free variable and consists of `x * 2`. When a value, say the number `3`, is substituted for the free variable, then a new value, in this case `6`, is generated: `{x * 2}(3) = 3 * 2 = 6`. So, when `mapDouble` is applied to the list `[1, 2, 3, 4]`, the following expressions are evaluated:

```haskell
mapDouble [1, 2, 3, 4] = [
    {x*2}(1) = 1 * 2 = 2,
    {x*2}(2) = 2 * 2 = 4,
    {x*2}(3) = 3 * 2 = 6,
    {x*2}(4) = 4 * 2 = 8,
]
```

## Varieties of anonymous functions

An anonymous function is just a function without a name, where "function" can be intended as generally as possible. So there are really 

```javascript

const arr = [1, 2, 3]

const mappedArr = arr.map(


```
