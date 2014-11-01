---
layout: blog_item
title:  "HowTo Install KeepNote on OSX Mavericks"
date:   2014-08-09 00:52:10
author: Arr0way
categories: [OSX]
---

**KeepNote** is my favorite tool for note keeping during penetration tests, it's advertised as cross platform and works great on Linux and Windows. But getting it to work on **OSX** takes a bit of work, if you are prepared to jump through some hoops I've documented the installation process below. 

##Install Homebrew

[Homebrew](http://brew.sh) is a package manager for OSX, it allows you to install applications in a similar way to BSD ports. 

{% highlight bash Install Homebrew %}
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
{% endhighlight %}

After running the Brew command the following **Apple Developer Tools** box will appears, click install (you no longer have to install Xcode).  

![Install Apple Developer Tools OSX Mavericks](https://i.imgur.com/a2hVkK5.png "Install Apple Developer Tools OSX Mavericks")

Output:
{% highlight bash %}
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/Library/...
/usr/local/share/man/man1/brew.1

Press RETURN to continue or any other key to abort
==> /usr/bin/sudo /bin/mkdir /usr/local

WARNING: Improper use of the sudo command could lead to data loss
or the deletion of important system files. Please double-check your
typing when using sudo. Type "man sudo" for more information.

To proceed, enter your password, or type Ctrl-C to abort.

Password:
==> /usr/bin/sudo /bin/chmod g+rwx /usr/local
==> /usr/bin/sudo /usr/bin/chgrp admin /usr/local
==> /usr/bin/sudo /bin/mkdir /Library/Caches/Homebrew
==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
==> Installing the Command Line Tools (expect a GUI popup):
==> /usr/bin/sudo /usr/bin/xcode-select --install
xcode-select: note: install requested for command line developer tools
Press any key when the installation has completed.
==> Downloading and installing Homebrew...
remote: Counting objects: 191438, done.
remote: Compressing objects: 100% (52107/52107), done.
remote: Total 191438 (delta 138158), reused 191438 (delta 138158)
Receiving objects: 100% (191438/191438), 38.69 MiB | 4.99 MiB/s, done.
Resolving deltas: 100% (138158/138158), done.
From https://github.com/Homebrew/homebrew
 * [new branch]      master     -> origin/master
HEAD is now at cc18ca2 rabbitmq-c: general cleanup.
==> Installation successful!
==> Next steps
Run `brew doctor` before you install anything
Run `brew help` to get started
{% endhighlight %}

Run brew doctor to confirm the Brew install is working.

{% highlight bash %}
brew doctor
Your system is ready to brew.
{% endhighlight %}

Settle the dependencies for KeepNote using Homebrew:

{% highlight bash install python with brew %}
brew install python aspell sqlite3
{% endhighlight %}

Use Brew to create Python links:

{% highlight bash %}
brew linkapps
{% endhighlight %}

Download:

[PyGTK](http://sourceforge.net/projects/macpkg/files/PyGTK/2.24.0/PyGTK.pkg/download)

[Xquartz](http://xquartz.macosforge.org/landing/)

## Install pyGTK & Xquartz

Install pyGTK, double click the download run through the options, you'll be prompted for your password. 


![PyGTK GTK+ Python OSX Install](https://i.imgur.com/g0cQ4Sh.png "PyGTK GTK+ Python OSX Install")


Then do the same for Xquartz and reboot.

![Quartz Install](https://i.imgur.com/77iRaHQ.png "XQuartz Install")

![XQuartz Mac](https://i.imgur.com/Pnxz9Q0.png "XQuartx Mac")


Download and extract the latest KeepNote somewhere sensible, like /usr/local/share/ and cd into the bin dir and execute ./keepnote - KeepNote will then open, my install complained about some missing fonts in the terminal when launching.


![KeepNote Mac OSX Install](https://i.imgur.com/ADcoDNl.png "KeepNote Mac OSX Install")


I've chosen not to worry about the missing fonts. 
