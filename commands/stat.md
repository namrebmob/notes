# **stat**

`stat` is a command-line utility that displays detailed information about given files or file systems.

This article explains how to use `stat` command.

## **Using the** `stat` Command

The syntax for the `stat` command is as follows:

```
stat [OPTION]... FILE...
```

`stat` accepts one or more input `FILE` names and includes a number of options that control the command behavior and output.

Let’s take a look at the following example:

```
stat file.txt
```

The output will look something like this:

```
  File: file.txt
  Size: 4030      	Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d	Inode: 13633379    Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/   linuxize)   Gid: ( 1000/   linuxize)
Access: 2019-11-06 09:52:17.991979701 +0100
Modify: 2019-11-06 09:52:17.971979713 +0100
Change: 2019-11-06 09:52:17.971979713 +0100
 Birth: -
```

When invoked without any options, `stat` displays the following file information:

* File - The name of the file.
* Size - The size of the file in bytes.
* Blocks - The number of allocated blocks the file takes.
* IO Block - The size in bytes of every block.
* File type - (ex. regular file, directory, symbolic link.)
* Device - Device number in hex and decimal.
* Inode - Inode number.
* Links - Number of hard links.
* Access - [File permissions](https://linuxize.com/post/chmod-command-in-linux/) in the numeric and symbolic methods.
* Uid - User ID and name of the [owner](https://linuxize.com/post/linux-chown-command/) .
* Gid - Group ID and name of the owner.
* Context - The SELinux security context.
* Access - The last time the file was accessed.
* Modify - The last time the file’s content was modified.
* Change - The last time the file’s attribute or content was changed.
* Birth - File creation time (not supported in Linux).

## **Displaying Information About the File System**

To get information about the file system where the given file resides, instead of information about the file itself, use the `-f`, (`--file-system`) option:

```
stat -f file.txt
```

The output of the command will look like this:

```
  File: "package.json"
    ID: 8eb53097b4494d20 Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 61271111   Free: 25395668   Available: 22265851
Inodes: Total: 15630336   Free: 13979610
```

When `stat` is invoked with the `-f` option, it shows the following information:

* File - The name of the file.
* ID - File system ID in hex.
* Namelen - Maximum length of file names.
* Fundamental block size - The size of each block on the file system.
* Blocks:
  * Total - Number of total blocks in the file system.
  * Free - Number of free blocks in the file system.
  * Available - Number of free blocks available to non-root users.
* Inodes:
  * Total - Number of total inodes in the file system.
  * Free - Number of free inodes in the file system.

## **Dereference (Follow) Symlinks**

By default, `stat` does not follow [symlinks](https://linuxize.com/post/how-to-create-symbolic-links-in-linux-using-the-ln-command/) . If you run the command on a symlink the output will include information about the symlink, not the file it points to:

```
stat /etc/resolv.conf
```

```
  File: /etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf
  Size: 39        	Blocks: 0          IO Block: 4096   symbolic link
Device: 801h/2049d	Inode: 8126659     Links: 1
Access: (0777/lrwxrwxrwx)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2019-11-06 21:12:26.875956073 +0100
Modify: 2018-07-24 11:11:48.128794519 +0200
Change: 2018-07-24 11:11:48.128794519 +0200
 Birth: -
```

To dereference (follow) the symlink and display information about the file to which the symlink points, use the `-L`, (`--dereference`) option:

```
stat -L /etc/resolv.conf
```

```
  File: /etc/resolv.conf
  Size: 715       	Blocks: 8          IO Block: 4096   regular file
Device: 17h/23d	Inode: 989         Links: 1
Access: (0644/-rw-r--r--)  Uid: (  101/systemd-resolve)   Gid: (  103/systemd-resolve)
Access: 2019-11-06 20:35:25.603689619 +0100
Modify: 2019-11-06 20:35:25.555689733 +0100
Change: 2019-11-06 20:35:25.555689733 +0100
 Birth: -
```

## **Customizing the Output**

The `stat` command has two options that allow you to customize the output according to your needs: `-c`, (`--format="format"`) and `--printf="format"`.

The difference between these two options is that when two or more files are used as operants `--format` automatically adds a newline after each operand’s output. The `--printf` interprets backslash escapes.

There are many format directives for files and file systems that can be used with `--format` and `--printf`.

For example, to view only the type of the file, you would run:

```
stat --format="%F" /dev/null
```

```
character special file
```

You can combine any number of formatting directives and optionally use custom separators between them. The separator can be a single character or a string:

```
stat --format="%n,%F" /dev/null
```

```
/dev/null,character special file
```

To interpret special characters like newline or tab, use the `--printf` option:

```
stat --printf='Name: %n\nPermissions: %a\n' /etc
```

`\n` prints a new line:

```
Name: /etc
Permissions: 755
```

The `stat` can also display the information in the terse form. This format is useful for parsing by other utilities.

Invoke the command with `-t` (`--terse`) option to print the output in the terse form:

```
stat -t /etc
```

```
/etc 12288 24 41ed 0 0 801 8126465 147 0 0 1573068933 1573068927 1573068927 0 4096
```

For a complete list of all format directives for files and file systems type, `man stat` or `stat --help` in your terminal.

## **Conclusion**

The `stat` command prints information about given files and file systems.

In Linux, several other commands can display information about given files, with `ls` being the most used one, but it shows only a chunk of the information provided by the `stat` command.
