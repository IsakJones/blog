---
layout: post
title:  "Tail Recursion, Explained"
date:   2021-12-06 18:20:49 +0100
categories: programming functional
---

The main advantage of tail recursion is that 

## Why use tail recursion?

Tail Call Optimization mainly benefits memory. It allows the expressivity of recursion without the buildup of stack frames that most recursive functions require. Even better, it promises to eliminate the risk of the famed stack overflow. 

Most blogs I've read on the topic first raise an example, then explain the underlying logic. In this post, I'll do the opposite. I'll start by explaining what stack frames are, how they build up during recursive calls, and then show an illustrative example.

## Stack frames

A stack frame is a contiguous region of memory on the call stack corresponding to a function in a program. 

```c
int main(int argc, char* argv[]) {
    int foo;

    return 0;
}
```

```c
int f(int n) {
    if (n == 0) {
        return 0;
    }
    return n + 
}
```

-- Consider a simple function that adds the first N natural numbers
sumRange :: Int -> Int 
sumRange 0 = 0
sumRange n = n + sumRange (n-1)

{--
Evaluates (at the deepest):

sumRange 5 = 5 + (4 + (3 + (2 + (1 + (sumRange 0)))))

That's quite deep. More importantly, that's 5 ints on the stack. Add another few and you'll get a stack overflow.
--}

-- Consider a somewhat more complicated function

sumRange' :: Int -> Int -> Int 
sumRange' 0 acc = acc
sumRange' n acc = sumRange' (n - 1) (acc + n)

{--
Ok, this doesn't look as good...

But look at how it progresses!

sumRange' 5 0
sumRange' 4 5
sumRange' 3 9
sumRange' 2 12
sumRange' 1 14
sumRange' 0 15
15

If your compiler is optimized, each nested stack frame is removed. This means no stack overflow!

Sadly, not all compilers are optimized for this. In fact, most aren't.
--}
