---
layout: blog_item
title:  "nbtscan Cheat Sheet"
date:   2015-03-29 14:37:10
author: Arr0way
description: 'nbtscan install, examples and nbtscan commands cheatsheet'
categories: [cheat-sheet]
tags:
- Windows
- SMB
- smb-enum
- Tools
---

**[nbtscan](http://www.unixwiz.net/tools/nbtscan.html)** is a command line tool that finds exposed NETBIOS nameservers, it's a good first step for finding open shares.

<div class="note tip">
  <h5>Don't use the version of nbtscan that ships with KALI</h5>
  <p>Grab nbtscan from the above link and build it from source, this version tends to find more information</p>
</div>

### Compile nbtscan on KALI

{% highlight bash %}
root@kali:~/nbtscan# wget http://www.unixwiz.net/tools/nbtscan-source-1.0.35.tgz
root@kali:~/nbtscan# tar -xvzf nbtscan-source-1.0.35.tgz
root@kali:~/nbtscan# make
root@kali:~/nbtscan# ./nbtscan
nbtscan 1.0.35 - 2008-04-08 - http://www.unixwiz.net/tools/

usage: ./nbtscan [options] target [targets...]

   Targets are lists of IP addresses, DNS names, or address
   ranges. Ranges can be in /nbits notation ("192.168.12.0/24")
   or with a range in the last octet ("192.168.12.64-97")
{% endhighlight %}

### nbtscan Cheat Sheet

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
        <p><code>nbtscan -v</code></p>
      </td>
      <td>
            <p>Displays the nbtscan version</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>nbtscan -f target(s)</code></p>
      </td>
      <td>
            <p>This shows the full NBT resource record responses for each machine scanned, not a one line summary, use this options when scanning a single host</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nbtscan -O file-name.txt target(s)</code></p>
      </td>
      <td>
            <p>Sends output to a file</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nbtscan -H</code></p>
      </td>
      <td>
            <p>Generate an HTTP header</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nbtscan -P</code></p>
      </td>
      <td>
            <p>Generate Perl hashref output, which can be loaded into an existing program for easier processing, much easier than parsing text output</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nbtscan -V</code></p>
      </td>
      <td>
           <p>Enable verbose mode</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nbtscan -n</code></p>
      </td>
      <td>
           <p>Turns off this inverse name lookup, for hanging resolution</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nbtscan -p PORT target(s)</code></p>
      </td>
      <td>
           <p>This allows specification of a UDP port number to be used as the source in sending a query</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nbtscan -m</code></p>
      </td>
      <td>
           <p>Include the MAC (aka "Ethernet") addresses in the response, which is already implied by the -f option.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>
