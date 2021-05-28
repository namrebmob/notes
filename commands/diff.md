# **diff**

`diff` is a command-line utility that allows you to compare two files line by line. It can also compare the contents of directories.

The `diff` command is most commonly used to create a patch containing the differences between one or more files that can be applied using the `patch` command.

## **How to Use the** `diff` Command

The syntax for the `diff` command is as follows:

```
diff [OPTION]... FILES
```

The `diff` command can display the output in several formats with the normal, context, and unified format being the most common ones. The output includes information about which lines in the files must be changed so that they become identical. If the files match, no output is produced.

To save the command output to a file, use the redirection operator:

```
diff file1 file2 > patch
```

In this article, we’ll use the following two files to explain how the `diff` command works:

file1

```
Ubuntu
Arch Linux
Debian
CentOS
Fedora
```

file2

```
Kubuntu
Ubuntu
Debian
Arch Linux
Centos
Fedora
```

## **Normal Format**

In its simplest form when the `diff` command is run on two text files without any option, it produces an output in the normal format:

```
diff file1 file2
```

The output will look something like this:

```
0a1
> Kubuntu
2d2
< Arch Linux
4c4,5
< CentOS
---
> Arch Linux
> Centos
```

The normal output format consists of one or more sections that describe the differences. Each section looks like this:

```
change-command
< from-file-line...
---
> to-file-line...
```

`0a1`, `2d2` and `4c4,5` are change commands. Each change command contains the following, from left to right:

* The line number or range of lines in the first file.
* A special change character.
* The line number or range of lines in the second file.

The change character can be one of the following:

* `a` - Add the lines.
* `c` - Change the lines.
* `d` - Delete the lines.

The change command is followed by the complete lines that are removed (`<`) and added to the file (`>`).

Let’s explain the output:

* `0a1` - Add line `1` of the second file at the beginning of the file1 (after the line `0`).
  * `> Kubuntu` - The line from the second line that is added to the first file as described above.
* `2d2` - Delete line `2` in the first file. The `2` after the `d` symbol means that if the line is not deleted it would appear on line `2` in the second file.
  * `< Arch Linux` - the deleted line.
* `4c4,5` - Replace (change) line `5` in the first file with lines `4-5` from the second file.
  * `< CentOS` - The line in the first file to be replaced.
  * `---` - Separator.
  * `> Arch Linux` and `> Centos` - Lines from the second file replacing the line in the first file.

## **Context Format**

When the context output format is used, the `diff` command displays several lines of context around the lines that differ between the files.

The `-c` option tells `diff` to produce output in the context format:

```
diff -c file1 file2
```

```
*** file1	2019-11-25 21:00:26.422426523 +0100
--- file2	2019-11-25 21:00:36.342231668 +0100
***************
*** 1,6 ****
  Ubuntu
- Arch Linux
  Debian
! CentOS
  Fedora
  
--- 1,7 ----
+ Kubuntu
  Ubuntu
  Debian
! Arch Linux
! Centos
  Fedora
```

The output starts with the names and the timestamps if the files that are compared, and one or more sections that describe the differences. Each section looks like this:

```
***************
*** from-file-line-numbers ****
  from-file-line...
--- to-file-line-numbers ----
  to-file-line...
```

* `from-file-line-numbers` and `to-file-line-numbers` - The line numbers or comma-separated range of lines in the first and second file, respectively.
* `from-file-line` and `to-file-line` - The lines that differ and the lines of context:
  * Lines starting with two spaces are lines of context, the lines that are the same in both files.
  * Lines starting with the minus symbol (`-`) are the lines that correspond to nothing in the second file. Lines missing in the second file.
  * Lines starting with the plus symbol (`+`) are the lines that correspond to nothing in the first file. Lines missing in the first file.
  * Lines starting with the exclamation mark (`!`) are the lines that are changed between two files. Each group of lines starting with `!` from the first file has a corresponding match in the second file.

Let’s explain the most important parts of the output:

* In this example we have only one section describing the differences.
* `1,6 *` and `--- 1,7 ----` tells us the range of the lines from the first and second files that are included in this section.
* Lines `Ubuntu`, `Debian`, `Fedora`, and the last empty line are the same in both files. These lines are starting with double space.
* Line `- Arch Linux` from the first file corresponds to nothing in the second file. Although this line also exists in the second file, the positions are different.
* Line `+ Kubuntu` from the second file corresponds to nothing in the first file.
* Line `! CentOS` from the first file and lines `! Arch Linux` and `! CentOS` from the second file are changed between the files.

By default the number of the context lines defaults to three. To specify another number use the `-C` (`--contexts`) option:

```
diff -C 1 file1 file2
```

```
*** file1	2019-11-25 21:00:26.422426523 +0100
--- file2	2019-11-25 21:00:36.342231668 +0100
***************
*** 1,5 ****
  Ubuntu
- Arch Linux
  Debian
! CentOS
  Fedora
--- 1,6 ----
+ Kubuntu
  Ubuntu
  Debian
! Arch Linux
! Centos
  Fedora
```

## **Unified Format**

The unified output format is an improved version of the context format and produces a smaller output.

Use the `-u` option to tell `diff` to print the output in the unified format:

```
diff -u file1 file2
```

```
--- file1	2019-11-25 21:00:26.422426523 +0100
+++ file2	2019-11-25 21:00:36.342231668 +0100
@@ -1,6 +1,7 @@
+Kubuntu
 Ubuntu
-Arch Linux
 Debian
-CentOS
+Arch Linux
+Centos
 Fedora
```

The output begins with the names and the timestamps of the files and one or more sections that describe the differences. Each section takes the following form:

```
***************
@@ from-file-line-numbers to-file-line-numbers @@
 line-from-files...
```

* `@@ from-file-line-numbers to-file-line-numbers @@` - The line number or range of the lines from the first and second files included in this section.
* `line-from-files` - The lines that differ and the lines of context:
  * Lines starting with two spaces are lines of context, the lines that are the same in both files.
  * Lines starting with the minus symbol (`-`) are the lines that are **removed** from the first file.
  * Lines starting with the plus symbol (`+`) are the lines that are **added** from the first file.

## **Ignore case**

As you may notice in the above examples, the `diff` command is case sensitive by default.

Use the `-i` option to tell `diff` to ignores case:

```
diff -ui file1 file2
```

```
--- file1	2019-11-25 21:00:26.422426523 +0100
+++ file2	2019-11-25 21:00:36.342231668 +0100
@@ -1,6 +1,7 @@
+Kubuntu
 Ubuntu
-Arch Linux
 Debian
+Arch Linux
 CentOS
 Fedora
```

## **Conclusion**

Comparing text files for differences is one of the most common tasks for Linux systems administrators.

The `diff` command compares files line by line. For more information, type `man diff` in your terminal.
