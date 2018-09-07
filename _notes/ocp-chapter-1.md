---
title: OCP Review 1 - Advanced Class Design
layout: post
tags: [java, ocp, class]
date: 2018-09-07
---

## Advanced Class Design

**Abstract classes:**

- An abstract class must not necessarily define abstract methods. But if it
  defines even one abstract method, it _must_ be marked as an abstract class.
- An abstract class cannot be instantiated.
- An abstract class forces all its non-abstract derived classes to implement the
  incomplete functionality in their own unique manner.
- If an abstract class is extended by another abstract class, the derived
  abstract class might _not_ implement the abstract methods of its base class.
- If an abstract class is extended by a concrete class, the derived class _must_
  implement all the abstract methods of its base class, or it won't compile.
- A derived class _must_ call its super class's constructor (implictly or
  explicitly), irrespective of whether the super class or derived class is an
  abstract class or concrete class.
- An abstract class cannot define abstract static methods. Because static
  methods belong to a class and not to an object, they aren't inherited. A
  method that cannot be inherited cannot be implemented. Hence this combination
  is invalid.
- A static variable defined in a base class is accessible to its derived class.
  Even though derived class does not define the static variable, it can access
  the static variable _defined in its base class_.

**Non-access modifier—static**:

- Static members are also known as _class fields_ or _class methods_ because
  they are said to belong to their class, and not to any instance of that class.
- A static variable and method cannot access non-static variables and methods of
  a class. But the reverse works: non-static variables and methods can access
  static variables and methods.
- When you instantiate a derived class,  the derived class instantiates its base
  class. The static initializers execute when a class is loaded in _memory_, so
  the order of execution of static and instance initializer blocks is as
  follow: base class static init-block, derived class static init-block, base
  class instance init-block, derived class instance init-block.

**Non-access modifier—final**:

- An instance final variable can be initialized either with its declaration in
  the initilizer block or in the class's constructor.
- A static final variable can be initialized either with its declaration or in
  the class's static initializer block.
- If you don't initialize a final local variable in a method, the compiler won't
  complain, as long as you don't use it.
- The Java compiler considers initialization of a final variable complete _only_
  if the initialization code will execute in _all_ conditions. If the Java
  compiler cannot be sure of execution of code that assigns a value to your
  final variabl, it will complain (code won't compile) that you haven't
  initialized a final variable. If an `if` construct uses constant values, the
  Java compiler can predetermine whether the `then` or `else` blocks will
  execute. In this case, it can predetermine whether these blocks of code will
  execute to initialize a final variable.
- The initialization of a final veriable defined in an abstract base class must
  complete in the class itself. It _connot_ be deferred to its derived class.

**Enumerated types:**

- All enums extend the abstract class `java.lang.Enum`, defined in Java API.
- Because a class can extend from only base class, an attempt to make your enum
  extend any other class will fail its compilation.
- The enum constants are _implicit_ static members.
- The creation of enum constants happens in a static initializer block, before
  the execution of the rest of the code defined in the `static` block.
- You can define multiple constructors in your enums.
- Enum constants can define new methods, but these methods cannot be called on
  the enum constant.
- All the constants of an enum type can be obtained by calling the _implicit_
  `public static T[] values()` method of that type. This array contains the
  enumerated types in natural order. (Probably defined by the private member
  `ordinal` of `java.lang.Enum`)
- You can define an enum as a top-level enum or within another class or
  interface.<sup>[2.1]</sup>
- You cannot define an enum local to a method.<sup>[2.2]</sup>

<sup>[2.1],[2.2]</sup> Example:

{% highlight java %}
// Top-level
enum Size { S, M, L }

// Member of a class
class MyClass {
  enum Size { S, M, L }
}

// Member of an interface
interface MyInterface {
  enum Size { S, M, L }
}

// WON'T compile: inside a method
class MyClass {
  void aMethod() {
    enum Size { S, M, L }
  }
}
{% endhighlight java %}

**Static nested class:**

- This class is not associated with any object of its outer class. Nested within
  its outer class, it's accessed like any other static member of a class—by
  using the class name of the outer class.
- The accessibility of the nested static class depends on its access modifier.
  For example, a private static nested class cannot be accessed outside its
  class.

**Inner class:**

- An object of an _inner_ class shares a special bound with its _outer class_
  and cannot exist without its instance.
- An inner class cannot define _static methods_ and non-final static variables.
- You can create an object of an inner class within an outer class<sup>[4]</sup>
  or outside an outer class<sup>[5]</sup>.

<sup>[4]</sup> Inside the outer class, an inner class is instantiated like:

{% highlight java %}
Inner inner = new Inner();
{% endhighlight java %}

<sup>[5]</sup> Outside the outer class, an inner class is instantiated like:

{% highlight java %}
Outer.Inner inner = new Outer().new Inner();
{% endhighlight java %}

Here's a complete example:

{% highlight java %}
public class Outer {

  public void print() {
    System.out.println("Outer: " + new Inner());
  }

  public class Inner {
    @Override
    public String toString() {
      return "Inner";
    }
  }
}
{% endhighlight java %}

{% highlight java %}
public class App {

  public static void main(String... args) {
    Outer.Inner inner = new Outer().new Inner();
    System.out.println("Outside: " + inner);
    Outer outer = new Outer();
    outer.print();
  }
}
{% endhighlight java %}

Compile and execute:

```
$ javac Outer.java App.java
$ java App
Outside: Inner
Outer: Inner
```

**Anonymous inner classes:**

- An anonymous inner class is created when you combine object instance creation
  with inheriting a class or implementing an interface.
- The following line creates an anonymous inner class that extends `Object` and
  assigns it to a reference variable of type `Object`:

      Object o = new Object(){};

- When an anonymous inner class is defined within a method, it can access only
  the final variables of the method in which it's defined. This is to prevent
  reassignment of the variable values by the inner class.
- Since Java 8, certain variables that are not declared `final` are instead
  considered _effectively_ final. See [JLS 4.12.4. final Variables][jls-4.12.4]
  for more detail.

**Method local inner classes:**

- Method local inner classes are defined within a static or instance method of a
  class.
- A method local inner class can define its own constructors, variables, and
  methods by using any of the four access levels—`public`, `protected`, default,
  and `private`.
- A method local inner class can access all variables and methods of its _outer_
  class, including its private members and the ones that it inherits from its
  base classes. It can only access the _final_ local variables of the method in
  which it's defined.

**Sample questions correction:**

Question 2-13: What is the output of the following code?

{% highlight java %}
enum BasicColor {
  RED;
  static {
    System.out.println("Static init");
  }
  {
    System.out.println("Init block");
  }
  BasicColor() {
    System.out.println("Constructor");
  }
  public static void main(String... args) {
    BasicColor red = BasicColor.RED;
  }
}
{% endhighlight %}

Here's part of the decompiled code. Note that the contents of the default
constructor and instance initializer blocks are added to the new constructor,
implicitly defined during the compilation process.

```
final class BasicColor extends Enum
{
  private BasicColor(String s, int i)
  {
    super(s, i);
    System.out.println("Init block");
    System.out.println("Constructor");
  }

  ...

  static
  {
    RED = new BasicColor("RED", 0);
    $VALUES = (new BasicColor[] {
      RED
    });
    System.out.println("Static init");
  }
}
```

Thus the answer is:

```
Init block
Constructor
Static init
```
