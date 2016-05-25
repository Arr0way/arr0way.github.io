---
layout: blog_item
title:  "HowTo: Kali Linux Chromium Install for Web App Pen Testing"
date:   2016-03-24 11:29:10
author: Arr0way
description: 'HowTo Install Chromium on Kali Linux and setup for Web Application Testing'
categories: [Kali Linux]
tags:
- Kali
- 'Web Application Penetration Testing'
- Chromium
---

* list element with functor item
{:toc}

## Why use Chromium for Web Application Testing ?

The primary reason I use Chromium is for DOM based XSS testing which as far as I know cannot be disabled in Firefox. If you have never heard of Chromium it's the opensource version of Google Chrome and doesn't have flash player built in and various other codecs such as: AAC, H.264, and MP3 Support.

It's possible to disable all security features in Chromium or Chrome using the switch <code>--disable-web-security</code>, this will disable all security options and allow you to test for DOM based XSS.


## Kali Install Chromium Browser

Chromium exists within the Kali repositories and can be installed using:    

{% highlight bash %}
apt-get install chromium
{% endhighlight %}

## Chromium Won't Launch on Kali

By default chromium won't launch on Kali Linux, this is due to chromium running as the root user. You can fix this by opening <code>/etc/chromium.d/default-flags</code> in vim and adding the following lines:

{% highlight bash %}
# Run as root Kali
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --password-store=detect --no-sandbox --user-data-dir"
{% endhighlight %}

This disables the <code>user-data-dir</code> and <code>sandboxing</code>, disabling sandboxing will have some obvious security issues but this browser is for web application penetration testing only.

## Chromium Setup for Web Application Testing

In order to use chromium for Web Application Penetration Testing you need to disable all the security features, allowing for DOM based XSS testing in chromium.


{% highlight bash %}
# Disable Chromium security features for web app testing
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --disable-web-security"
{% endhighlight %}

## Complete Chromium Config

What my entire Chromium config looks like:

{% highlight bash %}
# A set of command line flags that we want to set by default.

# Do not hide any extensions in the about:extensions dialog
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --show-component-extension-options"

# Don't use the GPU blacklist (bug #802933)
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --ignore-gpu-blacklist"

# Run as root Kali
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --password-store=detect --no-sandbox --user-data-dir"

# Disable Chromium security features for web app testing
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --disable-web-security"

{% endhighlight %}

## Kali Chromium Error: You Are using an Unsupported Command line flag --disable-web-security. Security and Stability will suffer

Ignore the following error, Chromium still process DOM based XSS. The same error occurs in Google Chrome.

![Kali Chromium Error: You Are using an Unsupported Command line flag --disable-web-security. Security and Stability will suffer](/img/blog/kali-chromium/kali-chromium-web-app-testing.png)


Enjoy.
