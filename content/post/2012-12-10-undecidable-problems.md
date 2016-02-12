---
title: Undecidable Problems
author: joefkelley
layout: post
date: 2012-12-11
url: /111
sfw_pwd:
  - O7rVeiZcP9JX
categories:
  - Math
  - Programming

---
One of the most surprising things I learned in my freshman year studying computer science was that there are certain mathematical/computational problems that can be very simply, concretely, and exactly stated, yet cannot be solved. These problems are called "undecidable" problems, and to me it seems counterintuitive that they should exist, and yet, they very clearly do, and I'll go over one of the most interesting ones in this post.

First, for readers without a background in theoretical computer science, let's clear up some terms. When I say a "problem", I just mean a function, or maybe a question, that, for any specific input, has a specific and well-defined output. For example, "what is the square of a number?" or "is a number odd?" are simple examples of problems. A more complex and well-studied example is the "traveling salesman problem", which is: "given a list of cities, and the distances between them, find the shortest route that visits each city." All of these are very well-defined, with clear mathematical answers. And they are all decidable, because we can come up with simple algorithms for computing them. The first two are obvious, but for the traveling salesman, we can enumerate all possible orderings of the cities, calculate the total distance of each ordering, and pick the minimum - a simple, albeit slow, algorithm.

An "undecidable" problem is one for which no such algorithm exists. This does not mean that we don't know of one, but may in the future find one; this means that none exists at all, and none will ever exist. Think about that for a second... there are several known examples of simple mathematical questions, for which answers always exist (they are well-defined), and yet we cannot find an algorithm for computing those answers, no matter how clever we are.

Perhaps the most famous undecidable problem is known as "the halting problem" and it is: "given a program and an input, determine whether the program will eventually terminate (halt) or enter an infinite loop". Stated more mathematically, let `\(P\)` be a program, and `\(x\)` be an input to that program. Then we can define the "halting" function, `\(H\)` as: 

<div>$$
H(P,x)=\left\{ \begin{array}{rl}
1 &\mbox{ if P halts on x} \\
0 &\mbox{ otherwise}
\end{array} \right.
$$</div>

Like I said, although this problem is simply stated, and well-defined for all inputs, it is impossible to come up with a program to compute it. The proof is one by contradiction: we start by assuming such a program exists. Then, we can define another program, `\(Z\)`, as follows: 

<div>$$
Z(x)=\left\{ \begin{array}{rl}
infinite loop &\mbox{ if H(x,x)=1} \\
halt &\mbox{ otherwise}
\end{array} \right.
$$</div>

Because we are assuming the existence of a program to compute `\(H\)`, we can use that same program in constructing `\(Z\)`, so a program for `\(Z\)` must exist.

But now consider what would happen if we tried to compute `\(Z(Z)\)`. There are two possible scenarios:

1. `\(Z(Z)\)` loops forever. This implies that `\(H(Z,Z)=1\)`, but by the definition of `\(H\)`, that means that `\(Z(Z)\)` must terminate.
2. `\(Z(Z)\)` terminates. Similarly, this means that `\(H(Z,Z)=0\)`, but that would mean that `\(Z(Z)\)` does not terminate.

In both cases, we have a contradiction, so the assumption must be invalid. It is not possible to construct a program that computes `\(H\)` in all cases.

Now, to be fair, I haven't been totally rigorous. It's worth defining what a "program" is, and what it means to have one program be the input to another program. For example, are programs written in all languages equal? What about ones written on different types of computers? Alan Turing did the work of formalizing all of this for a specific class of program that emulates what today's computers can do, and is considered the father of modern computer science for it. But it also generalizes to other types of programs, even types that we haven't invented yet.

For example, let's say we find an `\(H\)` that only takes as input a specific type of program - call them type `\(A\)` programs. `\(H\)` itself could be a completely new type of program, one that hasn't been invented yet, from a different type, `\(B\)`. Then the proof breaks down; `\(Z\)` doesn't make sense, because it must also be of type `\(B\)`, so it cannot take itself as input, because the new `\(H\)` is not defined over `\(B\)` programs.

However, then we may have solved the halting problem for programs of type `\(A\)`, but we have created a new halting problem for type `\(B\)` programs! In fact, no matter how many different computational classes of programs we come up with, the halting problem will always be undecidable for at least one.

Of course, the most simple consequence of this is pretty straightforward: don't try to write a program that detects infinite loops in other programs. As useful as it might be, it's impossible.

But more importantly, I think the existence of the halting problem and other undecidable problems is philosophically relevant. Computer science is an extremely powerful tool, but it has clear limits. It's interesting that we've found similar hard limits in other disciplines (e.g. Gödel's incompleteness theorems or the Heisenberg uncertainty principle). On the one hand, I find these limits frustrating; that knowledge is somehow bounded is a little bit scary. But on the other hand, I at least find it exciting to be living in a time where we are pressing right up against these limits, and I'm hopeful that in some cases we might even find a way around a few (ala [compressed sensing][1] beating the [Nyquist-Shannon sampling theorem][2]).

 [1]: http://en.wikipedia.org/wiki/Compressed_sensing
 [2]: http://en.wikipedia.org/wiki/Nyquist–Shannon_sampling_theorem
