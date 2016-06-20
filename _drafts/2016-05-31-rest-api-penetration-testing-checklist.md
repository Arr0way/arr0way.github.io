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

<ul>
<li><input type="checkbox"> Request 2 Test Accounts for each access level</li>
<li><input type="checkbox"> Request API Documentation</li>
<li><input type="checkbox"> Request Developer Examples if not already present in API documentation</li>
</ul>

## 2 Explore Visible Content - Map the API

Typically REST API's don't have WSDL files, in order to map the API content typically you need to explore the API using the documentation / developer examples.

#### 2.1.0 Build list of API content

Review the supplied API documentation and developer supplied API examples and build a list.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Request API Documentation</li>
</ul>

#### 2.1.2 Map Content in SoapUI

Use SoapUI to map the resources for the API, add each function and parameter and send a request for each.

#### 2.1.11 Configure SoapUI to use Burp Proxy

Set the application default proxy to use Burp <code>127.0.0.1:8080</code>

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Configure SoapUI to use Burp Proxy</li>
</ul>

#### 2.1.3 Setup Browser and Burp

Disable all your browsers security features and configure Burp Proxy, ideally using FoxyProxy.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Verified all security features are disabled in your browsers</li>
</ul>

#### 2.1.4 Examine requests from SoapUI

When sending requests to the REST API from SoapUI examine the requests via Burp intercept.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Examined all requests from SoapUI in Burp</li>
</ul>

#### 2.1.5 Ensure all API resources are present in Burp Sitemap  

Check in Burp that all API resources appear within the Burp's site map.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Check all API resources are mapped in Burp Site map</li>
</ul>

### 2.2 Check Public Resources

#### 2.2.1 Use Search Engines to Disclose domain info

Use Google / DuckDuckGo to search for domain related information, use <code>site:api.domain.com</code> and <code>link:api.domain.com</code>.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Performed search engine recon against the domain</li>
</ul>

#### 2.2.2 Perform a search against discovered Names or Email addresses

Perform a google search against any discovered email addresses, in an effort to disclose information about the API (forums, news group, stack overflow etc).

Tools for Email Discovery:

- Burp
- CeWL
- Curl

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Perform search engine recon against any discovered names / email addresses</li>
</ul>

### 2.3 Discover Hidden Content

#### 2.3.1 Fingerprinting: Identify the API Technology in Use

Use Wappalyzer to identify the software running on the target API.

Other useful tools:

- Analyze default banner using netcat HEAD http method
- Use Whatweb from Kali
- Curl
- Nmap
- Nikto

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Document the Web Framework</li>
<li><input type="checkbox"> Document Programing Languages</li>
<li><input type="checkbox"> Document the Web Server software</li>
<li><input type="checkbox"> Document target Operating System</li>
<li><input type="checkbox"> Document Cookie types</li>
<li><input type="checkbox"> Document misc technology in use</li>
</ul>

#### 2.3.2 Obtain lists of common file and directory names and Extensions

Using the information discovered in section 2.3.1, obtain lists of likely files, directory names or extensions e.g. <code>.php</code>.

##### Checklist:

Build lists of the following:

<ul class="checkbox">
<li><input type="checkbox"> Likely File Extensions</li>
<li><input type="checkbox"> Likely Framework Files</li>
<li><input type="checkbox"> Likely OS / Web Server Files</li>
<li><input type="checkbox"> Likely Function Parameters (based on other functions / research and past experience)</li>
</ul>

Useful resources:

- [FuzzDB](https://github.com/fuzzdb-project/fuzzdb)
- [SecLists](https://github.com/fuzzdb-project/fuzzdb)

#### 2.3.3 Forced Browsing / Fuzzing Burp Intruder  

Using the targeted lists prepared in section 2.3.2 fuzz / brute force the API using Burp intruders / OWASP Dirbuster.   

#### 2.3.4 Find Functions / Parameters

Using the API documentation, gain an understanding of the typical input functions / parameters. Use the information you gained from learning how the API functions to search for hidden functions, parameters or scripts and apply this technique recursively to any discovered content.

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Use Burp intruder to fuzz function parameters</li>
<li><input type="checkbox"> Establish what happens when the API functions error</li>
<li><input type="checkbox"> Sort results by response / size in Burp Intruder</li>
</ul>

### 2.4 - Discover Default Content

#### 2.4.1 Run Nikto against the API

Run Nikto against the API to discover any default content.

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Verify any identified findings manually to rule out false positives</li>
<li><input type="checkbox"> Run Nikto against vhosts + the server IP address and compare findings</li>
<li><input type="checkbox"> Run Nikto using various User-Agents and compare findings</li>
<li><input type="checkbox"> Analyse findings, including common files <code>/feed.xml</code> and <code>/robots.txt</code> etc</li>
</ul>

#### 2.5 Test for Debug Parameters

Using the applications standard input format add some common debug methods (ASP / .NET).

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Check for debug parameters</li>
</ul>

### 3 Analyse the Application

#### 3.1.0 Identify API Primary Function

What core function does the API provide ?

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Document the primary role of the API</li>
</ul>

#### 3.1.1 Identify Role of Each Function & Parameter

Build a list of all functions & parameters and list their roles.

e.g. <code>/user/reset</code> - allows for user password resets.

#### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Document the role of each function</li>
<li><input type="checkbox"> Document the role of each functions parameters</li>
</ul>

#### 3.1.2 Identify Core Security Mechanisms

Understand the core security mechanisms used by the API.

#### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Document how the API handles Authentication</li>
<li><input type="checkbox"> Document how the API handles Session Management</li>
<li><input type="checkbox"> Document how the API handles Access Control</li>
<li><input type="checkbox"> Document how the API handles User Registration</li>
<li><input type="checkbox"> Document how the API handles Password Resets</li>
</ul>

### 3.2 Identify Data Entry Points

#### 3.2.1 Identify All the Different Entry Points

Assess API functions / parameters or other discovered content for entry points.

#### Checklist:

Consider / check for the following entry points:

<ul class="checkbox">
<li><input type="checkbox"> API Login Functions</li>
<li><input type="checkbox"> API Search Functions</li>
<li><input type="checkbox"> API Comment Functions</li>
<li><input type="checkbox"> API Import Functions</li>
<li><input type="checkbox"> API Contact Form Functions</li>
<li><input type="checkbox"> Cookies</li>
<li><input type="checkbox"> Assess API GET / POST requests</li>
<li><input type="checkbox"> API Hidden Function Parameters</li>
<li><input type="checkbox"> API File Upload Functions</li>
</ul>

#### 3.2.2 Review Content Mapping Findings

Go over the previously discovered content mapping finding for a potential entry point.
#### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Re-assess Content Mapping Findings </li>
</ul>

#### 3.3.3 Google :inurl Discover Hidden Function Parameters

Google using the <code>:inurl</code> qualifier to search for API functions / parameters to find other sites using the same API. Use this technique to find hidden function parameters.

e.g. Google for: <code>inurl:/user/activate/?token=foobar</code>

##### Checklist:

<ul class="checkbox">
<li><input type="checkbox"> Google :inurl API Functions / Parameters</li>
</ul>

### 3.4 Map the Attack Surface

#### 3.4.1 Understand the API / Web Server Structure

Use the previously discovered information to ascertain the internal structure of the target API / Web Server / Operating System.

#### 3.4.2 Download the Target Framework / Read Documentation

If the API Framework / Web Application is available download it or and view it's documentation.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Study target API technology</li>
</ul>

#### 3.4.3 Identify Common Vulnerabilities

Assess each previously discovered entry point for common vulnerabilities, e.g. an API file upload function might be vulnerable to shell upload of path traversal.

##### Checklist:
<ul class="checkbox">
<li><input type="checkbox"> Build list of Common Vulnerabilities for each Entry Point </li>
</ul>

#### 3.4.4 Plan the Assessment

Using the discovered entry points and associated vulnerabilities, plan your attack accordingly. Be mindful of the amount of time you have for the assessment and allocate the time you spend on each section accordingly, spending more time on areas that are more likely to be vulnerable.

### 4.0 Authentication Mechanism

#### 4.1.1 Understand the Authentication Mechanism

Understand how the authentication function works:

##### Checklist:

Understand how the API performs the following authentication related functionality:

<ul class="checkbox">
<li><input type="checkbox"> Login </li>
<li><input type="checkbox"> Password reset </li>
<li><input type="checkbox"> Account Registration </li>
<li><input type="checkbox"> Account Recovery </li>
</ul>

### 4.2 Test Password Quality

#### 4.2.1 Identify Password Policy

##### Checklist:

Test the documented password policy is correctly being enforced, if there is no documented policy, attempt to enter weak passwords and see if the application is enforcing a secure password policy.

<ul class="checkbox">
<li><input type="checkbox">Test Password Policy Rules.</li>
</ul>

#### 4.2.2 Test for Incomplete Validation of Credentials

Set a strong password, using special characters, uppercase, lowercase and numbers. Attempt to authenticate using various techniques to ensure the applications is not incorrectly filtering / trimming password length.

##### Checklist:

Perform the following password checks after setting a secure password:

<ul class="checkbox">
<li><input type="checkbox"> Remove the last character</li>
<li><input type="checkbox"> Change the case of characters</li>
<li><input type="checkbox"> Remove special characters</li>
<li><input type="checkbox"> Remove numbers </li>
<li><input type="checkbox"> Set a max length password and remove last character </li>
</ul>  

Tip: Use Burp Intruder to speed up this process.

#### 4.2.3 Identify any Built in Accounts

Attempt to identify any potential built in accounts with common / weak credentials, e.g. a test or admin account with weak credentials.

- Use Burp Intruder and a bruteforce credentials list

<ul class="checkbox">
<li><input type="checkbox">Attempt to identify any default accounts with weak credentials</li>
</ul>

### 4.3 Test Credentials are Transmitted using Encryption in Transit

Test credentials are transmitted using encryption in transit.

<ul class="checkbox">
<li><input type="checkbox"> Test if you are able to authenticate using HTTP transmission</li>
<li><input type="checkbox"> Ensure HTTPS is used for transmission of credentials</li>
</ul>

### 4.4 Username Enumeration

#### 4.4.1 Identify All Functions / Parameters where Usernames are Entered

Identify all entry points where usernames are entered, enumerate each form for a different response when a valid or invalid username or password is entered.

<ul class="checkbox">
<li><input type="checkbox"> Assess the Web App response with a valid username and bad password</li>
<li><input type="checkbox"> Assess the Web App response when an invalid username is entered</li>
<li><input type="checkbox"> If an email address is used as the username, try incorrect@valid-domain.com</li>
<li><input type="checkbox"> If an email address is used as the username, try correct@invalid-domain.com</li>
<li><input type="checkbox"> Test account creation forms for a specific username format e.g. foobar@valid-domain.com</li>
</ul>

#### 4.5 Test Resilience to Password Guessing

Test the API has correct controls in place to prevent password guessing (brute forcing account passwords).

##### 4.5.1 Identify all Functions / Parameters Where Passwords are Entered

Identify all entry points where passwords are entered, enumerate each form for a different response when a valid or invalid username or password is entered.

##### 4.5.2 Test Account Lockout Policy

At each previously identified password entry poiny, enter 10 or so failed login attempts for an account you control. If the Account allows a login after 10 failed account attempts in quick succession it's like an account lockout policy is not in use.

<b>Checklist:</b>

<ul class="checkbox">
<li><input type="checkbox"> Identify / Build a list of all Password Entry point functions / parameters for the API</li>
<li><input type="checkbox"> Test created list for Account Lockout Policy</li>
</ul>  

#### 4.6 Test Account Recovery

Test password recovery / forgot your password functions / parameters.

##### 4.6.1 Test the Account Recovery Process Works

Run through the account recovery process and test that it works as expected, check for insecure mechanisms / logic flaws such as the API sending out passwords via plain text email.

##### 4.6.2 Enumerate Secret Questions

If the password reset feature asks for a secret question then use this feature to see if username enumeration is possible. If so, try to identify any secret questions that can easily be guessed or brute forced with a list.

##### 4.6.3 Secret Question Lockout

After entering a number of incorrect challenge password attempts for the password reset function the API should perform an account lockout.

##### 4.6.4 Password Hint Testing

If the application uses a password hint instead of a secret question, perform steps 4.6.2 and 4.6.3 against the password hint function.

##### 4.6.5 Password Reset Email

Using accounts you control perform a password reset for several accounts, assess the emails for any guessable patterns with the reset function such a predictable URL one-time-only token.

Additionally test to see if it's possible to attempt to manipulate the email where the password reset email is sent.

#### 4.7 Remember Me Function Testing

If the REST API has a function / parameter for "remember me" enable it an exam any changes with the authentication process in Burp.

<b>Checklist:</b>

<ul class="checkbox">
<li><input type="checkbox"> Check Cookies when Remember Me is activated for anything that idetnifies the user</li>
<li><input type="checkbox"> Check for similarities between users that have remember me enabled, even if the cookie data is encoded / hashed - look for patterns or predicable tokens </li>
</ul>  

#### 4.8 Test Username Uniqueness

##### 4.8.1 Create Duplicate Accounts, Different Passwords

Attempt to register two user accounts with different passwords, if the API blocks registration of the second account you can use this for username enumeration.

If the API blocks the creation of the account, prefix / postfix the account with white space.

##### 4.8.2 Login using the Duplicate Account

Login to the API using the duplicate account, what behavior is observed:

- Do you have access to the other users account ?
- Can you use this for privilege escalation ?
- What happens if the password is set the same on both sets of colliding credentials ?
- Are any errors generated when performing the above ?


If an error is returned when a duplicate account name is entered with the same password, it's likely this can be leveraged to guess a users password.

1. Create a duplicate account
2. Brute force the passwords until  
