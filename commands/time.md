# **time**

The time command is used to determine how long a given command takes to run. It is useful for testing the performance of your scripts and commands.

For example, if you have two different scripts doing the same job and you want to know which one performs better you can use the Linux time command to determine the duration of execution of each script.

**Time Command Versions**

Both Bash and Zsh, the most widely used Linux shells have their own built-in versions of the time command which take precedence over the Gnu time command.

You can use the[ ](https://linuxize.com/post/linux-type-command/)`type` command to determine whether time is a binary or a built-in keyword.

```bash
type time
```

```shell
# Bash
time is a shell keyword

# Zsh
time is a reserved word

# GNU time (sh)
time is /usr/bin/time
```

To use the Gnu time command, you need to specify the full path to the time binary, usually `/usr/bin/time`, use the `env` command or use a leading backslash `\time` which prevents both and built-ins from being used.

The Gnu time allows you to format the output and provides other useful information like memory I/O and IPC calls.

## **Using Linux Time Command**

In the following example, we are going to measure the time taken to download the [Linux kernel](https://www.kernel.org/) using the [wget tool](https://linuxize.com/post/wget-command-examples/) :

```bash
time wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.9.tar.xz
```

What will be printed as an output depends on the version of the time command you’re using:

```shell
# Bash
real	0m33.961s
user	0m0.340s
sys	0m0.940s

# Zsh
0.34s user 0.94s system 4% cpu 33.961 total

# GNU time (sh)
0.34user 0.94system 0:33.96elapsed 4%CPU (0avgtext+0avgdata 6060maxresident)k
0inputs+201456outputs (0major+315minor)pagefaults 0swaps
```

* **real** or **total** or **elapsed** (wall clock time) is the time from start to finish of the call. It is the time from the moment you hit the `Enter` key until the moment the `wget` command is completed.
* **user** - amount of CPU time spent in user mode.
* **system** or **sys** - amount of CPU time spent in kernel mode.

## **Conclusion**

By now you should have a good understanding of how to use the time command. If you want to learn more about the Gnu time command visit the [time man](http://man7.org/linux/man-pages/man1/time.1.html) page.
