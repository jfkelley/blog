---
title: Sink the Ship Puzzle - Solution
author: joefkelley
layout: post
date: 2014-02-21
url: /681
sfw_pwd:
  - 0arOeFoXVOD3
categories:
  - Math
  - Programming
  - Puzzle

---
Here's a solution to [the "sink the ship" puzzle][1] I posted a little while ago:

First of all, let's reduce this to one dimension. Let's say the ship starts at an integer position on a line: `\(x_0\)`, and moves with some integer velocity: `\(v\)`. All possible combinations of those two values must be eventually hit by our strategy.

A first intuition might be to try to drop missiles on every possible location. So you could fire one at 0, 1, -1, 2, -2, 3, -3, etc. If the ship wasn't moving, this would work, but you can see that the shots are moving outward at a pretty slow rate, so any ship moving away from the origin and not hit by one of the first shots is never going to be hit.

The key insight is to try not to hit the ship where it currently is, but to try to hit a specific starting position/velocity pair, and work out based on the current time where the ship is now. For example, with the first shot (t=0), you could try to hit a ship starting at 0, with velocity 0, so you fire at 0. With the second shot (t=1) you would try to hit a ship starting at 0, with velocity 1, so fire at 1. Next shot (t=2) try to hit one that started at 0 with velocity 2, so fire at 4. You could try to make a table:

<div>$$
\begin{array}{ c | c c | c }
t & x_0 & v & x_t \\
\hline
0 & 0 & 0 & 0 \\
1 & 0 & 1 & 1 \\
2 & 0 & 2 & 4 \\
3 & 0 & 3 & 9 \\
...
\end{array}
$$</div>

The goal is that every combination of `\(x_0\)` and `\(v\)` appear at some row t. If you keep going in the above fashion, however, this isn't the case. You'll only ever hit ships that started at 0, and some other unlucky ones. You won't, for example, hit any that start negative and move in the negative direction.

To put it in math terms: If you think of the strategy as a function from the natural numbers (`\(\mathbb{N}\)`) to pairs of integers (`\(\mathbb{Z}\times\mathbb{Z}\)`), then this function needs to be surjective.

It's easier to break this function into a composition of multiple functions, and show that each one is surjective.

First, let the function `\(c\)` be the inverse of the [Cantor pairing function][2]. This function maps `\(\mathbb{N}\)` to `\(\mathbb{N}\times\mathbb{N}\)`.

Second, apply a function to each component that maps `\(\mathbb{N}\)` to `\(\mathbb{Z}\)`. I alluded to this earlier; it's just the sequence 0, 1, -1, 2, -2, 3, -3, etc. Call this function `\(z\)`. Formally: `\(z(n)=\left\{\begin{array}{lr}\frac{-n}{2} & \text{if }n\text{ is even}\\ \frac{n+1}{2} & \text{if }n\text{ is odd}\end{array}\right.\)`

Then the final function is `\(f(n)=z(c(n))\)`. Note that I'm abusing the notation for these functions a little bit because what I actually want is `\(z\)` applied to each component of `\(c(n)\)` individually. I guess you could write that as something like `\(f(n)=(z(c(n)_1), z(c(n)_2))\)` but I'll just skip that. Anywho, here are the first few values:

<div>$$
\begin{array}{ r | c | c | r r | r}
t & c(t) & z(c(t)) & x_0 & v & x_t \\
\hline
0 & (0, 0) & (0, 0) & 0 & 0 & 0 \\
1 & (1, 0) & (1, 0) & 1 & 0 & 1 \\
2 & (0, 1) & (0, 1) & 0 & 1 & 2 \\
3 & (2, 0) & (-1, 0) & -1 & 0 & -1 \\
4 & (1, 1) & (1, 1) & 1 & 1 & 5 \\
5 & (0, 2) & (0, -1) & 0 & -1 & -5 \\
6 & (3, 0) & (2, 0) & 2 & 0 & 2 \\
7 & (2, 1) & (-1, 1) & -1 & 1 & 6 \\
8 & (1, 2) & (1, -1) & 1 & -1 & -7 \\
9 & (0, 3) & (0, 2) & 0 & 2 & 18 \\
...
\end{array}
$$</div>

And that's good enough for the one-dimensional version! The last column gives the locations to fire missiles at. Thinking about the problem in this way, it's not actually that much more difficult to move to two dimensions. Just compose another function in there; in fact using Cantor's pairing function again is sufficient. One application is bijective to `\(\mathbb{N}^2\)`, so two applications is bijective to `\(\mathbb{N}^4\)`. Here's the table (and final answer!):

<div>$$
\begin{array}{ r | c | c | c | r r | r r | c }
t & c(t) & c(c(t)) & z(c(c(t))) & x_0 & v_x & y_0 & v_y & (x_t, y_t) \\
\hline
0 & (0, 0) & (0, 0, 0, 0) & (0, 0, 0, 0) & 0 & 0 & 0 & 0 & (0, 0) \\
1 & (1, 0) & (1, 0, 0, 0) & (1, 0, 0, 0) & 1 & 0 & 0 & 0 & (1, 0) \\
2 & (0, 1) & (0, 0, 1, 0) & (0, 0, 1, 0) & 0 & 0 & 1 & 0 & (0, 1) \\
3 & (2, 0) & (0, 1, 0, 0) & (0, 1, 0, 0) & 0 & 1 & 0 & 0 & (3, 0) \\
4 & (1, 1) & (1, 0, 1, 0) & (1, 0, 1, 0) & 1 & 0 & 1 & 0 & (1, 1) \\
5 & (0, 2) & (0, 0, 0, 1) & (0, 0, 0, 1) & 0 & 0 & 0 & 1 & (0, 5) \\
6 & (3, 0) & (2, 0, 0, 0) & (-1, 0, 0, 0) & -1 & 0 & 0 & 0 & (-1, 0) \\
7 & (2, 1) & (0, 1, 1, 0) & (0, 1, 1, 0) & 0 & 1 & 1 & 0 & (7, 1) \\
8 & (1, 2) & (1, 0, 0, 1) & (1, 0, 0, 1) & 1 & 0 & 0 & 1 & (1, 8) \\
9 & (0, 3) & (0, 0, 2, 0) & (0, 0, -1, 0) & 0 & 0 & -1 & 0 & (0, -1) \\
10 & (4, 0) & (1, 1, 0, 0) & (1, 1, 0, 0) & 1 & 1 & 0 & 0 & (11, 0) \\
11 & (3, 1) & (2, 0, 1, 0) & (-1, 0, 1, 0) & -1 & 0 & 1 & 0 & (-1, 1) \\
12 & (2, 2) & (0, 1, 0, 1) & (0, 1, 0, 1) & 0 & 1 & 0 & 1 & (12, 12) \\
13 & (1, 3) & (1, 0, 2, 0) & (1, 0, -1, 0) & 1 & 0 & -1 & 0 & (1, -1) \\
14 & (0, 4) & (0, 0, 1, 1) & (0, 0, 1, 1) & 0 & 0 & 1 & 1 & (0, 15) \\
15 & (5, 0) & (0, 2, 0, 0) & (0, -1, 0, 0) & 0 & -1 & 0 & 0 & (-15, 0) \\
16 & (4, 1) & (1, 1, 1, 0) & (1, 1, 1, 0) & 1 & 1 & 1 & 0 & (17, 1) \\
17 & (3, 2) & (2, 0, 0, 1) & (-1, 0, 0, 1) & -1 & 0 & 0 & 1 & (-1, 17) \\
18 & (2, 3) & (0, 1, 2, 0) & (0, 1, -1, 0) & 0 & 1 & -1 & 0 & (18, -1) \\
19 & (1, 4) & (1, 0, 1, 1) & (1, 0, 1, 1) & 1 & 0 & 1 & 1 & (1, 20) \\
20 & (0, 5) & (0, 0, 0, 2) & (0, 0, 0, -1) & 0 & 0 & 0 & -1 & (0, -20) \\
21 & (6, 0) & (3, 0, 0, 0) & (2, 0, 0, 0) & 2 & 0 & 0 & 0 & (2, 0) \\
22 & (5, 1) & (0, 2, 1, 0) & (0, -1, 1, 0) & 0 & -1 & 1 & 0 & (-22, 1) \\
23 & (4, 2) & (1, 1, 0, 1) & (1, 1, 0, 1) & 1 & 1 & 0 & 1 & (24, 23) \\
24 & (3, 3) & (2, 0, 2, 0) & (-1, 0, -1, 0) & -1 & 0 & -1 & 0 & (-1, -1) \\
25 & (2, 4) & (0, 1, 1, 1) & (0, 1, 1, 1) & 0 & 1 & 1 & 1 & (25, 26) \\
26 & (1, 5) & (1, 0, 0, 2) & (1, 0, 0, -1) & 1 & 0 & 0 & -1 & (1, -26) \\
27 & (0, 6) & (0, 0, 3, 0) & (0, 0, 2, 0) & 0 & 0 & 2 & 0 & (0, 2) \\
...
\end{array}
$$</div>

Just for fun, here's a scala implementation that proves that it works. Enter any starting position and velocity, and it will eventually hit the ship. It just might take a very long time.

~~~scala
object SinkTheShip {
  def main(args: Array[String]): Unit = {

    print("enter x0: ")
    val x0 = BigInt(readLine())
    print("enter y0: ")
    val y0 = BigInt(readLine())
    print("enter vx: ")
    val vx = BigInt(readLine())
    print("enter vy: ")
    val vy = BigInt(readLine())
    print("print every shot? [y/n]")
    val debug = readLine().toLowerCase().charAt(0) == 'y'

    var x = x0
    var y = y0
    var t = BigInt(0)
    while (true) {
      val (shotX, shotY) = getTarget(t)
      if (debug) println(s"Your ship is at ($x, $y). I fire at ($shotX, $shotY).")
      if (shotX == x && shotY == y) {
        val nShots = t + 1
        println(s"Hit! after $nShots shots, at ($x, $y)")
        return
      } else {
        if (debug) println("Miss.")
      }
      t = t + 1
      x = x + vx
      y = y + vy
    }
  }

  def getTarget(t: BigInt): (BigInt, BigInt) = {
    val (a, b, c, d) = doubleInverseCantor(t)
    val (x, vx, y, vy) = (toAllIntegers(a), toAllIntegers(b), toAllIntegers(c), toAllIntegers(d))
    (x + t * vx, y + t * vy)
  }

  def doubleInverseCantor(z: BigInt): (BigInt, BigInt, BigInt, BigInt) = {
    val (x, y) = inverseCantor(z)
    val (a, b) = inverseCantor(x)
    val (c, d) = inverseCantor(y)
    (a, b, c, d)
  }

  def inverseCantor(z: BigInt): (BigInt, BigInt) = {
    val w = (sqrt((z << 3) + 1) - 1) >> 1
    val t = (w * w + w) >> 1
    val y = z - t
    val x = w - y
    (x, y)
  }

  def toAllIntegers(z: BigInt): BigInt = {
    if (isEven(z)) {
      -(z >> 1)
    } else {
      (z + 1) >> 1
    }
  }

  def isEven(z: BigInt): Boolean = !z.testBit(0)

  // from https://issues.scala-lang.org/browse/SI-3739
  def sqrt(number : BigInt): BigInt = {
    def next(n : BigInt, i : BigInt) : BigInt = (n + i/n) >> 1

    val one = BigInt(1)
    var n = one
    var n1 = next(n, number)
    while ((n1 - n).abs > one) {
      n = n1
      n1 = next(n, number)
    }
    while (n1 * n1 > number) {
      n1 -= one
    }
    n1
  }

}
~~~

 [1]: /677
 [2]: http://en.wikipedia.org/wiki/Pairing_function#Cantor_pairing_function "Cantor pairing function"
