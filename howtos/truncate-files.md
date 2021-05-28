# **How to Truncate (Empty) Files**

In some situations, you might want to truncate (empty) an existing file to a zero-length. In simple words, truncating a file means removing the file contents without deleting the file.

Truncating a file is much faster and easier than [deleting the file](https://linuxize.com/post/how-to-remove-files-and-directories-using-linux-command-line/) , [recreating](https://linuxize.com/post/create-a-file-in-linux/) it, and setting the correct permissions and [ownership](https://linuxize.com/post/linux-chown-command/) . Also, if the file is opened by a process, removing the file may cause the program that uses it to malfunction.

This tutorial explains how to truncate files to zero size in Linux systems using shell redirection and the `truncate` command.

## **Shell Redirection**

The easiest and most used method to truncate files is to use the `>` shell redirection operator.

The general format for truncating files using redirection is:

```shell
: > filename
```

Let’s break down the command:

* The `:` colon means `true` and produces no output.
* The redirection operator `>` redirect the output of the preceding command to the given file.
* `filename`, the file you want to truncate.

If the [file exists](https://linuxize.com/post/shell-check-if-file-exists/) , it will be truncated to zero. Otherwise, the file will be created.

Instead of `:` can also use another command that produces no output.

Here is an example of using the `cat` command to output the contents of the `/dev/null` device, which returns only an end-of-file character:

```shell
cat /dev/null > filename
```

Another command that can be used is `echo` . The `-n` option tells `echo` not to append a newline:

```shell
echo -n > filename
```

On most modern shells such as shell or Zsh you can omit the command before the redirection symbol and use:

```shell
> filename
```

To be able to truncate a file, you need to have write permissions on the file. Usually, you would use `sudo` for this, but the elevated root privileges do not apply to the redirection. Here is an example:

```shell
sudo : > /var/log/syslog
```

```shell
shell: /var/log/syslog: Permission denied
```

There are several solutions that allow redirecting with `sudo`. The first option can run a new shell with sudo and execute a command inside that shell using the `-c` flag:

```shell
sudo sh -c '> filename'
```

Another option is to pipe the output to the `tee` command, elevate the `tee` privileges with `sudo`, and write the empty output to a given file:

```shell
: | sudo tee filename
```

## `truncate` Command

`truncate` is a command-line utility that allows you to shrink or extend the size of a file to a given size.

The general syntax for truncating files to zero size with the `truncate` command, is as follows:

```shell
truncate -s 0 filename
```

The `-s 0` option sets the file size to zero.

For example, to empty the Nginx access log you would use:

```shell
sudo truncate -s 0 /var/log/nginx/access.log
```

## **Empty All Log Files**

Over time, your disk drive may get cluttered with a lot of [large log files](https://linuxize.com/post/find-large-files-in-linux/) taking up large amounts of disk space.

The following command will empty files ending with “.log” under the `/var/log` directory:

```shell
sudo truncate -s 0 /var/log/**/*.log
```

A better option would be to rotate, compress, and remove the logs files with the `logrotate` tool.

## **Conclusion**

To truncate a file in Linux use the redirection operator `>` followed by the file name.
