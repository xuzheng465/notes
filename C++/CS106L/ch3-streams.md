### Overview

A **stream** is a ***channel*** between a source and a destination which allows the source to push formatted data to the destination.

| source   | destination  |
| -------- | ------------ |
| program  | file on disk |
| keyboard | program      |
|          |              |

```c++
cin >> myInteger >> myString; // Read an integer and string from cin
```

This will pause until the user enters an integer, hits enter, then enters a string, then hits enter once more. These values will be stored in myInteger and myString, respectively.



### Reading and writing Files

`<fstream>` file stream

`ifstream` input file stream

`ofstream` output file stream



```c++
#include <iostream>
#include <fstream>

int main() {
		std::ofstream myStream("myFile2.txt");

    myStream << "Files can be tricky, but it is fun enough!";

    myStream.close();
  
  std::ifstream stream2; // Note: did not specify the file
  stream2.open("myFile.txt");
  
}
```

After trying to open a file, you should check if the stream is **valid** by using the .is_open() member function.

验证是否已经打开。 



```c++
ifstream inputfile("myfile.txt");
if (!inputfile.is_open())
  cerr << "Couldn't open the file myfile.txt" << endl;
```

`cerr` 和cout相似，都是输出流。

cerr是为了报告错误。

```c++
ofstream myStream("myFile.txt"); // Write to myFile.txt
```

如果myFile.txt不存在， ofstream将会创建一个文件。

但如果打开一个已存在的文件，ofstream将会覆盖该文件的所有内容

:exclamation: 在写入一些重要文件前要先备份他们



stream库在string类型之前就有了。

如果一个文件的文件名以C++的string存储，你需要将它转换成c-style string

string class 中的 .c_str() 方法

```c++
ifstream input(myString.c_str());
```



```c++

```



```c++

```



```c++

```



```c++

```



```c++

```

