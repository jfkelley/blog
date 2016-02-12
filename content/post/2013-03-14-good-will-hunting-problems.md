---
title: Good Will Hunting problems
author: joefkelley
layout: post
date: 2013-03-14
url: /293
sfw_pwd:
  - ofs4VLfW1G3O
categories:
  - Math

---
I recently watched the movie Good Will Hunting for the first time. In it, Matt Damon plays a genius janitor that is discovered when he solves two sets of math problems that were left on a chalk board. Surprisingly, those problems were not nearly as difficult as they were portrayed in the movie. Here's the first set, which the camera helpfully pauses on briefly in the movie:

Consider the following graph, G:
<pre>
  4
 / \
1 - 2 = 3
</pre>
  
Forgive the rough representation; there are edges between nodes 1 and 2, 1 and 4, 4 and 2, and two edges between 2 and 3.</p> 

  1. Find the adjacency matrix A of the graph G.
  2. Find the matrix giving the number of 3-step paths in G.
  3. Find the generating function for paths from point i to j.
  4. Find the generating function for paths from points 1 to 3.

he only hard part of the first question is knowing what an adjacency matrix is. For a graph with `\(n\)` nodes, the adjacency matrix `\(A\)` is an `\(n\times n\)` matrix, such that:</p> 

`\(A_{ij}=\text{number of edges between nodes }i\text{ and }j\)`
  
So in this case, we have:

<div>$$
A=\begin{bmatrix}
0&1&0&1\\
1&0&2&1\\
0&2&0&0\\
1&1&0&0
\end{bmatrix}
$$</div>
  
The second problem is a little bit trickier. You can probably do it "by hand", but that's error-prone and tedious. Instead, first consider if we wanted the number of 2-step paths from just node `\(i\)` to node `\(j\)`. We could do this by considering every possible intermediate node `\(k\)`, like so:

`\(\sum_{k=1}^{n}(\text{edges between }i\text{ and }k)\times (\text{edges between }k\text{ and }j)=\sum_{k=1}^{n}A_{ik}A_{kj}\)`
  
Conveniently, that's the exact formula for the multiplication of two matrices. So the matrix whose entries give the number of 2-step paths between each pair of nodes is simply `\(AA=A^2\)`. And it's not hard to show by induction that for a `\(p\)`-step path, it's `\(A^p\)`. So the answer to the second question is:

<div>$$
A^3=\begin{bmatrix}
2&7&2&3\\
7&2&12&7\\
2&12&0&2\\
3&7&2&2
\end{bmatrix}
$$</div>
  
The last two questions are really the same; the fourth is just a specific case of the third. The generating function for a series `\(a_k\)` is defined by: `\(f(x)=\sum_{k=0}^{\infty}a_kx^k\)` 

These functions have a number of interesting properties, with perhaps the most commonly-used being that, given a generating function, the original sequence can be recovered by taking derivatives: 

<div>$$
a_k=\frac{f^{(k)}(0)}{k!}
$$</div>

Anyway, for the specific case of paths of length `\(k\)` from `\(i\)` to `\(j\)`, the generating function is:

`\(f_{ij}(x)=\sum_{k=0}^{\infty}(A^k)_{ij}x^k\)`
  
While that's technically a correct answer, it's useless unless simplified. First, let's generalize this to a generating function for the entire matrix of path counts:

<div>$$
\begin{align}
F(x)&=\sum_{k=0}^{\infty}A^kx^k\\
F(x)Ax&=\sum_{k=0}^{\infty}A^{k+1}x^{k+1}\\
F(x)Ax&=\sum_{k=1}^{\infty}A^kx^k\\
F(x)Ax+A^0x^0&=A^0x^0+\sum_{k=1}^{\infty}A^kx^k\\
F(x)Ax+I&=\sum_{k=0}^{\infty}A^kx^k\\
F(x)Ax+I&=F(x)\\
F(x)(I-Ax)&=I\\
F(x)&=(I-Ax)^{-1}
\end{align}
$$</div>

And so, the final specific generating function for paths from `\(i\)` to `\(j\)` is simply:

<div>$$
f_{ij}(x)=(F(x))_{ij}=((I-Ax)^{-1})_{ij}
$$</div>
  
And if you churn through the matrix inverse calculation, the answer for `\(i=1\)` and `\(j=3\)` is: `\(f_{1 3}(x)=\frac{2x^2+2x^3}{1-7x^2-2x^3+4x^4}\)`
