---
title: Undecidable Problems II
author: joefkelley
layout: post
date: 2013-01-23
url: /191
sfw_pwd:
  - EmesJ14O9xbH
categories:
  - Math
  - Programming

---
In a [previous post][1], I talked about the Halting Problem, and the fact that it was undecidable- that is, it was impossible to write a program to solve it for all cases. There are a few interesting related problems that may give some understanding as to why this must be the case.

As in the previous post, let:

<div>$$
H(P,x)=\left\{ \begin{array}{rl}  
1 &\mbox{ if P halts on x} \\
0 &\mbox{ otherwise}
\end{array} \right.
$$</div> 

Now I'll consider a simple, but unproven statement, known as Goldbach's conjecture. It is:

> Every even integer greater than 2 can be expressed as the sum of two primes.

So, for some examples, `\(4=2+2\)`, `\(6=3+3\)`, `\(8=5+3\)`, `\(10=5+5\)`, `\(12=7+5\)`, etc.

This has been checked for many numbers, and so far it has held true, but nobody has been able to prove that it is always true, for all even numbers.

However, consider the following program (in pseudo-code):
  


~~~
let n=4
while(TRUE):
	let found=FALSE
	for x in 2 through (n-1):
		if (isPrime(x) and isPrime(n-x)):
			found=TRUE

	if (not found):
		HALT
	else:
		n=n+2

~~~


  
Essentially, this program searches for counter-examples to Goldbach's conjecture. It checks each positive even integer, in order, until it reaches one that cannot be expressed as the sum of two primes.

Now, what if we had a solution to the halting problem? Call the pseudo-code program `\(P_{G}\)`, and consider running `\(H(P_{G},[nothing])\)`. If it returns 1, then we know `\(P_{G}\)` eventually halts, so Goldbach's conjecture is false! And if it returns 0, then we know `\(P_{G}\)` never finishes, and so the conjecture is true!. Simple as that, we've solved one of the longest-standing open problems in mathematics.

In fact, there's a whole host of unsolved problems that a solution to the Halting Problem would make short work of. Some of them you have to be a little bit more clever. For example, consider the Collatz Conjecture:

> Consider a function `\(f(n)\)`:
  
> `\(f(n)=\left\{ \begin{array}{rl} \frac{n}{2} &\mbox{ if n is even} \\ 3n+1 &\mbox{ if n is odd} \end{array} \right.\)`
  
> Then, `\(\lim_{k \to \infty}f^{k}(n)=1\)` for all integers `\(n>0\)`. That is, repeated application of the function will always converge to `\(1\)`, regardless of the starting point. 

Here's pseudo-code for a program, `\(P_{C1}\)` that takes as input a number, `\(n\)`:
  

~~~
while (n != 1):
	if (n is even):
		n = n / 2
	else
		n = 3 * n + 1
~~~

Now, `\(H(P_{C1},n)\)` will confirm whether an individual number eventually converges to 1. Now we can use that to construct another program, call it `\(P_{C2}\)`
  


~~~
let x=2
while (TRUE):
	if (H(PC1,x) == 0):
		HALT
	else
		x=x+1
~~~

Running `\(H(P_{C2},[nothing])\)` will either confirm or reject the Collatz conjecture. If it returns `\(1\)`, that means that `\(P_{C2}\)` would find a number for which `\(P_{C1}\)` does not halt, so the conjecture is false. If it returns `\(0\)`, that means it would never find a number, so the conjecture is true.

Let's knock one more out, just to prove a point. The Twin Primes Conjecture is:

> There are infinitely many primes `\(p\)` such that `\(p+2\)` is also prime.

Call the following program `\(P_{TP1}\)`, and have it take as input an integer, `\(x\)`:
  


~~~
while (TRUE):
	if (isPrime(x) and isPrime(x+2)):
		HALT
	else
		x = x + 1
~~~

Now here's another program, `\(P_{TP2}\)`:
  
~~~
let n=1
while (TRUE):
	if (H(PTP1,n) == 0):
		HALT
	else
		n = n + 1
~~~

Now, `\(H(P_{TP2},[nothing])\)` will confirm or reject the conjecture. There are many extremely difficult problems that are trivially solved if the Halting Problem is solved. Reducing one problem to another is a powerful technique, if you can reach a solvable problem, but unfortunately it's little more than an interesting exercise if none of the problems involved have solutions.

 [1]: /111
