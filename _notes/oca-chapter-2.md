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


1. Operators

	TODO: Study precendece operators

2. Modulus operations results between 0 and (y  - 1).

	For example: 11 % 3 = 2 = 3 - 1 = 2

3. Numeric Promotion Rules:
	1. If two values have different data types, Java will automatically promote one of the values to the larger of the two data types.
	2. If one of the values is integral and the other is floating-point, Java will automatically
	promote the integral value to the floating-point value’s data type.
	3. Smaller data types, namely byte, short, and char, are first promoted to int any time
	they’re used with a Java binary arithmetic operator, even if neither of the operands is	int.
	4. After all promotion has occurred and the operands have the same data type, the resulting value will have the same data type as its promoted operands.

4. For ternary operator parentheses are not required. That means that you can have a expression like this:

	int x = 5;
	System.out.println(x > 2? x < 4? 10:8:7);
	output: 8

5. One tricky infinitive loop
	
	for (int i = 0; i < 10;){
		i = i++;
	}

	i++ increments de i bu then asign the old value to i again what means i still with the same value in that case it is 0.

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

	 ** Note that boolean and long, and their associated wrapper classes, are not supported by switch statements

	 The values in each case statement must be compile-time constant values of the same data type as the switch value. 
	 This means you can use only literals, enum constants, or final constant variables of the same data type. By final constant
	 , we mean that the variable must be marked with the final modifier and initialized with a literal value 
	 in the same expression in which it is declared.

	 There is no requirement that the case or default statements be in a particular
	order.
