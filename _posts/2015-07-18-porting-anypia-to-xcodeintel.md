---
layout: post
title: "Porting SSA Detailed Calculator to Xcode/Intel: Intro"
description: "Porting the Social Security Administration's Detailed Calculator from CodeWarrior to Xcode and Intel"
category: 
tags: [anypia,carbon,codewarrior,mac,osx]
fullview: true
---

<img src="/assets/posts/porting-anypia-to-xcodeintel/anypia.png" alt="Detailed Calculator running on Yosemite" style="width: 700px;"/>

The U.S. Social Security Administration maintains a [Detailed Calculator](http://www.ssa.gov/OACT/anypia/anypia.html) (often referred to as Anypia) which is used to estimate retirement benefits. It's a C++ application available for Windows and Mac OS X, and source code is available.

Unfortunately, the OS X version is still a PowerPC binary built with Metrowerks CodeWarrior 9 (!). This means it will not run on any version of OS X newer than 10.6 Snow Leopard (since that was the last release to include the Rosetta PowerPC translator). 

The Detailed Calculator was originally ported from OS 9 to OS X in 2005 by David Phillip Oster, who detailed the process [in a post on comp.sys.mac.oop.powerplant](https://groups.google.com/forum/#!msg/comp.sys.mac.oop.powerplant/sQf2j-b4YCE/gSA7WLsnXx4J) and [on his website](http://www.turbozen.com/mac/anypia/). I came across David's newsgroup post in May 2015, and was amazed to find that the SSA was still using CodeWarrior and releasing PowerPC-only binaries. I thought it would be a fun retrocomputing and Mac development exercise to port the Detailed Calculator to Xcode in order to build universal binaries that would run on the latest Macs, and that is now complete!

If you're reading this post and just want to download a binary of the Detailed Calculator that will run on Intel Macs, the latest binary release can be downloaded from [github](https://github.com/bslabs/anypiamac/releases). Make sure to read the note and notice in the [README file](https://github.com/bslabs/anypiamac/blob/master/README.md).

* * *

However if you want the gory details, read on! Converting a Carbon project from CodeWarrior to Xcode was a common process 10 years ago, but modern write-ups are quite rare! I've broken it down into 5 steps, and I'll be publishing separate posts soon for each one.

<img src="/assets/posts/porting-anypia-to-xcodeintel/steve.png" alt="Steve Jobs at WWDC 2005" style="width: 500px;"/>

1. [Compile the existing project and import into a Git repo](/2015/07/26/porting-ssa-detailed-calculator-to-xcodeintel-part-1)
2. Using CodeWarrior, build a Mach-O binary
3. Import project into Xcode and start fixing errors
4. Use open-powerplant and fix remaining errors
5. Test functionality and tweak Info.plist

Stay tuned!
