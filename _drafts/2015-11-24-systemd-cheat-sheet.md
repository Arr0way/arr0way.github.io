---
layout: blog_item
title:  "Systemd Cheat Sheet"
date:   2015-03-29 14:37:10
author: Arr0way
description: 'systemd, examples and systemd commands cheat sheet'
categories: [Cheat-Sheet]
tags:
- linux-enum
- RHEL
- CentOS
---

Systemd is now the default in RHEL / CentOS 7, the following post is a cheat
sheet for systemd commands. 


<div class="note tip">
  <h5>Systemd is becoming the default on most distros</h5>
  <p>Systemd is becoming the default in many distros, <b>RHEL, CentOS, Ubuntu 15</b> 
  and it offers a single command to manage your system, instead of switching
  between <code>chkconfig</code> or running init scripts.</p>
</div>

## Systemd Service Commands 

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
        <p><code>systemctl stop service-name</code></p>
      </td>
      <td>
            <p>systemd stop running service</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>systemctl start service-name</code></p>
      </td>
      <td>
            <p>systemctl start service</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl restart service-name</code></p>
      </td>
      <td>
            <p>systemd restart running service</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>systemctl reload service-name</code></p>
      </td>
      <td>
            <p>reloads all config files for service</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl status service-name</code></p>
      </td>
      <td>
            <p>systemctl show if service is running</p> 
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl enable service-name</code></p>
      </td>
      <td>
           <p>systemctl start service at boot</p> 
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctrl disable service-name</code></p>
      </td>
      <td>
           <p>systemctl - disable service at boot</p> 
      </td>
    </tr>
    <tr>
      <td>
        <p><code>systemctl show service-name</code></p>
      </td>
      <td>
           <p>show systemctl service info</p> 
      </td>
    </tr>
    <tr>
      <td>
        <p><code>systemctl -H target command service-name</code></p>
      </td>
      <td>
           <p>run systemctl commands remotely</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

## Systemd Information Commands 

Systemd commands that show useful system information. 

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
        <p><code>systemctl list-dependencies</code></p>
      </td>
      <td>
            <p>show and units dependencies</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>systemctl list-sockets</code></p>
      </td>
      <td>
            <p>systemd list sockets and activities</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl list-jobs</code></p>
      </td>
      <td>
            <p>view active systemd jobs</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>systemctl list-unit-files</code></p>
      </td>
      <td>
            <p>systemctl list unit files and their states</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl list-units</code></p>
      </td>
      <td>
            <p>systemctl list default target (like run level)</p> 
      </td>
    </tr>

      </tbody>
</table>
</div>

## Changing System State 

systemd reboot, shutdown, default target etc 

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
        <p><code>systemctl reboot</code></p>
      </td>
      <td>
            <p>systemctl reboot the system</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>systemctl poweroff</code></p>
      </td>
      <td>
            <p>systemctl shutdown (power off the system)</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>systemctl emergency</code></p>
      </td>
      <td>
            <p>Put in emergency mode</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>systemctl default</code></p>
      </td>
      <td>
            <p>systemctl default mode</p>
      </td>
    </tr>

      </tbody>
</table>
</div>

##Systemctl Viewing Log Messages 

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
        <p><code>journalctl</code></p>
      </td>
      <td>
            <p>show all collected log messages</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>journalctl -u sshd.service</code></p>
      </td>
      <td>
            <p>see sshd service messages</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>journelctl -f</code></p>
      </td>
      <td>
            <p>follow messages as they appear</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>journelctl -k</code></p>
      </td>
      <td>
            <p>show kernel messages only</p>
      </td>
    </tr>

      </tbody>
</table>
</div>
