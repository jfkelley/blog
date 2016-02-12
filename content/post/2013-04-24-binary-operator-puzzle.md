---
title: Binary Operator Puzzle
author: joefkelley
layout: post
date: 2013-04-24
url: /362
sfw_pwd:
  - l4yQbvoGE8Rj
categories:
  - Puzzle

---
Simple puzzle: can you come up with a binary operator over integers that is commutative and associative, but has no identity element? For example, addition is commutative because `\(x+y=y+x\)` and associative because `\((x+y)+z=x+(y+z)\)`, but it does not satisfy the puzzle because `\(0\)` is an identity element: `\(x+0=x\)`. Likewise, multiplication does not work because `\(1\)` is an identity element. `\(\text{ }f(x,y)=xy+1\)` is commutative and has no identity element, but is not associative. There is at least one (and probably more) trivial, degenerate solution(s), so I'll give another restraint: the range of the operator must also be the entire set of integers.

If you prefer a more formal phrasing, find `\(f:\mathbb{Z}\times\mathbb{Z}\rightarrow\mathbb{Z}\)` such that:

<div>$$
\forall x,y \in \mathbb{Z} : f(x,y)=f(y,x) \quad \text{(commutative)}\\  
\forall x,y,z \in \mathbb{Z} : f(f(x,y),z)=f(x,f(y,z)) \quad \text{(associative)}\\
\forall x \in \mathbb{Z}: \exists y \in \mathbb{Z} \text{ such that } f(x,y)\neq x \quad \text{(no identity)}\\
\forall x \in \mathbb{Z} : \exists y,z \in \mathbb{Z} \text { such that } f(y,z)=x \quad \text{(full range)}
$$</div>

Update: [solutions posted][1]

 [1]: /383
