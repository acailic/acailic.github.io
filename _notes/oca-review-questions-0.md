---
title: OCA  Start Question review
layout: post
tags: [java, oca]
date: 2018-07-18
---
- return type (i.e. void) and method name (i.e. main) are NEVER separated. They are always together.

- An abstract class can be extended by an abstract or a concrete class.
- Any class, whether abstract or concrete, can implement any interface.
- subclass of an abstract class can be declared abstract.
-  String is a final class and final classes cannot be extended.
- continue  cannot occur in a switch.
- All member fields (static and non-static) are initialized to their default values. Objects are initialized to null (String is also an object), numeric types to 0 (or 0.0 ) and boolean to false.

- It is not possible to access x from main without making it static. Because main is a static method and only static members are accessible from static methods. There is no 'this' available in main so none of the this.x are valid.

- catch block cannot follow a finally block!

- Interfaces are always abstract. You cannot provide a method body in an interface method unless you mark it as default (or static). You cannot use super keyword in an interface's method to invoke a method defined in its super interface.

- Interfaces cannot be final. An interface may have default methods. A method marked default is considered a non-abstract instance method. A non-abstract class that implements this interface doesn't necessarily have to implement a default method. 

- You cannot have two methods with the same signature (name and parameter types) in one class. Also, even if you put one method() method in other class which is a subclass of this class, it won't compile because you cannot override/hide a static method with a non static method and vice versa.


- Encapsulation - The key is that your data variable should be private and the functionality that is to be exposed outside should be public. Further, your setter methods should be coded such that they don't leave the data members inconsistent with each other.

- default values of uninitialized primitives and object references. booleans are initialized to false, numeric types to 0 and object references to null. Elements of arrays are initialized to the default values of their types. So, elements of a boolean array are initialized to false. int, char, float to 0 and Objects to null.
- field a is static and there will be only one copy of a no matter how many instances of Test you create. Changes made to it by one instance will be reflected in the other instance as well.

- valid identifier name because an identifier must not start with a digit (although it can contain a digit.) An identifier may start with and contain the underscore character _.
- Comparison operators have lower precedence than mathematical operators.
- == has less precedence than > .

- Java byte code is basically just a set of instructions that are intepreted by a virtual machine and is independent of the actual machine and OS i.e. the platform. JRE (Java Runtime Environment) is the virtual machine that interprets the given byte code and converts it into the acutal platform understandable instructions. Therefore, all you need to run the byte code is the virtual machine (JRE) for that specific platform on which you want to run it. 
  
  Since the byte code itself is platform independent, you can compile your java code on any platform because no matter where you compile your code, the same byte code will be produced. Therefore, you don't need a java compiler for a particular platform. You just need the JRE for that platform. Oracle provides JRE for several platforms inluding Windows and Unix.

- Object class's equals() method just checks whether the two references are pointing to the same location or not. In this case they really are pointing to the same location because of obj2 = obj1; so it returns true.

- superclass reference cannot be assigned to subclass reference without explicit cast.

- Comparison operators have lower precedence than mathematical operators.
 Therefore, 1 + 5 < 3 + 7 is evaluated as (1 + 5) < (3 + 7) i.e. 6<10, which prints true.
  Similarly,  (2 + 2) >= 2 + 3 is evaluated as  (2 + 2) >= (2 + 3) i.e. 4>=5, which prints false.
  
  If you have an expression,  2 + (2 >= 2) + 3, it would be tempting to answer 2true3, but actually, it would not compile because it would resolve to 2 + true + 3 and + operator is not overloaded for anything except String. Here, neither of the operands of + operator is a String. 
  
  
# Test Cases Facts
 
1. Local variables must be initialized before use. If you don't will get a compiler Error.

2. Overrided method cannot be more restrictive. For example:

	* interface Hastail { int getTailLength()}
	* abstract class Puma implements Hastail {
		protected int getTailLength() { return 4; }
	}

3. Parent constructor which throw an exception subclass must be declare their constructor and throws a covariant exception.

4. Printstacktrace
 ```	
	static void sum(int a,int b){System.out.println("int method invoked");}
    static void sum(long a,long b){System.out.println("long method invoked");}
```

 ```   
    public static void main( String[] args )
    {
        sum(1,1);
        sum(1l, 1l);
        Child c = new Child();

        try {
            c.checkedException();
        } catch (Exception e) {
            System.out.println();
            System.out.println(e.getClass());
            System.out.println(e.getClass().getSimpleName());
            e.printStackTrace();
        }
    }
```

    Output:
    
 ```
     int method invoked
	 long method invoked
	 java.io.IOException
		at com.java.courses.oca.inheritance.children.Child.checkedException(Child.java:43)
		at com.java.courses.oca.App.main(App.java:31)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:498)
		at com.intellij.rt.execution.application.AppMain.main(AppMain.java:147)

	* class java.io.IOException
	* IOException
```

5. Tricky switch
```
	int count = 0;
    int x = 3;
    while (count++ < 3){
        int y = (1 + 2 * count) % 3;
        out.println(y);
        switch (y){
            default:
                out.println("reached default");
            case 0: x -= 1; out.println("reached 0"); break;
            case 1: x+= 5; out.println("reached 5");
        }
        out.println(x);
    }
    out.println(x);
``` 

    Output:
    
```
    Loop: 1, Y: 0
	reached 0
	2
	Loop: 2, Y: 2
	reached default
	reached 0
	1
	Loop: 3, Y: 1
	reached 5
	6
```
6. Arrays
```
	List<Integer> l = new ArrayList<>(5);
    int [] i = new int[3];
    int [] b [] = new int[3][];
    int d = 5, e [][];
    e = new int[4][];
    out.println(d); // You can define a int and int [] in the same line.
    out.println(e); // Print the position in stack with the prefix of type. Ex: [[I@1b6d3586
    out.println(Arrays.toString(e)); // Way to convert to String for arrays
    out.println(l); // List has its own way to convert a String
```

    Output:
    
```
    5
	[[I@1b6d3586
	[null, null, null, null]
	[]
```
7. When a method is declared private it cannot be override, it can be hidden... 
   So if you use this method from the same class it would use the type method.

8. Exceptions

   Only checked exceptions are required to be handled (caught) or declared. Runtime exceptions 
   are commonly thrown by both the JVM and programmer code. Checked exceptions are usually thrown by programmer code. 
   Errors are intended to be thrown by the JVM. While a programmer could throw one, this would be a horrible practice.

10. You only can use the "default" keyword at method interfaces. In case one concrete or abstract class implements 
	two interfaces with the same sign of a "default method", this class will need to implemnts its own method.
	Regarless it is a abstract class or a concrete class.

11. If you implements two parent-child interfaces on a class, the class only will implements the child.
	Ex:
	
```

	public interface Animal {  
	  public default String getName () 
	  { return "parent";} 
	 }
	  
	public interface Mammal extends Animal{   
	  public default String getName () { return "child";}
	}
	
	public abstract class Otter implements Animal, Mammal{}
	
	public class ImplOtter extends Otter { 
	public static void main(String ... args)
	 { ImplOtter o = new ImplOtter(); out.println(o.getName());}
	 }
	 
	 
```

	Output:


```
	// Child
```
12. Interface

	- You cannot have "package private", "private", "protected" modifier.
	- You cannot combine either "abstract" with "default" and "abstract" with "static".
	- Since a class cannot inherit two interfaces that both define default methods with the same signature
	 , unless the class implementing the interfaces overrides it with an abstract or concrete method.
	- State member always have the modifier "public static final".
	- Static methods only can be invoked by its own interface.

13. Lambda Expressions

	- You can have one parameter inside parentheses without specify the type.
	- You cannot use autoboxing for primitive type.
	- The parentheses are only optional when there is one parameter and it doesn’t have a type declared. 
	
	

- Encapsulation is the technique used to package the information in such a way as to hide what should be hidden, and make visible what is intended to be visible. In simple terms, encapsulation generally means making the data variables private and providing public accessors. It helps make sure that clients have no accidental dependence on the choice of representation. It helps avoiding name clashes as internal variables are not visible outside. 

# When you do i++, what actually happens

`i = Integer.valueOf( i.intValue()  + 1);  `

As you can see, a different Integer object is assigned back to i.  
However, to save on memory, Java 'reuses' all the wrapper objects whose values fall in the following ranges:

  All Boolean values (true and false) 
  
  All Byte values 
  
  All Character values from \u0000 to \u007f (i.e. 0 to 127 in decimal) 
  
  All Short and Integer values from -128 to 127 
  
  
  So ==  will always return true when their primitive values are the same and belong to the above list of values.  Once catch, however, is that when you create a primitive wrapper using the new keyword, a new object is created and a cached object, even if available, is not used. 
  
  For example: Integer i = 10; //Wrapper created without using the new keyword and is, therefore, cached.
  
  Integer j = 10; //Cached object is reused. No new object created. 
  
  Integer k = new Integer(10); //New object is created. Cached object is not reused. 
  
  This implies that i == j is true but i == k is false. 
  
  Note that the following will not compile though: 
  
  Byte b = 1; Integer i = 1; b == i; //Invalid because both operands are of different class.

