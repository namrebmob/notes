# **timeout**

`timeout` is a command-line utility that runs a specified command and terminates it if it is still running after a given period of time. In other words, `timeout` allows you to run a command with a time limit. The `timeout` command is a part of the GNU core utilities package, which is installed on almost any Linux distribution.

It is handy when you want to run a command that doesn’t have a built-in timeout option.

In this article, we will explain how to use the Linux `timeout` command.

## **How to Use the** `timeout` Command

The syntax for the `timeout` command is as follows:

```bash
timeout [OPTIONS] DURATION COMMAND [ARG]…
```

The `DURATION` can be a positive integer or a floating-point number, followed by an optional unit suffix:

* `s` - seconds (default)
* `m` - minutes
* `h` - hours
* `d` - days

When no unit is used, it defaults to seconds. If the duration is set to zero, the associated timeout is disabled.

The command options must be provided before the arguments.

Here are a few basic examples demonstrating how to use the `timeout` command:

* Terminate a command after five seconds:

  ```bash
  timeout 5 ping 8.8.8.8
  ```
* Terminate a command after five minutes:

  ```bash
  timeout 5m ping 8.8.8.8
  ```
* Terminate a command after one minute and six seconds:

  ```bash
  timeout 1.1m ping 8.8.8.8
  ```

If you want to run a command that requires elevated privileges such as `tcpdump` , prepend `sudo` before `timeout`:

```bash
sudo timeout 300 tcpdump -n -w data.pcap
```

## **Sending Specific Signal**

If no signal is given, `timeout` sends the `SIGTERM` signal to the managed command when the time limit is reached. You can specify which signal to send using the `-s` (`--signal`) option.

For example, to send `SIGKILL` to the `ping` command after one minute, you would use:

```bash
sudo timeout -s SIGKILL ping 8.8.8.8
```

You can specify the signal by the name, such as `SIGKILL`, or its number like `9`. The following command is identical to the previous one:

```bash
sudo timeout -s 9 ping 8.8.8.8
```

To get a list of all available signals, use the `kill -l` command:

```bash
kill -l
```

## **Killing Stuck Processes**

`SIGTERM`, the default signal sent when the time limit is exceeded, can be caught or ignored by some processes. In those situations, the process continues to run after the termination signal is sent.

To make sure the monitored command is killed, use the `-k` (`--kill-after`) option followed by a time period. When this option is used after the given time limit is reached, the `timeout` command sends the `SIGKILL` signal to the managed program that cannot be caught or ignored.

In the following example, `timeout` runs the command for one minute, and if it is not terminated, it will kill it after ten seconds:

```bash
sudo timeout -k 10 1m ping 8.8.8.8
```

timeout -k “./test.sh”

killed after the given time limit is reached

## **Preserving the Exit Status**

`timeout` returns `124` when the time limit is reached. Otherwise, it returns the [exit status](https://linuxize.com/post/bash-exit/) of the managed command.

To return the exit status of the command even when the time limit is reached, use the `--preserve-status` option:

```bash
timeout --preserve-status 5 ping 8.8.8.8
```

## **Running in Foreground**

By default, `timeout` runs the managed command in the background. If you want to run the command in the foreground, use the `--foreground` option:

```bash
timeout --foreground 5m ./script.sh
```

This option is useful when you want to run an interactive command that requires user input.
