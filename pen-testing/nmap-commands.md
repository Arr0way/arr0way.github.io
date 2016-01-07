---
layout: docs
title: Nmap Commands 
overview: true
category: 'network enumeration'
tags:
- 'penetration testing'
---

## NMAP Commands

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
        <p><code>nmap -v -sS -A -T4 target</code></p>
      </td>
      <td>
            <p>Nmap verbose scan, runs syn stealth, T4 timing (should be ok on LAN), OS and service version info, traceroute and scripts against services</p>
      </td>
    </tr>

        <tr>
      <td>
        <p><code>nmap -v -sS -p 1-65535 -A -T4 target</code></p>
      </td>
      <td>
            <p>As above but scans all TCP ports (takes a lot longer)</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nmap -v -sU -sS -p 1-65535 -A -T4 target</code></p>
      </td>
      <td>
            <p>As above but scans all TCP ports and UDP scan (takes even longer)</p>
      </td>
    <tr>
      <td>
        <p><code>nmap -v -p 445 --script=smb-check-vulns <br>--script-args=unsafe=1 192.168.1.X</code></p>
      </td>
      <td>
            <p>Nmap script to scan for vulnerable SMB servers</p>
      </td>
    </tr>
      <td>
        <p><code>ls /usr/share/nmap/scripts/* | grep ftp</code></p>
      </td>
      <td>
            <p>Search nmap scripts for keywords</p>
      </td>
    </tr>
  </tbody>
  </table>
  </div>
