---
layout: blog_item
title:  "enum4linux Cheat Sheet"
date:   2015-03-29 00:58:10
author: Arr0way
description: 'enum4linux description, examples, cheatsheet and practical examples'
categories: [Cheat-Sheet]
tags:
- Windows
- SMB
- Tools
---

**[enum4linux](https://labs.portcullis.co.uk/tools/enum4linux/)** is an alternative to enum.exe on Windows, enum4linux is used to enumerate Windows and Samba hosts. 
 

**enum4linux in a nutshell**

* RID cycling (When RestrictAnonymous is set to 1 on Windows 2000)
* User listing (When RestrictAnonymous is set to 0 on Windows 2000)
* Listing of group membership information
* Share enumeration
* Detecting if host is in a workgroup or a domain
* Identifying the remote operating system
* Password policy retrieval (using polenum)

### enum4linux Cheat Sheet

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
        <p><code>enum4linux -v target-ip</code></p>
      </td>
      <td>
            <p>Verbose mode, shows the underlying commands being executed by enum4linux</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>enum4linux -a target-ip</code></p>
      </td>
      <td>
            <p>Do Everything, runs all options apart from dictionary based share name guessing</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>enum4linux -U target-ip</code></p>
      </td>
      <td>
            <p>Lists usernames, if the server allows it - (RestrictAnonymous = 0)</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>enum4linux -u administrator <br>-p password -U target-ip</code></p>
      </td>
      <td>
            <p>If you've managed to obtain credentials, you can pull a full list of users regardless of the RestrictAnonymous option</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>enum4linux -r target-ip</code></p>
      </td>
      <td>
            <p>Pulls usernames from the default RID range (500-550,1000-1050)</p> 
      </td>
    </tr>

    <tr>
      <td>
        <p><code>enum4linux -R 600-660 target-ip</code></p>
      </td>
      <td>
           <p>Pull usernames using a custom RID range</p> 
      </td>
    </tr>

    <tr>
      <td>
        <p><code>enum4linux -G target-ip</code></p>
      </td>
      <td>
           <p>Lists groups. if the server allows it, you can also specify username <code>-u</code> and password <code>-p</code></p> 
      </td>
    </tr>
    <tr>
      <td>
        <p><code>enum4linux -S target-ip</code></p>
      </td>
      <td>
           <p>List Windows shares, again you can also specify username <code>-u</code> and password <code>-p</code></p> 
      </td>
    </tr>
    <tr>
      <td>
        <p><code>enum4linux -s shares.txt target-ip</code></p>
      </td>
      <td>
           <p>Perform a dictionary attack, if the server doesn't let you retrieve a share list </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>enum4linux -o target-ip</code></p>
      </td>
      <td>
           <p>Pulls OS information using smbclient, this can pull the service pack version on some versions of Windows</p>
      </td>
    </tr>
    <td>
        <p><code>enum4linux -i target-ip</code></p>
      </td>
      <td>
           <p>Pull information about printers known to the remove device.</p>
      </td>
      </tbody>
</table>
</div>
