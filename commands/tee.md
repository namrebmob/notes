# **tee**

The `tee` command reads from the standard input and writes to both standard output and one or more files at the same time. `tee` is mostly used in combination with other commands through piping.

In this article, we’ll cover the basics of using the `tee` command.

## `tee` Command Syntax

The syntax for the `tee` command is as follows:

```shell
tee [OPTIONS] [FILE]
```

* `OPTIONS` :
* `-a` (`--append`) - Do not overwrite the files instead append to the given files.
* `-i` (`--ignore-interrupts`) - Ignore interrupt signals.
* Use `tee --help` to view all available options.
* `FILE_NAMES` - One or more files. Each of which the output data is written to.

## **How to Use the** `tee` Command

The most basic usage of the `tee` command is to display the standard output (`stdout`) of a program and write it in a file.

In the following example, we are using the `df` command to get information about the amount of available disk space on the file system. The output is piped to the `tee` command, which displays the output to the terminal and writes the same information to the file `disk_usage.txt`.

```shell
df -h | tee disk_usage.txt
```

```
Filesystem      Size  Used Avail Use% Mounted on
dev             7.8G     0  7.8G   0% /dev
run             7.9G  1.8M  7.9G   1% /run
/dev/nvme0n1p3  212G  159G   43G  79% /
tmpfs           7.9G  357M  7.5G   5% /dev/shm
tmpfs           7.9G     0  7.9G   0% /sys/fs/cgroup
tmpfs           7.9G   15M  7.9G   1% /tmp
/dev/nvme0n1p1  511M  107M  405M  21% /boot
/dev/sda1       459G  165G  271G  38% /data
tmpfs           1.6G   16K  1.6G   1% /run/user/120
```

You can view the content of the `disk_usage.txt` file using the [cat command](https://linuxize.com/post/linux-cat-command/) .

## **Write to Multiple File**

The `tee` command can also write to multiple files. To do so, specify a list of files separated by space as arguments:

```shell
command | tee file1.out file2.out file3.out
```

## **Append to File**

By default, the `tee` command will overwrite the specified file. Use the `-a` (`--append`) option to [append the output to the file](https://linuxize.com/post/bash-append-to-file/) :

```shell
command | tee -a file.out
```

## **Ignore Interrupt**

To ignore interrupts use the `-i` (`--ignore-interrupts`) option. This is useful when stopping the command during execution with `CTRL+C` and want `tee` to exit gracefully.

```shell
command | tee -i file.out
```

## **Hide the Output**

If you don’t want `tee` to write to the standard output, you can redirect it to `/dev/null`:

```shell
command | tee file.out >/dev/null
```

## **Using tee in Conjunction with sudo**

Let’s say you want to write to a file that is owned by root as a sudo user. The following command will fail because the redirection of the output is not performed by sudo. The redirection is executed as the unprivileged user.

```shell
sudo echo "newline" > /etc/file.conf
```

The output will look something like this:

```shell
bash: /etc/file.conf: Permission denied
```

Simply prepend `sudo` before the `tee` command as shown below:

```shell
echo "newline" | sudo tee -a /etc/file.conf
```

`tee` will receive the output of the [echo command](https://linuxize.com/post/echo-command-in-linux-with-examples/) , elevate to sudo permissions and write to the file.

Using `tee` in conjunction with `sudo` allows you to write to files owned by other users.

## **Conclusion**

The `tee` command reads from standard input and writes it to standard output and one ore more files.
