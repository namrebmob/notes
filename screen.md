# screen

# How To Use Linux Screen

Have you ever faced the situation where you perform a long-running task on a remote machine, and suddenly your connection drops, the SSH session is terminated, and your work is lost. Well, it has happened to all of us at some point, hasn’t it? Luckily, there is a utility called `screen` that allows us to resume the sessions.

## Introduction

Screen or GNU Screen is a terminal multiplexer. In other words, it means that you can start a screen session and then open any number of windows (virtual terminals) inside that session. Processes running in Screen will continue to run when their window is not visible even if you get disconnected.

## Install Linux GNU Screen

The screen package is pre-installed on most Linux distros nowadays. You can check if it is installed on your system by typing:

```sh
screen --version
```

```sh
Screen version 4.06.02 (GNU) 23-Oct-17
```

If you don’t have screen installed on your system, you can easily install it using the package manager of your distro.

### Install Linux Screen on Ubuntu and Debian

```sh
sudo apt updatesudo apt install screenCopyCopy
```

### Install Linux Screen on CentOS and Fedora

```sh
sudo yum install screenCopy
```

## Starting Linux Screen

To start a screen session, simply type `screen` in your console:

```sh
screen
```

This will open a screen session, create a new window, and start a shell in that window.

Now that you have opened a screen session, you can get a list of commands by typing:

`Ctrl+a` `?`

### Starting Named Session

Named sessions are useful when you run multiple screen sessions. To create a named session, run the screen command with the following arguments:

```sh
screen -S session_name
```

It’s always a good idea to choose a descriptive session name.

## Working with Linux Screen Windows

When you start a new screen session, it creates a single window with a shell in it.

You can have multiple windows inside a Screen session.

To create a new window with shell type `Ctrl+a` `c`, the first available number from the range `0...9` will be assigned to it.

Below are some most common commands for managing Linux Screen Windows:

* `Ctrl+a` `c` Create a new window (with shell)
* `Ctrl+a` `"` List all window
* `Ctrl+a` `0` Switch to window 0 (by number )
* `Ctrl+a` `A` Rename the current window
* `Ctrl+a` `S` Split current region horizontally into two regions
* `Ctrl+a` `|` Split current region vertically into two regions
* `Ctrl+a` `tab` Switch the input focus to the next region
* `Ctrl+a` `Ctrl+a` Toggle between the current and previous region
* `Ctrl+a` `Q` Close all regions but the current one
* `Ctrl+a` `X` Close the current region

## Detach from Linux Screen Session

You can detach from the screen session at any time by typing:

`Ctrl+a` `d`

The program running in the screen session will continue to run after you detach from the session.

## Reattach to a Linux Screen

To resume your screen session use the following command:

```sh
screen -r
```

In case you have multiple screen sessions running on your machine, you will need to append the screen session ID after the `r` switch.

To find the session ID list the current running screen sessions with:

```sh
screen -ls
```

```sh
There are screens on:
    10835.pts-0.linuxize-desktop   (Detached)
    10366.pts-0.linuxize-desktop   (Detached)
2 Sockets in /run/screens/S-linuxize.
```

If you want to restore screen 10835.pts-0, then type the following command:

```sh
screen -r 10835
```

## Customize Linux Screen

When `screen` is started, it reads its configuration parameters from `/etc/screenrc` and `~/.screenrc` if the file is present. We can modify the default Screen settings according to our preferences using the `.screenrc` file.

Here is a sample `~/.screenrc` configuration with customized status line and few additional options:

\~/.screenrc

\# Turn off the welcome message
startup_message off

\# Disable visual bell
vbell off

\# Set scrollback buffer to 10000
defscrollback 10000

\# Customize the status line
hardstatus alwayslastline
hardstatus string '%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n\*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %m-%d %{W}%c %{g}]'

![Gnu Screen Terminal](https://linuxize.com/post/how-to-use-linux-screen/gnu-screen-terminal_huda639eed55d78a91f0c2d2928023b132_295393_768x0_resize_q75_lanczos.jpg?ezimgfmt=rs:726x454/rscb87/ng:webp/ngcb87)

## Basic Linux Screen Usage

Below are the most basic steps for getting started with screen:

1. On the command prompt, type `screen`.
2. Run the desired program.
3. Use the key sequence `Ctrl-a` + `Ctrl-d` to detach from the screen session.
4. Reattach to the screen session by typing `screen -r`.

## Conclusion

In this tutorial, you learned how to use Gnu Screen. Now you can start using the Screen utility and create multiple screen windows from a single session, navigate between windows, detach and resume screen sessions and personalize your screen terminal using the `.screenrc` file.

There’s lots more to learn about Gnu Screen at [Screen User’s Manual](https://www.gnu.org/software/screen/manual/screen.html) page.

If you have any questions or feedback, feel free to leave a comment.
