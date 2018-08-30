---
title: OCA Review 4 - Methods and encapsulation
layout: post
tags: [java, oca]
date: 2018-08-26
---
## METHODS 

**Method**

- The access modifier of an inherited method cannot be more restrictive than
  its parent.
  
- Overloaded method means the method name is the same and the method parameter
  list is different. Anything else is allowed to vary. Remember that Java is
  case sensitive.
- Overloaded methods may or may not define a different return type.
- Overloaded methods may or may not define different access levels.
- Return type is not part of a method signature.
- If you try to execute a method using values that can be passed to multiple
  overloaded methods, in this case, the code will fail to compile.
- Java prefers the most specific overloaded signature it can find.
- Java prefers a single object over a vararg parameter.
- Java prefers an autoboxed parameter over a vararg parameter.
- The `return` statement need not be the last statement in a method, but it
  must be the last statement to execute in a method.
- If the compiler determines that a `return` statement isn't the last
  statement to _execute_ in a method, the method will fail to compile.
- If the return type of a method is `int`, the method can return a value of
  type `byte`.
- If a `char` variable gets expanded among a list of overloaded methods, it
  will be expanded to its nearest type, e.g. `int` instead of `long`.
- If the return type of a method is `int`, the method can return a value of
  type `byte`.
 
 **Method overriding and virtual method invocation:**
 
 - Whenever you intend to override methods in a derived class, use the
   annotation `@Override`. It will warn you if a method cannot be overridden or
   if you're actually overloading a method rather than overriding it.
 - Overridden methods can define the same or covariant return types.
   <sup>[1.2]</sup>
 - A derived class cannot override a base class method to make it _less
   accessible_.
 - Static methods cannot be overridden. They're not polymorphic and they are
   bound at compile time.
 - The `instanceof` operator must be followed by the name of an interface,
   class, or enum.
 - In a derived class, a static method with the same signature as that of a
   static method in its base class hides the base class method.
   
 - _Constructor cannot be overridden_ because a base class constructor is not
   inherited by a derived class.
 - A method that can be overridden by a derived class is call a _virtual method_.
 - static methods are never overridden. They are HIDDEN or SHADOWED just like static or non-static fields
 `A class cannot override the super class's constructor.`
 
- An Overriding method is allowed to make the overridden method more accessible, and since protected is more accessible than default (package), this is allowed. Note that protected access will allow access to the subclass even if the subclass is in a different package but package access will not. 
 
**Lambda Expression**

- Parentheses can be omitted ONLY if there's one parameter and the parameter
  type is not declared. So, `(String s) -> ...` or `s -> ...`.
- When braces are used around the body, the `return` keyword and the semicolon
  are required. For example, `p -> { return p.getAge() < 5; }`
- Lambdas work only with functional interfaces—interfaces that define exactly
  one `abstract` method.
- Each lambda expression has multiple optional and mandatory sections:
  - Parameter type (optional)
  - Parameter name (mandatory)
  - Arrow (mandatory)
  - Curly braces (optional)
  - Keyword `return` (optional)
  - Lambda body (mandatory)
- The return type of the functional method `test` in the functional interface
  `Predicate` is `boolean`. If you try to return another type, the code fails to
  compile. So `Predicate<String> p = (s) -> s == null ? "A" : "B";` is
  incorrect.
- A simple lambda expression could be written in one line:
  `Validate v = p -> p.getAge() <= 26;`
  
- Predicate returns a boolean. 
specifying the parameter type is optional  because the compiler can figure out the parameter types by looking at the signature of the abstract method of any functional interface (here, Predicate's test method).
 does not create a new scope for variables
 
`checkList(List list, Predicate<List> p)`

- Ex. Predicate is typed to List (not ArrayList) in the checkList method, therefore, the parameter type in the lambda expression must also be List. It cannot be ArrayList.

- write a lambda expression for a functional interface, you are essentially providing an implementation of the method declared in that interface but in a very concise manner.  Therefore, the lambda expression code that you write must contain all the pieces of the regular method code except the ones that the compiler can easily figure out on its own such as the parameter types, return keyword, and brackets. 
`
(parameter list) OR single_variable_without_type -> { regular lines of code } OR just_an_expression_without_semicolon`

- A functional interface is an interface that contains exactly one abstract method. It may contain zero or more default methods and/or static methods. Because a functional interface contains exactly one abstract method, you can omit the name of that method when you implement it using a lambda expression. 

- The implementation of Predicate interface must have a method that takes exactly one parameter. You cannot reuse the local variable names that have already been used in the enclosing method to declare the variables in you lambda expression. It would be like declaring the same variable twice.


## Flow control

- The braces `{}` are required for methods, `try` blocks, and `catch` blocks
  even if there's only one statement inside.
- If there're duplicate constant values in a `switch` statements, all the
  duplicate cases will raise a compile error.  a switch statement must have a body.
- The `default` case executes only of no matching values are found.  
- an unreachable statement causes a compilation error.


- double/float/long/boolean cannot be used in a switch statement.


- A break statement with no label attempts to transfer control to the innermost enclosing switch, while, do, or for statement; this statement, which is called the break target, then immediately completes normally. If no switch, while, do, or for statement encloses the break statement, a compile-time error occurs.
  A break statement with label Identifier attempts to transfer control to the enclosing labeled statement  that has the same Identifier as its label; this statement, which is called the break target, then immediately completes normally. In this case, the break target need not be a while, do, for, or switch statement.
  
  
- A continue statement with no label attempts to transfer control to the innermost enclosing while, do, or for statement; this statement, which is called the continue target, then immediately ends the current iteration and begins a new one. If no while, do, or for statement encloses the continue statement, a compile-time error occurs.
  A continue statement with label Identifier attempts to transfer control to the enclosing labelled statement that has the same Identifier as its label; that statement, which is called the continue target, then immediately ends the current iteration and begins a new one. The continue target must be a while, do, or for statement or a compile-time error occurs. If no labelled statement with Identifier as its label contains the continue statement, a compile-time error occurs.


- You cannot have break or continue in an 'if' or 'else' block without being inside a loop. Note that the problem statement mentions, "...occuring by themselves". This implies that the given statement is not wrapped within any other block. 


Note: break with a label is possible in an if/else statement without a loop:     
 label: if(true){          
      System.out.println("break label");    
      break label; //this is valid    
   }



## QUESTIONS

Test Cases Facts

1. Only the access modifier or optional specifiers are allowed before the return type. But optional specifiers are allowed in any
order.

vararg can be send like one parametar and have to be last.
(otherway it cant be clear).

There is no null pointer on calling static methods.

2. Array initializer isn't allowed passing to methods. For example(search another in question ch 4 no 5):
	....
	howMany(true, {true, true});
	....
3. Access Modifier

	■ private: Only accessible within the same class
	■ default (package private) access: private and other classes in the same package
	■ protected: default access and child classes
	■ public: protected and classes in the other packages

4. You cannot create Cyrcle inheritance, two classes inherit each other. 

- There is no way to go more than one level up for methods. variables because variable are never overridden. They are shadowed. So to access any of the super class's variable, you can unshadow it using a cast. For example, ((A) c).data; 

5. You cannot asign "void" like a type to any variable or field state of a class.

6. JavaBeans naming conventios for accessors and mutators

	- Properties are private. private int numEggs;
	- Getter methods begin with is if the property is a boolean. public boolean isHappy() {return happy;}
	- Getter methods begin with get if the property is not a boolean. public int getNumEggs() { return numEggs; }
	- Setter methods begin with set. public void setHappy(boolean happy) { this.happy = happy; }
	- The method name must have a prefix of set/get/is, followed by the first letter of the property in uppercase, followed by the rest of the property name.
	public void setNumEggs(int num) { numEggs = num;

7. Static methods are not allowed to call instance methods. Neither instance variables.

8. Static final variables must be set exactly once, and it must be in the declaration line or in a static initialization block

9. You cannot use a final instance variable before initilize it.

10. Number literals:

	 Floating-point literals are assumed to be double, unless postfixed with an f. For example:
	 double a = 1.0 // double
	 float f = 1.0 // it doesn't compile. It must be with postfixed. 1.0f would be correct.

11. When you concatenate two classes with + and one string, this operation will call the method toString of each class 
	involves in the operation.

12. You cannot call the default constructor created by the compiler if you have created one before. 
	Because the compiler won't add the default constructor since you have added your properly contrusctor o constructors.
	
 Note that calling super(); will not always work because if the super class has defined a constructor with arguments and has not defined a no args constructor then no args constructor will not be provided by the compiler. It is provided only to the class that does not define ANY constructor explicitly.
13. You can call a private constructor from inside the same class using a static method 
	or in the same declaration of a variable member.

14. When java is choosing the most specify method by a call, java prefers autoboxing to varargs. For example:
```
	public class Create {
		Create(Integer num) {
			System.out.print("3 ");
		}
		Create(Object num) {
			System.out.print("4 ");
		}
		Create(int... nums) {
			System.out.print("5 ");
		}
		public static void main(String[] args) {
			new Create(100);
			new Create(1000L);
		}
	}
```
	For this the output would be: 3 4

15. Lambda expressions
	
	1. The parentheses are only optional when there is one parameter and it doesn’t have a type declared.
	For example:
	
print(a, b -> a.startsWith("test")); // DOES NOT COMPILE 

print((a, b) -> a.startsWith("test")); // COMPILE

   
    2. Lambdas work with interfaces that have only one method. 
	   TODO: Review topics.
	   
	3. Remember the one method in the interface called test()? It takes any one reference type parameter and returns a boolean.
	
    4. a lambda expression does not create a new scope for variables. Therefore, you cannot reuse the local variable names that have already been used in the enclosing method to declare the variables in you lambda expression.
      
    5. The implementation of Predicate interface must have a method that takes exactly one parameter.
    
     
 17. StringBuilder:
 append() changes by reference value of string.


- method is said to be overloaded when the other method's name is same and parameters ( either the number or their order) are different.

# Override covariant types
- Covariant returns are allowed since Java 1.5, which means that an overriding method can change the return type to a subclass of the return type declared in the overridden method. But remember than covarient returns does not apply to primitives.

-In case of overriding, the return type of the overriding method must match exactly to the return type of the overridden method if the return type is a primitive.
(In case of objects, the return type of the overriding method may be a subclass of the return type of the overridden method.)

- Cant override with superclass.

- variables and static methods are not overridden and so access to variables and static methods is determined at compile time based on the type of the variable (instead of type of the object referred to by the variable, as is the case with instance methods.) static methods are never overridden. They are HIDDEN or SHADOWED just like static or non-static fields.


- override a method in the subclass, the overriding method (i.e. the one in the subclass) MUST HAVE: .same name .same return type in case of primitives (a subclass is allowed for classes, this is also known as covariant return types). .same type and order of parameters .it may throw only those exceptions that are declared in the throws clause of the superclass's method or exceptions that are subclasses of the declared exceptions. It may also choose NOT to throw any exception. method can throw any RuntimeException (such as a NullPointerException) even without declaring it in its throws clause. you can make it abstract - You would have to make the class as abstract as well though.

-  The overriding method can have more visibility. (It cannot have less.) Note that 'protected' is less restrictive than default 'no modifier'. cannot make the overridden method more private.

`private->'no modifier'->protected->public ( public is accessible to all and private is accessible to none except itself.)`

- static method cannot be overridden by a non-static method and vice versa.

- As a rule, fields defined in an interface are PUBLIC, STATIC, and FINAL. The methods are public. Here, the interface IInt defines thevalue and thus any class that implements this interface gets this field. Therefore, it can be accessed using s.thevalue or just thevalue inside the class. Also, since it is static, it can also be accessed using IInt.thevalue or Sample.thevalue.

- An interface an redeclare a default method and also make it abstract.

- An interface can redeclare a default method and provide a different implementation.

- An interface can have a static method but the method must have a body in that case because a static method cannot be abstract.  static methods can never be abstract (neither in an interface not in a class). A default method must have a body.

