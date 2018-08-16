---
title: OCA Review 2 - Operators and statements
layout: post
tags: [java, oca]
date: 2018-07-19
---

## Concatenation with String

Placing one `String` before the other `String` and combining them together is
called string _concatenation_. In Java, the concatenation is called by using the
`+` operator. There're several roles to remember for this operator:

1. If both operands are numeric, `+` means numeric addition.
2. If either operand is a `String`, `+` means concatenation.
3. The expression is evaluated left to right.

Here're some examples:

{% highlight java %}
System.out.println(1 + 2);           // 3
System.out.println(1 + 2 + ".");     // 3.
System.out.println("It's " + true);  // It's true
System.out.println("It's " + 1);     // It's 1
System.out.println("It's " + "OK");  // It's OK
{% endhighlight %}

## The String Pool

Since strings are everywhere in Java, they uses up a lot of memory. In some
production applications, they can use up 25-40 percent of the memory in the
entire program. Therefore, JVM optimizes the usage of strings by introducing
the _string pool_ concept: the idea is to reuse the common ones the program in
an internal location of JVM (Java Virtual Machine). **The string pool contains
literal values appeared in the program.** For example, `"name"` is a literal
value, but `myObject.toString()` isn't.

{% highlight java %}
String literal = "name";
String nonLiteral1 = new String("name");
String nonLiteral2 = myObject.toString();
{% endhighlight %}

## Mutability and Chaining of StringBuilder

Previously, we saw the usage of `String`. Actually, `String` is immutable, which
means that if we keep using a lot of strings as intermediate references for
concatenation, it will be very inefficient: almost all of them are immediately
eligible for garbage collection after their creation.

Another solution is to use `StringBuilder`. The class `StringBuilder` is
mutable, and more interestingly, it can be used for chaining multiple
concatenations, which avoids the creation of string interims:

{% highlight java %}
String str = new StringBuilder()
    .append("hello")
    .append("string")
    .append("chaining")
    .toString();
{% endhighlight %}

## Understanding Java Arrays

Now, let's take a look in _Arrays_, an ordered list in Java. It can be created
in several ways:

{% highlight java %}
int[] numbers = new int[3];
int[] numbers = new int[]{1, 2, 3};
int[] numbers = {1, 2, 3};  // only works during asssignment
{% endhighlight %}

We can use `equals()` to compare two arrays because an array is an object.
However, the `equals()` method on arrays does not look at the elements of the
array. As for primitives, their array is still an object.

{% highlight java %}
int one = 1;                // primitive
int[] numbers = {1, 2, 3};  // object
{% endhighlight %}

There're still many things to explore about array, but I can't explain more
here because of the time limit.


- Equals method of a primitive wrapper class ( e.g. java.lang.Integer, Double, Float etc) are:

 1. symmetric => a.equals(b) returns same as b.equals(a) 
 2. transitive => if a.equals(b) and b.equals(c) return true, then a.equals(c) returns true.
 3. reflexive => a.equals(a) return true.  For example, the following code for the equals method on Integer explains how it works: 
  
```
  public boolean equals(Object obj) { 
     if (obj instanceof Integer) {   
       return value == ((Integer)obj).intValue(); 
     }   
   return false; 
  }
```

## Converting Between Array and List

There're several ways to convert between an array and an `ArrayList`. Now, let's
take a look about how to convert a `List` to an array.

{% highlight java %}
List<String> list = new ArrayList<>();
list.add("one");
list.add("two");
String[] array = list.toArray(new String[0]);  // ["one", "two"]
{% endhighlight %}

You might ask: why we need to specify the size of 0 for the array input, is it
incorrect? Actually, `ArrayList` will create a new array of the proper size for
the return value, if the input size does not fit the return one. Here's the
source code of `ArrayList#toArray(T[] a)` in Java 8:

{% highlight java %}
@SuppressWarnings("unchecked")
public <T> T[] toArray(T[] a) {
    if (a.length < size)
        // Make a new array of a's runtime type, but my contents:
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    System.arraycopy(elementData, 0, a, 0, size);
    if (a.length > size)
        a[size] = null;
    return a;
}
{% endhighlight %}

Now, let's take a look how to convert an array to `List` through 3 ways: using
`Arrays#asList(T...)`, `ArrayList`, and Java stream.

{% highlight java %}
String[] wordArray = {"one", "two"};

// 1. using Arrays#asList(T...)
List<String> wordList1 = Arrays.asList(wordArray);
wordList1.remove(1);     // unsupported operation
wordList1.add("three");  // unsupported operation

// 2. using ArrayList
List<String> wordListi2 = new ArrayList<>();
for (String word : wordArray) {
  wordList2.add(word);
}

// 3. using Java stream:
List<String> wordList3 = Stream.of(wordArray).collect(Collectors.toList());
{% endhighlight %}

* boolean remove ArrayList:  
 *The JavaDoc API description of this method is important for the exam -  public boolean remove(Object o) Removes the first occurrence of the specified element from this list, if it is present (optional operation). If this list does not contain the element, it is unchanged. More formally, removes the element with the lowest index i such that (o==null ? get(i)==null : o.equals(get(i))) (if such an element exists). Returns true if this list contained the specified element (or equivalently, if this list changed as a result of the call).

## Working with Dates and Times

In Java 8, a famous Java Date library—Joda Time—has been integrated into the
built-in package as `java.time`. Thanks to this library, handling date and time
become much easier. Here's a table listing the most important classes we need to
remember:

Class | Date | Time | Time Zone
:--- | --- | --- | ---
`LocalDate` | Yes | No | No
`LocalTime` | No | Yes | No
`LocalDateTime` | Yes | Yes | No
`ZonedDateTime` | Yes | Yes | Yes


- 

Given:

        LocalDate d1 = LocalDate.parse("2015-02-05", DateTimeFormatter.ISO_DATE);
         ~LocalDate.of(2015, 2, 5)
        ~ LocalDate.now()

will produce 2015-02-05.



## Java Data Types

**Primitives**

- Characters `char` can be concatenated, such as `'a' + 'b'`. When doing so,
  they are considered as integer using their corresponding value in ASCII
  table.
- Binary number starts with the prefix `0b` or `0B`, e.g. `0b1101`.
- Octal number starts with the prefix `0`, e.g. `011`.
- Decimal number does not start with `0`, e.g. `11`.
- Hexadecimal number starts with the prefix `0x`, e.g. `0xAAA`.
- A literal ends with a character could be a `float`, a `double`, a `long`, or
  a hexadecimal number.
- A `long` can be assigned using an integer `int`, because Java knows how to
  widen the type.
- An integer cannot be assigned as a decimal number automatically.
- Wrapper classes `Byte`, `Short`, `Integer`, and `Long` cache objects with
  values in the range of `-128` to `127`.
- Wrapper class `Character` caches objects with values `0` to `127`.
- Wrapper class `Boolean` has 2 cached instances, `Boolean.TRUE` and
  `Boolean.FALSE`. They're accessible directly because only two exist.
- `Int` is not defined in the Java API, the correct wrapper class for `int` is
  `Integer`.
- `Bool` is not defined in the Java API, the correct wrapper class for `boolean`
  is `Boolean`.

**Operators**

- The logical operator AND `&&` has a higher operator precedence than the
  operator `||`.


## Exam Essentials

In this post, I reviewed the most difficult / tricky part of Java Core APIs,
including operations of `String`, `StringBuiler`, Arrays, `ArrayList`, and Java
Time in Java 8. As for the exam essentials, you need to know about:

- Be able to determine the output of code using `String`.
- Be able to determine the output of code using `StringBuilder`.
- Understand the difference between `==` and `equals`.
- Be able to determine the output of code using `ArrayList`.
- Recognize invalid uses of dates and times.

## QUESTIONS

Test Cases Facts

Scope variable.
Cant compare different type variables.

1. Operators

	TODO: Study precendece operators
	
	% and * are same precendece.
	'=' lowest precendece.

2. Modulus operations results between 0 and (y  - 1).

	For example: 11 % 3 = 2 = 3 - 1 = 2

3. Numeric Promotion Rules:
	1. If two values have different data types, Java will automatically promote one of the values to the larger of the two data types.
	2. If one of the values is integral and the other is floating-point, Java will automatically
	promote the integral value to the floating-point value’s data type.
	3. Smaller data types, namely byte, short, and char, are first promoted to int any time
	they’re used with a Java binary arithmetic operator, even if neither of the operands is	int.
	4. After all promotion has occurred and the operands have the same data type, the resulting value will have the same data type as its promoted operands.



- A narrowing primitive conversion may be used if all of the following conditions are satisfied: 
 1. The expression is a constant expression of type int. 
 2. The type of the variable is byte, short, or char. 
 3. The value of the expression (which is known at compile time, because it is a constant expression) is representable in the type of the variable. 
 
Note that float: `d = 0 * 1.5f;`  and `float d = 0 * (float)1.5 ;` are OK
Note that narrowing 
conversion does not apply to long or double.
 So, `char ch = 30L;` will fail even though 30 is representable in char. NOT OK

4. For ternary operator parentheses are not required. That means that you can have a expression like this:

	int x = 5;
	System.out.println(x > 2? x < 4? 10:8:7);
	output: 8

5. One tricky infinitive loop
	
	for (int i = 0; i < 10;){
		i = i++;
	}

	i++ increments de i bu then asign the old value to i again what means i still with the same value in that case it is 0.

        
    if(k) compiles.
    while(k) doesnt compile.
    
    
6.  You cannot campare different data types using relational operators(==, >=, <=, ect).

7. Advise: Be careful with variables out of scope like the question number 16 of Reviews Cuestion of chapter 2:

	do {
		int y = 0;
	} while(y <= 10);

	y = out of scope for the line of the while.

8. "For Loop" statement

	1. The structure of a basic for statement
	1. Parentheses (required)
	1. Semicolons (required)
	1. for(initialization; booleanExpression; updateStatement) {
	// Body
	} Curly braces required for block of multiple statements, optional for single statement

	1.  Initialization statement executes
	2. 2 If booleanExpression is true continue, else exit loop
	3. Body executes
	4. Execute updateStatements
	5. Return to Step 2

9. Data types supported by switch statements include the following:
	 * int and Integer
	 * byte and Byte
	 * short and Short
	 * char and Character
	 * int and Integer
	 * String
	 * enum values

- Note that boolean and long, and their associated wrapper classes, are not supported by switch statements

	 The values in each case statement must be compile-time constant values of the same data type as the switch value. 
	 This means you can use only literals, enum constants, or final constant variables of the same data type. By final constant
	 , we mean that the variable must be marked with the final modifier and initialized with a literal value 
	 in the same expression in which it is declared.

	 There is no requirement that the case or default statements be in a particular
	order.
	
# Bad coding practice forward : )
 	
10. Break - Optional label parameter allow to break out of higher level outer loop. Break inside while, do-while, for loops ends the loop early.

 Continue - optional label gets flow to finish te execution of current loop and  goes to boolean expresion. 
 
 - break breaks the nearest outer loop. Once a break is encountered, no further iterations of that loop will execute.
 continue simply starts the next iteration of the loop. Once a continue is encountered, rest of the statements within that loop are ignored (not executed ) and the next iteration is started.

- break leaves a loop, continue jumps to the next iteration


Flow | Break | Continue
--------|------ | ------ 
if | No | No
while | Yes | Yes
do while | Yes | Yes
for | Yes | Yes
switch | Yes | No

- Math.round: Observe that rounding is a standard mathematical procedure where the number that lies exactly between two numbers always rounds up to the higher one. So .5 rounds to 1 and -.5 rounds to 0.

- enhanced for loop needs either an array or an object of a class that implements java.lang.Iterable. Map does not implement Iterable, though you can use keySet() or values() methods to get a Collection (which extends Iterable) and then iterate over that Collection.

- Ex : 
```
What will the following code print?
void crazyLoop(){
   int c = 0;
   JACK: while (c < 8){
       JILL: System.out.println(c);
       if (c > 3) break JILL; else c++;
   }
}
```
break JILL; would be valid only when it is within the block of code under the scope of the label JILL. In this case, the scope of JILL extends only up till System.out.println(c); and break JILL; is out of the scope of the label. 


- switch statement:
1. Only String, byte, char, short, int, (and their wrapper classes Byte, Character, Short, and Integer), and enums can be used as types of a switch variable. (String is allowed only since Java 7). 
2. The case constants must be assignable to the switch variable. For example, if your switch variable is of class String, your case labels must use Strings as well.
3. The switch variable must be big enough to hold all the case constants. For example, if the switch variable is of type char, then none of the case constants can be greater than 65535 because a char's range is from 0 to 65535.
4.  All case labels should be COMPILE TIME CONSTANTS. 
5. No two of the case constant expressions associated with a switch statement may have the same value.
6. At most one default label may be associated with the same switch statement.
 7. Note that long, float, double, and boolean are not allowed.
 
-   rest of the statements after are unreachable :
```
if(index == 3){       
   break;   
  }else {  
   continue; 
   }
```


- Example:


```
public class ForSwitch{
    public static void main(String args[]){
        char i;
        LOOP: for (i=0;i<5;i++){
            switch(i++){
                case '0': System.out.println("A");
                case 1: System.out.println("B"); break LOOP;
                case 2: System.out.println("C"); break;
                case 3: System.out.println("D"); break;
                case 4: System.out.println("E");
                case 'E' : System.out.println("F");
            }
        }
    }
}

```


Defining i as char doesn't mean that it can only hold characters (a, b, c etc). It is an integral data type which can take any +ive integer value from 0 to 2^16 -1.
Integer 0 or 1, 2 etc. is not same as char '0', '1' or '2' etc.
so when i is equal to 0, nothing gets printed and i is incremented to 1 (due to i++ in the switch).
 i is then incremented again by the for loop for next iteration. so i becomes 2.
when i = 2, "C" is printed and i is incremented to 3 (due to i++ in the switch) and then i is incremented to 4 by the for loop so i becomes 4.
when i = 4, "E" is printed and since there is no break, it falls through to case 'E' and "F" is printed.
i is incremented to 5  (due to i++ in the switch) and then it is again incremented to 6 by the for loop. Since i < 5 is now false, the for loop ends.


- In Java, a while or do/while construct takes an expression that returns a boolean. But unlike a for loop, you cannot put instantiation and increment sections in the while condition.

-  The Java language, like C and C++ and many languages before them, arbitrarily decree that an else clause belongs to the innermost.

```
int i = 5; 
float f = 5.5f;
 if (i == f)  //  value of i will be promoted to a float i.e. 5.0, and so it returns false.
```



- `int k = ((Integer) t).intValue()/10;` Since both the operands of / are ints, it is a integer division. This means the resulting value is truncated (and not rounded). 



-  `k += (k = 4) * (k + 2);` -1 of k is saved by the compound assignment operator += before its right-hand operand (k = 4) * (k + 2) is evaluated. Evaluation of this right-hand operand then assigns 4 to k, calculates the value 6 for k + 2, and then multiplies 4 by 6 to get 24. This is added to the saved value 1 to get 25, which is then stored into k by the += operator
