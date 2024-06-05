---
layout: blog_item
title:  "HTTPX Cheat Sheet - Commands & Examples Tutorial"
date:   2024-06-04 05:37:10
author: Arr0way
description: 'HTTPX cheat sheet - Learn HTTPX with this Tutorial containing Flags, & HTTPX Command Examples.'
categories: [cheat-sheet]
tags:
- Web
- Tools
---


![httpx logo](/img/httpx-logo.png)

## What is HTTPX? 

HTTPX is a fast and multi-purpose HTTP toolkit made by Project Discovery that allows running multiple probes using the retryablehttp library. It is designed to maintain result reliability with an increased number of threads. HTTPX can be used to obtain web server information, such as headers, pages and take screenshots of targets, perfect to validate http/https servers for large scopes on bugbounty programs or performing asset management / penetration testing. 

* list element with functor item
{:toc}

## HTTPX Installation 

How to install HTTPX: 

{% highlight bash %}

go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest

{% endhighlight %}

## HTTPX Supported Probes 

The type of data HTTPX can obtain from target web servers: 


| Probes          | Default check | Probes         | Default check |
|-----------------|---------------|----------------|---------------|
| URL             | true          | IP             | true          |
| Title           | true          | CNAME          | true          |
| Status Code     | true          | Raw HTTP       | false         |
| Content Length  | true          | HTTP2          | false         |
| TLS Certificate | true          | HTTP Pipeline  | false         |
| CSP Header      | true          | Virtual host   | false         |
| Line Count      | true          | Word Count     | true          |
| Location Header | true          | CDN            | false         |
| Web Server      | true          | Paths          | false         |
| Web Socket      | true          | Ports          | false         |
| Response Time   | true          | Request Method | true          |
| Favicon Hash    | false         | Probe  Status  | false         |
| Body Hash       | true          | Header  Hash   | true          |
| Redirect chain  | false         | URL Scheme     | true          |
| JARM Hash       | false         | ASN            | false         |


<div class='note'><h2>TIP: Take Screenshots with HTTPX</h2>
<p>To take screenshots with HTTPX use <code>-screenshot</code> or <code>-ss</code></p> 
</div>

## HTTPX Input Commands

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-l, -list string</code></p></td>
      <td><p>input file containing list of hosts to process</p></td>
    </tr>
    <tr>
      <td><p><code>-rr, -request string</code></p></td>
      <td><p>file containing raw request</p></td>
    </tr>
    <tr>
      <td><p><code>-u, -target string[]</code></p></td>
      <td><p>input target host(s) to probe</p></td>
    </tr>
  </tbody>
</table>


## HTTPX Probe Commands

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-sc, -status-code</code></p></td>
      <td><p>display response status-code</p></td>
    </tr>
    <tr>
      <td><p><code>-cl, -content-length</code></p></td>
      <td><p>display response content-length</p></td>
    </tr>
    <tr>
      <td><p><code>-ct, -content-type</code></p></td>
      <td><p>display response content-type</p></td>
    </tr>
    <tr>
      <td><p><code>-location</code></p></td>
      <td><p>display response redirect location</p></td>
    </tr>
    <tr>
      <td><p><code>-favicon</code></p></td>
      <td><p>display mmh3 hash for '/favicon.ico' file</p></td>
    </tr>
    <tr>
      <td><p><code>-hash string</code></p></td>
      <td><p>display response body hash (supported: md5,mmh3,simhash,sha1,sha256,sha512)</p></td>
    </tr>
    <tr>
      <td><p><code>-jarm</code></p></td>
      <td><p>display jarm fingerprint hash</p></td>
    </tr>
    <tr>
      <td><p><code>-rt, -response-time</code></p></td>
      <td><p>display response time</p></td>
    </tr>
    <tr>
      <td><p><code>-lc, -line-count</code></p></td>
      <td><p>display response body line count</p></td>
    </tr>
    <tr>
      <td><p><code>-wc, -word-count</code></p></td>
      <td><p>display response body word count</p></td>
    </tr>
    <tr>
      <td><p><code>-title</code></p></td>
      <td><p>display page title</p></td>
    </tr>
    <tr>
      <td><p><code>-bp, -body-preview</code></p></td>
      <td><p>display first N characters of response body (default 100)</p></td>
    </tr>
    <tr>
      <td><p><code>-server, -web-server</code></p></td>
      <td><p>display server name</p></td>
    </tr>
    <tr>
      <td><p><code>-td, -tech-detect</code></p></td>
      <td><p>display technology in use based on wappalyzer dataset</p></td>
    </tr>
    <tr>
      <td><p><code>-method</code></p></td>
      <td><p>display http request method</p></td>
    </tr>
    <tr>
      <td><p><code>-websocket</code></p></td>
      <td><p>display server using websocket</p></td>
    </tr>
    <tr>
      <td><p><code>-ip</code></p></td>
      <td><p>display host ip</p></td>
    </tr>
    <tr>
      <td><p><code>-cname</code></p></td>
      <td><p>display host cname</p></td>
    </tr>
    <tr>
      <td><p><code>-asn</code></p></td>
      <td><p>display host asn information</p></td>
    </tr>
    <tr>
      <td><p><code>-cdn</code></p></td>
      <td><p>display cdn/waf in use (default true)</p></td>
    </tr>
    <tr>
      <td><p><code>-probe</code></p></td>
      <td><p>display probe status</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Headless Options 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-ss, -screenshot</code></p></td>
      <td><p>enable saving screenshot of the page using headless browser</p></td>
    </tr>
    <tr>
      <td><p><code>-system-chrome</code></p></td>
      <td><p>enable using local installed chrome for screenshot</p></td>
    </tr>
    <tr>
      <td><p><code>-esb, -exclude-screenshot-bytes</code></p></td>
      <td><p>enable excluding screenshot bytes from json output</p></td>
    </tr>
    <tr>
      <td><p><code>-ehb, -exclude-headless-body</code></p></td>
      <td><p>enable excluding headless header from json output</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Match in Response 

Allows HTTPX to match something in the server response header / body / http response code or url etc. 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-mc, -match-code string</code></p></td>
      <td><p>match response with specified status code (-mc 200,302)</p></td>
    </tr>
    <tr>
      <td><p><code>-ml, -match-length string</code></p></td>
      <td><p>match response with specified content length (-ml 100,102)</p></td>
    </tr>
    <tr>
      <td><p><code>-mlc, -match-line-count string</code></p></td>
      <td><p>match response body with specified line count (-mlc 423,532)</p></td>
    </tr>
    <tr>
      <td><p><code>-mwc, -match-word-count string</code></p></td>
      <td><p>match response body with specified word count (-mwc 43,55)</p></td>
    </tr>
    <tr>
      <td><p><code>-mfc, -match-favicon string[]</code></p></td>
      <td><p>match response with specified favicon hash (-mfc 1494302000)</p></td>
    </tr>
    <tr>
      <td><p><code>-ms, -match-string string</code></p></td>
      <td><p>match response with specified string (-ms admin)</p></td>
    </tr>
    <tr>
      <td><p><code>-mr, -match-regex string</code></p></td>
      <td><p>match response with specified regex (-mr admin)</p></td>
    </tr>
    <tr>
      <td><p><code>-mcdn, -match-cdn string[]</code></p></td>
      <td><p>match host with specified cdn provider (cloudfront, fastly, google, leaseweb, stackpath)</p></td>
    </tr>
    <tr>
      <td><p><code>-mrt, -match-response-time string</code></p></td>
      <td><p>match response with specified response time in seconds (-mrt '&lt; 1')</p></td>
    </tr>
    <tr>
      <td><p><code>-mdc, -match-condition string</code></p></td>
      <td><p>match response with dsl expression condition</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Extract Regex Strings 

Allows HTTPX to extract regex strings from the reponse. 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-er, -extract-regex string[]</code></p></td>
      <td><p>display response content with matched regex</p></td>
    </tr>
    <tr>
      <td><p><code>-ep, -extract-preset string[]</code></p></td>
      <td><p>display response content matched by a pre-defined regex (mail, url, ipv4)</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Filters 

Filter by response code, length, server version, error page, url etc

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-fc, -filter-code string</code></p></td>
      <td><p>filter response with specified status code (-fc 403,401)</p></td>
    </tr>
    <tr>
      <td><p><code>-fep, -filter-error-page</code></p></td>
      <td><p>filter response with ML based error page detection</p></td>
    </tr>
    <tr>
      <td><p><code>-fl, -filter-length string</code></p></td>
      <td><p>filter response with specified content length (-fl 23,33)</p></td>
    </tr>
    <tr>
      <td><p><code>-flc, -filter-line-count string</code></p></td>
      <td><p>filter response body with specified line count (-flc 423,532)</p></td>
    </tr>
    <tr>
      <td><p><code>-fwc, -filter-word-count string</code></p></td>
      <td><p>filter response body with specified word count (-fwc 423,532)</p></td>
    </tr>
    <tr>
      <td><p><code>-ffc, -filter-favicon string[]</code></p></td>
      <td><p>filter response with specified favicon hash (-ffc 1494302000)</p></td>
    </tr>
    <tr>
      <td><p><code>-fs, -filter-string string</code></p></td>
      <td><p>filter response with specified string (-fs admin)</p></td>
    </tr>
    <tr>
      <td><p><code>-fe, -filter-regex string</code></p></td>
      <td><p>filter response with specified regex (-fe admin)</p></td>
    </tr>
    <tr>
      <td><p><code>-fcdn, -filter-cdn string[]</code></p></td>
      <td><p>filter host with specified cdn provider (cloudfront, fastly, google, leaseweb, stackpath)</p></td>
    </tr>
    <tr>
      <td><p><code>-frt, -filter-response-time string</code></p></td>
      <td><p>filter response with specified response time in seconds (-frt '&gt; 1')</p></td>
    </tr>
    <tr>
      <td><p><code>-fdc, -filter-condition string</code></p></td>
      <td><p>filter response with dsl expression condition</p></td>
    </tr>
    <tr>
      <td><p><code>-strip</code></p></td>
      <td><p>strips all tags in response. supported formats: html,xml (default html)</p></td>
    </tr>
  </tbody>
</table>


## HTTPX Rate Limiting 

Limit the number of requests HTTPX can make per second / per minute and configure the number of threads. 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-t, -threads int</code></p></td>
      <td><p>number of threads to use (default 50)</p></td>
    </tr>
    <tr>
      <td><p><code>-rl, -rate-limit int</code></p></td>
      <td><p>maximum requests to send per second (default 150)</p></td>
    </tr>
    <tr>
      <td><p><code>-rlm, -rate-limit-minute int</code></p></td>
      <td><p>maximum number of requests to send per minute</p></td>
    </tr>
  </tbody>
</table>

## Misc HTTPX Commands 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-pa, -probe-all-ips</code></p></td>
      <td><p>probe all the ips associated with same host</p></td>
    </tr>
    <tr>
      <td><p><code>-p, -ports string[]</code></p></td>
      <td><p>ports to probe (nmap syntax: eg http:1,2-10,11,https:80)</p></td>
    </tr>
    <tr>
      <td><p><code>-path string</code></p></td>
      <td><p>path or list of paths to probe (comma-separated, file)</p></td>
    </tr>
    <tr>
      <td><p><code>-tls-probe</code></p></td>
      <td><p>send http probes on the extracted TLS domains (dns_name)</p></td>
    </tr>
    <tr>
      <td><p><code>-csp-probe</code></p></td>
      <td><p>send http probes on the extracted CSP domains</p></td>
    </tr>
    <tr>
      <td><p><code>-tls-grab</code></p></td>
      <td><p>perform TLS(SSL) data grabbing</p></td>
    </tr>
    <tr>
      <td><p><code>-pipeline</code></p></td>
      <td><p>probe and display server supporting HTTP1.1 pipeline</p></td>
    </tr>
    <tr>
      <td><p><code>-http2</code></p></td>
      <td><p>probe and display server supporting HTTP2</p></td>
    </tr>
    <tr>
      <td><p><code>-vhost</code></p></td>
      <td><p>probe and display server supporting VHOST</p></td>
    </tr>
    <tr>
      <td><p><code>-ldv, -list-dsl-variables</code></p></td>
      <td><p>list json output field keys name that support dsl matcher/filter</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Update 

How to update HTTPX + how to disable auto update. 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-up, -update</code></code></p></td>
      <td><p>update httpx to latest version</p></td>
    </tr>
    <tr>
      <td><p><code>-duc, -disable-update-check</code></p></td>
      <td><p>disable automatic httpx update check</p></td>
    </tr>
  </tbody>
</table>


## HTTPX File Output 

HTTPX output file options. 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-o, -output string</code></p></td>
      <td><p>file to write output results</p></td>
    </tr>
    <tr>
      <td><p><code>-oa, -output-all</code></p></td>
      <td><p>filename to write output results in all formats</p></td>
    </tr>
    <tr>
      <td><p><code>-sr, -store-response</code></p></td>
      <td><p>store http response to output directory</p></td>
    </tr>
    <tr>
      <td><p><code>-srd, -store-response-dir string</code></p></td>
      <td><p>store http response to custom directory</p></td>
    </tr>
    <tr>
      <td><p><code>-csv</code></p></td>
      <td><p>store output in csv format</p></td>
    </tr>
    <tr>
      <td><p><code>-csvo, -csv-output-encoding string</code></p></td>
      <td><p>define output encoding</p></td>
    </tr>
    <tr>
      <td><p><code>-j, -json</code></p></td>
      <td><p>store output in JSONL(ines) format</p></td>
    </tr>
    <tr>
      <td><p><code>-irh, -include-response-header</code></p></td>
      <td><p>include http response (headers) in JSON output (-json only)</p></td>
    </tr>
    <tr>
      <td><p><code>-irr, -include-response</code></p></td>
      <td><p>include http request/response (headers + body) in JSON output (-json only)</p></td>
    </tr>
    <tr>
      <td><p><code>-irrb, -include-response-base64</code></p></td>
      <td><p>include base64 encoded http request/response in JSON output (-json only)</p></td>
    </tr>
    <tr>
      <td><p><code>-include-chain</code></p></td>
      <td><p>include redirect http chain in JSON output (-json only)</p></td>
    </tr>
    <tr>
      <td><p><code>-store-chain</code></p></td>
      <td><p>include http redirect chain in responses (-sr only)</p></td>
    </tr>
    <tr>
      <td><p><code>-svrc, -store-vision-recon-cluster</code></p></td>
      <td><p>include visual recon clusters (-ss and -sr only)</p></td>
    </tr>
  </tbody>
</table>

## HTTPX Config Options 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-config string</code></p></td>
      <td><p>path to the httpx configuration file (default $HOME/.config/httpx/config.yaml)</p></td>
    </tr>
    <tr>
      <td><p><code>-r, -resolvers string[]</code></p></td>
      <td><p>list of custom resolver (file or comma separated)</p></td>
    </tr>
    <tr>
      <td><p><code>-allow string[]</code></p></td>
      <td><p>allowed list of IP/CIDR's to process (file or comma separated)</p></td>
    </tr>
    <tr>
      <td><p><code>-deny string[]</code></p></td>
      <td><p>denied list of IP/CIDR's to process (file or comma separated)</p></td>
    </tr>
    <tr>
      <td><p><code>-sni, -sni-name string</code></p></td>
      <td><p>custom TLS SNI name</p></td>
    </tr>
    <tr>
      <td><p><code>-random-agent</code></p></td>
      <td><p>enable Random User-Agent to use (default true)</p></td>
    </tr>
    <tr>
      <td><p><code>-H, -header string[]</code></p></td>
      <td><p>custom http headers to send with request</p></td>
    </tr>
    <tr>
      <td><p><code>-http-proxy, -proxy string</code></p></td>
      <td><p>http proxy to use (eg http://127.0.0.1:8080)</p></td>
    </tr>
    <tr>
      <td><p><code>-unsafe</code></p></td>
      <td><p>send raw requests skipping golang normalization</p></td>
    </tr>
    <tr>
      <td><p><code>-resume</code></p></td>
      <td><p>resume scan using resume.cfg</p></td>
    </tr>
    <tr>
      <td><p><code>-fr, -follow-redirects</code></p></td>
      <td><p>follow http redirects</p></td>
    </tr>
    <tr>
      <td><p><code>-maxr, -max-redirects int</code></p></td>
      <td><p>max number of redirects to follow per host (default 10)</p></td>
    </tr>
    <tr>
      <td><p><code>-fhr, -follow-host-redirects</code></p></td>
      <td><p>follow redirects on the same host</p></td>
    </tr>
    <tr>
      <td><p><code>-rhsts, -respect-hsts</code></p></td>
      <td><p>respect HSTS response headers for redirect requests</p></td>
    </tr>
    <tr>
      <td><p><code>-vhost-input</code></p></td>
      <td><p>get a list of vhosts as input</p></td>
    </tr>
    <tr>
      <td><p><code>-x string</code></p></td>
      <td><p>request methods to probe, use 'all' to probe all HTTP methods</p></td>
    </tr>
    <tr>
      <td><p><code>-body string</code></p></td>
      <td><p>post body to include in http request</p></td>
    </tr>
    <tr>
      <td><p><code>-s, -stream</code></p></td>
      <td><p>stream mode - start elaborating input targets without sorting</p></td>
    </tr>
    <tr>
      <td><p><code>-sd, -skip-dedupe</code></p></td>
      <td><p>disable dedupe input items (only used with stream mode)</p></td>
    </tr>
    <tr>
      <td><p><code>-ldp, -leave-default-ports</code></p></td>
      <td><p>leave default http/https ports in host header (eg. http://host:80 - https://host:443)</p></td>
    </tr>
    <tr>
      <td><p><code>-ztls</code></p></td>
      <td><p>use ztls library with autofallback to standard one for tls13</p></td>
    </tr>
    <tr>
      <td><p><code>-no-decode</code></p></td>
      <td><p>avoid decoding body</p></td>
    </tr>
    <tr>
      <td><p><code>-tlsi, -tls-impersonate</code></p></td>
      <td><p>enable experimental client hello (ja3) tls randomization</p></td>
    </tr>
    <tr>
      <td><p><code>-no-stdin</code></p></td>
      <td><p>Disable Stdin processing</p></td>
    </tr>
  </tbody>
</table>


## HTTPX Debug Options 

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-health-check, -hc</code></p></td>
      <td><p>run diagnostic check up</p></td>
    </tr>
    <tr>
      <td><p><code>-debug</code></p></td>
      <td><p>display request/response content in cli</p></td>
    </tr>
    <tr>
      <td><p><code>-debug-req</code></p></td>
      <td><p>display request content in cli</p></td>
    </tr>
    <tr>
      <td><p><code>-debug-resp</code></p></td>
      <td><p>display response content in cli</p></td>
    </tr>
    <tr>
      <td><p><code>-version</code></p></td>
      <td><p>display httpx version</p></td>
    </tr>
    <tr>
      <td><p><code>-stats</code></p></td>
      <td><p>display scan statistic</p></td>
    </tr>
    <tr>
      <td><p><code>-profile-mem string</code></p></td>
      <td><p>optional httpx memory profile dump file</p></td>
    </tr>
    <tr>
      <td><p><code>-silent</code></p></td>
      <td><p>silent mode</p></td>
    </tr>
    <tr>
      <td><p><code>-v, -verbose</code></p></td>
      <td><p>verbose mode</p></td>
    </tr>
    <tr>
      <td><p><code>-si, -stats-interval int</code></p></td>
      <td><p>number of seconds to wait between showing a statistics update (default: 5)</p></td>
    </tr>
    <tr>
      <td><p><code>-nc, -no-color</code></p></td>
      <td><p>disable colors in cli output</p></td>
    </tr>
  </tbody>
</table>

## Optimizations 

Improve the performance of HTTPX tune the settings to the target environment.

<table>
  <thead>
    <tr>
      <th>COMMAND</th>
      <th>DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><p><code>-nf, -no-fallback</code></p></td>
      <td><p>display both probed protocol (HTTPS and HTTP)</p></td>
    </tr>
    <tr>
      <td><p><code>-nfs, -no-fallback-scheme</code></p></td>
      <td><p>probe with protocol scheme specified in input</p></td>
    </tr>
    <tr>
      <td><p><code>-maxhr, -max-host-error int</code></p></td>
      <td><p>max error count per host before skipping remaining path/s (default 30)</p></td>
    </tr>
    <tr>
      <td><p><code>-ec, -exclude-cdn</code></p></td>
      <td><p>skip full port scans for CDN/WAF (only checks for 80,443)</p></td>
    </tr>
    <tr>
      <td><p><code>-eph, -exclude-private-hosts</code></p></td>
      <td><p>skip any hosts which have a private ip address</p></td>
    </tr>
    <tr>
      <td><p><code>-retries int</code></p></td>
      <td><p>number of retries</p></td>
    </tr>
    <tr>
      <td><p><code>-timeout int</code></p></td>
      <td><p>timeout in seconds (default 10)</p></td>
    </tr>
    <tr>
      <td><p><code>-delay value</code></p></td>
      <td><p>duration between each http request (eg: 200ms, 1s) (default -1ns)</p></td>
    </tr>
    <tr>
      <td><p><code>-rsts, -response-size-to-save int</code></p></td>
      <td><p>max response size to save in bytes (default 2147483647)</p></td>
    </tr>
    <tr>
      <td><p><code>-rstr, -response-size-to-read int</code></p></td>
      <td><p>max response size to read in bytes (default 2147483647)</p></td>
    </tr>
  </tbody>
</table>

## Real World Examples

### DNSX to HTTPX

Run domains through dnsx to confirm resolution, then through httpx to confirm a 200 response from the webserver: 

{% highlight bash %}

dnsx -d roots.txt -w <key,words> | httpx -sc -mc 200

{% endhighlight %}

### HTTPX Screenshot 

Take a screenshot of targets that return 200 response: 

{% highlight bash %}

httpx -sc -mc 200 -ss

{% endhighlight %}

### Basic Recon 

{% highlight bash %}

httpx -t 200 -random-agent -nc -silent -timeout 8 -sc -server -title -o httpx.out

{% endhighlight %}

## Conclusion 

We hope this HTTPX cheat sheet was useful in covering the usage of this excellent HTTP toolkit by Project Discovery. 
