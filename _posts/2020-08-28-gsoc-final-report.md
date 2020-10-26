---
layout: post
title:  "RTEMS GSoC 2020 Final Report: BSP Buildset for EPICS"
summary: Summary of the Google Summer of Code Project
author: Mritunjay Sharma
date: '2020-08-28 23:32:23 +0530'
category: GSoC
thumbnail: /assets/img/posts/gsoc.png
keywords: Thanos, go, sre, beginner
permalink: /blog/gsoc-final-report
---

# RTEMS GSoC 2020 Final Report: BSP Buildset for EPICS

What an amazing summer — filled with a lot of moments to cherish — whether those buggy nights or those silly beginner mistakes(that I still make :p) – is coming to an end but of course with the promise of a new beginning :-)


<img src = "https://cdn-images-1.medium.com/max/3200/0*7y-0E-e7AByIygpH.png" width=500 class = center>
<img src = "https://cdn-images-1.medium.com/max/2000/0*WtsM5NxAR-WNg132.png" width=500 class = center>

<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>



## Project Overview:

The development of the freely available, open-source, RTEMS, solved one of the biggest obstacles that beamline researchers used to face in buying commercial Real-Time operating systems that used EPICS base software.

However, with both EPICS and RTEMS gearing up for their latest stable releases, i.e, EPICS7 and RTEMS5 respectively, they should be made fully compatible to ensure continuous support in the newer versions.

This project aims to solve this problem and create a vertically integrated BSP Build Set which will be able to create a bootable BSP image with EPICS7+RTEMS 5 that can run in a simulator. RTEMS provides RTEMS Source Builder which is a tool to build packages from source and adding packages like EPICS to the RSB as libraries is called vertical integration. This project will create build sets that stack library dependencies vertically to create a stack. The main goals of this project are to make progress for some low-barrier simulation approaches like qemu/i386, qemu/xilinx_zynq and powerpc/psim used by EPICS community and then later on move to real hardware systems.

## Mentors:

* Chris Johns

* Gedare Bloom

* Heinz Junkes

* Pavel Pisa

## Project Objectives:

1. Building qemu/xilinx_zynq by hand for EPICS7+RTEMS5

1. Automating (1) using the RSB recipe.

1. Vertically integrating the EPICS package to the RSB Library.

1. Creating a bootable BSP image of (3) that can run in the simulator.

## Summary of the work :

The entire span of work was divided into three phases. Let’s discuss each one of them.

### Phase 1:

The first phase started with a minor change in goal for Phase 1. Working upon the advice of Heinz, I, initially started with adding PTP support to RTEMS RSB. The purpose behind this was to add libbsd network stack support to EPICS instead of the old legacy stack.

To do so, I made the following [changes](https://github.com/ptpd/ptpd/compare/master...mritunjaysharma394:master) in code:

1. Written .cfg and .bset files for ptp daemon in rtems/rsb

1. Modification in ptpd repo to make it suitable for it to build using RSB.

It was later suggested that we should try the intitally decided path of building the EPICS package for RTEMS by hand and then move on to making RSB recipes. This laid the foundation of things to be done in Phase 2.

### Phase 2:

The first step to vertically integrate any third-party package in RTEMS RSB begins by building that package by hand (manually). This made me work to build EPICS7 for RTEMS4.10 for pc386 BSP by hand. We deliberately chose initially RTEMS4.10 as it was stable with the upstream branch of EPICS.

After learning this process for RTEMS4.10, I tried to build it for RTEMS5. Few bugs were encountered and fixed and the build of EPICS7 for RTEMS5 with pc386 BSP was successful.

The entire process is documented in these two blogs:

* [Installing RTEMS and Building BSP by hand](https://medium.com/@mritunjaysharma394/installing-rtems-ecosystem-and-building-your-first-bsp-993d1cf38902)

* [Building EPICS7 for the BSP in RTEMS5 by hand](https://medium.com/@mritunjaysharma394/building-epics7-with-rtems5-by-hand-12d9fbd2eb82)

The next task was to use this documentation to write RSB recipes which will work towards vertical integration of EPICS7.

I worked out on it and eventually, this is how they came out in the form of code commits:

* [The first commit for RSB recipes ](https://github.com/RTEMS/rtems-source-builder/commit/2588079173f0ca02497e591e08999b45ef4e0868)that were not able to cd inside the directory of EPICS package installed.

* [This commit fixed the above issue](https://github.com/RTEMS/rtems-source-builder/commit/160ca29c7ea4716d93a40cfb0436e4e9c5a9036d) but build was failing because configure files has different Path to RTEMS_BASE and the CROSS_COMPILER_TARGET_ARCHS than that of the host.

* [This commit added made the Build run](https://github.com/RTEMS/rtems-source-builder/commit/9f2c72ad963fe8d057482639ac472956509824a4) successfully for EPICS7 with RTEMS5 for pc386 using RSB recipe but it used a [patch](https://github.com/mritunjaysharma394/epics-mritunjay/blob/master/patches/0001-Added-Support-for-RTEMS-pc386.patch) for it. This was just a temporary solution because building patch for each BSP is something that would have been tedious and we are still not giving the dynamic control of setting the RTEMS_BASE and CROSS_COMPILER_TARGET_ARCHS from the terminal.

As the second phase was about to end, I had to take a little break to take part in the Grand Finale of Smart India Hackathon -2020 (August 1st-August 4th), in which my team became a Winner.

The next steps for Phase 3 were very clear — to find out the alternative of using Patches and making the process of selecting the RTEMS_BASE and CROSS_COMPILER_TARGET_ARCHS dynamic for EPICS.

### Phase 3:

In order to implement the goals set by the end of Phase 2, we decided to go with ‘xilinx_zynq_a9_qemu’ BSP instead of pc386 BSP.

The initial steps were almost the same as in Phase 2 of building the BSP by hand. However, this time we used libbsd instead old legacy network stack and so configuration has --disable-networking option and I followed this [blog](https://rmeena840.github.io/Fourth-Post/) of a previous GSoC to build libbsd network stack for ‘xilinx_zynq_a9_qemu’ BSP.

What followed next was a series of commits in the process to build the RSB recipe that can be merged:

* The [initial recipe commit](https://github.com/RTEMS/rtems-source-builder/commit/aa4ac78b8a28b8a238f7951304018acd4c65c096) that used the earlier inefficient patch method.

* Series of discussions led to think of using the sed tool to change the RTEMS_BASE and the CROSS_COMPILER_TARGET_ARCHS variables in piped configuration files of epics-base . This [commit](https://github.com/RTEMS/rtems-source-builder/commit/1e7d7d4237a479dce564711b1080a9b3225e9e9a) tried to do it and it was successful in it.

* There were still two major problems. The first one that sed was not a cross-platform tool. It worked differently for GNU and BSD based systems and therefore I was advised to build some python tool that can work as an alternative for sed. The second problem was that changing the variables RTEMS_BASE and the CROSS_COMPILER_TARGET_ARCHS was still not user-oriented, i.e, it was still getting hardcoded.

* The first problem was fixed and it was fixed in such a way that led to the **creation of my first open-source project **called [**sedpy](https://pypi.org/project/sedpy/) **— a cross-platform open-source distributive (both Python 2.7 and Python 3+) alternative from scratch to be used in RSB cfg scripts. The project released on August 19th has already more than **250 downloads **and is receiving amazing contributions (especially from the first time contributors) here: [https://github.com/mritunjaysharma394/sedpy](https://github.com/mritunjaysharma394/sedpy)

<figure>
<img src = "https://cdn-images-1.medium.com/max/3946/1*g76RxN4fH1qbHHpFtxznqg.png" width=1000 class = center>
<figcaption class = center> RTEMS RSB using sedpy. </figcaption>
</figure>

* However, we found out that in EPICS base we can use of make utility to play with the variables and so I did the same in this [commit](https://github.com/RTEMS/rtems-source-builder/commit/3b9b4178409706bb87c7c80e0bedabf3e47d5726)

* The second problem still remained till the time it struck that we can use of--trace command to generate log reports with macros . This idea given by one of my mentors changed the scenario and finally, we were able to fix the second problem with this [commit](https://github.com/RTEMS/rtems-source-builder/commit/a505877157f63f6ae17906276b3ffcb699ed1297)

* Made few more changes in the RSB recipe based on the feedback of mentors and have sent the[ mergeable patch](https://lists.rtems.org/pipermail/devel/2020-August/061655.html) in the devel mailing list.

* The EPICS 7 finally **built successfully **with RTEMS5 using the RSB recipe with the following user-end command:

    ```../source-builder/sb-builder --with-rtems-bsp="xilinx_zynq_a9_qemu" --log=log_epics epics/epics-base  --trace --prefix=$HOME/development/rtems/5-arm --host=arm-rtems5
    ```

<figure>
<img src = "https://cdn-images-1.medium.com/max/3256/1*aZLE9FtbxircscTjzIyRDg.png" width=1000 class = center>
<figcaption class = center> EPICS Building Successfully using RTEMS RSB. </figcaption>
</figure>

### Links to all my commits and patches:

* [PTP commit](https://github.com/ptpd/ptpd/compare/master...mritunjaysharma394:master)s

* [The very first commit for RSB recipe](https://github.com/RTEMS/rtems-source-builder/commit/2588079173f0ca02497e591e08999b45ef4e0868)

* [The commit that fixed an issue in the above commit](https://github.com/RTEMS/rtems-source-builder/commit/160ca29c7ea4716d93a40cfb0436e4e9c5a9036d)

* [This commit made the Recipe Build run](https://github.com/RTEMS/rtems-source-builder/commit/9f2c72ad963fe8d057482639ac472956509824a4)

* [The initial inefficient RSB recipe commit for ‘xilinx_zynq_a9_qemu’ BSP](https://github.com/RTEMS/rtems-source-builder/commit/aa4ac78b8a28b8a238f7951304018acd4c65c096)

* [Commit that used sed tool to improve the above RSB recipe](https://github.com/RTEMS/rtems-source-builder/commit/1e7d7d4237a479dce564711b1080a9b3225e9e9a)

* [Replaced sed and sedpy with make utility commit](https://github.com/RTEMS/rtems-source-builder/commit/3b9b4178409706bb87c7c80e0bedabf3e47d5726)

* [Used macros to make remove hardcoded values commit](https://github.com/RTEMS/rtems-source-builder/commit/a505877157f63f6ae17906276b3ffcb699ed1297)

* [Final mergeable Patch](https://lists.rtems.org/pipermail/devel/2020-August/061655.html)

## Future Scope:

* Though the Build is working fine with sb-builder , however, RSB recipe need improvements to make it completely compatible with sb-set-builder .

* Though we have EPICS building as a package now, we can move to discuss if it becomes part of the default package set the BSPs build set’s build.

* Integration like this has and still is a long term goal for the mentors. We
would like EPICS to sit here with NASA’s core flight executive so the bar is as low as possible to have these important software packages build ready to run.

* The tests (epics: make runtests and qemu) also has to come into play. This is something we will need to address. The RSB cleans a build environment
once it completes a build. The model is unpack source, patch, configure, build, install, and then clean.

Completing these future goals will be a major part of my **post GSoC involvement. **At the same time, I will try to improve the documentation and introduce more and more people to RTEMS and EPICS community to help both these projects grow further.

I will also express my gratitude and thanks to all my mentors and other members of both RTEMS and EPICS community for their regular support and feedback. It could have never been possible without them✨

It was a summer full of learning and fun with code that I will cherish forever 😃
