---
title: Mersenne Primes
author: joefkelley
layout: post
date: 2013-02-07
url: /251
sfw_pwd:
  - JKqS6Ojz0c3m
categories:
  - Math
  - Programming

---
About a week ago, the largest known prime number was discovered. It has over 17 million digits, so I'm not going to post it here directly, but I will post a simpler representation: `\(2^{57,885,161}-1\)`. This prime is more than four million digits longer than the previous record-holder. Think about how big that number is for a second... the number of particles in the universe has only about 80 digits. The number of different possible chess games has about 120 digits. Those are not even in the same league as this prime number. Pretty impressive, eh? I'll explain a bit more about the math behind how it was found.

Numbers of this form are actually very well studied; they are known as "Mersenne numbers", named after a French monk who studied them in the 1600s. They are defined by:

`\(M_n=2^n-1\)`
  
There is an ongoing project called the "Great Internet Mersenne Prime Search" whose goal is to find Mersenne numbers that are also prime, and in fact it was this project that found the new record-holder. They specifically search Mersenne numbers becase they have two interesting relationships with prime numbers.

First, `\(M_n\)` can only be prime if `\(n\)` is prime. Here's a brief proof:

Assume `\(n\)` is composite. Then, let `\(n=ab\)`, where `\(a\)` and `\(b\)` are integers greater than 1.

<div>$$
\begin{align}
2^n-1&=2^{ab}-1 \\
2^n-1&=\sum_{k=0}^{ab-1}2^k \\
2^n-1&=\sum_{i=0}^{b-1}(\sum_{j=0}^{a-1}2^{ai+j}) \\
2^n-1&=\sum_{i=0}^{b-1}2^{ai}(\sum_{j=0}^{a-1}2^j) \\
2^n-1&=\sum_{i=0}^{b-1}2^{ai}(2^a-1) \\
2^n-1&=(2^a-1)(\sum_{i=0}^{b-1}2^{ai}) \\
\end{align}
$$</div>

`\(a\)` and `\(b\)` are both integers, so `\((2^a-1)\)` and `\((\sum_{i=0}^{b-1}2^{ai})\)` are also integers. Therefore, `\(2^n-1\)` is composite.


That proof is a little bit hairy and hard to follow, so here's a more intuitive version. Consider the binary representation of a Mersenne number, such as `\(M_9=2^9-1\)`:
    
`\(M_9=111111111\)`
  
Each Mersenne number `\(M_n\)` has a binary representation consisting of `\(n\)` 1s.
  
Now one simple way of factoring (still in binary) `\(M_9\)` is like so:
    
`\(M_9=111111111=(111)(1001001)\)`
  
Notice that this works essentially because we can split the 1s into three groups of three. This works with any composite number of ones. More examples:
    
`\(M_{10}=1111111111=(11)(101010101)\)`
  
`\(M_{12}=111111111111=(1111)(100010001)\)`
  
`\(M_{15}=111111111111111=(11111)(10000100001)\)`
    
Thus, if `\(n\)` is composite, `\(M_n\)` will always be factorizable in this way.
    
This fact about Mersenne numbers is useful in searching for primes because it allows for most Mersenne numbers to be discarded very easily. For example, determining if `\(1125899906842623\)` is prime or composite might be difficult, but if I tell you it's equal to `\(2^{50}-1\)`, it's clearly composite, because `\(50\)` is composite. Even for very large Mersenne numbers, the exponent stays relatively small, and can be easily checked for primality.
    
However, if the number `\(n\)` _is_ prime, then that does not necessarily mean that `\(M_n\)` is also prime, and in fact we have to do a more intensive check. This leads us to the second key fact about Mersenne numbers: there exists an efficient algorithm for checking if they are prime, known as the Lucas-Lehmer primality test. It is:
    
Let `\(x_0 = 4\)` and `\(x_i = x_{i-1}^2-2\)`
  
then, `\(M_n\)` is prime if and only if `\(x_{n-2}\)` is divisible by `\(M_n\)`.
    
The proof of this is complicated and unsatisfying, so check [Wikipedia][1] if you want to see it.
    
With a little bit of modular arithmetic, it can be shown that it is sufficient to generate only `\(x_i\bmod{M_n}\)` instead of the full representation of each term in the series. This keeps execution time low, and in fact it is significantly faster than the fastest-known generalized primality test.
    
With all of these in mind, here's a simple Python script that searches for Mersenne primes. In about 45 seconds on my laptop, it finds `\(M_{4423}\)`, which was the largest known prime as recently as 1961, and the first known prime with over 1000 decimal digits! After 20 minutes, it found `\(M_{11213}\)`, which has 3376 digits. The actual Great Internet Mersenne Prime Search uses a much, much more optimized version of this algorithm, and it's distributed across thousands of computers, but the basic principles are there.
    
~~~Python
import time

def main():
	n = 1
	start = time.time()
	while True:
		if mersenneIsPrime(n):
			print "M" + str(n) + "=" + str(mersenne(n))
			elapsed = time.time() - start
			print "\tfound in %.5fs" % elapsed
			print ""
		n = n + 1

def mersenne(n):
	return 2 ** n - 1

def mersenneIsPrime(n):
	if not simpleIsPrime(n):
		return False
	if n == 1:
		return False
	if n == 2:
		return True
	m = mersenne(n)
	x = 4
	for i in range(n-2):
		x = (x * x - 2) % m
	return x == 0

def simpleIsPrime(n):
	if n == 1:
		return False
	div = 2
	while div * div &lt;= n:
		if n % div == 0:
			return False
		div = div + 1
	return True

main()
~~~

 [1]: http://en.wikipedia.org/wiki/Lucas-Lehmer_primality_test#Proof_of_correctness
