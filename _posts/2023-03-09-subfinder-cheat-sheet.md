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

![Subfinder Logo](/img/subfinder-logo.png)

* list element with functor item
{:toc}


## What is Subfinder

Subfinder is a passive subdomain discovery tool made by Project Discovery. The following subfinder cheat sheet provides an overview of the command flags for Subfinder and common command examples for real world usage. Subfinder can be used to obtain a number of valid subdomains both passively and actively, to identify more attack surface for [penetration testing](/penetration-testing/) or bug bounty recon or assessment. 

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


## Subfinder API Setup 

Configuring Subfinder to use free or paid API services will likely improve the discovered domains the tool can find. You can list the sources Subfinder uses by running ```subfinder -ls```. 

### Subfinder Config File

In order to setup subfinder API keys you need to create or modify the existing configuration file. The filesystem location for the subfinder config file is at: ```$HOME/.config/subfinder/provider-config.yaml``` the subfinder config file needs to be populated with the API keys that you will need to obtain from the various sources that have (kindly) been listed below. 

### Subfinder API Sources 

Subfinder supports the following data API sources:  

<table>
  <tr>
    <th>NAME</th>
    <th>URL</th>
  </tr>
  <tr>
    <td>BeVigil</td>
    <td><p><code>https://bevigil.com/osint-api</code></p></td>
  </tr>
  <tr>
    <td>BinaryEdge</td>
    <td><p><code>https://binaryedge.io</code></p></td>
  </tr>
  <tr>
    <td>BufferOver</td>
    <td><p><code>https://tls.bufferover.run</code></p></td>
  </tr>
  <tr>
    <td>C99</td>
    <td><p><code>https://api.c99.nl/</code></p></td>
  </tr>
  <tr>
    <td>Censys</td>
    <td><p><code>https://censys.io</code></p></td>
  </tr>
  <tr>
    <td>CertSpotter</td>
    <td><p><code>https://sslmate.com/certspotter/api/</code></p></td>
  </tr>
  <tr>
    <td>Chaos</td>
    <td><p><code>https://chaos.projectdiscovery.io</code></p></td>
  </tr>
  <tr>
    <td>Chinaz</td>
    <td><p><code>http://my.chinaz.com/ChinazAPI/DataCenter/MyDataApi</code></p></td>
  </tr>
  <tr>
    <td>DNSDB</td>
    <td><p><code>https://api.dnsdb.info</code></p></td>
  </tr>
  <tr>
    <td>Fofa</td>
    <td><p><code>https://fofa.info/static_pages/api_help</code></p></td>
  </tr>
  <tr>
    <td>FullHunt</td>
    <td><p><code>https://fullhunt.io</code></p></td>
  </tr>
  <tr>
    <td>GitHub</td>
    <td><p><code>https://github.com</code></p></td>
  </tr>
  <tr>
    <td>Intelx</td>
    <td><p><code>https://intelx.io</code></p></td>
  </tr>
  <tr>
    <td>PassiveTotal</td>
    <td><p><code>http://passivetotal.org</code></p></td>
  </tr>
  <tr>
    <td>quake</td>
    <td><p><code>https://quake.360.cn</code></p></td>
  </tr>
  <tr>
    <td>Robtex</td>
    <td><p><code>https://www.robtex.com/api/</code></p></td>
  </tr>
  <tr>
    <td>SecurityTrails</td>
    <td><p><code>http://securitytrails.com</code></p></td>
  </tr>
  <tr>
    <td>Shodan</td>
    <td><p><code>https://shodan.io</code></p></td>
  </tr>
  <tr>
    <td>ThreatBook</td>
    <td><p><code>https://x.threatbook.cn/en</code></p></td>
  </tr>
  <tr>
    <td>VirusTotal</td>
    <td><p><code>https://www.virustotal.com</code></p></td>
  </tr>
  <tr>
    <td>WhoisXML API</td>
    <td><p><code>https://whoisxmlapi.com/</code></p></td>
  </tr>
  <tr>
    <td>ZoomEye</td>
    <td><p><code>https://www.zoomeye.org</code></p></td>
  </tr>
  <tr>
    <td>ZoomEye API</td>
    <td><p><code>https://api.zoomeye.org</code></p></td>
  </tr>
  <tr>
    <td>dnsrepo</td>
    <td><p><code>https://dnsrepo.noc.org</code></p></td>
  </tr>
  <tr>
    <td>Hunter</td>
    <td><p><code>https://hunter.qianxin.com/</code></p></td>
  </tr>
  <tr>
    <td>Facebook</td>
    <td><p><code>https://developers.facebook.com</code></p></td>
  </tr>
  <tr>
    <td>BuiltWith</td>
    <td><p><code>https://api.builtwith.com/domain-api</code></p></td>
  </tr>
</table>


## Example Subfinder API Config File

The following is an example of the API config file: 

{% highlight bash %}

binaryedge:
  - 0bf8919b-aab9-42e4-9574-d3b639324597
  - ac244e2f-b635-4581-878a-33f4e79a2c13
censys:
  - ac244e2f-b635-4581-878a-33f4e79a2c13:dd510d6e-1b6e-4655-83f6-f347b363def9
certspotter: []
passivetotal:
  - sample-email@user.com:sample_password
redhuntlabs:
  - ENDPOINT:API_TOKEN
  - https://reconapi.redhuntlabs.com/community/v1/domains/subdomains:joEPzJJp2AuOCw7teAj63HYrPGnsxuPQ
securitytrails: []
shodan:
  - AAAAClP1bJJSRMEYJazgwhJKrggRwKA
github:
  - ghp_lkyJGU3jv1xmwk4SDXavrLDJ4dl2pSJMzj4X
  - ghp_gkUuhkIYdQPj13ifH4KA3cXRn8JD2lqir2d4
zoomeyeapi:
  - 4f73021d-ff95-4f53-937f-83d6db719eec
quake:
  - 0cb9030c-0a40-48a3-b8c4-fca28e466ba3
facebook:
  - APP_ID:APP_SECRET
intelx:
  - HOST:API_KEY
  - 2.intelx.io:s4324-b98b-41b2-220e8-3320f6a1284d

{% endhighlight %}

Above file source: https://docs.projectdiscovery.io/tools/subfinder/install#post-install-configuration

## Subfinder Usage 

How to use Subfinder to find domains: 

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
	    <td><p><code>-d, -domain string[]</code></p></td>
	    <td><p>domains to find subdomains for</p></td>
    </tr>
    <tr>
	    <td><p><code>-dL, -list string</code></p></td>
	    <td><p>file containing list of domains for subdomain discovery</p></td>
    </tr>
    <tr>
	    <td><p><code>-s, -sources string[]</code></p></td>
      <td><p>specific sources to use for discovery (-s crtsh,github). Use -ls to display all available sources.</p></td>
    </tr>
    <tr>
      <td><p><code>-recursive</code></p></td>
      <td><p>use only sources that can handle subdomains recursively (e.g. subdomain.domain.tld vs domain.tld)</p></td>
    </tr>
    <tr>
      <td><p><code>-all</code></p></td>
      <td><p>use all sources for enumeration (slow)</p></td>
    </tr>
    <tr>
      <td><p><code>-es, -exclude-sources string[]</code></p></td>
      <td><p>sources to exclude from enumeration (-es alienvault,zoomeye)</p></td>
    </tr>
    <tr>
      <td><p><code>-m, -match string[]</code></p></td>
      <td><p>subdomain or list of subdomain to match (file or comma separated)</p></td>
    </tr>
    <tr>
      <td><p><code>-f, -filter string[]</code></p></td>
      <td><p>subdomain or list of subdomain to filter (file or comma separated)</p></td>
    </tr>
    <tr>
      <td><p><code>-rl, -rate-limit int</code></p></td>
      <td><p>maximum number of http requests to send per second</p></td>
    </tr>
    <tr>
      <td><p><code>-t int</code></p></td>
      <td><p>number of concurrent goroutines for resolving (-active only) (default 10)</p></td>
    </tr>
    <tr>
      <td><p><code>-o, -output string</code></p></td>
      <td><p>file to write output to</p></td>
    </tr>
    <tr>
      <td><p><code>-oJ, -json</code></p></td>
      <td><p>write output in JSONL(ines) format</p></td>
    </tr>
    <tr>
      <td><p><code>-oD, -output-dir string</code></p></td>
      <td><p>directory to write output (-dL only)</p></td>
    </tr>
    <tr>
      <td><p><code>-cs, -collect-sources</code></p></td>
      <td><p>include all sources in the output (-json only)</p></td>
    </tr>
    <tr>
      <td><p><code>-oI, -ip</code></p></td>
      <td><p>include host IP in output (-active only)</p></td>
    </tr>
    <tr>
      <td><p><code>-config string</code></p></td>
      <td><p>flag config file (default "$HOME/.config/subfinder/config.yaml")</p></td>
    </tr>
    <tr>
      <td><p><code>-pc, -provider-config string</code></p></td>
      <td><p>provider config file (default "$HOME/.config/subfinder/provider-config.yaml")</p></td>
    </tr>
    <tr>
      <td><p><code>-r string[]</code></p></td>
      <td><p>comma separated list of resolvers to use</p></td>
    </tr>
    <tr>
      <td><p><code>-rL, -rlist string</code></p></td>
      <td><p>file containing list of resolvers to use</p></td>
    </tr>
    <tr>
      <td><p><code>-nW, -active</code></p></td>
      <td><p>display active subdomains only</p></td>
    </tr>
    <tr>
      <td><p><code>-proxy string</code></p></td>
      <td><p>http proxy to use with subfinder</p></td>
    </tr>
    <tr>
      <td><p><code>-ei, -exclude-ip</code></p></td>
      <td><p>exclude IPs from the list of domains</p></td>
    </tr>
    <tr>
      <td><p><code>-silent</code></p></td>
      <td><p>show only subdomains in output</p></td>
    </tr>
    <tr>
      <td><p><code>-version</code></p></td>
      <td><p>show version of subfinder</p></td>
    </tr>
    <tr>
      <td><p><code>-v</code></p></td>
      <td><p>show verbose output</p></td>
    </tr>
    <tr>
      <td><p><code>-nc, -no-color</code></p></td>
      <td><p>disable color in output</p></td>
    </tr>
    <tr>
      <td><p><code>-ls, -list-sources</code></p></td>
      <td><p>list all available sources</p></td>
    </tr>
    <tr>
      <td><p><code>-timeout int</code></p></td>
      <td><p>seconds to wait before timing out (default 30)</p></td>
    </tr>
    <tr>
      <td><p><code>-max-time int</code></p></td>
      <td><p>minutes to wait for enumeration results (default 10)</p></td>
    </tr>
	</tbody>	  
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

## Conclusion 

We hope you found this Subfinder cheat sheet useful, and it helps you get started with this powerful subdomain enumeration tool to find more assets for assessment. 

## Document Changelog 

- **Last Updated:** 04/06/2024 (6th of June 2024)
- **Author:** Arr0way
- **Notes:** Checked syntax was current for latest version of Subfinder + fixed typos.

---

- **Last Updated:** 12/02/2024 (12th of February 2024)
- **Author:** Arr0way 
- **Notes:** Checked syntax was current for latest version of Subfinder + added Subfinder API sources table. 
