---
layout: post
title: "Compiling Pararena 2"
author: Brendan Shanks
description: ""
category: retrocomputing
tags: [mac,classicmac,codewarrior]
icon: "/assets/posts/building-pararena/pararena_splash_only.png"
excerpt: "How to compile the classic Mac game Pararena 2 from source code (recently released on GitHub)"
---
<img src="/assets/posts/building-pararena/pararena_splash_only.png" alt="Pararena 2 splash screen"/>

Pararena 2 is a classic Mac game written by John Calhoun (of [Glider](https://en.wikipedia.org/wiki/Glider_PRO) fame) and released commercially by Casady & Greene in 1991. Calhoun released the source code under the MIT license [on GitHub](https://github.com/softdorothy/pararena_2) in January 2016, but I think I may be the first to actually build and run it.

If you have a Mac, it's pretty straightforward! Any version of OS X that can run Basilisk II should work (I used 10.11). Note that THINK C 6 does not run well under the OS X Classic Environment (both THINK C and its installer will crash Classic regularly).

1. First, install Basilisk II and System 7.5 using [this howto at Emaculation](http://emaculation.com/doku.php/basiliskii_osx_setup). Make sure that "Ignore Illegal Memory Accesses" is checked in the setup app. Once you install Mac OS, reboot into your clean new install: <img src="/assets/posts/building-pararena/fresh_boot.png" alt="fresh Mac OS 7.5"/>
2. Download [THINK C 6.0.1](/assets/posts/building-pararena/THINK C 6.0.1.zip), and add all four of the floppy images as volumes in the Basilisk II setup app. Relaunch Basilisk, run the installer off disk 1, and install THINK C: <img src="/assets/posts/building-pararena/think_installer.png" alt="Running THINK C installer"/>
3. Back on the host OS X system, clone the [Pararena 2 git repository](https://github.com/softdorothy/pararena_2) into a folder inside the Basilisk transfer folder. At the time of this guide, I used commit `08c83ddb59ee`.
4. A couple of files in the repository will need to be un-BinHexed. In the OS X Finder, open `Pararena.project.bin`, `Para Sounds.bin`, `Sources/Para.bloon.bin`, `Sources/sms.a.π.bin`, and `Sources/SMSCore.a.π.bin`. Each file should extract to the same filename without the `.bin` (i.e. `Para Sounds.bin` will become `Para Sounds`)
5. Rez needs to be run on `Pararena.project.r`, to compile the text .r format into an actual resource fork file. From a terminal, inside the Pararena 2 repository, run `Rez Pararena.project.r > Pararena.project.rsrc` (you may be prompted to install Xcode and/or the Command Line Tools)
6. Inside Basilisk, copy the `pararena_2` folder from the Unix transfer volume to Macintosh HD (THINK C can't open the files from the transfer volume directly, probably because the file type code is not set correctly). Also, rename the `sms.a._` and `SMSCore.a._` files to end with `.π` as shown below. Option-P is the key combo to type `π`. <img src="/assets/posts/building-pararena/rename.png" alt="Change ._ files to .π"/>
7. Now, double-click to open `Pararena.project`. You just opened a project file that hasn't been changed since June 1992! <img src="/assets/posts/building-pararena/open_project.png" alt="Pararena.project opened"/>
8. THINK C is very basic compared to modern IDEs, but double-clicking on a file does open it for editing: <img src="/assets/posts/building-pararena/open_file.png" alt="File opened"/>
9. Close the file, and use Project -> Bring Up To Date (command-U) to bring the whole project up to date. Even emulated, this runs really fast on a modern Mac! <img src="/assets/posts/building-pararena/up-to-date.png" alt="Project up-to-date"/>
10. THINK C works differently than modern IDEs/compilers: all object files are stored inside the single project file (`Pararena.project` in this case), and the target application is not automatically linked/built. Choose Project -> Build Application... to link and save the application. You should see it in the Finder. <img src="/assets/posts/building-pararena/app_built.png" alt="Pararena built"/>
11. Now double-click on the Pararena 2 application. It runs! <img src="/assets/posts/building-pararena/it_runs.png" alt="Pararena runs"/>
12. But after changing the color depth, the next dialog asks for the master disk in order to confirm you legitimately own the game. This does not play nicely with Basilisk, and I saw Pararena quit unexpectedly, the system crash with a bomb, and this odd request: <img src="/assets/posts/building-pararena/insert_disk.png" alt="Pararena copy protection dialog"/>
13. Clearly we need to bypass this copy protection, and since we have the source code it'll be easy to crack. It was straightforward to find where the copy protection code lives: in `ValidInstall.c`. I made the following change inside `ValidInstallation()` at the bottom of the file: <img src="/assets/posts/building-pararena/crack.png" alt="Bypass copy protection by adding 'return true' inside ValidInstallation()"/>
(Also, note the exception for the [PowerBook 100](https://en.wikipedia.org/wiki/PowerBook_100). It was the only Macintosh at the time that did not include an internal floppy drive, which I assume is why the master disk check is bypassed)
14. With that modification made, bring the project up-to-date and rebuild the application. Launch the app, and it should work without any copy protection! <img src="/assets/posts/building-pararena/pararena_splash.png" alt="Pararena 2 splash screen"/> Unfortunately the gameplay itself runs very slowly inside Basilisk, but consider it a good reason to pick up some actual vintage Mac hardware 😛