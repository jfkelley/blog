---
title: Binary Matrix Proof Puzzle
author: joefkelley
layout: post
date: 2014-10-23
url: /743
sfw_pwd:
  - 3tUjQjTfdMVA
categories:
  - Math
  - Puzzle

---
This puzzle is one of the hardest I've ever solved, but it's a surprisingly elegant and creative solution. Shout-out to Mark Liu, who I originally heard it from. I'll warn you ahead of time - it probably requires some Math/CS theory.

Consider a matrix with the following constraints:

1. It contains only 1s and 0s
2. It is wider than it is tall (more columns than rows)
3. There is at least one 1 in each column

Prove that there exists a 1 at some position in the matrix, for which there are more 1s in that row than in that column. For example, if there is a 1 at row-i, column-j, then there are more 1s in row-i than in column-j.

Stop reading here if you want to try to solve it yourself.

First, simplify the problem slightly by removing rows in the matrix with no 1s. Notice this new matrix has all the same constraints, and proving the result in the new matrix is the same as proving it in the original.

The crucial step in how I solved this was to then translate this to a graph problem. Call the matrix `\(A\)`, let it have `\(m\)` rows and `\(n\)` columns, with `\(m < n\)`. Then consider a graph with one vertex for every row (call this set of vertices `\(M\)`) and one vertex for every column (call this set `\(N\)`). And let the set of edges (`\(E\)`) connect `\(M_i\)` to `\(N_j\)` if and only if `\(A_{ij}=1\)`. That is, the vertex for a row is connected to the vertex for a column if there's a 1 in that row and that column. Now the problem can be restated as: prove that there exists an edge `\((M_i,N_j)\)` such that `\(degree(M_i) > degree(N_j)\)`.

One thing to note about this graph is that every vertex has at least one connected edge. This is a consequence of constraint 3, and the simplification of removing rows with no 1s. So that means that the number of vertexes can be "counted" by summing over edges, like this:
<div>$$
|M|=\sum_{(M_i,N_j) \in E}{\frac{1}{degree(M_i)}}
$$</div>

For example, a vertex with 3 edges will contribute 3 terms to the sum, each 1/3. So it contributes 1 overall. This is true of all vertices.

With that in mind, assume the opposite of what we seek to prove: for all edges, `\((M_i,N_j)\)`, `\(degree(M_i) \leq degree(N_j)\)`. An immediate consequence of this is: `\(\frac{1}{degree(M_i)} \geq \frac{1}{degree(N_j)}\)`

Then:


<div>$$
\begin{align}
m=|M|=\sum_{(M_i,N_j) \in E}{\frac{1}{degree(M_i)}} &\geq \sum_{(M_i,N_j) \in E}{\frac{1}{degree(N_j)}}=|N|=n \\
m &\geq n
\end{align}
$$</div>

But that's a contradiction! The assumption is incorrect. QED.

It's possible that you could apply the same logic without translating this to a graph problem, but I think it makes the notation and summations uglier and less intuitive.
