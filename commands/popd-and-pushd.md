#  **popd and pushd**

`pushd` and `popd` are commands that allow you to work with directory stack and change the current working directory in Linux and other Unix-like operating systems. Although `pushd` and `popd` are very powerful and useful commands, they are underrated and rarely used.

In this tutorial, we will show you how to use the `pushd` and `popd` commands to navigate your system’s directory tree.

## **Directory Stack**

The directory stack is a list of directories you have previously navigated to. The contents of the directory stack can be seen using the `dirs` command. Directories are added to the stack when changing to a directory using the `pushd` command and removed with the `popd` command.

The current working directory is always on the top of the directory stack. The [current working directory](https://linuxize.com/post/current-working-directory/) is the directory (folder) in which the user is currently working in. Each time you interact with the command line, you are working within a directory.

The `pwd` command allows you to find out what directory you are currently in.

When navigating through the file system, use the `Tab` key to autocomplete the names of directories. Adding a slash at the end of the directory name is optional.

`pushd`, `popd` and `dirs` are shell builtins, and its behavior may slightly differ from shell to shell. We will cover the Bash builtin version of the commands.

## `pushd` Command

The syntax for the `pushd` command is as follows:

```
pushd [OPTIONS] [DIRECTORY]
```

For example to save the current directory to the top of the directory stack and change to `/var/www` you would type:

```
~$ pushd /var/www
```

On success, the command above will print the directory stack. `~` is the directory in which we executed the `pushd` command. The tilde symbol `~` means home directory.

```
/var/www ~
```

`pushd` first saves the current working directory to the top of the stack and then navigates to the given directory. As the current directory must always be on the top of the stack, once changed the new current directory goes to the top of the stack but it is not saved in the stack. To save it you must invoke `pushd` from it. If you use `cd` to change to another directory, the top item of the stack will be lost,

Let’s add another directory to the stack:

```
/var/www$ pushd /opt
```

```
/opt /var/www ~
```

To suppress changing to directory, use the `-n` option. For example, to add the `/usr/local` directory to the stack but not change into it you would type:

```
/opt$ pushd -n /usr/local
```

As the current directory (which is always in the top) is not changed, the `/usr/local` directory is added second from the top of the stack:

```
/opt /usr/local /var/www ~
```

The `pushd` accepts two options, `+N` and `-N` that allows you to navigate to `Nth` directory of the stack. The `+N` option changes to `Nth` element of the stack list counting from left to right starting with zero. When `-N` is used the direction of the count is from right to left.

To better illustrate the options, let’s print the current directory stack:

```
/opt$ dirs -l -v
```

The output will show an indexed list of the directory stack:

```
 0  /opt
 1  /usr/local
 2  /var/www
 3  /home/linuxize
```

If you want to change to the `/var/www` directory, and bring it to the top of the stack you will use one of the following.

When counting from top to bottom (or left to right), the index of the directory is `2`.

```
pushd +2
```

When counting from bottom to top the index of the `/var/www` directory is `1`.

```
pushd -1.
```

When used without any argument, `pushd` will toggle the top two directories and makes the new top the current directory. This is the same as when using the `cd -` command.

## `popd` Command

The `popd` command takes the form:

```
popd [OPTIONS]
```

When used with no argument, `popd` removes the top directory from the stack and navigates to the new top directory.

Let’s say we have the following directory stack:

```
/opt /usr/local /var/www /etc/nginx ~
```

If you run the `popd` command it will remove the `/opt` from the stack and change to the `/usr/local` directory:

```
/opt$ popd
```

The output will show the new directory stack:

```
/usr/local /var/www /etc/nginx ~
```

The `-n` option suppresses the default directory change and removes the second item from the stack:

```
/opt$ popd -n
```

```
/usr/local /etc/nginx ~
```

Same as `pushd`, `popd` also accepts the `+N` and `-N` options that can be used to remove the `Nth` directory of the stack.

```
/opt$ popd +1
```

```
/usr/local ~
```

## **Conclusion**

Normally, you would use the `cd` command to move from one directory to another. However, if you spend a lot of time on the command line, `pushd` and `popd` commands will increase your productivity and efficiency.
