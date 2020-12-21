

# Variables



## Local Variable

A local variable is local to the shell. They are available to the shell but not by commands launched from it. Ubuntu defaults to nano as a text editor but we can use the `EDITOR` variable to adjust this. A local variable though will not affect commands like crontab.



## Environment Variable

Configuring an environment variable will make the variable available to crontab and other commands.



## Command Variable

A variable that only needs to be effective for the single instance of a command execution are command variables. The variable does not persist after the execution.



## Constants

The declare command can also be used to create **constants** or **readonly** variables.

Readonly variables cannot be unset and remain for the shell session.

Constants add security to your shell commands or scripts ensuring the values cannot be altered from, perhaps, a value set in a login script.



```bash
declare -r name # constant
```



**Array**

```bash
declare -a user_name 

user_name[0]=bob; user_name[1]=smith

echo ${user_name[0]}
# bob
unset user_name; declare -A user_name
# -A associative array
user_name=([first]=bob [last]=smith)

echo ${user_name[first]}

```



```bash
ANSIBLE_CONFIG=/etc/services
declare -xr ANSIBLE_CONFIG
```



```bash
declare -a user_name # array
user_name[0]=bob
user_name[1]=smith

```



# Conditional

* Simple AND/OR constructs
* Creating IF statements
* Arithmetic evaluations
* Comparing strings in evaluations
* Using regular expr as part of a test
* Testing file attributes
* Using case statements



OR

```bash
echo hello || echo bye
# hello
echo hello && echo bye
# hello
# goodbye
```

IF

An `if` statement in the BASH or ZSH has at least one condition to test followed by one or more actions.

The statement is terminated with the `fi` keyword.

```bash
> declare -i days=30
> if [ $days -lt 1 ]; then echo "Enter correct value"; fi
```





```bash
declare -i days=30
if (( days < 1 || days >30 )) ;
then echo "Enter correct value"; else echo "All good"; 
fi
```



```bash
declare -i days

read days
> Monday

if (( days < 1 )); then echo "Enter numeric value";
elif (( days > 30 )); then echo "Too high" ;
else echo "The value is $days";
fi
```

## Testing Regular Expression

Using the `[[` syntax in advanced shells we use the match operator `=~` to test for regular expressions.

The result is also stored in the array `BASH_REMATCH`



```bash
declare -l test_var
read test_var
> color

```



```bash
declare -l user_name
# -l means minus lower case
read user_name
# > bob_admin
[[ $user_name == *_admin ]]
echo $?
# 0
```



```bash
unset user_name
declare -l user_name
read user_name
# bob_admin
declare -p user_name
[[ $user_name =~ _admin$]]
echo $? # output: 0
[[ $user_name =~ _user$ ]]
echo $? # output: 1

```

# The Test Command

There is a command test. The `[` is a synonym for test 

and `[[` is the advanced test that should be used in precedence to `[`

```bash
type test [ [[
# test is a shell builtin
# [ is a shell builtin
# [[ is a shell keyword
```

## Testing File Attributes

We can test to see if a file is of a specific type. Here we check for a regular file.

```bash
test -f /etc/hosts && echo YES
```

`-f`

To test for a regular file

`[[ -f /etc/hosts ]]`



`-d`

Tests for a directory

`[[ -d /etc ]]`



`-L` 

Tests for symbolic link

`[[ -L /etc/localtime ]]`



`-r`

Tests for the read permission, r=read, w=write, x=execute

`[[ -r /etc/hosts ]]`



`-k`

Tests for the sticky bit

`[[ -k /tmp ]]`



`-S`

Tests for the SUID bit, use `g` for the GUID bit

`[[ -s /bin/passwd ]]`



-e

exist

```bash
test -e dir1 || mkdir dir1
test -w dir1 && touch dir1/file1

```

## Put it together

```shell
#!/bin/bash
declare -l DIR
echo -n "Enter the name of the directory to create: "
read DIR
if [[ -e $DIR ]]
	then
		echo "A file exists with the name $DIR"
		exit 1
	else
		if [[ -w $PWD ]]
			then
				mkdir $DIR
      else
      	echo "You don't have permissions. You cannot create $DIR with $PWD"
      	exit 2
    fi
fi
```



## Case Statement

more efficient than many `elif` statements being able to read the test condition just once.

Start with `case` and tertminates with `esac`. 

Each block ends with `;;`.



```bash
case $USER in
	tux )
		echo "You are cool"
		;;
  root )
  	echo "You are the boss"
  	;;
esac
```



Pluralsight's Four Seasons

```bash
#!/bin/bash
declare -l month=$(date +%b)
case $month in
	dec | jan | feb )
		echo "Winter" ;;
	mar | apr | may )
		echo "Spring" ;;
	jun | jul | aug )
		echo "Summer" ;;
	sep | oct | nov )
		echo "Autumn" ;;
esac
```



```bash
> chmod 755 seasons.sh
> chmod u+x seasons.sh
# -rwxr-xr-x
```



# Building Effective Functions

* Listing functions in the shell
* Creating and calling functions
* Exporting functions
* Passing arguments to functions
* Working with return values

## Functions

Shell functions encapsulate blocks of code in named elements that can be executed or called from scripts or directly at the CLI.

* List Functions

```bash
declare -f
# -f prints details of functions
# -F prints the function names
```



## Return Values

Using the command return in a similar way to exit in conditional statements.



```bash
function create_user () {
if ( getent passwd $1 > /dev/null ); then
	echo "$1 already exists";
	return 1;
else
	echo "Creating user $1";
	sudo useradd $1;
	return 0;
}
```

## Best Practices

Functions should be standalone and not dependent on other elements such as variables from the master script. This limits how much the function can be used in other scripts.

```bash

```

## Summary

List functions:

- detailed: declare -f
- summary: declare -F

Export function:

- declare -fx function_name

Unset function:

- unset -f function_name

Exit function using return

Keyword `local` is used to keep variable local to the function

```bash

```

# Loop



```bash
declare -i x=0

while (( x < 11 )) ; do echo $x; x=x+1 ; done
```



```bash
declare -p x
until (( x==0 )) ; do echo $x; x=x-1; done
```



```bash
for (( i=0; i<5; i++ )); do echo $i; done

for ((i=5;i>0;i--)); do echo $i; done
```



```bash
declare -a users=("bob" "joe" "sue")
echo ${#users[*]}
for ((i=0; i<${#users[*]}; i++)); do
	sudo useradd ${users[$i]};
done
```



```bash
for f in $(ls); do stat -c "%n %F" $f ; done
# %n shows name
# %F shows file type
```

### List Files Not Directories

We can test the file read and if it is a directory ignore the item by using continue

```bash
for file in $(ls); do
	if [[ -d $file ]]; then
		continue
	fi
	echo $file
done
```



```bash

```



```bash

```



```bash

```



```bash

```

