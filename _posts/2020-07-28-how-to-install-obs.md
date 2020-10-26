---
layout: post
title:  "How to install OBS on Windows 10 and Live Stream on YouTube?"
summary: Tutorial to Install OBS Studio on Windows 10
author: Mritunjay Sharma
date: '2020-07-28 23:00:00 +0530'
category: GSoC
thumbnail: /assets/img/posts/obs.png
keywords: tutorial, OBS, YouTube, Stream, Windows
permalink: /blog/obs-yt-tutorial
---


# How to install OBS on Windows 10 and Live Stream on YouTube?

It’s 2020 and it’s surely not that Corona alone has gone viral — Live Streams are not only viral now but also have gone mainstream 📹

And when we talk about Live Streams, it will be a dismal injustice if we don’t talk about one of the biggest free and open source software for video recording and live streaming— **OBS Studio!**

In this tutorial, we will learn how to install and configure (minimal for now) it for a **YouTube Stream**. So, let’s begin!

### Before we install OBS…

Yes, before installing OBS for YouTube, **we need to ensure that our YouTube Live Streaming Feature is enabled.**

Don’t worry if it isn’t, just follow the below steps and wait for 24 hours (Ahh! yes 😢) and it will be enabled if it isn’t already 😃

***Steps to Activate Live Streaming your Account:***

* Login to your YouTube account.

* Click the upload icon (it is like a camera with a plus in the center, in the upper right hand corner)

* Click ‘Go Live.’

* After clicking ‘Go Live’, YouTube will prompt you to activate your live streaming account.

* Follow the Instructions.

* It takes 24 hours to activate your account for live streaming. Once activated, you can go live instantly.

### Installing OBS

**Step 1:** Very simple — just go to this link [https://obsproject.com/](https://obsproject.com/) and select Windows. Your OBS-Studio Windows Installer will be downloaded.

**Step 2:** Run the installer and give it the permission to make changes. Now on the setup window click on next, accept the license agreement, select your installation path and click on install button.

**Step 3:** After the installation, OBS-Studio will open automatically. Carefully follow these steps as told below.

Run the Auto-Configuration Wizard by clicking the “Yes” button.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2148/1*yJ1fB7LfOpbF6_v0y4oPig.jpeg" width=1000 class = center>
<figcaption class = center> Click Yes </figcaption>
</figure>

**Step 4:** Select ‘Optimize for streaming, recording is secondary option’ and click next.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2168/1*GFQoOyeP6VC6CtOweCVs2g.jpeg" width=1000 class = center>
<figcaption class = center> Optimize for streaming, recording is secondary option </figcaption>
</figure>


**Step 5:** Continue with the default options visible and clicking “Next” till you encounter something like this.

<figure>
<img src = "https://cdn-images-1.medium.com/max/3488/1*UrgtI43mv0_NBIdnvy7VVg.jpeg" width=1000 class = center>
<figcaption class = center> We are going to use for YouTube. So we need to change the settings. </figcaption>
</figure>

Now change the Service to ‘YouTube’ and click ‘Next’.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2972/1*cKX4-GoHJ5HHINxZ8jYtAg.jpeg" width=1000 class = center>
<figcaption class = center> Select YouTube </figcaption>
</figure>

**Step 6:** You will have something like this now and now you should click on the ‘Get Stream Key’ option. It will take you to your own YouTube Studio Home Page where the stream key will be available.

<figure>
<img src = "https://cdn-images-1.medium.com/max/3044/1*cK8Hq4Sx7zGXFiMmbGZ0XQ.jpeg" width=1000 class = center>
<figcaption class = center> Click ‘Get Stream Key’ </figcaption>
</figure>

**Step 7:** Reveal and Copy the Stream Key. Paste it on the window above and click ‘Next’.

<figure>
<img src = "https://cdn-images-1.medium.com/max/7668/1*fixW5HHdNKo2Qv9cZPKd1Q.jpeg" width=1000 class = center>
<figcaption class = center> Reveal and copy the Stream Key </figcaption>
</figure>

**Step 8:** Apply the settings and here it is — OBS Studio installed on your machine.🏁

I will also recommend you to enable the Studio Mode which will help you with Transitions and Streaming.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2754/1*rK0hENhKC1wRh82e07NhLg.jpeg" width=1000 class = center>
<figcaption class = center> Default Screen of OBS installed on Windows 10. </figcaption>
</figure>

### Live Streaming to YouTube

Now that we have installed OBS, let’s move on to Live Streaming something on YouTube.

For this Tutorial, I **will do a “Windows Capture” of VS Code and stream it live on YouTube**. It totally depends on you on what you will like to prefer.

So, let’s begin!

**Step 1:** Can you see that ‘+’ button in ‘Sources’ section? Yes, click it and select “Windows Capture” option. After which you will have screen something like this. I have made the name of the Source as ‘VS Code’ and you can of course choose a name of your liking.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2782/1*tcPFuPT7k01kfUE5GWj3MA.jpeg" width=1000 class = center>
<figcaption class = center> Creating Windows Capture for VS Code. </figcaption>
</figure>

On Clicking ‘OK’, you will have to select the ‘Window’ you want to capture. In my case, I selected VS Code. Click ‘OK’ and proceed.

<figure>
<img src = "https://cdn-images-1.medium.com/max/2768/1*meUP8WbyTONnO7nvJ5m7Gg.jpeg" width=1000 class = center>
<figcaption class = center> Select the Window and Proceed with clicking ‘OK’ </figcaption>
</figure>

**Step 2:** You can modify the window as per your requirement, I preferred here to use “Fit to Screen” (Right click on the window area inside OBS and you will have the option visible).

<figure>
<img src = "https://cdn-images-1.medium.com/max/4238/1*AmMcSAFZJmDCM5xhFwP6ZQ.jpeg" width=1000 class = center>
</figure>

**Step 3**: Open your browser and go to your YouTube Page and click the ‘Go live’ option.
<figure>
<img src = "https://cdn-images-1.medium.com/max/2000/1*IH2o66EuCDEYVXWNKKQSqw.jpeg" width=1000 class = center>
<figcaption class = center> Select ‘Go Live’. </figcaption>
</figure>

**Step 4:** You will be redirected to YouTube Studio Control Room and now have to configure the settings which are somewhat like this in my case. I have deliberately kept the video ‘Unlisted’ and you are always welcome to make it ‘Public’ if it’s meant for them . After this, click the “Create Stream” option that is there below these configure settings.

<figure>
<img src = "https://cdn-images-1.medium.com/max/7434/1*vo75hw20qhtCcpfGvpY6YA.jpeg" width=1000 class = center>
<figcaption class = center> Configure your YouTube Stream. </figcaption>
</figure>

**Step 5:** Now Copy the “Stream Key” again.
<figure>
<img src = "https://cdn-images-1.medium.com/max/5674/1*_mANc9ry0Zt9I2IjEVx0Zg.jpeg" width=1000 class = center>
<figcaption class = center> Copy the new Stream Key. </figcaption>
</figure>

**Step 6**: Come back to OBS. Select the File->Settings option. Go to ‘Stream’ and paste the new “Stream Key”.

<figure>
<img src = "https://cdn-images-1.medium.com/max/3714/1*tOc9PakIv0XIKqlgn2gdhg.jpeg" width=1000 class = center>
<figcaption class = center> Paste the new Stream Key. </figcaption>
</figure>

**Step 7:** Click on the Start Streaming option on OBS.

<figure>
<img src = "https://cdn-images-1.medium.com/max/3770/1*CkIVMKzhIuQxn0jnvT3khw.jpeg" width=1000 class = center>
<figcaption class = center> Start Streaming on OBS. </figcaption>
</figure>

**Step 8:** Return to your browser where the YouTube Studio Control Room is opened. Click the **‘Go Live’ **option and wohoo!** You are live!**

<figure>
<img src = "https://cdn-images-1.medium.com/max/7680/1*tVlxe10pI6HBUHkQloEn8Q.jpeg" width=1000 class = center>
<figcaption class = center> Get ready to ‘Go Live’</figcaption>
</figure>


Thank you so much for reading the tutorial! I sincerely hope that will help you to start with OBS 😃

For any queries and feedback, you can post your responses below and as well as can reach out to me on :

Twitter: [https://twitter.com/mritunjay394](https://twitter.com/mritunjay394) (preferable)

Linkedin: [https://www.linkedin.com/in/mritunjay394/](https://www.linkedin.com/in/mritunjay394/)

<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>