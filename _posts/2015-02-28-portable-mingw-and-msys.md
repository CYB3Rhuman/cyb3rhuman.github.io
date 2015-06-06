---
layout: post
title: Portable MinGW and MSYS
---

Sometimes I have to use Windows (for example in my university) and each times it turns out to be sort of a pain in the ass. Standard terminal window is not resizable, text editor is only able to handle DOS linebreaks, even grep is not present on some systems (on the others it is still not that functional) and the whole system is full of things that are opposite to obvious and seem very strange to me.

I decided to create a USB flash drive with a handy terminal, convenient text editor, compiler and popular Unix utilities. [MinGW and MSYS](http://www.mingw.org/) seemed a good choice, but it needed some workarounds to be done to make my installation portable.

## Installation

I was following the [official installation instructions](http://www.mingw.org/wiki/getting_started) in a virtual machine.

First of all, download `mingw-get-setup.exe` and run it.

![Portable MinGW and MSYS (image 1)](/images/mingw-portable-1.png)

Then, select your USB drive's root as installation target.

![Portable MinGW and MSYS (image 2)](/images/mingw-portable-2.png)

This is what you will have on your flash drive after downloading the installer.

![Portable MinGW and MSYS (image 3)](/images/mingw-portable-3.png)

Select your desired options in the installer and apply. I only selected `msys-base` and `mingw32-base` (probably, you can leave it out if you don't need a complier). And this is what you will have on your flash drive after.

![Portable MinGW and MSYS (image 4)](/images/mingw-portable-4.png)

To finish the installation, run `msys/1.0/msys.bat`, type `postinstall/pi.sh` and answer it's questions.

![Portable MinGW and MSYS (image 5)](/images/mingw-portable-5.png)

You can later add and remove packages with `mingw-get`. I wanted to have terminal emulator and a text editor, so I also installed `msys-mintty` and `msys-vim`.

![Portable MinGW and MSYS (image 6)](/images/mingw-portable-6.png)

## Making it portable

I wrote this batch script `launch.bat` on my flash drive that modifies `/etc/fstab` to use correct Windows drive letter assigned to this flash drive and then launch mintty. It also clears the `PATH`, so only MinGW and MSYS binaries are in the path, so utilities like `ping` will not be accessible unless installed with `mingw-get`.

```batch
@echo off
set MSYSDRIVE=%~d0
set PATH=
set MSYSTEM=MINGW32

%MSYSDRIVE%\msys\1.0\bin\sed.exe -i 's/.*\/mingw/%MSYSDRIVE% \/mingw/g' %MSYSDRIVE%\msys\1.0\etc\fstab

%MSYSDRIVE%\msys\1.0\msys.bat --mintty
```

This does the trick. Now I can use my portable system almost on any Windows computer.

## Adding pseudo package manager

Official MinGW documentation recommends installing software with `/mingw` prefix which adds some extra mess to the system. So I would create a directory `/mingw/addons` and install each package to `/mingw/addons/$pkgname`. Each package will install binaries, [headers](http://www.mingw.org/wiki/includepathhowto), [libraries](http://www.mingw.org/wiki/HOWTO_Specify_the_Location_of_Libraries_for_use_with_MinGW) and other resources. If any information needs to be altered to make the package portable, add it to `/mingw/addons/$pkgname/install.sh` which will be executed each time when system starts.

To make the system see those files, add this to your `/etc/profile`:

```shell
for i in /mingw/addons/* ; do
  if [ -d $i ]; then
    if [ -d $i/bin ]; then
      export PATH="$PATH:$i/bin"
    fi
    if [ -d $i/include ]; then
      export CPATH="$CPATH;$i/include"
    fi
    if [ -d $i/lib ]; then
      export LIBRARY_PATH="$LIBRARY_PATH;$i/lib"
    fi
    if [ -d $i/lib/pkgconfig ]; then
      export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:$i/lib/pkgconfig"
    fi
    if [ -d $i/share/pkgconfig ]; then
      export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:$i/share/pkgconfig"
    fi
    if [ -f $i/install.sh ]; then
      . $i/install.sh
    fi
  fi
done
```

Now you generally will only need these steps to install new software:

```shell
./configure --prefix=/mingw/addons/$pkgname
make
make install
```

Now this system is more or less complete and easily extensible.

If you have similar portable system, you can use any Windows computer comfortably and compile your software with favorite compiler.

![Portable MinGW and MSYS (image 7)](/images/mingw-portable-7.png)

![Portable MinGW and MSYS (image 8)](/images/mingw-portable-8.png)
