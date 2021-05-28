# **cut**

There are many utilities available in Linux and Unix systems that allow you to process and filter text files. `cut` is a command-line utility that allows you to cut parts of lines from specified files or piped data and print the result to standard output. It can be used to cut parts of a line by delimiter, byte position, and character.

In this article, we will show you how to use the `cut` command through practical examples and detailed explanations of the most common options.

## **How to Use the** `cut` Command

The syntax for the `cut` command is as follows:

```shell
cut OPTION... [FILE]...
```

The options that tell `cut` whether to use a delimiter, byte position, or character when cutting out selected portions the lines are as follows:

* `-f` (`--fields=LIST`) - Select by specifying a field, a set of fields, or a range of fields. This is the most commonly used option.
* `-b` (`--bytes=LIST`) - Select by specifying a byte, a set of bytes, or a range of bytes.
* `-c` (`--characters=LIST`) - Select by specifying a character, a set of characters, or a range of characters.

You can use one, and only one of the options listed above.

Other options are:

* `-d` (`--delimiter`) - Specify a delimiter that will be used instead of the default “TAB” delimiter.
* `--complement` - Complement the selection. When using this option `cut` displays all bytes, characters, or fields except the selected.
* `-s` (`--only-delimited`) - By default `cut` prints the lines that contain no delimiter character. When this option is used, `cut` doesn’t print lines not containing delimiters.
* `--output-delimiter` - The default behavior of `cut` is to use the input delimiter as the output delimiter. This option allows you to specify a different output delimiter string.

The `cut` command can accept zero or more input FILE names. If no `FILE` is specified, or when `FILE` is `-`, `cut` will read from the standard input.

The `LIST` argument passed to the `-f`, `-b`, and `-c` options can be an integer, multiple integers separated by commas, a range of integers or multiple integer ranges separated by commas. Each range can be one of the following:

* `N` the Nth field, byte or character, starting from 1.
* `N-` from the Nth field, byte or character, to the end of the line.
* `N-M` from the Nth to the Mth field, byte, or character.
* `-M` from the first to the Mth field, byte, or character.

## **How to Cut by Field**

To specify the fields that should be cut invoke the command with the `-f` option. When not specified, the default delimiter is “TAB”.

In the examples below, we will use the following file. The fields are separated by tabs.

test.txt

```
245:789 4567    M:4540  Admin   01:10:1980
535:763 4987    M:3476  Sales   11:04:1978
```

For example, to display the 1st and the 3rd field you would use:

```shell
cut test.txt -f 1,3
```

```shell
245:789	M:4540
535:763	M:3476
```

Or if you want to display from the 1st to the 4th field:

```shell
cut test.txt -f -4
```

```shell
245:789	4567	M:4540	Admin
535:763	4987	M:3476	Sales
```

### **How to cut based on a delimiter**

To cut based on a delimiter, invoke the command with the `-d` option, followed by the delimiter you want to use.

For example, to display the 1st and 3rd fields using “:” as a delimiter, you would type:

```shell
cut test.txt -d ':' -f 1,3
```

```shell
245:4540	Admin	01
535:3476	Sales	11
```

You can use any single character as a delimiter. In the following example, we are using the space character as a delimiter and printing the 2nd field:

```shell
echo "Lorem ipsum dolor sit amet" | cut -d ' ' -f 2
```

```shell
ipsum
```

### **How to complement the selection**

To complement the selection field list use `--complement` option. This will print only those fields that are not selected with the `-f` option.

The following command will print all field except the 1st and 3rd:

```shell
cut test.txt -f 1,3 --complement
```

```shell
4567	Admin	01:10:1980
4987	Sales	11:04:1978
```

### **How to specify an output delimiter**

To specify the output delimiter use the `--output-delimiter` option. For example, to set the output delimiter to `_` you would use:

```shell
cut test.txt -f 1,3 --output-delimiter='_'
```

```shell
245:789_M:4540
535:763_M:3476
```

## **How to Cut by Bytes and Characters**

Before going any further, let’s make a distinction between bytes and characters.

One byte is 8 bits and can represent 256 different values. When the ASCII standard was established, it took into account all of the letters, numbers, and symbols necessary to work with English.

The ASCII character table has 128 characters, and each character is represented by one byte. When computers started to become globally accessible, tech companies began to introduce new character encodings for different languages.

For languages that have more than 256 characters, a simple 1 to 1 mapping was not possible. This leads to different problems such as sharing documents or browsing websites, and a new Unicode standard that can handle most of the world’s writing systems was needed. UTF-8 was created to solve these problems.

In UTF-8, not all characters are represented with 1 byte. Characters can be represented with 1 byte to 4 bytes.

The `-b` (`--bytes`) option tells the command to cut sections from each line specified by given byte positions.

In the following examples, we are using the `ü` character that takes 2 bytes.

Select the 5th byte:

```shell
echo 'drüberspringen' | cut -b 5
```

```shell
b
```

Select the 5th, 9th, and 13th bytes:

```shell
echo 'drüberspringen' | cut -b 5,9,13
```

```shell
bpg
```

Select the range from 1st to 5th byte:

```shell
echo 'drüberspringen' | cut -b 1-5
```

```shell
drüb
```

At the time of writing this article, the version of `cut` bundled in GNU coreutils doesn’t have an option to cut by characters. When using the `-c` option, `cut` behaves the same as when using the `-b` option.

## **Cut Examples**

The `cut` command is usually used in combination with other commands through piping. Here are a few examples:

### **Get a list of all users**

The output of the `getent passwd` command is passed to `cut`, which prints the 1st field using `:` as delimiter.

```shell
getent passwd | cut -d ':' -f1
```

The output shows a [list of all system users](https://linuxize.com/post/how-to-list-users-in-linux/).

### **View 10 most frequently used commands**

In the following example, `cut` is used to strip the first 8 bytes from each line of the `history` command output.

```shell
history | cut -c8- | sort | uniq -c | sort -rn | head
```

## **Conclusion**

`cut` command is used to display selected fields from each line of given files or the standard input.

Although very useful, `cut` has some limitations. It doesn’t support specifying more than one characters as a delimiter and it doesn’t support multiple delimiters.
