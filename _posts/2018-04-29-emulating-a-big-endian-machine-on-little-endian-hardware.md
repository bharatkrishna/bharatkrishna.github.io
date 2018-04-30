---
layout: post
title:  "Emulating a Big-Endian machine on Little-Endian hardware"
date:   2018-04-29 16:51:51 -0700
categories: archive
---

> This is an old post from 2012 which I wrote when I was a graduate student. My old blog was hosted on my university's CS department's server which seem to no longer exist. I had backups of some posts from my old blog which I am republishing now as it might still be relevant. 

I had to test some code for [Endian](http://en.wikipedia.org/wiki/Endianness) independence. Most processors these days are Little-Endian. VMware & Virtual Box provide only x86 virtualization. [QEMU](http://wiki.qemu.org/) provides virtualization of Big-Endian machines like MIPS and PowerPC.

On Debian based Linux distributions, QEMU can be installed using `apt-get install qemu`. [QEMU Manager](http://www.davereyn.co.uk/) and Q provide GUI front-ends for Windows & OS X respectively. I tried setting it up on all the OSes. Q for OS X seems to be buggy and didn't boot any of the VMs I created. On Linux & Windows, it was more stable.

The easiest way to setup a PPC or MIPS machine would be to download the pre-installed Debian disc images from [here](http://people.debian.org/~aurel32/qemu/powerpc/) for PPC and [here](http://people.debian.org/~aurel32/qemu/mips/) for MIPS. The ReadMe file on the download page mentions how to use the images with QEMU. This version of Debian is a bare bones install. We'll have to install GCC, G++ and other tools we might need.

On Linux, while tying to boot the PPC image, it could throw an error which says `qemu: hardware error: qemu: could not load PowerPC bios 'openbios-ppc'`. The solution mentioned in [this](https://lists.ubuntu.com/archives/ubuntu-users/2011-November/254572.html) post solves the issue.

Endianness of the emulated PPC machine can be quickly verified by running this code: [https://gist.github.com/3950497](https://gist.github.com/3950497)

| ![Debian booting on Windows QEMU]({{ "/assets/images/DebainPPC_WindowsQEMU.jpg" | absolute_url }})
|:--:| 
| *Debian booting on Windows QEMU* |

| ![Screen showing Machine Information]({{ "/assets/images/SysInfo.jpg" | absolute_url }})
|:--:| 
| *Screen showing Machine Information* |
