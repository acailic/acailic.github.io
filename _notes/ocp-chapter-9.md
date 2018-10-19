---
title: OCP Review 9 - NIO.2
layout: post
tags: [java, ocp, io]
date: 2018-09-06
---
## NIO2: New I/O api, v2

- introduced in Java 1.4 as replacement for Streams
    - not used by anyone
- Java 1.7 brings up NIO2
    - still nobody uses it

---

## Path

- interface `java.nio.file.Path`
- understands symbolic links
- to create:

```java
Path bin = Paths.get("/bin");

// using varargs so no need to change between / & \
// the correct system-dependent path.separator gets added

Path bin = Paths.get("/", "bin");

```

---

## Path from URIs

```java
Path bin = null;
try {
    bin = Paths.get(new URI("file:///bin"));
} catch (URISyntaxException e) {
    e.printStackTrace();
}

System.out.println(bin.getFileName());

```

---

## FileSystem

- get a Path

```java
Path bin = FileSystems.getDefault().getPath("/bin");

System.out.println(bin.getFileName());
```

- all mounted volumes

```java
FileSystems.getDefault().getFileStores().forEach(System.out::println);
```

---

## Converting Path <--> File

```java
Path bin = FileSystems.getDefault().getPath("/bin");

System.out.println(bin.getFileName());

File f = bin.toFile();  // converts from "new" Path to "old" File

Path newBin = f.toPath();

System.out.println(newBin);

```
---

## Splitting the path into components

```java
Path bin = FileSystems.getDefault().getPath("/usr/local/bin");

for (int i = 0; i < bin.getNameCount(); i++) {
    System.out.println(bin.getName(i));
}

usr
local
bin
```

---

## Some utilities

```java
Path bin = FileSystems.getDefault().getPath("/usr/local/bin");

System.out.println(bin.getFileName());  // bin
System.out.println(bin.getRoot());      // /
System.out.println(bin.getParent());    // /usr/local

System.out.println(bin.isAbsolute());   // true
```

---

## Getting absolute path from relative

```java
Path bin = FileSystems.getDefault().getPath("hello.txt");

System.out.println(bin.toAbsolutePath());
```

---

## Subpath

```java
Path bin = FileSystems.getDefault().getPath("/usr/local/bin/this/that");

System.out.println(bin.subpath(0, 2));
System.out.println(bin.subpath(3, 5));

usr/local
this/that
```

---

## Testing file exists

```java
 Path bin = FileSystems.getDefault().getPath("/usr/local/bin/this/that");

System.out.println(Files.exists(bin));
```

---

## `Files`: Creating directories

```java
try {
    Path hello = Paths.get("hello");
    if (Files.exists(hello)) {
        Files.delete(hello);
    } else {
        Files.createDirectory(hello);
    }
} catch (IOException e) {
    e.printStackTrace();
}

```

---

## `Files`: Deleting directories

```java
Path hello = Paths.get("hello");
            
Files.deleteIfExists(hello);
```

---

## Other `Files` methods

- isSameFile
- copy
- delete
- move

---

## Printing all lines from a file

```java
try {
    Files.readAllLines(Paths.get("/Users/aca/.bash_history")).
        stream().
        forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}

```

---

## Walking a directory

- print all files in `/usr/local`

```java
try {
    Files.walk(Paths.get("/usr/local")).forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}

```

---

- now just the MarkDown files, please

```java
try {
    Files.walk(Paths.get("/usr/local")).filter(f -> f.getFileName().toString().endsWith(".md")).forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}

```

---

- only directories

```java
try {
    Files.walk(Paths.get("/usr/local")).
        filter(f -> Files.isDirectory(f)).
        forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}
```

```java
try {
    Files.walk(Paths.get("/usr/local")).
        filter(f -> Files.isDirectory(f) && f.getParent().toString().equals("/usr/local") ).
        forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## Find files

- Predicate takes: Path & File Attributes

```java
Path home = Paths.get("/Users/aca");

Stream<Path> foundJavas = null;
try {
    foundJavas = Files.find(home, 4, (p, a) -> p.toString().endsWith(".java"));
} catch (IOException e) {
    e.printStackTrace();
}
foundJavas.forEach(System.out::println);

```



### NIO.2

- [Creating Paths](https://github.com/acailic/java8-learning/blob/master/Java-8/src/nio2/CreatingPaths.java) <br />
- [Interacting with Paths and Files](https://github.com/acailic/java8-learning/blob/master/Java-8/src/nio2/InteractingWithPathsAndFiles.java) <br />
- [New Stream Methods](https://github.com/acailic/java8-learning/blob/master/Java-8/src/nio2/NewStreamMethods.java) <br />
- [Understanding File Attributes](https://github.com/acailic/java8-learning/blob/master/Java-8/src/nio2/UnderstandingFileAttributes.java) <br />

### QUESTIONS
-  Path.resolve() method as given in JavaDoc API:
  
  public Path resolve(Path other)
  
  Resolve the given path against this path.
  
  If the other parameter is an absolute path then this method trivially returns other. If other is an empty path then this method trivially returns this path. Otherwise this method considers this path to be a directory and resolves the given path against this path. In the simplest case, the given path does not have a root component, in which case this method joins the given path to this path and returns a resulting path that ends with the given path. Where the given path has a root component then resolution is highly implementation dependent and therefore unspecified.
  
  Parameters:
  other - the path to resolve against this path 
  
  Returns:
  the resulting path
  
- From the JavaDoc API documentation for Files.copy: Attempts to copy the file attributes associated with source file to the target file. The exact file attributes that are copied is platform and file system dependent and therefore unspecified. Minimally, the last-modified-time is copied to the target file if supported by both the source and target file store. Copying of file timestamps may result in precision loss.

public static Path copy(Path source, Path target, CopyOption... options)                  throws IOException Copy a file to a target file. This method copies a file to the target file with the options parameter specifying how the copy is performed. By default, the copy fails if the target file already exists or is a symbolic link, except if the source and target are the same file, in which case the method completes without copying the file. File attributes are not required to be copied to the target file. If symbolic links are supported, and the file is a symbolic link, then the final target of the link is copied. If the file is a directory then it creates an empty directory in the target location (entries in the directory are not copied).  The options parameter may include any of the following:  REPLACE_EXISTING     If the target file exists, then the target file is replaced if it is not a non-empty directory. If the target file exists and is a symbolic link, then the symbolic link itself, not the target of the link, is replaced.  COPY_ATTRIBUTES     Attempts to copy the file attributes associated with this file to the target file. The exact file attributes that are copied is platform and file system dependent and therefore unspecified. Minimally, the last-modified-time is copied to the target file if supported by both the source and target file store. Copying of file timestamps may result in precision loss.  NOFOLLOW_LINKS     Symbolic links are not followed. If the file is a symbolic link, then the symbolic link itself, not the target of the link, is copied. It is implementation specific if file attributes can be copied to the new link. In other words, the COPY_ATTRIBUTES option may be ignored when copying a symbolic link. An implementation of this interface may support additional implementation specific options.  Copying a file is not an atomic operation. If an IOException is thrown then it possible that the target file is incomplete or some of its file attributes have not been copied from the source file. When the REPLACE_EXISTING option is specified and the target file exists, then the target file is replaced. The check for the existence of the file and the creation of the new file may not be atomic with respect to other file system activities.

- Path getRoot() Returns the root component of this path as a Path object, or null if this path does not have a root component.  Path getName(int index) Returns a name element of this path as a Path object. The index parameter is the index of the name element to return. The element that is closest to the root in the directory hierarchy has index 0. The element that is farthest from the root has index count-1.



- Constraints: 
    
1. By default, Files.move method attempts to move the file to the target file, failing if the target file exists except if the source and target are the same file, in which case this method has no effect. Therefore, this code should throw an exception because a.java exists in the target directory.

2. However, when the CopyOption argument of the move method is StandardCopyOption.ATOMIC_MOVE, the operation is implementation dependent if the target file exists. The existing file could be replaced or an IOException could be thrown. If the exiting file at p2 is replaced, Files.delete(p1) will throw java.nio.file.NoSuchFileException.



- Move : public static Path move(Path source,  Path target,    CopyOption... options)                  throws IOException Move or rename a file to a target file. By default, this method attempts to move the file to the target file, failing if the target file exists except if the source and target are the same file, in which case this method has no effect. If the file is a symbolic link then the symbolic link itself, not the target of the link, is moved. This method may be invoked to move an empty directory. In some implementations a directory has entries for special files or links that are created when the directory is created. In such implementations a directory is considered empty when only the special entries exist. When invoked to move a directory that is not empty then the directory is moved if it does not require moving the entries in the directory. For example, renaming a directory on the same FileStore will usually not require moving the entries in the directory. When moving a directory requires that its entries be moved then this method fails (by throwing an IOException). To move a file tree may involve copying rather than moving directories and this can be done using the copy method in conjunction with the Files.walkFileTree utility method.  The options parameter may include any of the following:  REPLACE_EXISTING     If the target file exists, then the target file is replaced if it is not a non-empty directory. If the target file exists and is a symbolic link, then the symbolic link itself, not the target of the link, is replaced. ATOMIC_MOVE     The move is performed as an atomic file system operation and all other options are ignored. If the target file exists then it is implementation specific if the existing file is replaced or this method fails by throwing an IOException. If the move cannot be performed as an atomic file system operation then AtomicMoveNotSupportedException is thrown. This can arise, for example, when the target location is on a different FileStore and would require that the file be copied, or target location is associated with a different provider to this object. An implementation of this interface may support additional implementation specific options.  Where the move requires that the file be copied then the last-modified-time is copied to the new file. An implementation may also attempt to copy other file attributes but is not required to fail if the file attributes cannot be copied. When the move is performed as a non-atomic operation, and a IOException is thrown, then the state of the files is not defined. The original file and the target file may both exist, the target file may be incomplete or some of its file attributes may not been copied from the original file. 



- If the argument is a relative path (i.e. if it doesn't start with a root), the argument is simply appended to the path to produce the result.
  Path.resolve() method as given in JavaDoc API:
  
  public Path resolve(Path other)
  
  Resolve the given path against this path.
  
  If the other parameter is an absolute path then this method trivially returns other. If other is an empty path then this method trivially returns this path. Otherwise this method considers this path to be a directory and resolves the given path against this path. In the simplest case, the given path does not have a root component, in which case this method joins the given path to this path and returns a resulting path that ends with the given path. Where the given path has a root component then resolution is highly implementation dependent and therefore unspecified.
  
  Parameters:
  other - the path to resolve against this path 
  
  Returns:
  the resulting path
  
- Files.lines methods. One takes just a Path and the second list method allows you the specify the charset of the source file as well.   
  
- API Docs Paths.get(URI)

public static Path get(URI uri) Converts the given URI to a Path object. This method iterates over the installed providers to locate the provider that is identified by the URI scheme of the given URI. URI schemes are compared without regard to case. If the provider is found then its getPath method is invoked to convert the URI. In the case of the default provider, identified by the URI scheme "file", the given URI has a non-empty path component, and undefined query and fragment components. Whether the authority component may be present is platform specific. The returned Path is associated with the default file system. The default provider provides a similar round-trip guarantee to the File class. For a given Path p it is guaranteed that Paths.get(p.toUri()).equals( p.toAbsolutePath()) so long as the original Path, the URI, and the new Path are all created in (possibly different invocations of) the same Java virtual machine. Whether other providers make any guarantees is provider specific and therefore unspecified.  Parameters: uri - the URI to convert  Returns: the resulting Path  Throws: IllegalArgumentException - if preconditions on the uri parameter do not hold. The format of the URI is provider specific. FileSystemNotFoundException - The file system, identified by the URI, does not exist and cannot be created automatically, or the provider identified by the URI's scheme component is not installed SecurityException - if a security manager is installed and it denies an unspecified permission to access the file system

- public static Stream<Path> find(Path start, int maxDepth, BiPredicate<Path,BasicFileAttributes> matcher, FileVisitOption... options) throws IOException Return a Stream that is lazily populated with Path by searching for files in a file tree rooted at a given starting file.

