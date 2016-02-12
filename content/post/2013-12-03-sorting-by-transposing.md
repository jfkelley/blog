---
title: Sorting by transposing
author: joefkelley
layout: post
date: 2013-12-03
url: /556
sfw_pwd:
  - 6NthRUua5EWj
categories:
  - Math
  - Programming

---
Here's a clever algorithm for sorting integers. The bulk of the work is actually done by a transposition of a list of lists of different lengths. Normally, you would think of a transpose as something like this:

<div>$$
\begin{array}{ccc}
a & b & c \\
d & e & f \\
g & h & i
\end{array} \Rightarrow
\begin{array}{ccc}
a & d & g \\
b & e & h \\
c & f & i
\end{array}
$$</div>

But what if each row has a different number of elements? We might just do this

<div>$$
\begin{array}{ccc}
a & b \\
c \\
d & e & f
\end{array} \Rightarrow
\begin{array}{ccc}
a & c & d \\
b & & e \\
& & f
\end{array}
$$</div>

However, this is really a list of lists, so there has to be some way of representing the "spaces" between elements. Usually we might use null, or some other special value:

<div>$$
\begin{array}{ccc}
a & b \\
c \\
d & e & f
\end{array} \Rightarrow
\begin{array}{ccc}
a & c & d \\
b & \emptyset & e \\
\emptyset & \emptyset & f
\end{array}
$$</div>

But what if, instead, we just let elements shift over, and fill in the spaces? In that case, we would get:

<div>$$
\begin{array}{ccc}
a & b \\
c \\
d & e & f
\end{array} \Rightarrow
\begin{array}{ccc}
a & c & d \\
b & e \\
f
\end{array}
$$</div>

Notice that it makes a nice triangle shape, with lists of decreasing size as we move down the rows. This is not by accident. The first row in the transpose has a length equal to the number of rows in the original that have a first element. The length of the second row in the transpose is equal to the number of rows in the original that have at least two elements. And so on.

This is actually a neat fact that we can use to sort integers by an algorithm called Bead Sort. Start by representing each integer as a list of "beads", where the length of the list is equal to the value of the integer. For example:

<div>$$
\begin{array}{c}
4 \\
2 \\
3
\end{array} \Rightarrow
\begin{array}{cccc}
\odot & \odot & \odot & \odot \\
\odot & \odot \\
\odot & \odot & \odot
\end{array}
$$</div>

Next, do the transpose on the resulting list of lists of beads:

<div>$$
\begin{array}{cccc}
\odot & \odot & \odot & \odot \\
\odot & \odot \\
\odot & \odot & \odot
\end{array} \Rightarrow
\begin{array}{ccc}
\odot & \odot & \odot \\
\odot & \odot & \odot \\
\odot & \odot \\
\odot
\end{array}
$$</div>

Finally, think about the first row in this resulting matrix of beads. It has three elements, so we know that the original matrix of beads had exactly three rows that had a first element. Because those rows are based on the input integers, we know that there are exactly three elements that are at least one. From the other rows, we know there are exactly three elements that are at least two, exactly two elements that are at least three, exactly one element that is at least four, and no elements that are greater than four. Walking down this list of facts, we can reproduce the original elements.

If there are exactly three elements two or greater, and exactly two elements three or greater, then there must be exactly one two.

If there are exactly two elements three or greater, and exactly one element four or greater, then there must be exactly one three.

If there is exactly one element four or greater, and no elements five or greater, then there must be exactly one four, and no elements greater than four.

And finally, note that this reproduction gives the original list back in sorted order! Here's a graphical representation of the whole process:

<div>$$
\begin{array}{c}
4 \\
2 \\
3
\end{array} \Rightarrow
\begin{array}{cccc}
\odot & \odot & \odot & \odot \\
\odot & \odot \\
\odot & \odot & \odot
\end{array} \Rightarrow
\begin{array}{ccc}
\odot & \odot & \odot \\
\odot & \odot & \odot \\
\odot & \odot \\
\odot
\end{array} \Rightarrow
\begin{array}{c}
3 \\
3 \\
2 \\
1
\end{array} \Rightarrow
\begin{array}{c}
3 - 3 \\
3 - 2 \\
2 - 1 \\
1 - 0
\end{array} =
\begin{array}{c}
0 \\
1 \\
1 \\
1
\end{array} \Rightarrow
\begin{array}{c}
0x 1's \\
1x 2 \\
1x 3 \\
1x 4
\end{array} \Rightarrow
\begin{array}{c}
2 \\
3 \\
4
\end{array}
$$</div>

And here's an implementation, in Scala:

~~~scala
object BeadSort {

  def transpose[A](xs: List[List[A]]): List[List[A]] = {
    val nonEmpty = xs.filter(_.nonEmpty)
    if (nonEmpty.isEmpty)
      Nil
    else
      nonEmpty.map(_.head) :: transpose(nonEmpty.map(_.tail))
  }

  def sort(input: List[Int]): List[Int] = {
    val min = input.min
    val xs = input.map(_ - min + 1)
    val cols = transpose(xs.map(List.fill(_)(null))).map(_.size)
    val diffs = cols.zip(cols.tail :+ 0).map{ case (x, y) => x - y }.zipWithIndex
    diffs.map{ case (n, i) => List.fill(n)(i + 1) }.flatten.map(_ + min - 1)
  }

  def main(args: Array[String]): Unit = {
    println(sort(List(1, 3, -4, 3, 6, 2)))
  }

}
~~~

Note that I'm handling negative integers by simply shifting everything up to be at least one, and then shifting back down at the end. This has the side-effect of increasing efficiency for sorting lists with only large positive numbers.

In fact, in some very limited cases, this is faster than the standard sorting algorithms! The bulk of the work here is in the transpose. This puts the run-time at: `\(O\left(\sum_{i=1}^{n}{x_i}\right)\)`

Therefore, if `\(mean(x_i) < O\left(\log{n}\right)\)`, then Bead Sort is asymptotically faster than the usual `\(O(n\log{n})\)` algorithms, like quicksort and merge sort. Much like radix sort, it's not a strict comparison sort, so it doesn't violate any proofs that `\(O(n\log{n})\)` is optimal.
