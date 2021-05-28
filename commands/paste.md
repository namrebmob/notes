# **paste**

`paste` is a command that allows you to merge lines of files horizontally. It outputs lines consisting of the sequentially corresponding lines of each file specified as an argument, separated by tabs.

In this tutorial, we will explain how to use the `paste` command.

## **How to Use the** `paste` Command

`paste` is one of the lesser-known and used Linux and Unix command-line utilities.

The general syntax for the `paste` command is as follows:

```
paste [OPTION].. [FILE]...
```

If no input files are provided or when `-` is given as argument, `paste` uses the standard input.

Suppose we have the following files:

file1

```
Iron Man
Thor
Captain America
Hulk
Spider Man
```

file2

```
Black Widow
Captain Marvel
Dark Phoenix
Nebula
```

When invoked without an option `paste` will read all files given as arguments and horizontally merge the corresponding lines of the files, separated by space:

```
paste file1 file2
```

```
Iron Man	Black Widow
Thor	Captain Marvel
Captain America	Dark Phoenix
Hulk	Nebula
Spider Man
```

Instead of displaying the output on the screen, you can redirect it to a file using the `>`, `>>` operators:

```
paste file1 file2 > file3
```

If the file doesn’t exist, it will be created. The `>` operator will overwrite an existing file, while the `>>` operator will append the output to the file.

The `-d`, `-delimiters` option allows you to specify a list of characters to be used as delimiters instead of the default `TAB` separator.

Each delimiter is consecutively used. When the list is exhausted, `paste` starts again from the first delimiter character.

To use the `_` (underscore) character as a delimiter instead of `TAB`, you would type:

```
paste -d '_' file1 file2
```

```
Iron Man_Black Widow
Thor_Captain Marvel
Captain America_Dark Phoenix
Hulk_Nebula
Spider Man_
```

Here is an example of using two delimiters:

```
paste -d '%|' file1 file2 file1
```

The lines from the first and the second file are separated with the first character from the delimiters list. The second and the third file lines are separated with the second delimiter.

If more files were given, `paste` starts again from the beginning of the list.

```
Iron Man%Black Widow|Iron Man
Thor%Captain Marvel|Thor
Captain America%Dark Phoenix|Captain America
Hulk%Nebula|Hulk
Spider Man%|Spider Man  
```

The `-s`, `--serial` option tells `paste` to display the lines of one file at a time instead of one line from each file.

```
paste -s file1 file2
```

The command will merge all lines from the given file in separated lines:

```
Iron Man	Thor	Captain America	Hulk	Spider Man
Black Widow	Captain Marvel	Dark Phoenix	Nebula
```

When used with the `-z`, `--zero-terminated` option, `paste` uses a null character to delimit the items instead of the default newline character. This behavior is handy when `paste` is used in combination with `find -print0` and `xargs -0` commands to handle file names containing special characters.

## **Conclusion**

The `paste` command is used to merge corresponding lines of given files.
