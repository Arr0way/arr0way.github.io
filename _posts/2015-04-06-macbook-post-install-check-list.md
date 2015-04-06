---
title: 'MacBook - Post Install Config + Apps'
layout: blog_item
date: 2015-04-06 01:04:18
author: Arr0way
description: 'List of initial OSX config changes and app installs, everything I do after installing OSSX.'
categories: [urandom]
tags:
- 'mac-osx'
---


Just for fun, here is my list of post install config changes plus list of apps I install after installing <code> Mac OS X </code>

##Side Dock

Open: <code>System Preferences > Dock</code> click **Left**.

![Max OS X Dock Left hand side](/img/blog/macbook-post-install-config-apps/mac-osx-dock-on-left.png)

##Enable Right Click MacBook Trackpad

Open: <code>System Preferences > Trackpad</code> click **click in bottom right corner** under *Secondary Click*.

![Max OS X Enable Right Click Trackpad](/img/blog/macbook-post-install-config-apps/mac-osx-enable-right-click-trackpad.png)

##Enable path view in finder 

Open Finder <code>View > Show Path Bar</code>

The path will render at the bottom of the Finder window. 

##Create symlink for /Volumes 

{% highlight bash %}
ln -s /Volumes/ ~/Desktop/Volumes/
{% endhighlight %} 

If I don't do this, my cifs mounts show as a single drive - if you have none browsable cifs mounts then this can become a pain.

##Mount smb / cifs share in OSX 

Open Finder, press <code>CMD+k</code> and enter the path of your smb <code>smb://</code> or cifs <code>cifs://</code> server. 

![Max OS X Mount Cifs / SMB Share](/img/blog/macbook-post-install-config-apps/mac-osx-mount-cifs-share.png)

## Finder show file extensions 

Show file extensions on Mac OS X, open Finder: <code>Preferences > Advanced</code> tick **Show all filename extensions**, I also untick **Show warnnig before changing an extension** 

![Max OS X Show File Extensions in Finder](/img/blog/macbook-post-install-config-apps/os-x-finder-show-file-extensions.png)

##Delete junk from dock 

Delete all unused apps / icons from dock.

##Increase Terminal Font Size

<code>Terminal > Preferences > Text</code> adjust font size. 

##Install Brew  

## Setup home brew 
{% highlight bash %}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Install command line tools when prompted.

## Setup Jekyll

###Install RVM 

{% highlight bash %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
{% endhighlight %}

###Install Jekyll 

{% highlight bash %}
gem install jekyll 
{% endhighlight %}

###Trouble Shooting Jekyll Install 

you don't have write permissions for the /library/ruby/gems/2.0.0 directory. mac

Close and reopen terminal, re-run gem install jekyll - the gems should build fine, **DO NOT USE** sudo. 

cd into your jekyll dir and run "jekyll serve" 

Install the following via brew:

{% highlight bash %}
nmap vim netcat git
{% endhighlight %}

Copy .ssh back over for ssh keys. 

## Install Apps 

List of Apps I use. 

- Evernote 
- Disk Inventory X (for when du -h doesn’t cut it) 
- Home Brew - It’s self contained, easy to null should you need to. 
- VLC
- Wireshark 
- uTorrent 
- MS Office for Mac
- Google Drive 
- Alfred 2
- The Unarchiver 
- Xcode
- iTerm 
- OSX Command Line Tools
- MacVim 
- Reader 2 - RSS reader + feedly 
- Quassel Client - IRC
- Skype 
- Spotify
- Firefox 
- Chrome
- Airmail 2 
- VMWare Fusion 
- KeePassX 
- Caffeine 
- Trim Enabler (only needed for after market SSD's) 
- iStat Menus 5
- Battery Logger

## Swap spotlight for Alfred - shortcut key. 

First go to <code>Settings > Spotlight</code> and untick "Spotlight search keyboard shortcut: CMD + SPACE"

Open Alfred and change it to <code>CMD+SPACE</code> 

## Disable "Automatically rearrange Spaces"

This drives me insane, <code>System Preferences > Mission Control</code> untick **Automatically rearrange Spaces based on recent use**   

## Change OSX Screenshot Location 

{% highlight bash %}
defaults write com.apple.screencapture location ~/Pictures/Screenshots

killall SystemUIServer 
{% endhighlight %}

When using an aftermarket SSD, I used [Trim Enabler](https://www.cindori.org/software/trimenabler/) 

<div class="note warning">
  <h5>Warning turns off text security setting for ktexts.</h5>
  <p>Ktext signing works by checking if all the drivers in the system are unaltered by a third party, or approved by Apple. If they have been modified, Yosemite will no longer load the driver.</p>
</div>

![Max OS X Enable Trim](/img/blog/macbook-post-install-config-apps/trim-enabler-disable-ktexts.png)

## Disable Sudden Motion Sensor - SMS 

Again if you have an Apple SSD, skip this. 

{% highlight bash %}
sudo pmset -a sms 0
{% endhighlight %}

confirm with: 

{% highlight bash %}
pmset -g 
{% endhighlight %}

## Disable sleep image

{% highlight bash %}
sudo rm /private/var/vm/sleepimage
sudo pmset -a hibernatemode 0 
pmset -g | grep hibernatemode 
{% endhighlight %}

All I can think of for now... 
