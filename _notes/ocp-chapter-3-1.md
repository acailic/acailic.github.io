---
title: OCP Review 3.1 - Collections 
layout: post
tags: [java, ocp, collections]
date: 2018-09-07
---
# Arrays & ArrayLists
---
## ArrayList

- extends from `AbstractList` and `AbstractCollection`
- ArrayList can contain only objects (not primitives)
- how can I have a list of `boolean`? Using wrapper class `Boolean`

---

## Arrays

- bad creation

```
String[] pokemon = new String[0];
pokemon[0] = "Charmander"; // Error: ArrayIndexOutOfBoundsException: 0
pokemon[1] = "GeoDude";
```

- good one

```java

String[] pokemon = new String[2];
pokemon[0] = "Charmander";
pokemon[1] = "GeoDude";

// or this one

String[] pokemon2 = new String[] {
    "Charmander", 
    "GeoDude"
};

// also

String[] pokemon3 = {
    "Charmander",
    "GeoDude"
};

```

---

## Spot why these are wrong

```java
String[] pokemon2 = new String[]() {
    "Charmander",
    "GeoDude"
};

String[] pokemon2 = new String[](2) {
    "Charmander",
    "GeoDude"
};

String[2] pokemon2 = new String[] {
    "Charmander",
    "GeoDude"
};

String pokemon2[2] = new String[] {
    "Charmander",
    "GeoDude"
};

```

---

## Watch out!

```java
List<String> names = Arrays.asList("groucho", "harpo", "chicco");
        
for (String s: names) {
    System.out.println(s);
}

names.add("zeppo"); // crash: can't add more elements to this list
// asList: Returns a fixed-size list backed by the specified array
```

---

## Watch out! 

```java
 String[] pokemon = new String[2];
pokemon[0] = "Charmander";
pokemon[1] = "GeoDude";


// nice typed List
List<String> names = Arrays.asList(pokemon);

for (String s: names) {
    System.out.println(s);
}

pokemon[0] = "Pikachu";

// after changing element 0 in array, list also changes !!

for (String s: names) {
    System.out.println(s);
}
```


---

## Searching & Sorting

```java
int[] numbers = {9, 1, 4, 6};

Arrays.sort(numbers);

for (int n: numbers) {  // arrays get modified
    System.out.print(n + "-");
}
System.out.println("");

System.out.println(Arrays.binarySearch(numbers, 4));
System.out.println(Arrays.binarySearch(numbers, 3));
System.out.println(Arrays.binarySearch(numbers, 5));
System.out.println(Arrays.binarySearch(numbers, 7));    // -4: 1-4-6- *7* -9
System.out.println(Arrays.binarySearch(numbers, 8));    // -4: 1-4-6- *8* -9
```

---

## Wrapper Classes

boolean	Boolean	
byte	Byte	
char	Character	
double	Double
float	Float
int	    Integer
long	Long
short	Short

---

## Diamond notation

```java
List<String> stringList = new ArrayList<>();
```

---

## Questions

- Arrays.binarySearch() method returns the index of the search key, if it is contained in the list; otherwise, (-(insertion point) - 1). 

- Deque is an important interface for the exam. To answer the questions, you must remember that a Deque can act as a Queue as well as a Stack. Based on this fact, you can deduce the following: 
1. Since Queue is a FIFO structure (First In First Out i.e. add to the end and remove from the front), it has methods offer(e)/add(e)(for adding an element to the end or tail) and poll()/remove()(for removing an element from the front or head) for this purpose.  Note that offer and add are similar while poll and remove are similar.
2. Since Stack is a LIFO structure (Last In First Out i.e. add to the front and remove from the front), it provides methods push(e) and pop() for this purpose, where push adds to the front and pop removes from the front.  
Besides the above methods, Deque also has variations of the above methods. But it is easy to figure out what they do:  pollFirst()/pollLast() - poll is a Queue method. Therefore pollFirst and pollLast will remove elements from the front and from the end respectively. removeFirst()/removeLast() - These are Deque specific methods. They will remove elements from the front and from the end respectively. These methods differ from pollFirst/pollLast only in that they throw an exception if this deque is empty.  

offerFirst(e)/offerLast(e) - offer is a Queue method. Therefore offerFirst and offerLast will add elements to the front and to the end respectively. addFirst(e)/addLast(e) - add is a Queue method. Therefore addFirst and addLast will add elements to the front and to the end respectively.  peek(), peekFirst(): return the first element from the front of the queue but does not remove it from the queue. peekLast() : returns the last element from the end of the queue but does not remove it from the queue. element(): retrieves, but does not remove, the head of the queue represented by this deque (in other words, the first element of this deque). This method differs from peek only in that it throws an exception if this deque is empty.


- There is an important concept to be understood here so please read this carefully:
Map<Object, ?> m = new LinkedHashMap<Object, Object>();  While this is a valid declaration, it will not allow you to put anything into 'm'. The reason is that m is declared to be of type Map that takes an instance of Object class as a key and instance of 'Unknown class' as value. Therefore, if you try to put an Object, or Integer, or anything, the compiler will not allow it because that 'Unknown' class is not necessarily Object or Integer or any other class.  Even though the actual object pointed to by 'm' is of type LinkedHashMap<Object, Object>, the compiler looks only at the reference type of the variable. Thus, 'm' is read-only. It would have worked if m were declared as Map<Object, Object> m = .... Because in this case the compiler *knows* that m can take an instance of Object as value. Instance of Object covers all kind of objects.  

Map<Object, ? super ArrayList> m = new LinkedHashMap<Object, ArrayList>();   You should read it aloud as follows: 'm' is declared to be of type Map that takes an instance of Object class as a key and an instance of 'a class that is either ArrayList or a superclass of Arraylist' as value. This means that the value can be an instance of ArrayList or its subclass (since an ArrayList object or its subclass object can be assigned to a reference of type ArrayList or its super class.). However, you cannot put Object (which is a superclass of ArrayList) in it because the compiler doesn't know the exact superclass that 'm' can take. It could be AbstractList, or Object, or any other super class of ArrayList. The compiler only knows that it is a superclass but not the exact type. So option 4 is correct but 5 is wrong. 

Thus, you can do: m.add(anyObj, new ArrayList());  Just the opposite of super is extends. Consider the following method:  public void m1(List<? extends Number> list) {    list.add(new Integer(10));  //Error at compile time because the compiler                        //only knows that list contains Number or its subclass objects. But it doesn't know the exact type.                             //Therefore, it will not allow you to add anything to it.     Number n = list.get(1);  //This will work because the compiler knows that every object in list IS-A Number.  }   


-   Map.Entry<K,V> pollFirstEntry() 
          Removes and returns a key-value mapping associated with the least key in this map, or null if the map is empty. 
 Map.Entry<K,V> pollLastEntry() 
          Removes and returns a key-value mapping associated with the greatest key in this map, or null if the map is empty. 
 NavigableMap<K,V> tailMap(K fromKey, boolean inclusive) 
          Returns a view of the portion of this map whose keys are greater than (or equal to, if inclusive is true) fromKey. 
-  A NavigableSet is a SortedSet extended with navigation methods reporting closest matches for given search targets. Methods lower, floor, ceiling, and higher return elements respectively less than, less than or equal, greater than or equal, and greater than a given element, returning null if there is no such element. 
Since NavigableSet is a SortedSet, it keeps the elements sorted.


- LinkedHashMap class maintains the elements in the order of their insertion time. This property can be used to build the required cache as follows:
    1. Insert the key-value pairs as you do normally where key will be the object identifier and value will be the object to be cached.
    2. When a key is requested, remove it from the LinkedHashMap and then insert it again. This will make sure that this pair marked as inserted latest.
    3. If the capacity is full, remove the first element.
    
    You cannot simply insert the key-value again. (without first removing it) because a reinsertion operation does not affect the position of the pair.


-   Deque can act as a Queue as well as a Stack. Based on this fact, you can deduce the following:
 1. Queue is a FIFO structure (First In First Out i.e. add to the end and remove from the front), it has methods offer(e)/add(e)(for adding an element to the end or tail) and poll()/remove()(for removing an element from the front or head) for this purpose.  Note that offer and add are similar while poll and remove are similar.
 2. Stack is a LIFO structure (Last In First Out i.e. add to the front and remove from the front), it provides methods push(e) and pop() for this purpose, where push adds to the front and pop removes from the front.
 pollFirst()/pollLast() - poll is a Queue method. Therefore pollFirst and pollLast will remove elements from the front and from the end respectively. removeFirst()/removeLast() - These are Deque specific methods. They will remove elements from the front and from the end respectively. These methods differ from pollFirst/pollLast only in that they throw an exception if this deque is empty.  offerFirst(e)/offerLast(e) - offer is a Queue method. Therefore offerFirst and offerLast will add elements to the front and to the end respectively. addFirst(e)/addLast(e) - add is a Queue method. Therefore addFirst and addLast will add elements to the front and to the end respectively.  peek(), peekFirst(): return the first element from the front of the queue but does not remove it from the queue. peekLast() : returns the last element from the end of the queue but does not remove it from the queue. element(): retrieves, but does not remove, the head of the queue represented by this deque (in other words, the first element of this deque). This method differs from peek only in that it throws an exception if this deque is empty.
 
- `
The signature of a method :
 <E extends CharSequence> List<? super E> doIt(List<E> nums) 
result = doIt(in);
Given that String implements CharSequence interface, what should be the reference type of 'in' and 'result' variables?`
The input parameter has been specified as List<E>, where E has to be some class that extends CharSequence. So ArrayList<String>, List<String>, or List<CharSequence> are all valid as reference types for 'in'.  The output type of the method has been specified as List<? super E> , which means that it is a List that contains objects of some class that is a super class of E. Here, E will be typed to whatever is being used for 'in'. For example, if you declare ArrayList<String> in, E will be String
Concept here once the method returns, there is no way to know what is the exact class of objects stored in the returned List. 
So you cannot declare out in a way that ties it to any particular class, not even Object.  Thus, the only way to accomplish this is to either use non-typed reference type, such as:  List result; or use the same type as the return type mentioned in the method signature i.e. List<? super String> (because E will be bound to String in this case.)

 
 
   