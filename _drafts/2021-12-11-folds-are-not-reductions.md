---
layout: post
title:  "Folds are not reductions"
date:   2022-01-11 18:20:49 +0100
categories: programming functional-programming
---

This is a somewhat pedantic post on a functional programming feature.

Most resources state that folds and reductions are the same thing. You just need to look at the Wikepedia page, or whatever other source.

However, the reductions were originally called so because they reduce the dimensions of a data structure. So, the classic reduction example is the `sum` function:

```haskell
sum' :: Num a => [a] -> a
sum' = foldr (+) 0
```

In this case, `foldr` carries out a reduction because it reduces an array of numbers to a single numeric value. Similarly, mapping `sum` to a 2D matrix reduces a two-dimensional array to a one-dimensional array. And so on - any N-dimensional array is reduced to a (N-1)-dimensional array.

However, `foldr` need not carry a reduction. In fact, it can be made equivalent to the `map` function:

```haskell
map' :: (a -> b) -> [a] -> [b]
map' f = foldr (\x xs -> f x : xs)
```

In this case, there is no reduction, since the array's dimensions are not decreased.
Trivially, we can write a function with `foldr` that returns an array with more dimensions.

```haskell
higherFold :: [a] -> [[a]]
higherFold n = foldr (\x xs -> replicate n x : xs) []
```

This function takes a one-dimensional array and outputs a two-dimensional array with each original elements repeated n times.

Moral: Haskell functions are quite flexible