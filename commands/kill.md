# **kill**

Linux is a great and advanced operating system, but it is not perfect. Once in a while, some applications may start behaving erratically and become unresponsive or start consuming a lot of system resources. Unresponsive applications cannot be restarted because the original application process never shuts down completely. The only solution is to either restart the system or kill the application process.

There are several utilities that allow you to terminate errant processes with `kill` the being the most commonly used.

## `kill` Command

`kill` is a shell builtin in most Bourne-derived shells such as Bash and Zsh. The command behavior is slightly different between the shells and the standalone `/bin/kill` executable.

Use the `type` command to display all locations on your system containing `kill`:

```
type -a kill
```

```
kill is a shell builtin
kill is /bin/kill
```

The output above tells that the shell builtin has priority over the standalone executable, and it is used whenever you type `kill`. If you want to use the binary, type the full path to the file `/bin/kill`. In this article, we will use the Bash builtin.

The syntax of the `kill` command takes the following form:

```
kill [OPTIONS] [PID]...
```

The `kill` command sends a signal to specified processes or process groups, causing them to act according to the signal. When the signal is not specified, it defaults to `-15` (-TERM).

The most commonly used signals are:

* `1` (`HUP`) - Reload a process.
* `9` (`KILL`) - Kill a process.
* `15` (`TERM`) - Gracefully stop a process.

To get a list of all available signals, invoke the command with the `-l` option:

```
kill -l
```

![kill-a-process-in-linux](https://linuxize.com/post/kill-command-in-linux/kill-a-process-in-linux_hu69063a613c0e8baf3bfb360622079274_108622_768x0_resize_q75_lanczos.jpg?ezimgfmt=rs:726x266/rscb87/ng:webp/ngcb87)

Signals can be specified in three different ways:

1. Using number (e.g., `-1` or `-s 1`).
2. Using the “SIG” prefix (e.g., `-SIGHUP` or `-s SIGHUP`).
3. Without the “SIG” prefix (e.g., `-HUP` or `-s HUP`).

The following commands are equivalent to one another:

```
kill -1 PID_NUMBERkill -SIGHUP PID_NUMBERkill -HUP PID_NUMBER
```

The PIDs provided to the `kill` command can be one of the following:

* If `PID` is greater than zero, the signal is sent to the process with ID equal to the `PID`.
* If `PID` is equal to zero, the signal is sent to all processes in the current process group. In other words, the signal is sent to all processes belonging to the GID of the shell that invoked the `kill` command. Use `ps -efj` command to view the process group IDs (GIDs).
* If `PID` is equal to `-1`, the signal is sent to all processes with the same UID as the user invoking the command. If the invoking user is root, the signal is sent to all processes except init and the `kill` process itself.
* If `PID` is less than `-1`, the signal is sent to all processes in the process group eq with GID equal to the absolute value of the `PID`.

Regular users can send signals to their own processes, but not those that belong to other users, while the root user can send signals to other user’s processes.

## **Terminating Processes Using the** `kill` Command

To terminate or [kill a process](https://linuxize.com/post/how-to-kill-a-process-in-linux/) with the `kill` command, first you need to find the process ID number (PID). You can do this using different commands such as `top`, `ps` , `pidof` and `pgrep` .

Let’s say the Firefox browser has become unresponsive, and you need to kill the Firefox process. To find the browser PIDs use the `pidof` command:

```
pidof firefox
```

The command will print the IDs of all Firefox processes:

```
6263 6199 6142 6076
```

Once you know the processes numbers, you can kill all of them by sending the `TERM` signal:

```
kill -9 6263 6199 6142 6076
```

Instead of searching for PIDs and then killing the processes, you can combine the above commands into one:

```
kill -9 $(pidof firefox)
```

## **Reloading Processes Using the** `kill` Command

Another common use case for `kill` is to send the `HUP` signal, which tells the processes to reload its settings.

For example, to [reload Nginx](https://linuxize.com/post/nginx-commands-you-should-know/) , you need to send a signal to the master process. The process ID of the Nginx master process can be found in the `nginx.pid` file, which is typically is located in the `/var/run` directory.

Use the `cat` command to find the master PID:

```
cat /var/run/nginx.pid
```

```
30251
```

Once you found the master PID reload the Nginx settings by typing:

```
sudo kill -1 30251
```

The command above must be run as root or user with [sudo](https://linuxize.com/post/sudo-command-in-linux/) privileges.

## **Conclusion**

The `kill` command is used to send a signal to processes. The most frequently-used signal is `SIGKILL` or `-9`, which terminates the given processes.
