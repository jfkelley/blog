---
title: Obscure Java features
author: joefkelley
layout: post
date: 2013-03-06
url: /274
sfw_pwd:
  - JBcegYEheevB
categories:
  - Programming

---
Here's a few features of the Java language that you might not have heard of. I think it's interesting to plumb the depths of a relatively simple language.

* * *

### 1. Labeled loops and other strange control flow

Did you know that you can give loops and if-statements names? For instance:
  


~~~Java
countToTen: for(int n = 1; n <= 10; n++) {
	System.out.print(n + "... ");
	evenOrOdd: if (n % 2 == 0) {
		System.out.println("is even.");
	} else {
		System.out.println("is odd.");
	}
}
~~~


  
You can also do `break [label];` and `continue [label];` for tricky nested loop situations. Now I recognize it as a code-smell, but the one time I can remember actually using this went something like this:
  


~~~Java
Scanner in = openInput();
outerLoop: while (true) {
	String input;
	do {
		input = in.nextLine();
		if (isEndOfInput(input)) {
			break outerLoop;
		}
	} while (!isValidInput(input));
	processInput(input);
}
~~~


  
At this point, I don't remember why I felt the need to do this. It's definitely a feature that should probably never be used. But hey, this post is about obscure Java features, and if it was actually useful, it probably wouldn't be obscure!

* * *

### 2. goto

"goto" is a reserved word in Java. Try naming a variable "goto" and the compiler will throw an error.
  


~~~Java
int goto = 3;
~~~


  
Spits out:
  
`Syntax error on token "goto", invalid VariableDeclaratorId`
  
It's not used for anything, though.

* * *

### 3. Double-bracket initialization

Take a look at this code for initializing a list:
  


~~~Java
List<String> words = new ArrayList<String>() {{
	add("quick");
	add("brown");
	add("fox");
}};
~~~


  
The double-bracket at first seems like a strange thing, but it's actually a combination of two often-used constructs: an anonymous inner class, and initialization code. Written like this, it's a bit clearer:
  


~~~Java
List<String> words = new ArrayList<String>() {
	{
		add("quick");
		add("brown");
		add("fox");
	}
};
~~~


  
The first bracket starts the definition of an anonymous inner class, and the second bracket provides some code that is run when that class is instantiated. Note that this code is run _after_ the super-class constructor, so it's safe to call any methods like "add".

It's probably a bad idea to do this in most cases, but it can sometimes be appropriate. For this specific example, I would probably prefer:
  


~~~Java
List<String> words = Arrays.asList("quick", "brown", "fox");
~~~

* * *

### 4. Array type syntax

~~~Java
public char foo(int n)[] {
	char a[] = new char[n];
	return a;
}
~~~

Java is actually pretty flexible on where you put the brackets when declaring methods with the an array return type, or variables whose type is an array.

* * *

### 5. Primitive classes

There are actually classes for int, double, char, etc. (No, not Integer, Double, Character). They sometimes come up with reflection. Consider:

~~~
import java.lang.reflect.Method;

public class Junk {

	public static void main(String[] args) throws Exception {
		Junk j = new Junk();
		Method m = j.getClass().getMethod("foo", int.class);
		m.invoke(j, 4);
	}

	public void foo(int n) {
		System.out.println("int: " + n);
	}

	public void foo(double d) {
		System.out.println("double: " + d);
	}

}
~~~

Because the foo method is overloaded, we must specify which we want by passing in the class of the parameter. To make this possible when parameters are primitive types, all of the primitives have respective class objects. Note that changing that to "Integer.class" causes a NoSuchMethodException.

* * *

### 6. Calling static methods on specific objects

The usual way of calling static methods is by the class name itself. However, they can also be called on instances of the class. For example:

~~~Java
String s1 = "hi";
String s2 = s1.valueOf(5);
~~~

The value of s2 will be "5". The value of s1 is completely ignored. In fact, you can do the same on null objects. This looks so wrong; it definitely seems like it should throw a NullPointerExceptions, but this works:

~~~Java
System s = null;
s.out.println("hello");
~~~

Note that this will use the _compile-time_ type of an object, not the run-time type. To illustrate:

~~~Java
public class Junk {

	public static void main(String[] args) {
		Junk j = new SubJunk();
		j.foo();
	}

	public static void foo() {
		System.out.println("Hello");
	}

	private static class SubJunk extends Junk {
		public static void foo() {
			System.out.println("Goodbye");
		}
	}

}
~~~

This will output "Hello". You don't get inheritance or method overriding or anything like that with static methods.

* * *

### 7. Non-static inner classes

Most are familiar with static inner classes and interfaces; the Java standard libraries are full of them. Map.Entry is a common example. But inner classes can also be non-static, and it changes how they work slightly. Basically, each instance of the inner class has to be associated with an "enclosing" instance of the outer class. Here's an example that illustrates a few things:

~~~Java
public class Junk {

	private class InnerJunk {
		private String s;
		public InnerJunk(String str) {
			s = str;
		}

		public void foo() {
			System.out.println("InnerJunk: " + s);
			Junk.this.foo();
		}
	}

	private String s;

	public Junk(String str) {
		s = str;
	}

	public void foo() {
		System.out.println("OuterJunk: " + s);
	}

	public InnerJunk createInner(String str) {
		return new InnerJunk(str);
	}

	public static void main(String[] args) {
		/*
		 * InnerJunk inner = new InnerJunk("sup")
		 *    ^^ would cause a compile error:
		 *
		 * No enclosing instance of type Junk is accessible. Must
		 * qualify the allocation with an enclosing instance of
		 * type Junk (e.g. x.new A() where x is an instance of Junk)
		 */

		InnerJunk inner = new Junk("sup").new InnerJunk("hello");
		inner.foo();
	}

}
~~~

First, note that the inner class can access its enclosing outer class instance by doing "Junk.this". Second, notice that there are two ways of associating an inner class instance with an enclosing outer class instance: implicitly, as is done in the createInner method, or explicitly, as is done in the main method.
