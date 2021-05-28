title: bash while loop
date: 2021-04-08
tags: [bash, shell]

# Bash while loop

Loops are one of the fundamental concepts of programming languages. Loops are handy when you want to run a series of commands a number of times until a particular condition is met.

In scripting languages such as Bash, loops are useful for automating repetitive tasks. There are three basic loop constructs in Bash scripting, `for` loop , `while` loop, and `until` loop .

This tutorial covers the basics of `while` loops in Bash. We’ll also show you how to use the `break` and `continue` statements to alter the flow of a loop.

## Bash while Loop

The `while` loop is used to performs a given set of commands an unknown number of times as long as the given condition evaluates to true.

The Bash `while` loop takes the following form:

```bash
while [CONDITION]
do
  [COMMANDS]
done
```

The `while` statement starts with the `while` keyword, followed by the conditional expression.

The condition is evaluated before executing the commands. If the condition evaluates to true, commands are executed. Otherwise, if the condition evaluates to false, the loop is terminated, and the program control will be passed to the command that follows.

In the example below, on each iteration, the current value of the variable `i` is printed and [incremented](https://linuxize.com/post/bash-increment-decrement-variable/) by one.

```bash
i=0

while [ $i -le 2 ]
do
  echo Number: $i
  ((i++))
done
```

Tue loop iterates as long as `i` is less or equal than two. It will produce the following output:

```bash
Number: 0
Number: 1
Number: 2
```

## Infinite while Loop

An infinite loop is a loop that repeats indefinitely and never terminates. If the condition always evaluates to true, you get an infinite loop.

In the following example, we are using the built-in command `:` to create an infinite loop. `:` always returns true. You can also use the `true` built-in or any other statement that always returns true.

```bash
while :
do
  echo "Press <CTRL+C> to exit."
  sleep 1
done
```

The `while` loop above will run indefinitely. You can terminate the loop by pressing `CTRL+C`.

Here is a single-line equivalent:

```bash
while :; do echo 'Press <CTRL+C> to exit.'; sleep 1; done
```

## Read a File Line By Line

One of the most common usages of the `while` loop is to read a file, data stream, or variable line by line.

Here is an example that reads the `/etc/passwd` file line by line and prints each line:

```bash
file=/etc/passwd

while read -r line; do
  echo $line
done < "$file"
```

Instead of controlling the `while` loop with a condition, we are using input redirection (`< "$file"`) to pass a file to the `read` command, which controls the loop. The `while` loop will run until the last line is read.

When reading file line by line, always use `read` with the `-r` option to prevent backslash from acting as an escape character.

By default, the `read` command trims the leading/trailing whitespace characters (spaces and tabs). Use the `IFS=` option before `read` to prevent this behavior:

```bash
file=/etc/passwd

while IFS= read -r line; do
  echo $line
done < "$file"
```

## break and continue Statements

The `break` and `continue` statements can be used to control the while loop execution.

### break Statement

The `break` statement terminates the current loop and passes program control to the command that follows the terminated loop. It is usually used to terminate the loop when a certain condition is met.

In the following example, the execution of the loop will be interrupted once the current iterated item is equal to `2`.

```bash
i=0

while [ $i -lt 5 ]
do
  echo "Number: $i"
  ((i++))
  if [[ "$i" == '2' ]]; then
    break
  fi
done

echo 'All Done!'
```

```bash
Number: 0
Number: 1
All Done!
```

### continue Statement

The `continue` statement exits the current iteration of a loop and passes program control to the next iteration of the loop.

In the following below, once the current iterated item is equal to `2` the `continue` statement will cause execution to return to the beginning of the loop and to continue with the next iteration.

```bash
i=0

while [ $i -lt 5 ]
do
  ((i++))
  if [[ "$i" == '2' ]]; then
    continue
  fi
  echo "Number: $i"
done

echo 'All Done!'
```

```bash
Number: 1
Number: 3
Number: 4
Number: 5
All Done!
```

## Conclusion

The `while` loop repeatedly executes a given set of commands as long as a condition is true.
