---
title: OCA Review 6 - Exceptions
layout: post
tags: [java, oca, exceptions]
date: 2018-08-26
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


- CloneNotSupportedException extends Exception Thrown to indicate that the clone method in class Object has been called to clone an object, but that the object's class does not implement the Cloneable interface. (NOT RUNTIME)

- System.exit(0) kills JVM, after that is not executed.

- Finally is ALWAYS executed. (Only exception is System.exit() ).

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

- Both - ArrayIndexOutOfBoundsException and StringIndexOutOfBoundsException, extend IndexOutOfBoundsException

- ia[-1]- ArrayIndexOutOfBoundsException

- A try without resources must have either a catch or a finally. It may have both as well.

-  `tc.method(null);` The reason is quite simple, the most specific method depending upon the argument is called. Here, null can be passed to all the 3 methods but FileNotFoundException class is the subclass of IOException which in turn is the subclass of Object. So, FileNotFoundException class is the most specific class. So, this method is called.



 # override Constructor with exceptions
 
-  Constructor must declare all the checked exceptions declared in the base constructor (or the super classes of the checked exceptions). They may add other exceptions as well. This behavior is exactly opposite from that of methods. The overriding method cannot throw any checked exception other than what the overridden method throws. It may throw subclasses of those exceptions as well.

``` 
public CleanConnector(){  
  try{ super(); }catch(Exception e){} //WRONG : call to super must be first statement in constructor }
``` 


- Observe that the rule for overriding a method is opposite to the rule for constructors. 
An overriding method cannot throw a superclass exception, while a constructor of a subclass cannot throw subclass exception (Assuming that the same exception or its super class is not present in the subclass constructor's throws clause). For example:  
``` 
  class A{   
  public A() throws IOException{ }   
    void m() throws IOException{ }  
 }  
  class B extends A{
     //IOException is valid here, but FileNotFoundException is invalid  
   public B() throws IOException{ }      //FileNotFoundException is valid here, but Exception is invalid   
  void m() throws FileNotFoundException{ } 
} 
``` 
(Note: FileNotFoundException is a subclass of IOException, which is a subclass of Exception) If the subclass constructor's throws clause includes the same exception or its superclass, then it can throw any other exception as well.

# Questions

- try-with-resources statement :
  1. The resource class must implement java.lang.AutoCloseable interface. Many standard JDK classes such as BufferedReader, BufferedWriter) implement java.io.Closeable interface, which extends java.lang.AutoCloseable.
  2. Resources are closed at the end of the try block and before any catch or finally block. 
  3. Resources are not even accessible in the catch or finally block. For example: ` try(Device d = new Device()){             d.read();         }finally{             d.close(); //This will not compile because d is not accessible here.         } `
  4. Resources are closed in the reverse order of their creation.  
  5. Resources are closed even if the code in the try block throws an exception.  
  6. java.lang.AutoCloseable's close() throws Exception but java.io.Closeable's close() throws IOException.  
  7. If code in try block throws exception and an exception also thrown while closing is resource, the exception thrown while closing the resource is suppressed. The caller gets the exception thrown in the try block.


- Assertions can be enabled or disabled for specific packages or classes. To specify a class, use the class name. 
  To specify a package, use the package name followed by "..." (three dots):
  java -ea:<class> myPackage.MyProgram
  java -da:<package>... myPackage.MyProgram 
  Each enable or disable modifies the one before it. This allows you to enable assertions in general, but disable them in a package and its subpackages:
  java -ea -da:<package>... myPackage.myProgram
  To enable assertion for one package and disable for other you can use: 
  java -ea:<package1>... -da:<package2>... myPackage.MyProgram 
  You can enable or disable assertions in the unnamed root (default)package (the one in the current directory) using the following commands: 
  java -ea:... myPackage.myProgram java -da:... myPackage.myProgram
   when you use a package name in the ea or da flag, the flag applies to that package as well as its subpackages.


-  two valid ways of writing assertions:
  1. assert <boolean_expression>;
  In this style, the boolean expression must evaluate to true or false. If it evaluates to true, everything is fine. If it evaluates to false, a new AssertionError is thrown. Note that <boolean_expression> can also be a method call that returns a boolean. The above line is thus equivalent to the following code:
  if( !<boolean_expression>  ) throw new AssertionError();
  2. assert <boolean_expression> : <any_expression_but_void>;
  This is same as the above except that if <boolean_expression> evaluates to false, <any_expression_but_void> is evaluated and its value is passed to the constructor of the AssertionError object. <any_expression_but_void> should evaluate to any object or a primitive. Thus, it cannot be a method call that returns void. Note that, if <boolean_expression> evaluates to true, <any_expression_but_void> is not evaluated. The above line is thus equivalent to the following code:
  if( !<boolean_expression>  ) throw new AssertionError(<any_expression_but_void>);
  
- Assertions should be used for: 
  1. Validating input parameters of a private method.[But NOT for public methods. public methods should throw regular exceptions when passed bad parameters.)
  2. Anywhere in the program to ensure the validity of a fact which is almost certainly true.
  3. Validating post conditions at the end of any method. This means, after executing the business logic, you can use assertions to ensure that the internal state of your variables or results is consistent with what you expect. For example, a method that opens a socket or a file can use an assertion at the end to ensure that the socket or the file is indeed opened.
  
  
- Assertions should not be used for:
 1. Validating input parameters of a public method. Since assertions may not always be executed, the regular exception mechanism should be used. 
 2. Validating constraints on something that is input by the user. Same as above. 
 3. Should not be used for side effects. 
 
- If an exception is thrown within the try-with-resources block, then that is the exception that the caller gets. But before the try block returns, the resource's close() method is called and if the close() method throws an exception as well, then this exception is added to the original exception as a supressed exception. 

- An assertion signifies a basic assumption made by the programmer that he/she believes to be true at all times. It is never a wise idea to try to recover when an assertion fails because that is the whole purpose of assertions: that the program should fail if that assumption fails.
- controlling the execution of assertions at run time: -da, -enableassertions

 - Here assert is being used as an identifier (a method name is an identifier). However, beginning Java 1.4, assert has become a keyword. You cannot use a keyword as an identifier. Therefore, to use 'assert'  as an identifier instead of a keyword, you have to tell the compiler that your code is 1.3 compliant. It will generate a warning but it will compile. To do so just use the -source option like this:  javac -source 1.3 Assertion.java 
  Remember that you CANNOT use 'assert' as a keyword as well as an identifier at the same time.
