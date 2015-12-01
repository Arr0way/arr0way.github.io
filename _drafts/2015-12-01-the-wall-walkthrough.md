---
layout: blog_item
title:  "The Wall Boot2Root Walkthrough"
date:   2015-12-02 00:00:10
author: Arr0way
description: 'The Wall Boot2Root - Walkthrough Guide'
categories: [walkthrough]
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
               <p><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Description


**Author:** [@xerubus](https://twitter.com/xerubus)

**Download:** [The Wall VM](https://www.vulnhub.com/entry/the-wall-1,130/) via [@VulnHub](https://twitter.com/VulnHub)

## Enumeration

WHATTTTTTT
An initial Nmap scan yielded no results.

WireShark was used to expose an ARP broadcast for <code>TCP: 1337</code>, a netcat listener was setup on port 1337. The target machine connected with the following message:

{% highlight bash %}
netcat -n -v -l -p

                      .u!"`
                  .x*"`
              ..+"NP
           .z""   ?
         M#`      9     ,     ,
                  9 M  d! ,8P'
                  R X.:x' R'  ,
                  F F' M  R.d'
                  d P  @  E`  ,
     ss           P  '  P  N.d'
      x         ''        '
      X               x             .
      9     .f       !         .    $b
      4;    $k      /         dH    $f
     'X   ;$$     z  .       MR   :$
       R   M$$,   :  d9b      M'   tM
       M:  #'$L  ;' M `8      X    MR
       `$;t' $F  # X ,oR      t    Q;
        $$@  R$ H :RP' $b     X    @'
        9$E  @Bd' $'   ?X     ;    W
        `M'  `$M d$    `E    ;.o* :R   ..
         `    '  "'     '    @'   '$o*"'

             The Wall by @xerubus
         -= Welcome to the Machine =-

If you should go skating on the thin ice of modern life, dragging behind you the silent reproach of a million tear-stained eyes, don't be surprised when a crack in the ice appears under your feet. - Pink Floyd, The Thin Ice

{% endhighlight %}

### Port Scanning

After the machine successfully connected to the netcat listener.

{% highlight bash %}

# nmap -sV -p 1-65535 -A -v 192.168.221.129

Starting Nmap 7.00 ( https://nmap.org ) at 2015-11-30 06:29 EST
NSE: Loaded 132 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 06:29
Completed NSE at 06:29, 0.00s elapsed
Initiating NSE at 06:29
Completed NSE at 06:29, 0.00s elapsed
Initiating ARP Ping Scan at 06:29
Scanning 192.168.221.129 [1 port]
Completed ARP Ping Scan at 06:29, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 06:29
Completed Parallel DNS resolution of 1 host. at 06:29, 0.00s elapsed
Initiating SYN Stealth Scan at 06:29
Scanning 192.168.221.129 [65535 ports]
Discovered open port 80/tcp on 192.168.221.129
SYN Stealth Scan Timing: About 20.45% done; ETC: 06:31 (0:02:01 remaining)
SYN Stealth Scan Timing: About 48.81% done; ETC: 06:31 (0:01:04 remaining)
Discovered open port 1965/tcp on 192.168.221.129
Completed SYN Stealth Scan at 06:30, 104.59s elapsed (65535 total ports)
Initiating Service scan at 06:30
Scanning 2 services on 192.168.221.129
Completed Service scan at 06:32, 96.15s elapsed (2 services on 1 host)
Initiating OS detection (try #1) against 192.168.221.129
NSE: Script scanning 192.168.221.129.
Initiating NSE at 06:32
Completed NSE at 06:32, 7.22s elapsed
Initiating NSE at 06:32
Completed NSE at 06:32, 0.00s elapsed
Nmap scan report for 192.168.221.129
Host is up, received arp-response (0.00027s latency).
Not shown: 65533 filtered ports
Reason: 65533 no-responses
PORT     STATE SERVICE REASON         VERSION
80/tcp   open  http    syn-ack ttl 64 OpenBSD httpd
| http-methods:
|_  Supported Methods: GET HEAD
|_http-server-header: OpenBSD httpd
|_http-title: Site doesn't have a title (text/html).
1965/tcp open  ssh     syn-ack ttl 64 OpenSSH 7.0 (protocol 2.0)
| ssh-hostkey:
|   2048 70:26:15:de:7b:29:9a:56:a3:eb:33:e0:7e:fb:92:d8 (RSA)
|_  256 6c:2b:d1:2c:4f:1c:b5:7a:1b:1e:e9:4b:8e:9b:4b:5a (ECDSA)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.00%I=7%D=11/30%Time=565C336F%P=x86_64-pc-linux-gnu%r(Get
SF:Request,2D9,"HTTP/1\.0\x20200\x20OK\r\nConnection:\x20close\r\nContent-
SF:Length:\x20539\r\nContent-Type:\x20text/html\r\nDate:\x20Sun,\x2029\x20
SF:Nov\x202015\x2022:51:15\x20GMT\r\nLast-Modified:\x20Sat,\x2024\x20Oct\x
SF:202015\x2015:20:23\x20GMT\r\nServer:\x20OpenBSD\x20httpd\r\n\r\n<html>\
SF:n<body\x20bgcolor=\"#000000\">\n<center><img\x20src=\"pink_floyd\.jpg\"
SF:</img></center>\n</body>\n</html>\n\n\n<!--If\x20you\x20want\x20to\x20f
SF:ind\x20out\x20what's\x20behind\x20these\x20cold\x20eyes,\x20you'll\x20j
SF:ust\x20have\x20to\x20claw\x20your\x20way\x20through\x20this\x20disguise
SF:\.\x20-\x20Pink\x20Floyd,\x20The\x20Wall\n\nDid\x20you\x20know\?\x20The
SF:\x20Publius\x20Enigma\x20is\x20a\x20mystery\x20surrounding\x20the\x20Di
SF:vision\x20Bell\x20album\.\x20\x20Publius\x20promised\x20an\x20unspecifi
SF:ed\x20reward\x20for\x20solving\x20the\x20\nriddle,\x20and\x20further\x2
SF:0claimed\x20that\x20there\x20was\x20an\x20enigma\x20hidden\x20within\x2
SF:0the\x20artwork\.\n\n737465673d3333313135373330646262623337306663626539
SF:373230666536333265633035-->\n\n")%r(HTTPOptions,218,"HTTP/1\.0\x20405\x
SF:20Method\x20Not\x20Allowed\r\nDate:\x20Sun,\x2029\x20Nov\x202015\x2022:
SF:51:15\x20GMT\r\nServer:\x20OpenBSD\x20httpd\r\nConnection:\x20close\r\n
SF:Content-Type:\x20text/html\r\nContent-Length:\x20376\r\n\r\n<!DOCTYPE\x
SF:20html>\n<html>\n<head>\n<title>405\x20Method\x20Not\x20Allowed</title>
SF:\n<style\x20type=\"text/css\"><!--\nbody\x20{\x20background-color:\x20w
SF:hite;\x20color:\x20black;\x20font-family:\x20'Comic\x20Sans\x20MS',\x20
SF:'Chalkboard\x20SE',\x20'Comic\x20Neue',\x20sans-serif;\x20}\nhr\x20{\x2
SF:0border:\x200;\x20border-bottom:\x201px\x20dashed;\x20}\n\n--></style>\
SF:n</head>\n<body>\n<h1>405\x20Method\x20Not\x20Allowed</h1>\n<hr>\n<addr
SF:ess>OpenBSD\x20httpd</address>\n</body>\n</html>\n")%r(RTSPRequest,218,
SF:"HTTP/1\.0\x20405\x20Method\x20Not\x20Allowed\r\nDate:\x20Sun,\x2029\x2
SF:0Nov\x202015\x2022:51:15\x20GMT\r\nServer:\x20OpenBSD\x20httpd\r\nConne
SF:ction:\x20close\r\nContent-Type:\x20text/html\r\nContent-Length:\x20376
SF:\r\n\r\n<!DOCTYPE\x20html>\n<html>\n<head>\n<title>405\x20Method\x20Not
SF:\x20Allowed</title>\n<style\x20type=\"text/css\"><!--\nbody\x20{\x20bac
SF:kground-color:\x20white;\x20color:\x20black;\x20font-family:\x20'Comic\
SF:x20Sans\x20MS',\x20'Chalkboard\x20SE',\x20'Comic\x20Neue',\x20sans-seri
SF:f;\x20}\nhr\x20{\x20border:\x200;\x20border-bottom:\x201px\x20dashed;\x
SF:20}\n\n--></style>\n</head>\n<body>\n<h1>405\x20Method\x20Not\x20Allowe
SF:d</h1>\n<hr>\n<address>OpenBSD\x20httpd</address>\n</body>\n</html>\n");
MAC Address: 00:0C:29:31:3C:84 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: OpenBSD 5.X
OS CPE: cpe:/o:openbsd:openbsd:5
OS details: OpenBSD 5.0 - 5.4
Uptime guess: 0.000 days (since Mon Nov 30 06:32:25 2015)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=258 (Good luck!)
IP ID Sequence Generation: Randomized

TRACEROUTE
HOP RTT     ADDRESS
1   0.27 ms 192.168.221.129

NSE: Script Post-scanning.
Initiating NSE at 06:32
Completed NSE at 06:32, 0.00s elapsed
Initiating NSE at 06:32
Completed NSE at 06:32, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 210.78 seconds
           Raw packets sent: 131193 (5.775MB) | Rcvd: 91 (4.108KB)

{% highlightend %}

W0t

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
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
               <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>OpenBSD httpd</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 1965</code></p></pc>
           </td>
           <td>
              <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 7.0 (protocol 2.0)</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

### HTTP Enumeration

Interrogation of the page source revealed a code comment with an **ASCII** encoded string:

{% highlight html %}
<!--If you want to find out what's behind these cold eyes, you'll just have to claw your way through this disguise. - Pink Floyd, The Wall

Did you know? The Publius Enigma is a mystery surrounding the Division Bell album.  Publius promised an unspecified reward for solving the
riddle, and further claimed that there was an enigma hidden within the artwork.

737465673d3333313135373330646262623337306663626539373230666536333265633035-->
{% endhighlight %}

Decoding revealed the MD5 hash:<code>steg=33115730dbbb370fcbe9720fe632ec05</code>

#### MD5 Hashcat

The discovered hash was cracked with **HashCat**:

{% highlight bash %}
hashcat -m 0 -a 0 thewall.txt /usr/share/wordlists/rockyou.txt
{% endhighlight %}

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Hash</th>
      <th>Password</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>33115730dbbb370fcbe9720fe632ec05</code></p></pc>
           </td>
           <td>
               <pc><p>divisionbell</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>
