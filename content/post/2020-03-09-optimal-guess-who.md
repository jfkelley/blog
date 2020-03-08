---
title: Optimal Guess Who
author: joefkelley
layout: post
date: 2020-03-09
url: /906
categories:
  - Math
  - Programming
  
---

This week's Riddler on FiveThirtyEight involved determining an optimal strategy for the game Guess Who. The resulting solution was a bit surprising to me since I had always assumed you would want to cut your remaining candidates in half, but that turned out to not be the case. So I'll walk through my derivation of the strategy and give a table that shows what you should do in every situation to play optimally.

First, the rules: each player secretly picks one person from a set of 24. You take turns asking yes-or-no questions about the other player's chosen person until you are able to correctly guess the individual. Players maintain a set of possible candidates and eliminate ones that don't match the answers as they go. Some versions of the rules only allow you ask certain types of questions, but I'm going to give a strategy that assumes you can ask any sort of question. Concretely, the easiest system to implement in person is to just order the candidates alphabetically and ask questions like "Is their name alphabetically before \<name\>?".

Note that the rules state that if you ask a question that narrows it down to only one candidate, but the question was not "Is your person \<name\>?", then you have not won yet and must specifically ask about that person on your next turn. But this is irrelevant for optimal play since you would clearly never ask a question that could narrow it down to only one. It's always better to just ask if it is that candidate and potentially win one turn sooner.

The common but wrong way to approach this is to minimize your expected total number of questions. This leads to always trying to cut the pool in half. To see why this does not work, imagine a situation in which you are far behind: maybe you have 16 left and your opponent is down to 2. They are guaranteed to win in either one or two turns. If you try to ask balanced questions, you'll cut it down to 8 and then 4, and surely lose. But if you just guess one person at random, you have a small but non-zero chance of winning.

So rather than try to calculate expected values, we have to just calculate the probability of winning directly. Let's define `\(p(x,y)\)` to be your probability of winning if you have `\(x\)` candidates remaining, your opponent has `\(y\)` candidates remaining, and it is your turn. If you are in this situation, but it is not your turn, then your opponent is faced with the flipped situation, and so your probability of winning is `\(1-p(y,x)\)`.

The first type of move you can make is to just ask if it is some specific person. In this case, you win immediately with probability `\(\frac{1}{x}\)`, and move on with one fewer person in the candidate pool with probability `\(\frac{x-1}{x}\)`. If this happens, your opponent is now faced with `\(y\)` candidates to your `\(x-1\)`, so they will win with probability `\(p(y,x-1)\)`, meaning you win with probability `\(1-p(y,x-1)\)`. Your overall win probability with this type of move is therefore:
<div>$$
\frac{1}{x}+\frac{x-1}{x}(1-p(y,x-1))
$$</div>

The other type of move you can make is to ask some question that narrows it down to some `\(n\)` candidates, where `\(n > 1\)`. The answer being "yes" happens with probability `\(\frac{n}{x}\)` and leaves you with probability `\(1-p(y,n)\)` of winning afterward. The answer being "no" happens with probability `\(\frac{x-n}{x}\)` and leaves you with probability `\(1-p(y,x-n)\)` of winning. Overall, that is:

<div>$$
\frac{n}{x}(1-p(y,n))+\frac{x-n}{x}(1-p(y,x-n))
$$</div>

Finally, you should choose whatever move maximizes your probability of winning, and doing so gives you your overall probability in that situation. Specifically:

<div>$$
p(x,y) = \max_{1 \leq n < x}
\left\{ \begin{array}{lr}
\frac{1}{x}+\frac{x-1}{x}(1-p(y,x-1)) & \text{if } n=1 \\
\frac{n}{x}(1-p(y,n))+\frac{x-n}{x}(1-p(y,x-n)) & \text{if }n>1 \\
\end{array} \right.
$$</div>

It's not terribly hard to translate this to recursive Scala code, with a bit of extra tuple-ing/untuple-ing to keep track of the optimal questions as well as the win probability:

~~~scala
import spire.implicits._
import spire.math._

def p(x: Int, y: Int): (Int, Rational) = {
  if (x == 1) {
    (1, Rational(1))
  } else {
    (1 to x/2).map { n =>
      val prob = if (n == 1) {
        Rational(1, x) + Rational(x-1, x) * (Rational(1) - p(y, x-1)._2)
      } else {
        Rational(n, x) * (Rational(1) - p(y, n)._2) +
	      Rational(x-n, x) * (Rational(1) - p(y, x-n)._2)
      }
      (n, prob)
    }.maxBy(_._2)
  }
}
~~~

But this does do a lot of recalculation further down the recursion. Probably best would be to reframe the code using dynamic programming, but the quick-and-dirty solution is just to use a global cache:

~~~scala
import spire.implicits._
import spire.math._
import scala.collection.mutable

val cache = mutable.Map.empty[(Int, Int), (Int, Rational)]

def p(x: Int, y: Int): (Int, Rational) = cache.get((x, y)) match {
  case Some(ans) => ans
  case None => {
    val ans = if (x == 1) {
      (1, Rational(1))
    } else {
      (1 to x/2).map { n =>
        val prob = if (n == 1) {
          Rational(1, x) + Rational(x-1, x) * (Rational(1) - p(y, x-1)._2)
        } else {
          Rational(n, x) * (Rational(1) - p(y, n)._2) +
	        Rational(x-n, x) * (Rational(1) - p(y, x-n)._2)
        }
        (n, prob)
      }.maxBy(_._2)
    }
    cache += (x, y) -> ans
    ans
  }
}
~~~

To save you the effort of running it yourself, here's a table of the results. `\(x\)` is your candidate pool size, `\(y\)` is your opponent's, you should ask a question that could narrow it down to `\(n\)`, and `\(p\)` is your win probability if you do and your opponent plays optimally. Feel free to use it to beat everyone at Guess Who!

<div>$$
\begin{array}{ r | r | r | r }
x & y & n & p \\
\hline
24 & 24 & 12 & \frac{2}{3} \\
24 & 23 & 12 & \frac{15}{23} \\
24 & 22 & 12 & \frac{7}{11} \\
24 & 21 & 12 & \frac{13}{21} \\
24 & 20 & 12 & \frac{3}{5} \\
24 & 19 & 12 & \frac{11}{19} \\
24 & 18 & 12 & \frac{5}{9} \\
24 & 17 & 12 & \frac{9}{17} \\
24 & 16 & 12 & \frac{1}{2} \\
24 & 15 & 12 & \frac{7}{15} \\
24 & 14 & 12 & \frac{3}{7} \\
24 & 13 & 12 & \frac{5}{13} \\
24 & 12 & 6 & \frac{1}{3} \\
24 & 11 & 6 & \frac{7}{22} \\
24 & 10 & 6 & \frac{3}{10} \\
24 & 9 & 6 & \frac{5}{18} \\
24 & 8 & 6 & \frac{1}{4} \\
24 & 7 & 6 & \frac{3}{14} \\
24 & 6 & 3 & \frac{1}{6} \\
24 & 5 & 3 & \frac{3}{20} \\
24 & 4 & 3 & \frac{1}{8} \\
24 & 3 & 1 & \frac{1}{12} \\
24 & 2 & 1 & \frac{1}{16} \\
24 & 1 & 1 & \frac{1}{24} \\
23 & 24 & 11 & \frac{31}{46} \\
23 & 23 & 11 & \frac{349}{529} \\
23 & 22 & 11 & \frac{163}{253} \\
23 & 21 & 11 & \frac{101}{161} \\
23 & 20 & 11 & \frac{14}{23} \\
23 & 19 & 11 & \frac{257}{437} \\
23 & 18 & 11 & \frac{13}{23} \\
23 & 17 & 11 & \frac{211}{391} \\
23 & 16 & 11 & \frac{47}{92} \\
23 & 15 & 11 & \frac{11}{23} \\
23 & 14 & 11 & \frac{71}{161} \\
23 & 13 & 11 & \frac{119}{299} \\
23 & 12 & 6 & \frac{8}{23} \\
23 & 11 & 6 & \frac{84}{253} \\
23 & 10 & 6 & \frac{36}{115} \\
23 & 9 & 6 & \frac{20}{69} \\
23 & 8 & 6 & \frac{6}{23} \\
23 & 7 & 6 & \frac{36}{161} \\
23 & 6 & 3 & \frac{4}{23} \\
23 & 5 & 3 & \frac{18}{115} \\
23 & 4 & 3 & \frac{3}{23} \\
23 & 3 & 1 & \frac{2}{23} \\
23 & 2 & 1 & \frac{3}{46} \\
23 & 1 & 1 & \frac{1}{23} \\
22 & 24 & 10 & \frac{15}{22} \\
22 & 23 & 10 & \frac{169}{253} \\
22 & 22 & 10 & \frac{79}{121} \\
22 & 21 & 10 & \frac{7}{11} \\
22 & 20 & 10 & \frac{34}{55} \\
22 & 19 & 10 & \frac{125}{209} \\
22 & 18 & 10 & \frac{19}{33} \\
22 & 17 & 10 & \frac{103}{187} \\
22 & 16 & 10 & \frac{23}{44} \\
22 & 15 & 10 & \frac{27}{55} \\
22 & 14 & 10 & \frac{5}{11} \\
22 & 13 & 10 & \frac{59}{143} \\
22 & 12 & 6 & \frac{4}{11} \\
22 & 11 & 6 & \frac{42}{121} \\
22 & 10 & 6 & \frac{18}{55} \\
22 & 9 & 6 & \frac{10}{33} \\
22 & 8 & 6 & \frac{3}{11} \\
22 & 7 & 6 & \frac{18}{77} \\
22 & 6 & 3 & \frac{2}{11} \\
22 & 5 & 3 & \frac{9}{55} \\
22 & 4 & 3 & \frac{3}{22} \\
22 & 3 & 1 & \frac{1}{11} \\
22 & 2 & 1 & \frac{3}{44} \\
22 & 1 & 1 & \frac{1}{22} \\
21 & 24 & 9 & \frac{29}{42} \\
21 & 23 & 9 & \frac{109}{161} \\
21 & 22 & 9 & \frac{51}{77} \\
21 & 21 & 9 & \frac{95}{147} \\
21 & 20 & 9 & \frac{22}{35} \\
21 & 19 & 9 & \frac{81}{133} \\
21 & 18 & 9 & \frac{37}{63} \\
21 & 17 & 9 & \frac{67}{119} \\
21 & 16 & 9 & \frac{15}{28} \\
21 & 15 & 9 & \frac{53}{105} \\
21 & 14 & 9 & \frac{23}{49} \\
21 & 13 & 9 & \frac{3}{7} \\
21 & 12 & 6 & \frac{8}{21} \\
21 & 11 & 6 & \frac{4}{11} \\
21 & 10 & 6 & \frac{12}{35} \\
21 & 9 & 6 & \frac{20}{63} \\
21 & 8 & 6 & \frac{2}{7} \\
21 & 7 & 6 & \frac{12}{49} \\
21 & 6 & 3 & \frac{4}{21} \\
21 & 5 & 3 & \frac{6}{35} \\
21 & 4 & 3 & \frac{1}{7} \\
21 & 3 & 1 & \frac{2}{21} \\
21 & 2 & 1 & \frac{1}{14} \\
21 & 1 & 1 & \frac{1}{21} \\
20 & 24 & 8 & \frac{7}{10} \\
20 & 23 & 8 & \frac{79}{115} \\
20 & 22 & 8 & \frac{37}{55} \\
20 & 21 & 8 & \frac{23}{35} \\
20 & 20 & 8 & \frac{16}{25} \\
20 & 19 & 8 & \frac{59}{95} \\
20 & 18 & 8 & \frac{3}{5} \\
20 & 17 & 8 & \frac{49}{85} \\
20 & 16 & 8 & \frac{11}{20} \\
20 & 15 & 8 & \frac{13}{25} \\
20 & 14 & 8 & \frac{17}{35} \\
20 & 13 & 8 & \frac{29}{65} \\
20 & 12 & 6 & \frac{2}{5} \\
20 & 11 & 6 & \frac{21}{55} \\
20 & 10 & 6 & \frac{9}{25} \\
20 & 9 & 6 & \frac{1}{3} \\
20 & 8 & 6 & \frac{3}{10} \\
20 & 7 & 6 & \frac{9}{35} \\
20 & 6 & 3 & \frac{1}{5} \\
20 & 5 & 3 & \frac{9}{50} \\
20 & 4 & 3 & \frac{3}{20} \\
20 & 3 & 1 & \frac{1}{10} \\
20 & 2 & 1 & \frac{3}{40} \\
20 & 1 & 1 & \frac{1}{20} \\
19 & 24 & 7 & \frac{27}{38} \\
19 & 23 & 7 & \frac{305}{437} \\
19 & 22 & 7 & \frac{13}{19} \\
19 & 21 & 7 & \frac{89}{133} \\
19 & 20 & 7 & \frac{62}{95} \\
19 & 19 & 7 & \frac{229}{361} \\
19 & 18 & 7 & \frac{35}{57} \\
19 & 17 & 7 & \frac{191}{323} \\
19 & 16 & 7 & \frac{43}{76} \\
19 & 15 & 7 & \frac{51}{95} \\
19 & 14 & 7 & \frac{67}{133} \\
19 & 13 & 7 & \frac{115}{247} \\
19 & 12 & 6 & \frac{8}{19} \\
19 & 11 & 6 & \frac{84}{209} \\
19 & 10 & 6 & \frac{36}{95} \\
19 & 9 & 6 & \frac{20}{57} \\
19 & 8 & 6 & \frac{6}{19} \\
19 & 7 & 6 & \frac{36}{133} \\
19 & 6 & 3 & \frac{4}{19} \\
19 & 5 & 3 & \frac{18}{95} \\
19 & 4 & 3 & \frac{3}{19} \\
19 & 3 & 1 & \frac{2}{19} \\
19 & 2 & 1 & \frac{3}{38} \\
19 & 1 & 1 & \frac{1}{19} \\
18 & 24 & 6 & \frac{13}{18} \\
18 & 23 & 6 & \frac{49}{69} \\
18 & 22 & 6 & \frac{23}{33} \\
18 & 21 & 6 & \frac{43}{63} \\
18 & 20 & 6 & \frac{2}{3} \\
18 & 19 & 6 & \frac{37}{57} \\
18 & 18 & 6 & \frac{17}{27} \\
18 & 17 & 6 & \frac{31}{51} \\
18 & 16 & 6 & \frac{7}{12} \\
18 & 15 & 6 & \frac{5}{9} \\
18 & 14 & 6 & \frac{11}{21} \\
18 & 13 & 6 & \frac{19}{39} \\
18 & 12 & 6 & \frac{4}{9} \\
18 & 11 & 6 & \frac{14}{33} \\
18 & 10 & 6 & \frac{2}{5} \\
18 & 9 & 6 & \frac{10}{27} \\
18 & 8 & 6 & \frac{1}{3} \\
18 & 7 & 6 & \frac{2}{7} \\
18 & 6 & 3 & \frac{2}{9} \\
18 & 5 & 3 & \frac{1}{5} \\
18 & 4 & 3 & \frac{1}{6} \\
18 & 3 & 1 & \frac{1}{9} \\
18 & 2 & 1 & \frac{1}{12} \\
18 & 1 & 1 & \frac{1}{18} \\
17 & 24 & 6 & \frac{25}{34} \\
17 & 23 & 6 & \frac{283}{391} \\
17 & 22 & 6 & \frac{133}{187} \\
17 & 21 & 6 & \frac{83}{119} \\
17 & 20 & 6 & \frac{58}{85} \\
17 & 19 & 6 & \frac{215}{323} \\
17 & 18 & 6 & \frac{11}{17} \\
17 & 17 & 6 & \frac{181}{289} \\
17 & 16 & 6 & \frac{41}{68} \\
17 & 15 & 6 & \frac{49}{85} \\
17 & 14 & 6 & \frac{65}{119} \\
17 & 13 & 6 & \frac{113}{221} \\
17 & 12 & 6 & \frac{8}{17} \\
17 & 11 & 6 & \frac{84}{187} \\
17 & 10 & 6 & \frac{36}{85} \\
17 & 9 & 6 & \frac{20}{51} \\
17 & 8 & 6 & \frac{6}{17} \\
17 & 7 & 6 & \frac{36}{119} \\
17 & 6 & 3 & \frac{4}{17} \\
17 & 5 & 3 & \frac{18}{85} \\
17 & 4 & 3 & \frac{3}{17} \\
17 & 3 & 1 & \frac{2}{17} \\
17 & 2 & 1 & \frac{3}{34} \\
17 & 1 & 1 & \frac{1}{17} \\
16 & 24 & 6 & \frac{3}{4} \\
16 & 23 & 6 & \frac{17}{23} \\
16 & 22 & 6 & \frac{8}{11} \\
16 & 21 & 6 & \frac{5}{7} \\
16 & 20 & 6 & \frac{7}{10} \\
16 & 19 & 6 & \frac{13}{19} \\
16 & 18 & 6 & \frac{2}{3} \\
16 & 17 & 6 & \frac{11}{17} \\
16 & 16 & 6 & \frac{5}{8} \\
16 & 15 & 6 & \frac{3}{5} \\
16 & 14 & 6 & \frac{4}{7} \\
16 & 13 & 6 & \frac{7}{13} \\
16 & 12 & 6 & \frac{1}{2} \\
16 & 11 & 6 & \frac{21}{44} \\
16 & 10 & 6 & \frac{9}{20} \\
16 & 9 & 6 & \frac{5}{12} \\
16 & 8 & 6 & \frac{3}{8} \\
16 & 7 & 6 & \frac{9}{28} \\
16 & 6 & 3 & \frac{1}{4} \\
16 & 5 & 3 & \frac{9}{40} \\
16 & 4 & 3 & \frac{3}{16} \\
16 & 3 & 1 & \frac{1}{8} \\
16 & 2 & 1 & \frac{3}{32} \\
16 & 1 & 1 & \frac{1}{16} \\
15 & 24 & 6 & \frac{23}{30} \\
15 & 23 & 6 & \frac{87}{115} \\
15 & 22 & 6 & \frac{41}{55} \\
15 & 21 & 6 & \frac{11}{15} \\
15 & 20 & 6 & \frac{18}{25} \\
15 & 19 & 6 & \frac{67}{95} \\
15 & 18 & 6 & \frac{31}{45} \\
15 & 17 & 6 & \frac{57}{85} \\
15 & 16 & 6 & \frac{13}{20} \\
15 & 15 & 6 & \frac{47}{75} \\
15 & 14 & 6 & \frac{3}{5} \\
15 & 13 & 6 & \frac{37}{65} \\
15 & 12 & 6 & \frac{8}{15} \\
15 & 11 & 6 & \frac{28}{55} \\
15 & 10 & 6 & \frac{12}{25} \\
15 & 9 & 6 & \frac{4}{9} \\
15 & 8 & 6 & \frac{2}{5} \\
15 & 7 & 6 & \frac{12}{35} \\
15 & 6 & 3 & \frac{4}{15} \\
15 & 5 & 3 & \frac{6}{25} \\
15 & 4 & 3 & \frac{1}{5} \\
15 & 3 & 1 & \frac{2}{15} \\
15 & 2 & 1 & \frac{1}{10} \\
15 & 1 & 1 & \frac{1}{15} \\
14 & 24 & 6 & \frac{11}{14} \\
14 & 23 & 6 & \frac{125}{161} \\
14 & 22 & 6 & \frac{59}{77} \\
14 & 21 & 6 & \frac{37}{49} \\
14 & 20 & 6 & \frac{26}{35} \\
14 & 19 & 6 & \frac{97}{133} \\
14 & 18 & 6 & \frac{5}{7} \\
14 & 17 & 6 & \frac{83}{119} \\
14 & 16 & 6 & \frac{19}{28} \\
14 & 15 & 6 & \frac{23}{35} \\
14 & 14 & 6 & \frac{31}{49} \\
14 & 13 & 6 & \frac{55}{91} \\
14 & 12 & 6 & \frac{4}{7} \\
14 & 11 & 6 & \frac{6}{11} \\
14 & 10 & 6 & \frac{18}{35} \\
14 & 9 & 6 & \frac{10}{21} \\
14 & 8 & 6 & \frac{3}{7} \\
14 & 7 & 6 & \frac{18}{49} \\
14 & 6 & 3 & \frac{2}{7} \\
14 & 5 & 3 & \frac{9}{35} \\
14 & 4 & 3 & \frac{3}{14} \\
14 & 3 & 1 & \frac{1}{7} \\
14 & 2 & 1 & \frac{3}{28} \\
14 & 1 & 1 & \frac{1}{14} \\
13 & 24 & 6 & \frac{21}{26} \\
13 & 23 & 6 & \frac{239}{299} \\
13 & 22 & 6 & \frac{113}{143} \\
13 & 21 & 6 & \frac{71}{91} \\
13 & 20 & 6 & \frac{10}{13} \\
13 & 19 & 6 & \frac{187}{247} \\
13 & 18 & 6 & \frac{29}{39} \\
13 & 17 & 6 & \frac{161}{221} \\
13 & 16 & 6 & \frac{37}{52} \\
13 & 15 & 6 & \frac{9}{13} \\
13 & 14 & 6 & \frac{61}{91} \\
13 & 13 & 6 & \frac{109}{169} \\
13 & 12 & 6 & \frac{8}{13} \\
13 & 11 & 6 & \frac{84}{143} \\
13 & 10 & 6 & \frac{36}{65} \\
13 & 9 & 6 & \frac{20}{39} \\
13 & 8 & 6 & \frac{6}{13} \\
13 & 7 & 6 & \frac{36}{91} \\
13 & 6 & 3 & \frac{4}{13} \\
13 & 5 & 3 & \frac{18}{65} \\
13 & 4 & 3 & \frac{3}{13} \\
13 & 3 & 1 & \frac{2}{13} \\
13 & 2 & 1 & \frac{3}{26} \\
13 & 1 & 1 & \frac{1}{13} \\
12 & 24 & 6 & \frac{5}{6} \\
12 & 23 & 6 & \frac{19}{23} \\
12 & 22 & 6 & \frac{9}{11} \\
12 & 21 & 6 & \frac{17}{21} \\
12 & 20 & 6 & \frac{4}{5} \\
12 & 19 & 6 & \frac{15}{19} \\
12 & 18 & 6 & \frac{7}{9} \\
12 & 17 & 6 & \frac{13}{17} \\
12 & 16 & 6 & \frac{3}{4} \\
12 & 15 & 6 & \frac{11}{15} \\
12 & 14 & 6 & \frac{5}{7} \\
12 & 13 & 6 & \frac{9}{13} \\
12 & 12 & 6 & \frac{2}{3} \\
12 & 11 & 6 & \frac{7}{11} \\
12 & 10 & 6 & \frac{3}{5} \\
12 & 9 & 6 & \frac{5}{9} \\
12 & 8 & 6 & \frac{1}{2} \\
12 & 7 & 6 & \frac{3}{7} \\
12 & 6 & 3 & \frac{1}{3} \\
12 & 5 & 3 & \frac{3}{10} \\
12 & 4 & 3 & \frac{1}{4} \\
12 & 3 & 1 & \frac{1}{6} \\
12 & 2 & 1 & \frac{1}{8} \\
12 & 1 & 1 & \frac{1}{12} \\
11 & 24 & 5 & \frac{37}{44} \\
11 & 23 & 5 & \frac{211}{253} \\
11 & 22 & 5 & \frac{100}{121} \\
11 & 21 & 5 & \frac{9}{11} \\
11 & 20 & 5 & \frac{89}{110} \\
11 & 19 & 5 & \frac{167}{209} \\
11 & 18 & 5 & \frac{26}{33} \\
11 & 17 & 5 & \frac{145}{187} \\
11 & 16 & 5 & \frac{67}{88} \\
11 & 15 & 5 & \frac{41}{55} \\
11 & 14 & 5 & \frac{8}{11} \\
11 & 13 & 5 & \frac{101}{143} \\
11 & 12 & 5 & \frac{15}{22} \\
11 & 11 & 5 & \frac{79}{121} \\
11 & 10 & 5 & \frac{34}{55} \\
11 & 9 & 5 & \frac{19}{33} \\
11 & 8 & 5 & \frac{23}{44} \\
11 & 7 & 5 & \frac{5}{11} \\
11 & 6 & 3 & \frac{4}{11} \\
11 & 5 & 3 & \frac{18}{55} \\
11 & 4 & 3 & \frac{3}{11} \\
11 & 3 & 1 & \frac{2}{11} \\
11 & 2 & 1 & \frac{3}{22} \\
11 & 1 & 1 & \frac{1}{11} \\
10 & 24 & 4 & \frac{17}{20} \\
10 & 23 & 4 & \frac{97}{115} \\
10 & 22 & 4 & \frac{46}{55} \\
10 & 21 & 4 & \frac{29}{35} \\
10 & 20 & 4 & \frac{41}{50} \\
10 & 19 & 4 & \frac{77}{95} \\
10 & 18 & 4 & \frac{4}{5} \\
10 & 17 & 4 & \frac{67}{85} \\
10 & 16 & 4 & \frac{31}{40} \\
10 & 15 & 4 & \frac{19}{25} \\
10 & 14 & 4 & \frac{26}{35} \\
10 & 13 & 4 & \frac{47}{65} \\
10 & 12 & 4 & \frac{7}{10} \\
10 & 11 & 4 & \frac{37}{55} \\
10 & 10 & 4 & \frac{16}{25} \\
10 & 9 & 4 & \frac{3}{5} \\
10 & 8 & 4 & \frac{11}{20} \\
10 & 7 & 4 & \frac{17}{35} \\
10 & 6 & 3 & \frac{2}{5} \\
10 & 5 & 3 & \frac{9}{25} \\
10 & 4 & 3 & \frac{3}{10} \\
10 & 3 & 1 & \frac{1}{5} \\
10 & 2 & 1 & \frac{3}{20} \\
10 & 1 & 1 & \frac{1}{10} \\
9 & 24 & 3 & \frac{31}{36} \\
9 & 23 & 3 & \frac{59}{69} \\
9 & 22 & 3 & \frac{28}{33} \\
9 & 21 & 3 & \frac{53}{63} \\
9 & 20 & 3 & \frac{5}{6} \\
9 & 19 & 3 & \frac{47}{57} \\
9 & 18 & 3 & \frac{22}{27} \\
9 & 17 & 3 & \frac{41}{51} \\
9 & 16 & 3 & \frac{19}{24} \\
9 & 15 & 3 & \frac{7}{9} \\
9 & 14 & 3 & \frac{16}{21} \\
9 & 13 & 3 & \frac{29}{39} \\
9 & 12 & 3 & \frac{13}{18} \\
9 & 11 & 3 & \frac{23}{33} \\
9 & 10 & 3 & \frac{2}{3} \\
9 & 9 & 3 & \frac{17}{27} \\
9 & 8 & 3 & \frac{7}{12} \\
9 & 7 & 3 & \frac{11}{21} \\
9 & 6 & 3 & \frac{4}{9} \\
9 & 5 & 3 & \frac{2}{5} \\
9 & 4 & 3 & \frac{1}{3} \\
9 & 3 & 1 & \frac{2}{9} \\
9 & 2 & 1 & \frac{1}{6} \\
9 & 1 & 1 & \frac{1}{9} \\
8 & 24 & 3 & \frac{7}{8} \\
8 & 23 & 3 & \frac{20}{23} \\
8 & 22 & 3 & \frac{19}{22} \\
8 & 21 & 3 & \frac{6}{7} \\
8 & 20 & 3 & \frac{17}{20} \\
8 & 19 & 3 & \frac{16}{19} \\
8 & 18 & 3 & \frac{5}{6} \\
8 & 17 & 3 & \frac{14}{17} \\
8 & 16 & 3 & \frac{13}{16} \\
8 & 15 & 3 & \frac{4}{5} \\
8 & 14 & 3 & \frac{11}{14} \\
8 & 13 & 3 & \frac{10}{13} \\
8 & 12 & 3 & \frac{3}{4} \\
8 & 11 & 3 & \frac{8}{11} \\
8 & 10 & 3 & \frac{7}{10} \\
8 & 9 & 3 & \frac{2}{3} \\
8 & 8 & 3 & \frac{5}{8} \\
8 & 7 & 3 & \frac{4}{7} \\
8 & 6 & 3 & \frac{1}{2} \\
8 & 5 & 3 & \frac{9}{20} \\
8 & 4 & 3 & \frac{3}{8} \\
8 & 3 & 1 & \frac{1}{4} \\
8 & 2 & 1 & \frac{3}{16} \\
8 & 1 & 1 & \frac{1}{8} \\
7 & 24 & 3 & \frac{25}{28} \\
7 & 23 & 3 & \frac{143}{161} \\
7 & 22 & 3 & \frac{68}{77} \\
7 & 21 & 3 & \frac{43}{49} \\
7 & 20 & 3 & \frac{61}{70} \\
7 & 19 & 3 & \frac{115}{133} \\
7 & 18 & 3 & \frac{6}{7} \\
7 & 17 & 3 & \frac{101}{119} \\
7 & 16 & 3 & \frac{47}{56} \\
7 & 15 & 3 & \frac{29}{35} \\
7 & 14 & 3 & \frac{40}{49} \\
7 & 13 & 3 & \frac{73}{91} \\
7 & 12 & 3 & \frac{11}{14} \\
7 & 11 & 3 & \frac{59}{77} \\
7 & 10 & 3 & \frac{26}{35} \\
7 & 9 & 3 & \frac{5}{7} \\
7 & 8 & 3 & \frac{19}{28} \\
7 & 7 & 3 & \frac{31}{49} \\
7 & 6 & 3 & \frac{4}{7} \\
7 & 5 & 3 & \frac{18}{35} \\
7 & 4 & 3 & \frac{3}{7} \\
7 & 3 & 1 & \frac{2}{7} \\
7 & 2 & 1 & \frac{3}{14} \\
7 & 1 & 1 & \frac{1}{7} \\
6 & 24 & 3 & \frac{11}{12} \\
6 & 23 & 3 & \frac{21}{23} \\
6 & 22 & 3 & \frac{10}{11} \\
6 & 21 & 3 & \frac{19}{21} \\
6 & 20 & 3 & \frac{9}{10} \\
6 & 19 & 3 & \frac{17}{19} \\
6 & 18 & 3 & \frac{8}{9} \\
6 & 17 & 3 & \frac{15}{17} \\
6 & 16 & 3 & \frac{7}{8} \\
6 & 15 & 3 & \frac{13}{15} \\
6 & 14 & 3 & \frac{6}{7} \\
6 & 13 & 3 & \frac{11}{13} \\
6 & 12 & 3 & \frac{5}{6} \\
6 & 11 & 3 & \frac{9}{11} \\
6 & 10 & 3 & \frac{4}{5} \\
6 & 9 & 3 & \frac{7}{9} \\
6 & 8 & 3 & \frac{3}{4} \\
6 & 7 & 3 & \frac{5}{7} \\
6 & 6 & 3 & \frac{2}{3} \\
6 & 5 & 3 & \frac{3}{5} \\
6 & 4 & 3 & \frac{1}{2} \\
6 & 3 & 1 & \frac{1}{3} \\
6 & 2 & 1 & \frac{1}{4} \\
6 & 1 & 1 & \frac{1}{6} \\
5 & 24 & 2 & \frac{37}{40} \\
5 & 23 & 2 & \frac{106}{115} \\
5 & 22 & 2 & \frac{101}{110} \\
5 & 21 & 2 & \frac{32}{35} \\
5 & 20 & 2 & \frac{91}{100} \\
5 & 19 & 2 & \frac{86}{95} \\
5 & 18 & 2 & \frac{9}{10} \\
5 & 17 & 2 & \frac{76}{85} \\
5 & 16 & 2 & \frac{71}{80} \\
5 & 15 & 2 & \frac{22}{25} \\
5 & 14 & 2 & \frac{61}{70} \\
5 & 13 & 2 & \frac{56}{65} \\
5 & 12 & 2 & \frac{17}{20} \\
5 & 11 & 2 & \frac{46}{55} \\
5 & 10 & 2 & \frac{41}{50} \\
5 & 9 & 2 & \frac{4}{5} \\
5 & 8 & 2 & \frac{31}{40} \\
5 & 7 & 2 & \frac{26}{35} \\
5 & 6 & 2 & \frac{7}{10} \\
5 & 5 & 2 & \frac{16}{25} \\
5 & 4 & 2 & \frac{11}{20} \\
5 & 3 & 1 & \frac{2}{5} \\
5 & 2 & 1 & \frac{3}{10} \\
5 & 1 & 1 & \frac{1}{5} \\
4 & 24 & 1 & \frac{15}{16} \\
4 & 23 & 1 & \frac{43}{46} \\
4 & 22 & 1 & \frac{41}{44} \\
4 & 21 & 1 & \frac{13}{14} \\
4 & 20 & 1 & \frac{37}{40} \\
4 & 19 & 1 & \frac{35}{38} \\
4 & 18 & 1 & \frac{11}{12} \\
4 & 17 & 1 & \frac{31}{34} \\
4 & 16 & 1 & \frac{29}{32} \\
4 & 15 & 1 & \frac{9}{10} \\
4 & 14 & 1 & \frac{25}{28} \\
4 & 13 & 1 & \frac{23}{26} \\
4 & 12 & 1 & \frac{7}{8} \\
4 & 11 & 1 & \frac{19}{22} \\
4 & 10 & 1 & \frac{17}{20} \\
4 & 9 & 1 & \frac{5}{6} \\
4 & 8 & 1 & \frac{13}{16} \\
4 & 7 & 1 & \frac{11}{14} \\
4 & 6 & 1 & \frac{3}{4} \\
4 & 5 & 1 & \frac{7}{10} \\
4 & 4 & 1 & \frac{5}{8} \\
4 & 3 & 1 & \frac{1}{2} \\
4 & 2 & 1 & \frac{3}{8} \\
4 & 1 & 1 & \frac{1}{4} \\
3 & 24 & 1 & \frac{23}{24} \\
3 & 23 & 1 & \frac{22}{23} \\
3 & 22 & 1 & \frac{21}{22} \\
3 & 21 & 1 & \frac{20}{21} \\
3 & 20 & 1 & \frac{19}{20} \\
3 & 19 & 1 & \frac{18}{19} \\
3 & 18 & 1 & \frac{17}{18} \\
3 & 17 & 1 & \frac{16}{17} \\
3 & 16 & 1 & \frac{15}{16} \\
3 & 15 & 1 & \frac{14}{15} \\
3 & 14 & 1 & \frac{13}{14} \\
3 & 13 & 1 & \frac{12}{13} \\
3 & 12 & 1 & \frac{11}{12} \\
3 & 11 & 1 & \frac{10}{11} \\
3 & 10 & 1 & \frac{9}{10} \\
3 & 9 & 1 & \frac{8}{9} \\
3 & 8 & 1 & \frac{7}{8} \\
3 & 7 & 1 & \frac{6}{7} \\
3 & 6 & 1 & \frac{5}{6} \\
3 & 5 & 1 & \frac{4}{5} \\
3 & 4 & 1 & \frac{3}{4} \\
3 & 3 & 1 & \frac{2}{3} \\
3 & 2 & 1 & \frac{1}{2} \\
3 & 1 & 1 & \frac{1}{3} \\
2 & 24 & 1 & \frac{47}{48} \\
2 & 23 & 1 & \frac{45}{46} \\
2 & 22 & 1 & \frac{43}{44} \\
2 & 21 & 1 & \frac{41}{42} \\
2 & 20 & 1 & \frac{39}{40} \\
2 & 19 & 1 & \frac{37}{38} \\
2 & 18 & 1 & \frac{35}{36} \\
2 & 17 & 1 & \frac{33}{34} \\
2 & 16 & 1 & \frac{31}{32} \\
2 & 15 & 1 & \frac{29}{30} \\
2 & 14 & 1 & \frac{27}{28} \\
2 & 13 & 1 & \frac{25}{26} \\
2 & 12 & 1 & \frac{23}{24} \\
2 & 11 & 1 & \frac{21}{22} \\
2 & 10 & 1 & \frac{19}{20} \\
2 & 9 & 1 & \frac{17}{18} \\
2 & 8 & 1 & \frac{15}{16} \\
2 & 7 & 1 & \frac{13}{14} \\
2 & 6 & 1 & \frac{11}{12} \\
2 & 5 & 1 & \frac{9}{10} \\
2 & 4 & 1 & \frac{7}{8} \\
2 & 3 & 1 & \frac{5}{6} \\
2 & 2 & 1 & \frac{3}{4} \\
2 & 1 & 1 & \frac{1}{2} \\
1 & 24 & 1 & 1 \\
1 & 23 & 1 & 1 \\
1 & 22 & 1 & 1 \\
1 & 21 & 1 & 1 \\
1 & 20 & 1 & 1 \\
1 & 19 & 1 & 1 \\
1 & 18 & 1 & 1 \\
1 & 17 & 1 & 1 \\
1 & 16 & 1 & 1 \\
1 & 15 & 1 & 1 \\
1 & 14 & 1 & 1 \\
1 & 13 & 1 & 1 \\
1 & 12 & 1 & 1 \\
1 & 11 & 1 & 1 \\
1 & 10 & 1 & 1 \\
1 & 9 & 1 & 1 \\
1 & 8 & 1 & 1 \\
1 & 7 & 1 & 1 \\
1 & 6 & 1 & 1 \\
1 & 5 & 1 & 1 \\
1 & 4 & 1 & 1 \\
1 & 3 & 1 & 1 \\
1 & 2 & 1 & 1 \\
1 & 1 & 1 & 1 \\
\end{array}
$$</div>