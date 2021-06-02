---
layout: blog_item
title: "Password Reset Testing Cheat Sheet"
date:   2021-06-01 03:29:10
author: Arr0way
description: 'Password reset function assessment cheat sheet, several techniques for assessing password reset functions on web apps and API.'
categories: [cheat-sheet]
tags:
- 'Authentication'
- 'Web Application Penetration Testing'
---

* list element with functor item
{:toc}

The following document outlines some key techniques used when assessing password reset function on modern web application and API's, a useful resource for bug bounty hunters and penetration testers when performing password reset security testing.  

<!--more-->


## Header Poisoning 

### Host: Header Injection Password Reset URL

The following headers can be modified in an attempt to modify the password reset token URL to an attacker controlled domain. Potentially allowing an attacker to obtain the reset token when a user clicks the link to perform a password reset. 

#### Potential Vulnerable Headers 

##### Host Header Password Reset

Replace the Host: header with a server you control.

{% highlight bash %} 
Host: attackers-domain.com
{% endhighlight %}

##### Double Host Header

Often when you modify the Host header the application, or WAF fails to the process the request, a technique which can potentially bypass this filter if to simply add a second Host header to the request. 

{% highlight bash %}
Host: target.com
Host: attackers-domain.com
{% endhighlight %}

##### X-Forwarded-Host Header

Add an attacker controlled domain in the X-forwarded-Host header directive.

{% highlight bash %}
Host: target.com 
X-Forwarded-Host: attackers-domain.com
{% endhighlight %} 


#### Confirm Vulnerable 

1. Perform a password reset normally, and modify the host header with a different url
2. Check the email sent by the application has the test url, not the applications url

#### PoC 

1. Replace vulnerable header with the attacker controlled domain 
2. Send the request
3. Wait for the victim to click reset link (or show PoC to a second account)
4. Exatract the token from the webserver logs you control
5. Use the extracted token to take over the user account

## Password Reset Token Filter Bypass

If the app is built on ruby, try adding a .json extension to the end of the password reset URL. In certain circumstances ACL bugs may exist, adding the extension could potentially bypass any additional layers of protection the application has in in place.

1. Add a .json to the reset token
2. Observe the applications response to see if the 

## Email Parameter Manipulation

### Password Change Functions

#### Intercept & Change the Email

1. Interecept the request, and replace the email address to one that you control 

### Password Reset Functons

#### Add Second Email 

Vulnerable applications can be manipulated to send password reset codes to multiple email addresses.  

1. Interecept the request, and add a second email address to the request

{% highlight bash %}
&
%20
|
%0a%0dcc:
%0a%0dbcc:
{% endhighlight %}
 
Modify the parameter matching the applications format, example:

{% highlight bash %} 
email="victim@mail.tld",email="attacker@mail.tld"
{% endhighlight %}

<div class="note tip">
  <h5>Match the API Request Schema</h5>
  <p>Match the application / API's request formatting when adding the additional email address.</p>
</div>


## Password Token Referral Header Leak 

Vulnerable applications leak the password reset URL via the referal header. 

Assess the target using an intercepting proxy to identify if the referral header leaks the token through the referral header. 

## Check List 

- Host Header Injection Password Reset Function 
- Double Host Header Injection Password Reset Function
- X-Forwarded-Host Header Injection Password Reset Function
- Email Parameter Manipulation *add attacker controller second email address*


Enjoy.

