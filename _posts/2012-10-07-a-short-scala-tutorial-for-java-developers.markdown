---
layout: post
title: "A short Scala tutorial for Java Developers"
date: 2012-10-07 21:10
comments: true
categories: Scala
---

This post is a tutorial guide for Java programmers who want to learn [Scala](http://www.scala-lang.org/). 

I myself have programmed in Java for many years, yet jumping straight into Scala has made me clueless, and so I started to read some [baby steps](http://www.artima.com/scalazine/articles/steps.html) to learn the new language.

The following post is what I have learnt. 

<!-- more -->


## Methods Declaration ##

We use an example of a max function to illustrate the different ways to declare a method.

You do it like this, in a single line.

``` scala
	def max(x: Int, y: Int): Int = if (x < y) y else x
```

Or, you can skip the return type (Int in this case) and let the compiler infer.

``` scala
	def max(x: Int, y: Int) = if (x < y) y else x
```

If the method is more than a single line, you can wrap with curly braces.

``` scala
	def max(x: Int, y: Int) = { 
		// More lines of code ..
		if (x < y) y else x 
	}
```

You might have already notice. The `return` keyword is actually optional in Scala.  If omitted, the last expression is the value that will be returned.

To be verbose, you can specify the return type with the `return` keyword.

``` scala
	def max(x: Int, y: Int) = { 
		return if (x < y) y else x 
	}
```



## Method with 0 parameter is Special ##

If you have the following method

``` scala
	def foo() = println("foo!")
```

You may call it in 2 ways

``` scala
	foo()
```

Or simply

``` scala
	foo
```

However, there is a guideline when to use which style. If there is a side effect, you should use the parenthesis. In other words, a getter method can skip the parenthesis.



## Method with 1 parameter is Special ##

Int has a method `to` that takes 1 parameter of Int to return a sequence.

``` scala
	0.to(10)
```

You can drop the . and ( ) and simplify to 

``` scala
	0 to 10
```

This alone makes the Scala language beautiful in many ways.



## Method names can contain .+*/ ##

Surprisingly, Scala doesn't have operators, and therefore no operator overloading. 

But it can have a method name '+'. So an expression `1 + 2` is actually

``` scala
	1.+(2)
```

But since *Method with 1 parameter is Special* (read above), 1.+(2) can be simplified to 

``` scala
	1 + 2
```


## val and var declarations ##

To declare a variable, 

``` scala
	var foo = 1
	foo = 2
```

To declare a value, which does not allow you to change/reassign,

``` scala
	val foo = 1
	// foo = 2 is not possible
```

Also, a semi-colon at the end of a line is optional.



## Class constructor ##

The constructor is the class declaration itself, and any constructor parameters can be used in other methods

``` scala
	class Color(color: String, index: Int) {
		// foo() uses color
		def foo() = println(color)
	}

	// Usage
	val c = new Color("Blue", 1)
	c.foo
```

If you need to have some code in the constructor, you could write it right in the class body, right after the declaration.

``` scala
	class Color(color: String, index: Int) {

		if (color == null)
			throw new NullPointerException("Color is null")

		if (index <= 0)
			index = 0

		def foo() = println(color)
	}
```

If you have multiple constructors, you can add them with `this` method. 

``` scala
	class Color(color: String, index: Int) {

		def this(color:String) = this(color, 0)

		def foo() = println(color)
	}

	// Usage
	val c = new Color("Blue")
```


## Static/Singleton object ##

You cannot have static classes or variables in a class. 

Instead, if you want to add a static method, you have to use the `object` declaration, also known as Singleton objects.

``` scala
	object Color {
		def exclaim(s: String) = println(s + " color!")
	}

	// Usage
	Color.exclaim("Pink")
	// Prints "Pink color!"
```

You can also use the static method in the class method.

``` scala
	class Color(color: String, index: Int) {
		def foo() = Color.exclaim(color)
	}

	object Color {
		def exclaim(s: String) = println(s + " color!")
	}
```


## Interface is traits ##

In Java, you have `Interface`. In Scala, you use `traits`. Moreover, you can have non-abstract methods in `traits`.

``` scala
	trait Friendly {
		def greet() = "Hi"
	}
```

To have a `class` implement the `traits`, you use the `extends` keyword. And if you need to override the method, you need to explicitly use `override def`. 

``` scala
	class Dog extends Friendly {
		override def greet() = "Woof"
	}
```

Similar to Java, a class can extend 1 class and multiple traits.

Another difference is that Scala can mix in traits at instantiation time. In the following, we create another trait and use the `with` keyword to instantiate a Dog with that trait.

``` scala
	trait ExclamatoryGreeter extends Friendly {
	  override def greet() = super.greet() + "!"
	}

	// Usage
	val pup = new Dog with ExclamatoryGreeter
	println(pup.greet())
	// Prints "Woof!"
```



## Array does not use subscript [ ] ##

To access an array, you use ( ) instead of [ ]. It is not a matter of symbol choice. Scala uses ( ) because an array is an object with methods.

To access the 4th element of an array, you write

``` scala
	myArray(3)
```

Behind the scene, it is in fact calling a method `apply`. 

``` scala
	myArray.apply(3)
```

Similarly, for setting an array element, you write

``` scala
	myArray(3) = "foo"
```

Which is interpreted as 

``` scala
	myArray.update(3, "foo")
```



## Functions are first class constructs ##

Java is *imperative* style. Scala is *imperative*, but excels in *functional* style too.

Being a functional language, functions are first class constructs. We use an example of printing `args`.

``` scala
	args.foreach(arg => println(arg))
```

The `foreach` is being passed a function

``` scala
	arg => println(arg)
```

The above function has a few characteristics:

- It is an anonymous function (has no name)
- It get passed a single parameter named `arg` and the type is being inferred by compiler
- The main code is simply the `println`
- Yeah, `=>` is used, also call a right arrow

A more complete example of an anonymous method with explicit parameter type

``` scala
	(x: Int, y: Int) => {
		// More code
		x + y
	}
```

As we said, functions are first class constructs, so you basically could assign it to a variable

``` scala
	var add = (x: Int, y: Int) => {
		// More code
		x + y
	}
```

Then use it

``` scala
	add(2, 3)
	// returns 5
```


## for arg in args ##

This is how you use `for (arg in args)` 

``` scala
	for (arg <- args)
  		println(arg)
```

Some characteristics:

- For each element in `args`, it is assigned to `arg` using `val (not `var`, so you can re-assign)
- Yeah, it is using a `<-`, which you can interpret as 'in'
- It is not a `<=` because that would mean less-than-or-equal



## Array, List, and Tuple ##

Much about immutability from Java is different in Scala.

- Array is mutable
- List is immutable
- Tuple is immutable, and can contain different types

You can read more about [Array](http://www.scala-lang.org/api/current/scala/Array.html) and [List](http://www.scala-lang.org/api/current/scala/collection/immutable/List.html). 

However, I would want to point out about Tuple, as that is never heard of in Java. In Java, when you want to return multiple objects, you will probably create a POJO (plain old java object) to contain the multiple objects. Using tuple, you can avoid POJO like classes. 

``` scala
	val color = ("blue", 258, 'b')
```

You can then access the tuple using a dot, underscore, and the one-based index of the element.

``` scala
	println(color._1)
	// Prints blue
```


## Set and Map ##

The way immutability works for Set and Map is different.

To use a mutable Set, you import the mutable HashSet.

``` scala
	import scala.collection.mutable.HashSet

	val colorSet = new HashSet[String]
	colorSet += "blue"
	colorSet += ("red", "green")
```

To use an immutable Set, you import the *immutable version*.

``` scala
	import scala.collection.immutable.HashSet

	val colorSet = HashSet[String]("blue", "red", "green")
```

Similarly for map, there is a mutable and immutable version. Let's take a look at just the mutable HashMap.

``` scala
	import scala.collection.mutable.HashMap

	val colorMap = new HashMap[Int, String]
	colorMap += 1 -> "Blue"
	colorMap += 2 -> "Red"
```

The expression `1 -> "Blue"` means `1.->("Blue")`. The method `->` is available for any object in Scala, and it returns a 2-element tuple. So basically a 2-element tuple of (Int, String) is added to colorMap.

You might think the equivalent is

``` scala
	colorMap += (1, "Blue")
```

However, that would not work as `+=` method will interpret as you want to add 2 elements - a Int and a String - where in fact you want to add a tuple (Int, String). Hence you need to add ( ) for the tuple

``` scala
	colorMap += ((1, "Blue"))
```

You can also create Map with a shorthand

``` scala
	var a = Map(1 -> "Blue", 2 -> "Red", 3 -> "Green")
```

Once again, that's same as

``` scala
	var a = Map( (1, "Blue"), (2, "Red"), (3, "Green") )
```










