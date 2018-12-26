---
title: OCP Review 8 - IO
layout: post
tags: [java, ocp, io]
date: 2018-09-06
---
 
# IO

---

## Review

- __File__
    - some data
    - can be empty
    - can't contain other files / dirs
- __Directory__
    - _file_ containing a list of files / dirs
    - can contain other files / dirs
    - can be empty

---

```
[dfreniche@Tesla log]$ tree /var/log
.
├── Bluetooth
├── CDIS.custom
├── CoreDuet
├── DiagnosticMessages
│   ├── 2017.04.19.asl
│   └── StoreData
├── alf.log
├── apache2
├── appfirewall.log
├── asl
│   ├── 2017.04.12.G80.asl
│   ├── BB.2018.04.30.G80.asl
│   ├── Logs
│   │   ├── aslmanager.20170417T193440+02
│   │   └── aslmanager.20170419T084317+02
│   └── StoreData
├── com.apple.revisiond [error opening dir]
├── com.apple.xpc.launchd
├── corecaptured.log
├── cups
│   ├── access_log
│   ├── error_log
│   └── page_log
├── daily.out
├── wifi.log.0.bz2
└── wifi.log.1.bz2

```

---

## File System: Paths

- __relative__

 `mydir`, `./thisDir`, `bin/bash`

- __absolute__: start with the root directory

 `c:\\windows`, `/bin`

---

## File: `java.io.File`

- reads information about existing files & directories
- needed to create, delete, rename files, check directory contents... 
- needs a Path to be created

```java
File f = new File("/bin");

System.out.println("Getname:" + f.getName());
System.out.println("Canwrite: " + f.canWrite());
System.out.println("Exists:" + f.exists());
```

---

## Absolute vs Relative paths

- relative Path to our project / app

```java
File f = new File("bin");   
```

- absolute Path

```java
File f = new File("/bin");   
```

- we can create a file starting with a base path

```java
File root = new File("/");
File f = new File(root, "bin");
```

---

## System-independent paths

- `File.separator`: character used to separate parts of a relative / absolute path
    - on UNIX systems, __"/"__, on Windows __"\\"__
    - `/bin/ls`, `c:\\windows\\system32`
- `File.pathSeparator`: character used to separate directories in the `PATH` environment variable 😱
    - on UNIX systems, __":"__, on Windows __";"__

```java
File root = new File(File.separator);
File f = new File(root, "bin");
```

---

## Useful methods on File class

|   |   |
|---|---|
| exists() |          delete() |
| getName() |              lastModified() |
| getAbsolutePath() |      renameTo(File) |
| isDirectory()         | mkdir() |
| isFile()              | mkdirs() |
| length                | getParent() |
| listFiles()

---

## Stream magic

```java
Arrays.stream(f.listFiles()).sorted().forEach(System.out::println);
```

---

## Reading from the keyboard: old way

- `System.in` is a `java.io.BufferedInputStream`
    - raw stream of bytes
- to read from it, we need an `InputStreamReader`

```java
InputStreamReader in = new InputStreamReader(System.in);
int c = 0;
do {
    c = in.read();
    System.out.println(c);
} while (c != 32);
```

---

## Reading from the keyboard: old way

- this reads a _stream_ of bytes, one at a time. We need to read complete lines!
- to read complete lines, we use a `BufferedReader`

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

String input = reader.readLine();
```

--- 

## Reading from the keyboard: "new" way

- since Java 6. New way... 🤔
- `Console` singleton class
- `Console` can be null!

```java
Console console = System.console();
if (console == null) {
    System.out.println("Null console");
    return;
}

String input =  console.readLine();
```

---

## Reader & Writer

- reader() ~= System.in

Retrieves the unique Reader object associated with this console.

```java
Console con = System.console();
 if (con != null) {
     Scanner sc = new Scanner(con.reader());
     ...
 }
```
---

## Console: format & printf

```java
console.writer().println("Hello");
console.writer().format("World %s", "!");
console.printf("This %s", "rules");
```

---

## flush, readLine, readPassword

- Flushes the console and forces any buffered output to be written immediately .


---

## Streams (file streams)

- a list of data elements
- if _Stream_ is in the name: all types of binary / byte data
    - `FileInputStream`
- if _Reader / Writer_ is in the name: chars / Strings
    - `FileReader`
    - is a _stream_ of chars / Strings (although Stream does not appear in the name)


---


## InputStream

- abstract class
- all children include "InputStream" in the name

```
                        +-------------+
                        | InputStream |
                        | <abstract>  |
                        +-------------+
                               ^
       +-----------------------|---------------------------+
       |                       |                           |
+-----------------+    +-------------------+    +-------------------+
| FileInputStream |    | FilterInputStream |    | ObjectInputStream |
| <low level>     |    |                   |    |                   |
+-----------------+    +-------------------+    +-------------------+
                                ^
                                |
                      +---------------------+
                      | BufferedInputStream |
                      |                     |
                      +---------------------+
```

---

## OutputStream

```
                        +--------------+
                        | OutputStream |
                        |  <abstract>  |
                        +--------------+
                               ^
       +-----------------------|---------------------------+
       |                       |                           |
+------------------+    +--------------------+    +--------------------+
| FileOutputStream |    | FilterOutputStream |    | ObjectOutputStream |
| <low level>      |    |                    |    |                    |
+------------------+    +--------------------+    +--------------------+
                                ^
                      ----------+-------------
                      |                      |
          +----------------------+      +-------------+
          | BufferedOutputStream |      | PrintStream |
          |                      |      +-------------+
          +----------------------+
```



---

## Reader

---

## Writer

---

## Serializable

```java
class Person implements Serializable {
    public static final long serialVersionUID = 1L;
    
    private String name;
    private transient String myId;

    public String getName() {
        return name;
    }

    public Person setName(String name) {
        this.name = name;
        return this;
    }

    public String getMyId() {
        return myId;
    }

    public Person setMyId(String myId) {
        this.myId = myId;
        return this;
    }
}

```


---



### Links IO

- [File Class](https://github.com/acailic/java8-learning/blob/master/Java-8/src/io/FileClass.java) <br />
- [Iteracting with Users](https://github.com/acailic/java8-learning/blob/master/Java-8/src/io/IteractingWithUsers.java) <br />
- [Streams](https://github.com/acailic/java8-learning/blob/master/Java-8/src/io/Streams.java) <br />
- [Streams Continue](https://github.com/acailic/java8-learning/blob/master/Java-8/src/io/StreamsContinue.java) <br />


### Questions

- serialized and deserialized in another JVM:

Remember that static fields are never serialized irrespective of whether they are marked transient or not.  In fact, making static fields as transient is redundant. Thus, f1, f2 and f4 will not be serialized at all. However, since f4 is being initialized as a part of class initialization, it will be initialized to the same value in another JVM. Thus, its value will be same as the one initialized by the code.

- Remember that transient fields and static fields are never serialized. Constructor, instance blocks, and field initialization of the class being deserialized are also not invoked. So, when boo is deserialized, the value of ti is set to 0.

- Console is meant to interact with the user typically through command/shell window and keyboard. Thus, binary data doesn't make sense for the console.  You can read whatever the user types using readLine() and readPassword() method. You can also acquire a Reader object using reader() method on Console object. All these provide character data. Similarly, you can acquire PrintWriter object using writer() method on Console, which allows you to write character data to the console.

1. Console class is in java.io package. 
2. Correct way to retrieve the Console object is System.console(); There is only one Console object so new Console(); doesn't make sense. And therefore, Console's constructor is not public. 
3. You can read user's input using either readLine() or readPassword(). Here, since you are reading password, readPassword() should be used. readPassword() ensures that the keys typed by the user aren't echoed to the command prompt.

- A Reader such as a FileReader provides only low level operations such as reading a single character or array of characters. It does not understand the notion of "lines". BufferedReader "decorates" Reader to provide higher level method readLine() by buffering characters. It is an efficient way of reading characters, character arrays, and lines.  The same relationship exists between FileWriter and BufferedWriter but for writing.

- BufferedWriter does not have writeUTF method but it does have newLine and write(String) methods. So the code will fail to compile at //1. You should remember that following points:
 1.BufferedWriter only adds the functionality of buffering on top of a Writer. It doesn't directly deal with encoding. Encoding is handled by the underlying Writer object. 
2.FileWriter is a concrete subclass of java.io.Writer that writes data to the underlying file in default encoding. If you want to write text in a different encoding, you will need to create an OutputStreamWriter with that encoding. For example, OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("utf8.txt"), Charset.forName("UTF-8").newEncoder()  );
 You can then create a BufferedWriter over this OutputStreamWriter.
 
- A BufferedReader can wrap any Reader. Both FileReader and BufferedReader are Readers.
- none of PrintWriter's print or write methods throw I/O exceptions (although some of its constructors may). This is unlike other streams, where you need to include exception handling (i.e. a try/catch or throws clause) when you use the stream.
- PrintWriter's write method writes a single character to the file. The size in bytes of a character depends on the default character encoding of the underlying platform.
  For example, if the encoding is UTF-8, only 1 byte will be written and the size of the file will be 1 byte.
 
-  FileOutputStream take an int parameter but write only the low 8 bits (i.e. 1 byte) of that integer.  DataOutputStream provides methods such as writeInt, writeChar, and writeDouble, for writing complete value of the primitives to a file. So if you want to write an integer to the file, you should use writeInt(1) in which case a file of size 4 bytes will be created. You can read back the stored primitives using methods such as DataInputSream.readInt().   

- To customize the behavior of class serialization, the readObject and writeObject methods should be implemented. Constructor of the class for an object being deserialized is never invoked.

- If the object graph contains non-serializable objects, an exception is thrown and nothing is serialized. Object graph means all the objects that are linked/referenced by the first object (directly or indirectly) that is being serialized.
  Fields of an Object that are marked as transient are not serialized and so they do not cause an exception. Any field that is not marked transient but points to an object of a class that does not implement Serializable, will cause an exception to be thrown. Thus, the given statement is wrong.
