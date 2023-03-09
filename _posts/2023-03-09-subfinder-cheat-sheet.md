---
layout: blog_item
title:  "Subfinder Cheat Sheet"
date:   2023-03-09 14:37:10
author: Arr0way
description: 'Subdfinder install, examples and Subfinder commands cheatsheet'
categories: [cheat-sheet]
tags:
- Subfinder
- Tools
- 'Project Discovery'
---

Subfinder is a subdomain discovery tool made by Project Discovery, the following cheat sheet provides and overview of the command flags for Subfinder and common commamnd examples for real world usage. 

## Install Subfinder 

{% highlight bash %}
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
{% endhighlight %}

<div class="note tip">
  <h5>Configure API Keys</h5>
  <p>Subfinder works straight after install, however with API keys (even a free key) will improve passive subdomain results.</p>
</div>

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Subfinder Flags & Syntax</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">subfinder -h</span>
        </p>
          <span class="output"><br></span>
          <span class="output"><br></span>
      </div>
    </div>
</section>


<div class="mobile-side-scroller">
  <table>
	<thread>  
    <tr>
      <th>Flag</th>
      <th>Description</th>
    </tr>
	  </thread>
	  <tbody>
    <tr>
      <td><code>-d, -domain string[]</code></td>
      <td>domains to find subdomains for</td>
    </tr>
    <tr>
      <td><code>-dL, -list string</code></td>
      <td>file containing list of domains for subdomain discovery</td>
    </tr>
    <tr>
      <td><code>-s, -sources string[]</code></td>
      <td>specific sources to use for discovery (-s crtsh,github). Use -ls to display all available sources.</td>
    </tr>
    <tr>
      <td><code>-recursive</code></td>
      <td>use only sources that can handle subdomains recursively (e.g. subdomain.domain.tld vs domain.tld)</td>
    </tr>
    <tr>
      <td><code>-all</code></td>
      <td>use all sources for enumeration (slow)</td>
    </tr>
    <tr>
      <td><code>-es, -exclude-sources string[]</code></td>
      <td>sources to exclude from enumeration (-es alienvault,zoomeye)</td>
    </tr>
    <tr>
      <td><code>-m, -match string[]</code></td>
      <td>subdomain or list of subdomain to match (file or comma separated)</td>
    </tr>
    <tr>
      <td><code>-f, -filter string[]</code></td>
      <td>subdomain or list of subdomain to filter (file or comma separated)</td>
    </tr>
    <tr>
      <td><code>-rl, -rate-limit int</code></td>
      <td>maximum number of http requests to send per second</td>
    </tr>
    <tr>
      <td><code>-t int</code></td>
      <td>number of concurrent goroutines for resolving (-active only) (default 10)</td>
    </tr>
    <tr>
      <td><code>-o, -output string</code></td>
      <td>file to write output to</td>
    </tr>
    <tr>
      <td><code>-oJ, -json</code></td>
      <td>write output in JSONL(ines) format</td>
    </tr>
    <tr>
      <td><code>-oD, -output-dir string</code></td>
      <td>directory to write output (-dL only)</td>
    </tr>
    <tr>
      <td><code>-cs, -collect-sources</code></td>
      <td>include all sources in the output (-json only)</td>
    </tr>
    <tr>
      <td><code>-oI, -ip</code></td>
      <td>include host IP in output (-active only)</td>
    </tr>
    <tr>
      <td><code>-config string</code></td>
      <td>flag config file (default "$HOME/.config/subfinder/config.yaml")</td>
    </tr>
    <tr>
      <td><code>-pc, -provider-config string</code></td>
      <td>provider config file (default "$HOME/.config/subfinder/provider-config.yaml")</td>
    </tr>
    <tr>
      <td><code>-r string[]</code></td>
      <td>comma separated list of resolvers to use</td>
    </tr>
    <tr>
      <td><code>-rL, -rlist string</code></td>
      <td>file containing list of resolvers to use</td>
    </tr>
    <tr>
      <td><code>-nW, -active</code></td>
      <td>display active subdomains only</td>
    </tr>
    <tr>
      <td><code>-proxy string</code></td>
      <td>http proxy to use with subfinder</td>
    </tr>
    <tr>
      <td><code>-ei, -exclude-ip</code></td>
      <td>exclude IPs from the list of domains</td>
    </tr>
    <tr>
      <td><code>-silent</code></td>
      <td>show only subdomains in output</td>
    </tr>
    <tr>
      <td><code>-version</code></td>
      <td>show version of subfinder</td>
    </tr>
    <tr>
      <td><code>-v</code></td>
      <td>show verbose output</td>
    </tr>
    <tr>
      <td><code>-nc, -no-color</code></td>
      <td>disable color in output</td>
    </tr>
    <tr>
      <td><code>-ls, -list-sources</code></td>
      <td>list all available sources</td>
    </tr>
    <tr>
      <td><code>-timeout int</code></td>
      <td>seconds to wait before timing out (default 30)</td>
    </tr>
    <tr>
      <td><code>-max-time int</code></td>
      <td>minutes to wait for enumeration results (default 10)</td>
    </tr>
	<tbody>	  
  </table>
</div>


## Example Subfinder Commands 

### Find Subdomains Single Domain 

Find subdomains for a single domain with subfinder:

{% highlight bash %}
subfinder -d hackerone.com

               __    _____           __
   _______  __/ /_  / __(_)___  ____/ /__  _____
  / ___/ / / / __ \/ /_/ / __ \/ __  / _ \/ ___/
 (__  ) /_/ / /_/ / __/ / / / / /_/ /  __/ /
/____/\__,_/_.___/_/ /_/_/ /_/\__,_/\___/_/ v2.5.1

		projectdiscovery.io

Use with caution. You are responsible for your actions
Developers assume no liability and are not responsible for any misuse or damage.
By using subfinder, you also agree to the terms of the APIs used.

[INF] Enumerating subdomains for hackerone.com
info.hackerone.com
design.hackerone.com
docs.hackerone.com
events.hackerone.com
web-seo-content-for-business.theflyingkick.websitedesignresource.api.hackerone.com
zendesk2.hackerone.com
fsdkim.hackerone.com
email.gh-mail.hackerone.com
a.ns.hackerone.com
support.hackerone.com
www.hackerone.com
mta-sts.managed.hackerone.com
api.hackerone.com
gslink.hackerone.com
zendesk1.hackerone.com
3d.hackerone.com
links.hackerone.com
mta-sts.hackerone.com
resources.hackerone.com
zendesk4.hackerone.com
zendesk3.hackerone.com
go.hackerone.com
mta-sts.forwarding.hackerone.com
_dmarc.hackerone.com
b.ns.hackerone.com
hackerone.com
defcon.hackerone.com
[INF] Found 27 subdomains for hackerone.com in 30 seconds 33 milliseconds
{% endhighlight %}


### Verify Subfinder Results With HTTPX 

Chain up other tools within your workflow, such as verifying targets have web servers using HTTPX:  

{% highlight bash %}
echo hackerone.com | subfinder -silent | httpx -silent
https://docs.hackerone.com
https://mta-sts.forwarding.hackerone.com
https://mta-sts.hackerone.com
https://mta-sts.managed.hackerone.com
http://a.ns.hackerone.com
https://www.hackerone.com
http://b.ns.hackerone.com
http://zendesk4.hackerone.com
http://fsdkim.hackerone.com
http://zendesk1.hackerone.com
http://zendesk2.hackerone.com
http://zendesk3.hackerone.com
https://hackerone.com
https://support.hackerone.com
https://resources.hackerone.com
https://gslink.hackerone.com
https://api.hackerone.com
{% endhighlight %}

### Subfinder + Naabu Portscan 

{% highlight bash %}
echo hackerone.com | subfinder -silent | naabu -silent
docs.hackerone.com:443
docs.hackerone.com:80
mta-sts.forwarding.hackerone.com:443
mta-sts.forwarding.hackerone.com:80
mta-sts.hackerone.com:80
mta-sts.hackerone.com:443
mta-sts.managed.hackerone.com:80
mta-sts.managed.hackerone.com:443
<--SNIP-->
{% endhighlight %}
