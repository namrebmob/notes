# **rename**

Renaming multiple files and directories with the `mv` command can be a tedious process as it involves writing complex commands with pipes, [loops](https://linuxize.com/post/bash-for-loop/) , and so on.

This is where the `rename` command comes handy. It renames the given files by replacing the search expression in their name with the specified replacement.

In this tutorial, we will explain how to use the `rename` command to batch rename files.

## **Installing** `rename`

There are two versions of the `rename` command with different syntax and features. We will be using the Perl version of the `rename` command.

If this version is not installed on your system, use the package manager of your Linux distribution to install it:

* **Ubuntu and Debian**

  ```
  sudo apt updatesudo apt install rename
  ```
* **CentOS and Fedora**

  ```
  sudo yum install prename
  ```
* **Arch Linux**

  ```
  yay perl-rename
  ```

## **Using** `rename`

The following is the general syntax for the `rename` command:

```
rename [OPTIONS] perlexpr files
```

The `rename` command is basically a Perl script. It will rename the given `files` according to the specified `perlexpr` regular expression. You can read about Perl regular expressions [here](https://perldoc.perl.org/perlre.html#Regular-Expressions) .

For example, the following command will change the extension of all `.css` files to `.scss`:

```
rename 's/.css/.scss/' *.css
```

Let’s explain the command in more details:

* `s/search_pattern/replacement/` - The substitution operator.
* `.css` - The search pattern. It is the first argument in the substitution operator. The `rename` command will search for this pattern in the given file name and if found it will replace it with the replacement argument.
* `.scss` - The replacement. The second argument in the substitution operator.
* `*.css` - All files with “.css” extension. Wildcard (`*`) is a symbol used to represent zero, one or more characters.

Before running the actual command and rename the files and directories it is always a good idea to use the `-n` option that will perform a “dry run” and show you what files will be renamed:

```
rename -n 's/.css/.scss/' *.css
```

The output will look something like this:

```
rename(file-0.css, file-0.scss)
rename(file-1.css, file-1.scss)
rename(file-2.css, file-2.scss)
rename(file-3.css, file-3.scss)
rename(file-4.css, file-4.scss)
```

By default, the `rename` command doesn’t overwrite the existing files. Use the `-f` option which tells `rename` to overwrite the existing files:

```
rename -f 's/.css/.scss/' *.css
```

If you want `rename` to print the names of files that are successfully renamed, use the `-v` (verbose) option:

```
rename -v 's/.css/.scss/' *.css
```

```
file-0.css renamed as file-0.scss
file-1.css renamed as file-1.scss
file-2.css renamed as file-2.scss
file-3.css renamed as file-3.scss
file-4.css renamed as file-4.scss
```

## `rename` Examples

Below are a few common examples of how to use the rename command:

### **Replace spaces in filenames with underscores**

```
rename 'y/ /_/' *
```

### **Convert filenames to lowercase**

```
rename 'y/A-Z/a-z/' *
```

### **Convert filenames to uppercase**

```
rename 'y/a-z/A-Z/' *
```

### **Remove** `.bak` from the filenames

```
rename 's/\.bak$//' *.bak
```

### **Rename** `.jpeg` and `.JPG` filenames to `.jpg`

```
rename 's/\.jpe?g$/.jpg/i' *
```
