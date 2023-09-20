+++
categories = ["project"]
date = "2019-07-07T00:00:00Z"
description = "A native, non-web embedding of WebAssembly for Windows/Linux."
image = "img/innative.png"
links = [["fa-brands fa-github", "Github", "https://github.com/innative-sdk/innative"]]
tags = ""
title = "inNative"

+++
An AOT (ahead-of-time) compiler for WebAssembly that creates C compatible binaries, either as sandboxed plugins you can dynamically load, or as stand-alone executables that interface directly with the operating system. This allows webassembly modules to participate in C linking and the build process, either statically, dynamically, or with access to the host operating system.