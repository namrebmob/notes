# **du**

The `du` command, short for “disk usage” reports the estimated amount of disk space used by given files or directories. It is practically useful for finding files and directories taking up large amounts of disk space.

## **How to Use the** `du` command

The general syntax for the `du` command is as follows:

```
du [OPTIONS]... FILE...
```

If the given `FILE` is a directory, `du` will summarize disk usage of each file and subdirectory in that directory. If no `FILE` is specified, `du` will report the disk usage of the [current working directory](https://linuxize.com/post/current-working-directory/) .

When executed without any option `du` displays the disk usage of the given file or directory and each of its subdirectories in bytes.

```
du ~/Documents 
```

You can also pass multiple files and directories to the `du` command as arguments:

```
du ~/Documents ~/Pictures ~/.zshrc
```

If you run `du` on a file or directory for which you don’t have permissions, you will get something like “du: cannot read directory”. In this situation, you’ll need to prepend the command with `sudo` .

`du` has lots of options, we’ll outline just the most frequently used ones.

The `-a` option tells `du` to report the disk space usage of each file within the directory.

```
du -a ~/Documents 
```

Usually, you would want to display only the space occupied by the given directory in a human-readable format. To do that, use the `-h` option.

For example, to get the total size of the `/var/lib` and all of its subdirectories, you would run the following command:

```
sudo du -h /var
```

We are using `sudo` because most of the files and directories inside the `/var/lib` directory are owned by the root user and are not readable by the regular users. The output will look something like this:

```
...
4.0K	/var/lib/apt/mirrors/partial
8.0K	/var/lib/apt/mirrors
205M	/var/lib/apt
2.9G	/var/lib/
```

To report only the total size of the specified directory, and not for subdirectories use the `-s` option:

```
sudo du -sh /var
```

```
2.9G	/var
```

The `-c` option tells `du` to report a grand total. This is useful when you want to get the combined size of two or more directories.

```
sudo du -csh /var/log /var/lib
```

```
1.2G	/var/log
2.9G	/var/lib
4.1G	total
```

If you want to display the disk usage of the n-level subdirectories use the `--max-depth` option and specify the subdirectories level. For example, to get a report about the first-level directories you would use:

```
sudo du -h --max-depth=1 /var/lib
```

```
...
544K	/var/lib/usbutils
4.0K	/var/lib/acpi-support
205M	/var/lib/apt
2.9G	/var/lib
```

The default behavior of the `du` utility is to re the disk space used by the directory or file. To find the apparent size of a file, use the `--apparent-size` switch. The “apparent size” of a file is how much data is actually in the file.

```
sudo du -sh --apparent-size /var/lib
```

```
2.9G	/var/lib
```

`du` also allows you to use shell pattern. For example, to get the size of all directories starting with “Do” in your home directory you would run:

```
sudo du -csh ~/Do*
```

```
102M	/home/linuxize/Documents
358M	/home/linuxize/Downloads
460M	total
```

## **Using** `du` with Other Commands

The `du` command can be combined with other commands with pipes.

For example, to print the 5 [largest directories](https://linuxize.com/post/find-large-files-in-linux/) inside the `/var` directory you would pass the output of `du` to the `sort` command to sort the directories by their size and then pipe the output to the `head` command which will print only the top 5 directories:

```
sudo du -h /var/ | sort -rh | head -5
```

```
4.6G	/var/
2.9G	/var/lib
2.6G	/var/lib/snapd
1.7G	/var/lib/snapd/snaps
1.2G	/var/log/journal/af8ce1d394b844fea8c19ea5c6a9bd09
```
