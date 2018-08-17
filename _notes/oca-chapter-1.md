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
	
	
	
- This is because the information was lost during the conversion from type int to type float as values of type float are not precise to nine significant digits. Note: You are not required to know the number of significant digits that can be stored by a float for the exam. However, it is good to know about loss of precision while using float and double.

 `int i = 1234567890;`
 `float f = i;`  
 `System.out.println(i - (int)f);`


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
 
- Access to static and instance fields and static methods depends on the class of reference variable and not the actual object to which the variable points to. Observe that this is opposite of what happens in the case of instance methods.  In case of instance methods the method of the actual class of the object is called.
```
MNOP extends ABCD
ABCD a = new MNOP();  
  System.out.println(a.x);  
  System.out.println(a.y);
accessed because a is of type ABCD
```


- instance member can be a variable, a constant or a method. It belongs to an instance of the class.  All non-static members are instance members.

- variables are SHADOWED and methods are OVERRIDDEN. Which variable will be used depends on the class that the variable is declared of. Which method will be used depends on the actual class of the object that is referenced by the variable.

- The local 'x' simply shadows the member variable 'x'.

```
 static int x = 5; 
    public static void main(String[] args){    
      int x  = ( x=3 ) * 4;  // 1  
           System.out.println(x);   
     }
```

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

- Java does not allow chained initialization in declaration.  Chaining to use a value of a variable at the time of declaration is not allowed. If already declared, it would have been valid.  


- int [].getClass().isArray() will return true. 

- new int[2][4];  it will instantiate both the dimensions of the array and not throw null pointer exception accessing each value;

- Java does not allow chained initialization in declaration 
```
Chaining to use a value of a variable at the time of declaration is not allowed. 
Had b and c been already declared, it would have been valid.
 For example, the following is valid: 
   int  b = 0, c = 0;  
  int a = b = c = 100; 
 Even the following is valid:  
  int  b , c;  //Not initializing b and c here.  
  int a = b = c = 100; //declaring a and initializing c, b, and a at the same time.
 
  Notice the order of initialization of the variables - c is initialized first, b is initialized next by assigning to it the value of c. Finally, a is initialized.
```

- two integral primitives can be compared using == operator.

- s instanceof Short : left operand of instanceof MUST be an object and not a primitive.  can NEVER refer to an object of class java.util.Date. So, it will not accept this code.
 --At run time, f points to an object of class Eagle. Now, Eagle extends Bird so f instanceof Bird returns true. e points to an object of class Eagle. Eagle extends Bird, which in turn implements Flyer. So an Eagle is a Flyer. Therefore, e instanceof Flyer will also return true.  b points to an object of class Bat, which does not extend from Bird. Therefore, b instanceof Flyer returns false.
 
 
 - When the program is run, the JVM looks for a method named main() which takes an array of Strings as input and returns nothing (i.e. the return type is void). But in this case, it doesn't find such a method ( the given main() method is returning long!) so it throws a java.lang.NoSuchMethodError. Note that java.lang.Error does not extend Exception class. It  extends java.lang.Throwable and so it can be "thrown"
 
 
 - defaulted imported package : java.lang.(System and String class in this package.). If there is no package statement in the source file, the class is assumed to be created in a default package that has no name. In this case, all the types created in this default package will be available to this class without any import statement.  However, note that this default package cannot be imported in classes that belong to any other package at all, not even with any sort of import statement. So for example, if you have a class named SomeClass in package test, you cannot access TestClass defined in the problem statement (as it is defined in the default package) at all because there is no way to import it.
 
 
 -  1.0 and 43e1 can fit into a float, implicit narrowing does not happen because implicit narrowing is permitted only among byte, char, short, and int.
 
 ```
 float f1 = 1.0; X 
 float f = 43e1; X
 ```
 
 - integral types means byte, short, int, long, and char. The integral types are byte, short, int, and long, whose values are 8-bit, 16-bit, 32-bit and 64-bit signed two's-complement integers, respectively,
  and char, whose values are 16-bit unsigned integers representing UTF-16 code units. unlike &&, & will not "short circuit" the expression if used on boolean parameters.  
 
 - points about Boolean: 
 
  1. Boolean class has two constructors - Boolean(String) and Boolean(boolean) The String constructor allocates a Boolean object representing the value true if the string argument is not null and is equal, ignoring case, to the string "true". Otherwise, allocate a Boolean object representing the value false. Examples: new Boolean("True") produces a Boolean object that represents true. new Boolean("yes") produces a Boolean object that represents false.  The boolean constructor is self explanatory.
   2. Boolean class has two static helper methods for creating booleans - parseBoolean and valueOf. Boolean.parseBoolean(String ) method returns a primitive boolean and not a Boolean object (Note - Same is with the case with other parseXXX methods such as Integer.parseInt - they return primitives and not objects). The boolean returned represents the value true if the string argument is not null and is equal, ignoring case, to the string "true".  Boolean.valueOf(String ) and its overloaded Boolean.valueOf(boolean ) version, on the other hand, work similarly but return a reference to either Boolean.TRUE or Boolean.FALSE wrapper objects. Observe that they dont create a new Boolean object but just return the static constants TRUE or FALSE defined in Boolean class.
   3. When you use the equality operator ( == ) with booleans, if exactly one of the operands is a Boolean wrapper, it is first unboxed into a boolean primitive and then the two are compared (JLS 15.21.2). If both are Boolean wrappers, then their references are compared just like in the case of other objects. Thus, new Boolean("true") == new Boolean("true") is false, but new Boolean("true") == Boolean.parseBoolean("true") is true.
   
   
- boolean operators have more precedence than =.  
- The arithmetic operators *, / and % have the same level of precedence.