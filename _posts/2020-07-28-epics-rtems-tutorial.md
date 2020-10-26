---
layout: post
title:  "Building EPICS7 with RTEMS5 by hand"
summary: Tutorial to build EPICS-7 with RTEMS by hand(manually)
author: Mritunjay Sharma
date: '2020-07-28 23:32:23 +0530'
category: GSoC
thumbnail: /assets/img/posts/epics.png
keywords: tutorial, RTEMS, EPICS, beginner, GSoC
permalink: /blog/epics-rtems-tutorial
---

# Building EPICS7 with RTEMS5 by hand

***CAUTION***: The EPICS branch, I will be using in this blog is not an upstream branch, be warned to use it only for experimental purpose.

In the [previous](https://medium.com/@mritunjaysharma394/installing-rtems-ecosystem-and-building-your-first-bsp-993d1cf38902) blog where I tried to explain how to build a BSP for RTEMS5. This time I will go a step further and will tell you how to build the same for EPICS! But hey, you might be wondering what is EPICS?

### What is EPICS?

EPICS offers a collection of software tools for the construction of distributed control systems for experimental physics projects.

![EPICS (Pic Source: [https://epics.mpg.de/index.php](https://epics.mpg.de/index.php))](https://cdn-images-1.medium.com/max/2000/0*Rn5Fz1HH9kJd7QWM)
*EPICS (Pic Source: [https://epics.mpg.de/index.php](https://epics.mpg.de/index.php))*

EPICS includes a runtime database, robust network protocols, an extensive collection of device drivers for hardware connectivity, and a set of client tools for operator control and monitoring. It also includes data archiving and alarms.

### Let’s build!

Prerequisites: Everything will be good to go if you have installed/built pc-386 BSP from this [tutorial](https://medium.com/@mritunjaysharma394/installing-rtems-ecosystem-and-building-your-first-bsp-993d1cf38902)

Step 1: Download this particular branch of [EPICS](https://github.com/hjunkes/epicsBaseOwnPlayground/archive/3afec267ab08568ea454789e562450b00feea5c0.zip). This branch is “playground” version of EPICS of my mentor Heinz which has some key components to support RTEMS 5.

Step 2: Get inside the ‘development’ directory as created and discussed in the previous blog and make another directory named EPICS.

    mkdir EPICS 

Unzip the downloaded Zip file inside this directory.

Step 3: The development directory should look somewhat like this now.

    development
    |__EPICS
       |__epicsBaseOwnPlayground-3afec267ab08568ea454789e562450b00feea5c0
    |__rtems
       |__rtems-5
       |__src
       |   |__rsb
       |__5
       |__kernels-5
          |__pc-386

Step 4: Rename ‘epicsBaseOwnPlayground-3afec267ab08568ea454789e562450b00feea5c0’ to ‘epics-sep’ just for a simpler name. Now cd into ‘epics-sep’

    cd epics-sep

Step 5: Open your favourite code editor at this point of the directory as now we have to make few changes in the configuration files of ‘epics-sep’ to build pc-386 BSP.

Step 6: In epics-base/configure/CONFIG_SITE, set:

    CROSS_COMPILER_TARGET_ARCHS= RTEMS-pc386

Step 7: Then we have to set in configure/os/CONFIG_SITE.Common.RTEMS,

where to find RTEMS part.

    # Where to find RTEMS

    #

    # APS:

    RTEMS_SERIES = 5

    RTEMS_VERSION = 5

    RTEMS_BASE = /home/mritunjay/development/rtems/$(RTEMS_VERSION)

Please change the RTEMS_BASE according to the location where you have stored it.

Step 8: Save the above changes and run make

    make

And there it is! You have built EPICS7 for RTEMS5 for pc-386 successfully!

Thank you for reading this! For any queries and feedback you can post your responses below and as well as can reach out to me on :

Twitter : [https://twitter.com/mritunjay394](https://twitter.com/mritunjay394) (preferrable)

Linkedin: [https://www.linkedin.com/in/mritunjay394/](https://www.linkedin.com/in/mritunjay394/)

**References:**

1. [https://epics.mpg.de/index.php](https://epics.mpg.de/index.php)
