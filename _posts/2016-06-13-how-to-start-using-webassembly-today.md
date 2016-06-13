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

**DISCLAIMER:** you should not start using WebAssembly in your projects right now, because the standard is under development and none of the browsers support it. But you can try.

# Browser

The first thing you need is a browser that supports wasm: you can either use [Firefox Nightly](https://nightly.mozilla.org/) or [Chrome Canary](https://www.google.com/chrome/browser/canary.html). Get the browser, then enable WebAssembly support: for Firefox open `about:config` and set `javascript.options.wasm` to `true`, for Chrome `chrome://flags/#enable-webassembly` and enable the switch.

# The toolchain

For building wasm code we will need three tools [LLVM with Clang](http://clang.llvm.org/) (built with WebAssembly support), [Binaryen](https://github.com/WebAssembly/binaryen) and [sexpr-wasm translator](https://github.com/WebAssembly/sexpr-wasm-prototype).

Here is how to build it:
 
{% gist tpimh/41f035e0eb1334d2f02d2fcd00e2a902 %}

# The code

First create our `index.html`. It can be as simple as this:


