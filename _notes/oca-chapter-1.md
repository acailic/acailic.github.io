---
title: OCA Review 1 - Java Basics
layout: post
tags: [java, oca]
date: 2018-07-18
---

## Java Basics

- Check access modifier
- Check non-access modifier
- Check return type
- Check method type (constructor, getter / setter)
- Check parameter types

{% highlight java %}
public static void main(String[] args) { ... }
// ^   ^      ^    ^    ^
// |   |      |    |    +- parameter type(s)
// |   |      |    +- method type
// |   |      +- return type
// |   +- non-access modifier
// +- access modifier
{% endhighlight %}


- to run a class from the command line, you need a main(String[] ) method that takes an array of Strings array not just a String( Also String ... can be sent).

- args array is never null. Run without any arguments, args points to a String array of length 0. 



**Java Packages**

- An `import` statement allows to import a class or interface.
- An `import` statement allows to import a package using wildcard `*`.
- An `import static` statement allows to import a static method.
- An `import static` statement allows to import all the static methods of the
target class using wildcard `*`.
- If a `package` statement is present, it must be the first non-commented line
  of code in the file.

- If there is no package, default package will be created.   java.lang is default import(inside String and System).


**Compilation**

- If a variable declaration fails to compile, then all the following lines
  which use this variable will fail to compile, because variable is undefined.
- A local variable must be initialized before being used. Otherwise, Java fails
  to compile at those lines where this variable is used.
- A local variable is not required to be initialized if it is not used.

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

package and import are optional.

Import by name has precedence.

Importing with Wildcards do not look at subdirectories. Class names imports take prededence.
 
 Object is eligible for garbage collection when is all references are nulled.
 
-Access to static and instance fields and static methods depends on the class of reference variable and not the actual object to which the variable points to. Observe that this is opposite of what happens in the case of instance methods.  In case of instance methods the method of the actual class of the object is called.
```
MNOP extends ABCD
ABCD a = new MNOP();  
  System.out.println(a.x);  
  System.out.println(a.y);
accessed because a is of type ABCD
```


- variables are SHADOWED and methods are OVERRIDDEN. Which variable will be used depends on the class that the variable is declared of. Which method will be used depends on the actual class of the object that is referenced by the variable.

- boolean operators have more precedence than =.

```
(b2 != b1 = !b2)  
first b2 != b1 is evaluated which returns a value 'false'.
 So the expression becomes false = !b2. And this is illegal because false is a value and not a variable!
```

-JVM looks for a method named main() which takes an array of Strings as input and returns nothing (i.e. the return type is void).  it doesn't find such a method java.lang.NoSuchMethodError.

-java.lang.Error does not extend Exception class. It  extends java.lang.Throwable and so it can be "thrown".

Keyword | Type
------------ | -------------
boolean | true or false
byte | 8-bit integral
short | 16-bit integral
int | 32-bit integral
long | 64-bit integral
float | 32-bit floating point 123.45f
double | 64-bit floating point
char | 16-bit unicode value

- implicit narrowing does not happen because implicit narrowing is permitted only among byte, char, short, and int.