---
title: Explicitly using Scala Implicit conversions
author: joefkelley
layout: post
date: 2015-11-18
url: /904
sfw_pwd:
  - pZWc5MuSQiOU
xyz_fbap:
  - 1
categories:
  - Programming

---
It seems a bit of a contradiction, but I recently had a problem with Scala's implicit conversions being a bit _too_ implicit. I wanted a simple way to make them more explicit.

Here's an example usage of implicit conversions:

~~~scala
case class Foo(x: Int)

case class Bar(s: String) {
  def doSomething(): Unit = {
    println("Bar: " + s)
  }
}

implicit def foo2bar(foo: Foo): Bar = new Bar(foo.x.toString)

new Foo(10).doSomething()
~~~

Predictably, this prints "Bar: 10". But there are some problems with implicit conversions like this: they have some fairly complex scope and usage requirements, and it can often be easy to lose track of which type something is at what point, and which conversions are being applied. Also, there are some cases where the conversion is ambiguous. Consider adding a third class:

~~~scala
case class Baz() {
  def doSomething(): Unit = {
    println("Baz")
  }
}

implicit def foo2baz(foo: Foo): Baz = new Baz()

new Foo(10).doSomething()
~~~

Now, this last line fails with a compiler error:

> type mismatch; found : Foo required: ?{def doSomething: ?} Note that implicit conversions are not applicable because they are ambiguous: both method foo2bar of type (foo: Foo)Bar and method foo2baz in object FooBarBaz of type (foo: Foo)Baz are possible conversion functions from Foo to ?{def doSomething: ?}

Both implicit conversions are applicable, so the compiler responsibly refuses to apply either since it cannot know which one we want. Of course, you can simply call either method explicitly to resolve this:

~~~scala
foo2bar(new Foo(10)).doSomething()
foo2baz(new Foo(10)).doSomething()
~~~

But that's a little clunky. First of all, it messes up my ability to chain methods and think about executing them left-to-right: "`x.doA().doB(y).doC()`" is much more readable than "`doB(x.doA(), y).doC()`", I think. Also, the point of implicit conversions is that I don't actually care _which_ method does the conversion, just that _some_ implicit method exists and type-checks correctly. Wouldn't it be nice if the Scala language had some feature to specify the type you want to convert to without specifying the method? Something like:

~~~scala
new Foo(10).as[Bar].doSomething()
new Foo(10).as[Baz].doSomething()
~~~

Luckily, no need for the language to implement this; it's easily implementable (ironically) as an implicit conversion! Here's the real meat of this post:

~~~scala
class ImplicitApplicator[A](a: A) {
  def as[B](implicit conv: A => B): B = conv(a)
}
implicit def toImplicitApplicator[A](a: A) = new ImplicitApplicator(a)
~~~

Importing this class and implicit conversion allows the use of the above syntax on any object! I've even started using it in some places where it's not needed, simply to provide a kind of mid-line type annotation for readability.
