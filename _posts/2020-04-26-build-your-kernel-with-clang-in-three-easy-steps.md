---
layout: post
title: Build your kernel with Clang in three (easy?) steps
---

Building Linux with Clang is a thing now and I strongly recommend anyone who considers oneself a power user to try it. Why? There are multiple reasons:

- possibility to find bugs
- it may be faster (or slower if you are not lucky)
- it’s just fun to try new and experimental stuff

Linux could originally only be compiled with GNU C Compiler, but recently a lot of work was done allowing it to be compiled with Clang, an alternative to GCC. Compiler is a tool translating human-readable source code to machine code that CPU can execute. Different compilers implement source code parsing algorithms differently and have a different set of warnings that they can produce. Due to this compiling code with different compilers may help discover bugs or imperfections. Usually during the compilation process some optimizations are applied. Different compilers may implement optimizations differently, and this is where the size and performance difference might come from.

There are many Linux distributions available, and you are probably running one (or multiple) right now. Most distros apply their own unique set of patches and use specific kernel configuration. While mainline kernel with defconfig is tested pretty well thanks to a collaborative effort of the [ClangBuiltLinux project](https://clangbuiltlinux.github.io/), most distro-specific kernels were probably never built with Clang at the time I’m writing this. You can attempt to build your kernel with Clang, report whether or not you succeeded, and share information on warnings and errors you got in the process. Bonus points for fixing the problems and submitting patches!

### Step one: obtaining the kernel source, patches and config

If you don’t know where to find your distro’s source code, try searching online. Usually this information is pretty easy to find. If you didn’t succeed in it, ask on forums, mailing lists, and chatrooms related to your distro of interest. If you still got nothing, keep harassing the developers and tech support, and eventually you will get what you want.

Check the kernel version (run `make kernelversion`), last three LTS kernels should work pretty well, but may require a couple of additional patches that were not backported yet. If your kernel is really old and doesn’t belong to an LTS branch, you may have better luck with a newer release of your distro. Ideally, your kernel should be as close to the latest release as possible, otherwise, prepare for a lot of errors and a lot of patching.

Another important thing to check is your kernel’s target architecture. It depends on the hardware that your system is running on. Chances are it’s x86_64 for your desktop and arm64 for your phone, otherwise, your experience can be very different, but also extremely valuable (also prepare for even more errors and patching). You may tell what architecture you are using by running `uname -m`. Check that your config targets your architecture and not something else. If you can build your distro kernel for other architectures as well, that’s great, please do!

If your kernel source doesn’t come pre-patched, you have to apply patches yourself. Using `patch` utility is described in detail, if you are not familiar with it, refer to [Applying Patches To The Linux Kernel](https://www.kernel.org/doc/html/latest/process/applying-patches.html). Note that if the kernel you want to build is old, you may need to apply some additional patches (which may not apply cleanly). If you are no stranger to building Linux kernel, you might have your patches and own kernel config, so if it’s the case don’t forget to share them when reporting problems with your build. After all patches are applied, make sure the config is moved to the kernel source directory and named `.config`.

### Step two: downloading or building Clang

A lot of distros have Clang binaries in their repos, unfortunately, most of them are dramatically outdated and not able to compile a functional kernel. Check the version of Clang provided by your distro, some even provide multiple versions. Once again, the closer to the latest release the better. I recommend using [APT packages provided by the LLVM project](https://apt.llvm.org/) if you are using Debian or Ubuntu as your host system, otherwise building from source. I already covered building LLVM and Clang from source in [How to start using WebAssembly today](https://blog.golovin.in/how-to-start-using-webassembly-today/), and if you want to automate the process of building LLVM and Clang, check out the scripts from [tc-build](https://github.com/ClangBuiltLinux/tc-build) or my [NGTC](https://github.com/tpimh/ngtc). If you need to apply a patch to Clang or LLVM source, building from source is pretty much your only option, it will take some time.

### Step three: finally building Linux with Clang!

By default KBuild would try to use GCC to compile the kernel. This is not what you want, so you append `CC=clang HOSTCC=clang` to every `make` command that you execute. If your Clang binary is named differently, use appropriate name; if your Clang binary is not in your `PATH`, use an absolute path to it. Start your build with `make clean CC=clang HOSTCC=clang`, `make oldconfig CC=clang HOSTCC=clang`, and `make CC=clang HOSTCC=clang `. Now wait a bit and there is a high chance that you will see a warning or an error message in your terminal.

Copy the message and try to find reports of similar messages in the [ClangBuiltLinux issue tracker](https://github.com/ClangBuiltLinux/linux/issues). You may find a discussion about your problem and quite possibly a solution to it. If you found nothing similar, submit a new issue, include information about your problem, distro, kernel version, config, and patches. As I already mentioned, if you also provide a solution, you get bonus points ;)

There are still a lot of unresolved issues in the tracker. If you can reproduce them, provide additional information or a solution, please do! Also if you need help with any of the three steps or are willing to help the others, welcome to the ClangBuiltLinux [mailing list](https://groups.google.com/forum/#!forum/clang-built-linux) and [Telegram chat](https://t.me/ClangBuiltLinux)!
