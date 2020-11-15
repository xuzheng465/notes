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
* Returns: A file pointer (sometime referred to as a file handle and associated with a file stream) on success or NULL on failure.
* When working with binary files, a "b" modifier maybe included as a sufflix in mode. C11 also added the "x" modifier, which forces "w" or "w+" to fail if the file exists, instead of discarding its contents.