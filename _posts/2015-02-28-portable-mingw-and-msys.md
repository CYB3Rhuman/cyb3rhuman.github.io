---
layout: post
title: Portable MinGW and MSYS
published: false
---

Sometimes I have to use Windows (for example in my university) and each times it turns out to be sort of a pain in the ass. Standard terminal window is not resizable, text editor is only able to handle DOS linebreaks, even grep is not present on some systems (on the others it is still not that functional) and the whole system is full of things that are opposite to obvious and seem very strange to me.

I decided to create a USB flash drive with a handy terminal, convenient text editor, compiler and popular Unix utilities. [MinGW and MSYS](http://www.mingw.org/) seemed a good choice, but it needed some workarounds to be done to make my installation portable.

I was following the [official installation instructions](http://www.mingw.org/wiki/getting_started) in a virtual machine.

First of all, download `mingw-get-setup.exe` and run it.

*img*

Then, select your USB drive's root as installation target.

*img*

