---
layout: post
title: "Porting SSA Detailed Calculator to Xcode/Intel: Part 2"
author: Brendan Shanks
description: "Porting the Social Security Administration's Detailed Calculator from CodeWarrior to Xcode and Intel: Part 2"
category: anypia
tags: [anypia,carbon,codewarrior,mac,osx]
icon: "/assets/posts/porting-anypia-to-xcodeintel/anypiaapp.jpg"
excerpt: "Using CodeWarrior to build the Detailed Calculator as a Mach-O binary"
---

*For the other posts in this series, see [Porting SSA Detailed Calculator to Xcode/Intel: Intro](/2015/07/18/porting-anypia-to-xcodeintel)*

#### Part 2: Build a Mach-O binary using CodeWarrior

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-2/project.png" alt="CodeWarrior Mach-O target"/>

And we're back! After the last post, CodeWarrior 9 can be used to build a CFM/PEF binary of the Detailed Calculator, and the codebase is in a Git repository. The next step I took was to build a Mach-O binary with CodeWarrior. This isn't strictly necessary (since the end goal is a Mach-O binary built with Xcode) but it's a useful middle step. The Metrowerks compiler, linker, and C/C++ library (MSL) are still being used, but OS X system headers are also being used.

As before, I used my [17" PowerBook G4](http://lowendmac.com/2004/17-powerbook-g4-1-5-ghz-mid-2004/), running OS X 10.4 (since it's the newest OS X version supported by CodeWarrior 9). You will need to use CodeWarrior 9.6--it's the earliest version can build Mach-O binaries on 10.4.

First, I duplicated the existing CFM/PEF target, since I want to keep it around and working. To modify it for Mach-O, I closely followed the instructions in `/Applications/Metrowerks CodeWarrior 9.0/Release Notes/Mach-O Notes/Making_Mach-O_Projects_Note.txt`. It was also very helpful to double-check the details against a new Mach-O PowerPlant project created from stationery. Take a look at the CodeWarrior project file in my Git repository if you're curious, there's a lot of changes: different include paths, new frameworks, building a package, and more.

Once I made the changes and compiled the project, I got an interesting compile error:<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-2/check_error1.png" alt="check() error message"/>

It turns out that Apple's system headers `#define check()` in `/usr/include/AssertMacros.h`:<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-2/check_error3.png" alt="check() error message"/>
which conflicts with this function definition in the code: <img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-2/check_error2.png" alt="check() error message" style="width: 300px;"/>

My fix for this was not pretty: I `#define __ASSERTMACROS__` in the prefix header, so the file never gets included (none of its macros/defines are used).

Another part of building the Mach-O target is to build an application package. The CFM binary was just a single classic Mac-style application file. Luckily, CodeWarrior has good support for creating the application package, and creating the Info.plist from a `plc` file (property list compiler). The only change I had to make for the Mach-O binary was to [conditionalize the Info.plist icon file](https://github.com/bslabs/anypiamac/commit/9496cecdc3530a70f5a92815de6b25afb13865e2): as a single file the icon was a resource in the file, with the package it's a file in the bundle.

Creating a Mach-O target in CodeWarrior was a fairly easy process, which is a testament to the quality of Metrowerks CodeWarrior and the included documentation/examples. This Mach-O target isn't particularly useful by itself, but is a useful stepping stone to the final goal: Xcode.

*Next time: Creating the Xcode project*
