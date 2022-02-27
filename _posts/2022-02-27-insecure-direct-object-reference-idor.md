---
layout: blog_item
title: "Insecure Direct Object Reference (IDOR): Definition, Examples & How to Find"
date:   2022-02-27 01:29:10
author: Arr0way
description: 'What is a Insecure Direct Object Reference (IDOR) vulnerability, examples and how to find IDOR.'
categories: [Web App Security]
tags:

- 'Web App Security'
---


## What is an Insecure Direct Object Reference (IDOR) Vulnerability? 

What is a Insecure Direct Object Reference (IDOR) Vulnerability? In the most basic form an IDOR is an object referenced within a web appliation without the correct controls in place to prevent an unauthorised user directly access, either via enumeration or guessing / predicting the object. IDOR vulnerabilties typically occur when the access control mechanism uses a user-controlled parameter value, that is used to access functionality or reasources directly. Typically this uses a numeric or predictible parameter value, that an attacker or malicious user could predict, brute force and then manipulate to gain access to data and/or functionality that was not intended.

<div class="note tip">
  <h5>How To Pronounce IDOR</h5>
  <p>IDOR is typically pronounced <i>eye-door</i> - this is arguably the most important piece of information in the whole document :)</p>
</div>

* list element with functor item
{:toc}


## Insecure Direct Object Reference (IDOR) Examples

<p>The following documents some IDOR examples, where the access control mechanism is vulnerable due to a user-controlled parameter value, that is used to access functionality or reasources directly. Typically a numeric or predictible parameter value, that an attacker or malicious user could manipulate.</p>

## IDOR Example: Direct Database Reference

A typical numeric IDOR vulnerable function would look like: 

{% highlight bash %}
foo.com/profile/user_id=7747
{% endhighlight %} 

If the ```user_id``` parameter is vulnerable to IDOR an attacker could simply modify the numeric value and access another users profile. If successful an attacker could gain access to a user acverticalcount profile and potentially perform horizontal, or vertical privilege escalation against the vulnerable application. 

## IDOR Example: File Name 

Another typical example of an IDOR vulnerability would be file names with a predictable value that could be bruteforced, guessed or predicted if sequential, such as: 


{% highlight bash %}
foo.com/download/12432-secure-document.pdf 
{% endhighlight %}

If the file name is vulnerable to IDOR an attacker could simply predict or bruteforce the numeric value and access another users file. If successful an attacker could potentially gain access to the document. 

## How To Find IDOR Vulnerabilties

<p>This document walks through some potential techniques on how to find IDOR vulnerabilities witin vulnerable web applications. Due to the manual process of building and implimenting access control systems, mistakes could be made (human error). Unfortunately, identifying IDOR vulnerabilities is typically best done manually.</p>

## IDOR Entry Points

Assess the application for predictable parameters or URL's, some food for thought: 

1. Profile URL's or ID's
2. Password reset functions (great for privilege escalation)
3. Numeric parameters
4. Predictable parameters


{% highlight bash %}
foo.com/profile/user_id=7747
{% endhighlight %} 


If the ```user_id``` parameter is vulnerable to IDOR an attacker could simply modify the numeric value and access another users profile. If successful an attacker could gain access to a user acverticalcount profile and potentially perform horizontal, or vertical privilege escalation against the vulnerable application. 

## IDOR Example: File Name 

Another typical example of an IDOR vulnerability would be file names with a predictable value that could be bruteforced, guessed or predicted if sequential, such as: 

{% highlight bash %}
foo.com/download/12432-secure-document.pdf
{% endhighlight %}

If the file name is vulnerable to IDOR an attacker could simply predict or bruteforce the numeric value and access another users file. If successful an attacker could potentially gain access to the document. 

## Finding Access Control Bug (IDOR) 

You could argue that strictly speaking this is not an IDOR bug, however (call it what you want to call it), either way it is still an issue.

1. Using BurpSuite or another similar tool, browse the application as a user or ideally a privileged user account 
2. Authenticate as another user account, or an account with lower priveleges - and obtain a session identifier (token/cookie) 
3. Use the session identifier obtained during step 2 against the URL's from step 1 

<div class="note tip">
  <h5>Web Server Response Codes Lie</h5>
  <p>Do not depend solely on response codes, many web applications respond with incorrect response codes.</p>
</div>

## IDOR vs Forced Browsing: What's the Difference? 

Forced browsing and IDOR vulnerabilties are very similar access control vulnerabilities. The main thing that seperates the vulnerabilties is the method of discovery is typically bruteforcing URL's for forced browsing e.g. with a large word list and IDOR detection is typically discovered by brute forcing predicatable parameters.  
