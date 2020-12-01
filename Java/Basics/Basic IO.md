# Basics



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



# Reader



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

```java
File file = new File("files/data.txt");
Writer writer = new FileWriter(file);

// It can also append the new content to the existing file
Writer writer = new FileWriter(file, true);

PrintWriter printer = new PrintWriter(writer);
```

By default, a file writer writer from the beginning of a file



```java
BufferedWriter bufferedWriter1 = new BufferedWriter(fileWriter);

Path path = Paths.get("files/data.txt");
BufferedWriter bufferedWriter2 = Files.newBufferedWriter(path, StandardCharsets.ISO_8859_1);
// One can also pass other charsets
```



**OpenOption**

- WRITE, APPEND
- CREATE, CREATE_NEW
- DELETE_ON_CLOSE



Writing on an I/O resource (disk or network) is usually made on a buffer

Then flushed to the output resource

There is a **flush()** method on Writer.

Closing a writer **triggers** a **flush** call



A flush call propagrate to all the streams

Until the output resource is reached

And will trigger a system call

The writing on the I/O is the responsibility of OS



```java
// Formatter === sprintf in unix

```

# Reading and Writing Bytes

<img src="./Basic IO.assets/image-20201130094952389.png" alt="image-20201130094952389" style="zoom:50%;" />

```java
InputStream is = ...; //
int nextByte = is.read();
while(nextByte!=-1) {
  //do something with nextByte
  nextByte = is.read();
}

byte[] is = new byte[1024];
int number = is.read(buffer);
while (number!=-1) {
  // do something with buffer
  number = is.read(buffer);
}
```

```java
OutputStream os = ...;
os.write(charc);
```



### classes for a certain type of medium

- Disk: **FileInputStream** / **FileOutputStream**
- In-memory: **ByteArrayInputStream** / **ByteArrayOutputStream**
- Network: **SocketInputStream** / **SocketOutputStream**

### classes that add behavior

- **BufferedOutputStream**
- **GzipOutputStream**
- **ZipOutputStream**



### Writing and reading bytes is also about

- writing primitive types: int, float, etc...
- and objects: Serialization

There are classes for that!



#### DataInputStream & DataOutputStream

For **primitive** types

```java
OutputStream os = ...;
DataOutputStream dos = new DataOutputStream(os);
// There is a collection of writeXXX() methods
// One for each primitive type
dos.writeInt(10);
dos.writeDouble(3.14d);
dos.writeChar('H');
dos.writeUTF("Hello"):
```



```java
InputStream is = ...;
DataInputStream dis = new DataInputStream(is);

int i = dis.readInt();
double d = dis.readDouble();
char c = dis.readChar();
String s = dis.readUTF();
```



```java
OutputStream os = Files.newOutputStream(
	Paths.get("files/file.bin"),
  StandardOpenOptions.CREATE
);
DataOutputStream dos = new DataOutputStream(os);
IntStream.range(0, 100).forEach(dos::writeInt);
// This code writes 100 ints in a regular file
```





``` java
OutputStream os = Files.newOutputStream(
	Paths.get("files/file.bin.gz"),
  StandardOpenOptions.CREATE
);

GZIPOutputStream gzos = new GZIPOutputStream(os);
DataOutputStream dos = new DataOutputStream(os);
IntStream.range(0, 100).forEach(dos::writeInt);
```



# R & W data and objects

## About Serialization

* Serialization is a general mechanism
* It's about creating a portable representation of an object



## Makeing a class serializable



Only instances of serializable classes can be serialized.

The only thing to do for the class is to implement the Serializable interface.

Which has no method

Add a special static filed: **serialVersionUID**

If it is not, then it will be computed when needed

e.g.

```java
public class Person implements Serializable {
  private static final long serialVersionUID = 232424354512L;
  
  private String name;
  private int age;
}
```

* How is the serial version UID computed?
* The computation is fully specified in the Java Language Specification.
* It is a hash computed from the class name, interfaces implemented, methods, and fields using a SHA.

* 如果编译器就有了, 就用, 不验证

## Writing and reading serialized



```java
OutputStream os = Files.newOutputStream(
	Paths.get("files/people.bin"),
  StandardOpenOptions.CREATE
);

ObjectOutputStream oos = new ObjectOutputStream(os);
oos.writeObject(person1);
```



```java
InputStream is = Files.newInputSream(
	Paths.get("files/people.bin"),
  StandardOpenOptions.READ
);
ObjectInputStream ois = new ObjectInputStream(is);
Person person1 = (Person)ois.readObject();
```

