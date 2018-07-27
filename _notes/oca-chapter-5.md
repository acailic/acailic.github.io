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


-When class A extends or implements B directly or indirectly, you can say that A is-a B. Here, Car directly extends Vehicle and directly implements Drivable. Therefore, a Car is-a Vehicle and a Car is-a Drivable.
 Similarly, an SUV is-a Car and since Car is-a Vehicle and is-a Drivable, SUV is also a Vehicle and a Drivable.
 -if you have a container that is meant to contain A, then you can add anything that is-a A. Cant put parent on place of child.