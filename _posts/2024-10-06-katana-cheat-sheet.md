---
layout: blog_item
title:  "Katana Cheat Sheet - Commands, Flags & Examples"
date:   2024-06-10 14:37:10
author: Arr0way
description: 'Katana install, examples and Katana commands cheatsheet'
categories: [cheat-sheet]
tags:
- Katana
- Tools
- 'Project Discovery'
---

![Katana Logo](/img/katana-logo.png)


## What is Katana

Katana is a fast web crawler made by Project Discovery. The tool is both headless and non-headless with a focus on being used in automation workflows. For example Katana could be used to crawl a target and stored all crawled data, or Katana could be used to crawl a site and store all urls with inputs. The following Katana cheat sheet aims to provide an overview of the tools functionality and provide real world examples from existing workflows. 

## Install Katana 

{% highlight bash %}
go install github.com/projectdiscovery/katana/cmd/katana@latest
{% endhighlight %}

* list element with functor item
{:toc}

## Basic Usage 

{% highlight bash %}
katana -u target-domain.com 
{% endhighlight %}

### Katana Config File

In order to setup subfinder API keys you need to create or modify the existing configuration file. The filesystem location for the subfinder config file is at: ```$HOME/.config/subfinder/provider-config.yaml``` the subfinder config file needs to be populated with the API keys that you will need to obtain from the various sources that have (kindly) been listed below. 

## Katana Cheat Sheet 

### Katana Input Commands 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-u, -list string[]</code></p></td>
        <td><p>target url / list to crawl</p></td>
      </tr>
      <tr>
        <td><p><code>-resume string</code></p></td>
        <td><p>resume scan using resume.cfg</p></td>
      </tr>
      <tr>
        <td><p><code>-e, -exclude string[]</code></p></td>
        <td><p>exclude host matching specified filter ('cdn', 'private-ips', cidr, ip, regex)</p></td>
      </tr>
    </tbody>
  </table>
</div>

### Katana Configuration Options 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-u, -list string[]</code></p></td>
        <td><p>target url / list to crawl</p></td>
      </tr>
      <tr>
        <td><p><code>-resume string</code></p></td>
        <td><p>resume scan using resume.cfg</p></td>
      </tr>
      <tr>
        <td><p><code>-e, -exclude string[]</code></p></td>
        <td><p>exclude host matching specified filter ('cdn', 'private-ips', cidr, ip, regex)</p></td>
      </tr>
      <tr>
        <td><p><code>-r, -resolvers string[]</code></p></td>
        <td><p>list of custom resolver (file or comma separated)</p></td>
      </tr>
      <tr>
        <td><p><code>-d, -depth int</code></p></td>
        <td><p>maximum depth to crawl (default 3)</p></td>
      </tr>
      <tr>
        <td><p><code>-jc, -js-crawl</code></p></td>
        <td><p>enable endpoint parsing / crawling in javascript file</p></td>
      </tr>
      <tr>
        <td><p><code>-jsl, -jsluice</code></p></td>
        <td><p>enable jsluice parsing in javascript file (memory intensive)</p></td>
      </tr>
      <tr>
        <td><p><code>-ct, -crawl-duration value</code></p></td>
        <td><p>maximum duration to crawl the target for (s, m, h, d) (default s)</p></td>
      </tr>
      <tr>
        <td><p><code>-kf, -known-files string</code></p></td>
        <td><p>enable crawling of known files (all,robotstxt,sitemapxml), a minimum depth of 3 is required to ensure all known files are properly crawled.</p></td>
      </tr>
      <tr>
        <td><p><code>-mrs, -max-response-size int</code></p></td>
        <td><p>maximum response size to read (default 9223372036854775807)</p></td>
      </tr>
      <tr>
        <td><p><code>-timeout int</code></p></td>
        <td><p>time to wait for request in seconds (default 10)</p></td>
      </tr>
      <tr>
        <td><p><code>-aff, -automatic-form-fill</code></p></td>
        <td><p>enable automatic form filling (experimental)</p></td>
      </tr>
      <tr>
        <td><p><code>-fx, -form-extraction</code></p></td>
        <td><p>extract form, input, textarea & select elements in jsonl output</p></td>
      </tr>
      <tr>
        <td><p><code>-retry int</code></p></td>
        <td><p>number of times to retry the request (default 1)</p></td>
      </tr>
      <tr>
        <td><p><code>-proxy string</code></p></td>
        <td><p>http/socks5 proxy to use</p></td>
      </tr>
      <tr>
        <td><p><code>-H, -headers string[]</code></p></td>
        <td><p>custom header/cookie to include in all http request in header:value format (file)</p></td>
      </tr>
      <tr>
        <td><p><code>-config string</code></p></td>
        <td><p>path to the katana configuration file</p></td>
      </tr>
      <tr>
        <td><p><code>-fc, -form-config string</code></p></td>
        <td><p>path to custom form configuration file</p></td>
      </tr>
      <tr>
        <td><p><code>-flc, -field-config string</code></p></td>
        <td><p>path to custom field configuration file</p></td>
      </tr>
      <tr>
        <td><p><code>-s, -strategy string</code></p></td>
        <td><p>Visit strategy (depth-first, breadth-first) (default "depth-first")</p></td>
      </tr>
      <tr>
        <td><p><code>-iqp, -ignore-query-params</code></p></td>
        <td><p>Ignore crawling same path with different query-param values</p></td>
      </tr>
      <tr>
        <td><p><code>-tlsi, -tls-impersonate</code></p></td>
        <td><p>enable experimental client hello (ja3) tls randomization</p></td>
      </tr>
      <tr>
        <td><p><code>-dr, -disable-redirects</code></p></td>
        <td><p>disable following redirects (default false)</p></td>
      </tr>
    </tbody>
  </table>
</div>


### Katana Debug Options 

<div class="mobile-side-scroller">
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
        <td><p><code>-elog, -error-log string</code></p></td>
        <td><p>file to write sent requests error log</p></td>
      </tr>
    </tbody>
  </table>
</div>


### Katana Headless Mode Options 

Allow Katana to scan using a real browser, to pretent targets / wafs fingerprint blocking - your traffic will appear to be from a legitimate web browsers fingerprint.   

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-hl, -headless</code></p></td>
        <td><p>enable headless hybrid crawling (experimental)</p></td>
      </tr>
      <tr>
        <td><p><code>-sc, -system-chrome</code></p></td>
        <td><p>use local installed chrome browser instead of katana installed</p></td>
      </tr>
      <tr>
        <td><p><code>-sb, -show-browser</code></p></td>
        <td><p>show the browser on the screen with headless mode</p></td>
      </tr>
      <tr>
        <td><p><code>-ho, -headless-options string[]</code></p></td>
        <td><p>start headless chrome with additional options</p></td>
      </tr>
      <tr>
        <td><p><code>-nos, -no-sandbox</code></p></td>
        <td><p>start headless chrome in --no-sandbox mode</p></td>
      </tr>
      <tr>
        <td><p><code>-cdd, -chrome-data-dir string</code></p></td>
        <td><p>path to store chrome browser data</p></td>
      </tr>
      <tr>
        <td><p><code>-scp, -system-chrome-path string</code></p></td>
        <td><p>use specified chrome browser for headless crawling</p></td>
      </tr>
      <tr>
        <td><p><code>-noi, -no-incognito</code></p></td>
        <td><p>start headless chrome without incognito mode</p></td>
      </tr>
      <tr>
        <td><p><code>-cwu, -chrome-ws-url string</code></p></td>
        <td><p>use chrome browser instance launched elsewhere with the debugger listening at this URL</p></td>
      </tr>
      <tr>
        <td><p><code>-xhr, -xhr-extraction</code></p></td>
        <td><p>extract xhr request url,method in jsonl output</p></td>
      </tr>
    </tbody>
  </table>
</div>


### Katana Passive Crawling 

Using third party locations such as the wayback machine, crawl a target passively (without ever touching the target). 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-ps, -passive</code></p></td>
        <td><p>enable passive sources to discover target endpoints</p></td>
      </tr>
      <tr>
        <td><p><code>-pss, -passive-source string[]</code></p></td>
        <td><p>passive source to use for url discovery (waybackarchive,commoncrawl,alienvault)</p></td>
      </tr>
    </tbody>
  </table>
</div>

### Katana Scope Options 

Scope Katana to define what is in scope / out of scope including filters and exlcudes for file types. E.g., don't store crawled videos or jpg, fonts etc. 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-cs, -crawl-scope string[]</code></p></td>
        <td><p>in scope url regex to be followed by crawler</p></td>
      </tr>
      <tr>
        <td><p><code>-cos, -crawl-out-scope string[]</code></p></td>
        <td><p>out of scope url regex to be excluded by crawler</p></td>
      </tr>
      <tr>
        <td><p><code>-fs, -field-scope string</code></p></td>
        <td><p>pre-defined scope field (dn,rdn,fqdn) or custom regex (e.g., '(company-staging.io|company.com)') (default "rdn")</p></td>
      </tr>
      <tr>
        <td><p><code>-ns, -no-scope</code></p></td>
        <td><p>disables host based default scope</p></td>
      </tr>
      <tr>
        <td><p><code>-do, -display-out-scope</code></p></td>
        <td><p>display external endpoint from scoped crawling</p></td>
      </tr>
    </tbody>
  </table>
</div>


### Katana Filters 

Configure Katana to match or filter or exclude results based on the following configuration options. 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-cs, -crawl-scope string[]</code></p></td>
        <td><p>in scope url regex to be followed by crawler</p></td>
      </tr>
      <tr>
        <td><p><code>-cos, -crawl-out-scope string[]</code></p></td>
        <td><p>out of scope url regex to be excluded by crawler</p></td>
      </tr>
      <tr>
        <td><p><code>-fs, -field-scope string</code></p></td>
        <td><p>pre-defined scope field (dn,rdn,fqdn) or custom regex (e.g., '(company-staging.io|company.com)') (default "rdn")</p></td>
      </tr>
      <tr>
        <td><p><code>-ns, -no-scope</code></p></td>
        <td><p>disables host based default scope</p></td>
      </tr>
      <tr>
        <td><p><code>-do, -display-out-scope</code></p></td>
        <td><p>display external endpoint from scoped crawling</p></td>
      </tr>
    </tbody>
  </table>
</div>


### Katana Rate Limiting 

Configure the number of threads, or requests per second or per minute for Katana. 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-c, -concurrency int</code></p></td>
        <td><p>number of concurrent fetchers to use (default 10)</p></td>
      </tr>
      <tr>
        <td><p><code>-p, -parallelism int</code></p></td>
        <td><p>number of concurrent inputs to process (default 10)</p></td>
      </tr>
      <tr>
        <td><p><code>-rd, -delay int</code></p></td>
        <td><p>request delay between each request in seconds</p></td>
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
</div>

### How To Update Katana 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>-up, -update</code></p></td>
        <td><p>update katana to latest version</p></td>
      </tr>
      <tr>
        <td><p><code>-duc, -disable-update-check</code></p></td>
        <td><p>disable automatic katana update check</p></td>
      </tr>
    </tbody>
  </table>
</div>

### Katana Output File Options 

Output Katana crawl data to file types. 

<div class="mobile-side-scroller">
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
        <td><p>file to write output to</p></td>
      </tr>
      <tr>
        <td><p><code>-sr, -store-response</code></p></td>
        <td><p>store http requests/responses</p></td>
      </tr>
      <tr>
        <td><p><code>-srd, -store-response-dir string</code></p></td>
        <td><p>store http requests/responses to custom directory</p></td>
      </tr>
      <tr>
        <td><p><code>-or, -omit-raw</code></p></td>
        <td><p>omit raw requests/responses from jsonl output</p></td>
      </tr>
      <tr>
        <td><p><code>-ob, -omit-body</code></p></td>
        <td><p>omit response body from jsonl output</p></td>
      </tr>
      <tr>
        <td><p><code>-j, -jsonl</code></p></td>
        <td><p>write output in jsonl format</p></td>
      </tr>
      <tr>
        <td><p><code>-nc, -no-color</code></p></td>
        <td><p>disable output content coloring (ANSI escape codes)</p></td>
      </tr>
      <tr>
        <td><p><code>-silent</code></p></td>
        <td><p>display output only</p></td>
      </tr>
      <tr>
        <td><p><code>-v, -verbose</code></p></td>
        <td><p>display verbose output</p></td>
      </tr>
      <tr>
        <td><p><code>-debug</code></p></td>
        <td><p>display debug output</p></td>
      </tr>
      <tr>
        <td><p><code>-version</code></p></td>
        <td><p>display project version</p></td>
      </tr>
    </tbody>
  </table>
</div>


## Katana Example Commands 

### Katana Output Query Paramaters 

Build a list of URL input injection fields from a target: 

{% highlight bash %}
katana -f qurl -o qurl-output.txt
{% endhighlight %}

Do the same but from a httpx scan output text file: 

{% highlight bash %}
cut -d " " -f 1 httpx.txt | katana -f qurl -o qurl-httpx.txt
{% endhighlight %}

## Conclusion 

We hope you found this Katana cheat sheet useful, and it helps you get started with this powerful web crawler by Project Discovery. 

