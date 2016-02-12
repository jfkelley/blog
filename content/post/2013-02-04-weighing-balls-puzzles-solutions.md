---
title: 'Weighing Balls Puzzles - Solutions'
author: joefkelley
layout: post
date: 2013-02-04
url: /239
sfw_pwd:
  - R9X1yXRPojDD
categories:
  - Puzzle

---
Here are the solutions to the puzzles in the last post. If you want to read those first, click [here.][1]

Version 1: Easy
  
The basic idea is that there are three possible outcomes to every weighing: the left side is heavier, the right is heavier, or they are equal. So in the first weighing we want to have those three outcomes split the possible heavy balls into three groups of three possibilities. To do that, weigh balls 1 through 3 against balls 4 through 6. If 1 through 3 are heavier, then we know one of those is the heavier one, and the same if 4 through 6 are heavier. If they are equal, then one of 7 through 9 is heavier. We can then weigh one of the remaining candidate balls against another. If one of those is heavier, it's the solution. If they are equal, then the third must be the heaviest. Written out more formally:

~~~
weigh [1, 2, 3] - [4, 5, 6]
if [1, 2, 3] is heavier:
	weigh 1 - 2
	if 1 is heavier:
		1 is heavy
	if 2 is heavier:
		2 is heavy
	if equal:
		3 is heavy
if [4, 5, 6] is heavier:
	weigh 4 - 5
	if 4 is heavier:
		4 is heavy
	if 5 is heavier:
		5 is heavy
	if equal:
		6 is heavy
if equal:
	weigh 7 - 8
	if 7 is heavier:
		7 is heavy
	if 8 is heavier:
		8 is heavy
	if equal:
		9 is heavy
~~~

Version 2: Medium
  
Here there are 8 possible outcomes: 1 is heavier, 2 is heavier, 3 is heavier, 4 is heavier, 1 is lighter, 2 is lighter, 3 is lighter, and 4 is lighter. Let ball 0 be the one we know is the right weight. I'll abbreviate these as: 1+, 2+, 3+, 4+, 1-, 2-, 3-, and 4-, respectively. By the second weigh, we must have already narrowed it down to only three possibilities. Any more, and it would not be possible to narrow it down to only one possibility with only one weighing, which has only three possible outcomes. So the first weigh must split these possibilities into two groups of three and one of two. Let's try a few. A natural first guess is 1 and 2 against 3 and 4.

  * If 1 and 2 are heavier, then we have narrowed the possibilities to: [1+, 2+, 3-, 4-]
  * If 3 and 4 are heavier, we have narrowed it to: [1-, 2-, 3+, 4+]
  * If they are equal... well, they can't be equal

Here we are "wasting" the possibility of them being equal, and consequently, the other two outcomes only narrow the possibilities down to 4. Let's try again, by weighing just 1 vs 2.

  * If 1 is heavier, then: [1+, 2-]
  * If 2 is heavier, then: [1-, 2+]
  * If equal: [3+, 3-, 4+, 4-]

Better, but we still have four possibilities in the equal case, which is too many. How about 0 and 1 vs 2 and 3.

  * If 0 and 1 are heavier: [1+, 2-, 3-]
  * If 2 and 3 are havier: [1-, 2+, 3+]
  * If equal: [4+, 4-]

So that will work! Now resolving the equal case is easy: just weigh 4 against 0. The other cases are a little bit trickier, but they can both be handled by weighing 2 against 3. If one is heavier than the other, then one of those two is the odd one. If they are equal, then 1 is the odd one. The full process:

~~~
weigh [0, 1] - [2, 3]
if [0, 1] is heavier:
	weigh 2 - 3
	if 2 is heavier:
		3 is light
	if 3 is heavier:
		2 is light
	if equal:
		1 is heavy
if [2, 3] is heavier:
	weigh 2 - 3
	if 2 is heavier:
		2 is heavy
	if 3 is heavier:
		3 is heavy
	if equal:
		1 is light
if equal:
	weigh 0 - 4
	if 0 is heavier:
		4 is light
	if 4 is heavier:
		4 is heavy
	if equal:
		(impossible)
~~~

Versions 3. hard and 4. impossible:
  
First, take a look at the solution to the 12 balls problem, then I'll explain it and explain how to find it:

1. Weigh [1, 4, 5, 6] against [7, 10, 11, 12]
2. Weigh [2, 4, 10, 11] against [5, 8, 9, 12]
3. Weigh [3, 8, 10, 12] against [6, 7, 9, 11]

Consider what happens if the first weighing comes out heavier for the first group, and the other two are equal. The only ball involved in the first and no others is 1, so it must be heavier. If instead the first weighing had been heavier for the other group, then 1 would be lighter.

Consider what happens if the first comes out equal, the next comes out heavier for the left, and the third heavier for the right. Both balls 8 and 9 are involved in only the second and third weighings, but ball 8 is on opposite sides, whereas ball 9 is on the right both times. Therefore, it must be ball 8 that's different, and based on which side was heavier in each weighing, we can tell that 8 is lighter.

In fact, each of the balls is used in the weighings in such a way that this is always possible. Either they are used in different sets of weighings, or if used in the same set, they are placed on different sides in such a way that it's always possible to tell which is the odd one.

Now, let's go about how you would construct a set of weighings like this. First, let's represent the weighings that a ball is present in as a vector with three values, and each value is either 0, 1, or -1; the value will be 0 if the ball is not in that weighing at all, 1 if it is on the left, and -1 if it is on the right. For example, ball 1 would be [1,0,0], and ball 8 would be [0,-1,1]. We must come up with a set of 12 such vectors, with no duplicates, and if no "additive inverses". That is, if [1,0,0] is in the set, [-1,0,0] must not be. Furthermore, these vectors must all add up to [0,0,0], otherwise we will have weighings with more balls on one side than the other.

It's actually a little easier to do this in two steps: First, come up with a set of 12 that are unique (and "inverse-unique") but don't add up to [0,0,0]. Then, multiply some of them by -1 in order to make them add to [0,0,0]. Let's go through this for the above solution. Here's the first step:

  1. [1,0,0]
  2. [0,1,0]
  3. [0,0,1]
  4. [1,1,0]
  5. [1,-1,0]
  6. [1,0,-1]
  7. [1,0,1]
  8. [0,1,-1]
  9. [0,1,1]
 10. [-1,1,1]
 11. [1,-1,1]
 12. [1,1,-1]

Then, we must take the inverse of some of these in order to have everything add up to 0. In this case, if we flip 7, 8, 9, 11, and 12, everything works out. So our final vectors are:

  1. [1,0,0]
  2. [0,1,0]
  3. [0,0,1]
  4. [1,1,0]
  5. [1,-1,0]
  6. [1,0,-1]
  7. [-1,0,-1]
  8. [0,-1,1]
  9. [0,-1,-1]
 10. [-1,1,1]
 11. [-1,1,-1]
 12. [-1,-1,1]

From this, we can recover the original weighings.

This process is easily generalizable to more balls and more weighings. The length of the vectors must equal the number of weighings, and the number of vectors is the number of balls, but the constraints about uniqueness and summing to 0 are the exact same. For the "impossible" and up versions, I wrote a program to do this. The first step simply generates all `\(3^n\)` vectors consisting of 0, 1, and -1, and then only keeps unique ones. The second step does a recursive greedy search, which in theory could be prohibitively slow, but in practice, is quick enough.

Here are the solutions I generated. These really aren't that interesting once you know the process, but I'll post them just to prove that it does, in fact, work:

39 balls, 4 weighings:
  
~~~
[4, 5, 6, 7, 15, 16, 18, 19, 20, 31, 32, 35, 39] - [1, 2, 3, 12, 13, 14, 17, 21, 22, 23, 30, 33, 34]
[1, 2, 6, 7, 8, 17, 18, 19, 21, 26, 29, 34, 35] - [3, 4, 5, 9, 10, 11, 16, 20, 22, 23, 27, 28, 38]
[1, 3, 5, 7, 9, 12, 13, 15, 22, 25, 28, 29, 37] - [2, 4, 6, 8, 10, 11, 14, 20, 21, 23, 24, 32, 33]
[2, 3, 5, 6, 10, 12, 14, 15, 19, 24, 25, 30, 31] - [1, 4, 7, 8, 9, 11, 13, 16, 17, 18, 26, 27, 36]
~~~

120 balls, 5 weighings:
  
~~~
[2, 5, 6, 10, 11, 14, 15, 17, 19, 20, 25, 30, 32, 35, 39, 41, 44, 49, 50, 58, 61, 64, 65, 67, 68, 70, 72, 74, 76, 78, 83, 85, 87, 93, 98, 99, 100, 107, 114, 116] - [1, 3, 4, 7, 9, 12, 13, 16, 18, 23, 24, 31, 33, 38, 40, 43, 45, 46, 48, 56, 59, 60, 62, 63, 66, 69, 71, 73, 75, 77, 79, 84, 86, 92, 94, 96, 106, 111, 112, 118]
[6, 12, 13, 17, 18, 21, 23, 24, 33, 36, 38, 40, 44, 47, 48, 52, 54, 55, 57, 58, 61, 66, 69, 71, 73, 75, 78, 80, 81, 83, 85, 87, 89, 93, 97, 99, 105, 107, 115, 120] - [1, 7, 14, 16, 19, 20, 22, 25, 30, 34, 35, 37, 39, 42, 43, 51, 53, 56, 59, 60, 62, 65, 68, 70, 72, 74, 76, 79, 82, 84, 86, 88, 92, 94, 95, 101, 104, 106, 113, 117]
[19, 20, 32, 35, 43, 45, 46, 48, 51, 57, 58, 60, 62, 63, 65, 68, 69, 71, 73, 75, 77, 80, 81, 84, 87, 89, 90, 95, 98, 100, 101, 102, 105, 107, 111, 113, 114, 116, 117, 120] - [8, 18, 31, 33, 38, 44, 47, 49, 50, 52, 56, 59, 61, 64, 66, 67, 70, 72, 74, 76, 78, 79, 82, 83, 85, 86, 88, 91, 96, 97, 103, 104, 106, 108, 109, 110, 112, 115, 118, 119]
[1, 5, 9, 12, 13, 16, 22, 27, 30, 32, 35, 39, 41, 42, 43, 47, 52, 54, 55, 56, 59, 64, 66, 67, 70, 80, 81, 83, 85, 88, 93, 95, 96, 101, 102, 107, 108, 109, 110, 111] - [4, 8, 10, 11, 14, 17, 21, 28, 29, 31, 33, 38, 40, 44, 48, 49, 50, 51, 53, 58, 63, 65, 68, 69, 72, 74, 76, 78, 82, 84, 89, 92, 94, 97, 98, 100, 103, 106, 112, 120]
[1, 7, 9, 11, 12, 17, 22, 26, 27, 29, 32, 37, 39, 41, 43, 45, 49, 52, 57, 61, 62, 64, 68, 71, 76, 78, 79, 85, 87, 90, 92, 97, 99, 101, 104, 109, 111, 112, 113, 119] - [3, 5, 8, 13, 15, 18, 20, 23, 25, 30, 34, 35, 38, 42, 47, 48, 53, 54, 56, 58, 65, 67, 70, 74, 75, 77, 80, 82, 83, 88, 94, 96, 100, 102, 107, 110, 116, 117, 118, 120]
~~~

363 balls, 6 weighings:
  
~~~
[3, 4, 8, 14, 15, 18, 20, 21, 24, 30, 31, 33, 35, 37, 38, 41, 44, 47, 49, 50, 55, 59, 62, 63, 73, 77, 81, 86, 88, 92, 94, 95, 97, 98, 100, 106, 109, 114, 118, 124, 125, 126, 129, 132, 136, 137, 139, 141, 143, 145, 147, 152, 155, 157, 159, 161, 162, 165, 166, 167, 175, 178, 179, 183, 188, 193, 199, 200, 202, 209, 217, 218, 220, 222, 228, 232, 238, 239, 240, 242, 244, 249, 251, 253, 261, 264, 273, 276, 279, 281, 282, 285, 286, 287, 288, 291, 294, 298, 300, 302, 304, 306, 307, 309, 310, 313, 315, 318, 320, 326, 332, 335, 336, 338, 339, 341, 345, 351, 354, 356, 362] - [1, 2, 5, 11, 16, 17, 19, 22, 23, 27, 28, 29, 34, 36, 39, 42, 43, 45, 46, 48, 51, 53, 56, 60, 61, 64, 65, 71, 75, 79, 83, 84, 96, 99, 101, 103, 104, 111, 113, 115, 123, 127, 128, 130, 135, 138, 140, 142, 144, 146, 148, 149, 150, 151, 154, 158, 160, 163, 164, 168, 176, 177, 180, 182, 185, 190, 194, 195, 201, 203, 207, 211, 219, 226, 229, 230, 234, 236, 237, 241, 243, 248, 250, 252, 256, 266, 269, 270, 271, 272, 274, 280, 283, 284, 289, 290, 295, 296, 299, 301, 303, 305, 308, 311, 312, 314, 316, 317, 319, 324, 329, 333, 334, 337, 340, 343, 349, 353, 355, 360, 363]
[2, 3, 7, 8, 14, 15, 17, 20, 21, 22, 24, 26, 30, 31, 33, 35, 37, 39, 53, 55, 66, 71, 72, 75, 80, 85, 86, 88, 90, 95, 97, 98, 100, 101, 102, 103, 104, 105, 107, 110, 114, 118, 121, 123, 125, 126, 129, 130, 133, 135, 136, 139, 141, 143, 145, 147, 150, 151, 153, 158, 160, 161, 163, 168, 170, 171, 174, 180, 186, 187, 190, 192, 194, 197, 201, 203, 205, 207, 208, 211, 212, 214, 217, 220, 222, 228, 232, 237, 239, 258, 262, 269, 270, 272, 277, 280, 281, 285, 292, 295, 296, 297, 299, 302, 304, 305, 306, 316, 317, 319, 328, 329, 330, 332, 334, 338, 343, 350, 352, 355, 359] - [1, 4, 5, 9, 11, 12, 16, 18, 19, 23, 25, 29, 32, 34, 36, 38, 42, 50, 51, 56, 58, 68, 73, 74, 76, 77, 78, 82, 84, 87, 89, 91, 92, 93, 94, 96, 99, 106, 108, 112, 115, 117, 120, 122, 124, 127, 128, 131, 132, 134, 137, 138, 140, 142, 144, 146, 148, 149, 152, 155, 156, 165, 167, 169, 172, 173, 178, 179, 184, 188, 189, 191, 196, 198, 200, 202, 204, 206, 209, 210, 213, 215, 218, 219, 226, 229, 234, 236, 238, 240, 241, 263, 265, 267, 268, 273, 275, 276, 278, 279, 283, 293, 298, 300, 301, 303, 307, 308, 309, 318, 320, 326, 331, 335, 336, 337, 345, 348, 356, 358, 361]
[4, 9, 12, 18, 25, 32, 39, 52, 53, 57, 66, 69, 71, 72, 73, 77, 79, 80, 83, 84, 87, 89, 90, 92, 96, 97, 100, 102, 104, 105, 107, 110, 111, 113, 115, 116, 117, 120, 123, 126, 128, 130, 132, 136, 137, 139, 141, 144, 145, 150, 151, 153, 155, 157, 159, 161, 164, 165, 167, 169, 172, 173, 175, 181, 182, 185, 188, 193, 199, 209, 217, 220, 221, 222, 225, 226, 229, 231, 232, 235, 237, 239, 242, 244, 245, 247, 249, 251, 253, 255, 257, 259, 261, 269, 270, 274, 275, 278, 283, 284, 287, 288, 292, 295, 296, 297, 302, 308, 310, 316, 317, 319, 331, 333, 337, 340, 348, 349, 353, 360, 363] - [2, 7, 17, 22, 26, 38, 40, 50, 54, 58, 67, 68, 70, 74, 75, 76, 78, 81, 82, 85, 86, 88, 91, 93, 94, 95, 98, 99, 101, 103, 106, 108, 109, 112, 114, 118, 119, 121, 122, 124, 125, 127, 129, 135, 138, 140, 142, 143, 147, 148, 152, 154, 156, 158, 160, 162, 163, 166, 168, 170, 171, 174, 176, 177, 183, 190, 194, 195, 207, 211, 216, 218, 219, 223, 224, 227, 228, 230, 233, 234, 236, 238, 240, 241, 243, 246, 248, 250, 252, 254, 256, 260, 265, 267, 268, 271, 277, 281, 282, 285, 286, 289, 293, 298, 301, 303, 304, 306, 309, 311, 328, 330, 332, 338, 339, 341, 350, 351, 352, 354, 362]
[1, 4, 5, 10, 15, 18, 19, 20, 23, 33, 35, 36, 42, 44, 47, 52, 55, 57, 58, 62, 66, 69, 72, 81, 82, 87, 89, 91, 96, 104, 108, 109, 112, 116, 117, 120, 122, 123, 128, 130, 131, 134, 135, 138, 140, 144, 150, 151, 157, 159, 162, 165, 166, 167, 169, 175, 180, 186, 187, 190, 194, 196, 198, 201, 203, 205, 207, 211, 213, 215, 216, 217, 220, 223, 233, 234, 236, 239, 249, 251, 252, 256, 260, 266, 271, 274, 277, 279, 281, 284, 285, 289, 290, 292, 297, 298, 302, 304, 306, 309, 311, 312, 314, 323, 326, 327, 328, 330, 332, 333, 335, 336, 338, 344, 345, 349, 350, 352, 353, 356, 359] - [2, 3, 6, 13, 16, 17, 21, 22, 27, 28, 34, 37, 40, 45, 46, 51, 54, 56, 60, 61, 67, 68, 70, 79, 80, 83, 85, 90, 94, 95, 98, 106, 110, 111, 113, 119, 121, 124, 125, 129, 132, 133, 137, 139, 141, 143, 147, 152, 154, 161, 163, 164, 168, 170, 171, 178, 179, 184, 188, 197, 200, 202, 204, 206, 209, 214, 219, 221, 222, 230, 232, 235, 241, 245, 248, 250, 253, 257, 259, 264, 265, 267, 268, 269, 270, 275, 278, 282, 283, 286, 287, 288, 291, 293, 294, 295, 296, 301, 303, 308, 310, 313, 315, 316, 317, 318, 319, 320, 325, 331, 334, 337, 343, 346, 347, 348, 351, 354, 355, 358, 361]
[1, 5, 6, 11, 13, 16, 17, 22, 24, 25, 32, 37, 38, 41, 44, 55, 60, 61, 64, 65, 66, 67, 70, 72, 86, 88, 90, 95, 97, 98, 100, 101, 103, 104, 108, 109, 112, 116, 117, 120, 131, 132, 134, 137, 140, 142, 144, 148, 152, 153, 154, 155, 164, 165, 167, 170, 171, 174, 178, 179, 181, 182, 185, 190, 194, 195, 197, 203, 205, 208, 212, 216, 223, 224, 226, 227, 229, 232, 237, 239, 242, 244, 249, 253, 255, 257, 259, 263, 266, 271, 273, 276, 277, 279, 281, 284, 285, 293, 301, 303, 305, 309, 311, 313, 316, 317, 319, 321, 323, 327, 329, 331, 334, 337, 339, 341, 345, 351, 357, 358, 361] - [3, 8, 10, 14, 15, 18, 20, 26, 27, 28, 36, 39, 40, 42, 43, 51, 56, 58, 62, 63, 68, 69, 71, 84, 91, 92, 93, 94, 96, 99, 102, 105, 106, 107, 110, 111, 113, 119, 121, 133, 135, 141, 143, 145, 146, 147, 149, 150, 151, 156, 157, 158, 159, 160, 161, 162, 163, 166, 169, 172, 173, 180, 183, 188, 193, 196, 198, 199, 204, 206, 210, 221, 225, 228, 230, 234, 236, 238, 240, 241, 243, 248, 252, 254, 256, 258, 260, 262, 264, 269, 270, 272, 275, 278, 282, 283, 286, 292, 297, 302, 307, 310, 312, 314, 315, 318, 320, 322, 324, 325, 328, 330, 332, 333, 335, 336, 338, 340, 343, 349, 359]
[2, 5, 7, 12, 19, 20, 21, 22, 29, 31, 32, 35, 36, 40, 46, 47, 50, 54, 57, 63, 65, 68, 70, 72, 78, 79, 80, 84, 88, 89, 92, 93, 97, 103, 105, 109, 111, 117, 121, 122, 123, 125, 128, 131, 141, 147, 150, 152, 153, 154, 155, 159, 160, 162, 164, 169, 171, 172, 174, 176, 181, 182, 183, 193, 194, 198, 204, 205, 207, 210, 212, 216, 217, 221, 222, 227, 229, 233, 236, 237, 240, 244, 245, 248, 254, 258, 261, 263, 265, 272, 273, 274, 279, 281, 286, 288, 289, 294, 296, 297, 301, 304, 307, 308, 314, 316, 321, 325, 327, 328, 329, 334, 336, 342, 344, 347, 348, 352, 354, 356, 358] - [1, 6, 10, 11, 14, 15, 17, 24, 25, 27, 33, 38, 41, 42, 44, 48, 51, 55, 58, 59, 60, 62, 67, 71, 73, 74, 75, 83, 87, 90, 94, 96, 98, 100, 102, 104, 110, 112, 115, 118, 119, 127, 130, 132, 134, 135, 136, 139, 142, 143, 145, 146, 161, 163, 167, 168, 175, 178, 180, 184, 187, 189, 190, 192, 196, 199, 201, 202, 203, 211, 214, 215, 218, 220, 224, 226, 230, 231, 234, 239, 242, 246, 250, 253, 256, 257, 260, 266, 268, 270, 271, 275, 277, 280, 282, 285, 291, 292, 295, 300, 303, 309, 310, 312, 315, 319, 320, 324, 326, 330, 332, 333, 337, 340, 341, 345, 351, 357, 360, 361, 362]
~~~

 [1]: /233
