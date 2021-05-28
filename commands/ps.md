# **ps**

In Linux, a running instance of a program is called process. Occasionally, when working on a Linux machine, you may need to find out what processes are currently running.

There are number of commands that you can use to find information about the running processes, with `ps`, `pstree` , and `top` being the most commonly used ones.

This article explains how to use the `ps` command to list the currently running processes and display information about those processes.

## **How to Use** `ps` Command

The general syntax for the `ps` command is as follows:

```
ps [OPTIONS]
```

For historical and compatibility reasons, the `ps` command accepts several different types of options:

* UNIX style options, preceded by a single dash.
* BSD style options, used without a dash.
* GNU long options, preceded by two dashes.

Different option types can be mixed, but in some particular cases, conflicts can appear, so it is best to stick with one option type.

BSD and UNIX options can be grouped.

In it’s simplest form, when used without any option, `ps` will print four columns of information for minimum two processes running in the current shell, the shell itself, and the processes that run in the shell when the command was invoked.

```
ps
```

The output includes information about the shell (`bash`) and the process running in this shell (`ps`, the command that you typed):

```
  PID TTY          TIME CMD
 1809 pts/0    00:00:00 bash
 2043 pts/0    00:00:00 ps
```

The four columns are labeled `PID`, `TTY`, `TIME`, and `CMD`.

* `PID` - The process ID. Usually, when running the `ps` command, the most important information the user is looking for is the process PID. Knowing the PID allows you to [kill a malfunctioning process](https://linuxize.com/post/how-to-kill-a-process-in-linux/) .
* `TTY` - The name of the controlling terminal for the process.
* `TIME` - The cumulative CPU time of the process, shown in minutes and seconds.
* `CMD` - The name of the command that was used to start the process.

The output above is not very useful as it doesn’t contain much information. The real power of the `ps` command comes when launched with additional options.

The `ps` command accepts a vast number of options that can be used to display a specific group of processes and different information about the process, but only a handful are needed in day-to-day usage.

`ps` is most frequently used with the following combination of options:

**BSD form**:

```
ps aux
```

* The `a` option tells `ps` to display the processes of all users. Only the processes that not associated with a terminal and processes of group leaders are not shown.
* `u` stands for a user-oriented format that provides detailed information about the processes.
* The `x` option instructs `ps` to list the processes without a controlling terminal. Those are mainly processes that are started on boot time and [running in the background](https://linuxize.com/post/how-to-run-linux-commands-in-background/) .

The command displays information in eleven columns labeled `USER`, `PID`, `%CPU`, `%MEM`, `VSZ`, `RSS`, `STAT`, `START`, `TTY`, `TIME`, and `CMD`.

```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.8  77616  8604 ?        Ss   19:47   0:01 /sbin/init
root         2  0.0  0.0      0     0 ?        S    19:47   0:00 [kthreadd]
...
```

We already explained `PID`, `TTY`, `TIME` and `CMD` labels. Here is an explanation of other labels:

* `USER` - The user who runs the process.
* `%CPU` - The [cpu](https://linuxize.com/post/get-cpu-information-on-linux/) utilization of the process.
* `%MEM` - The percentage of the process’s resident set size to the physical memory on the machine.
* `VSZ` - Virtual memory size of the process in KiB.
* `RSS` - The size of the physical [memory](https://linuxize.com/post/free-command-in-linux/) that the process is using.
* `STAT` - The the process state code, such as `Z` (zombie), `S` (sleeping), and `R` (running).
* `START` - The time when the command started.

The `f` option tells `ps` to display a tree view of parent to child processes:

```
ps auxf
```

The `ps` command also allows you to sort the output. For example, to sort the output based on the [memory usage](https://linuxize.com/post/check-memory-linux/) , you would use:

```
ps aux --sort=-%mem
```

**UNIX form**:

```
ps -ef
```

* The `-e` option instructs `ps` to display all processes.
* The `-f` stands full-format listing, which provides detailed information about the processes.

The command displays information in eight columns labeled `UID`, `PID`, `PPID`, `C`, `STIME`, `TIME`, and `CMD`.

```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 19:47 ?        00:00:01 /sbin/init
root         2     0  0 19:47 ?        00:00:00 [kthreadd]
...
```

The labels that are not already explained have the following meaning:

* `UID` - Same as `USER`, the user who runs the process.
* `PPID` - The ID of the parent process.
* `C` - Same as `%CPU`, the process CPU utilization.
* `STIME` - Same as `START`, the time when the command started.

To view only the processes running as a specific user, type the following command, where `linuxize` is the name of the user:

```
ps -f -U linuxize -u linuxize
```

## **User-defined Format**

The `o` option allows you to specify which columns are displayed when running the `ps` command.

For example, to print information only about the `PID` and `COMMAND`, you would run one of the following commands:

```
ps -efo pid,comm
```

```
ps auxo pid,comm
```

## **Using** `ps` With Other Commands

`ps` can be used in combination with other commands through piping.

If you want to display the output of the `ps` command, one page at a time pipe it to the `less` command:

```
ps -ef | less
```

The output of the `ps` command can be filtered with `grep` . For example, to show only the process belonging to the root user you would run:

```
ps -ef | grep root
```

## **Conclusion**

The `ps` command is one of the most commonly used commands when troubleshooting issues on Linux systems. It has many options, but usually, most users are using either `ps aux` or `ps -ef` to gather information about running processes.

For more information about `ps`, type `man ps` in your terminal.
