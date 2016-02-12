---
title: Sequence Alignment
author: joefkelley
layout: post
date: 2013-01-11
url: /131
sfw_pwd:
  - JKRG5Tii2TIi
categories:
  - Programming

---
Sequence alignment is the task of aligning two sequences of characters, with possible gaps inserted, in order to maximize the matching characters. Its most notable application is in biology, where it is commonly used on sequences of nucleotides (DNA) or proteins.

Usually, to "score" an alignment, we choose three values: a match score, a mismatch penalty, and a gap penalty. For example, it's common to choose match=2, mismatch=-1, gap=-1. With those choices, the following alignment would get a score of 5 (4 matches, 2 gaps, 1 mismatch):
  
`Sequence alignment is the task of aligning two sequences of characters, with possible gaps inserted, in order to maximize the matching characters. Its most notable application is in biology, where it is commonly used on sequences of nucleotides (DNA) or proteins.

Usually, to "score" an alignment, we choose three values: a match score, a mismatch penalty, and a gap penalty. For example, it's common to choose match=2, mismatch=-1, gap=-1. With those choices, the following alignment would get a score of 5 (4 matches, 2 gaps, 1 mismatch):

<pre>
ATTC-GA
-TTCACA
</pre>

The task is: given the input sequences, find the alignment that maximizes the overall score. I'll walk through a solution I wrote in Java. First, let's get through some simple skeleton code.

Here's an interface for scoring (to be implemented later):
  


~~~Java
package alignment;

public interface Scorer {
	public int getMatchScore(char c1, char c2);
	public int getGapPenalty();
}
~~~

And a simple data class for storing results:

~~~Java
package alignment;

public class Alignment {
	private final String str1;
	private final String str2;
	private final int score;

	public Alignment(String str1, String str2, int score) {
		this.str1 = str1;
		this.str2 = str2;
		this.score = score;
	}

	public String getStr1() {
		return str1;
	}

	public String getStr2() {
		return str2;
	}

	public int getScore() {
		return score;
	}

	public String toString() {
		return str1 + "\n" + str2 + "\n" + score;
	}
}
~~~

And the skeleton of the class that will do the actual alignment:

~~~Java
package alignment;

public class Aligner {
	private final Scorer scorer;
	public Aligner(Scorer scorer) {
		this.scorer = scorer;
	}

	public Alignment getAlignment(String str1, String str2) {
		// To be implemented
		return null;
	}
}
~~~

The simplest strategy is to just try every possible alignment recursively. We will look at the first part of each string, and try either a) matching the first two characters, b) putting a gap in the first, or c) putting a gap in the second. Then we recursively do the rest of the strings, and choose the best. In code:

~~~Java
public Alignment getAlignment(String str1, String str2) {
	if (str1.length() == 0) { // base case 1: str1 is empty
		int len = str2.length();
		return new Alignment(dashes(len),
							 str2,
							 scorer.getGapPenalty() * len);
	} else if (str2.length() == 0) { // base case 2: str2 is empty
		int len = str1.length();
		return new Alignment(str1,
							 dashes(len),
							 scorer.getGapPenalty() * len);
	}
	char c1 = str1.charAt(0);
	char c2 = str2.charAt(0);

	// match c1 to c2
	Alignment match = getAlignment(str1.substring(1), str2.substring(1));
	int matchScore = match.getScore() + scorer.getMatchScore(c1, c2);

	// match c2 to a gap
	Alignment gap1 = getAlignment(str1, str2.substring(1));
	int gap1Score = gap1.getScore() + scorer.getGapPenalty();

	// match c1 to a gap
	Alignment gap2 = getAlignment(str1.substring(1), str2);
	int gap2Score = gap2.getScore() + scorer.getGapPenalty();

	// return the best one
	if (matchScore >= gap1Score && matchScore >= gap2Score) {
		return new Alignment(c1 + match.getStr1(),
							 c2 + match.getStr2(),
							 matchScore);
	} else if (gap1Score >= gap2Score) {
		return new Alignment("-" + gap1.getStr1(),
							 c2 + gap1.getStr2(),
							 gap1Score);
	} else {
		return new Alignment(c1 + gap2.getStr1(),
							 "-" + gap2.getStr2(),
							 gap2Score);
	}
}

private static String dashes(int len) {
	String result = "";
	for (int i = 0; i &lt; len; i++) {
		result += "-";
	}
	return result;
}
~~~

Not too bad. However, we can do a lot better. This actually ends up re-computing a lot of values. For instance, after adding a gap in each string, it will execute the same sub-problem as if it had just matched characters from each. This solution takes exponentially longer as the inputs grow, which is pretty awful.

To explain the better way of doing this, let's go back to the earlier example of aligning ATTCGA to TTCACA.

Let's start at the end of the strings, instead of the start. If we were to align the trailing A at the end of each string, then the final result would simply be the alignment of ATTCG to TTCAC, followed by two As. If we were to align the first A with a gap, the result would be the alignment of ATTCG to TTCACA, and vice versa for aligning the second A with a gap.

In general, we can think about aligning the first N characters of the first string with the first M characters of the second, call this `\(A(N,M)\)`. In order to do this, we need to know three things: `\(A(N-1,M-1)\)`, `\(A(N-1,M)\)`, and `\(A(N,M-1)\)`. More formally, we can fully define the function as:

<div>$$
A(M,N)=max\left\{
\begin{array}{l}
A(M-1,N-1) + match\_score(str1[M],str2[N]) \\
A(M,N-1) + gap\_penalty \\
A(M-1,N) + gap\_penalty
\end{array}
\right.
$$</div>

The key insight to getting acceptable performance is that we can do all of this in a table, storing all intermediate results. We start with an empty table, and the strings written along the edges:
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& - & - & - & - & - & - & - \\
T & - & - & - & - & - & - & - \\
T & - & - & - & - & - & - & - \\
C & - & - & - & - & - & - & - \\
A & - & - & - & - & - & - & - \\
C & - & - & - & - & - & - & - \\
A & - & - & - & - & - & - & -
\end{array}
$$</div>
  
The value in each cell of the table will be equal to the score of the alignment of each string up to that point, or `\(A(row,column)\)`. We can start by putting a 0 in the top corner, and filling in the edges with gap penalties:
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& 0 & -1 & -2 & -3 & -4 & -5 & -6 \\
T & -1 & - & - & - & - & - & - \\
T & -2 & - & - & - & - & - & - \\
C & -3 & - & - & - & - & - & - \\
A & -4 & - & - & - & - & - & - \\
C & -5 & - & - & - & - & - & - \\
A & -6 & - & - & - & - & - & -
\end{array}
$$</div>
  
Next, we start filling in the rest of the table, starting in the top-left. For each entry, we look at three entries: the one above, the one to the left, and the one diagonally up and left. The first two correspond to gaps, so we add the gap penalty, and the third corresponds to a match, so we add either the match or mismatch score, respectively. So for that, first top-left entry, our options are `\(-1 + -1 = -2\)`, `\(-1 + -1 = -2\)`, or `\(0 + -1 = -1\)`, so we choose the match option, for a score of `\(-1\)`
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& 0 & -1 & -2 & -3 & -4 & -5 & -6 \\
T & -1 & -1 & - & - & - & - & - \\
T & -2 & - & - & - & - & - & - \\
C & -3 & - & - & - & - & - & - \\
A & -4 & - & - & - & - & - & - \\
C & -5 & - & - & - & - & - & - \\
A & -6 & - & - & - & - & - & -
\end{array}
$$</div>
  
Continue to the entry to the right: looking above and adding a gap gets a score of `\(-2 + -1 = -3\)`, looking to the left and adding a gap gets a score of `\(-1 + -1 = -2\)`, and looking diagonally and adding a match gets a score of `\(-1 + 2 = 1\)`. So the match is best (since the characters are both T), so we add that value:
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& 0 & -1 & -2 & -3 & -4 & -5 & -6 \\
T & -1 & -1 & 1 & - & - & - & - \\
T & -2 & - & - & - & - & - & - \\
C & -3 & - & - & - & - & - & - \\
A & -4 & - & - & - & - & - & - \\
C & -5 & - & - & - & - & - & - \\
A & -6 & - & - & - & - & - & -
\end{array}
$$</div>
  
Continuing along the top row...
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& 0 & -1 & -2 & -3 & -4 & -5 & -6 \\
T & -1 & -1 & 1 & 0 & -1 & -2 & -3 \\
T & -2 & - & - & - & - & - & - \\
C & -3 & - & - & - & - & - & - \\
A & -4 & - & - & - & - & - & - \\
C & -5 & - & - & - & - & - & - \\
A & -6 & - & - & - & - & - & -
\end{array}
$$</div>
  
And continuing on to fill out the entire table, row by row:
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& 0 & -1 & -2 & -3 & -4 & -5 & -6 \\
T & -1 & -1 & 1 & 0 & -1 & -2 & -3 \\
T & -2 & -2 & 1 & 3 & 2 & 1 & 0 \\
C & -3 & -3 & 0 & 2 & 5 & 4 & 3 \\
A & -4 & -1 & -1 & 1 & 4 & 3 & 6 \\
C & -5 & -2 & -2 & 0 & 3 & 3 & 5 \\
A & -6 & -3 & -3 & -1 & 2 & 2 & 5
\end{array}
$$</div>
  
Notice, in the bottom-right corner, we have recovered our maximal alignment score of 5. In order to recover the actual alignment, we need to do some additional bookkeeping. We will maintain "backpointers" that are just markers to which choice provided the maximal score at each entry in our table. For our example above, this would look like:
  
<div>$$
\begin{array}{r|rrrrrrr}
& & A & T & T & C & G & A \\ \hline
& \bullet & \leftarrow & \leftarrow & \leftarrow & \leftarrow & \leftarrow & \leftarrow \\
T & \uparrow & \nwarrow & \nwarrow & \leftarrow & \leftarrow & \leftarrow & \leftarrow \\
T & \uparrow & \uparrow & \nwarrow & \nwarrow & \leftarrow & \leftarrow & \leftarrow \\
C & \uparrow & \uparrow & \uparrow & \uparrow & \nwarrow & \leftarrow & \leftarrow \\
A & \uparrow & \nwarrow & \uparrow & \uparrow & \uparrow & \uparrow & \nwarrow \\
C & \uparrow & \uparrow & \uparrow & \uparrow & \nwarrow & \nwarrow & \uparrow \\
A & \uparrow & \uparrow & \uparrow & \uparrow & \uparrow & \uparrow & \nwarrow
\end{array}
$$</div>
  
We can then recover the alignment that resulted in the maximal score by starting in the bottom-right corner and walking back until the top-left is reached, building up the strings and inserting gaps as we go.

So let's walk through the code for this:

First of all, an enum for backpointers:
  
~~~Java
private enum Marker {
	MATCH, GAP1, GAP2, START
}
~~~

And now to the actual getAlignment method, which will have the same method signature as before. Step 1 is to create these tables, and initialize the starting values:
  

~~~Java
int[][] scores = new int[str1.length() + 1][str2.length() + 1];
Marker[][] markers = new Marker[str1.length() + 1][str2.length() + 1];
scores[0][0] = 0;
markers[0][0] = Marker.START;
for (int r = 1; r < scores.length; r++) {
	scores[r][0] = r * scorer.getGapPenalty();
	markers[r][0] = Marker.GAP2;
}
for (int c = 1; c < scores[0].length; c++) {
	scores[0][[c]] = c * scorer.getGapPenalty();
	markers[0][[c]] = Marker.GAP1;
}
~~~
  
Next, we go through the bulk of the algorithm, filling in those tables based on our recursive formula:

~~~Java
for (int r = 1; r < scores.length; r++) {
	for (int c = 1; c < scores[r].length; c++) {
		int match = scorer.getMatchScore(str1.charAt(r - 1), str2.charAt(c - 1)) + scores[r-1][[c-1]];
		int gap1 = scorer.getGapPenalty() + scores[r][[c - 1]];
		int gap2 = scorer.getGapPenalty() + scores[r - 1][[c]];
		if (match >= gap1 && match >= gap2) {
			scores[r][[c]] = match;
			markers[r][[c]] = Marker.MATCH;
		} else if (gap1 >= gap2) {
			scores[r][[c]] = gap1;
			markers[r][[c]] = Marker.GAP1;
		} else {
			scores[r][[c]] = gap2;
			markers[r][[c]] = Marker.GAP2;
		}
	}
}
~~~

A simple double for-loop, and then lines 3, 4, and 5 compute the three options. The rest of the loop simply picks the best one and fills in the table entries appropriately.

Finally, we walk back through the markers table to recover the alignment strings and return the results:

~~~Java
int r = str1.length();
int c = str2.length();
int finalScore = scores[r][[c]];
String result1 = "";
String result2 = "";
while (markers[r][[c]] != Marker.START) {
	if (markers[r][[c]] == Marker.GAP1) {
		result1 = "-" + result1;
		result2 = str2.charAt(--c) + result2;
	} else if (markers[r][[c]] == Marker.GAP2) {
		result1 = str1.charAt(--r) + result1;
		result2 = "-" + result2;
	} else {
		result1 = str1.charAt(--r) + result1;
		result2 = str2.charAt(--c) + result2;
	}
}

return new Alignment(result1, result2, finalScore);
~~~


  
And that's it!

... but let's kick it up a notch. It turns out that in the world of DNA sequence alignment, this isn't as practical as we could make it. The way DNA is sequenced is that many short segments are read, and then they are all aligned and aggregated into one final sequence. However, the reading process is error-prone, so the same segment will be read many times, and if it is covered enough times, then we hope to be able to recognize and correct those errors, because they won't agree with other reads. Gaps in the reads are interesting case, because it turns out that not all gaps are equally likely to occur. Gaps are relatively rare, but when they do occur, it is likely that more than just one nucleotide was skipped. So two separate gaps are less likely than one gap of length two, for example.

To fix this, we typically use what's called an "affine gap score", which basically means that the gap penalty is parameterized by two values: a "gap open" penalty, and a "gap continue" penalty. For example, we might set these to -2 and -1, respectively. Then, a gap of length 1 would be a score of -2 (just the gap open), a gap of length 2 would be a score of -3 (gap open + one gap continue), length 3 would be -4, etc.

To do this, things get somewhat uglier. We now have to maintain three separate tables. Table A will contain overall best scores up to M,N, table B will contain best scores up to M,N _with an open gap in the first string_, and table B will contain best scores up to M,N with an open gap in the second string. Our new recurrence relationships are:
  
<div>$$
\begin{align}
A(M,N)&=max\left\{
\begin{array}{l}
A(M-1,N-1) + match\_score(str1[M],str2[N]) \\
B(M,N) \\
C(M,N)
\end{array}
\right. \\
B(M,N)&=max\left\{
\begin{array}{l}
B(M,N-1) + gap\_continue \\
A(M,N-1) + gap\_open
\end{array}
\right. \\
C(M,N)&=max\left\{
\begin{array}{l}
C(M-1,N) + gap\_continue \\
A(M-1,N) + gap\_open
\end{array}
\right.
\end{align}
$$</div>

And for the implementation:

~~~Java
package alignment;

public interface AffineScorer extends Scorer {
	public int getGapOpenPenalty();
}
~~~

~~~Java
public Alignment getAffineAlignment(String str1, String str2) {
	if (scorer instanceof AffineScorer) {
		AffineScorer aScorer = (AffineScorer)scorer;
		int[][] as = new int[str1.length() + 1][str2.length() + 1];
		int[][] bs = new int[str1.length() + 1][str2.length() + 1];
		int[][] cs = new int[str1.length() + 1][str2.length() + 1];
		Marker[][] markers = new Marker[str1.length() + 1][str2.length() + 1];
		as[0][0] = bs[0][0] = cs[0][0] = 0;
		markers[0][0] = Marker.START;
		for (int r = 1; r < as.length; r++) {
			as[r][0] = bs[r][0] = cs[r][0] = aScorer.getGapOpenPenalty() + (r - 1) * scorer.getGapPenalty();
			bs[r][0] += (aScorer.getGapOpenPenalty() - scorer.getGapPenalty());
			markers[r][0] = Marker.GAP2;
		}
		for (int c = 1; c < as[0].length; c++) {
			as[0][[c]] = bs[0][[c]] = cs[0][[c]] = aScorer.getGapOpenPenalty() + (c - 1) * scorer.getGapPenalty();
			cs[0][[c]] += (aScorer.getGapOpenPenalty() - scorer.getGapPenalty());
			markers[0][[c]] = Marker.GAP1;
		}

		for (int r = 1; r < as.length; r++) {
			for (int c = 1; c < as[r].length; c++) {
				int gapOpen1 = as[r][[c-1]] + aScorer.getGapOpenPenalty();
				int gapContinue1 = bs[r][[c-1]] + scorer.getGapPenalty();
				bs[r][[c]] = Math.max(gapOpen1, gapContinue1);
				int gapOpen2 = as[r-1][[c]] + aScorer.getGapOpenPenalty();
				int gapContinue2 = cs[r-1][[c]] + scorer.getGapPenalty();
				cs[r][[c]] = Math.max(gapOpen2, gapContinue2);

				int fromA = as[r-1][[c-1]] + scorer.getMatchScore(str1.charAt(r-1), str2.charAt(c-1));
				int fromB = bs[r][[c]];
				int fromC = cs[r][[c]];

				if (fromA >= fromB && fromA >= fromC) {
					as[r][[c]] = fromA;
					markers[r][[c]] = Marker.MATCH;
				} else if (fromB >= fromC) {
					as[r][[c]] = fromB;
					markers[r][[c]] = Marker.GAP1;
				} else {
					as[r][[c]] = fromC;
					markers[r][[c]] = Marker.GAP2;
				}
			}
		}

		int r = str1.length();
		int c = str2.length();
		int finalScore = as[r][[c]];
		String result1 = "";
		String result2 = "";
		while (markers[r][[c]] != Marker.START) {
			if (markers[r][[c]] == Marker.GAP1) {
				result1 = "-" + result1;
				result2 = str2.charAt(--c) + result2;
			} else if (markers[r][[c]] == Marker.GAP2) {
				result1 = str1.charAt(--r) + result1;
				result2 = "-" + result2;
			} else {
				result1 = str1.charAt(--r) + result1;
				result2 = str2.charAt(--c) + result2;
			}
		}

		return new Alignment(result1, result2, finalScore);
	}
	return null;
}
~~~

And there you go - sequence aligment with affine gap scores.

... but let's kick it up another notch or two.

Back to the science: when these many short DNA segments are being read, they are read pretty much at random. There is no precise control over what portion of the DNA they come from, or over how long each subsequence is. So you get a mix of random length strings from random positions, some of which overlap. Because of this, penalizing gaps at the beginning or end of a match doesn't make sense. That would probably just mean that the segment came from a different position in the DNA. For example, if the two strings are:

<pre>
AATCGGAGTTCAT
AGTTCATTAC
</pre>


Then the proper alignment is probably:

<pre>
AATCGGAGTTCAT---
------AGTTCATTAC
</pre>


However, our model punishes that inappropriately because of all the gaps. In fact, it's a very good alignment, it just happens to be of two strings from different portions of the original.

We can modify our algorithm pretty simply to achieve this. First of all, the top row and left column in our tables should just be filled with 0's so that there is no penalty for starting with a gap. Also, we will allow our "finishing" entry to be any entry in the bottom row or right column, not just the single entry in the bottom-right. The best alignment comes from whichever of those entries has the maximal score.

One last addition: there are some situations in which we don't care about aligning the entire strings; we only want the best possible alignment of any substrings of those strings. For example, if we have two different DNA sequences that we know are responsible for similar biological functions, and we want to find the subsequences of nucleotides in each sequence that encode the overlap in their functions, then we could use this technique.

To accomplish this, we allow our algorithm to "reset" at any time by setting an entry in the table to 0. If a score would every go below 0, we instead set it to 0 and this becomes the new "start" of the alignment. At the end, we scan the entire table for the maximal score, and the alignment "ends" at that entry.

That's usually about as complicated as it needs to be for most applications, but there are a few more complex extensions that are possible. For example, more sophisticated gap penalties can be used. If the penalty as a function of the size of the gap is composed of finitely many linear sections (the earlier gap-open/gap-extend model is an example of this with two sections), then we can come up with an algorithm using 2n-1 tables (where n is the number of sections).

Also, there's no reason it needs to be only two strings being aligned at once. An alignment of `\(n\)` strings can be done using an `\(n\)`-dimensional table. In reality, it's rare to do more than three at once. If all `\(n\)` strings have length `\(O(m)\)`, then the overall runtime will be `\(O(m^n)\)`, which is pretty prohibitive for large `\(n\)`. Instead, the strings are typically aligned in groups, a "composite" string is generated for each group, and then the procedure is repeated on the composite strings. This produces pretty good results in practice, although it is not guaranteed to produce optimal results like the full `\(n\)`-dimensional algorithm is.

I've personally never implemented any of these last few extensions... the code can definitely get pretty hairy, so I'll leave that as an exercise to the reader 😉

Here's a zip of all of the code used in this post: [alignment.zip][1]

 [1]: /assets/alignment.zip
