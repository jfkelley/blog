---
title: Let the compiler prevent bugs
author: joefkelley
layout: post
date: 2014-04-24
url: /714
sfw_pwd:
  - MNlP2eoI3kRQ
categories:
  - Programming

---
I'm a fan of statically-typed languages. A type-aware compiler is extremely helpful in suggesting the correctness of code, but it has to be used properly. This is an idea that I think good programmers are aware of, but I don't think I've seen it explicitly called out. You should write your code in such a way that the logic is tied to the type. This allows the compiler to do more for you and prevent more bugs.

I think the classic example is the argument against null and in favor of an Option type. Take as an example, some code that might allow a user to comment on a picture:

~~~java
public void commentOnPicture(long pictureId, long userId, String comment) {
	Picture pic = getPictureFromDatabase(pictureId);
	User user = getUserFromDatabase(userId);
	user.commentOn(pic.getComments(), comment);
}

private User getUserFromDatabase(long userId) {
	Database db = Database.openConnection();
	try {
		return db.getUser(userId)
	} catch (NotFoundException e) {
		return null;
	} finally {
		db.close();
	}
}

private Picture getPictureFromDatabase(long pictureId) {
	Database db = Database.openConnection();
	try {
		return db.getPicture(pictureId);
	} catch (NotFoundException e) {
		return null;
	} finally {
		db.close();
	}
}
~~~

The obvious question is what happens if that pictureId and that userId aren't found in the database. This code would throw a NullPointerException.

Better would be to return an Option type from the "get*FromDatabase" methods. As a sample Option type:

~~~java
public abstract class Option<T> {
	public abstract boolean hasValue();
	public abstract T getValue();

	public static <T>; Some<T> some(T t) { return new Some<T>(t); }
	public static <T> None<T> none() { return new None<T>(); }

	private static class Some<T> extends Option<T> {
		private final T t;
		private Some(T t) {
			this.t = t;
		}
		public boolean hasValue() { return true; }
		public T getValue() { return t; }
	}

	private static class None<T> extends Option<T> {
		private None() { }
		public boolean hasValue() { return false; }
		public T getValue() { throw new RuntimeException("can't get a value from None"); }
	}
}
~~~

And a refactor to the getFromDatabase methods:

~~~java
private Option<User> getUserFromDatabase(long userId) {
	Database db = Database.openConnection();
	try {
		return Option.some(db.getUser(userId));
	} catch (NotFoundException e) {
		return Option.<User>none();
	} finally {
		db.close();
	}
}

private Option<Picture> getPictureFromDatabase(long pictureId) {
	Database db = Database.openConnection();
	try {
		return Option.some(db.getPicture(pictureId));
	} catch (NotFoundException e) {
		return Option.<Picture>none();
	} finally {
		db.close();
	}
}
~~~

Then, our original code would fail to compile. So would this:

~~~java
public void commentOnPicture(long pictureId, long userId, String comment) {
	Option<Picture> pic = getPictureFromDatabase(pictureId);
	Option<User> user = getUserFromDatabase(userId);
	user.commentOn(pic.getComments(), comment);
}
~~~

It forces you to write code like this to even get it to compile:

~~~java
public void commentOnPicture(long pictureId, long userId, String comment) {
	Option<Picture> pic = getPictureFromDatabase(pictureId);
	Option<User> user = getUserFromDatabase(userId);
	if (pic.hasValue() && user.hasValue()) {
		user.getValue().commentOn(pic.getValue().getComments(), comment);
	}
}
~~~

There is still the possibility that this code could compile, however:

~~~java
public void commentOnPicture(long pictureId, long userId, String comment) {
	Option<Picture> pic = getPictureFromDatabase(pictureId);
	Option<User> user = getUserFromDatabase(userId);
	user.getValue().commentOn(pic.getValue().getComments(), comment);
}
~~~

The introduction of functions in Java 8 allows for a better Option type (in fact, it's now in the standard libraries: [Optional][1]), but going down the functional/flatMap/monad rabbit hole could be a whole other blog post. The main thing that I want to say is that encouraging the caller to only use a result in a safe way is a very good thing. For example, here's the Scala version of the above:

~~~Scala
def commentOnPicture(pictureId: Long, userId: Long, comment: String) = {
  (getPictureFromDatabase(pictureId), getUserFromDatabase(userId)) match {
    case (Some(pic), Some(user)) => user.commentOn(pic.getComments(), comment)
  }
}
~~~

By using pattern-matching, the same piece of code that checks that we actually got a result (it is a Some and not a None) also extracts the value from the result. If you always use pattern-matching when dealing with Scala Options, it's literally impossible to screw it up and try to do something with a None that you can't do.

Another problem with the original code is that it's easy to make an error when calling the method. This will compile:

~~~java
long pictureId = 12345;
long userId = 67890;
commentOnPicture(pictureId, userId, "nice pic!");
commentOnPicture(userId, pictureId, "nice pic!");
~~~

Without scrolling up, do you remember which is correct? What if we created simple classes to encapsulate the ID's:

~~~java
public class UserID {
	public final long id;
	public UserID(long id) { this.id = id; }
}

public class PictureID {
	public final long id;
	public PictureID(long id) { this.id = id; }
}
~~~

And then we could change the method signature to accept these classes instead of just longs. Then this compiles:

~~~java
commentOnPicture(new PictureID(12345), new UserID(67890), "nice pic!");
~~~

But this does not:

~~~java
commentOnPicture(new UserID(67890), new PictureID(12345), "nice pic!");
~~~

This type of thing can really help guard against complicated refactoring that involves changing method signatures. Well-typed code should make it so that simply compiling correctly gives you a small amount of confidence that you didn't break anything too badly. I'll admit that this is questionable to actually do in practice; it might be just a little too much bloat. But for things like complicated configuration options with 10 different strings, encapsulating that into one or more good types is an awesome idea.

The final example of a pattern I've seen a few times is a great way to deal with stateful, mutable objects. Let's say there's a Car class, and it has an engine state- off or on. Maybe it's a hybrid car, so it can also be running on just the electric motor. As a first pass:

~~~java
public enum EngineState {
	OFF, ON, ELECTRIC
}

public class Car {

	private EngineState engineState;
	public final String color;
	public final String manufacturer;

	public Car(EngineState engineState, String color, String manufacturer) {
		this.engineState = engineState;
		this.color = color;
		this.manufacturer = manufacturer;
	}

	public void setEngineState(EngineState state) {
		this.engineState = state;
	}

}

public class CarController {

	public static void turnOff(Car car) {
		car.setEngineState(EngineState.OFF);
	}

	public static void turnOn(Car car) {
		car.setEngineState(EngineState.ELECTRIC);
	}

	public static void goFaster(Car car) {
		car.setEngineState(EngineState.ON);
	}

}
~~~

The CarController class is pretty suspect here. For instance, what should happen if "turnOn" is called on a car that's already on? For a car that's off, turning it on should probably just start the electric motor, but what if it's a car with both already running; seems weird that turnOn would actually turn off the gas engine. It's probably best that turnOn restricts its input to only cars that are already off. Similarly, goFaster should only allow cars that are in electric-only mode. turnOff can turn off any car. We can do this by moving the engine state into the type of the Car. We could do this through subclasses, but I think a better way is through generics:

~~~java
public interface EngineState {
	public static interface ON extends EngineState {}
	public static interface OFF extends EngineState {}
	public static interface ELECTRIC extends EngineState {}
}

public class Car<STATE extends EngineState> {

	public final String color;
	public String manufacturer;

	public Car(String color, String manufacturer) {
		this.color = color;
		this.manufacturer = manufacturer;
	}

	public Car(Car<?> other) {
		this(other.color, other.manufacturer);
	}

}

public class CarController {

	public static Car<EngineState.OFF> turnOff(Car<?> car) {
		return new Car<EngineState.OFF>(car);
	}

	public static Car<EngineState.ELECTRIC> turnOn(Car<EngineState.OFF> car) {
		return new Car<EngineState.ELECTRIC>(car);
	}

	public static Car<EngineState.ON> goFaster(Car<EngineState.ELECTRIC> car) {
		return new Car<EngineState.ON>(car);
	}

}
~~~

Rather than add all the logic to restrict which types of cars are passed in, the compiler is now doing the work. If you try to pass a `Car<ON>` into turnOn, the compiler will error. Note also that the return types are very specific. Perhaps later if turnOn was changed to turn on both the electric and gas motors, then it would have a different return type. Any code that was calling turnOn and expecting a `Car<ELECTRIC>` back would now not compile. This is a good thing! The compiler is pointing out a bug that would not otherwise be there. If instead the caller did not care about the engine state of the car and only wanted a `Car<?>`, then the compiler would also be just fine. Again, this is a good thing! If the caller is only using it as a `Car<?>`, then we know that they can't really do anything bad with it; the only engine operation possible at that point is turnOff. The compiler is actually helping to check business logic! Can your Python REPL do that?

... but trash-talking aside, I actually would be interested to hear if dynamically-typed languages have techniques for letting the language do more to help prevent bugs and check business logic. I barely know any Python.

 [1]: http://docs.oracle.com/javase/8/docs/api/java/util/Optional.html
