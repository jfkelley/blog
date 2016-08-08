---
title: Byzantine Generals Riddler
author: joefkelley
layout: post
date: 2016-08-08
url: /905
categories:
  - puzzle
  
---

Every Friday, FiveThirtyEight.com posts a probability/statistics riddle as part of their ["Riddler" column](http://fivethirtyeight.com/tag/the-riddler/). I highly recommend them, they are usually quite tricky, but often very clever.

Anyway, I believe the solution to last week's riddle was wrong! Well, not wrong technically, but not optimal. Here's the problem:

> You are the emperor of Byzantium (lucky you!) and you have N generals working for you. You know that more than half of your generals are loyal, and the rest are traitors. You can ask any general about the loyalty of any other general: If the general you ask is loyal, he will answer correctly, but if he is a traitor he can answer however he likes. Your goal is to find one general you are absolutely certain is loyal while asking the fewest possible questions.
> 
> 
> What is the minimum number of questions (in terms of N) that will guarantee a solution, and what strategy produces it?

I had actually heard this quesiton before, albeit with software engineers and project managers isntead of loyal and traitorous generals, respectively.

The given solution required **N-1** questions to find a loyal general among N total generals. You can find the details [here.](http://fivethirtyeight.com/features/should-the-grizzly-bear-eat-the-salmon/)

However, I think it's possible to do it in **N-2** at worst! And often fewer than that.

The basic idea is as follows:

Pair up all of the generals, and ask the first in each pair about the loyalty of the second. If the first says the second is loyal, keep the second general. Discard everyone else. Repeat on the new pool of generals until you are down to only 1.

The pairs can be (Loyal, Traitor), (Traitor, Loyal), (Traitor, Traitor), or (Loyal, Loyal). If it is (L,T) then he will be correctly called a traitor and discarded. If it is (T,L), then the traitor has no chance of being picked. So in order to keep trators in the pool, they must be in (T,T) pairs. However, there must be more (L,L) pairs than (T,T) pairs. This is because (L,T) and (T,L) pairs don't contribute to the overall balance of traitors vs loyal, so in order to have more total loyal individuals, you must have more (L,L) than (T,T) pairs. This alone means the resulting pool has more loyal than traitorous generals, but there could even be additional loyal generals from (T,L) pairs.

A cursory look might imply this only gets you to N-1 questions. At each level, we are asking N/2 questions and eliminating at worst N/2 generals. So to elimnate N-1 total, we must ask N-1 questions. But there are two details to consider:

Firstly, when you are down to 3 generals, you only need to ask one question. There is at most one traitor. So ask general 1 about general 2. If he says he's loyal, then pick general 2. If he says he's a traitor, then one of either general 1 or 2 is the traitor, so pick general 3. This alone will get you down to N-2 questions total. Actually, the same logic applies if you have 4 generals, so in that case it's N-3.

Second, handling odd numbers of generals is a bit tricky, but actually an opportunity to shave off a few more questions. In this case, set aside one general, and go through the pairing process. If you end up with an even number of generals left, add the one set aside into the continuing pool. If you end up with and odd number left, just remove the one set aside.

If the set-aside general is a traitor, then the pairing procedure still works as argued. If you end with an odd number, you know you have more loyal than traitors, and you throw away the traitor. If you end with an even number, then you actually know you must have at least 2 more loyal than traitors, so adding the set-aside traitor back in doesn't change your overall balance of having more loyal.

If the set-aside general is loyal, then the pairing procedure might have an equal number of loyal and traitorous generals instead of strictly more loyal. But the same logic applies to this case - you must have at least as many loyal as traitors at the end of it. If you end up with an odd number, they can't be equal, so you must have more loyal. You can discard the set-aside loyal general without worry. If you end with an even number, you might have equal loyal and traitors, but adding the loyal general back in will restore the property of having more loyal.

This addendum saves you a single question every time you start with an odd pool, do the pairing procedure, and end with an odd number of generals. For some numbers of generals, this never happens, but for those of the form 4x+3, it will always save you one question.

I've yet to find a nice way of putting exactly how many questions this strategy requires with respect to N. At wost, it's **N-2**. Every number that can be written `\(N=2^k+1\)` exhibits this behavior.

At best, though, it saves a logarithmic number of additional questions. If you start with `\(N=2^k-1\)` generals, then you must ask just `\(N-k\)` questions.