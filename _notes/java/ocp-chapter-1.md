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

### Advanced Class Design

- [Anonymous Inner Class](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/AnonymousInnerClass.java) <br />
- [Hash Code](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/HashCode.java) <br />
- [Local Inner Class](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/LocalInnerClass.java) <br />
- [Member Inner Class](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/MemberInnerClass.java) <br />
- [Polymorphism](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/Polymorphism.java) <br />
- [Static Nested Class](https://github.com/acailic/java8-learning/blob/master/Java-8/src/advancedClassDesign/StaticNestedClass.java)

### Questions

- if two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result. In this case, equals() will return true for: new Info("aa", "b", "c") and new Info("a", "ab", "c") but their hashCode() values will be different.

- From outer can see inner class:

`class OuterWorld {   public InnerPeace i = new InnerPeace();   private class InnerPeace   {      private String reason = "none";   }   void m(){     System.out.println(i.reason);   }    }`

- The static block will be executed only once when the class is loaded. A class is loaded when it is first referenced. In this case, it is first referenced by the JVM when it tries to run the main() method of this class.

-  aggregate features from multiple classes OOP: ggregate features of another class, you either extend that class (i.e. inheritance) or have an object of the other class in your class (i.e. composition). 
    1.Inheritance
    2.Composition

- public String toString() - method that returns a String representation of the object. This means all classes have this method. However, this String contains just the name of the class and its hashcode. It doesn't contain information about the attributes of the class and is therefore not meaningful for human reading. Therefore, if you want to include the value of the attributes of a class in its String representation, you should override toString method of that class.   
- Composition is a good way to reuse functionality from multiple class because it does not require your class to extend from any other class.  
- Composition is implemented simply by forwarding a call to another object. The calling class has no dependence on implementation details of the called object. This is very useful when your class already extends another class.  Composition and Inheritance both are techniques for code reuse.


-  Singleton pattern: The class has a public static method that returns an instance of that class. The class has a private class variable that refers to an instance of the same class.

- The Singleton patterns enforces that only one object of the class is ever created. To achieve this, 
you need to do 3 things: 
 1. Make the constructor of the class private so that no one can instantiate it except this class itself. 
 2. Add a private static variable of the same class to the class and instantiate it.
 3. Add a public static method (usually named getInstance()), that returns the class member created in step 2. 
 
- rule to remember is: If the equals() method returns true, the hashCode() of the two objects MUST be the same. The reverse is desirable but not necessary.  Further, equals method must follow these rules: It should be reflexive: for any reference value x, x.equals(x) should return true. It should be symmetric: for any reference values x and y, x.equals(y) should return true if and only if y.equals(x) returns true. It should be transitive: for any reference values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true. It should be consistent: for any reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the object is modified. For any non-null reference value x, x.equals(null) should return false. 

- It can throw any exception as long as it is a subclass of any of the exceptions thrown by the overridden method. It can also be a subclass of RuntimeException. eg. If the base class A has: void m1() throws IOException then overriding method in class B can be : void m1() throws FileNotFoundException because FileNotFoundException is a subclass of IOException.
- Note that print() is declared private in class A. So a.print() is not valid in class B's code as the class of 'a' is A. Now, you may think that the actual object is of class B and method to be executed is selected according to the actual object. While that is true (except in case of private instance methods, which are bound at compile time anyway), access rights are checked at compile time. At compile time, the compiler has no knowledge of the actual object and so access rights are checked against the class of the variable.

-  1. The signature of the standard equals method in Object class is public boolean equals(Object ). So you cannot override that method with anything which is not public. In this case, however, Resource class does not even have equals(Object ) method. So it is not overriding Object class's equals method. It is overloading it  and is thus free to make it private, protected, or default access.  2. The parameter type of Object class's equals method is Object, which means that you can pass any object as an argument.  3. A method with default access is accessible only to classes within that package.  4. When multiple methods of a class are applicable to be selected, the most specific method is chosen.

- 1. The + operator is overloaded for Strings and when it sees a String on one side and another object on the other side, it calls toString() method on the other object to get a String representation of that object. (If the other object is null, it doesn't call toString on null and assumes "null" as the value.) 2. Object class has a public toString() method that returns a String representation of the object. This means all classes have this method. However, this String contains just the name of the class and its hashcode. It doesn't contain information about the attributes of the class. Therefore, if you want to include the value of the attributes of a class in its String representation, you should override toString method of that class.


-  illustrates the fundamental aspect of overriding, which is that it is the actual class of object and not the class of the reference type that determines which instance method will be invoked. Here, actual class of the object pointed to by w is GoodWidget and so GoodWidget's doWidgetStuff will be invoked. This method does nothing and so nothing is printed.  Notice that the explicit cast to Widget has no impact because the class of the reference is not considered while invoking the instance methods at all. But if you try to access the field (or a static method) directly, the class of the reference is used. Therefore,  
        System.out.println(w.data); //prints data   
      System.out.println(((GoodWidget)w).data); //prints big data

-  here is that an overriding method cannot make the overridden method more private. It can make the method more public though.  The access hierarchy in increasing levels of accessibility is: private->'no modifier'->protected->public ( public is accessible to all and private is accessible to none except itself.)  Here, class OverridingSaloon has no modifier for m1() so it is trying to reduce the accessibility of protected to default. 'protected' means the method will be accessible to all the classes in the same package and all the subclasses (even if the subclass is in a different package). No modifier (which is the default level) means the method will be accessible only to all the classes in the same package. (i.e. not even to the subclass if the subclass is in a different package.)


- An interface can redeclare a default method and also make it abstract. An interface can redeclare a default method and provide a different implementation.

- non-static variable s cannot be referenced from a static context. 's' is not accessible from the inner class. This is because the inner class is created in a static method so it does not have any reference to TestFrame 
`... 
 String s="Message";   
  public static void main(String args[])  
    {       TestFrame t = new TestFrame();    
       Button b = new Button("press me");      
        b.addActionListener(new ActionListener()  {        
                      public void actionPerformed(ActionEvent e)        
                                    {  System.out.println("Message is " +s); }...`
                                    
                                    
- When interfaces are involved, more than one method declaration may be overridden by a single overriding declaration. In this case, the overriding declaration must have a throws clause that is compatible with ALL the overridden declarations.

- When interfaces are involved, more than one method declaration may be implemented by a single method declaration. In this case, the overriding declaration must have a throws clause that is compatible with ALL the overridden declarations. The declaration public void m1(){} satisfies both the declarations. 

- Three concepts : 

1. A Set (such as a TreeSet or HashSet) does not allow duplicate elements. If you add a duplicate element, it is ignored. Thus, only three unique SIZE elements are stored.

It is important to understand how the add() method of a Set works :
boolean add(E o)
	Adds the specified element to this set if it is not already present (optional operation). More formally, adds the specified element, o, to this set if this set contains no element e such that (o==null ? e==null : o.equals(e)). If this set already contains the specified element, the call leaves this set unchanged and returns false. In combination with the restriction on constructors, this ensures that sets never contain duplicate elements.

2. TreeSet sorts the elements in their natural order. So while retrieving elements, it returns them in the sorted order.

3. The natural order of enums is the order in which they are defined. It is not necessarily same as alphabetical order of their names.


- facts about enums: 
1. Enum constructor is always private. You cannot make it public or protected. If an enum type has no constructor declarations, then a private constructor that takes no parameters is automatically provided.
2. An enum is implicitly final, which means you cannot extend it. 
3. You cannot extend an enum from another enum or class because an enum implicitly extends java.lang.Enum. But an enum can implement interfaces. 
4. Since enum maintains exactly one instance of its constants, you cannot clone it. You cannot even override the clone method in an enum because java.lang.Enum makes it final. 
5. Compiler provides an enum with two public static methods automatically -  values() and valueOf(String). The values method returns an array of its constants and valueOf method tries to match the String argument exactly (i.e. case sensitive) with an enum constant and returns that constant if successful otherwise it throws java.lang.IllegalArgumentException.  

- The following are a few more important facts about java.lang.Enum which you should know:  
1. It implements java.lang.Comparable (thus, an enum can be added to sorted collections such as SortedSet, TreeSet, and TreeMap). 
2. It has a method ordinal(), which returns the index (starting with 0) of that constant i.e. the position of that constant in its enum declaration.
3. It has a method name(), which returns the name of this enum constant, exactly as declared in its enum declaration.


- following scenarios will you need to create an abstract class : You want to define common method signatures in the class but force subclasses to provide implementations for such methods.



- Unlike a regular java class, you cannot access a non-final static field from an enum's constructor.
- Here, CAT is actually an instance of an anonymous subclass of Pets. This subclass is overriding getData() method:
    `...
    public enum Pets {    DOG(1, "D"),    CAT(2, "C")    {     
        public String getData(){ return type+name; }    }, 
       FISH(3, "F");    int type;    String name;    
       Pets(int t, String s) { this.name = s; this.type = t;}   
     public String getData(){ return name+type; } 
    }
    `
- If the inner class is non static, 
all the static and non-static members of the outer class are accessible (otherwise only static are accessible). 
Prior to java 8, only final local variables were accessible to the inner class but in Java 8, 
even effectively final local variables of the method are accessible to the inner defined in that method as well.


- All keys in a map are unique.
  