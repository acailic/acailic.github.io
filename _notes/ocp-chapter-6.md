---
title: OCP Review 6 - Exceptions and assertions 
layout: post
tags: [java, ocp, exceptions, assertions]
date: 2018-09-07
---

# Exceptions

```
Object
  |
Throwable
  |----------------|
  |                |
Exception         Error
  |
RuntimeException
```

---

## Checked vs Unchecked

- all exceptions occur at runtime
- checked: handle or declare: compiler forces us
- unchecked or _runtime_: we don't need to handle or declare
    - because they're too common: NullPointerException, ArrayIndexOutOfBoundsException, etc.

---

## Should I "catch" Errors? 🤔

- never, ever
- never
- really
- don't do it
- ever
- I'm serious
- don't


---

## Exception on OCP

- study! (page 288)

---

## Try-catch-finally

```java

try {
	// do something that can throw
} catch (Exception e) { 
    // do something if this exception is injected
} finally {
    // do this always
}

```

---

## Try-catch gotchas

* Error! Needs a block

```java
try
	int i = 0;
catch (Exception e) ;
```

---

## Try-catch gotchas

* Error! Try need a catch / finally

```java
try  {
    System.out.println("2");
}   // error!
```

* This is fine

```java

try {
	int i = 0;
} catch (Exception e) { }
```

---
## Try-catch gotchas

Just __one__ try, please

```java

try {
		
} try {
		
} catch (Exception e) {}
```


---

## Try-catch gotchas

```java
// we can have a try-catch with Throwable
try {
	int i = 0;
} catch (Throwable t) {
    //  Pokémon catch: Gotta catch'em all!
}

try {
	int i = 0;
} catch (Exception t) {
	// i++;               Error: i - local to try block
}

```

---

## Try-catch gotchas


```java 
// can have more than one catch
try {
	int i = 0;
} catch (Exception e) {

} catch (Throwable t) {

}

try {
	int i = 0;
} catch (Throwable t) {

} catch (Exception e) { // error: Exception has already been caught

}
```

---

## Finally always gets called

```java
static void print(String s) {
    try  {
        System.out.println("2");
        if (s == null) return;
    } finally {
        System.out.println("3");
    }
    System.out.println("1");

    System.out.println("2");
}
```

---

## Creating your own exceptions


```java
// Runtime, non-checked Exception 
class NonCheckedException extends RuntimeException {
    public NonCheckedException() {  // typical constructors
    }

    public NonCheckedException(String message) {
        super(message);
    }

    public NonCheckedException(Exception cause) {
        super(cause);
    }
}

// Compile-time, checked exception, handle or declare rule
class CheckedException extends Exception {

}

```

---

## Catch-Or-Declare rule (Checked exceptions)

```java 
public class Main {
    public static void main(String[] args) {
        throwsNonChecked(); // no problem with non-checked
        try {
            throwsChecked();
        } catch (CheckedException e) {
            e.printStackTrace();
        }
    }

    static void throwsNonChecked() {
        throw new NonCheckedException();
    }

    static void throwsChecked() throws CheckedException {
        throw new CheckedException();
    }
}

```

--- 

## What does this prints? 😈

```java 
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("DogeBegin");
            new Main().saveToDisk();
        } catch (IOException e) {
            System.out.println(e.getMessage());
        } finally {
            System.out.println("DogeFinally");
        }
    }

    void saveToDisk() throws IOException{
        throw new IOException("Such litte space. Much files. Wow");
    }
}
```

---

## What does this prints?

```
DogeBegin
Such litte space. Much files. Wow
DogeFinally	
```

---
## What does this prints? 😈

```java 
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("DogeBegin");
            new Main().saveToDisk();
        } catch (IOException e) {
            System.out.println("IODoge" + e.getMessage());
        } catch (Exception e) {
            System.out.println("ExceptionDoge"+ e.getMessage());
        } finally {
            System.out.println("DogeFinally");
        }
    }

    void saveToDisk() throws IOException{
        throw new IOException("Such litte space. Much files. Wow");
    }
}
```

---

## What does this prints?

```
DogeBegin
IODogeSuch litte space. Much files. Wow
DogeFinally

```


---


## What does this prints? 😈

```java
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        new Main().run();
    }

    void run() {
        try {
            System.out.println("DogeBegin");
            saveToDisk();
        } catch (IOException e) {
            System.out.println("IODoge" + e.getMessage());
            return;
        } finally {
            System.out.println("DogeFinally");
        }
        System.out.println("DogeReturn");
    }

    void saveToDisk() throws IOException{
        throw new IOException("Such litte space. Much files. Wow");
    }
}
```
---

## What does this prints?

```

DogeBegin
IODogeSuch litte space. Much files. Wow
DogeFinally
```

---
## What does this prints? 😈

```java
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        new Main().run();
    }

    void run() {
        try {
            System.out.println("DogeBegin");
            saveToDisk();
        } catch (IOException e) {
            System.out.println("IODoge" + e.getMessage());
            return;
        } finally {
            System.out.println("DogeFinally");
        }
        System.out.println("DogeReturn");
    }

    void saveToDisk() throws IOException{
        throw new RuntimeException("Such litte space. Much files. Wow");
    }
}

```

---

```
Exception in thread "main" java.lang.RuntimeException: Such litte space. Much files. Wow
DogeBegin
	at Main.saveToDisk(Main.java:22)
DogeFinally
	at Main.run(Main.java:11)
	at Main.main(Main.java:5)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:497)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:134)
```

---

## Multicatch

```java
try {
            
} catch (ArrayIndexOutOfBoundsException | ArithmeticException e) {  
    // can't use parent & child here
} finally {

}

try {

} catch (Exception | ArithmeticException e) {   // won't compile

} finally {

}

```

---

## Multicatch is effectively final

```java
try {

} catch (ArithmeticException  | ArrayIndexOutOfBoundsException e) {
    e = new ArithmeticException();  // error: e is final
} 
```

- but this compiles!

```java
try {

} catch (ArithmeticException  e) {
    e = new ArithmeticException();
} finally {

}
```

---


## Remember we can catch something that can be thrown

```java
static void print(String s) {
    try {
        thrower();
    } catch (SQLException e) {

    } 
}

static void thrower() throws ArrayIndexOutOfBoundsException {
    
}
```

---

## Try-with-resources

- need that Connection and Statement implement AutoCloseable
- AutoCloseable implemented by more that 150 classes!

```java
try (Connection connection = DriverManager.getConnection(url);
    Statement statement = connection.createStatement()) {

}
```

---

## Try-with-resources

- no need for that try
- here's how this magic works

```java
try (Connection connection = DriverManager.getConnection(url);
    Statement statement = connection.createStatement()) {

} finally {
    statement.close();  // these are the 1st two lines in the finally block
    connection.close(); // like super / this in a constructor

    // can't use the statement here: closed!
}
```

--- 

## AutoCloseable

```java
try(MyFile file = new MyFile()) {

}
// ...
class MyFile implements AutoCloseable {
    public MyFile() {
        super();

        System.out.println("Creating " + this);
    }

    @Override
    public void close() {
        System.out.println("Closing " + this);
    }
}

```

---

## Autocloseable is a functional interface...

```java
try (AutoCloseable c = new AutoCloseable() {
    @Override
    public void close() {

    }
}) {

} catch (Exception e) {
    
}
```

- as lambda

```java
try (AutoCloseable c = () -> {

}) {

} catch (Exception e) {

}

```

---

## Assertions

- conditions that can never happen while the program is running
- they're absurd, but we want to catch them while developing
- we need to enable assertions (-ea) Edit Configurations > VM Options

---

## Useful asserts

- we're on the main thread, or in a background thread
- check exhaustive switch (with assert that fails in default)
- check an else is never met
- check some object that shouldn't be null is never null...

```java
// check we're running on Main Thread

assert (Thread.currentThread().getName().equals("main"));
```



### Links Exceptions and Assertions

- [Assertions](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/Assertions.java) <br />
- [Calling Methods that Throw Exceptions](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/CallingMethodsThatThrowExceptions.java) <br />
- [Catching Various Types of Exceptions](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/CatchingVariousTypesOfExceptions.java) <br />
- [Common Exception Types](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/CommonExceptionTypes.java) <br />
- [Creating Custom Exceptions](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/CreatingCustomExceptions.java) <br />
- [Exception Intro](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/ExceptionIntro.java) <br />
- [Nested Exception](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/NestedException.java) <br />
- [Try with Resources](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/TryWithResources.java) <br />
- [Using Multi-Catch](https://github.com/acailic/java8-learning/blob/master/Java-8/src/exceptionsAndAssertions/UsingMultiCatch.java) <br />


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

- 1. An overriding method must not throw any new or broader checked exceptions than the ones declared in the overridden method. This means, the overriding method can only throw the exceptions or the subclasses of the exceptions declared in the overridden method. It can throw any subclass of Error or RuntimeException as well because it is not mandatory to declare Errors and RuntimeExceptions in the throws clause. An overriding method may also choose not to throw any exception at all.
2. AssertionError is a subclass of Error.


