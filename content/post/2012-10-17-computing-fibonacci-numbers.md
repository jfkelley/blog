---
title: Computing Fibonacci numbers
author: joefkelley
layout: post
date: 2012-10-17
url: /20
sfw_pwd:
  - yaGhNcWVGvbm
categories:
  - Math
  - Programming

---
The Fibonacci series is a very famous recursively-defined series that is often used as an introductory exercise in beginning programming courses. It has a very simple definition:
  


<center>
  `\(f(n)=f(n-1)+f(n-2)\)`
</center>


  
We often choose `\(f(0)=0\)` and `\(f(1)=1\)`, which makes the first few terms of the series:
  


<center>
  `\(0,1,1,2,3,5,8,13,21,34...\)`
</center>


  
And it's as simple as that. But computing an individual element of the series via computer is actually an interesting optimization problem.

Here's a first attempt of implementing this simple function in Java:
  
~~~Java
public static long f(int n) {
	if (n == 0) return 0;
	if (n == 1) return 1;
	return f(n - 1) + f(n - 2);
}
~~~

Simple enough, but if you try to actually run this code on any reasonably-high number, you'll quickly find that it is extremely slow. This is because it re-computes values many times. For example, to compute f(100), it must first do f(99) and f(98). But f(99) also calls f(98), so it redoes the exact same complication twice. Lower numbers are redone many times over. In fact, the entire runtime complexity to calculate `\(f(n)\)` is `\(O(f(n))\)`, which grows very fast!

Surely we can do better, and in fact there is an obvious solution: iteration instead of recursion. Like so:
  

~~~Java
public static BigInteger f(int n) {
	if (n == 0) return BigInteger.ZERO;
	if (n == 1) return BigInteger.ONE;
	BigInteger x1 = BigInteger.ZERO;
	BigInteger x2 = BigInteger.ONE;
	for (int i = 0; i < n - 1; i++) {
		BigInteger x3 = x1.add(x2);
		x1 = x2;
		x2 = x3;
	}
	return x2;
}
~~~


Note that I've switched from longs to BigIntegers, as this series quickly outgrows the maximum value of a Java long, `\(2^{63}-1\)`.

This is much better. If we assume that BigIntegers add in constant time (they technically don't), the runtime here is clearly `\(O(n)\)`, which is entirely tractable. But can we do better?

The first thing to think about in a lot of these situations is back to the math. Is there anything we can do with the formula to put it in a form that's more conducive to efficient computation? Actually, there is a little-known fact about fibonacci numbers: they have a closed-form representation. It's called Binet's Formula, and it is:
  


<center>
  `\(f(n)=\frac{1}{\sqrt{5}}\left(\left(\frac{1+\sqrt{5}}{2}\right)^n-\left(\frac{1-\sqrt{5}}{2}\right)^n\right)\)`
</center>


  
Yes, even though there are plenty of irrational numbers in this formula, it turns out that for integer values of `\(n\)`, it gives integer values out.

Although, if you look closely, you might notice that the term `\(\left(\frac{1-\sqrt{5}}{2}\right)^n\)` will approach zero pretty quickly. In fact it approaches just quickly enough that if we omit it, the results are close enough that just rounding to the nearest integer is sufficient. So the new formula becomes:
  


<center>
  `\(f(n)=round\left(\frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^n\right)\)`
</center>


  
So that's pretty simple, but is it useful? Let's give it a shot:

~~~Java
public static BigInteger f(int n) {
	double sqrtFive = Math.sqrt(5.0);
	double phi = (1 + sqrtFive) / 2;
	double result = Math.pow(phi, n) / sqrtFive;
	return BigInteger.valueOf(Math.round(result));
}
~~~


  
Looks good, but when you run it, it's only accurate up through `\(n=70\)`. After that, it starts giving back incorrect results! It turns out that Java doubles are not precise enough. They are approximations, especially for irrational numbers like `\(\sqrt{5}\)`, and that approximation gets compounded when raised to a power. It's called [numerical instability][1] and it's something to always keep in mind when writing algorithms involving non-integral numbers. We could use the BigDecimal class, or some other precise math library, but that will get ugly fast. And the amount of precision needed will keep growing for the size of the input. If we'd like this to be robust, decimals are just not a great option.

Back to the math again... it turns out there's a neat representation of the fibonacci series if we use a little linear algebra. Take a look at this:
  


<center>
  `\(\begin{bmatrix}f(k+2)\\f(k+1)\end{bmatrix}=\begin{bmatrix}1&&1\\1&&0\end{bmatrix}\begin{bmatrix}f(k+1)\\f(k)\end{bmatrix}\)`
</center>

If you work it out, this is trivial to arrive at from just the definition of the series. And if we apply it again, we get:
  
<center>
  `\(\begin{bmatrix}f(k+3)\\f(k+2)\end{bmatrix}=\begin{bmatrix}1&&1\\1&&0\end{bmatrix}\begin{bmatrix}1&&1\\1&&0\end{bmatrix}\begin{bmatrix}f(k+1)\\f(k)\end{bmatrix}\)`
</center>


  
And we can generalize this to:
  


<center>
  `\(\begin{bmatrix}f(n+k+1)\\f(n+k)\end{bmatrix}=\begin{bmatrix}1&&1\\1&&0\end{bmatrix}^n\begin{bmatrix}f(k+1)\\f(k)\end{bmatrix}\)`
</center>


  
And if we set `\(k=0\)`, we achieve a great little formula for the n-th fibonacci number:
  


<center>
  `\(\begin{bmatrix}f(n+1)\\f(n)\end{bmatrix}=\begin{bmatrix}1&&1\\1&&0\end{bmatrix}^n\begin{bmatrix}1\\0\end{bmatrix}\)`
</center>


  
Now it appears that this is no better than the iterative solution. We are multiplying a matrix by itself `\(n\)` times, so the overall runtime should be `\(O(n)\)`. But raising anything to a power is actually not linear in the power if you use a trick called "exponentiation by squaring". Here's the idea: let's consider computing `\(5^8\)`. To do so, we can first compute `\(5^15^1 = 5^2\)`, then square that to get `\(5^25^2=5^4\)`, then square that to get `\(5^45^4=5^8\)`. Three operations instead of eight. For powers that don't work as nicely as eight, we add in an extra multiplication by the base occasionally. More precisely, we'll actually do this top-down, and any time we want an odd power, we'll add in a multiplication. For example, to compute `\(3^{15}\)` we do:

<div>$$
3^{15}=3^{14}3^1 \\
3^{14}=3^73^7 \\
3^7=3^63^1 \\
3^6=3^33^3 \\
3^3=3^23^1 \\
3^2=3^13^1
$$</div>

So the actual order of computation is the reverse of that. This process results in `\(O(log(n))\)` multiplications. It actually turns out, however, that this is sub-optimal. Consider the above example, `\(3^{15}\)`, where we use six steps. We could have done it in five:

<div>$$
3^{15}=3^33^{12} \\
3^{12}=3^63^6 \\
3^6=3^33^3 \\
3^3=3^23^1 \\
3^2=3^13^1
$$</div>

However, an algorithm for efficiently finding a sequence of multiplications that computes a power in the minimum number of steps has never been found; it's an open problem! There's even reason to believe there is no efficient method (because a closely-related problem is NP-complete). This technique is more generally called "Addition-chain exponentiation" and [there's more information on Wikipedia][2] about it.

Regardless, exponentiation by squaring is good enough, so we have enough to implement it.

~~~Java
private static final BigInteger[][] MATRIX =
	{{BigInteger.ONE, BigInteger.ONE},
	 {BigInteger.ONE, BigInteger.ZERO}};

public static BigInteger f(int n) {
	if (n == 0) return BigInteger.ZERO;
	if (n == 1) return BigInteger.ONE;
	return power(MATRIX, n - 1)[0][0];
}

private static BigInteger[][] power(BigInteger[][] m, int power) {
	if (power == 1) return m;
	BigInteger[][] sub = power(m, power / 2);
	if (power % 2 == 0) {
		return multiply(sub, sub);
	} else {
		return multiply(multiply(sub, sub), m);
	}
}

private static BigInteger[][] multiply(BigInteger[][] m1, BigInteger[][] m2) {
	return new BigInteger[][]{
		{m1[0][0].multiply(m2[0][0]).add(m1[0][1].multiply(m2[1][0])),
		 m1[0][0].multiply(m2[0][1]).add(m1[0][1].multiply(m2[1][1]))},
		{m1[1][0].multiply(m2[0][0]).add(m1[1][1].multiply(m2[1][0])),
		 m1[1][0].multiply(m2[0][1]).add(m1[1][1].multiply(m2[1][1]))}
	};
}
~~~


  
Forgive my multiply method; it's ugly but fast.

This is the fastest I've been able to come up with. I'm sure there are plenty of implementation optimizations I could do, but algorithmically, this seems to be the best. And it is definitely faster than the iterative method: on my laptop, computing `\(f(1,000,000)\)` took 26 seconds with the naive iterative method, and under 3 seconds with the matrix multiplication method - quite an improvement! For the record, the direct computation method didn't finish within a few minutes, so I stopped it. And the recursive method probably would have taken longer than the age of the universe.

If anyone knows of any other (faster) ways of computing fibonacci numbers, I'd be happy to hear them - leave a comment.

 [1]: http://en.wikipedia.org/wiki/Numerical_instability
 [2]: http://en.wikipedia.org/wiki/Addition_chain_exponentiation
