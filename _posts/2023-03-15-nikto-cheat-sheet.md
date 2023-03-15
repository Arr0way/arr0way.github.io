---
layout: blog_item
title:  "Nikto Cheat Sheet"
date:   2023-03-15 14:37:10
author: Arr0way
description: 'Nikto cheat sheet - Flags and Nikto Example Commands'
categories: [cheat-sheet]
tags:
- Web
- Tools
---

## What is Nikto

Nikto is an open-source web server scanner that performs comprehensive tests to identify potentially dangerous files/programs, outdated versions of servers, server configuration items, and installed web servers and software. It also supports LibWhisker's anti-IDS methods to avoid detection. While not every check is a security issue, most are, and there are also info-only checks and checks for unknown items.

## Nikto2 Features

- SSL Support (Unix with OpenSSL or maybe Windows with ActiveState's Perl/NetSSL)
- Full HTTP proxy support
- Checks for outdated server components
- Save reports in plain text, XML, HTML, NBE or CSV
- Template engine to easily customize reports
- Scan multiple ports on a server, or multiple servers via input file (including nmap output)
- LibWhisker's IDS encoding techniques
- Easily updated via command line
- Identifies installed software via headers, favicons and files
- Host authentication with Basic and NTLM
- Subdomain guessing
- Apache and cgiwrap username enumeration
- Mutation techniques to "fish" for content on web servers
- Scan tuning to include or exclude entire classes of vulnerability checks
- Guess credentials for authorization realms (including many default id/pw combos)
- Authorization guessing handles any directory, not just the root directory
- Enhanced false positive reduction via multiple methods: headers, page content, and content hashing
- Reports "unusual" headers seen
- Interactive status, pause and changes to verbosity settings
- Save full request/response for positive tests
- Replay saved positive requests
- Maximum execution time per target
- Auto-pause at a specified time
- Checks for common "parking" sites

## Nikto Installation 

```
git clone https://github.com/sullo/nikto
```

### Main script is in program

```
cd nikto/program
```

### Check out the 2.5.0 branch

```
git checkout nikto-2.5.0
```

### Run using the shebang interpreter

```
./nikto.pl -h http://www.foo.com

```
### Run using perl (if you forget to chmod)

```
perl nikto.pl -h http://www.foo.com
```


## Nikto Cheat Sheet 

<table>
  <tr>
    <th>Command</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com</code></p></td>
    <td><p>Scans the specified host</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com -Tuning 6</code></p></td>
    <td><p>Uses a specific scan tuning level</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com -port 8000</code></p></td>
    <td><p>Scans the specified port</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com -ssl</code></p></td>
    <td><p>Scans for SSL vulnerabilities</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com -Format html</code></p></td>
    <td><p>Formats output in HTML</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h http://foo.com -output out.txt</code></p></td>
    <td><p>Saves the output to a file</p></td>
  </tr>
</table>


## Nikto Command Flags Sheet

<table>
    <tr>
        <th>Option</th>
        <th>Value</th>
    </tr>
    <tr>
        <td><code>-ask+</code></td>
        <td><p>yes Ask about each (default)</p><p>no Don't ask, don't send</p><p>auto Don't ask, just send</p></td>
    </tr>
    <tr>
        <td><code>-Cgidirs+</code></td>
        <td><p>"none", "all", or values like "/cgi/ /cgi-a/"</p></td>
    </tr>
    <tr>
        <td><code>-config+</code></td>
        <td><p>Use this config file</p></td>
    </tr>
    <tr>
        <td><code>-Display+</code></td>
        <td><p>1 Show redirects</p><p>2 Show cookies received</p><p>3 Show all 200/OK responses</p><p>4 Show URLs which require authentication</p><p>D Debug output</p><p>E Display all HTTP errors</p><p>P Print progress to STDOUT</p><p>S Scrub output of IPs and hostnames</p><p>V Verbose output</p></td>
    </tr>
    <tr>
        <td><code>-dbcheck</code></td>
        <td><p>Check database and other key files for syntax errors</p></td>
    </tr>
    <tr>
        <td><code>-evasion+</code></td>
        <td><p>1 Random URI encoding (non-UTF8)</p><p>2 Directory self-reference (/./)</p><p>3 Premature URL ending</p><p>4 Prepend long random string</p><p>5 Fake parameter</p><p>6 TAB as request spacer</p><p>7 Change the case of the URL</p><p>8 Use Windows directory separator (\)</p><p>A Use a carriage return (0x0d) as a request spacer</p><p>B Use binary value 0x0b as a request spacer</p></td>
    </tr>
    <tr>
        <td><code>-Format+</code></td>
        <td><p>csv Comma-separated-value</p><p>htm HTML Format</p><p>msf+ Log to Metasploit</p><p>nbe Nessus NBE format</p><p>txt Plain text</p><p>xml XML Format</p><p>(if not specified the format will be taken from the file extension passed to -output)</p></td>
    </tr>
    <tr>
        <td><code>-Help</code></td>
        <td><p>Extended help information</p></td>
    </tr>
    <tr>
        <td><code>-host+</code></td>
        <td><p>Target host</p></td>
    </tr>
    <tr>
        <td><code>-IgnoreCode</code></td>
        <td><p>Ignore Codes--treat as negative responses</p></td>
    </tr>
    <tr>
        <td><code>-id+</code></td>
        <td><p>Host authentication to use, format is id:pass or id:pass:realm</p></td>
    </tr>
    <tr>
        <td><code>-key+</code></td>
        <td><p>Client certificate key file</p></td>
    </tr>
    <tr>
        <td><code>-list-plugins</code></td>
        <td><p>List all available plugins, perform no testing</p></td>
    </tr>
    <tr>
        <td><code>-maxtime+</code></td>
        <td><p>Maximum testing time per host</p></td>
    </tr>
    <tr>
        <td><code>-mutate+</code></td>
        <td><p>1 Test all files with all root directories</p><p>2 Guess for password file names</p><p>3 Enumerate user names via Apache (/~user type requests)</p><p>4 Enumerate user names via cgiwrap (/cgi-bin/cgiwrap/~user type requests)</p><p>5 Attempt to brute force sub-domain names, assume that the host name is the parent domain</p><p>6 Attempt to guess directory names from the supplied dictionary file</p></td>
    </tr>
    <tr>
        <td><code>-mutate-options</code></td>
        <td><p>Provide information for mutates</p></td>
    </tr>
    <tr>
        <td><code>-nointeractive</code></td>
        <td><p>Disables interactive features</p></td>
    </tr>
    <tr>
        <td><code>-nolookup</code></td>
        <td><p>Disables DNS lookups</p></td>
    </tr>
    <tr>
        <td><code>-nossl</code></td>
        <td><p>Disables the use of SSL</p></td>
    </tr>
    <tr>
        <td><code>-no404</code></td>
        <td><p>Disables nikto attempting to guess a 404 page</p></td>
    </tr>
    <tr>
        <td><code>-output+</code></td>
        <td><p>Write output to this file ('.' for auto-name)</p></td>
    </tr>
    <tr>
        <td><code>-Pause+</code></td>
        <td><p>Pause between tests (seconds, integer or float)</p></td>
    </tr>
    <tr>
        <td><code>-Plugins+</code></td>
        <td><p>List of plugins to run (default: ALL)</p></td>
    </tr>
    <tr>
        <td><code>-port+</code></td>
        <td><p>Port to use (default 80)</p></td>
    </tr>
    <tr>
        <td><code>-RSAcert+</code></td>
        <td><p>Client certificate file</p></td>
    </tr>
    <tr>
        <td><code>-root+</code></td>
        <td><p>Prepend root value to all requests, format is /directory</p></td>
    </tr>
    <tr>
        <td><code>-Save</code></td>
        <td><p>Save positive responses to this directory ('.' for auto-name)</p></td>
    </tr>
    <tr>
        <td><code>-ssl</code></td>
        <td><p>Force ssl mode on port</p></td>
    </tr>
    <tr>
        <td><code>-Tuning+</code></td>
        <td><p>1 Interesting File / Seen in logs</p><p>2 Misconfiguration / Default File</p><p>3 Information Disclosure</p><p>4 Injection (XSS/Script/HTML)</p><p>5 Remote File Retrieval - Inside Web Root</p><p>6 Denial of Service</p><p>7 Remote File Retrieval - Server Wide</p><p>8 Command Execution / Remote Shell</p><p>9 SQL Injection</p><p>0 File Upload</p><p>a Authentication Bypass</p><p>b Software Identification</p><p>c Remote Source Inclusion</p><p>x Reverse Tuning Options (i.e., include all except specified)</p></td>
    </tr>
    <tr>
        <td><code>-timeout+</code></td>
        <td><p>Timeout for requests (default 10 seconds)</p></td>
    </tr>
    <tr>
        <td><code>-Userdbs</code></td>
        <td><p>Load only user databases, not the standard databases</p><p>all Disable standard dbs and load only user dbs</p><p>tests Disable only db_tests and load udb_tests</p></td>
    </tr>
    <tr>
        <td><code>-until</code></td>
        <td><p>Run until the specified time or duration</p></td>
    </tr>
    <tr>
        <td><code>-update</code></td>
        <td><p>Update databases and plugins from CIRT.net</p></td>
    </tr>
    <tr>
        <td><code>-useproxy</code></td>
        <td><p>Use the proxy defined in nikto.conf</p></td>
    </tr>
    <tr>
        <td><code>-Version</code></td>
        <td><p>Print plugin and database versions</p></td>
    </tr>
    <tr>
        <td><code>-vhost+</code></td>
        <td><p>Virtual host (for Host header)</p></td>
    </tr>
</table>


## Nikto Example Commands

### Basic Scanning

<table>
  <tr>
    <th>Command</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><p><code>nikto -h [target]</code></p></td>
    <td><p>Basic scan, no HTTP options.</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h [target] -Tuning [tuning]</code></p></td>
    <td><p>Scan with a specific tuning.</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h [target] -mutate [mutate]</code></p></td>
    <td><p>Scan with a specific mutation.</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h [target] -ssl</code></p></td>
    <td><p>Scan using SSL.</p></td>
  </tr>
  <tr>
    <td><p><code>nikto -h [target] -nointeractive</code></p></td>
    <td><p>Run the scan non-interactively.</p></td>
  </tr>
</table>

## Nikto Using a Proxy 

Using Nikto with a proxy such as Burp or another intercepting proxy.

<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-useproxy</code></p></td>
      <td><p>Enable usage of the HTTP/SOCKS proxy</p></td>
    </tr>
    <tr>
      <td><p><code>-noproxy</code></p></td>
      <td><p>Specify comma separated list of hosts not to use proxy for</p></td>
    </tr>
    <tr>
      <td><p><code>-proxyhost</code></p></td>
      <td><p>Hostname or IP address of the HTTP/SOCKS proxy</p></td>
    </tr>
    <tr>
      <td><p><code>-proxyport</code></p></td>
      <td><p>Port of the HTTP/SOCKS proxy</p></td>
    </tr>
    <tr>
      <td><p><code>-proxypass</code></p></td>
      <td><p>Password for the HTTP/SOCKS proxy</p></td>
    </tr>
    <tr>
      <td><p><code>-proxyuser</code></p></td>
      <td><p>Username for the HTTP/SOCKS proxy</p></td>
    </tr>
  </tbody>
</table>