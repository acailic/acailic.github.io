---
title: OCA Review 6 - Exceptions
layout: post
tags: [java, oca, exceptions]
date: 2018-07-25
---

## Exception Handling

- Exceptions are divided into three categories: checked exceptions, runtime
  (unchecked) exceptions, and errors.
- You should not handle errors.
- If a method throws a checked exception, it must be either handled by the
  method or specified in its `throws` clause.
- If a method throws a runtime exception, it may include the exception in its
  `throws` clause.
- An exception is an object of the class `java.lang.Throwable`.
- When calling a method having checked exception(s), you must handle them
  properly, e.g. `throws` from method or adding a `try ... catch` block.
- If a method declares to throw a checked exception, its body can't throw a more
  general exception.
- Error is not a checked or unchecked exception.
- If the creation of an object calls itself recursively without an exit
  condition, it will result a `java.lang.StackOverflowError`. For example:
  `class MyClass { MyClass my = new MyClass(); }`.
- `ExceptionInInitializerError` may be thrown by the JVM when a `static`
  initializer in your code throws a `NullPointerException`.

- When you use System.out.println(exception), a stack trace is not printed. Just the name of the exception class and the message is printed. When you use exception.printStackTrace(), a complete chain of the names of the methods called, along with the line numbers, is printed. It contains the names of the methods in the chain of method calls that led to the place where the exception was created going back up to the point where the thread, in which the exception was created, was started.

## QUESTIONS

Test Cases Facts

1. ExceptionHandling with MethodOverriding 


	If the superclass method does not declare an exception
	, subclass overridden method cannot declare the checked exception but it can declare unchecked exception.
	If the superclass method declares an exception
	, subclass overridden method can declare same, subclass exception or no exception but cannot declare parent exception.

2. Runtime exceptions are also known as unchecked exceptions. They are allowed to be declared, but they don’t have to be. Checked exceptions must be handled or
	declared. Legally, you can handle java.lang.Error subclasses, but it’s not a good idea

3. Exceptions:

	- java.io.IOException is thrown by many methods in the java.io package, but it is always thrown programmatically. 
	The same is true for NumberFormatException; it is thrown programmatically by the wrapper classes of java.lang.
	- 


4. You cannot declare an Exception which is never thrown in that context.
``` 
   try {
        out.print(test());
    } catch (IOException io) {

    }
```
5. Exceptions:

	- Checked exceptions are required to be handled or declared.
	- Runtime exceptions are allowed to be handled or declared. 
	- Errors are allowed to be handled or declared, but this is bad practice.

6 Commons Exceptions:

	- Runtime Exceptions

		Thrown by the JVM

		- ArithmeticException 
		- ArrayIndexOutOfBoundsException 
		- ClassCastException 
		- NullPointerException 

		Thrown by the programer 
		- IllegalArgumentException 
		- NumberFormatException 
		- SecurityException

	- Checked Exceptions

		- FileNotFoundException
		- IOException

	- Errors

		- ExceptionInInitializerError
		- StackOverflowError
		- NoClassDefFoundError


- System.exit(0) kills JVM, after that is not executed.

-  There is no multiple catch exceptions executed( catch->finally).

- RuntimeException
   --ClassCastException (JVM)
   --SecurityException (programer)
   --IndexOutOfBoundsException
   --NullPointerException

- SecurityException extends RuntimeException: It is thrown by the security manager upon security violation. For example, when a java program runs in a sandbox (such as an applet) and it tries to use prohibited APIs such as File I/O, the security manager throws this exception. Since this exception is explicitly thrown using the new keyword by a security manager class, it can be considered to be thrown by the application programmer.

- Exception are not imported automatically. IOException, the java.io package (or just java.io.IOException class) must be imported.

- java.lang.OutOfMemoryError. Note that this is not a subclass of RuntimeException or even Exception. It is a subclass of java.lang.Error. 



-  try block generates NullPointerException which will be caught by the catch block.

``` 
public class TestClass{
   public static void main(String args[]){
      try{
         RuntimeException re = null;
         throw re;
      }
      catch(Exception e){
         System.out.println(e);
      }
   }
}
``` 

# override methods with exceptions

``` 
import java.io.*;
class Great {
    public void doStuff() throws FileNotFoundException{
    }    
}

class Amazing extends Great { 
  public void doStuff() throws IOException, IllegalArgumentException{
  }    
}

public class TestClass {
    public static void main(String[] args) throws IOException{
        Great g = new Amazing();
        g.doStuff();
    }
}
``` 

- The rule is that an overriding method cannot throw an exception that is a super class of the exception thrown by the overridden method.  Now, FileNotFoundException is a subclass of IOException. Therefore, Amazing's doStuff() cannot throw IOException if the base class's doStuff throws only FileNotFoundException. 
 Think of it this way:  FileNotFoundException fne = new IOException(); // Will this work? No, because an IOException is NOT a FileNotFoundException. 
 IOException ioe = new FileNotFoundException(); // Will this work? Yes, because a FileNotFoundException is an IOException. 
  Therefore, overriding method must not throw an exception that cannot be assigned to a variable whose class is the class of the overridden method's exception.
  
- A try MUST be followed by at least one catch or finally.  (a catch block cannot follow a finally block!.)

- ( }catch(IndexOutOfBoundsException e){ throw new NullPointerException()})
 Inside this block, a new NullPointerException is thrown. As this exception is not thrown inside the try block, it will not be caught by any of the remaining catch blocks.
 
- java.io.IOException extends Exception. It cannot be caught by a catch block that catches RuntimeException.

- The finally block is always executed, no matter how control leaves the try block. Only if, in a try or catch block, System.exit() is called then finally will not be executed.


- How many objects have been created by the time the main method reaches its end in the following code? Look for exceptions also.
