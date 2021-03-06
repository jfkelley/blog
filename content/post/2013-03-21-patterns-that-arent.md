---
title: "Patterns that aren't"
author: joefkelley
layout: post
date: 2013-03-21
url: /330
sfw_pwd:
  - 6r3cEl9NVQh9
categories:
  - Math

---
In eighth grade, a friend of mine discovered an interesting pattern: start with the number 11, add 2, then add 4, then 6, etc. It appears that this generates prime numbers! He showed me the first few numbers in the series:

`\(11, 13, 17, 23, 31, 41, 53, 67...\)`
  
I was amazed. All of those are indeed prime numbers! But look what happens if you go a little further...

`\(...83, 101, 121\)`
  
And `\(121\)` is not a prime number. It is, of course, equal to `\(11^2\)`. Oh well, our dreams of becoming famous mathematicians were crushed by that 11th term in the sequence. In fact, this is a well-known sequence. The terms are of the form `\(2n^2-4n+13\)`, and there are actually many other similar polynomials that also generate primes for a while. For example, take a look at `\(n^2+n+41\)`. Here are the first fifteen terms:

`\(41, 43, 47, 53, 61, 71, 83, 97, 113, 131, 151, 173, 197, 223, 251, 281\)`
  
Those are all prime numbers. In fact, it would be quite tedious to check by hand that it eventually stops generating primes. The first time this happens is at `\(n=41\)`, which is `\(1763=41 \times 43\)`

All of this just shows the importance of rigorous proofs. Just because something "looks like" it works, it doesn't mean it actually does. Computers have helped somewhat with this, as they make checking patterns like this much easier, at least to disprove. A computer could show that the above polynomials fail eventually in just a few milliseconds. But sometimes even computers aren't enough. Consider the two numbers:

`\(n^{17}+9 \text{ and } (n+1)^{17}+9\)`
  
Are they always relatively prime? That is, is their greatest common factor always 1? It appears so. If you were to ask a computer to check this, it actually would seem to agree. The computer could sit there, churning away, checking each `\(n=1,2,3...\)` in order, for years, and never find a counter-example. But it's actually not true. The first value of `\(n\)` that doesn't work is this number:

`\(8424432925592889329288197322308900672459420460792433\)`
  
Another cool idea in a similar vein is an "almost integer", a number that is very close to an integer. Here's an example:

`\(\sin{(2017\sqrt[5]{2})}=-0.9999999999999999785...\)`
  
And a bit more involved one:

`\(\text{let }X=\sum_{n=1}^{\infty}\frac{x_n}{n}\text{ where } x_n=1\text{ or }-1\text{ with equal probability}\)`
  
And consider the [probability density function][1] for `\(X\)`, call it `\(f_X(x)\)`.

`\(\text{then }100f_X(2)=124.9999999999999999999999999999999999999997642...\)`
  
Again, just because something looks equal, or seems to be a pattern, it doesn't mean that it actually is!

![][2]

 [1]: http://en.wikipedia.org/wiki/Probability_density_function
 [2]: http://imgs.xkcd.com/comics/e_to_the_pi_minus_pi.png
