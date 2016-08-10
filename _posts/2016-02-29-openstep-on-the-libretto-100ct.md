---
layout: post
title: "OPENSTEP 4.2 on the Libretto 100ct"
author: Brendan Shanks
description: "Installing/running OPENSTEP 4.2 on the Toshiba Libretto"
category: 
tags: [libretto,openstep,next]
fullview: true
---

<img src="/assets/posts/openstep-on-the-libretto-100ct/login.jpg" alt="Libretto at OPENSTEP login screen" style="width: 800px;"/>

*This post inspired by [NeXT month at r/retrobattlestations](https://www.reddit.com/r/retrobattlestations/comments/43pkr8/february_is_next_month/)*

I bought this Libretto 100ct off eBay around 10 years ago.

Specs:

- Intel Pentium MMX 166 MHz (the Libretto 110ct is the same machine, just with a 233 MHz processor)
- 64MB RAM (maxed out!)
- 2GB 2.5" HDD
- 7.1" 800x480 display

Win95 came installed on the hard drive. I saved an image of that, and installed OpenBSD. OpenBSD ran pretty well, although I haven't used it much over the years. But now it's [NeXT month at r/retrobattlestations](https://www.reddit.com/r/retrobattlestations/comments/43pkr8/february_is_next_month/), and this machine fits nicely into the system requirements for OPENSTEP on Intel. 

### Installation
**Problem**: a CD drive is required to install OPENSTEP, and I don't have a CD drive that will work on this laptop

**Solution**: Install OPENSTEP inside VMware Fusion, and then copy the installed OS onto the Libretto's hard drive (installed in a USB enclosure)

I started by installing OPENSTEP 4.2 inside VMware Fusion 8.0. It's pretty straightforward, the main details are to use the "Primary/Secondary(Dual) EIDE/ATAPI Device Controller" disk driver, and also to size the VM's hard drive correctly (see below for more details). After installing OPENSTEP, I installed the developer tools, and then updated to Patch 4.
This is a [good walkthrough](http://openstep.bfx.re) of the installation steps in a VM.
I then booted the OPENSTEP VM with an Ubuntu ISO, and used `dd` to copy the VM disk onto the Libretto HD (installed in a USB enclosure and passed through to the VM). I put the HD back in the Libretto, booted, and ran Configure.app to set up devices!

### Pictures
Even with the currently lousy hardware support (see below), the Libretto running OPENSTEP is still a lot of fun to play with!

<img src="/assets/posts/openstep-on-the-libretto-100ct/ipad.jpg" alt="Libretto with iPad Air 2" style="width: 800px;"/>

*Hanging out with my iPad Air 2*

<img src="/assets/posts/openstep-on-the-libretto-100ct/chess.jpg" alt="Chess.app on OS X and OPENSTEP" style="width: 800px;"/>

*Playing Chess.app with Uncle 15" rMBP*

<img src="/assets/posts/openstep-on-the-libretto-100ct/grab.jpg" alt="Grab.app on OS X and OPENSTEP" style="width: 800px;"/>

*Can you see the family resemblance?*

<img src="/assets/posts/openstep-on-the-libretto-100ct/xcode.jpg" alt="Project Builder/Xcode" style="width: 800px;"/>

*Project Builder, meet Xcode*

<img src="/assets/posts/openstep-on-the-libretto-100ct/ipad-pb.jpg" alt="Libretto running PB with iPad Air 2" style="width: 800px;"/>

*Which one is a "real computer"?*

<img src="/assets/posts/openstep-on-the-libretto-100ct/reddit.jpg" alt="Hi /r/retrobattlestations" style="width: 800px;"/>

*Hi reddit!*

### Storage
The Libretto's BIOS has a built-in hibernate function which will dump the contents of RAM to disk and power the machine off in response to closing the lid or pushing the power button, all without OS support. This is cool, but obviously requires that some disk space be left unpartitioned. The Libretto BIOS lies to the operating system and reports that the disk is smaller than it actually is, to reserve this space. This works fine if the disk is partitioned on the Libretto, but when partitioning on another machine, this space needs to be manually left unpartitioned.
The [Toshiba Linux Utilities](http://www.buzzard.me.uk/toshiba/hibernate.html) site describes this in more detail, but in short: for disks < 8.4GB, the hibernation space is at the end of the disk. For disks > 8.4GB, it starts at the 8.4GB mark.

I simply made my VM's hard drive around 200 MB smaller than the physical HD I'm using (which is the Libretto's factory original 2GB drive).

### Graphics
This Libretto uses a NeoMagic NM-2160 graphics chip, which I doubt there are native OPENSTEP video drivers for.
But, the VESA VBE video driver included with OPENSTEP 4.2 Update 4 does work, and provides a correctly-sized 800x480x8bpp mode. Unfortunately, the hardware is capable of thousands of colors (16bpp).
I found a Linux user who said that VBE mode 295 is 800x480x16, but if I set this using the "Expert..." dialog in Configure.app, OPENSTEP gives a "VBE mode not supported" message on the console at boot :(
Maybe there's a hard-coded table of VBE modes in the driver? This is a reverse-engineering project to tackle someday.

### Networking
I have a 3C575C 10/100 PCMCIA Ethernet card (a XJACK), which I think OPENSTEP has drivers for, but I've seen no sign that it detects the card at all. This will need more work.

### Sound
The Libretto has a Yamaha OPL3-SA3 chip, which seems to support both WSS and SBPro interfaces. I set it up in OPENSTEP as a SoundBlaster 8 but it's only working slightly (I get some noises and clicks when a sound is played). This needs more work, but should be fixable without needing extra drivers.

