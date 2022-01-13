---
layout: post
title:  "Something about Anonymous Functions"
date:   2021-12-14 18:20:49 +0100
categories: programming functional-programming
---
CHECKLIST:

 - Gather sources
 - Write "Imperative Anonymous Functions"
 - Write all successive sections
 - Write introduction
 - Edit lightly
 - Send to Miccah

## Intro

Anonymous functions have been mainstream for a while now. While Java has only recently added anonymous functions in 2014, C# included them in its 3.0 release in 2007, and Python has had them since 1994. In fact, of the top 10 programming languages included in 2021's TIOBE index, all of them sport anonymous functions except for SQL, which is a query language and doesn't allow function definitions, and C and Assembly, both of which have a minimal syntax.

While anonymous functions have been mainstream for a while, I want to argue that they are more subversive than they seem. Non-functional languages usually force programmers to think in terms of *statements* that manipulate state, but anonymous functions usually rely on *expressions* that map a set of values to another. In the first section I'll define what I mean by statements and their centrality in conventional computational thinking. In the second section I'll contrast them to expressions, tracing their historic origin. In the third section I'll show how anonymous functions in mainstream programming languages rely on expressions as opposed to statements. In the conclusion I'll muse on whether the inclusion of more functional features in mainstream languages will actually bring to a "functional" paradigm shift in programming, or whether in fact it's just a natural evolution afforded by ever higher abstraction.

## Statements 

I'll define a statement as a machine instruction that modifies state. Defining state is hard, as it's easy to poke holes into any given definition, but broadly I would define state as a set of persistent values that are physically recorded in a machine. State is essential not only to practical computer architecture, but also to the foundations of computational thinking. 

Alan Turing's famous "universal machine", capable of performing all theoretically possible computations, relied solely on state. The machine is defined as an (infinitely long) strip of tape with uniform slots, and a "head" scanning the tape that can read and print characters on a single slot or shift to the next or previous slot. Most instructions imparted to the machine either move the head left, or right, or print a character, or delete a character; there are also conditional instructions that activate one set of instructions over another based on the value of the current slot. Hence the machine's computations are purely a result of the printed sequence of characters on the tape, and the position of the head along the tape. While the Turing machine is not physical, the sequence of characters in the tape and the head's position are persistant values, so I think they should count as state, hence the Turing machine's computations rely solely on state.

State is also an essential feature of the contemporary Von Neumann computer architecture. Take the following C code:

```c
int main() {
   int a, ln, i;
   a = 10;
   ln = 5;

   int *arr = (int*)malloc(ln * sizeof(int));
   for (i = 0; i < ln; i++)
      *(arr + 1) = a;

   return 1;
}
```

In the main function, the program first allocates space for three ints on the stack. It then physically changes the values of two allocated ints. It continues by allocating space for 5 ints in on the heap, and assigning a value to each of them within a loop, itself relying on the third int. Hence the basic operations of initialization, assignment, and even basic control flow all rely on state. This is essential given that in the Von Neumann architecture the CPU performs computation by manipulating physical values stored in its registers, memory, or storage.

Most importantly, state is often central to programmers' thinking. Some programmers might not even have a general idea of the Von Neumann architecture, but they almost certainly use assignment and control flow in most of their programs. That was definitely the case for me when I started learning programming in Python - the variable was one of the first concepts I remember having to learn, as a store whose value changed during the execution of a program. In fact, when I became better, I remember being puzzled by the idea of a variable whose value stays constant - why would you store a value if you're not going to change it at some point? While my thinking has of course evolved, mutable state and by extension statements mutating said state were central to my thinking. 



OUTLINE:

 - SETUP:
    Why are they such a big deal?
    I personally found them hard to deal with in Python
    Java addition
    summary of reasoning (lambda calculus + statements/subroutines vs expressions)

### Statements and Subroutines

 - Statement: a machine instruction that modifies some state, such as memory.
 - Subroutine: a collection of statements with coordinated execution (I'm being careful here - by saying sequential, I would be excluding subroutines with multiple concurrent threads).
   - Coordination can be sequential, with control flow, or concurrent.
 - Originally for Turing machine, with long tape and shifting head - the only way it performs computation is by printing and erasing characters on the tape.
 - In Von Neumann architecture, values are stored in CPU registers, memory slots, and storage (hard disk).  

### Expressions and Functions

 - While, theoretically, imperative programming relies on statements, i.e. state manipulations to perform computations, functional programming relies on expressions.
 - This again harkens back to the root of modern computational thinking. Alonzo Church derived the same theorem of undecidability as Turing through what he called functions, which included statements.
 - Throughout the rest of this blog, unless specified otherwise, I will refer to functions consisting of expressions as, well, functions. (This is improper in computer science but more accurate mathematically)

$$
\{\;\lambda x\ .\ x^{2}+5x-7\;\}(\ 3\ ) 
$$

 - in front of lambda is an argument, following is the function definition, and outside curly braces is a value that is substituted for function argument
 - In this case, it evaluates to a value, $(3)^{2}+5\cdot(3)-7\; =\; 17$.
 - Expressions need not evaluate to a numerical value; they can also evaluate to another expression, such as in the following example:

$$
\{\;\lambda x\ .\ \lambda y\ .\ x+y\ \}(\ 3\ )\; \Rightarrow\; \lambda y\ .\ 3+y
$$

 - The example includes a simple expression, $x+y$, but when only one argument is passed instead of evaluating to a number it evaluates to another function, in this case when $3$ is passed, the resulting function outputs $3+y$, where $y$ is the function argument. 

### Are statements and expressions actually different?

 - Are functions not subroutines, given that they have to run on the Von Neumann architecture? Yes!
 - Functional programmers are well aware of that, but they believe that the advantage lies not in a fundamentally different implementation, but in the convenience that functional languages allow abstracting from their imperative foundations. 
 - Thinking in expressions is profoundly different.

### Anonymous functions

 - Anonymous functions in OO languages are much more similar to lambda expressions than to anonymous functions in Go, in that the arguments are enumerated first, and the function is defined as an expression. In fact, python even includes the lambda:

 ```python
lambda x: x**2 + 5*x - 7
 ```

 - Intellectual origins of lambda calculus have made it to mainstream languages, and regular programmers have to think not only in terms of subroutines, but also in terms of expressions, and that is the big deal.

 - First I'll present an example of what I'll call an anonymous subroutine.
    - An anonymous function is just any nameless function. Usually used for a one-time function.  The go code below makes for a good example. (JavaScript is another example)

```go
func(x int) int {
   // function body
   y := x*2
   return y+1
}
```
    - In go, they're often used to set adjacent blocks of concurrent code.

```go
go func() {
   // Separate goroutine
   Server.Listen()
}()
// Main goroutine
fmt.Println("...")
```

 - Syntax is very useful to accurately represent concurrent operations without having them separate.
 - But when functional programmers rave about anonymous functions, they mean something more akin to this Java code:

 ```java
x -> (x * 2) + 1;
 ```

 - This I think we should define as a lambda expression

### What a lambda expression isn't

 - Turing had expressed computation as a series of changes to the internal state.
 - I think most programmers are trained to think of computation as such. This is especially true in assembly, where everything that is done is with state.
 (maybe an assembly language snippet?)
 - But I think that Python is similar. Lists are inherently mutable (i.e. Internal state), and modifying lists is expected. 
 - Hence from now on I'll refer to all functions conceptualized as a sequence of changes to internal state as subroutines.



### Expressions amid subroutines
 - Anonymous functions in OO languages are much more similar to lambda expressions than to anonymous functions in Go, in that the arguments are enumerated first, and the function is defined as an expression. In fact, python even includes the lambda:

 ```python
lambda x: x**2 + 5*x - 7
 ```

 - Intellectual origins of lambda calculus have made it to mainstream languages, and regular programmers have to think not only in terms of subroutines, but also in terms of expressions, and that is the big deal.


