# auto-rclone

**Note**: There are a series of problems with this script that may lead to data-loss.
A smarter algorithmn and a nicer GUI are necessary, but hardly doable with bash.
Some more sophisticated language (like Python) would be more suitable.
Let's see if I have time.

Bash script to automate rclone syncing (from local to remote and vice versa).

## Behaviour

When the script is executed, changes from remote(s) are pulled to local.
Afterwards pull action will be performed periodically based on the given
`pull frequency`.
When filesystem events are observed in the directory corresponding to a remote,
an push action will be issued.

Push means `rclone sync local remote`.
Pull means `rclone sync remote local`.

## Usage

Write a configuration file called `~/.config/auto-rclone/remotes.conf`
with the following format:

```
my-onedrive:    $HOME/OneDrive    60 1800
my-googledrive: $HOME/GoogleDrive 60 1800
```

* **First column**: the rclone remote's name (trailing semicolon included)
* **Second column**: the remote's correspondent local directory 
(when needed, please use $HOME instead of ~)
* **Third column**: local directory inactivity time (in seconds)

 e.g. when some filesystem event occurred in the directory (create, delete, move, modify),
 only after given time of inactivity (i.e. no other event occurs), an push process
 will start. This way we avoid too many fragmentary pushes.
* **Fourth column**: pull frequency (in seconds)

## Installation

Just put this script somewhere reachable (i.e. somewhere listed in `$PATH`)
and execute it (don't forget to write the configuration file).

You can execute this script in your `.profile` to have everything automatically
working at each login.

## Remarks

Inspired by [ZetaoYang](https://github.com/zetaoyang)'s
[post](https://zty.js.org/post/2017/12/26/rclone-and-notes.html)
