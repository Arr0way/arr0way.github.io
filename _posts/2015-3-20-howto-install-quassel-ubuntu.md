---
layout: blog_item
title:  "HowTo Install Quassel on Ubuntu"
date:   2015-03-20 00:52:10
author: Arr0way
categories: [Ubuntu]
---

**Quassel IRC**, comes in two parts an IRC client and a bouncer called "Quassel Core". Quassel Core works like a BNC, ZNC bouncer keeping your chats / channels online while you're client machine is disconnected. GUI Quassel Clients are available for Windows, Linux and OSX - which was the deal breaker for me.

Setting Quassel up from source proved to be a bit of nightmare and I spent a fair amount of time getting this right, so I thought I'd share my notes on setting up Quassel Core on Ubuntu.

## Install Quassel PPA Ubuntu Repository

{% highlight bash %}
sudo add-apt-repository ppa:mamarley/quassel
{% endhighlight %}


## Install Quassel Core on Ubuntu

{% highlight bash %}
sudo apt-get install quassel-core
{% endhighlight %}

Check it's running:

{% highlight bash %}
netstat -tulpn
{% endhighlight %}

Quassels default port is 4242:

{% highlight bash %}
tcp6       0      0 :::4242                 :::*                    LISTEN      -
{% endhighlight %}

Connect your Quassel Client to your server and follow the setup wizard.

Enjoy.
