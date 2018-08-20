---
title: OCA Review 5 - Class design
layout: post
tags: [java, oca, class]
date: 2018-07-24
---


## Inheritance

**Interface**

- All interface variables are implicitly assumed `public static final`. For
  these variables, they mush declare a value when they are initialized.
  Otherwise, the compilation fails.

- Interface methods are assumed to be `public` and `abstract` even if they're
  not written as modifier. Therefore, a class implementing the methods of an
  interface must follow the same accessibility: `public`.
- Interface method cannot be marked `static` or `default` if it is already
  marked `abstract`.
- A `static` method in an interface can't be called using a reference variable.
  It must be called using the interface name.
- Unlike an interface, if you define a `static` method in a base class, it can
  be accessed using either a reference variable or the class name.


- Although you know that o will refer to an object that is a Runnable at runtime, the compiler doesn't know about it. That is why, you have to do: Runnable r = (Runnable) o; You can assign a subclass object reference to superclass reference without a cast but to assign a super class object reference to a subclass (or interface) reference you need an explicit cast as in option 2


 
Interfaces

The methods of an interface are implicitly abstract and public.
The vairables of an interface are implicitly public, static, and final.
Because the methods in an interface are implicitly public, if you try to assign a weaker access to the implemented method in a class, it won't compile. Interface can have a static method but the method must have a body.

- Inherited interface  can redeclare a default method and also make it abstract. Also can redeclare a default method and provide a different implementation.  static methods can never be abstract (neither in an interface not in a class).

# Casting class
```
AA extends A 
A a; AA aa; 
a = (AA)aa;
aa = (AA) a; //potential classcastexception
a = new AA();
((AA)a).doStuff(); //potential classcastexception
```
a is declared as a reference of class A and therefore, at run time, it is possible for a to point to an object of class AA (because A is a super class of AA). Hence, the compiler will not complain. Although if a does not point to an object of class AA at run time, a ClassCastException will be thrown.

((AA)a).doStuff();
Once you cast a to AA, you can call methods defined in AA. Of course, if a does not point to an object of class AA at runtime, a ClassCastException will be thrown. In this particular case, a NullPointerException will be thrown because a points to null and a null can be cast to any class.

- superclass reference cannot be assigned to subclass reference without explicit cast.

- For example, consider two classes: Automobile and Car, where Car extends Automobile Now, Automobile a = new Car(); is valid because a car is definitely an Automobile. So it does not need an explicit cast.


## QUESTIONS

Test Cases Facts


1. All interfaces methods are implicity public. Since Java 8 now includes default and static
   methods and they are never abstract, you cannot assume the abstract modifier will be
   implicitly applied to all methods by the compiler.

2. Overriding rules:

	- the rules for overriding a method allow a subclass to define a method with an 
	exception that is a subclass of the exception in the parent method.
	- For nonprivate methods in the parent class, both methods must use static
	 (hide) or neither should use static (override).
	-  If instance variable is hide depends where the methods belongs which class it will be use the his own instance variable
	from its class.
	
```	
class Game{
  public void play() throws Exception{
    System.out.println("Playing...");
  }
}

public class Soccer extends Game{
   public void play(){
      System.out.println("Playing Soccer...");      
   }
   public static void main(String[] args){
       Game g = new Soccer();
       g.play();
   }
}	

```
- It will not compile. overrides the play() method without any throws clause. This is valid because a list of no exception is a valid subset of a list of exceptions thrown by the superclass method. even though the actual object referred to by 'g' is of class Soccer, the class of the variable g is of class Game. Therefore, at compile time, compiler assumes that g.play() might throw an exception, because Game's play method declares it.
  
3. Covariant Return Type
	
	Since Java5, it is possible to override method by changing the return type if subclass overrides any method 
	whose return type is Non-Primitive but it changes its return type to subclass type.

4. Which can only can be hidden and not overriden:

	- private instance methods.
	- static methods
	- public variables
	- private variables

	Variables may only be hidden, regardless of the access modifier.
    
    With hidden methods, the specific method used depends on where is it referenced.
    
    
    
 - An overriding method can be made less restrictive than the overridden method. The restrictiveness of access modifiers is as follows:
     private>default>protected>public (where private is most restrictive and public is least restrictive).
 there is no modifier named default. The absence of any access modifiers implies default access.
 
 
5. Polymorphic

	- A reference to an object requires an explicit cast if referenced with a subclass.
	If the cast is to a superclass reference, then an explicit cast is not required.
	- Because of polymorphic parameters, if a method takes the superclass of
	an object as a parameter, then any subclass references may be used without a cast
	- All objects extend java.lang.Object, so if a method takes that
	type, any valid object, including null, may be passed.

6. Interfaces and Abstract classes

	- Only interfaces can contain default methods
	- Both abstract classes and interfaces can contain static methods
	- Both structures require a concrete subclass to be instantiated
	- Though an instance of an object that implements an interface inherits java.lang.
	Object, the interface itself doesn’t; otherwise, Java would support multiple inheritance
	for objects, which it doesn’t.
	- Interface variables are assumed to be public static final

7. Constructor:

	1. The first statement of every constructor is a call to another constructor within the class
	using this(), or a call to a constructor in the direct parent class using super().
	2. The super() call may not be used after the first statement of the constructor.
	3. If no super() call is declared in a constructor, Java will insert a no-argument super()
	as the first statement of the constructor.
	4. If the parent doesn’t have a no-argument constructor and the child doesn’t define any
	constructors, the compiler will throw an error and try to insert a default no-argument
	constructor into the child class.
	5. If the parent doesn’t have a no-argument constructor, the compiler requires an explicit
	call to a parent constructor in each child constructor.

    6. It is possible that constructor of super class is not visible in subclass - > NO COMPILE
    
- The default constructor is provided by the compiler only when a class does not define ANY constructor explicitly. For example,
  public class A{
    public A()  //This constructor is automatically inserted by the compiler because there is no other constructor defined by the programmer explicitly.
    {
      super();  //Note that it calls the super class's default no-args constructor.
    }
  }
  public class A{
    //Compiler will not generate any constructor because the programmer has defined a constructor.
    public A(int i){
       //do something
    }
  }


- When class A extends or implements B directly or indirectly, you can say that A is-a B. Here, Car directly extends Vehicle and directly implements Drivable. Therefore, a Car is-a Vehicle and a Car is-a Drivable.
 Similarly, an SUV is-a Car and since Car is-a Vehicle and is-a Drivable, SUV is also a Vehicle and a Drivable.
 -if you have a container that is meant to contain A, then you can add anything that is-a A. Cant put parent on place of child.
 
 - A class is uninstantiable if the class is declared abstract. If a method has been declared as abstract, it cannot provide an implementation (i.e. it cannot have a method body ) and the class containing that method must be declared abstract). If a method is not declared abstract, it must provide a method body (the class can be abstract but not necessarily so). If any method in a class is declared abstract, then the whole class must be declared abstract. An class can still be made abstract even if it has no abstract method.
 
 
 - If a subclass does not have any declared constructors, the implicit default constructor of the subclass will have a call to super( ). You can either call super(<appropriate list of arguments>) or this(<appropriate list of arguments>) but not both from a constructor.
 
 - calling super(); will not always work because if the super class has defined a constructor with arguments and has not defined a no args constructor then no args constructor will not be provided by the compiler. It is provided only to the class that does not define ANY constructor explicitly.
 
 
 
 - Note that method print() is overridden in class B. Due to polymorphism, the method to be executed is selected depending on the class of the actual object. Here, when an object of class B is created, first A's constructor is called, which in turn calls print(). Now, since the class of actual object is B, B's print() is selected. At this point of time, variable i has not been initialized (because we are still initializing A at this point), so its default value i.e. 0 is printed. This happens because the method print() is non-private, hence polymorphic. 
 
 
 - variables and static methods are not overridden and so access to variables and static methods is determined at compile time based on the type of the variable (instead of type of the object referred to by the variable, as is the case with instance methods.) In the given code, if you declare b to be of type B i.e. B b = new B();, you can access b.i.
 
 - override a static method with a non-static method (and vice-versa) in a class will result in a compilation error. Even in case of interfaces, a subinterface not override a default method with a static method.
 
 - If interface A has default method, and other interface  B extends it, it will must provide an implementation of this method or be marked as abstract. Interfaces are always abstract. Cannot provide a method body in an interface method unless you mark it as default (or static). Cannot use super keyword in an interface's method to invoke a method defined in its super interface.
 
 - A class (or an interface) can invoke a default method of an interface that is explicitly mentioned in the class's implements clause (or the interface's extends clause) by using the same syntax i.e. <InterfaceName>.super.<methodName>.