---
title: OCA Review 3 - Core Java APIs
layout: post
tags: [java, oca]
date: 2018-07-20
---


## Java Core APIs

**String**

- All the string literals are automatically instantiated into a `String`
  object.
- Whenever the JRE receives a new request to initialize a `String` variable
  using the assignment operator, it checks whether a `String` object with the
  same value already exists in the string pool.
- `String` objects created using the operator `new` are never placed in the
  string pool.
- The method `substring` does not include the character at the end position.
- String is immutable. StringBuilder and StringBuffer are not immutable.
- String: concat() creates a new string; append changes current;
- if index is not okay : StringIndexOutOfBoundsException.
- has toString() method implemented.
- charAt() returns char value. 
-  have method `trim`. (Builder don't have.)
- A string must be enclosed in double quotes ".
- The String class has no reverse( ) .

**String Builder**

- `StringBuilder` and `StringBuffer` have the same methods.
- `StringBuilder` has a constructor without any parameter.
- `StringBuilder` has a constructor with a customized capacity as `int`.
- `StringBuilder` has a constructor with a default capacity and an initial word
  as `String`.
- `StringBuilder` does not have method `trim`.
- `StringBuilder` does not have method `concat`.
- `StringBuilder#subSequence(int, int)` does not modify the content of builder,
  a new string is returned.
- StringWrapper class does not implement toString method.

- String, StringBuilder, and StringBuffer - all are final classes.
- String class itself is final and so all of its methods are implicitly final.
- `public void ensureCapacity(int minimumCapacity)` - Ensures that the capacity is at least equal to the specified minimum. If the current capacity is less than the argument, then a new internal array is allocated with greater capacity. The new capacity is the larger of: 
 The minimumCapacity argument. 
 Twice the old capacity, plus 2. 
 If the minimumCapacity argument is nonpositive, this method takes no action and simply returns.
 
 - `public StringBuilder(int capacity)`
   Constructs a string builder with no characters in it and an initial capacity specified by the capacity argument.

 - append() method does not exist in String class. It exits only in StringBuffer and StringBuilder.
 
**Arrays**

- An array itself is an object.
- The creation of an array involves three steps: declaration of an array,
  allocation of an array, and initialization of array elements.
- An array is an object, so it's allocated using the keyword `new`.
- Elements of an array that store primitive data types store `0` for integer
  types (`byte`, `short`, `int`, `long`).
- Elements of an array that store primitive data types store `0.0` for decimal
  types (`float`, `double`).
- Elements of an array that store primitive data types store `false` for
  `boolean`.
- Elements of an array that store primitive data types store `/u0000` for
  `char` data.
- A multidimensional array can be asymmetrical. it may or may not define the
  same number of columns for each of its rows.
- Array anonymous initializer is only allowed in the declaration.


- if  al is declared as ArrayList<Double>, therefore the add method is typed to accept only a Double. 
{% highlight java %}
String[] arr = { "A", "B", "C" }; // OK
arr = { "C", "B", "A" }; // Does not compile!
{% endhighlight %}

**ArrayList**

- `ArrayList#remove(Object o)` removes the first occurrence of the specified
  element from this list, if it is present.
- Using generics only on one side in a declaration is allowed, so
  `List<Integer> nums = new ArrayList();` compiles. However, this is not
  suggested. An unchecked warning will be issued by the compiler.
- It's not possible to remove elements from an `ArrayList` while iterating
  through it using a `for` loop.
- `indexOf(Object o)` returns the index of the first occurrence of the specified
  element in the list, or `-1` if the list doesn't contain the element.
- An `ArrayList` can store any type of object.
- `ArrayList#contains(Object)` compares value and not reference.
- `ArrayList` has overridden the default `toString` method, so an empty array
  list prints `[]`.
- no length method (there is size ());

- ArrayList
```  
  ArrayList is a subclass of AbstractList.
  java.lang.Object
   -  java.util.AbstractCollection<E>
     -    java.util.AbstractList<E>
       -      java.util.ArrayList<E>
  All Implemented Interfaces:
  Serializable, Cloneable, Iterable<E>, Collection<E>, List<E>, RandomAccess
```
  
- ArrayList is a List so you can use it where ever a List is required. This include Collections methods such as sort, reverse, and shuffle. 
 
- It allows you to access its elements in random order. Because you can directly access any element using get(index) method. (This is unlike a LinkedList).

**Date**

- `LocalDate` instances are immutable.
- All the methods that seem to manipulate `LocalDate`'s value return a copy of
  the `LocalDate` instance on which it's called.
- The `withXX` methods return a copy of `LocalDate`'s value replacing the
  specified day, month, or year in it.
- `LocalTime` stores time to nanosecond precision.
- `LocalDate` doesn't define a `plus()` method, which accepts an integer value
  to be added to it. You should use `plusXXX` where the expression can be days,
  weeks, months, years.
- Cannot use a `DateTimeFormatter` to format a date object, because it has no
  time.
- DateTimeException because it doesn't have time component will throw an exception at run time. `"main" java.time.format.DateTimeParseException: Text '2015-01-01`

## QUESTIONS

Test Cases Facts


1.  Comparing by "==" your are comparing by reference equality between two objects.
	But otherwise String.equals compare as reference equality and even values of two string against each other.

	Also, when you declare two String like this:

	String a = "Olivia";
	String b = "Olivia";

	if (a == b)	They are equals. Because both point to the same value in the Java String pool 

2. How substrings work:

	Substring method allowed you call with just one value what meaning begin at.

	For example:
	String a = "0123456789"; a.substring(5) = 56789

	Even, you can call this using the length where the result would be nothing = ""

	String a = "0123456789"; out.println(a.substring(10)); = ""

	** substring(int beginIndex)
     * Returns a string that is a substring of this string. The
     * substring begins with the character at the specified index and
     * extends to the end of this string. <p>
     * Examples:
     * "unhappy".substring(2) returns "happy"
     * "Harbison".substring(3) returns "bison"
        012345678
     * "emptiness".substring(9) returns "" (an empty string)
   
     *
     * @param      beginIndex   the beginning index, inclusive.

	** substring(int beginIndex, int endIndex)
     * Returns a string that is a substring of this string. The
     * substring begins at the specified {@code beginIndex} and
     * extends to the character at index {@code endIndex - 1}.
     * Thus the length of the substring is {@code endIndex-beginIndex}.
     * <p>
     * Examples:
     * "hamburger".substring(4, 8) returns "urge"
     * "smiles".substring(1, 5) returns "mile"
 
     *
     * @param      beginIndex   the beginning index, inclusive.
     * @param      endIndex     the ending index, exclusive.

3. Java does not allow you to compare different types using ==. For example String and StringBuilder.

4. A String concatenated with any other type gives a String.d
	For example
```
	String a = "a";
	a += false; // afalse
	a += 0L; // afalse0
	a += 0d; // afalse00
```

5. Strings from the pool are different the string build with String from the pool.
	One comes directly from the string pool and the other comes from building using String operations.

6. Array in java:

	- Array is an ordered list
	- Arrays is an object.
	- has length
	- int[] numbers1 = new int[3] When using this form to instantiate an array, set all the elements to the default value for
	  that type. 
	- You can initialize an array like this "int[] numbers2 = new int[] {42, 55, 99}" for this case you don't need to
	  specify the size because java knows what is the size by the array on the left side of equal sign

	  Also, As a shortcut, Java lets you write this: int[] numbers2 = {42, 55, 99};

	- Multiple arryas in declaration:

		int[] ids, types; = int [] ids; int types [];
		
		but this case is different:
		
		int ids[], types; = int ids[]; int types;
	- Arrays can be cast like a primitiv type or reference type.
	- If initialize an array you must specified the size of it. Although it is legal to leave out the size for later
	dimensions of a multidimensional array, the first one is required. For example:

	int [][] scores = new int[5][];
	int [][] scores = new int[5][5];

7. An array is not able to change its size. Both an Array and an ArrayList are ordered and have indexes. 
	Neither is immutable because their elements can change.
	ArrayList has size();

8. An array does not override equals() and so uses object equality. ArrayList does
	override equals() and defines it as the same elements in the same order.

9. About Arrays class:

	Its method "int binarySearch(int[] a, int key)":

		They array "a" must be sorted prior to making this call. If it is not sorted. The result will be undefined.

		Returns: It will be the index of the search key, if it is contained in the array. Otherwise, (-(insertion point) - 1)

10. Collections API
	
	Sort:

		This method can sort either a List of numbers and a List of String. 
		In case of a list of numbers, the numbers are sort in the normal way. But in case of a List of String
		, the sort would be alphabetically which mean that the numbers will be first in their natural order
		regardless any contain letters. For example:

		For this: List<String> hex = new Arrays.asList({"30", "8", "3A", "FF"});
		The output would be = {"30", "3A", "8", "FF"}

Arrays.asList(String names[])



11. ArraysList class:

	- ArrayList implements equality to mean the same elements in the same order are equals.
	
- Using generics instatiate you have add elements which are in  diamond operator <>.

-  al.indexOf(Object object);

- al.get(al.size()) - throw an IndexOutOfBoundsException at run time;

12. LocalDate API

	- You cannot get an instance from LocalDate. You must to use LocalDate.of(YYYY, MM, DD).
	- Month stars counting with 1 rather than 0(The old way using Calendar.FIELD used to start with 0).
	- A LocalDate does not have a time element. Therefore, it has no method to add hours and the code does not compile.
	- Dates are immutable. Therefore plus methods have to assign their return values or will be ignored. 
	- Different result of input on different format styles.
 
- java 8 introduces a new package java.time to deal with dates. The old classes such as java.util.Date are not recommended anymore.
  
  Briefly:
  
  java.time Package: This is the base package of new Java Date Time API. All the commonly used classes such as LocalDate, LocalTime, LocalDateTime, Instant, Period, Duration are part of this package. All of these classes are immutable and thread safe. 
  
  java.time.format Package: This package contains classes used for formatting and parsing date time objects such as java.time.format.DateTimeFormatter.
  
  (The following two are not important for the exam.)
  
  java.time.zone Package: This package contains classes for supporting different time zones and their rules.
  
  java.time.chrono Package: This package defines generic APIs for non ISO calendar systems. We can extend AbstractChronology class to create our own calendar system.
  java.time.temporal Package: This package contains temporal objects and we can use it for find out specific date or time related to date/time object. For example, we can use these to find out the first or last day of the month. You can identify these methods easily because they always have format “withXXX”. 
	
13. LocalDateTime API (TODO: For study it)


- Note that LocalDateTime class does not contain Zone information but ISO_ZONED_DATE_TIME requires it.
   Thus, it will throw the following exception:

``` 
public String getDateString(LocalDateTime ldt){
    return DateTimeFormatter.ISO_ZONED_DATE_TIME.format(ldt); 
}
``` 
The code will compile but will always throw a DateTimeException (or its subclass) at run time.


14. Period

	- Doesn't Allow method chaining. Because it always create a new object in its way. 
	For example: 
		Period period = Period.ofDays(5).ofYears(5);
	Will just stay with: 5 years. Because the method works in this way

		return create(years, 0, 0);:


- new String[2], you create a String array of two elements. arr is therefore not nul. concat(str+" "+ind); ), it will generate a NullPointerException, which is a RuntimeException.

- boolean equals(Object o); So it can take any object. The equals methods of all wrapper classes first check if the two object are of same class or not. If not, they immediately return false.



```
String substring(int beginIndex) 
          Returns a new string that is a substring of this string. 
          
String substring(int beginIndex, int endIndex) 
          Returns a new string that is a substring of this string. 


int indexOf(int ch) 
          Returns the index within this string of the first occurrence of the specified character. 
          
 int indexOf(int ch, int fromIndex) 
          Returns the index within this string of the first occurrence of the specified character,
           starting the search at the specified index. 
           
 int indexOf(String str) 
          Returns the index within this string of the first occurrence of the specified substring. 
          
 int indexOf(String str, int fromIndex) 
          Returns the index within this string of the first occurrence of the specified substring,
           starting at the specified index. 
           
           
           
  public StringBuilder delete(int start, int end)   
  Removes the characters in a substring of this sequence. 
  The substring begins at the specified start and extends to the character at index end - 1 
  or to the end of the sequence if no such character exists. 
  If start is equal to end, no changes are made.
```


- Empty string builder: myStringBuilder.delete(0, myStringBuilder.length()); (sb.setLength(0); ) 


-  JavaDoc description of java.time.temporal.TemporalAdjusters is very helpful: 

 Adjusters are a key tool for modifying temporal objects. They exist to externalize the process of adjustment, permitting different approaches, as per the strategy design pattern.
  Examples might be an adjuster that sets the date avoiding weekends, or one that sets the date to the last day of the month. There are two equivalent ways of using a TemporalAdjuster. 
  
  ```
  
  The first is to invoke the method on the interface directly. 
  The second is to use Temporal.with(TemporalAdjuster):    // these two lines are equivalent, but the second approach is recommended   
   temporal = thisAdjuster.adjustInto(temporal);
   temporal = temporal.with(thisAdjuster);
   
 1) System.out.println(LocalDate.now().with(TemporalAdjusters.next(DayOfWeek.TUESDAY)));
 
  2) System.out.println(TemporalAdjusters.next(DayOfWeek.TUESDAY).adjustInto(LocalDate.now()));
  
```
