---
layout: post
title: "Re-engine, Not Re-imagine"
author: Brendan Shanks
description: "My predictions for the ARM Mac transition"
category: mac
tags: [mac,arm]
icon: "/assets/posts/reengine-not-reimagine/pentium-snail-hires.jpg"
excerpt: "My predictions for the ARM Mac transition"
---
<img src="/assets/posts/reengine-not-reimagine/pentium-snail-hires.jpg" alt="Pentium II snail"/>

Everyone has a prediction of how the Mac ARM transition is going to happen, so I thought I’d add my own (as an expansion of some [tweets I made in late April](https://twitter.com/realmrpippy/status/1253364186685440001)). This is based *only* on public rumors and my own gut feeling.

My core prediction: **The Mac is getting a re-engining, not a re-imagining**.

Much like the Mac's [switch to Intel](https://en.wikipedia.org/wiki/Apple%27s_transition_to_Intel_processors), this transition will be **fundamentally simple** and, ideally, **invisible to end users**.

This means:

- No massive hardware changes, like adding touchscreens.
- No massive software changes, like requiring apps to be sandboxed, or come from the App Store, or be written with UIKit and Catalyst.
- No complex schemes, like hybrid ARM/Intel Macs that run apps on both processors.

The result will be Macs that look, feel, and work the same as they do today, but are powered by custom Apple SoCs featuring higher performance and reduced power consumption.

Here's how I think that will happen.

***

**Hardware**

Apple will announce a Developer Transition Kit at WWDC, which will be available this summer. The DTK will use an [A12Z](https://en.wikipedia.org/wiki/Apple_A12Z) (the current iPad Pro SoC), inside a Mac mini chassis. Or, I think less likely, an Apple TV chassis with added I/O.

I don't think it will be a laptop: that would require full power management to be implemented, would be more expensive, and would result in battery life figures for **semi-prototype hardware** being reported all over the press. That's really not how Apple rolls.

The iPad Pro will **not** be usable as a DTK. John Gruber [described the reasons](https://daringfireball.net/2020/06/on_apple_announcing_the_mac_arm_transition_at_wwdc) better than I can, but in summary: not enough RAM, and it confuses these (still separate!) products in a way that Apple never publicly would.

The first ARM Macs will go on sale in **February 2021**. The MacBook Air will be part of the first tranche of ARM Macs released, along with the possible return of the 12" MacBook.
With the first ARM Macs, Apple will want to surprise with the power and efficiency made possible by their custom SoCs, and laptops will be the best showcase for that.

Also like the Intel switch, this will not be a long and drawn-out transition. My prediction is that by **October 2022**, Apple will no longer sell Intel Macs.

**Software**

macOS 10.16 will be identical on ARM and Intel, except:

- ARM has a Rosetta-style emulator to run 64-bit Intel apps

	Like [Rosetta](https://en.wikipedia.org/wiki/Rosetta_(software)) this will be a *temporary* compatibility aid, only around for a few years. Performance will be bearable, but not spectacular.

- some deprecated APIs (OpenGL, OpenCL, WebView, etc) may not be available for ARM apps. I wouldn't put money on this prediction though.
- no 3rd-party kexts on ARM
- [Hypervisor.framework](https://developer.apple.com/documentation/hypervisor?language=objc) will not be available on ARM with macOS 10.16, but will be added to macOS 10.17 with great fanfare

	Hopefully once Hypervisor.framework is available, virtualization apps with the same level of features and polish as VMware Fusion or Parallels Desktop will be available (either from VMware/Parallels, or others). These will only be useful for Linux/BSD at first, but maybe Microsoft can be convinced to release Windows on ARM for virtualization.

On the security front:

- Non-notarized apps can still run (with annoying warnings)
- SIP can still be disabled
- Secure Boot is always-on: any bare-metal OS must be signed by Apple

	It will be possible to dual-boot to older or beta macOS versions though.

Above all: these will be Macs. Macs exist to run Mac software, and Apple's **top priority** is to make it as easy as possible to recompile existing Mac apps to run natively on ARM.

This means removing as few APIs as possible and making the OS as consistent as possible between architectures. Apple perfected this strategy in the switch to PowerPC, and then again to Intel, and I expect to see it used for ARM.

**Conclusion**

In June 2005 when the Intel transition was announced, I was surprised less by the switch itself and more that Apple was picking Intel instead of AMD. To PC enthusiasts, AMD's Athlon 64 was clearly better than the warmed-over Pentium 4. It really concerned me: *Apple is leaving PowerPC for the Pentium 4*??

Of course, as we found out in 2006, Apple had no intention of using the Pentium 4 (except in the [DTK](http://vintagemacmuseum.com/the-apple-developer-transition-system-a-trojan-horse-powermac/)). The first generation of Intel Macs all used the [Core Duo/Solo](https://en.wikipedia.org/wiki/Intel_Core#Gen_I:_Dual-core_enhanced_Pentium_M), an enhanced flavor of the Pentium M that Intel used in laptops. The second generation of Intel Macs then used [Core 2](https://en.wikipedia.org/wiki/Intel_Core_(microarchitecture)), a legendary microarchitecture that kicked off a full decade of dominance for Intel.

When Apple decided to switch in 2005, they saw Intel's (then-confidential) roadmap and knew that no one else would be able to match it.

Today, I believe that Apple is looking at their own internal [ARM SoC](https://en.wikipedia.org/wiki/Apple-designed_processors) roadmap and thinking the same thing.

My last prediction: **we'll be blown away by what Apple's silicon team comes up with**.

<!--
Make no mistake: this is a huge risk for Apple and the Mac. They're giving up the compatibility and known-quantity performance of Intel, and will have to work hard to keep pace with (and ideally surpass) Intel and the rest of the PC industry.
If Apple or its silicon fab partners fall behind Intel in performance, they can only blame themselves

To reiterate, macOS on ARM **will not**:

- bring touchscreens to the Mac
- force all apps to be distributed through the App Store
- force all apps to be sandboxed
- lock down the application environment beyond macOS 10.16 on Intel hardware
- incorporate ARM and Intel chips in the same computer, and run applications on both
- run unmodified iOS apps
- "merge" the iOS/iPadOS and macOS user experience
- use a new kernel

Additionally, the Mac will *not* be moving to AMD CPUs.
-->