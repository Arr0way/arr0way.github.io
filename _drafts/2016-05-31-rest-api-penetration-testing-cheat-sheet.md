---
layout: blog_item
title:  "REST API Penetration Testing Cheat Sheet"
date:   2016-05-31 11:37:10
author: Arr0way
description: 'A technical Web Application Penetration Testing Methodology, laid out as a cheat sheet'
categories: [Web App Penetration Testing]
tags:
- 'Web Application Penetration Testing'
---

* list element with functor item
{:toc}

A technical methodology for REST API penetration testing, think of this as a high level overview of how I approach REST API pen testing laid out as a cheat sheet. Each step wont have massive detail - it will be a list of tools and how I approach them.

## 1 Scoping

### 1.1.0 What you need from the Client

- Request 2 test accounts (if testing ACL's ask for 2 test accounts per access level)
- Request API Documentation
- Request developer examples if not covered by provided documentation

## 2 Explore Visible Content

Typically REST API's don't have WSDL files, in order to map the API content typically you review the API supplied documentation.

#### 2.1.0 Build list of API content

Review the supplied API documentation and developer supplied API examples and build a list.

#### 2.1.2 Map Content in SoapUI

Use SoapUI to map the resources for the API, add each function and parameter and send a request for each.

#### 2.1.11 Configure SoapUI to use Burp Proxy

Set the application default proxy to use Burp <code>127.0.0.1:8080</code>

#### 2.1.3 Setup Browser and Burp

Disable all your browsers security features and configure Burp Proxy, ideally using FoxyProxy.

#### 2.1.4 Examine requests from SoapUI

When sending requests to the REST API from SoapUI examine the requests via Burp intercept.

#### 2.1.5 Ensure all API resources are present in Burp Sitemap  

Check in Burp that all API resources appear within the Burp's site map.

### 2.2 Check Public Resources

#### 2.2.1 Use Search Engines to Disclose domain info

Use Google / DuckDuckGo to search for domain related information, use <code>site:api.domain.com</code> and <code>link:api.domain.com</code>.  

#### 2.2.2 Use Way Back Machine   

If 2.2.1 discloses a web application using the API attempt to use the way back machine to expose any undocumented functions / parameters.

#### 2.2.3 Perform a search against discovered Names or Email addresses

Perform a google search against any discovered email addresses, in an effort to disclose information about the API (forums, news group, stack overflow etc).

Tools for Email Discovery:

- Burp
- CeWL
- Curl

### 2.3 Discover Hidden Content

#### 2.3.1 Identify the API Technology in Use

Use Wappalyzer to identify the software running on the target API.

Make note of the following:

- Web Frameworks in use
- Web Server in use
- Programming Languages in use

#### 2.3.2 Obtain lists of common file and directory names and Extensions

Using the information discovered in section 2.3.1, obtain lists of likely files, directory names or extensions e.g. <code>.php</code>.

Build lists of the following:

- Likely File Extensions
- Likely Framework files
- Likely OS / Web Server files
- Likely Function Parameters (based on other functions / research and past experience)

Useful resources:

- [FuzzDB](https://github.com/fuzzdb-project/fuzzdb)
- [SecLists](https://github.com/fuzzdb-project/fuzzdb)

#### 2.3.3 Forced Browsing / Fuzzing Burp Intruder  

Using the targeted lists prepared in section 2.3.2 fuzz / brute force the API using Burp intruders / OWASP Dirbuster.   

#### 2.3.4 Find Functions / Parameters

Using the API documentation, gain an understanding of the typical input functions / parameters. Use the information you gained from learning how the API functions to search for hidden functions, parameters or scripts and apply this technique recursively to any discovered content.

- Use Burp intruder to fuzz function parameters.  
- Establish what happens when the API errors
- Sort results by size / error code in Burp

### 2.4 - Discover Default Content

#### 2.4.1 Run Nikto against the API 

Run Nikto against the API to discover any default content.

- Verify any identified findings manually to rule out false positives
- Run Nikto against vhosts + the server IP address and compare findings
- Run Nikto using various User-Agents and compare findings

### 2.5 Test for Debug Parameters

Using the applications standard input format add some common
