---
title: OCP Review 4 - Functional programing 
layout: post
tags: [java, ocp, functional]
date: 2018-09-07
---

# Functional Programming

---

## Built-in functional interfaces

|Functional interface | Method |
| --- | --- |
| Supplier<T> | T get(); |
| Consumer<T> | void accept(T t); |
|  BiConsumer<T, U> |     void accept(T t, U u); |
| Predicate<T> |     boolean test(T t); |
| BiPredicate<T, U> | boolean test(T t, U u); |
| Function<T, R> |     R apply(T t); |
| BiFunction<T, U, R> |     R apply(T t, U u); |
| UnaryOperator<T> extends Function<T, T> | T apply(T t); |
| BinaryOperator<T> extends BiFunction<T,T,T> |  T apply(T t, T u); |


---

|Functional interface | Method |
| --- | --- |
| Supplier<T> | T get(); |


```java
interface Stackable<T> {
    void push(T element);
    T pop();
    T peek(); <-- Supplier!
    int size();
}

class Stack<T> implements Stackable<T> {
    private List<T> stack = new ArrayList<>();

// more code

    @Override
    public T peek() {
        if (stack.size() == 0) {
            return null;
        }

        T topElement = stack.get(stack.size() - 1);

        return topElement;
    }
}
```

---

## Suppliers examples

- Supplier: a chunk of code that holds and return a T

```java
Supplier<String> s1 = () -> "hello";
Supplier<String> s2 = String::new;

System.out.println(s1.get());
System.out.println(s2.get());
```

```java
class Stack<T> implements Supplier<T> {
    @Override
    public T get() {
        return null;
    }
}
```

---

|Functional interface | Method |
| --- | --- |
| Consumer<T> | void accept(T t); |

```java
Consumer<String> cs1 = System.out::print;
Consumer<Integer> ci1 = i -> {};

cs1.accept("Hello");
ci1.accept(10);
```

---

|Functional interface | Method |
| --- | --- |
|  BiConsumer<T, U> |     void accept(T t, U u); |

```java
HashMap<Integer, String> map = new HashMap<>();
map.put(1, "Diego");

BiConsumer<Integer, String > bc1 = (Integer i, String s) -> {};
BiConsumer<Integer, String > bc2 = map::put;
BiConsumer<Integer, String > bc3 = map::replace;

bc1.accept(2, "Hello");
bc2.accept(3, "World");
bc3.accept(3, "Nice!");

map.values().forEach(System.out::println);
```

prints:
```
Diego
Nice!
```

---

|Functional interface | Method |
| --- | --- |
| Predicate<T> |     boolean test(T t); |

- takes one value of type T, then test it -> Boolean

```java
Predicate<String> ps1 = String::isEmpty;
Predicate<String> ps2 = s -> s.contains("hello");

System.out.println(ps1.test("Diego") && ps2.test("Hello"));
```

---

## Predicates rule

- default implementations in `Predicate`

```java
Person p1 = new Person("Diego", 10_000);
Person p2 = new Person("Exploited Intern", 0);

Predicate<Person> hasSalary = person -> person.getSalary() > 0;
Predicate<Person> hasName = person -> !person.getName().isEmpty();

Predicate<Person> isValidEmployee = hasName.and(hasSalary);


if (isValidEmployee.test(p1)) {
    
}
```


---

|Functional interface | Method |
| --- | --- |
| BiPredicate<T, U> | boolean test(T t, U u); |

- is a test with two parameters

```java
BiPredicate<String, Integer> bp1 = (String s, Integer i) -> { return s.length() == i; };
bp1.test("Hello", 10);

BiPredicate<String, String> isEqual = (s, t) -> s.equals(t);
System.out.println(isEqual.test("pepe", "Manolo"));
```

---

|Functional interface | Method |
| --- | --- |
| Function<T, R> |     R apply(T t); |

```java
class Executor<T, U> {
    public U execute(T value, Function<T, U> function) {
        return function.apply(value);
    }
}

Executor<String, Integer> lengthCounter = new Executor<>();
Integer result = lengthCounter.execute("Hello", s -> s.length());
System.out.println(result);

```
---

```java
result = lengthCounter.execute("Hello", new VowelCounter()::countVowels);
System.out.println(result);

result = lengthCounter.execute("Hello", VowelCounter::staticCountVowels);
System.out.println(result);

class VowelCounter {
    public Integer countVowels(String s) {
        int vowelCount = 0;
        for (int i = 0; i < s.length(); ++i) {
            switch(s.charAt(i)) {
                case 'a':
                case 'e':
                case 'i':
                case 'o':
                case 'u':
                    vowelCount++;
                    break;
                default:
                    // do nothing
            }
        }
        return vowelCount;
    }

    public static Integer staticCountVowels(String s) {
        int vowelCount = 0;
        for (int i = 0; i < s.length(); ++i) {
            switch(s.charAt(i)) {
                case 'a':
                case 'e':
                case 'i':
                case 'o':
                case 'u':
                    vowelCount++;
                    break;
                default:
                    // do nothing
            }
        }
        return vowelCount;
    }
}

```


---


|Functional interface | Method |
| --- | --- |
| BiFunction<T, U, R> |     R apply(T t, U u); |

- is a function, taking two parameters

```java
// re-implementing a Predicate
BiFunction<String, String, Boolean> isEqualBiFunction= (s, t) -> s.equals(t);
System.out.println(isEqualBiFunction.apply("Hello", "Hello"));
```

---

|Functional interface | Method |
| --- | --- |
| UnaryOperator<T> extends Function<T, T> | T apply(T t); |

```java
UnaryOperator<String> inc = s -> s + UUID.randomUUID();
System.out.println(inc.apply("Hello-"));
```

---

|Functional interface | Method |
| --- | --- |
| BinaryOperator<T> extends BiFunction<T,T,T> |  T apply(T t, T u); |

```java 
Calculator<Integer> ci = new Calculator<>();
ci.calculate(1, 2, (n1, n2) -> n1 + n2);

class Calculator<T extends Number> {
    public T calculate(T n1, T n2, BinaryOperator<T> operator) {
        return operator.apply(n1, n2);
    }
}
```

---

## Our own Functional interfaces: Tuples!

```java
class Downloader {
    public Tuple<String, Exception> download(String url) {
        Tuple<String, Exception> t = new Tuple<>("Hello", new IllegalArgumentException());
        return t;
    }
}

class Tuple<T, S> implements Supplier<Tuple<T, S>> {

    private T t;
    private S s;

    public Tuple(T t, S s) {
        this.t = t;
        this.s = s;
    }

    @Override
    public Tuple<T, S> get() {
        return this;
    }

    public T getFirstElement() {
        return t;
    }

    public S getSecondElement() {
        return s;
    }
}
```

---




## Optionals

- represent the absence of some data
- latitude: 0 is a valid value. When created we want to know if we have set it or not
- in package `java.util.Optional`
- would have been nice to have non-Optionals...

---

## Optionals: create

- create an Optional __containing a value__

```java
Optional<String> name = Optional.of("Diego");

System.out.println(name);       // prints "Optional[Diego]"
System.out.println(name.get()); // prints "Diego", using Consumer
```

- create an __Empty Optional__

```java
name = Optional.empty();    // represents "nothing"
System.out.println(name);   // prints "Optional.empty"
```

---

## Optionals: create from possible null values

```java

Optional<String> nullString = Optional.of(null);

System.out.println(nullString.isPresent()); // crash

Optional<String> nullString = Optional.ofNullable(null);

System.out.println(nullString.isPresent()); // don't crash
```

---

## Optionals: check if Empty

```java
if (name.isPresent()) {
    System.out.println("Yes");
} else {
    System.out.println("No");
}
```

---

## Optionals: get value

- using `get`

```java
Optional<String> name = Optional.of("Diego");

System.out.println(name.get()); // prints "Diego", using Consumer FI
```

---

## Optionals: get value

- using `orElse` and `orElseGet`

```java 
System.out.println(name.orElse("Groucho"));
System.out.println(name.orElseGet(() -> "nothing".toUpperCase()));  // Supplier functional Interface
System.out.println(name.orElseGet(() -> UUID.randomUUID().toString()));

// name still not mutated
System.out.println(name);   // prints "Optional.empty"
```

---

## Optionals: get value

- throw Exception if Empty

```java
name.orElseThrow(() -> new IllegalStateException("Empty name!"));
```


---

## Optionals: example

```java
@FunctionalInterface interface DownloadFinished {
    void execute(Optional<String> json, Optional<String> errorDescription);
}

interface DownloadManager {
    void downloadJson(DownloadFinished completionLambda);
}

class DownloadManagerImpl implements DownloadManager {
    public void downloadJson(DownloadFinished completionLambda) {
        try {
            Thread.sleep(3000);         // fake download something from Internet
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        completionLambda.execute(Optional.of("Json"), Optional.empty());
    }
}

```

---

## Optionals: example

```java
new DownloadManagerImpl().downloadJson((json, errorDescription) -> {
    String jsonResult = null;
    if (errorDescription.isPresent()) {

    } else {
        jsonResult = json.orElseGet(new FakeJson()::get);
    }

    System.out.println(jsonResult);
});

// ...

class FakeJson implements Supplier<String> {
    @Override
    public String get() {
        return "{ name: Fake Json}";
    }
}
```

---

## Using Optionals with Streams

```java
Optional<String> s = names.stream().reduce((s1, s2) -> { return s1+s2; });
		
if (s.isPresent()) {
	System.out.println(s.get());
}

```

---

## Streams

- "sequence" of elements
- lazily created
- can be finite (created from Array or List) or infinite (generated)

---


## Creating finite Streams

```java
Stream<String> ss = Stream.of("Hello", "World");
System.out.println(ss.count()); // terminal operation

ss = Stream.of("Hello", "World");
System.out.println(ss.findFirst());

ss = Stream.of("Hello", "World");
System.out.println(ss.findFirst().isPresent());

ss = Stream.of("Hello", "World");
System.out.println(ss.findFirst().get()); // which Functional Interface?

```

---

## Infinite Streams

- we can't create infinite List
- an infinite Stream never ends (need to interrupt the program)

```java
Stream<String> uuids = Stream.generate(() -> UUID.randomUUID().toString());

uuids.forEach(System.out::print);
```

---

## Infinite Streams

```java
Stream<String> uuids = Stream.generate(() -> {
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    return UUID.randomUUID().toString();
});

uuids.forEach(System.out::println);


```

- to limit infinite Streams: http://stackoverflow.com/questions/20746429/limit-a-stream-by-a-predicate

---

## Streams

```java
Stream<Person> f = people.stream().filter((Person p) -> {return p.isHappy() == true;});
f.forEach(p -> System.out.println(p.getName()));
```

---

## Operations: Terminal & Intermediate

- Terminal
    - allMatch() / anyMatch() / noneMatch()
    - collect()
    - count()
    - findAny() / findFirst()
    - forEach()
    - min() / max()
    - reduce()

---

## Count

```java
Stream<Person> apple = Stream.of(new Person("Diego", 40), new Person("Tim", 55));
System.out.println(apple.count());

// ...

class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```

---

## Min & Max

- takes a comparator
- Min & Max == reduce (only return one value)

```java
Stream<Person> apple = Stream.of(new Person("Diego", 40), new Person("Tim", 55), new Person("Becario", 25));

apple.min((o1, o2) -> o1.getAge()-o2.getAge()).ifPresent(person -> {
    System.out.println(person.getName());
});

```

---

## FindAny / FindFirst

- terminal operations, but not reductions
- do not produce a value after looking at the whole Stream

```java
Stream<Person> apple = Stream.of(new Person("Diego", 40), new Person("Tim", 55), new Person("Becario", 25));

apple.findAny().ifPresent(person -> System.out.println(person.getName()));
```


---

## AllMatch / AnyMatch

```java
// all people > 18?
System.out.println(apple.allMatch(person -> person.getAge() > 18));     // true

// any person > 50?
System.out.println(apple.anyMatch(person -> person.getAge() > 50));     // true

// none person older than 50?
System.out.println(apple.noneMatch(person -> person.getAge() > 50));

// cleaner & reusable

Predicate<Person> olderThat50 = p -> p.getAge() > 50;
System.out.println(apple.noneMatch(olderThat50));

```

---

## ForEach


---

## Reduce

T reduce(T identity, BinaryOperator<T> accumulator);


```java
Stream<String> tokens = Stream.of("Hello", "World!", "tokens");
System.out.println(tokens.reduce("", (s, s2) -> s.concat(s2)));

// can omit initial value & use method reference
System.out.println(tokens.reduce(String::concat));
```

---

## Collect

- mutable reduction: same object reused to do the reduction
- 1st parameter: (supplier) the mutable object we're to use (we build it here with a `Supplier`)
- 2nd parameter: (accumulator) how we reduce two elements in the list
- 3rd parameter: (combiner) if we run this in parallel, and have 2 data collections, how we merge them

```java
StringBuilder result = tokens.collect(StringBuilder::new, StringBuilder::append, StringBuilder::append);
System.out.println(result);

// other way
StringBuilder result = tokens.collect(StringBuilder::new, (s, t) -> s.append(t), StringBuilder::append);
System.out.println(result);

```

---

## Collect explained

```java
StringBuilder result = tokens.collect(() -> 
        { return new StringBuilder(); },                            // supplier () -> StringBuilder
        (StringBuilder sb, String ss) -> sb.append(ss),             // accumulator (StringBuilder, String) -> StringBuilder 
        (StringBuilder s, StringBuilder t) -> s.append(t.reverse()) // StringBuilder, StringBuilder
);
System.out.println(result);
```

---

## Intermediate operations

- filter
- distinct
- limit / skip
- map

---

## Filter

- filter elements that return true for the Predicate

```java
Stream<Integer> numbers = Stream.of(10, 20, 30, 40);
numbers.filter(n -> n > 20).forEach(System.out::print);

Stream<Person> apple = Stream.of(new Person("Diego", 40), new Person("Tim", 55), new Person("Becario", 25), new Person("", 80));
apple.filter(person -> person.getName().isEmpty()).forEach(person -> System.out.println(person.getAge()));  // prints 80

```

---

## Distinct

- calls `equals`

```java
 Stream<Person> apple = Stream.of(new Person("Diego", 40), new Person("Tim", 55), new Person("Tim", 55), new Person("", 80));
apple.filter(person -> !person.getName().isEmpty()).distinct().forEach(person -> System.out.println(person.getAge()));
// not working as expected if we don't override equals
        
```

---

## limt & skip

```java
Stream<Integer> iterate = Stream.iterate(1, n -> n + 2);
iterate.limit(100).forEach(System.out::println);

 Stream<Integer> iterate = Stream.iterate(1, n -> n + 2);
iterate.limit(100).skip(10).forEach(System.out::println);
```


---


## map

```java
List<String> names = Arrays.asList("Grouch", "Chicc", "Harp");

// forEach to print
names.stream().forEach(e -> System.out.println(e ));

names.stream().map((e) -> e + "o").forEach(e -> System.out.printf(e + "\n"));
names.stream().map((e) -> e.toUpperCase()).forEach(e -> System.out.printf(e + "\n"));

```


---

## flatmap

```java
List<String> lotr = new ArrayList<>();
lotr.add("Aragorn");
lotr.add("Bilbo");
lotr.add("Gollum");

List<String> matrix = new ArrayList<>();
matrix.add("Neo");
matrix.add("Trinity");

Stream<List<String>> films = Stream.of(lotr, matrix);
//films.forEach(System.out::println);

films.flatMap(l -> l.stream()).forEach(s -> System.out.print(s.toUpperCase()));

```

---

## sorted

```java
Stream<Integer> numbers = Stream.of(3, 20, -1, 40);

numbers.sorted().forEach(System.out::println);
```

---

## peek

```java

Stream<Integer> iterate = Stream.iterate(1, n -> n + 1);
iterate.limit(100).skip(10).peek(System.out::print).forEach(i -> System.out.print(" " + i + " "));
```

---

## Primitive Streams

- IntStream
- LongStream
- DoubleStream

```java
IntStream is = IntStream.of(1, 2, 3, 4);
System.out.println(is.average());
```

---

## Creating primitive Streams

```java
LongStream ls = LongStream.empty();

DoubleStream randomStream = DoubleStream.generate(Math::random).limit(30);
randomStream.forEach(System.out::println);
```

---

## Ranges

```java
LongStream ls = LongStream.range(1, 10);    // 1...10, 1..<9
ls.forEach(System.out::println);

LongStream ls = LongStream.rangeClosed(1, 10);  // 1...11, 1..<10
ls.forEach(System.out::println);
```

---

## Summary statistics

```java
IntStream is = IntStream.of(1, 2, 3, 4);
is.summaryStatistics().getMax();
```



### Links Functional Programming

- [Built-in Functional Interfaces](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/BuiltInFunctionalInterfaces.java) <br />
- [Returning an Optional](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/ReturningAnOptional.java) <br />
- [Stream Common Intermediate Oprations](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/StreamCommonIntermediateOprations.java) <br />
- [Stream Common Terminal Operations](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/StreamCommonTerminalOperations.java) <br />
- [Stream Sources](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/StreamSources.java) <br />
- [Stream Working with Primitives](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/StreamWorkingWithPrimitives.java) <br />
- [Using Variables in Lambdas](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/UsingVariablesInLambdas.java) <br />
- [Working with Advanced Stream Pipeline Concepts](https://github.com/acailic/java8-learning/blob/master/Java-8/src/functionalProgramming/WorkingWithAdvancedStreamPipelineConcepts.java) <br />



### Questions

- A lambda expression in a method can make use of a local variable if it is declared as final or if it is effectively final. JLS 8 describes "effectively final" as follows:
 A local variable or a method, constructor, lambda, or exception parameter is effectively final if it is not declared final but it never occurs as the left hand operand of an assignment operator or as the operand of a prefix or postfix increment or decrement operator.
 In addition, a local variable whose declaration lacks an initializer is effectively final if all of the following are true:
   • It is not declared final.
   • Whenever it occurs as the left-hand operand of an assignment operator, it is definitely unassigned and not definitely assigned before the assignment; that is, it is definitely unassigned and not definitely assigned after the right-hand operand of the assignment.
   • It never occurs as the operand of a prefix or postfix increment or decrement operator.
If a variable is effectively final, adding the final modifier to its declaration will not introduce any compile-time errors. Conversely, a local variable or parameter that is declared final in a valid program becomes effectively final if the final modifier is removed.



- The flatMap method expects a Function that will take an element and create a Stream out of it. It then joins each of those streams (one stream for each element in the original stream) to return a big combined stream of elements.
 bs->bs.stream() correctly captures such a Function.  
The mapToDouble method expects a ToDoubleFunction object that will take an argument and return a double. It then returns a DoubleStream containing double primitives. 
 DoubleStream has method sum that simply returns the sum of all the elements.
 
- java.util.BiFunction - 
 1. It is a function that accepts two arguments and produces a result.
 2. The types of the arguments and the return value can all be different. 
 3. public V compute(K key, BiFunction<? super K,? super V,? extends V> remappingFunction)
 4. public V computeIfAbsent(K key, Function<? super K,? extends V> mappingFunction)
 5. public V computeIfPresent(K key, BiFunction<? super K,? super V,? extends V> remappingFunction)
 
 
-  IntFunction - regular functional interfaces by parameterizing them to Integer is inefficient as compared to using specially designed interfaces for primitives because they avoid the cost of boxing and unboxing the primitives. 
   Now, since the problem statement requires something to be returned after processing each int, you need to use a Function instead of a Consumer or a Predicate.
- primitive specialized versions of streams such as IntStream, DoubleStream, and LongStream.
- java.util.function.Supplier is a functional interface and has only one method named get. It doesn't have getAsDouble. Therefore, this code will not compile.  
 
- function.DoubleSupplier (and other similar Suppliers such as IntSupplier and LongSupplier) is a functional interface with the functional method named getAsDouble. The return type of this method is a primitive double (not Double). Therefore, if your lambda expression for this function returns a Double, it will automatically be converted into a double because of auto-unboxing. However, if your expression returns a null, a NullPointerException will be thrown. 



- distinction between "intermediate" and "terminal" operations and which operations of Stream are "intermediate" operations.
  
  A Stream supports several operations and these operations are divided into intermediate and terminal operations. The distinction between an intermediate operation and a termination operation is that an intermediate operation is lazy while a terminal operation is not. When you invoke an intermediate operation on a stream, the operation is not executed immediately. It is executed only when a terminal operation is invoked on that stream. In a way, an intermediate operation is memorized and is recalled as soon as a terminal operation is invoked. You can chain multiple intermediate operations and none of them will do anything until you invoke a terminal operation, at which time, all of the intermediate operations that you invoked earlier will be invoked along with the terminal operation.
  
  You should read more about this here: http://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#StreamOps
  
  It is easy to identify which operations are intermediate and which are terminal. All intermediate operations return Stream (that means, they can be chained), while terminal operations don't.
  
  filter, peek, and map are intermediate operations. Since the code does not invoke any terminal operation on the stream, the calls to these intermediate method do nothing. Therefore, no output is produced by the given code. 
  
  count, forEach, sum, allMatch, noneMatch, anyMatch, findFirst, and findAny are terminal operations.
  
- public interface UnaryOperator<T> extends Function<T,T> Represents an operation on a single operand that produces a result of the same type as its operand. This is a specialization of Function for the case where the operand and result are of the same type.

- most one terminal operation on a stream and that too at the end.


- java.util.function.Supplier is a functional interface and has only one method named get. It doesn't have getAsDouble. Therefore, this code will not compile.

- A reduction operation (also called a fold) takes a sequence of input elements and combines them into a single summary result by repeated application of a combining operation, such as finding the sum or maximum of a set of numbers, or accumulating elements into a list. The streams classes have multiple forms of general reduction operations, called reduce() and collect(), as well as multiple specialized reduction forms such as sum(), max(), or count(). 


- allMatch is a terminal operation that returns a boolean. You cannot chain any operation after that. If you had just this :

-  Collectors.partitioningBy method. This method takes a Predicate and returns Collector that distributes the elements of the stream into two groups - one containing elements for which the Predicate returns true, and another containing elements for which the Predicate returns false. The return type is a Map containing two keys - true and false and the values are Lists of the elements.  IntStream.rangeClosed(10, 15) creates an IntStream of int primitives containing elements 10, 11, 12, 13, 14, and 15 (Observe that 15 is included). IntStream does not support the various collect methods supported by a regular Stream of objects. But it does support a boxed() method that returns a Stream<Integer> containing Integer objects.
	
- 'Stream.of(l1, l2).flatMap((x)->x.stream()).forEach((x)->System.out.println(x));' The objective of flatMap is to take each element of the current stream and replace that element with elements contained in the stream returned by the Function that is passed as an argument to flatMap. It is perfect for the requirement of this question. You have a stream that contains Lists.  So you need a Function object that converts a List into a Stream of elements. Now, List does have a method named stream() that does just that. It generates a stream of its elements. Therefore, the lambda expression x->x.stream() can be used here to create the Function object.	

- Stream pipelines may execute either sequentially or in parallel. This execution mode is a property of the stream. 

Streams are created with an initial choice of sequential or parallel execution. (For example, Collection.stream() creates a sequential stream, and Collection.parallelStream() creates a parallel one.)  This choice of execution mode may be modified by the BaseStream.sequential() or BaseStream.parallel() methods. 

It is not documented by Oracle exactly what happens when you change the stream execution mode multiple times in a pipeline. It is not clear whether it is the last change that matters or whether operations invoked after calling () parallel can be executed in parallel and operations invoked after calling sequential() will be executed sequentially.


- Optional class: 
1. Optional has a static method named of(T t) that returns an Optional object containing the value passed as argument. It will throw NullPointerException if you pass null. If you want to avoid NullPointerException, you should use Optional.ofNullable(T t) method. This will return Optional.empty if you pass null.
2. You cannot change the contents of Optional object after creation. Optional does not have a set method. Therefore, grade.of, although technically correct, will not actually change the Optional object referred to by grade. It will return a new Optional object containing the passed argument. 
3. The orElse method returns the actual object contained inside the Optional or the argument passed to this method if the Optional is empty. It does not return an Optional object. Therefore, print(grade1.orElse("UNKNOWN")) will print UNKNOWN and not Optional[UNKNOWN].  4. isPresent() returns true if the Optional contains a value, false otherwise. 
5. ifPresent(Consumer ) executes the Consumer object with the value if the Optional contains a value. Not that it is the value contained in the Optional that is passed to the Consumer and not the Optional itself.


- forEach is a terminal operation. It returns void. This means that you cannot chain methods after calling forEach. 

- java.util.function.DoubleSupplier (and other similar Suppliers such as IntSupplier and LongSupplier) is a functional interface with the functional method named getAsDouble. The return type of this method is a primitive double (not Double). Therefore, if your lambda expression for this function returns a Double, it will automatically be converted into a double because of auto-unboxing. However, if your expression returns a null, a NullPointerException will be thrown.

- Stream operators explanation: 

1. The reduce method needs a BinaryOperator. This interface is meant to consume two arguments and produce one output. It is applied repeatedly on the elements in the stream until only one element is left. The first argument is used to provide an initial value to start the process. (If you don't pass this argument, a different reduce method will be invoked and that returns an Optional object. ) 
2. The Stream.max method requires a Comparator. All you need to implement this interface using a lambda expression is a reference to any method that takes two arguments and returns an int. The name of the method doesn't matter. That is why it is possible to pass the reference of Integer's max method as an argument to Stream's max method. However, Integer.max works very differently from Integer.compare. The max method returns the maximum of two numbers while the compare method returns a difference between two numbers. Therefore, when you pass Integer::max to Stream's max, you will not get the correct maximum element from the stream. That is why //2 will compile but will not work correctly.


- Since we are creating a parallel stream, it is possible for both the elements of the stream to be processed by two different threads. In this case, the identity argument will be used to reduce both the elements. Thus, it will print _a_b.
  It is also possible that the result of the first reduction ( _a ) is reduced further using the second element (b). In this case, it will print _ab. 
  
  Even though the elements may be processed out of order individualy in different threads, the final output will be produced by joining the individual reduction results in the same order. Thus, the output can never have b before a.
  
  
  
- Since the first stream is made parallel, it may be partitioned into multiple pieces and each piece may be processed by a different thread. findAny may, therefore, return a value from any of those partitions. Hence, any number from 13 to 15 may be printed. 
  
  The second stream is sequential and therefore, ideally, findAny should return the first element. However, findAny is deliberately designed to be non-deterministic. Its API specifically says that it may return any element from the stream. If you want to select the first element, you should use findFirst.
  
- class A is not static inner class of TestClass. So it cannot exist without an outer instance of TestClass. class B is static inner class and can be instantiated without an outer instance of TestClass. Now, the method useClasses() is an instance method. So, it already has instance of outer class associated with it. So, new A(); is also valid. new TestClass.A(); is same as new A().
  Although not related to this question, FYI, an anonymous class can never be static. Even if created in a static method.  
  
- TestClass is also a Thread because it extends Thread. The first Thread object is created at :
 TestClass tc = new TestClass(); The second Thread object is created at : new Thread( tc.new Runner() ).
 Runner is not a Thread object. Thread[] t = new Thread[5]; does not create any thread object. It only creates an array that can hold 5 Thread objects. Each of those 5 references are initialized to null.
 tc.new Runner() is a valid construct because Runner is a non-static inner class. So you cannot do 'new TestClass.Runner()'.
 
 
 - You can make any class abstract by adding abstract to its declaration. For example: public abstract class X{ }