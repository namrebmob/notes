# **su**

The `su` (short for substitute or switch user) utility allows you to run commands with another user’s privileges, by default the root user.

Using `su` is the simplest way to switch to the administrative account in the current login session. This is especially handy when the root user is not allowed to log in to the system through [ssh](https://linuxize.com/post/ssh-command-in-linux/) or using the GUI display manager.

In this tutorial, we will explain how to use the `su` command.

## **How to Use the** `su` Command

The general syntax for the `su` command is as follows:

```
su [OPTIONS] [USER [ARGUMENT...]]
```

When invoked without any option, the default behavior of `su` is to run an interactive shell as root:

```
su
```

You will be prompted to enter the root password, and if authenticated, the user running the command temporarily becomes root.

The session shell (`SHELL`) and home (`HOME`) [environment variables](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/) are set from substitute user’s `/etc/passwd` entry, and the current directory is not changed.

To confirm that the user is changed, use the `whoami` command:

```
whoami
```

The command will print the name of the user running the current shell session:

```
root
```

The most commonly used option when invoking `su` is `-` (`-l`, `--login`). This makes the shell a login shell with an environment very similar to a real login and changes the [current directory](https://linuxize.com/post/current-working-directory/) :

```
su -
```

If you want to run another shell instead of the one defined in the `passwd` file, use the `-s`, `--shell` option. For example, to switch to root and to run the `zsh` shell, you would type:

```
su -s /usr/bin/zsh
```

To preserve the entire environment (`HOME`, `SHELL`, `USER`, and `LOGNAME`) of the calling user, invoke the command with the `-p`, `--preserve-environment` option.

```
su -p
```

When the `-` option is used, `-p` is ignored.

If you want to run a command as the substitute user without starting an interactive shell, use the `-c`, `--command` option. For example, to invoke the `ps` command as root, you would type:

```
su -c ps
```

To switch to another user account, pass the user name as an argument to `su`. For example, to switch to the user `tyrion` you would type:

```
su tyrion
```

## **Sudo vs. Su**

On some Linux distributions like Ubuntu, the [root user account](https://linuxize.com/post/how-to-enable-and-disable-root-user-account-in-ubuntu/) is disabled by default for security reasons. This means that no password is set for root, and you cannot use `su` to switch to root.

One option to change to root would be to prepend the `su` command with `sudo` and enter the currently logged in user password:

```
sudo su -
```


The `sudo` command allows you to run programs as another user, by default the root user.

If the user is granted with `sudo` assess, the `su` command is invoked as root. Running `sudo su -` and then typing the user password has the same effect the same as running `su -` and typing the root password.

When used with the `-i` option, `sudo` run an interactive login shell with the root user’s environment:

```
sudo -i
```

`sudo -i` is basically the same as running `su -`.

The advantage of using `sudo` over `su` is that the root password doesn’t need to be shared among multiple administrative user accounts.

With `sudo` you can also allow users to run only specific programs with root privileges.

## **Conclusion**

`su` is a command-line utility that allows you to temporarily become another user and execute commands with the substitute user.
