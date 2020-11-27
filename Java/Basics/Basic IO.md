bytes and chars

disk, network, memory



<img src="./Basic IO.assets/image-20201127191643282.png" alt="image-20201127191643282" style="zoom:20%;" />

File and Path are models of a file or directory on the disk



## File



```java
File file = new File("files/data.txt");
file.exists();
file.isDirecotry();
file.isFile();

file.canRead();
file.canWrite();
file.canExecute();

// To create, touch or modify this file
file.createNewFile();
file.mkdir();
file.mkdirs();

file.delete();
file.deleteOnExit();
file.renameTo("files/file.txt");

//The path of a File is the String representing this file
file.getName();
file.getParent();
file.getPath();

// 
file.getAbsolutePath();
file.getCanonicalPath();
```

1. creating a `File` object does not create anything on the disk
2. a `File` object can be a file or a directory



## Path

The `Path` interface has the **same kind** of methods as File class

* to check for the file / dir
* to create / touch /delete
* to get its name / path / etc...
* can get the attributes of the file
* and other methods to check for directory events



Other interesting methods

- `normalize()`: removes redundant elements
- `toAbsolutePath()` 
- `toRealPath()`: resolves the symbolic links
- Files.isSame(path1, path2)
- `resolve() `and` relativize()` :question:

 Relativizing two paths consists in creating a **relative path** from **one** to the **other**



# Reading Characters



The `Reader` is an abstract class

It defines the basic operations:

* Reading of a **single character**
* Reading of an **array of characters**
* **Marking** and **resetting** a given position
* **Skipping** positions

And it can be **closed** 

```java
Read reader = ...; 
int nextChar = reader.read();
while (nextChar != -1) {
  // do sth with nextChar
  // might be to store the characters in a buffer
  nextChat = reader.read()
}
// when there is no more characters to read, the read() call returns -1
```



```java
Reader reader =...;
char[] buffer = new char[1024];

int num = reader.read(buffer);
// fill our buffer with the character it is reading and returns the number of read
while (num!=-1) {
  // do something with the buffer
  num = reader.read(buffer);
}
// when there is no more characters to read, the read() call returns -1
// num is the number of characters that have been read. It can be less than 1024
```



I/O Operations will throw exceptions. 

We need to add some code to be executed in case something goes wrong...

A disk that is **not there**, a network resource that is **unavailable**.



All the method calls may throw an `IOException`

Two strategies:

1. to throw it to the caller
2. to handle it locally
3. log it :exclamation:

```java
Reader reader = null;
try {
  reader = ...;
  int nextChar = reader.read();
	while (nextChar!=-1) {
    // do sth 
    nextChar = reader.read();
  }
} catch (IOException e) {
  // deal with the exception
}
```



1. call close()

   ```java
   Reader reader = null;
   try {
     reader = ...;
     // do something with reader
   } catch (IOException e) {
     // deal with the exception
   } finally {
     if (reader!=null) {
       try {
         reader.close()
       } catch (IOException e) {
         // deal with the exception
       }
     }
   }
   ```

   

2. try-with-resource

Several resources can be opened in the `try()`

The will be closed automatically when leaving the try block.

To be used in this pattern, a resource must implement ***AutoCloseable***.

With only one method to implement: ***close()***

Very easy to use for  homemake resources.

```java
try (Reader reader = ...;) {
  // do sth with reader
} catch (IOException e) {
  // deal with the exception
}
```



Marking, Resetting, Skipping

A `mark()` call puts a flag on a given ele

A `reset()` call **rewinds** to the pre marked ele.  

A **skip()** call skips the next elements



### Two ways of extending a `Reader`

1. classes for a certain type of input
   1. **Disk**: `FileReader`
   2. **In-memory**: `CharArrayReader`, `StringReader`
2. behavior to Reader
   1. BufferedReader: a reader using bufferization
   2. LIneNumberReader
   3. PushbackReader



```java
File file = new File("files/data.txt");
Reader reader = new FileReader(file);

String text = "Hello World!";
Reader strreader = new StringReader(text);
```

The `FileReader` class creates a reader on a file

==**BufferedReader**==

Reads the chars through buf

BuffereedReader adds the readLine() method to Reader

also supports the mark and skip ops

```java
File file = new File("data.txt");
FileReader fileReader = new FileReader(file);
BufferedReader bufferedReader = new BufferedReader(fileReader);

```

==**LineNumberReader**==

Same pattern for LineNumberReader

LineNumberReader **extends** BufferedReader

It adds a getLineNubmer() method

```java
File file = new File("data.txt");
FileReader fileReader = new FileReader(file);
LineNumberReader lineNumberReader = new LineNumberReader(fileReader);
```



#### factory method

```java
Path path = Paths.get("files/data.txt");
BufferedReader reader2 = Files.newBufferedReader(path);
```



The most convinent way 

```java
public static void main(String[] args) {
  Path path = Paths.get("files/bat-weasels.txt");
  try (Stream<String> lines = Files.newBufferedReader(path).lines()){
            lines.forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
}
```



# Writer

<img src="./Basic IO.assets/image-20201127213952104.png" alt="image-20201127213952104" style="zoom:50%;" />



