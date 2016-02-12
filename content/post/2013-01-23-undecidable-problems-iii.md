---
title: Undecidable Problems III
author: joefkelley
layout: post
date: 2013-01-24
url: /215
sfw_pwd:
  - AxjyjyEDU6eM
categories:
  - Math
  - Programming

---
In my [second undecidable problems post][1], I talked about using a hypothetical solution to the Halting Problem to solve open problems in math. In this post, I'll go the other direction, and use the non-existence of such a solution to prove a shocking fact: not only are there problems which cannot be computed, there are individual numbers that cannot be computed.

First, define a function `\(f\)` as "computable" if we can write a program to compute it for all inputs in its domain.

Next, define another function `\(F\)` as "universal", if for every computable function `\(f\)`, there exists an "encoding" of that function, `\(w\)`, such that `\(F(wx)=f(x)\)` for all `\(x\)` in the domain of `\(f\)`. Here, we've kind of defined `\(w\)` as the implementation of a program that computes `\(f\)`, and `\(F\)` as an environment that runs `\(w\)`. In concrete terms, you can think of them as program and operating system, respectively, or script and interpreter.

As an example, let `\(f(x)=3x^{2}\)`. Say we encode that function as "`\(111000\)`". Then, `\(F(1110000)=f(0)=0\)`, `\(F(1110001)=f(1)=3\)`, and `\(F(11100010)=f(2)=12\)`. Note that I'm representing the input to `\(f\)` as a binary number that is simply appended to the encoding of the function.

Finally, call a set of encodings "prefix-free" if there are no two such that one is a strict continuation of the other. For example, {11,110,101} would not be prefix-free because 110 is a continuation of 11. A property of such a set is that its size is less than or equal to `\(2^n\)`, where n is the maximum length of any member of the set. You can see this by considering a set of every element of length exactly `\(n\)`. Such a set has exactly `\(2^n\)` elements, and if you wish to add any shorter elements to the set, you must remove at least two longer ones to maintain it being prefix-free.

Now, given a universal function `\(F\)`, whose domain, `\(D_{F}\)` is prefix-free, consider the constant: `\(\Omega_{F}=\sum_{p \in D_{F}}2^{-|p|}\)` 

First of all, this number exists because an `\(F\)` that fits our conditions exists. The construction of that `\(F\)` is very complicated, but possible.

Second of all, it's finite. Based on our earlier statement about the domain being prefix-free, and the restriction on the size of such a domain, we can say that it is between the values of 0 and 1. Things get a little hairy to prove rigorously, since this set is infinite, but the result still holds.

Now, let's say we want to know the solution of the Halting Problem for some program-and-input combination that is `\(N\)` bits long. Assume we know the first `\(N\)` bits of `\(\Omega_{F}\)`. What we can do is simultaneously run all programs computing `\(F(p)\)` for every `\(p\)`. Whenever a program of length `\(k\)` terminates, we add `\(2^{-k}\)` to a running total. When the first `\(N\)` bits of this total reaches the known bits of `\(\Omega_{F}\)`, then we stop. If the program-and-input combination we are testing has not yet finished, then it will never finish. Otherwise, it finishing would affect the first `\(N\)` bits of the total. And so, we cannot have a program that can compute the bits of `\(\Omega_{F}\)`, because such a program would allow us to solve the Halting Problem. In fact, there are certain `\(F\)` where even the first bit cannot be computed.

Personally, I find this extremely strange. There's a number that is perfectly well defined, but we cannot even compute its first digit. It lies between 0 and 1, but we can't get any closer than that.

 [1]: /191
