---
layout: post
title:  "Installing RTEMS Ecosystem and Building your First BSP!"
summary: Tutorial to setup RTEMS development enviornment and building BSP.
author: Mritunjay Sharma
date: '2020-07-11 23:00:00 +0530'
category: GSoC
thumbnail: /assets/img/posts/rtems.png
keywords: tutorial, rtems, bsp, linux, gsoc
permalink: /blog/install-rtems
---

# Installing RTEMS Ecosystem and Building your First BSP!

This tutorial is a part of my learnings during my stint with GSoC 2020 at RTEMS which will also help you to begin your own ✨

It is a very beginner-friendly tutorial and will try to smoothen your journey to get started with Embedded Linux Development 😄

This being the first one will talk about setting up the development environment and getting RTEMS on the host (In our case Ubuntu Linux 20.04) along with it building a Board Support Package (BSP) for it.

### Before we begin…

Shouldn’t we discuss a little bit about RTEMS? No?

Very briefly, The** Real-Time Executive for Multiprocessor Systems (RTEMS)** is a multi-threaded, single address-space, a real-time operating system with no kernel-space/user-space separation. It is used in space flight, medical, networking and many more embedded devices. RTEMS currently supports 18 processor architectures and approximately 200 BSPs. These include ARM, PowerPC, Intel, SPARC, RISC-V, MIPS, and more.

RTEMS is currently **orbiting Mars **as part of the Electra software radio on NASA’s Mars Reconnaissance Orbiter, and the ESA’s Trace Gas Orbiter.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2048/0*_4GfHkmY3p1OjYyd.jpg" width=1000 class = center>
<figcaption class = center> RTEMS Ecosystem</figcaption>
</figure>

### What are we going to do?

* Select a place where we will keep the RTEMS source-code.

* Build a tool suite. (Don’t worry, it’s easy)

* Build a BSP. (Testing it on QEMU will be covered in another tutorial)

### Let’s begin

Okay, we cannot cook food without having the ingredients, no? Similarly, the RTEMS tools we will be using require some dependencies so that they can be cooked well 😉

So open your terminal (Ctrl+Alt+T) and enter the following:

    $ sudo apt-get install build-dep build-essential gcc-defaults g++ gdb git unzip pax bison flex texinfo unzip python3-dev libncurses5-dev zlib1g-dev

Other than this you also require python2.7(dev version) which can be installed with the below-mentioned snippet:

    $ sudo apt-get install python-dev

Now the system is prepared and should hopefully work without any unnatural behaviour.

1. Starting clean we create a development directory :

    $ cd~
    $ mkdir development

2. Create rtems directory under developmentand download the RTEMS source tree.

    $ cd development
    $ mkdir rtems
    $ cd rtems
    $ git clone git://git.rtems.org/rtems.git rtems-5

After the cloning, press ls and you should see rtems-5 there.

    development
    |__rtems 
       |__rtems-5

Till now, you have successfully downloaded the RTEMS source tree. Now we will enter the rtems-5 directory and switch to 5 branch (Since ‘master’ branch and the branch we are working on our different)

    $ cd rtems-5
    $ git checkout 5

3. After this do cd .. to return to development/rtemsdirectory. What next? We will have to build a tool suite (that will help build BSPs) and that is very easy to do if we use RTEMS Source Builder (RSB). Embedded development typically uses cross-compiling toolchains, debuggers, and debugging aids. Together we call these a **toolset**. The RTEMS Source Builder is designed to fit this specific niche but is not limited to it. You can know more about it [here](https://docs.rtems.org/releases/rtems-docs-4.11.2/rsb.pdf). For now, let’s focus on how to get and work with RSB.

In your development/rtems directory do the following.

    $ mkdir src
    $ cd src
    $ git clone git://git.rtems.org/rtems-source-builder.git rsb

As told earlier, we are working with branch 5 of RTEMS, so the RSB also has to be switched to the branch 5 .

    $ cd rsb
    $ git checkout 5

The BSP that we will be using for this tutorial is pc386 and so we need to build the tool suit for it. We will do this by entering the development/rtems/src/rsb/rtems directory and entering the following command.

    $ cd rtems
    $ ../source-builder/sb-set-builder --prefix=$HOME/development/rtems/5 5/rtems-i386

**Caution: This is a confusion that even I had and I don’t want that anyone suffers again. The Path to prefix that we have set is different from the RTEMS source tree we installed. The path to prefix will come into existence after the above command is entered. You don’t need to create the path to prefix (i.e the ‘5’ you see after ‘rtems’ in prefix=$HOME/development/rtems/5 ) on your own.**

The above step will take a little time in completion as the Toolchain Build completes.

4. In order to check that you have been doing it all right till now, come back to development/rtems directory and it should look something like this:

    development
    |__rtems
       |__rtems-5
       |__src
       |   |__rsb
       |__5

If all is okay then we have to set the following PATH variable next.

Inside the development/rtems directory itself, enter:

    export PATH=$HOME/development/rtems/5/bin:$PATH

We will enter the RTEMS source tree rtems-5 now and complete the Bootstrapping part with the following commands

    $ cd rtems-5
    $ ./bootstrap -c && $HOME/development/rtems/src/rsb/source-builder/sb-bootstrap

5. After the Bootstrap is complete, return to development/rtems directory and now create another directory to store the kernels of BSPs that we will be building/installing. Follow below:

    $ mkdir kernels-5
    $ cd kernels-5
    $ mkdir pcp-386
    $ cd pcp-386 

You should now be somewhere here $HOME/development/rtems/kernels-5/pcp-386 .

6. We will now be creating the Makefile with the below-mentioned configuration. Enter this now:

    $ $HOME/development/rtems/rtems-5/configure --target=i386-rtems5 --enable-rtemsbsp=pc386 --enable-tests=samples --prefix=$HOME/development/rtems/5 --enable-cxx --enable-networking --enable-posix

7. A makefile will be created after this. The steps after this are very easy.

    $ make

This may take a little time. To see if you have done this correctly you should get something like this as the last few lines in the terminal:

    make[1]: Entering directory '/home/mritunjay/development/rtems/kernels-5'
    make[1]: Nothing to be done for 'all-am'.
    make[1]: Leaving directory '/home/mritunjay/development/rtems/kernels-5'

8. Now just install using:

    $ make install

And yes, that’s it! You have finally installed RTEMS and Build your first BSP too 🏁

What next? In this[ next tutorial](https://medium.com/@mritunjaysharma394/building-epics7-with-rtems5-by-hand-12d9fbd2eb82), find how to build EPICS7 for RTEMS 5 (pc-386) BSP by hand!

Thank you so much for reading the tutorial!

For any queries and feedback you can post your responses below and as well as can reach out to me on :

Twitter : [https://twitter.com/mritunjay394](https://twitter.com/mritunjay394) (preferrable)

Linkedin: [https://www.linkedin.com/in/mritunjay394/](https://www.linkedin.com/in/mritunjay394/)

### References:

1. [https://www.rtems.org/](https://www.rtems.org/)

1. [https://docs.rtems.org/branches/master/user/installation/index.html#](https://docs.rtems.org/branches/master/user/installation/index.html#)

1. [https://docs.rtems.org/releases/rtems-docs-4.11.2/rsb.pdf](https://docs.rtems.org/releases/rtems-docs-4.11.2/rsb.pdf)
