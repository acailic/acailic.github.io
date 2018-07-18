---
title: OCA Review 1 - Java Basics
layout: post
tags: [java, oca]
date: 2018-07-18
---

## Numeric Literals

Numeric literals is a feature added in Java 7. You can have underscores in
numbers to make them easier to read. You can add underscores anywhere except at
the beginning of a literal, the end of a literal, right before a decimal, or
right after a decimal point.

{% highlight java %}
int million1 = 1000000;
int million2 = 1_000_000;
{% endhighlight %}

## Numeric Promotion

Java may do things that seem unusual to you. In numeric promotion, for example,
smaller data types, namely `byte`, `short`, and `char`, are first promoted to
`int` any time they're used with a Java binary arithmetic operator, even if
neither of the operands is `int`.

{% highlight java %}
short x = 10;
short y = 3;
short z = x * y;  // does not compile!
int z = x * y;    // ok: x and y are promoted to int
{% endhighlight %}

	Numeric Promotion Rules
	1. If two values have different data types, Java will automatically promote one of the values to the larger of the two data types.
	2. If one of the values is integral and the other is floating-point, Java will automatically
	promote the integral value to the floating-point value’s data type.
	3. Smaller data types, namely byte, short, and char, are first promoted to int any time
	they’re used with a Java binary arithmetic operator, even if neither of the operands is
	int.
	4. After all promotion has occurred and the operands have the same data type, the resulting value will have the same data type as its promoted operands.

	Numeric Literals:

	- all floating point are a doble	
	- all integer number are int.

	double d = 0D; or double d = 0d; 
	double d = 0.0;

## Optional Label Parameter

The optional label parameter allows us to break out of a higher level outer
loop. For example:

{% highlight java %}
PARENT_LOOP: for (int i = 0; i < 10; i++) {
  for (int j = 0; j < 5; j++) {
    if (i * j == 8) {
      break PARENT_LOOP;
    }
  }
}
{% endhighlight %}

## Implicit Casting in Compound Assignment Operators

Besides the simple assignment operator `=`, there're also numerous _compound
assignment operators_, e.g. `+=` and `-=`. Compound operators are useful for
more than just shorthand—they can also save us from having to explicitly cast a
value. For example, consider the following example, in which the last line will
not compile due to the result being promoted to a `long` and assigned to an
`int` variable. This could be fixed using the compound assignment operator.

{% highlight java %}
// compound assignment operator
int a = 0;
a += 1;

// implicit casting
long x = 10;
int y = 5;
y = y * x; // does not compile!
y *= x;    // ok
{% endhighlight %}

## Precedence of Importing

Given the following classes, which of the following snippets can be inserted in
place of `INSERT IMPORTS HERE` and have the code compile? (Choose all that
apply)

{% highlight java %}
package aquarium;
public class Water {
  boolean salty = false;
}

package aquarium.jellies;
public class Water {
  boolea salty = true;
}

package employee;
// INSERT IMPORTS HERE
public class WaterFiller {
  Water water;
}
{% endhighlight %}

You cannot import by classname two same classes

1. `import aquarium.*;`
2. `import aquarium.Water; import aquarium.jellies.*;`
3. `import aquarium.*; import aquarium.jellies.Water;`
4. `import aquarium.*; import aquarium.jellies.*;`
5. `import aquarium.Water; import aquarium.jellies.Water;`
6. None of these imports can make the code compile.

The answer is 123. Option 1 is correct because it imports all the classes in the
`aquarium` package including `aquarium.Water`. Option 2 and 3 are correct
because they import `Water` by classname. Since __importing by classname takes
precedence over wildcards__, these compile.


## QUESTIONS

Local variables don't get assigned default values. 
	The code fails to compile if a local variable is not explicity initialized and it's trying to use.

Java is completely CASE SENSITIVE that's include PACKAGE NAMES.

Values prefixex for number:

	0b ---> Binary Prefix
	0x ---> Hexadecimal Prefix
	0 for octal

Calling System.gc() has no effect on eligibility for garbage collection.

	Calling System.gc() suggests that Java might wish to run the garbage collector.
	Java is free to ignore the request. finalize() runs if an object
	attempts to be garbage collected.

Java doesn't has operator overloading and pointers. 

All of the arithmetic operators may be applied to any Java primitives, except boolean
	and String. Furthermore, only the addition operators + and += may be applied to String
	values, which results in String concatenation.
	
Valid Java identifier can contain "$","[A-Za-z]","_" and [0-9]. But not begin with [0-9].

	Also you can have identifier like this "Public" because Java is case sensitive. You can have wathever reserved word you want using case sensitive.
	

public
static void main(String[] args). Arguments are referenced starting with args[0]. Accessing
an argument that wasn’t passed in will cause the code to throw an exception.

Order:
* PACKAGE 
* IMPORT
* CLASS

Importing with Wildcards do not look at subdirectories. Class names imports take prededence. 

