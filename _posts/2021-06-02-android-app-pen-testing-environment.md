---
layout: blog_item
title: "Android Pen Testing Environment Setup"
date:   2021-06-02 15:29:10
author: Arr0way
description: 'A step-by-step guide for setting up and configuring an Android Pen Testing Environment using Burp Suite & GenyMotion.'
categories: [cheat-sheet]
tags:

- 'Mobile App Penetration Testing'
---

* list element with functor item
{:toc}

This document covers the least exciting aspect of Android mobile app security testing, configuring the testing environment. It is both time consuming and an extremely important part of the assessment process to get right. This guide covers setup of GenyMotion with Burp Suite on Mac OS, but it should be trivial to replicate on Linux or Windows. 

<!--more-->

## Install GenyMotion 

GenyMotion is the android emulator of choice for dynamic android app security testing. 

Installation on mac requires Virtual Box to be installed first, then run through the GenyMotion installer.  

1. Install Android device (Nexus 4 works well) 
2. Select Android 8.1 and deploy 

## Setup Burp Proxy with GenyMotion

If you are using DHCP you may want to statically assign an address, as the IP randomly changing requires this process to be completed again (which can get extremely annoying...). 

### 1. GenyMotion Burp Proxy Settings 

1. Select GenyMotion
2. Preferences
3. Network 
4. Proxy Settings and tick HTTP and add your local interface address and a different port to one that Burp is using


![Geny Motion Burp Proxy Settings](/img/android-pen-testing-env-01.png "Geny Motion Burp Proxy Settings")

### 2. Android 8.1 Proxy Settings

1. Swipe down the top and select Settings 
2. Tap Network & Internet > Wi-Fi > Long Tap on the connected Wi-Fi network and Select Modify Network 
3. Tap Advanced > Proxy > Manual and enter the same Proxy settings you entered in step 1

![Android Burp Proxy Settings](/img/android-pen-testing-env-02.png "Android Burp Proxy Settings")

### 3. Android Burp Certificate Installation 

1. Go to your web browser and download the certifcate file from http://burp 
2. Rename it to .cer 
3. Drag it into the running GenyMotion phone (this will place the file at /sd-card/)
4. On the phone go to Settings > Security & Location > Encryption & Location > Install from SD card (Install certificates from SD card) 
5. Click Downloads on the left and select the .cer file 
6. Install the certificate and call it Burp 

![Android Burp Certificate Install](/img/android-pen-testing-env-03.png "Android Burp Certificate Install")

7. You will need to set a pin code, set one 

### 4. Burp Proxy Settings

Add a Burp proxy on the interface with the IP and port used at step 1 

### 5. ADB 

1. Install brew 
2. ```brew install android-platform-tools``` 
3. ```adb devices``` 

{% highlight bash %}

List of devices attached
192.168.XX.XXX:5555	device

{% endhighlight %}

4. adb shell 

{% highlight bash %}
vbox86p:/ # ls
{% endhighlight %}

Your id should be root on GenyMotion. 

### 6. Installing APK FIles 

There are two options for installing APK files, using adb or dragging and dropping. 

Using ADB: 

{% highlight bash %}
adb install file.apk
{% endhighlight %}

Or drag and drop the apk file into the running GenyMotion Android device. 

### 7. ADB Basic Commands

Installed Android application location: 

{% highlight bash %}
cd /data/data
{% endhighlight %}

For a more in depth guide on how to use ADB see our [ADB commands](/blog/adb-commands-cheat-sheet/) cheat sheet here.

### 8. Open GApps

If you are assessing an application from the Play Store then you can install open gapps in GenyMotion by clickin on the icon on the right hand menu. 

Enjoy. 
