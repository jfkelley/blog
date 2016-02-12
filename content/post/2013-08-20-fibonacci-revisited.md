---
title: Fibonacci Revisited
author: joefkelley
layout: post
date: 2013-08-20
url: /469
sfw_pwd:
  - yNtCbmmmi9dn
categories:
  - Math
  - Programming

---
A coworker of mine (thanks, Jeff!) recently pointed out that I had been a little sloppy with my analysis of various algorithms to compute the `\(n^{th}\)` fibonacci number in a previous post. I made the simplifying assumption that arithmetic operations (addition and multiplication) happen in constant time. This is often, but not always, a good assumption to make; either the numbers involved in the computation are small enough that addition and multiplication _do_ happen in constant time, or the algorithm is dominated by other factors. Most modern computers are very optimized to do addition and multiplication of either 32- or 64-bit numbers in a fixed number of CPU cycles. These correspond to Java's int and long primitives, but in the earlier post, I was dealing with Java BigIntegers, and computing results taking up much more than 64 bits, so all of that is out the window.

Let's start with the recursive algorithm. The execution of a single call to `\(f(k)\)`, and not including any other recursive calls it makes, does a single addition. That addition has the runtime complexity of `\(O(b)\)`, where `\(b\)` is the max of the number of bits in the numbers being added. So the complexity of that individual call is: `\(O(log(f(k-1)))\)`

That's a little unwieldy, but it's easily simplified. The fibonacci numbers have a useful property:

<div>$$
\lim_{n \rightarrow \infty}{f(n)-\frac{\phi^n}{\sqrt{5}}}=0 \\
\text{where }\phi=\frac{1+\sqrt{5}}{2}
$$</div>

Therefore, `\(f(n)\)` itself is `\(\Theta(\phi^n)\)`, and thus also `\(O(\phi^n)\)`. Note, that's the behavior of the function itself, not the runtime complexity of an algorithm to compute it. Substituting that into the earlier complexity for a single call, we have: `\(O(log(f(k-1)))=O(log(\phi^n))=O(n)\)`

Now, if you trace the execution of calling `\(f(n)\)` and start counting how many times `\(f(n-1), f(n-2), f(n-3)\)`, etc. are called, a pattern quickly emerges (and can be proven). `\(f(k)\)` is called precisely `\(f(n-k+1)\)` times. So we can sum over that value to get the overall runtime complexity:

<div>$$
\begin{align}
&\sum_{k=1}^{n}f(n-k+1)O(k) \\
=&\sum_{k=1}^{n}O(\phi^{n-k+1})O(k) \\
\end{align}
$$</div>

Now I'll seek to prove the following, for some constants `\(c\)` and `\(m\)`: `\(\sum_{k=1}^{n}O(\phi^{n-k+1})O(k) \leq c\sum_{k=1}^{n}k\phi^{n-k} \leq m\phi^{n-1}\)`

The first inequality is trivial from the definition of Big-O notation. The second is trickier. I'll drop the `\(c\)` since the ratio of two constants (`\(m\)` and `\(c\)`) is still a constant. Consider the difference:

<div>$$
\begin{align}
&m\phi^{n-1} - \sum_{k=1}^{n}k\phi^{n-k} \\
=&m\phi^{n-1} - \phi^{n-1} - \sum_{k=2}^{n}k\phi^{n-k} \\
=&(m-1)\phi^{n-1} - \sum_{k=2}^{n}k\phi^{n-k} \\
=&(m-1)\phi^{n-1} - 2\phi^{n-2} - \sum_{k=3}^{n}k\phi^{n-k} \\
=&(\phi(m-1)-2)\phi^{n-2} - \sum_{k=3}^{n}k\phi^{n-k} \\
=&(\phi(\phi(m-1)-2)-3)\phi^{n-3} - \sum_{k=4}^{n}k\phi^{n-k} \\
\end{align}
$$</div>

You can see we're building up a kind of "residual difference" on the left, that must be non-negative in order for the inequality to hold. For large values of `\(m\)`, such as `\(1000\)`, it's easy to see that this will hold. Multiplying by `\(\phi\)` causes that difference to easily escape the linearly-growing subtractions. The lower-bound for escaping is `\(m=(\frac{\phi}{\phi-1})^2\)`. But anyway, we have enough to state that the overall complexity for the recursive algorithm is, simply: `\(O(\phi^n)\)`

Moving on to the iterative algorithm, the analysis is much easier. There are `\(n\)` additions, each taking linear time. More formally, the complexity is: `\(\sum_{k=1}^{n}O(k)\)`

So the overall complexity is still: `\(O(n^2)\)`

And finally, the matrix multiplication algorithm. It's worthwhile to note here that the time complexity of Java BigInteger's multiply method is usually `\(O(n^2)\)` in the number of bits, although algorithms with better asymptotic behavior are known for multiplication.

The last step in computing `\(f(n)\)` involves, essentially, the multiplication: `\(f(\frac{n}{2})f(\frac{n}{2})\)`, up to constant factors. We've previously established that the number of bits is linear in `\(n\)`, so the complexity of this multiplication is: `\(O(n^2)\)`

Move back one step in the computation, and the number of bits halves, so the execution time is down by `\(1/4\)`. Asymptotically, the behavior of the algorithm is proportional to: `\(n^2 + \frac{n^2}{4} + \frac{n^2}{16} + \frac{n^2}{64} + ...\)`

So overall: `\(O(n^2)\)`

If Java were to pick a [multiplication algorithm with better asymptotic behavior][1], a complexity of `\(O(n^{1.465})\)` or `\(O(n \log n \log \log n)\)` could be easily achieved.

And finally, as with any analysis of algorithms, it's worthwhile to remember that the constant factors really do matter. In practice, the matrix multiplication algorithm is significantly faster, even if it's asymptotic behavior is the same, and for most reasonable ranges of inputs, this would not actually be improved by a different multiplication algorithm.

 [1]: http://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations#Arithmetic_functions
