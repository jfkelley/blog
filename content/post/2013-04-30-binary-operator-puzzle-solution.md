---
title: 'Binary Operator Puzzle - Solution'
author: joefkelley
layout: post
date: 2013-04-30
url: /383
sfw_pwd:
  - D2v5LwKWiLTW
categories:
  - Puzzle

---
Here's one possible solution to [the puzzle I posted last week.][1] The task was to find a commutative and associative function with no identity element. I had hoped to post a more clever solution, but found some small errors in edge-cases and technicalities to everything I had planned on posting! But still, here's a solution that _does_ work, even if it's a little ugly:

<div>$$
f(x,y)=\left\{ \begin{array}{rl}
x+y &\mbox{ if }x \neq 0, y \neq 0 \\
0 &\mbox{ otherwise}
\end{array} \right.
$$</div>

Or similarly:

<div>$$
f(x,y)=\left\{ \begin{array}{rl}
xy &\mbox{ if }x \neq 1, y \neq 1 \\
1 &\mbox{ otherwise}
\end{array} \right.
$$</div>
 [1]: /362
