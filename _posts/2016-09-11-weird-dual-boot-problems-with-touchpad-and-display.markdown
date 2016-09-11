---
published: true
title: Weird dual-boot problems with touchpad and display
layout: post
---
So I got an Asus UX305UA laptop last month, only 700 bucks for a fairly well powered Skylake model. As a hesitant shopper who has missed a lot of good prices for desired products, I consider this a pretty good deal. Yet I had some small problems with Ubuntu on this machine. First, the touchpad did not work at all in Ubuntu 14.04; but that's fine, since 16.04 has become an LTS version lately, I upgraded it instead of following some tutorials of installing 3rd party dpkg drivers (which are now official packages in 16.04). Still the touchpad would be inactive randomly when I booted up, and the problem was usually solved by a reboot.

Some time later I kind of figured out what kind of "random" occasions it was that caused this. So, an external USB mouse always works fine. The touchpad thing seemed to only happen at the first reboot from Windows; no problem at all if the previous boot was Ubuntu. And when it happens, Ubuntu could detect the device and enable/disable it just fine, only no input at all. Also when I switched to tty to type reboot, I got this message when moving around with the touchpad:

```
elan_i2c i2c-ELAN0100:00: invalid report id data (1)
```

I'm not going to document the process I did different tries and learned this or that about the driver situation, since I looked into this from time to time and kind of lost track and honestly I'm not very interested. Long stories short, this [post](http://askubuntu.com/questions/11111/touchpad-is-weird-after-reboot-from-windows) mentions that touchpad drivers can sometimes reside on a flash memory, which is not cleared at reboot, and newer Windows specific i2c drivers are not compatible with Linux. And this [post](https://gist.github.com/precurse/6dc1990cd000551c8f11) gives the command to reload the driver:

```
sudo modprobe -r elan_i2c && sleep 5 && sudo modprobe elan_i2c
```

Well, I consider myself not really leaning towards either geeky or foolproof/practical/lazy solutions, but with my limited experience with Linux, I would say troubleshooting hardware/software issues in Linux is too much work. Some may think that all kinds of problems must have been encountered by others and you can get around them as long as you're good at searching, but it's really not! A lot of times the solutions you find just don't work and it takes a lot to know the thing to search for.

The post above also mentions the problem with HDMI outputs, which I have already given up on. Admittedly I have an awkward setup: because a conference room we're using only have a VGA projector, I'm forced to do a micro-HDMI - HDMI - VGA conversion (with two adapters, ughhh), if it could work, that is. And also I totally accept the argument that HDMI is purely digital and there's no way a passive connector can get VGA out of it. But how come I can get the projector to work in Windows but not in Ubuntu? Either a driver problem or my stupid coupling of adapters, I just don't know and nor am I interested. VGA projectors are obsolete hardware anyway and I'm fine with switching to Windows merely for the time to present.