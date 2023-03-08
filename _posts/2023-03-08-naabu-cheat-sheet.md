---
layout: blog_item
title:  "Naabu Cheat Sheet: Commands & Examples"
date:   2023-03-08 10:37:10
author: Arr0way
description: 'Naabu Cheat Sheet - A port scanner writte in Golang by Project Discovery. What is it, and how to use it with command examples.'
categories: [cheat-sheet]
tags:
- 'Penetration Testing'
- 'Projct Discovery'
- Tools
---

* list element with functor item
{:toc}

The following Nabuu cheat sheet aims to explain what Naabu is, what it does, and how to install it and use it by providing Nabuu command examples in a cheat sheet style documentation format.

## What is Naabu?

Naabu is a simple port scanner written in Golang by Project Discovery, with a goal of being simple and fast.

## Naabu vs Nmap 

Why use Naabu over [Nmap](https://highon.coffee/blog/nmap-cheat-sheet/), the primary reason for me personally is the automatic IP deduplication. Meaning, when performing subdomain or domain enumeration of a target organisation, and you feed Naabu an input file of domain or subdomain it will resolve them and only scan unique IP addresses, so you are not wasting time and resources scanning the same target IP address twice. 

![Naabu Cheat Sheet](/img/naabu-command-cheat-sheet.jpg)

## What does Naabu do:

* Host discovery
* Automatic IP Deduplication for DNS port scan
* Port discovery / enumeration
* SYN/CONNECT/UDP probe based scanning
* Passive port scanning via Shodan
* Performs IPv4/IPv6 port scanning
* Can be configured to call Nmap to run NSE scripts post port detection
* Multiple input support - STDIN/HOST/IP/CIDR/ASN
* Multiple output format support - JSON/TXT/STDOUT

## Download & Install Naabu

You can obtain Naabu via the [Project Discovery Github](https://github.com/projectdiscovery/naabu).  

### Naabu Linux Install 

```
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
```

### Kali

Kali has a package for Naabu (caveat, it may not be the latest version):

```
sudo apt install naabu
```


<div class="mobile-side-scroller">
  <h2>Naabu File Input Options</h2>
  <p>Naabu input options, allowing Naabu to read and proccess data from input files. </p>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-host string[]</code></td>
      <td>hosts to scan ports for (comma-separated)</td>
    </tr>
    <tr>
      <td><code>-list, -l string</code></td>
      <td>list of hosts to scan ports (file)</td>
    </tr>
    <tr>
      <td><code>-exclude-hosts, -eh string</code></td>
      <td>hosts to exclude from the scan (comma-separated)</td>
    </tr>
    <tr>
      <td><code>-exclude-file, -ef string</code></td>
      <td>list of hosts to exclude from scan (file)</td>
    </tr>
  </table>
  <h2>Naabu Port Options</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-port, -p string</code></td>
      <td>ports to scan (80,443, 100-200</td>
    </tr>
    <tr>
      <td><code>-top-ports, -tp string</code></td>
      <td>top ports to scan (default 100)</td>
    </tr>
    <tr>
      <td><code>-exclude-ports, -ep string</code></td>
      <td>ports to exclude from scan (comma-separated)</td>
    </tr>
    <tr>
      <td><code>-ports-file, -pf string</code></td>
      <td>list of ports to exclude from scan (file)</td>
    </tr>
    <tr>
      <td><code>-exclude-cdn, -ec</code></td>
      <td>skip full port scans for CDN's (only checks for 80,443)</td>
    </tr>
  </table>
  <h2>Nabu Rate Limiting</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-c int</code></td>
      <td>general internal worker threads (default 25)</td>
    </tr>
    <tr>
      <td><code>-rate int</code></td>
      <td>packets to send per second (default 1000)</td>
    </tr>
  </table>
  <h2>Naabu Scan Output Options</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-o, -output string</code></td>
      <td>file to write output to (optional)</td>
    </tr>
    <tr>
      <td><code>-json</code></td>
      <td>write output in JSON lines format</td>
    </tr>
    <tr>
      <td><code>-csv</code></td>
      <td>write output in csv format</td>
    </tr>
  </table>
  <h2>Naabu Configuration Options</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-scan-all-ips, -sa</code></td>
      <td>scan all the IP's associated with DNS record</td>
    </tr>
    <tr>
      <td><code>-scan-type, -s string</code></td>
      <td>type of port scan (SYN/CONNECT) (default "s")</td>
    </tr>
    <tr>
      <td><code>-source-ip string</code></td>
      <td>source ip</td>
    </tr>
    <tr>
      <td><code>-interface-list, -il</code></td>
      <td>list available interfaces and public ip</td>
    </tr>
    <tr>
      <td><code>-interface, -i string</code></td>
      <td>network Interface to use for port scan</td>
    </tr>
    <tr>
      <td><code>-nmap</code></td>
      <td>invoke nmap scan on targets (nmap must be installed) - Deprecated</td>
    </tr>
    <tr>
      <td><code>-nmap-cli string</code></td>
      <td>nmap command to run on found results (example: -nmap-cli 'nmap -sV')</td>
    </tr>
    <tr>
      <td><code>-r string</code></td>
      <td>list of custom resolver dns resolution (comma separated or from file)</td>
    </tr>
    <tr>
      <td><code>-proxy string</code></td>
      <td>socks5 proxy</td>
    </tr>
    <tr>
      <td><code>-resume</code></td>
      <td>resume scan using resume.cfg</td>
    </tr>
    <tr>
      <td><code>-stream</code></td>
      <td>stream mode (disables resume, nmap, verify, retries, shuffling, etc)</td>
    </tr>
  </table>
  <h2>Naabu Optimization Options</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-retries int</code></td>
      <td>number of retries for the port scan (default 3)</td>
    </tr>
    <tr>
      <td><code>-timeout int</code></td>
      <td>millisecond to wait before timing out (default 1000)</td>
    </tr>
    <tr>
      <td><code>-warm-up-time int</code></td>
      <td>time in seconds between scan phases (default 2)</td>
    </tr>
    <tr>
      <td><code>-ping</code></td>
      <td>ping probes for verification of host</td>
    </tr>
    <tr>
      <td><code>-verify</code></td>
      <td>validate the ports again with TCP verification</td>
    </tr>
  </table>
  <h2>Naabu Debug Options</h2>
  <table>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>-debug</code></td>
      <td>display debugging information</td>
    </tr>
    <tr>
      <td><code>-verbose, -v</code></td>
      <td>display verbose output</td>
    </tr>
    <tr>
      <td><code>-no-color, -nc</code></td>
      <td>disable colors in CLI output</td>
    </tr>
    <tr>
      <td><code>-silent</code></td>
      <td>display only results in output</td>
    </tr>
    <tr>
      <td><code>-version</code></td>
      <td>display version of naabu</td>
    </tr>
    <tr>
      <td><code>-stats</code></td>
      <td>display stats of the running scan</td>
    </tr>
    <tr>
      <td><code>-si, -stats-interval</code></td>
      <td>int number of seconds to wait between showing a statistics update (default 5)</td>
    </tr>
  </table>
</div>


## Naabu Example Command Options

The following are real world examples of Naabu commands.

### Naabu Scan All Ports

{% highlight bash %}

naabu -p 0-65535

{% endhighlight %}

## Naabu Input File, Fast Scan + Verify Port 21

{% highlight bash %}
cat 21.txt | ~/go/bin/naabu -verify -ec -rate 9000 -retries 1 -p 21 -warm-up-time 0 -c 50 -silent -o ftp.txt
{% endhighlight %}

## Naabu Fast Scan, Verify, Nmap Services 

Naabu input file, scan all ports, output to text, fast scan, verify open ports, use Nmap to perform service enumeration 

{% highlight bash %}
naabu -list subdomains.txt -verify -ec -rate 9000 -retries 1 -p 0-65535 -warm-up-time 0 -c 50 -nmap-cli "nmap -sV -oG nmap-naabu-out" -silent -o naabu-full.txt
{% endhighlight %}

If you found this Naabu cheat sheet useful, please share it below. 
