---
title: Decimal expansions containing series
author: joefkelley
layout: post
date: 2014-01-30
url: /635
sfw_pwd:
  - RBRK3hxrT4hG
categories:
  - Math

---
I just saw [a great question on the Math StackExchange][1] asking why `\(\frac{100000000}{99989999}\)` generates the fibonacci series in its decimal expansion: `\(1.000100020003000500080013002100340055...\)`. And it isn't just a trick; it really does give the fibonacci series until it can't fit any more in 4-digit chunks. And even then, it just carries the extra digits over.

It's a pretty neat technique to prove this. Basically, consider the sum: `\(f_0+f_1x+f_2x^2+f_3x^3+...\)`

Then, if you plug in a decimal like `\(0.0001\)` for `\(x\)`, you can see that it spaces out the terms in the series every four digits in the decimal expansion (exactly what you see above). The hard part is simplifying that sum. There is [another Math StackExchange answer][2] that gives the proof that: `\(f_0+f_1x+f_2x^2+f_3x^3+... = \frac{1}{1-(x+x^2)}\)`

This kind of gives a pretty general technique for generating all kinds of interesting decimal expansions. The easiest is probably the powers of a given number: `\(1+ax+a^2x^2+a^3x^3+...=\frac{1}{1-ax}\)`

Plug in `\(a=2\)` and `\(x=0.0001\)` and you get the powers of two: `\(\frac{5000}{4999}=1.0002000400080016003200640128025605121024...\)`

How about simply the integers in order?

<div>$$
\begin{align}
&1+2x+3x^2+4x^3+... \\
=&\frac{d}{dx}\left(x+x^2+x^3+x^4+...\right) \\
=&\frac{d}{dx}\left(\frac{x}{\left(1-x\right)}\right) \\
=&\frac{1}{\left(1-x\right)^2} \\
\end{align}
$$</div>

Plugging in `\(x=0.001\)` yields: `\(\frac{1000000}{998001}=1.002003004005006...\)`

Square numbers? Check this out:

<div>$$
\begin{align}
&\sum_{n=1}^{\infty}n^2x^n \\
=&x + \sum_{n=2}^{\infty}n^2x^n \\
=&\frac{1}{2}\left(2x + \sum_{n=2}^{\infty}2n^2x^n\right) \\
=&\frac{1}{2}\left(2x + \sum_{n=2}^{\infty}n\left(2n\right)x^n\right) \\
=&\frac{1}{2}\left(2x + \sum_{n=1}^{\infty}\left(n+1\right)\left(2n+2\right)x^{n+1}\right) \\
=&\frac{1}{2}\left(2x + \sum_{n=1}^{\infty}\left(n\left(n+1\right)+\left(n+1\right)\left(n+2\right)\right)x^{n+1}\right) \\
=&\frac{1}{2}\left(2x + \sum_{n=1}^{\infty}n\left(n+1\right)x^{n+1} + \sum_{n=1}^{\infty}\left(n+1\right)\left(n+2\right)x^{n+1}\right) \\
=&\frac{1}{2}\left(\sum_{n=1}^{\infty}n\left(n+1\right)x^{n+1} + \sum_{n=0}^{\infty}\left(n+1\right)\left(n+2\right)x^{n+1}\right) \\
=&\frac{1}{2}\left(\sum_{n=0}^{\infty}\left(n+1\right)\left(n+2\right)x^{n+2} + \sum_{n=0}^{\infty}\left(n+1\right)\left(n+2\right)x^{n+1}\right) \\
=&\frac{1}{2}\left(\sum_{n=0}^{\infty}\left(n+1\right)\left(n+2\right)\left(x^{n+2}+x^{n+1}\right)\right) \\
=&\frac{1}{2}\left(x^2+x\right)\left(\sum_{n=0}^{\infty}\left(n+1\right)\left(n+2\right)x^n\right) \\
=&\frac{1}{2}\left(x^2+x\right)\left(\frac{d^2}{dx^2}\sum_{n=0}^{\infty}x^n\right) \\
=&\frac{1}{2}\left(x^2+x\right)\left(\frac{d^2}{dx^2}\frac{1}{1-x}\right) \\
=&\frac{1}{2}\left(x^2+x\right)\left(\frac{2}{\left(1-x\right)^3}\right) \\
=&\frac{x^2+x}{\left(1-x\right)^3}
\end{align}
$$</div>

Plug in `\(x=0.001\)`: `\(\frac{1001000}{997002999}=0.001004009016025036...\)`

 [1]: http://math.stackexchange.com/questions/656183/why-does-frac1-99989999-generate-the-fibonacci-sequence
 [2]: http://math.stackexchange.com/questions/338740/the-generating-function-for-the-fibonacci-numbers
