---
layout: post
title:  "Mounting a SSH File System on Mac"
date:   2018-04-22 21:02:32 -0700
categories: mac
---

I used to use [Macfusion](http://macfusionapp.org/) to mount a lab machine on my Mac. I do this so that I can use a GUI editor (like [Visual Studio Code](https://code.visualstudio.com/)) instead of vim or emacs. 

After upgrading to MacOS Sierra, Macfusion stopped working. A discussion is going on in their github page about it but there is no solution yet. In the meantime, these are the workarounds I did: 

#### Get FUSE
Install latest FUSE for MacOS: https://osxfuse.github.io/

#### Mount File System using sshfs
Create a directory ~/Volumes/<mount_point>

Run sshfs to mount:
```
$ sshfs -o sshfs_debug -o defer_permissions root@198.168.0.9:/ ~/Volumes/198.168.0.9/
```
-o defer_permissions lets us write to file system even if write access is no available on the remote machine. 

#### Script to automate the mounting process
This is a quickly hacked up script to automate the process:

mount.sh: 
```bash
#!/usr/bin/env bash
 
USER_HOST=$1
DIRECTORY=/Users/$USER/Volumes/$USER_HOST
 
if [[ $# -ne 1 ]]; then
    echo "Not enough arguments"
    exit 1
fi
 
if [[ ! -d $DIRECTORY ]]; then
    mkdir -p $DIRECTORY
else
    MOUNTED=`ps aux | grep -i sftp | grep -v grep | grep -o "$USER_HOST"`
    if [[ -n $MOUNTED ]]; then
        echo "$USER_HOST already mounted"
        exit 1
    else
        rm -rf $DIRECTORY
        mkdir -p $DIRECTORY
    fi
fi

SSHFS_CMD="sshfs -o sshfs_debug -o defer_permissions $USER_HOST:/ $DIRECTORY/"
$SSHFS_CMD 
```

unmount.sh:
```bash
 #!/usr/bin/env bash
 
USER_HOST=$1
DIRECTORY=/Users/$USER/Volumes/$USER_HOST
 
umount $DIRECTORY
rm -rf $DIRECTORY
```

Set up alias on bash_profile for quickly calling these:

~/.bash_profile

```
alias ssh-mount="/Users/bharat/mount.sh"                                                                                           
alias ssh-umount="/Users/bharat/unmount.sh"
```

Have fun:
```
mpbyk:$ ssh-mount root@198.168.0.9
SSHFS version 2.5
Server version: 3
Extension: posix-rename@openssh.com <1>
Extension: statvfs@openssh.com <2>
Extension: fstatvfs@openssh.com <2>
Extension: hardlink@openssh.com <1>
Extension: fsync@openssh.com <1>


sjc-mpbyk:~ braghava$ ls ~/Volumes/root\@198.168.0.9/
home        libx32      opt         sbin        usr
lib         lost+found  proc        srv         var
bin         dev         lib32       media       root        sys
boot        etc         lib64       mnt         run         tmp


sjc-mpbyk:~ braghava$ ssh-umount root@198.168.0.9
```
