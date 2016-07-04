---
published: true
title: Fcitx, tmux colors and installing script on Atom
layout: post
---
Happy Independence Day!

I actually came across several situations I'd like to write down before I set up the blog, but I felt kind of reluctant to organize the relevant information like error messages, links and write down my solutions. Since today is a vacation day - well, I did actually do a little bit of work on my project - maybe I'd spend some time to get it done.

**Prevent resets of Fcitx settings on reboot for Ubuntu 14.04**

[Fcitx](https://fcitx-im.org/wiki/Fcitx) is an input method framework I'm currently using for Chinese IMEs on Ubuntu, and I had an annoying problem with the hotkeys: by default both SHIFT keys are set to switch IMEs, which is useless in my opinion and causes a ton of mistypes. There's a little program for changing the settings, but it seems to reset each reboot, probably due to some faulty design of the IME installed.

There's a [thread](http://askubuntu.com/questions/752324/fcitx-extra-key-for-trigger-input-method-changes-back-to-default-after-restart) on askubuntu on this issue for Ubuntu 15.10, but my config file doesn't work the same way as them, either because I'm using Ubuntu 14.04 or a different Fcitx version. So, after changing the key settings as described in the thread, I set the access permissions for `~/.config/fcitx/config` instead:

```
cd ~/.config/fcitx
chmod 444 config
```

(Well I actually used GUI to set the flags because I tried to look for the file then and had the file manager window open up, but yeah whatever)

**Enable colored output in tmux**

[tmux](https://tmux.github.io/) is a program to split terminal window for running multiple programs. I've used it for a little while and noticed the colored output of `ls` and `grep` is gone. I read some discussions online about the tmux color thing and learned the "colored" output is actually not default behavior - it's an alias of the command. To enable it, all I need to do is source `~/.bashrc`.

By the way, Ubuntu has some weird differences when it comes to handling `~/.bashrc` and `~/.bash_profile` and I think I read somewhere that something else is loaded when a terminal window is opened in desktop instead of the two. I'm not very interested in the details and since `~/.bash_profile` will be loaded when any tmux tab is opened, I just put it in there:

```
source ~/.bashrc
```

Maybe it's not the intended way; but it works for now and I'll fix it when I see any problem.

**Install script package for Atom on Windows**

[Atom](https://atom.io/) is a text editor that is developed by and closely related to GitHub developers and community, and I ran into a strange problem when installing the [script](https://atom.io/packages/script) package for it on Windows.

I haven't kept the exact error messages so that I can better describe the situation now, since I wasn't thinking about it before I got rid of the problem. Anyway, I got something like [this](https://github.com/npm/npm/issues/7456), complaining a cygwin virtual directory is not a legit git repo. The thread there has the developers' explanation on why the package manager npm won't support cygwin, but the funny thing is I wasn't even using cygwin at the time. I guess it has something to do with the default C/C++ compiler being a cygwin-based gcc/g++ or something like that.

The thread suggested using a fully functional Git Shell, and yes, installing from Atom GUI didn't work, command prompts and PowerShell both failed, but Git Shell completed the install without any problem.

Atom looks pretty good though :)