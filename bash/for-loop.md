title: bash for loop
date: 2021-04-13
tags: [bash, shell]

# Bash `for` loop

Loops are one of the fundamental concepts of programming languages. Loops are handy when you want to run a series of commands over and over again until a certain condition is reached.

In scripting languages such as Bash, loops are useful for automating repetitive tasks.

There are three basic loop constructs in Bash scripting, `for` loop, `while` loop , and `until` loop .

In this tutorial, we will cover the basics of for loops in Bash. We will also show you how to use the `break` and `continue` statements to alter the flow of a loop.

## The Standard Bash `for` Loop

The `for` loop iterates over a list of items and performs the given set of commands.

The Bash `for` loop takes the following form:

```bash
for item in [LIST]
do
  [COMMANDS]
done
```

The list can be a series of strings separated by spaces, a range of numbers, output of a command, an array, and so on.

### Loop over strings

In the example below, the loop will iterate over each item in the list of strings, and the variable `element` will be set to the current item:

```bash
for element in Hydrogen Helium Lithium Beryllium
do
  echo "Element: $element"
done
```

The loop will produce the following output:

```bash
Element: Hydrogen
Element: Helium
Element: Lithium
Element: Beryllium
```

### Loop over a number range

You can use the sequence expression to [specify a range](https://linuxize.com/post/bash-sequence-expression/) of numbers or characters by defining a start and the end point of the range. The sequence expression takes the following form:

```bash
{START..END}
```

Here is an example loop that iterates through all numbers from 0 to 3:

```bash
for i in {0..3}
do
  echo "Number: $i"
done
```

```bash
Number: 0
Number: 1
Number: 2
Number: 3
```

Starting from Bash 4, it is also possible to specify an increment when using ranges. The expression takes the following form:

```bash
{START..END..INCREMENT}
```

Here’s an example showing how to increment by 5:

```bash
for i in {0..20..5}
do
  echo "Number: $i"
done
```

```bash
Number: 0
Number: 5
Number: 10
Number: 15
Number: 20
```

### Loop over array elements

You can also use the `for` loop to iterate over an array of elements.

In the example below, we are defining an array named `BOOKS` and iterating over each element of the array.

```bash
BOOKS=('In Search of Lost Time' 'Don Quixote' 'Ulysses' 'The Great Gatsby')

for book in "${BOOKS[@]}"; do
  echo "Book: $book"
done
```

```bash
Book: In Search of Lost Time
Book: Don Quixote
Book: Ulysses
Book: The Great Gatsby
```

## The C-style Bash for loop

The syntax of the C-style `for` loop is taking the following form:

```bash
for ((INITIALIZATION; TEST; STEP))
do
  [COMMANDS]
done
```

The `INITIALIZATION` part is executed only once when the loop starts. Then, the `TEST` part is evaluated. If it is false, the loop is terminated. If the `TEST` is true, commands inside the body of the `for` loop are executed, and the `STEP` part is updated.

In the following example code, the loop stars by initializing `i = 0`, and before each iteration checks if `i ≤ 10`. If true it [prints](https://linuxize.com/post/echo-command-in-linux-with-examples/) the current value of `i` and [increments the variable] `i` by 1 (`i++`) otherwise the loop terminates.

```bash
for ((i = 0 ; i <= 1000 ; i++)); do
  echo "Counter: $i"
done
```

The loop will iterate 1001 times and produce the following output:

```bash
Counter: 0
Counter: 1
Counter: 2
...
Counter: 998
Counter: 999
Counter: 1000
```

## `break` and `continue` Statements

The `break` and `continue` statements can be used to control the for loop execution.

### `break` Statement

The `break` statement terminates the current loop and passes program control to the statement that follows the terminated statement. It is usually used to terminate the loop when a certain condition is met.

In the following example, we are using the `if` statement to terminate the execution of the loop once the current iterated item is equal to ‘Lithium’.

```bash
for element in Hydrogen Helium Lithium Beryllium; do
  if [[ "$element" == 'Lithium' ]]; then
    break
  fi
  echo "Element: $element"
done

echo 'All Done!'
```

```bash
Element: Hydrogen
Element: Helium
All Done!
```

### `continue` Statement

The `continue` statement exits the current iteration of a loop and passes program control to the next iteration of the loop.

In the following example, we are iterating through a range of numbers. When the current iterated item is equal to ‘2’, the `continue` statement will cause execution to return to the beginning of the loop and to continue with the next iteration:

```bash
for i in {1..5}; do
  if [[ "$i" == '2' ]]; then
    continue
  fi
  echo "Number: $i"
done
```

```bash
Number: 1
Number: 3
Number: 4
Number: 5
```

## Bash `for` Loop Examples

### Renaming files with spaces in the filename

The following example shows how to rename all of the files in the current directory with a space in its names by replacing space to underscore:

```bash
for file in *\ *; do
  mv "$file" "${file// /_}"
done
```

Let’s break down the code line by line:

* The first line creates a `for` loop and iterates through a list of all files with a space in its name. The expression `\ ` creates the list.
* The second line applies to each item of the list and moves the file to a new one replacing the space with an underscore (`_`). The part `${file// /_}` is using the [shell parameter expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html) to replace a pattern within a parameter with a string.
* `done` indicates the end of the loop segment.

### Changing file extension

The following example shows how to use the Bash `for` loop to [rename all files](https://linuxize.com/post/how-to-rename-files-in-linux/) ending with .jpeg in the current directory by replacing the file extension from .jpeg to .jpg.

```bash
for file in *.jpeg; do
    mv -- "$file" "${file%.jpeg}.jpg"
done
```

Let’s analyze the code line by line:

* The first line creates a `for` loop and iterates through a list of all files ending with ‘.jpeg’.
* The second line applies to each item of the list and moves the file to a new one replacing ‘.jpeg’ with ‘.jpg’. `${file%.jpeg}` to remove the ‘.jpeg’ part from the filename using the [shell parameter expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
* `done` indicates the end of the loop segment.

## Conclusion

The Bash `for` loop is used to execute a given set of commands repeatedly for a fixed number of times.
