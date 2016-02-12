---
title: I wish I was writing Scala
author: joefkelley
layout: post
date: 2014-01-14
url: /574
sfw_pwd:
  - BhxHNfP4aGa6
categories:
  - Programming

---
Today I wrote a chunk of Java code that really hammers home the usefulness of functional programming by how much it sucks. I had to take a list of strings, and create a new list containing only the first words of the input list. For simplicity, "word" means anything up until the first whitespace. Here it is:

~~~java
List<String>; result = new ArrayList<String>();
for (String line : input) {
	for (int i = 0; i < line.length(); i++) {
		if (Character.isWhitespace(line.charAt(i))) {
			result.add(line.substring(0, i)); break;
		} else if (i == line.length() - 1) {
			result.add(line);
		}
	}
}
return result;
~~~

And here is the same functionality, but in Scala. I chose Scala because it's such a close cousin of Java:

~~~scala
input.map(_.takeWhile(!_.isWhitespace))
~~~

Java 8 lambdas will certainly help, but I'm beginning to wonder if that's just putting lipstick on a pig.
