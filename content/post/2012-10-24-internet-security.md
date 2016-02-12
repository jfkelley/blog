---
title: Internet Security
author: joefkelley
layout: post
date: 2012-10-24
url: /83
sfw_pwd:
  - kvtDbdwM0qJe
categories:
  - Math
  - Tech

---
In a previous post, I explained the basics of how the infrastructure of the internet works to enable communication from one computer to another. This post will try to explain the basics of how sensitive information makes it all the way across the internet, with only the intended recipients able to read it.

I'd like to first note that I'll only be covering the basics of one of four principles of information security. All of them are:

  1. Authenticity - that all parties are who they claim to be
  2. Integrity - that third parties cannot tamper with or modify a message, and that it arrives at its recipient as-is
  3. Availability - that no party can prevent any other from sending or receiving information
  4. Confidentiality - that only the intended recipient can view the contents of a message

It's the last one, confidentiality, that I find most interesting, and that I'll be covering in this post. Internet packets have to travel through many different computers on the way to their destination, so what stops any of those intermediaries from viewing the packets? The answer is encryption. The sender first does some secret transformation of the message before sending it, and then the receiver does essentially the inverse of that transformation in order to recover the original message. Just to introduce some terminology: the plain, readable message is called the "cleartext" or "plaintext". The sender then "encrypts" that text. The result of the encryption is called the "ciphertext", and then the receiver "decrypts" the message to recover the cleartext.

The simple example that most people are familiar with is commonly called "ROT13". The process is pretty simple: convert each letter of the alphabet to a number, 1-26, then add 13 to that number, wrapping around to 1 if necessary, and convert back to letters. Here's an example:

<pre>
cleartext:   R  E  D     W  A  G  O  N
numbers:     18 5  4     23 1  7  15 14
+13:         31 18 17    36 14 20 28 27
wrap around: 5  18 17    10 14 20 2  1
ciphertext:  E  R  Q     J  N  T  B  A
</pre>

So the final message that is actually sent is "ERQ JNTBA". To decrpyt the message, the same operation is applied again, except with -13 instead of +13.

This is pretty useless, though. Any middleman could pretty quickly figure out that ROT13 was being used and simply decrypt the message himself. As the name implies, you could also do ROT1, ROT2, etc., but again, that's pretty easy to crack. There's only 25 different possiblities to try.

So let's try to be a little more clever. What if we add a different amount to each letter - then there would be many more different possibilities. Let's say we pick a sequence of ten numbers: that's already `\(25^{10}\)` combinations to try, which is about ten trillion, which is more than a computer could handle. But for the sake of an example, let's just pick four: 4, 10, 22, 7. Then our encryption goes as follows:

<pre>
cleartext:   R  E  D     W  A  G  O  N
numbers:     18 5  4     23 1  7  15 14
sequence:    4  10 22    7  4  10 22 7
+:           22 15 26    30 5  17 37 21
wrap around: 22 15 26    4  5  17 11 21
ciphertext:  V  O  Z     D  E  Q  K  U
</pre>

To decrypt, the sender would do a similar thing, but subtracting each number in the sequence instead of adding.

Already, we have a pretty secure encryption algorithm. Even if a middleman knew the process, he would still need the chosen sequence to do anything. This concept is called a "key", and it's central to practical encryption. This sequence (4, 10, 22, 7) is what we call the key in this case. What this implies is that the sender and receiver can publicly discuss what kind of encryption algorithm they are going to use, and then as long as they don't share the key with anyone else, it is still secure.

But this raises another problem: how to make sure both the sender and the receiver can agree on the same private key. If they transmit it un-encrypted, an attacker can just get the key then, and use it to look at any future encrypted traffic. And they can't send it encrypted... because how are they going to encrypt it; it's a chicken-and-egg problem. Sometimes, the key is transmitted over a different, secure channel. For example, at a company I used to work for, we used encrypted instant messaging, and I had to be told a key over the phone, type it into the program, and was then able to communicate with others in the company over IM. But this is cumbersome and not really feasible in many situations.

The solution is called public-key cryptography. There's a simple idea behind it, but it's tricky to implement. Each party maintains two keys: a public one and a private one. They share the public key with anyone who asks for it. The trick is that the public key is used for encryption, but the private key is used for decrypting. So if Alice wants to send a message to Bob, Alice encrypts the message with Bob's public key, and then Bob decrypts it using his own private key.

The trick is two-fold. First, the coming up with an encryption algorithm that acts like this at all is not easy. And second, it must be impossible to determine the private key from the public key. But because one is used to encrypt, and one to decrypt, they must be related in some way, so this is actually easier said than done.

There are actually a couple of algorithms that have been used for this, but the simplest, and most wide-spread is called RSA, and it works like this:

First of all, we need to define the "modulus" operator, abbreviated as "`\(\bmod\)`". It's equivalent to the remainder when dividing integers. So for some examples, `\(6\bmod{5}=1\)`, `\(17\bmod{6}=5\)`, and `\(20\bmod{4}=0\)`

With that in mind, generating public and private keys works as follows:

  1. Choose two prime numbers, `\(p\)` and `\(q\)`.
  2. Compute `\(n=pq\)`
  3. Choose an integer `\(e\)`, such that `\(e\)` and `\((p-1)(q-1)\)` are coprime. That is, they share no common factors.
  4. Compute `\(d\)` such that `\(de\bmod{(p-1)(q-1)}=1\)`
  5. The public key consists of `\(n\)` and `\(e\)`. The private key is `\(d\)`.

To encrypt a message:

1. Convert the message into a single number, call it `\(m\)`
2. Compute the ciphertext as a number, `\(c\)` as `\(c=m^e\bmod{n}\)` 
3. Convert `\(c\)` to text.

The method of converting between text and a number doesn't have to be fancy. For example, we could just cram together the number of every letter, like so: REDWAGON -> 18,5,4,23,1,7,15,14 -> 1805042301071514. Or, we could simply split it up into two messages, 180504, and 2301071514, and send them separately, one after the other. The specifics aren't relevant.
    
To decrypt a message:

1. Convert the cyphertext into a number, `\(c\)`
2. Compute the cleartext number as `\(m=c^d\bmod{n}\)`
3. Convert `\(m\)` back to text
    
It's a little bit more involved to actually prove why this works. As usual, if you want to learn more about that part, [Wikipedia has the answers.][1]
    
Let's walk through an example.
    
1. I'll choose `\(p=3\)` and `\(q=11\)` (normally much bigger prime numbers are used.)
2. `\(n=pq=33\)`
3. I'll choose `\(e=7\)` since `\(7\)` and `\((11-1)(3-1)=20\)` have no common factors
4. `\(d=3\)` since `\(de\bmod{(p-1)(q-1)}=21\bmod{20}=1\)`
5. So private key: `\(d=3\)`, and public key: `\(n=33, e=7\)`
    
Now let's say I just want to send a single letter, "H"
    
1. Convert "H" to a number: `\(m=8\)`
2. `\(c=m^e\bmod{n}=8^7\bmod{33}=2\)`
3. Convert 2 to a letter: "B"
    
So the ciphertext is "B", which can be transmitted safely. To decrypt:
    
1. Convert "B" to a number: `\(c=2\)`
2. `\(m=c^d\bmod{n}=2^3\bmod{33}=8\)`
3. Convert 8 to a letter: "H"
    
Voila! The cleartext message is recovered. Now what if you want to attack this? `\(n\)` and `\(e\)` are public, but `\(p\)`, `\(q\)`, and `\(d\)` are not. Given `\(n\)`, you could actually factor it to recover `\(p\)` and `\(q\)`, and then use those to calculate `\(d\)`. However, this is known to be a very difficult problem when dealing with very large numbers. Factorizing a number into its two prime components could take billions of years on even a modern computer, if the numbers are large enough. And finding very large primes is comparatively easy.
    
However, this whole process is still pretty computationally difficult, and so in practice, often a further optimization is used: a public-key process is only used for a short time, to exchange a shared, one-time-use private key, which is then used with something more similar to the original idea of adding a sequence of numbers. This is only used to solve the earlier chicken-and-egg problem, not for the whole stream of communication.
    
I'll sum this all up by saying that a shockingly low number of websites and other web applications actually use this! A surprising amount of sensitive information is transmitted every day completely un-encrypted. I could realistically sit in a coffee shop, on public wifi, and easily see a dozen people's passwords in a few hours. The only guarantee that your data is secure is if you see an "https" in your browser's address - that means everything is being encrypted. Secure wifi will also provide encryption, but only within that wifi network. Once the traffic leaves the wifi router, it's being sent as cleartext. Luckily, it's harder for hackers to get access to the infrastructure beyond the router, but theoretically possible. Personally, I never type in a password or anything sensitive on an unsecured wifi network, and never do any really important stuff (mostly banking) without checking that it is, in fact, https.

 [1]: http://en.wikipedia.org/wiki/RSA_encryption#Proofs_of_correctness
