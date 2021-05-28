title: bash until loop
date: 2021-04-09
tags: [bash, shell]

# Bash until loop

Loops are one of the fundamental concepts of programming languages. Loops are handy when you want to run a series of commands over and over again until a specific condition is met.

In scripting languages such as Bash, loops are useful for automating repetitive tasks. There are 3 basic loop constructs in Bash scripting, `for` loop , `while` loop , and `until` loop.

This tutorial explains the basics of the `until` loop in Bash.

## Bash until Loop

The `until` loop is used to execute a given set of commands as long as the given condition evaluates to false.

The Bash `until` loop takes the following form:

```bash
until [CONDITION]
do
  [COMMANDS]
done
```

The condition is evaluated before executing the commands. If the condition evaluates to false, commands are executed. Otherwise, if the condition evaluates to true the loop will be terminated and the program control will be passed to the command that follows.

In the example below, on each iteration the loop prints the current value of the variable `counter` and [increments the variable](https://linuxize.com/post/bash-increment-decrement-variable/) by one.

```bash
#!/bin/bash

counter=0

until [ $counter -gt 5 ]
do
  echo Counter: $counter
  ((counter++))
done
```

The loop iterates as long as the `counter` variable has a value greater than four. The script will produce the following output:

```bash
Counter: 0
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
```

Use the `break` and `continue` statements to control the loop execution.

## Bash until Loop Example

The following script may be useful when your git host has downtime, and instead of manually typing `git pull` multiple times until the host is online, you can run the script once. It will try to pull the repository until it is successful.

```bash
#!/bin/bash

until git pull &> /dev/null
do
    echo "Waiting for the git host ..."
    sleep 1
done

echo -e "\nThe git repository is pulled."
```

The script will print “Waiting for the git host …” and `sleep` for one second until the git host goes online. Once the repository is pulled, it will print “The git repository is pulled.”.

```bash
Waiting for the git host ...
Waiting for the git host ...
Waiting for the git host ...

The git repository is pulled.
```

## Conclusion

The `while` and `until` loops are similar to each other. The main difference is that the `while` loop iterates as long as the condition evaluates to `true` and the `until` loop iterates as long as the condition evaluates to `false`.
