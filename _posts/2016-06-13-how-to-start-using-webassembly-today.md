---
layout: post
title: How to start using WebAssembly today
---

> WebAssembly (short: wasm) is a new binary format for the web, created by Google, Microsoft, Mozilla and others. It will be used for performance critical code and to compile languages other than JavaScript (especially C/C++) to the web platform. It can be seen as a next step for asm.js
> - [Dr. Axel Rauschmayer](http://www.2ality.com/2015/06/web-assembly.html)
 
    .--.      .--.   ____       .-'''-. ,---.    ,---.
    |  |_     |  | .'  __ `.   / _     \|    \  /    |
    | _( )_   |  |/   '  \  \ (`' )/`--'|  ,  \/  ,  |
    |(_ o _)  |  ||___|  /  |(_ o _).   |  |\_   /|  |
    | (_,_) \ |  |   _.-`   | (_,_). '. |  _( )_/ |  |
    |  |/    \|  |.'   _    |.---.  \  :| (_ o _) |  |
    |  '  /\  `  ||  _( )_  |\    `-'  ||  (_,_)  |  |
    |    /  \    |\ (_ o _) / \       / |  |      |  |
    `---'    `---` '.(_,_).'   `-...-'  '--'      '--'

Technically WebAssembly is a virtual ISA that all modern browsers will soon implement. The code is compiled once and then executed in any browser on any computer in the exact same way (sounds very much like Java VM). WebAssembly has a lot of advantages, such as faster execution and smaller binary, but the greatest thing is that web programming is not limited to JavaScript anymore (well, not really).

# Browser

The first thing you need is a browser that supports wasm: support for WebAssembly is enabled by default starting with in Firefox 52 and Chrome 57. If it is not enabled for some reason, for Firefox open `about:config` and set `javascript.options.wasm` to `true`, for Chrome open `chrome://flags/#enable-webassembly` and enable the switch.

# The toolchain

For building wasm code we will need three tools [LLVM with Clang](http://clang.llvm.org/) (built with WebAssembly support), [Binaryen](https://github.com/WebAssembly/binaryen) and [WABT (The WebAssembly Binary Toolkit)](https://github.com/WebAssembly/wabt).

Here is how to build it:
 
{% gist tpimh/41f035e0eb1334d2f02d2fcd00e2a902 %}

# The code

First create our `index.html`. It can be as simple as this:

{% gist tpimh/3119e7b3dd2c1c68539874b40879ab86 %}

And we are going to need our C code:

{% gist tpimh/26d2009a9f730a2fbf2b2d86d2e200dd %}

# Building

For building we will need four tools located at:

- `llvm/build/bin/clang`
- `llvm/build/bin/llc`
- `binaryen/build/bin/s2wasm`
- `wabt/build/wast2wasm`

I will use their basenames because they are in my PATH.

Execute the following commands:

    clang -emit-llvm --target=wasm32 -S sample.c
    llc sample.ll -march=wasm32
    s2wasm sample.s -o sample.wast
    wast2wasm sample.wast -o sample.wasm

Now you have your C code first compiled to LLVM IR then to wasm through S-expressions representation.

Let's now take a closer look at each resulting file:

{% gist tpimh/d338d32f95edafa702e3872dfa745b6a %}

You are now free to delete everything except for `index.html` and `sample.wasm`. Start your favorite web-server (I use `busybox httpd -f -p 8000`) and point your web browser to it, it should show a button and execute your code each time you click it.

# The easy way

I have recently created a script that automates the building proccess described above. Just clone [this repo](https://github.com/tpimh/wasm-toolchain), then run `./build-all.sh` to build the toolchain, then you can pass the filename of your C source code as an argument to `c2wasm` script and wasm code will be produced.
