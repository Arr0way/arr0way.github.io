---
layout: blog_item
title:  "Web Application Testing OWASP - Burp Suite Injection Flaws"
date:   2015-03-29 14:37:10
author: Arr0way
description: 'OWASP Web Application Testing with Burp Suite, working through OWASPS WebGoat - part of the BWAP series.'
categories: [Web Application Testing]
tags:
- "Burp Suite"
- "Web App"
- "Web Goat"
---

## Introduction

This blog post documents working through OWSAPs WebGoat part of the [Broken Web Application Project](https://www.owasp.org/index.php/OWASP_Broken_Web_Applications_Project) application with Burp Suite, no other tools will be used for this post, it's essentially an OWASP Burp Suite tutorial.

![OWASP WebGoat](/img/blog/owasp-web-app-testing/webgoat.jpg)

If you want to follow along you can do so with [Kali Linux](https://www.kali.org/) and the free version of Burp Suite, the [OWASP Broken Web Applications Project v.1.2](https://www.vulnhub.com/entry/owasp-broken-web-applications-project-12,46/) is available via [@vulnhub](https://twitter.com/vulnhub)   


## Info

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Address</th>
      <th>Machine</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>192.168.30.128</code></p>
      </td>
      <td>
            <p>Attacking Machine</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>192.168.30.130</code></p>
      </td>
      <td>
            <p>Target Machine</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

## OWASP WebGoat - General

### HTTP Splitting (AKA CRLF Attack)

Enter the following html into Burp repeater:

{% highlight html %}
en
Content-Length: 0

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 31
<html>Hacked by HighOn.Coffee</html>  
{% endhighlight %}


## Basic Burp Suite Setup

### Setup FoxyProxy for Burp Suite

![Setup FoxyProxy Burp
Suite](/img/blog/owasp-web-app-testing/setup-burp-foxyproxy.gif)

## OWASP WebGoat Injection Flaws

### Command Injection

1. Enable Intercept

![Burp intercept on](/img/blog/owasp-web-app-testing/intercept-on-burp.gif)

2. Set FoxyProxy to use Burp Suite

![Burp FoxyProxy](/img/blog/owasp-web-app-testing/burp-foxyproxy.gif)

3. Select the drop down form and select the manual e.g. AccessControlMatrix.help click View
4. Go to burp proxy > intercept
5. click the Params tab
6. double click on the Value AccessControlMatrix.help and tack the command on the end: <code>example AccessControlMatrix.help" & ifconfig" - hit return</code>
7. Click forward in burp until the page renders, you should see the output from ifconfig on the page.

### Numeric SQL Injection

Setup burp the same as Command Injection

1. Go to burp > intercept
2. Go to params tab
3. Change the Value to <code>101 OR 1=1</code>
4. Click forward in burp until the page renders, you should see the output from other users on the page.

### Log Spoofing

1. Open burp > decoder  
2. test

3.
{% highlight html %}
Login succeeded for username: <code> admin<script>alert("your session has been stolen: " + document.cookie);</script> </code>
{% endhighlight %}
4. Select Encode as URL
5. Paste output into form on webpage

### XPATH Injection

#### WTF is XPATH Injection ?

In basic terms, XPATH injection is similar to SQL injection but for XML, more information can be found on the OWASP XPATH page.

1. <code>Mike' or 1=1 or 'a'='a</code>
2. test123

### String SQL Injection

1. <code>Smith' or 'a'='a</code>
2. Click Go

<div class="note tip">
  <h5>Use FoxyProxy to save your Sanity</h5>
  <p>Setup FoxyProxy in FireFox to allow for fast proxy switching or setup Burp Suite scope correctly to only intercept in scope machines.</p>
</div>

### Setup Broswer to use Burp Suite Proxy
