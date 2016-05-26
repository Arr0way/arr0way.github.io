---
layout: blog_item
title:  "Web Application Penetration Testing Methodology"
date:   2016-05-25 11:29:10
author: Arr0way
description: 'A technical Web Application Penetration Testing Methodology'
categories: [Web App Penetration Testing]
tags:
- 'Web Application Penetration Testing'
---

* list element with functor item
{:toc}

## 1.1 - Explore Visible Content

### 1.1.1 - Spider Website

Configure Burp proxy and passively spider the website.

### 1.1.2 - Monitor HTTP / HTML Content  

Use Burp to monitor HTTP / HTML content being processed by the browser.

### 1.1.3 - Browse The Entire site

Browse the **entire site** in the normal way, clicking **every link / URL** and submitting **every form** and proceed through **every multi step function** until completion.

- Browse the site with JavaScript enabled and disabled
- Visit every page
- Visit every link / URL
- Submit every form
- Complete all multi-step-functions

### 1.1.4 - Authenticate - Login To The Application

If the web application supports authentication and you have access to or can create an account login and browse all (potentially protected) available content using the process outlined in 1.1.3

### 1.1.5 - Analyse Content

Browse the site and monitor requests and responses, gain an understanding of the types of data being submitted and how the client controls the server side content.

### 1.1.6 - Review the sitemap

Use Burp plugin called **Site Map Fetcher** to view all links in the sitemap, manually review **all discovered sitemap content** reviewing the requests and responses using Burp.

### 1.1.7 - Spider the site using Burp

Correctly configure in scope URL's, excluding any problematic / dangerous URL's and use Burp to spider the web application.

### 1.1.8 - Use Burp Scanner Passive / Active

Depending on what was agreed with client, conduct either a passive and or active scan using Burp.  

** NOTE: ** Burp active scanner is particularly aggressive, sending a large amount of requests to the web application. In some cases this can DoS / knock over the application, server or backend database, care should be taken to test at a time the web application is not in use and / or has reduced load.

Take additional care when running active scanner against production systems.

## 1.2 - Use Public Resources

### 1.2.1 - User Search Engines and Wayback machine

Perform recon using Wayback machine + Google and other search engines to identify what info they have stored for your target application.

### 1.2.2 - Advanced search

Use Google switches such as <code>site:</code> and <code>link:</code> to perform advanced searches. Pages / content that has been removed from the web application could still exist in Google cache, the discovered cached pages may link to resources that have not yet been removed.

### 1.2.3 - Search Application Author Names / Email Addresses

Perform search for any discovered names / email addresses etc. Search for postings on forums / stack exchange / news groups / google groups etc.

### 1.2.4 - Review Published WSDL Files

Review any WDSL (Web Service Description Language) files to generate a list of function names and parameter values potentially used in the application.

## 1.3 Discover Hidden Content

### 1.3.1 - Anaylse How the Web Application Handles Nonexistent items

Make manual requests for valid and invalid resources and compare the output, establish a way to identify if content does not exist.

### 1.3.2 - List of Logical File Names / Directories

Look at the applications resource naming convention make logical guesses at other unlinked resource names. For example if a page called <code>AddFile.php</code> exists it's likely that <code>DelFile.php</code> exists or perhaps <code>DeleteFile.php</code> or <code>RemoveFile.php</code> exists. If the application uses open source or off the shelf code perhaps common application directory lists already exist. Try googling existing application resource names or error responses to identify backend.

### 1.3.3 - Review the Application Source

Look at the applications source code to reveal clues about hidden server-side content. Review code comments and hidden / disabled form values.

### 1.3.4 - Forced Browsing / Dir Brute Forcing

Build / use existing large lists of potential application directories and / or resources. Use tools such as **Brup Intruder** or **OWASP DirBuster** to enumerate the application for hidden content.

Time should be spent on section **1.3.2** building out lists of potential file names for the given application. If the application is open source, specific tools can be leverages that may contain path locations specific to the application.

| Web Application  | Tool  |
|---|---|
| Wordpress  | wpscan  |  
| Generic CMS  | cmsmap  |   
|  Joomla | joomscan  |
| Drupal  | droopscan  |

### 1.3.5 - Recursive Forced Browsing

After a non recursive first pass, perform recursive forced browsing on directories that may contain resources.

## 1.4 - Discover Default Content

### 1.4.1 - Run Nikto to Discover Default Content

Run Nikto against the target application to discover default directories or configurations.

### 1.4.2 - Verify Nikto False Positives

Manually verify interesting Nikto findings to eliminate false positives

### 1.4.3 - Nikto both the IP + Hostname

Make requests to the IP address of the server e.g. <code>http://XXX.XXX.XXX.XXX</code>, if the responses or served content differs from that of the hostname then run a Nikto scan against the IP address.

### 1.4.4 - Multiple user agents

Make multiple requests to the servers web root using a range of <code>User-Agent</code> headers.

Tools:

**Burp Intruder** - use User agents payload list within Intruder.
** Firefox Plugin, [User Agent Overrider](https://addons.mozilla.org/en-US/firefox/addon/user-agent-overrider/) **

## 1.5 - Enumerate Identified-Specified Functions

### 1.5.1 - Identify Application Functions that are Passed an Identifier
 Identify any instances where specific application functions are accessed by passing an identifier to an identifier of the function in a request parameter. Example: <code>/admin.jsp?action=editUser</code> or <code>/main.php?func=A43</code>

### 1.5.2 - Content Discovery for Individual Functions

Use the method described in step [1.3](#13-discover-hidden-content) to fuzz / brute force the mechanism being used to access individual functions.

Identify the applications response when an invalid function is specified.

Use Burp intruder with a list of likely functions for the application, analyse the size of the response when an invalid function is specified. Sort the output by size, this should help identify successful function names.

### 1.5.3 - Create a Content Map of the Web Application

Create a content map of the application to include within the report.

## 1.6 - Test for Debug Parameters

### 1.6.1 Locate Pages or Functions with Hidden Debug Parameters are Likely

Choose one or more application pages or functions where hidden debug Parameters such as  <code>debug=true</code> may be implemented. These are likely to appear in key functionality such as login, search and file upload or download.

### 1.6.2 - Enumerate Common Debug Parameter Names

Use a list of common debug Parameter names such as: <code>debug, test, hide, source</code> and common values such as <code>true, yes, on , 1</code>. Iterate through all permutations of these, submitting each name / value pair to each targeted function. For POST requests, supply the parameter in both the URL query string and the request body.

Use the **cluster Bomb** attack type in Burp Intruder to combine all permutations of two payload lists.

### 1.6.3 Review Application Responses

View the applications responses for any anomalies that may indicate that the added parameter has had an effect on the application's processing.

## 2 - Analyse the Application

## 2.1 Identify Functionality

### 2.1.1 Identify Core Application Functionality

Identify the core functionality the application was designed for and the action that each function is designed to perform when used as intended.

### 2.1.2 - Identify Applications Core Security Mechanisms

Identify the core security mechanisms employed by the application and understand how they work.

Understand the following application mechanisms:

- Understand the authentication mechanisms
- Understand session management
- Understand access control mechanisms

Understand the supporting functions for the above mechanisms:

- User registration
- Account recovery
- Password reset

### 2.1.3 - Identify Peripheral Functions

Identify the more peripheral functions and behaviour, such as:

- Redirects
- Off-site links
- Error Messages
- Administrative and logging functions

### 2.1.4 - Identify Additional Functionality

Identify any additional functionality unrelated to GUI appearance, parameter naming or navigation mechanism used elsewhere in the application, and single it out for in-depth testing.

## 2.2 - Identify Data Entry Points

### 2.2.1 - Identify all Different Data Entry Points

Identify entry points that exist for introducing user input into the applications processing.

- URLs
- Query string parameters
- Post data
- Cookies
- Other HTTP headers processed by the application (User Agent, etc)

### 2.2.2 - Examine any Customised Data Transmission

Examine any customised data transmission or encoding mechanisms used by the application, such as non standard query string format. Understand wheter the data being submitted encapsulates partameter names and values, or whether an alternative means of representation is being used.

### 2.2.3 - Identify any out-of-band channels

Identify any out-of-band channels where user-controllable or third-party data is being introduced into the application's processing. An example is a web mail application that processes and renders messages received via SMTP.   

## 2.2 - Idenitfy the Technologies Used

### 2.3.1 - Identify Different Client Side Technologies

Identify each of the client side technologies used, such as:


- Forms
- Scripts
- Cookies
- Java applets
- ActiveX controls
- Flash objects

### 2.3.2 - Identify Server Side Technologies

As far possible identify server side technologies being used, such as scripting languages, application platforms and interaction with backend components such as databases and e-mail systems.

### 2.3.3 - Check HTTP Header Responses

Check the HTTP Server header returned in application responses and also check for any other software identifiers contained within custom HTTP headers or HTML source code comments. Note that in some cases, different areas of the application are handled by different back-end components, so different banners may be recieved.

###  2.3.4 Run httprint tool to finger print the web server  

httprint is out of date.

### 2.3.5
