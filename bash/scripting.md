# Scripting
However there are no extensions in Linux, scripts are marked with **.sh** extension. First line of every script should contain path to an interpreter. Just like this: <br/>
`#!/bin/bash`

### How to start a script?
If we are in the directory where script is located simply `./name_of_script`. <br/>
**./** tells the system to ignore the **PATH** variable and search for the script in the current directory.

### Define a variable
Just simply `name_of_var=12` (no spaces allowed). Referring to a variable `$name_of_var`. A more sophisticated way to use variables is `{$name_of_var}`<br/> <br/>
To save the output of a command to a variable we have to use **myvar=\`some command`** or `myvar=$( ls /etc | wc -l )`.<br/>
If we want to use some variable in another script exporting is required. `export var1` and calling a script **.\script2.sh**.

### Command line arguments
- `$0`: name of the scripts
- `$#`: how many command line arguments were given to the script
- `$*` or `$@`: all of the arguments
- `$?`: return status of the previously run command or function
- `$1-$9`: any arguments

### Getting user input
- `read name_of_var`
- `read -p 'Username: ' name_of_var`
- `read -s name_of_var`: silent which is ideal to passwords

```shell
#!/bin/bash
# A basic summary of my sales report
echo Here is a summary of the sales data:
echo ====================================
echo

cat /dev/stdin | cut -d' ' -f 2,3 | sort
```
This scipt above represent how to write a script which can be used in piping.

### Make scripts pipeable
This section covers how to make our scripts be able to recieve piping. We should use just one of the followings:
- **STDIN**: `/dev/stdin`
- **STDOUT**: `/dev/stdout`
- **STDERR**: `/dev/stderr`

### Arithmetic operations
`let` keyword should be used. <br/>
If we want to assign an arithmetic expression to a variable we can also use `$(())` syntax.<br/>
`#name_of_var` gives back the length of the variable.

### If operator
```shell
if [ $1 -gt 100 ]
then
    ....
fi
```
##### Some common operators
- `-gt`: greater than
- `-lt`: less than
- `-eq`: equal
- `=`: string comparison
- `!=`: string comparison
- `-d`: file exits and is a directory
- `-e`: file exits

#### Functions
We define functions as the following. Passing arguments to functions is the same like calling a script with arguments.
```shell
#function definiton
print_something () {
  echo Hello $1
  return 5
}

print_something Mars
print_something Jupiter
echo The previous function has a return value of $?
```
Return value can be accessed via **$?**. Another approach to use function return value is printing the result out:
```shell
lines_in_file () {
  cat $1 | wc -l
}

num_lines=$( lines_in_file $1 )
```
