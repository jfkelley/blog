---
title: Integer Fractions Puzzle
author: joefkelley
layout: post
date: 2013-11-01
url: /518
sfw_pwd:
  - 9tyipk4SOiB0
categories:
  - Math
  - Programming

---
Here's a programming puzzle: Given an integer `\(n\)`, count the unique integer solutions `\((x, y)\)` to the equation:

`\(\frac{1}{x}+\frac{1}{y}=\frac{1}{n!}\)`

And let's add a requirement that it must be reasonable efficient. We could be dealing with `\(n=5000\)`, which means that `\(n!\)` is already an extremely large number. Any attempt at a brute-force-ish program is going to be useless with numbers that big. If you want to take a crack at the problem on your own, stop reading here and go for it. Otherwise, I'll explain my solution.

I'll start by re-arranging the equation: `\(\frac{n!}{x}+\frac{n!}{y}=1\)`

All equations of this kind must be able to be expressed in the form: `\(\frac{a}{a+b}+\frac{b}{a+b}=1\)`

This might simplify things a bit... instead of picking `\(x\)` and `\(y\)`, we can pick `\(a\)` and `\(b\)`. However, there are restrictions on what we can pick.

`\(\frac{n!}{x}\)` must simplify to `\(\frac{a}{a+b}\)`, so this means that `\(n!\)` must be divisible by `\(a\)`. Likewise, `\(n!\)` must also be divisible by `\(b\)`. But if our `\(a\)` and `\(b\)` satisfy those conditions, then we have a solution. Simply set:

<div>$$
x=\frac{(a+b)n!}{a} \\
y=\frac{(a+b)n!}{b}
$$</div>

Now, we've greatly simplified the problem. The question is identical to: "count the distinct pairs of factors of `\(n!\)`." Or, trivially, "count the factors of `\(n!\)`, then square that."

We're not totally out of the woods yet. Brute-force counting factors won't work, since `\(n!\)` is huge. But consider the prime factorization of, for example, `\(6!=720\)`: `\(2^4\times3^2\times5^1\)`

Each (not necessarily prime) factor of `\(720\)` must be a product of `\(0\)`, `\(1\)`, `\(2\)`, `\(3\)`, or `\(4\)` "`\(2\)`"s, `\(0\)`, `\(1\)`, or `\(2\)` "`\(3\)`"s, and `\(0\)` or `\(1\)` "`\(5\)`"s. Multiplying the number of options we have for each prime factor together, we can count that there are `\((4+1)\times(2+1)\times(1+1)=30\)` different factors of `\(720\)`.

But still, directly computing the prime factorization of `\(n!\)` could be very slow. Instead, we'll count the number of occurrences of prime factors directly, taking advantage of the fact that we're dealing with a factorial.

What if we want to know the exponent on `\(2\)` in the prime factorization of `\(7!\)`? If we look at the expansion of `\(7!\)` as `\(7\times6\times5\times4\times3\times2\times1\)`, clearly every other term is divisible by `\(2\)`. So we have:

`\(\left \lfloor{\frac{7}{2}}\right \rfloor\)`

But this leaves out the fact that numbers might be divisible by `\(4\)` or `\(8\)` and so could have multiple factors of `\(2\)`. So the total count will be:

`\(\left \lfloor{\frac{7}{2}}\right \rfloor+\left \lfloor{\frac{7}{4}}\right \rfloor+\left \lfloor{\frac{7}{8}}\right \rfloor+...\)`

In general, the number of times a prime factor, `\(k\)`, occurs in `\(n!\)` is:

`\(\left \lfloor{\frac{n}{k}}\right \rfloor+\left \lfloor{\frac{n}{k^2}}\right \rfloor+\left \lfloor{\frac{n}{k^3}}\right \rfloor+...\)`

Lastly, it's worth calling out that there can be no prime factors in `\(n!\)` greater than `\(n\)`.

Finally, this gives a sufficiently fast algorithm for solving the original problem. Here's a purely functional solution in Scala. My prime generating code looks a little weird, but it's an implementation of the [Sieve of Eratosthenes][1], utilizing Scala's lazy streams. I started with a simpler version of that algorithm, but was getting out of memory errors. That's something I've noticed that pops up when you're trying to keep stuff purely functional: sometimes you either get concise code, or memory-efficient code, rarely both. Scala's nice in that you can fall back to all of the mutable, imperative stuff when needed, but it's an interesting exercise to try to avoid that.

~~~scala
object FractionPuzzle {

  // Stream of all integers, starting with n
  def ints(n: Int): Stream[Int] =
    n #:: ints(n + 1)

  // Stream of primes numbers
  def primes(nums: Stream[Int] = ints(2), filters: Set[Int] = Set.empty): Stream[Int] = {
    val firstIndex = nums.indexWhere(n => !filters.exists(n % _ == 0))
    nums(firstIndex) #:: primes(nums.drop(firstIndex + 1), filters + nums(firstIndex))
  }

  // Stream of powers of n, starting with n^k
  def powers(n: Int, k: Int = 1): Stream[Long] =
    Math.round(Math.pow(n, k)) #:: powers(n, k + 1)

  // Count how many times the prime factor k occurs in n!
  def countFactorOcurrences(n: Int, k: Int): Long = {
    powers(k).takeWhile(_ <= n).map(n / _).sum
  }

  // Reads in a number, n, and prints out the number of integer solutions to 1/x + 1/y = 1/n!
  def main(args: Array[String]): Unit = {
    val n = readInt()
    val primeFactors = primes().takeWhile(_ <= n)
    val nFactors =
      primeFactors.map(countFactorOcurrences(n, _)).map(x => BigInt(x + 1)).product
    println(nFactors * nFactors)
  }

}
~~~

It starts to slow down around `\(n=200000\)` or so, but still, I think that's pretty impressive. The solution for that value of n is 21179 digits long!

 [1]: http://en.wikipedia.org/wiki/Sieve_of_eratosthenes
