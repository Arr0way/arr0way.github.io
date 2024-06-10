---
layout: blog_item
title:  "Nmap Cheat Sheet: Commands, Flags, Switches & Examples (2024)"
date:   2024-06-09 10:37:10
author: Arr0way
description: 'Nmap Cheat Sheet, learn Nmap commands, flags & real world exmaples allowing you to leverage the OG portscanner.'
categories: [cheat-sheet]
tags:
- 'Penetration Testing'
- 'Enumeration'
- host-enum
---


The following NMAP cheat sheet aims to explain what Nmap is, what it does, and how to use it by providing NMAP command examples in a cheat sheet style documentation format.

Orignal Published Date: 11th December 2014

## What is Nmap?

**Nmap** (network mapper), the god of port scanners used for network discovery and the basis for most security enumeration during the initial stages of a [penetration test](/penetration-testing/). The tool was written and maintained by Fyodor AKA Gordon Lyon.

[Nmap](http://nmap.org) displays exposed services on a target machine along with other useful information such as the verion and OS detection.

Nmap has made twelve movie appearances, including The Matrix Reloaded, Die Hard 4, Girl With the Dragon Tattoo, and The Bourne Ultimatum.


![Nmap Trinity](/img/nmap-trinity.png)

* list element with functor item
{:toc}

## What does Nmap do:

* Host discovery
* Port discovery / enumeration
* Service discovery
* Operating system version detection
* Hardware (MAC) address detection
* Service version detection
* Vulnerability / exploit detection, using Nmap scripts (NSE)
* Nmap IDS / Portscan Detection & Scan Time Optimisation 

## Download & Install Nmap

Nmap can be downloaded from [nmap.org](https://nmap.org/), however commonly Nmap is installed via your Linux distributions package manager: 

### Debian / Ubuntu / Kali

How to Install NMAP on Ubuntu, Debian, Kali or other Linux systems using the APT package manager.

{% highlight bash %}
apt install nmap
{% endhighlight %}

### Nmap RHEL / Fedora

How to Install NMAP on RHEL, Fedora, CentOS, Rocky Linux or other Linux systems using the DNF package manager.

{% highlight bash %}
dnf install nmap
{% endhighlight %}

### Nmap Windows

Download Nmap for Windows and install: [Nmap for Windows](https://nmap.org/download#windows) 

### Nmap MacOS

How to install nmap on MacOS using Brew.

{% highlight bash %}
brew install nmap
{% endhighlight %}

## Nmap Commands 

Basic Nmap scanning command examples, often used at the first stage of enumeration.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>nmap -sP 10.0.0.0/24</code></p>
      </td>
      <td>
            <p>Nmap scan the network, listing machines that respond to ping.</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>nmap -p 1-65535 -sV -sS -T4 target</code></p>
      </td>
      <td>
            <p>A full TCP port scan using with service version detection - <code>T1-T5</code> is the speed of the scan.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -v -sS -A -T4 target</code></p>
      </td>
      <td>
            <p>Prints verbose output, runs stealth syn scan, T4 timing, OS and version detection + traceroute and scripts against target services.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nmap -v -sS -A -T5 target</code></p>
      </td>
      <td>
            <p>Prints verbose output, runs stealth syn scan, T5 timing, OS and version detection + traceroute and scripts against target services.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -v -sV -O -sS -T5 target</code></p>
      </td>
      <td>
            <p>Prints verbose output, runs stealth syn scan, T5 timing, OS and version detection.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -v -p 1-65535 -sV -O -sS -T4 target</code></p>
      </td>
      <td>
           <p>Prints verbose output, runs stealth syn scan, T4 timing, OS and version detection + full port range scan.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -v -p 1-65535 -sV -O -sS -T5 target</code></p>
      </td>
      <td>
           <p>Prints verbose output, runs stealth syn scan, T5 timing, OS and version detection + full port range scan.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

<div class="note info">
  <h5>Agressive scan timings are faster, but could yeild inaccurate results!</h5>
  <p>T5 uses very aggressive scan timings and could lead to missed ports, T3-4 is a better compromise if you need fast results (depending on if local network or remote).</p>
</div>

### Nmap scan from file

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>nmap -iL ip-addresses.txt</code></p>
      </td>
      <td>
            <p>Scans a list of IP addresses, you can add options before / after.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Nmap Scan all Ports

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>nmap -p- target</code></p>
      </td>
      <td>
            <p>Nmap scan all ports, a full scan of all TCP ports on a target.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>


### Nmap output formats

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>

      <tr>
      <td>
        <p><code>nmap -sS -sV -T5 10.0.1.99 -oA output-all-formats</code></p>
      </td>
      <td>
            <p>nmap output to all formats. </p>
      </td>
    </tr>
      <tr>
      <td>
        <p><code>nmap -sV -p 139,445 -oG grep-output.txt 10.0.1.0/24</code></p>
      </td>
      <td>
            <p>Outputs "grepable" output to a file, in this example Netbios servers.</p>
            <p>E.g, The output file could be grepped for "Open".</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -sS -sV -T5 10.0.1.99 --webxml -oX -<br> | xsltproc --output file.html -</code></p>
      </td>
      <td>
            <p>Export nmap output to HTML report. </p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Nmap Netbios Examples

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>nmap -sV -v -p 139,445 10.0.0.1/24</code></p>
      </td>
      <td>
            <p>Find all Netbios servers on subnet</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -sU --script nbstat.nse -p 137 target</code></p>
      </td>
      <td>
            <p>Nmap display Netbios name</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap --script-args=unsafe=1 --script <br> smb-check-vulns.nse -p 445 target</code></p>
      </td>
      <td>
            <p>Nmap check if Netbios servers are vulnerable to MS08-067</p>
      </td>
    </tr>

      </tbody>
</table>
</div>

<div class="note warning">
  <h5>--script-args=unsafe=1 has the potential to crash servers / services</h5>
  <p>Becareful when running this command.</p>
</div>

### Nmap Nikto Scan

Nmap + [Nikto](/blog/nikto-cheat-sheet/) scanning for specific discovered HTTP ports.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>nmap -p80 10.0.1.0/24 -oG - | nikto.pl -h -</code></p>
      </td>
      <td>
            <p>Scans for http servers on port 80 and pipes into Nikto for scanning.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nmap -p80,443 10.0.1.0/24 -oG - | nikto.pl -h -</code></p>
      </td>
      <td>
            <p>Scans for http/https servers on port 80, 443 and pipes into Nikto for scanning.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

## Nmap Cheatsheet

### Target Specification

Nmap allows hostnames, IP addresses, subnets.

Example:

{% highlight bash %}
blah.highon.coffee, nmap.org/24, 192.168.0.1; 10.0.0-255.1-254
{% endhighlight %}

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-iL</code></p>
      </td>
      <td>
            <p>inputfilename: Input from list of hosts/networks</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-iR</code></p>
      </td>
      <td>
            <p>iterate hosts: Choose random targets from the input file</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--exclude</code></p>
      </td>
      <td>
            <p>host1[,host2][,host3],... : Exclude hosts/networks</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--excludefile</code></p>
      </td>
      <td>
            <p>exclude_file: nmap exclude hosts list from file</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Host Discovery

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-sL</code></p>
      </td>
      <td>
            <p>List Scan - simply list targets to scan</p>
      </td>
    </tr>   

    <tr>
      <td>
        <p><code>-sn</code></p>
      </td>
      <td>
            <p>Nmap ping scan / sweep - runs a nmap network scan, with port scanning disabled</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-Pn</code></p>
      </td>
      <td>
            <p>Treat all hosts as online -- skip host discovery</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-PS/PA/PU/PY[portlist]</code></p>
      </td>
      <td>
            <p>TCP SYN/ACK, UDP or SCTP discovery to given ports. Allows you to specify a specific port nmap uses to verify a host is up e.g., -PS22 (by default nmap sends to a bunch of common ports, this allows you to be specific)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-PE/PP/PM</code></p>
      </td>
      <td>
            <p>ICMP echo, timestamp, and netmask request discovery probes</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-PO[protocol list]</code></p>
      </td>
      <td>
            <p>IP Protocol Ping</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-n/-R</code></p>
      </td>
      <td>
            <p>Never do DNS resolution/Always resolve [default: sometimes]</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Scan Techniques

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-sS<br>-sT<br>-sA<br>-sW<br>-sM</code></p>
      </td>
      <td>
            <p>TCP SYN scan<br>Connect scan<br>ACK scan<br>Window scan<br>Maimon scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-sU</code></p>
      </td>
      <td>
            <p>UDP Scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-sN<br>-sF<br>-sX</code></p>
      </td>
      <td>
            <p>TCP Null scan<br>FIN scan<br>Xmas scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--scanflags</code></p>
      </td>
      <td>
            <p>Customize TCP scan flags</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-sI zombie host[:probeport]</code></p>
      </td>
      <td>
            <p>Idle scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-sY<br>-sZ</code></p>
      </td>
      <td>
            <p>SCTP INIT scan<br>COOKIE-ECHO scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-sO</code></p>
      </td>
      <td>
            <p>IP protocol scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code> -b "FTP relay host"</code></p>
      </td>
      <td>
            <p>FTP bounce scan</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Port Specification and Scan Order

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-p</code></p>
      </td>
      <td>
            <p>Specify ports, e.g. -p80,443 or -p1-65535</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-p U:PORT</code></p>
      </td>
      <td>
            <p>Scan UDP ports with Nmap, e.g. -p U:53</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-F</code></p>
      </td>
      <td>
            <p>Fast mode, scans fewer ports than the default scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-r</code></p>
      </td>
      <td>
            <p>Scan ports consecutively - don't randomize</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--top-ports "number"</code></p>
      </td>
      <td>
            <p>Scan "number" most common ports</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--port-ratio "ratio"</code></p>
      </td>
      <td>
            <p>Scan ports more common than "ratio"</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Service Version Detection

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-sV</code></p>
      </td>
      <td>
            <p>Probe open ports to determine service/version info</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--version-intensity "level"</code></p>
      </td>
      <td>
            <p>Set from 0 (light) to 9 (try all probes)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--version-light</code></p>
      </td>
      <td>
            <p>Limit to most likely probes (intensity 2)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--version-all</code></p>
      </td>
      <td>
            <p>Try every single probe (intensity 9)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--version-trace</code></p>
      </td>
      <td>
            <p>Show detailed version scan activity (for debugging)</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Script Scan

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-sC</code></p>
      </td>
      <td>
            <p>equivalent to --script=default</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--script="Lua scripts"</code></p>
      </td>
      <td>
            <p>"Lua scripts" is a comma separated list of
           directories, script-files or script-categories</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--script-args=n1=v1,[n2=v2,...]</code></p>
      </td>
      <td>
            <p>provide arguments to scripts</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-script-args-file=filename</code></p>
      </td>
      <td>
            <p>provide NSE script args in a file</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--script-trace</code></p>
      </td>
      <td>
            <p>Show all data sent and received</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--script-updatedb</code></p>
      </td>
      <td>
            <p>Update script database</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--script-help="Lua scripts"</code></p>
      </td>
      <td>
            <p>Show help about scripts</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### OS Detection

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-O</code></p>
      </td>
      <td>
            <p>Enable OS Detection</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--osscan-limit</code></p>
      </td>
      <td>
            <p>Limit OS detection to promising targets</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--osscan-guess</code></p>
      </td>
      <td>
            <p>Guess OS more aggressively</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Timing and Performance

<p>Options which take TIME are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).</p>

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-T 0-5</code></p>
      </td>
      <td>
            <p>Set timing template - higher is faster (less accurate)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--min-hostgroup SIZE <br>--max-hostgroup SIZE</code></p>
      </td>
      <td>
            <p>Parallel host scan group sizes</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--min-parallelism NUMPROBES <br>--max-parallelism NUMPROBES</code></p>
      </td>
      <td>
            <p>Probe parallelization</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--min-rtt-timeout TIME<br>--max-rtt-timeout TIME<br>--initial-rtt-timeout TIME</code></p>
      </td>
      <td>
            <p>Specifies probe round trip time</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--max-retries TRIES</code></p>
      </td>
      <td>
            <p>Caps number of port scan probe retransmissions</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--host-timeout TIME</code></p>
      </td>
      <td>
            <p>Give up on target after this long</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--scan-delay TIME <br>--max-scan-delay TIME</code></p>
      </td>
      <td>
            <p>Adjust delay between probes</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--min-rate NUMBER</code></p>
      </td>
      <td>
            <p>Send packets no slower than NUMBER per second</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--max-rate NUMBER</code></p>
      </td>
      <td>
            <p>Send packets no faster than NUMBER per second</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Firewalls IDS Evasion and Spoofing

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-f; --mtu VALUE</code></p>
      </td>
      <td>
            <p>Fragment packets (optionally w/given MTU)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-D decoy1,decoy2,ME</code></p>
      </td>
      <td>
            <p>Cloak a scan with decoys</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-S IP-ADDRESS</code></p>
      </td>
      <td>
            <p>Spoof source address</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-e IFACE</code></p>
      </td>
      <td>
            <p>Use specified interface</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-g PORTNUM<br>--source-port PORTNUM</code></p>
      </td>
      <td>
            <p>Use given port number</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--proxies url1,[url2],...</code></p>
      </td>
      <td>
            <p>Relay connections through HTTP / SOCKS4 proxies</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--data-length NUM</code></p>
      </td>
      <td>
            <p>Append random data to sent packets</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--ip-options OPTIONS</code></p>
      </td>
      <td>
            <p>Send packets with specified ip options</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--ttl VALUE</code></p>
      </td>
      <td>
            <p>Set IP time to live field</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--spoof-mac ADDR/PREFIX/VENDOR</code></p>
      </td>
      <td>
            <p>Spoof NMAP MAC address</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--badsum</code></p>
      </td>
      <td>
            <p>Send packets with a bogus TCP/UDP/SCTP checksum</p>
      </td>
    </tr>

      </tbody>
</table>
</div>

### Nmap Scan Output File Options

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-oN</code></p>
      </td>
      <td>
            <p>Output Normal</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-oX</code></p>
      </td>
      <td>
            <p>Output to XML</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-oS</code></p>
      </td>
      <td>
            <p>Script Kiddie / 1337 speak... sigh</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-oG</code></p>
      </td>
      <td>
            <p>Output greppable - easy to grep nmap output</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-oA BASENAME</code></p>
      </td>
      <td>
            <p>Output in the three major formats at once</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>-v</code></p>
      </td>
      <td>
            <p>Increase verbosity level use -vv or more for greater effect</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>-d</code></p>
      </td>
      <td>
            <p>Increase debugging level use -dd or more for greater effect</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--reason</code></p>
      </td>
      <td>
            <p>Display the reason a port is in a particular state</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--open</code></p>
      </td>
      <td>
            <p>Only show open or possibly open ports</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--packet-trace</code></p>
      </td>
      <td>
            <p>Show all packets sent / received</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--iflist</code></p>
      </td>
      <td>
            <p>Print host interfaces and routes for debugging</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--log-errors</code></p>
      </td>
      <td>
            <p>Log errors/warnings to the normal-format output file</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--append-output</code></p>
      </td>
      <td>
            <p>Append to rather than clobber specified output files</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--resume FILENAME</code></p>
      </td>
      <td>
            <p>Resume an aborted scan</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--stylesheet PATH/URL</code></p>
      </td>
      <td>
            <p>XSL stylesheet to transform XML output to HTML</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--webxml</code></p>
      </td>
      <td>
            <p>Reference stylesheet from Nmap.Org for more portable XML</p>
      </td>
    </tr>
    <tr>
    <td>
        <p><code>--no-stylesheet</code></p>
      </td>
      <td>
            <p>Prevent associating of XSL stylesheet w/XML output</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Misc Nmap Options

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>-6</code></p>
      </td>
      <td>
            <p>Enable IPv6 scanning</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-A</code></p>
      </td>
      <td>
            <p>Enable OS detection, version detection, script scanning, and traceroute</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--datedir DIRNAME</code></p>
      </td>
      <td>
            <p>Specify custom Nmap data file location</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--send-eth<br>--send-ip</code></p>
      </td>
      <td>
            <p>Send using raw ethernet frames or IP packets</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>--privileged</code></p>
      </td>
      <td>
            <p>Assume that the user is fully privileged</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>--unprivileged</code></p>
      </td>
      <td>
            <p>Assume the user lacks raw socket privileges</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>-V</code></p>
      </td>
      <td>
            <p>Show nmap version number</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>-h</code></p>
      </td>
      <td>
            <p>Show nmap help screen</p>
      </td>
    </tr>

      </tbody>
</table>
</div>

## Nmap Enumeration Command Examples

The following are real world examples of Nmap enumeration.

### Enumerating Netbios

The following example enumerates Netbios on the target networks, the same process can be applied to other services by modifying ports / NSE scripts.

Detect all exposed Netbios servers on the subnet.

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Nmap find exposed Netbios servers</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap -sV -v -p 139,445 10.0.1.0/24</span>
        </p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2014-12-11 21:26 GMT<br></span>
          <span class="output">Nmap scan report for nas.decepticons 10.0.1.12<br></span>
          <span class="output">Host is up (0.014s latency).<br></span>
          <span class="output"><br></span>
          <span class="output">PORT        STATE     SERVICE         VERSION<br></span>
          <span class="output">139/tcp     open      netbios-ssn     Samba smbd 3.X (workgroup: MEGATRON)<br></span>
          <span class="output">445/tcp     open      netbios-ssn     Samba smbd 3.X (workgroup: MEGATRON)<br></span>
          <span class="output"><br></span>
          <span class="output">Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .<br></span>
          <span class="output"><br></span>
          <span class="output">Nmap done: 256 IP addresses (1 hosts up) scanned in 28.74 seconds<br></span>
        </p>
      </div>
    </div>
</section>

### Nmap find Netbios name.

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Nmap find exposed Netbios servers</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap -sU --script nbstat.nse -p 137 10.0.1.12</span>
        </p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2014-12-11 21:26 GMT<br></span>
          <span class="output">Nmap scan report for nas.decepticons 10.0.1.12<br></span>
          <span class="output">Host is up (0.014s latency).<br></span>
          <span class="output"><br></span>
          <span class="output">PORT        STATE     SERVICE         VERSION<br></span>
          <span class="output">137/udp open  netbios-ns<br></span>
          <span class="output"><br></span>
          <span class="output">Host script results:<br></span>
          <span class="output">|_nbstat: NetBIOS name: STARSCREAM, NetBIOS user: unknown, NetBIOS MAC: unknown (unknown)
</span>
          <span class="output"><br></span>
          <span class="output">Nmap done: 256 IP addresses (1 hosts up) scanned in 28.74 seconds<br></span>
        </p>
      </div>
    </div>
</section>

### NMAP Netbios MS08-067

How to scan a target and identify if it is vulnerable to MS08-067

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Nmap check MS08-067</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap --script-args=unsafe=1 --script smb-check-vulns.nse -p 445</span>
          <span>10.0.0.1</span>
        </p>
          <span class="output"><br></span>
          <span class="output">Nmap scan report for ie6winxp.decepticons (10.0.1.1)<br></span>
          <span class="output">Host is up (0.00026s latency).<br></span>
          <span class="output">PORT    STATE SERVICE<br></span>
          <span class="output">445/tcp open  microsoft-ds<br></span>
          <span class="output">Host script results:<br></span>
          <span class="output">| smb-check-vulns:<br></span>
          <span class="output">|   MS08-067: VULNERABLE<br></span>
          <span class="output">|   Conficker: Likely CLEAN<br></span>
          <span class="output">|   regsvc DoS: NOT VULNERABLE<br></span>
          <span class="output">|   SMBv2 DoS (CVE-2009-3103): NOT VULNERABLE<br></span>
          <span class="output">|_  MS07-029: NO SERVICE (the Dns Server RPC service is inactive)<br></span>
          <span class="output">Nmap done: 1 IP address (1 host up) scanned in 5.45 seconds<br></span>
        </p>
      </div>
    </div>
</section>

The information gathered during the enumeration indicates the target is vulnerable to MS08-067, exploitation will confirm if it's vulnerable to MS08-067.

## Nmap Scan Tuning & Optimisation 

### Nmap Rate

To speed up your scan increase the rate, be aware that setting a high rate value will result in a less accurate scan. 

{% highlight bash %}
--max-rate 
--min-rate 
{% endhighlight %}


### NMAP Parallelism 

The maximum or minimum amount of parallel tasks scanned at the same time (in parallel).

TIP: If you have an basic IDS / portscan detection blocking your scans you could lower the --min-parallelism in an attempt to reduce the number of concurrent connections 

{% highlight bash %}
--min-parallelism
--max-parallelism
{% endhighlight %}

### NMAP Host Group Sizes 

The number of hosts scanned at the same time, Note: if you are writing output to a file e.g., -oA you will need to wait for the host group to complete scanning before the nmap output will be written to the file. Therefore if you get a lagging host you will may end up waiting a while for the output file, which brings us on to... host timeout.

{% highlight bash %}
--min-hostgroup 
--max-hostgroup 
{% endhighlight %}

### NMAP Host Timeout 

Nmap allows you to specify the timeout, which is the length of time it waits before giving up on the target. Be careful setting this super low, as you may end up with inaccurate results. 

The following example would giveup after 50 seconds. 

{% highlight bash %}
--host-timeout 50 
{% endhighlight %}

### NMAP Scan Delay 

An extremely useful option to defeat basic port scan detection (SOHO devices and some IDS) that essentially monitor and block X amount of connects per second (syn flood etc). In short the scan timing can be optimised to allow nmap to bypass firewall detection mechanism. 

{% highlight bash %}
--scan-delay 5s 
{% endhighlight %}

For example if you know you can get away with 2 req/sec without getting blacklisted then  you could use: 

{% highlight bash %}
--scan-delay 1.2
{% endhighlight %}

*added 200ms for a buffer* 

### NMAP Disable DNS Lookups 

Assuming you do not want domain names being looked up, use the ```-n``` flag to dissable resolution and speed up the scan. 

#### NMAP Black List Detection? 

1. It ussally takes and extemely long time to complete 
2. Droppped probes nmap will increase the timeout, but it's likely you are already black listed 
3. To confirm, recheck a port that you know was open before 

As far as I know there is no way of detecting for black listing within nmap natively. 

### NMAP Optimising Portscans for Targets 

Once you have identified a target firewall / IDS you can look up the default settings for the portscan black list by reading the manual and use the nmap command switches above to obtain the best performance without getting black listed.


If you found this Nmap cheat sheet useful, please share it below. 

## Nmap FAQ 

### What is Nmap Used for? 

Nmap (Network Mapper) is a free and open source tool for discovering and auditing networks. 
Many system and network administrators use Nmap to perform network inventories, asset management , manage service updating schedules, and monitor host or service availability.

### Is Nmap Illegal?

When used properly, Nmap could help you protect your network from intruders. 
But used inappropriately (e.g., maliciously, and/or without permission from the target), Nmap could (in rare cases) get you sued, fired, expelled, jailed, or banned by your ISP.

### Is Nmap a Vulnerability Scanner 

Nmap is a port scanner or network mapper, the tool identified if a system exists on the network or IP address you provide. However, NSE Scripts then extend the functionality of Nmap by allowing additional host checkes that provide nmap vulnerability scanning functionality to the tool. 

### Why do hackers use Nmap?

Attackers or hackers may use Nmap to identify targets and the exposed ports on a target in an effort to idenitfy potential attack surface to perform addtional security testing against. 

### Nmap Download

You can download nmap from [https://nmap.org/download](https://nmap.org/download) or a common option would be to install via your Linux distributions package manager or Brew on macos. 

### Nmap Scripts List 

You can find a lot of the current Nmap scripts list at [https://nmap.org/nsedoc/scripts/](https://nmap.org/nsedoc/scripts/) this list is actively updated by the Nmap project. 


## Document Changelog 

- **Original Post Date:** 13/12/2014
- **Last Updated:** 10/06/2024 (10th of June 2024)
- **Author:** Arr0way 
- **Notes:** Checked syntax was current for latest version of Nmap + added additional content.


<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is Nmap Used for?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Nmap (Network Mapper) is a free and open source tool for discovering and auditing networks. Many system and network administrators use Nmap to perform network inventories, asset management, manage service updating schedules, and monitor host or service availability."
      }
    },
    {
      "@type": "Question",
      "name": "Is Nmap Illegal?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "When used properly, Nmap could help you protect your network from intruders. But used inappropriately (e.g., maliciously, and/or without permission from the target), Nmap could (in rare cases) get you sued, fired, expelled, jailed, or banned by your ISP."
      }
    },
    {
      "@type": "Question",
      "name": "Is Nmap a Vulnerability Scanner?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Nmap is a port scanner or network mapper, the tool identifies if a system exists on the network or IP address you provide. However, NSE Scripts then extend the functionality of Nmap by allowing additional host checks that provide Nmap vulnerability scanning functionality to the tool."
      }
    },
    {
      "@type": "Question",
      "name": "Why do hackers use Nmap?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Attackers or hackers may use Nmap to identify targets and the exposed ports on a target in an effort to identify potential attack surface to perform additional security testing against."
      }
    },
    {
      "@type": "Question",
      "name": "Nmap Download",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "You can download Nmap from <a href=\"https://nmap.org/download\">https://nmap.org/download</a> or a common option would be to install via your Linux distribution's package manager or Brew on macOS."
      }
    },
    {
      "@type": "Question",
      "name": "Nmap Scripts List",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "You can find a lot of the current Nmap scripts list at <a href=\"https://nmap.org/nsedoc/scripts/\">https://nmap.org/nsedoc/scripts/</a> this list is actively updated by the Nmap project."
      }
    }
  ]
}
</script>
