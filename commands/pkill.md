# **pkill**

`pkill` is a command-line utility that sends signals to the processes of a running program based on given criteria. The processes can be specified by their full or partial names, a user running the process, or other attributes.

The `pkill` command is a part of the `procps` (or `procps-ng`) package, which is pre-installed on nearly all Linux distributions. `pkill` is basicity a wrapper around the `pgrep` program that only prints a list of matching processes.

## **How to Use the** `pkill` Command

The syntax for the `pkill` command is as follows:

```
pkill [OPTIONS] <PATTERN>
```

The matching `<PATTERN>` is specified using extended regular expressions.

When invoked without any option, `pkill` sends the `15` (`TERM`) signal to the PIDs of all running programs that match with the given name. For example, to gracefully stop all firefox processes, you would run:

```
pkill -15 firefox
```

The command returns `0` when at least one running process matches the requested name. Otherwise, the [exit code](https://linuxize.com/post/bash-exit/) is `1`. This can be useful when writing shell scripts.

To send a different signal to the matched processes, invoke the `pkill` command with the `--signal` option, followed by either the numeric or the symbolic signal name. Another way to send a signal is to run `pkill` followed by the signal name or number prefixed by a hyphen (`-`).

Use the `kill -l` command to list all available signals.

The most commonly used signals are:

* `1` (`HUP`): to reload a process.
* `9` (`KILL`): to kill a process.
* `15` (`TERM`): to gracefully stop a process.

Signals can be specified in three different ways:

* using a number (e.g., -1)
* with the “SIG” prefix (e.g., -SIGHUP)
* without the “SIG” prefix (e.g., -HUP).

For example, to [reload the Nginx](https://linuxize.com/post/start-stop-restart-nginx/) processes you would run:

```
pkill -HUP nginx
```

`pkill` uses regular expressions to match the processes names. It is always a good idea to use the `pgrep` command to print the matched processes before sending signals to them. For instance, to list all processes that contain “ssh” in their names:

```
1039 sshd
2257 ssh-agent
6850 ssh
31279 ssh-agent
```

If you want to send a signal only to the processes which names are exactly as the search pattern, you would use:

```
pkill '^ssh$'
```

The caret (`^`) character matches at the beginning of the string, and the dollar `$` at the end.

By default, `pkill` matches only against the process name. When `-f` option is used, the command matches against full argument lists. If the command contains spaces, quote the entire command:

```
pkill -9 -f "ping 8.8.8.8"
```

Use the `-u` option to tell `pkill` to match processes being run by a given user:

```
pkill -u mark
```

To specify multiple users, separate their names with commas:

```
pkill -u mark,danny
```

You can also combine options and search patterns. For example to send `KILL` signal all processes that run under user “mark” and contains “gnome” in their names you would type:

```
pkill -9 -u mark gnome
```

To display only the least recently (oldest) or the most recently (newest) started processes, use the `-n` (for newest) or the `-o` (for oldest) option.

For example, to kill the most recently created [screen](https://linuxize.com/post/how-to-use-linux-screen/) :

```
pkill -9 -n screen
```
