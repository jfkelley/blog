---
title: An abuse of the Java type system
author: joefkelley
layout: post
date: 2013-08-13
url: /460
sfw_pwd:
  - TXb0ima2oP4K
categories:
  - Programming

---
What if you wanted to have a List or Collection of items, but with completely different types? Obviously, the standard way to deal with this would be with simply a List<Object>, but then accessing items would require casts and type-checks and would not receive any of the typical benefits of statically-typed code. So is there a way to dynamically create a List, where each item has a statically-checked, and entirely different type?

The _real_ solution to this issue is to take a step back and ask why you want a list like this instead of a class encapsulating it with several members of different types... but there actually is a way to do this within the java type system that is completely ugly and terrible, but perhaps somewhat interesting:

~~~java
public abstract class TypedList<A, B extends TypedList<?,?>> {

	public abstract A head();
	public abstract B tail();
	public abstract boolean isEmpty();

	public static <A, B extends TypedList<?, ?>> TypedList<A, B> cons(A a, B b) {
		return new SomethingList<A, B>(a, b);
	}

	public static NothingList getEmptyList() {
		return NothingList.getInstance();
	}

	private static class NothingList extends TypedList<Void, NothingList> {

		private static final NothingList nl = new NothingList();

		public static NothingList getInstance() { return nl; }

		private NothingList() {}

		@Override
		public Void head() {
			throw new UnsupportedOperationException();
		}

		@Override
		public NothingList tail() {
			return this;
		}

		@Override
		public boolean isEmpty() {
			return true;
		}

	}

	private static class SomethingList<A, B extends TypedList<?, ?>> extends TypedList<A, B> {

		private A elem;
		private B rest;

		public SomethingList(A a, B b) {
			elem = a;
			rest = b;
		}

		@Override
		public A head() {
			return elem;
		}

		@Override
		public B tail() {
			return rest;
		}

		@Override
		public boolean isEmpty() {
			return true;
		}

	}
}
~~~

Basically, this is a functional-style linked list, with several additional type parameters. The first parameter is the type of the first element in the list, and the second parameter is the type of the rest of the list. With this, you can commit such atrocities as:

~~~java
TypedList<String, TypedList<Double, TypedList<String, TypedList<ArrayList<String>, NothingList>>>> list =
	cons("hello!", cons(3.14, cons("world!", cons(new ArrayList<String>(), TypedList.getEmptyList()))));

String first = list.head();
Double second = list.tail().head();
String third = list.tail().tail().head();
ArrayList<String> fourth = list.tail().tail().tail().head();
~~~

Here, we have a list containing first a String, then a Double, then a String, and then an ArrayList, and creating and accessing the elements in this list are all type-safe operations. Of course, the downside is that in order to get the type-safety, you must explicitly state the type of every element of the list ahead of time, which is impossible with variable-length lists. But if we want a list of arbitrary length, we can certainly abandon type-safety:

~~~java
// build the list
TypedList<?,?> list = TypedList.getEmptyList();
for (int i = 0; i < 10; i++) {
	list = cons(i, list);
}

// print out (and destroy) the list
while (!list.isEmpty()) {
	System.out.println(list.head());
	list = list.tail();
}
~~~

And we can even mix the two approaches (this code makes me cringe):

~~~java
TypedList<?,?> list = TypedList.getEmptyList();
for (int i = 0; i < 10; i++) {
	list = cons(i, list);
}
TypedList<String, ?> stringHeadedList = cons("Hello, ", list);
~~~

Ugh. And how about this class header...

~~~java
public class TypedTree<A, L extends TypedTree<?, ?, ?>, R extends TypedTree<?, ?, ?>>
~~~

Or this...

~~~java
public class TypedMap<K, V, A extends TypedMap<?, ?, ?>>
~~~

Basically, there are a lot of truly awful things you can do with Java generics; maybe I'll eventually get around to a post about the good, useful things.
