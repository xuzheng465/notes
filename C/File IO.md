# File I/O



## fopen

```c
FILE *fopen(const char *filename, const char *mode);
```

* Opens a file
* Parameters:
  * **filename** the name of the file to open or the path to it
  * ***mode*** acces mode
    * "r"
    * "r+"
    * "w"
    * "w+"
    * "a"
    * "a+
* Returns: A file pointer (sometime referred to as a file handle and associated with a file stream) on success or **NULL** on failure.
* When working with binary files, a "b" modifier maybe included as a sufflix in mode. C11 also added the "x" modifier, which forces "w" or "w+" to fail if the file exists, instead of discarding its contents.

<img src="/Users/xuzheng/Projects/notes/C/File IO.assets/fileaccess.png" alt="fileaccess" style="width:700px;" />

x x+ 如果存在file fail



### Function

#### fseek

| Signature   | `int fseek (FILE * stream, long offset, int origin)`         |
| ----------- | ------------------------------------------------------------ |
| Description | 将文件流的当前位置设置为相对于origin位置的offset位置<br />Sets the current position of the file stream to an offset relative to an origin location in the file stream |
| Parameters  | `stream` A file pointer                                      |
|             | `offset` The offset distance from an origin position         |
|             | `origin` A constant which denotes the position from which to offset the stream. |
|             | SEEK_SET (beginning) , SEEK_CUR (current stream pos), SEEK_END (end of the file) |

## Creating and Writing files

Creating files
Writing textual data
Moving the stream position
Writing formatted data
Appending to files
Error handling



















