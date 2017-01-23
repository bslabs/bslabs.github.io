---
layout: post
title: "Porting SSA Detailed Calculator to Xcode/Intel: Part 1"
author: Brendan Shanks
description: "Porting the Social Security Administration's Detailed Calculator from CodeWarrior to Xcode and Intel: Part 1"
category: anypia
tags: [anypia,carbon,codewarrior,mac,osx]
excerpt: "Part 1 of my series detailing how I ported the SSA Detailed Calculator from PowerPC to Intel Macs"
---

*For the first post in this series, see [Porting SSA Detailed Calculator to Xcode/Intel: Intro](/2015/07/18/porting-anypia-to-xcodeintel)*

#### Part 1: Compile the existing project and import into a Git repo

As always with an existing software project, the first step is to get the code to compile. I used my [17" PowerBook G4](http://lowendmac.com/2004/17-powerbook-g4-1-5-ghz-mid-2004/), running OS X 10.4 (since it's the newest OS X version supported by CodeWarrior 9).

The existing source code can be downloaded [from the SSA](http://www.ssa.gov/OACT/anypia/source.html). Since the files are distributed in .sitx archives, StuffIt Expander is needed to expand them. In addition, the project uses Boost 1.33.1 (which I believe was the last version that supported CodeWarrior). All these directories need to be put next to each other, like:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/source_files_extracted.png" alt="Source directories unpacked"/>


With that done, open the CodeWarrior project. I used CodeWarrior 9 since the project files were saved by that version, but I believe CW10 would also work. (You can find a copy of CW9 at the Internet's favorite botany-themed Mac abandonware site 😛). Note that CodeWarrior 9 needs the latest 9.6 update installed to work correctly on OS X 10.4. Open the project file (anypiamacprojOSX/anypiamac.mcp) and the project window should appear:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/project.png" alt="CodeWarrior project opened"/>

Now it's time to try compiling. Click the green arrow on the top bar (2nd from the right), and it should start building:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/compiling_codewarrior.png" alt="CodeWarrior building"/>

10 million lines later, CodeWarrior had a single compile error for me:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/compiling_codewarrior_error.png" alt="CodeWarrior compile error"/>

The error is actually in Boost: a class that isn't defined (`date_time::microsec_clock`) is being typedef'd. The fix is easy, put an #ifdef for `BOOST_DATE_TIME_HAS_HIGH_PRECISION_CLOCK` around the typedef (this is the same #ifdef that's around the `date_time::microsec_clock` class definition):

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/compiling_codewarrior_error_fix.png" alt="CodeWarrior error fix"/>

With that fixed, I ran the project again and it was successful!

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/compiling_codewarrior_works.png" alt="CodeWarrior build works"/>

A full build takes 13 minutes on my 1.5 GHz PowerBook G4, I suppose not bad for a single-core computer compiling hundreds of files. Keep that time in mind when we port the project to Xcode later on.

---

With the existing source code proven, my next step was to import it into a Git repository.

OS X 10.4 doesn't include git (understandable for an OS released in 2004), but the excellent [Tigerbrew](https://github.com/mistydemeo/tigerbrew) project is a PPC Tiger-compatible fork of Homebrew, and includes formulas for the latest git version.

So this should be easy, right? Just install git, add the files, and push to github. Unfortunately, Mac resource forks make this more tricky. Dealing with resource forks is a distant memory for current Mac programmers, but code this old means that resource forks will be involved.

git has no support for resource forks: they aren't stored in the repository, so the resource fork is effectively stripped off. Luckily, most of the files are just plain text C/C++ source code which shouldn't need resource forks.
The oactobjs folder consists entirely of text files, and I was surprised to see that so many had resource forks:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/resource_fork_oactobjs_files.png" alt="oactobjs files with resource forks"/>

*Note: since 10.4, the preferred way to access the resource fork of a file through BSD/UNIX APIs is by accessing `file/..namedfork/rsrc`*

These are text files with resource forks. Huh? I opened one in a resource editor ([Rezilla](http://sourceforge.net/projects/rezilla/) is a free one that's OS X native) to investigate:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/source_file_resource_fork.png" alt="source file resource fork"/>

There are only two resources, neither one containing much data. After seeing 'Monaco' (the classic Mac monospaced font), I believe these are just editor settings (maybe window position, font, etc) from CodeWarrior. These resources can be stripped off without a problem.

However, the other source folder (anypiamacprojOSX) contains the project files, resources, and more source files:

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/resource_fork_anypiamacprojOSX_files.png" alt="source file resource fork"/>

After checking those files, I found two which have meaningful data in the resource fork: `AppResources.ppob` and `AppResources.rsrc`.

<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/resource_fork_files_rezilla.png" alt="AppResources resource forks"/>

Some solution will be necessary to ensure that these files still work correctly after having their resource forks stripped off.

- `AppResources.ppob` is the PowerPlant resource file, created using Constructor. Luckily, Constructor has an option to "Save as flattened resource file" which puts everything in the data fork. I resaved `AppResources.ppob` using that option, and the new `ppob` file doesn't even have a resource fork.
<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/constructor_flattened_save.png" alt="AppResources resource forks"/>

- `AppResources.rsrc` contains all the app's resources (icons, etc.) and would be edited using something like Rezilla or ResEdit. Luckily, Apple already has a flattened format for resource files, and the `Rez` and `DeRez` command-line tools are used to compile/decompile to/from the flattened format. I used `DeRez` to convert `AppResources.rsrc`, and then replaced `AppResources.rsrc` in the CodeWarrior project with `AppResources.rsrc.r`.
<img src="/assets/posts/porting-ssa-detailed-calculator-to-xcodeintel-part-1/appresources_derez.png" alt="AppResources compile with Rez"/>

With those files fixed, I was able to `git add` all the files and push to [my repository](https://github.com/bslabs/anypiamac).

[*Next post: Building a Mach-O binary with CodeWarrior*](/2017/01/22/porting-ssa-detailed-calculator-to-xcodeintel-part-2)
