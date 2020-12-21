

```bash
dosomething < inputfile > outputfile
```

reads its input from `inputfile` and sends its output to `outputfile`.

If you omit the `> outputfile`, the output goes to your terminal window.

If you omit the `< inputfile`, the program takes its input from the keyboard. 



**echo**

It prints the arguments of the command line to the screen.

1. The shell is parsing the arguments on the `echo` command line.

2. Since they are parsed as arguments, the spacing between arguments is ignored.

```bash

```



### writing output without the Newline

```bash
printf "%s %s" next prompt
echo -n prompt
```

The `-n` option suppresses the trailing newline.



### Saving Output from a Command

```bash
echo fill it up > file.txt
```



```bash
cat first.half second.half > whole.file
```

`-C` columnar ouput

### Sending Output and Error Messages to different files

```bash
myprogram 1> message.out 2> message.err
```

1> and 2> the number is the file `descriptor`. 

1 is standard output (STDOUT)

2 is standard error (STDERR)

Numbering starts at 0, for standard input (STDIN)

When no number is specified, STDOUT is assumed.

```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```



```bash

```

