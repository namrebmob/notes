title: bash functions
date: 2021-04-12
tags: [bash, shell]

# Bash functions

A Bash function is essentially a set of commands that can be called numerous times. The purpose of a function is to help you make your bash scripts more readable and to avoid writing the same code repeatedly. Compared to most programming languages, Bash functions are somewhat limited.

In this tutorial, we will cover the basics of Bash functions and show you how to use them in your shell scripts.

## Defining Bash Functions

The syntax for declaring a bash function is straightforward. Functions may be declared in two different formats:

1. The first format starts with the function name, followed by parentheses. This is the preferred and more used format.

```bash
function_name () {
  commands
}
```

Single line version:

```bash
function_name () { commands; }
```

2. The second format starts with the reserved word `function`, followed by the function name.

```bash
function function_name {
  commands
}
```

Single line version:

```bash
function function_name { commands; }
```

Few points to be noted:

* The commands between the curly braces (`{}`) are called the body of the function. The curly braces must be separated from the body by spaces or newlines.
* Defining a function doesn’t execute it. To invoke a bash function, simply use the function name. Commands between the curly braces are executed whenever the function is called in the shell script.
* The function definition must be placed before any calls to the function.
* When using single line “compacted” functions, a semicolon `;` must follow the last command in the function.
* Always try to keep your function names descriptive.

To understand this better, take a look at the following example:

\~/hello_world.sh

```bash
#!/bin/bash

hello_world () {
  echo 'hello, world'
}

hello_world
```

Let’s analyze the code line by line:

* In line 3, we are defining the function by giving it a name. The curly brace `{` marks the start of the function’s body.
* Line `4` is the function body. The function body can contain multiple commands, statements and variable declarations.
* Line `5`, the closing curly bracket `}`, defines the end of the `hello_world` function.
* In line `7` we are executing the function. You can execute the function as many times as you need.

If you run the script, it will print `hello, world`.

## **Variables Scope**

Global variables are variables that can be accessed from anywhere in the script regardless of the scope. In Bash, all variables by default are defined as global, even if declared inside the function.

Local variables can be declared within the function body with the `local` keyword and can be used only inside that function. You can have local variables with the same name in different functions.

To better illustrate how variables scope works in Bash, let’s consider this example:

\~/variables_scope.sh

```bash
#!/bin/bash

var1='A'
var2='B'

my_function () {
  local var1='C'
  var2='D'
  echo "Inside function: var1: $var1, var2: $var2"
}

echo "Before executing function: var1: $var1, var2: $var2"

my_function

echo "After executing function: var1: $var1, var2: $var2"
```

The script starts by defining two global variables `var1` and `var2`. Then there is an function that sets a local variable `var1` and modifies the global variable `var2`.

If you run the script, you should see the following output:

```bash
Before executing function: var1: A, var2: B
Inside function: var1: C, var2: D
After executing function: var1: A, var2: D
```

From the output above, we can conclude that:

* When a local variable is set inside the function body with the same name as an existing global variable, it will have precedence over the global variable.
* Global variables can be changed from within the function.

## **Return Values**

Unlike functions in “real” programming languages, Bash functions don’t allow you to return a value when called. When a bash function completes, its return value is the status of the last statement executed in the function, `0` for success and non-zero decimal number in the 1 - 255 range for failure.

The return status can be specified by using the `return` keyword, and it is assigned to the variable `$?`. The `return` statement terminates the function. You can think of it as the function’s [exit status](https://linuxize.com/post/bash-exit/) .

\~/return_values.sh

```bash
#!/bin/bash

my_function () {
  echo "some result"
  return 55
}

my_function
echo $?
```

```bash
some result
55
```

To actually return an arbitrary value from a function, we need to use other methods. The simplest option is to assign the result of the function to a global variable:

\~/return_values.sh

```bash
#!/bin/bash

my_function () {
  func_result="some result"
}

my_function
echo $func_result
```

```bash
some result
```

Another, better option to return a value from a function is to send the value to `stdout` using [echo](https://linuxize.com/post/echo-command-in-linux-with-examples/) or `printf` like shown below:

\~/return_values.sh

```bash
#!/bin/bash

my_function () {
  local func_result="some result"
  echo "$func_result"
}

func_result="$(my_function)"
echo $func_result
```

```bash
some result
```

Instead of simply executing the function which will print the message to stdout, we are assigning the function output to the `func_result` variable using the `$()` command substitution. The variable can later be used as needed.

## **Passing Arguments to Bash Functions**

To pass any number of arguments to the bash function simply put them right after the function’s name, separated by a space. It is a good practice to double-quote the arguments to avoid the misparsing of an argument with spaces in it.

* The passed parameters are `$1`, `$2`, `$3` … `$n`, corresponding to the position of the parameter after the function’s name.
* The `$0` variable is reserved for the function’s name.
* The `$#` variable holds the number of positional parameters/arguments passed to the function.
* The `$*` and `$@` variables hold all positional parameters/arguments passed to the function.
  * When double-quoted, `"$*"` expands to a single string separated by space (the first character of IFS) - `"$1 $2 $n"`.
  * When double-quoted, `"$@"` expands to separate strings - `"$1" "$2" "$n"`.
  * When not double-quoted, `$*` and `$@` are the same.

Here is an example:

\~/passing_arguments.sh

```bash
#!/bin/bash

greeting () {
  echo "Hello $1"
}

greeting "Joe"
```

```bash
Hello Joe
```

## **Conclusion**

A Bash function is a block of reusable code designed to perform a particular operation. Once defined, the function can be called multiple times within a script.

You may also want to read about how to use a Bash function to create a [memorable shortcut command](https://linuxize.com/post/how-to-create-bash-aliases/) for a longer command.
