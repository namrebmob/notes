# xargs

The `xargs` utility allows you to build and execute commands from standard input. It is usually used in combination with other commands through piping.

With `xargs`, you can provide standard input as argument to command-line utilities like `mkdir` and `rm`.

In this tutorial, we’ll cover the basics of using the `xargs` command.

## How to Use Linux `xargs` Command

`xargs` reads arguments from the standard input, separated by blank spaces or newlines, and executes the specified command using the input as command’s arguments. If no command is provided, default is `/bin/echo`.

The syntax for the `xargs` command is as follows:

```sh
xargs [OPTIONS] [COMMAND [initial-arguments]]
```

The most basic example of using `xargs` would be to pass several strings separated with whitespace using a pipe to `xargs` and run a command that will use those strings as arguments.

```sh
echo "file1 file2 file3" | xargs touch
```

In the example above, we are piping the standard input to `xargs`, and the `touch` command is run for each argument, creating three files. This is the same as if you would run:

```sh
touch file1 file2 file3
```

## How to View the Command and Prompt the User

To print the command on the terminal before executing it use the `-t` (`--verbose`) option:

```sh
echo  "file1 file2 file3" | xargs -t touch
```

```bash
touch file1 file2 file3
```

If you want to get a prompt whether to run each command before executing it, use the `-p` (`--interactive`) option:

```bash
echo  "file1 file2 file3" | xargs -p touch
```

Type `y` or `Y` to confirm and run the command:

```bash
touch file1 file2 file3 ?...y
```

This option is useful when executing destructive commands.

## **How to Limit the Number of Arguments**

By default, the number of arguments passed to the command is determined by the system’s limit.

The `-n` (`--max-args`) option specifies the number of arguments to be passed to the given command. `xargs` runs the specified command as many times as necessary until all arguments are exhausted.

In the following example, the number of arguments that are read from the standard input is limited to 1.

```bash
echo  "file1 file2 file3" |  xargs -n 1 -t touch
```

As you can see from the verbose output below, the touch command is executed separately for each argument:

```bash
touch file1
touch file2
touch file3
```

## **How to Run Multiple Commands**

To run multiple commands with `xargs`, use the `-I` option. It works by defining a `replace-str` after the `-I` option and all occurrences of the `replace-str` are replaced with the argument passed to xargs.

The following `xargs` example will run two commands, first it will create the files using `touch`, and then it will list the files with the `ls` command:

```bash
echo "file1 file2 file3" | xargs -t -I % sh -c '{ touch %; ls -l %; }'
```

```bash
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file1
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file2
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file3
```

A common choice for `replace-str` is `%`. However, you can use another placeholder, for example, `ARGS`:

```bash
echo "file1 file2 file3" | xargs -t -I ARGS sh -c '{ touch ARGS; ls -l ARGS; }'
```

## **How to Specify a Delimiter**

Use the `-d` (`--delimiter`) option to set a custom delimiter, which can be either a single character or an escape sequence starting with `\`.

The following example we are using `;` as a delimiter:

```bash
echo  "file1;file2;file3" | xargs -d \; -t touch
```

```bash
touch file1 file2 file3
```

## **How to Read Items from File**

The xargs command can also read items from a file instead of standard input. To do so, use the `-a` (`--arg-file`) option followed by the file name.

In the following example, the `xargs` command will read the `ips.txt` file and ping each IP Address.

ips.txt

```
8.8.8.8
1.1.1.1
```

We are also using the `-L 1` option, which instructs `xargs` to read one line at the time. If this option is omitted `xargs` will pass all IPs to a single `ping` command.

```bash
xargs -t -L 1 -a ips.txt ping -c 1
```

```bash
ping -c 1 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=50 time=68.1 ms

...
ping -c 1 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=59 time=21.4 ms
```

## **Using** `xargs` with `find`

`xargs` is most often used in combination with the `find` command. You can use `find` to search for specific files and then use `xargs` to perform operations on those files.

To avoid issues with file names that contain newlines or other special characters, always use the find `-print0` option, which causes `find` to prints the full file name followed by a null character. This output can be correctly interpreted by `xargs` using the `-0`, (`--null`) option.

In the following example, `find` will print the full names of all files inside the `/var/www/.cache` directory and `xargs` will pass the file paths to the `rm` command:

```bash
find /var/www/.cache -type f -print0 | xargs -0 rm -f
```

## **Using xargs to Trim Whitespace Characters**

`xargs` can also be used as a tool to remove whitespace from both sides of a given string. Simply pipe the string to the `xargs` command, and it will do the trimming:

```bash
echo "  Long line " | xargs
```

```bash
Long line
```

This can be useful when [comparing strings](https://linuxize.com/post/how-to-compare-strings-in-bash/) in shell scripts.

```bash
#!/bin/bash

VAR1=" Linuxize "
VAR2="Linuxize"

if [[ "$VAR1" == "$VAR2" ]]; then
    echo "Strings are equal."
else
    echo "Strings are not equal."
fi

## Using xargs to trim VAR1
if [[ $(echo "$VAR1" | xargs) == "$VAR2" ]]; then
    echo "Strings are equal."
else
    echo "Strings are not equal."
fi
```

```bash
Strings are not equal.
Strings are equal.
```
