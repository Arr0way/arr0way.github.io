---
layout: blog_item
title:  "Linux Commands Cheat Sheet"
date:   2014-11-2 08:20:10
author: Arr0way
description: 'Linux Command Cheat Sheet and examples for penetration testing.'
categories: [cheat-sheet]
tags:
- 'Penetration Testing'
- 'linux-enum'
- 'Enumeration'
- 'linux-commands'
---

* list element with functor item
{:toc}

A collection of hopefully useful Linux Commands for pen testers, this is not a complete list but a collection of commonly used commands + syntax as a sort of "cheatsheet", this content will be constantly updated as I discover new awesomeness.

## Linux Penetration Testing Commands

The commands listed below are designed for local enumeration, typical commands a penetration tester would use during post exploitation or when performing command injection etc. See our cheat sheet for an in depth list of [pen testing tool commands](/blog/penetration-testing-tools-cheat-sheet/) and example usage. 

### Linux Network Commands

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
        <p><code>netstat -tulpn</code></p>
      </td>
      <td>
        <p>

          Show Linux network ports with process ID's (PIDs)

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>watch ss -stplu</code></p>
      </td>
      <td>
        <p>

          Watch TCP, UDP open ports in real time with socket summary.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>lsof -i</code></p>
      </td>
      <td>
        <p>

          Show established connections.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>macchanger -m MACADDR INTR</code></p>
      </td>
      <td>
        <p>
        Change MAC address on KALI Linux.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>ifconfig eth0 192.168.2.1/24</code></p>
      </td>
      <td>
        <p>

          Set IP address in Linux.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>ifconfig eth0:1 192.168.2.3/24</code></p>
      </td>
      <td>
        <p>

          Add IP address to existing network interface in Linux.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>ifconfig eth0 hw ether MACADDR</code></p>
      </td>
      <td>
        <p>

          Change MAC address in Linux using ifconfig.  

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>ifconfig eth0 mtu 1500</code></p>
      </td>
      <td>
        <p>

          Change MTU size Linux using ifconfig, change 1500 to your desired MTU.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>dig -x 192.168.1.1 </code></p>
      </td>
      <td>
        <p>

          Dig reverse lookup on an IP address.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>host 192.168.1.1 </code></p>
      </td>
      <td>
        <p>

          Reverse lookup on an IP address, in case dig is not installed.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>dig @192.168.2.2 domain.com -t AXFR</code></p>
      </td>
      <td>
        <p>

          Perform a DNS zone transfer using dig.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>host -l domain.com nameserver</code></p>
      </td>
      <td>
        <p>

          Perform a DNS zone transfer using host.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>nbtstat -A x.x.x.x</code></p>
      </td>
      <td>
        <p>

          Get hostname for IP address.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>ip addr add 192.168.2.22/24 dev eth0</code></p>
      </td>
      <td>
        <p>

          Adds a hidden IP address to Linux, does not show up when performing an ifconfig.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>tcpkill -9 host google.com</code></p>
      </td>
      <td>
        <p>

          Blocks access to google.com from the host machine.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>echo "1" > /proc/sys/net/ipv4/ip_forward</code></p>
      </td>
      <td>
        <p>

          Enables IP forwarding, turns Linux box into a router - handy for routing traffic through a box.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>echo "8.8.8.8" > /etc/resolv.conf</code></p>
      </td>
      <td>
        <p>

          Use Google DNS.

        </p>
      </td>
    </tr>


    </tbody>
</table>
</div>

### System Information Commands

Useful for local enumeration.

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
        <p><code>whoami</code></p>
      </td>
      <td>
        <p>

          Shows currently logged in user on Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>id</code></p>
      </td>
      <td>
        <p>

          Shows currently logged in user and groups for the user.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>last</code></p>
      </td>
      <td>
        <p>

          Shows last logged in users.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>mount</code></p>
      </td>
      <td>
        <p>

          Show mounted drives.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>df -h</code></p>
      </td>
      <td>
        <p>

          Shows disk usage in human readable output.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>echo "user:passwd" | chpasswd</code></p>
      </td>
      <td>
        <p>

          Reset password in one line.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>getent passwd</code></p>
      </td>
      <td>
        <p>

          List users on Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>strings /usr/local/bin/blah</code></p>
      </td>
      <td>
        <p>

          Shows contents of none text files, e.g. whats in a binary.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>uname -ar</code></p>
      </td>
      <td>
        <p>

          Shows running kernel version.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>PATH=$PATH:/my/new-path</code></p>
      </td>
      <td>
        <p>

          Add a new PATH, handy for local FS manipulation.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>history</code></p>
      </td>
      <td>
        <p>

          Show bash history, commands the user has entered previously.   

        </p>
      </td>
    </tr>


  </tbody>
</table>
</div>

#### Redhat / CentOS / RPM Based Distros

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
        <p><code>cat /etc/redhat-release</code></p>
      </td>
      <td>
        <p>

          Shows Redhat / CentOS version number.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>rpm -qa</code></p>
      </td>
      <td>
        <p>

          List all installed RPM's on an RPM based Linux distro.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>rpm -q --changelog openvpn</code></p>
      </td>
      <td>
        <p>

          Check installed RPM is patched against CVE,  grep the output for CVE.

        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>

#### YUM Commands

Package manager used by RPM based systems, you can pull some usefull information about installed packages and or install additional tools.

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
        <p><code>yum update</code></p>
      </td>
      <td>
        <p>

          Update all RPM packages with YUM, also shows whats out of date.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>yum update httpd</code></p>
      </td>
      <td>
        <p>

          Update individual packages, in this example HTTPD (Apache).

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>yum install package</code></p>
      </td>
      <td>
        <p>

          Install a package using YUM.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum --exclude=package kernel* update</code></p>
      </td>
      <td>
        <p>

          Exclude a package from being updates with YUM.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum remove package</code></p>
      </td>
      <td>
        <p>

          Remove package with YUM.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum erase package</code></p>
      </td>
      <td>
        <p>

          Remove package with YUM.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum list package</code></p>
      </td>
      <td>
        <p>

          Lists info about yum package.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum provides httpd</code></p>
      </td>
      <td>
        <p>

          What a packages does, e.g Apache HTTPD Server.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum info httpd</code></p>
      </td>
      <td>
        <p>

          Shows package info, architecture, version etc.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum localinstall blah.rpm</code></p>
      </td>
      <td>
        <p>

          Use YUM to install local RPM, settles deps from repo.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum deplist package</code></p>
      </td>
      <td>
        <p>

          Shows deps for a package.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum list installed | more</code></p>
      </td>
      <td>
        <p>

          List all installed packages.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum grouplist | more</code></p>
      </td>
      <td>
        <p>

          Show all YUM groups.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>yum groupinstall 'Development Tools'</code></p>
      </td>
      <td>
        <p>

          Install YUM group.

        </p>
      </td>
    </tr>

  </tbody>
</table>
</div>



#### Debian / Ubuntu / .deb Based Distros

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
        <p><code>cat /etc/debian_version</code></p>
      </td>
      <td>
        <p>

          Shows Debian version number.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>cat /etc/*-release</code></p>
      </td>
      <td>
        <p>

          Shows Ubuntu version number.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>dpkg -l</code></p>
      </td>
      <td>
        <p>

          List all installed packages on Debian / .deb based Linux distro.

        </p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Linux User Management

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
        <p><code>useradd new-user</code></p>
      </td>
      <td>
        <p>

          Creates a new Linux user.  

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>passwd username</code></p>
      </td>
      <td>
        <p>

          Reset Linux user password, enter just <code>passwd</code> if you are root.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>deluser username</code></p>
      </td>
      <td>
        <p>

          Remove a Linux user.

        </p>
      </td>
    </tr>
    </tbody>
</table>
</div>


### Linux Decompression Commands

How to extract various archives (tar, zip, gzip, bzip2 etc) on Linux and some other tricks for searching inside of archives etc.

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
        <p><code>unzip archive.zip</code></p>
      </td>
      <td>
        <p>

          Extracts zip file on Linux.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>zipgrep *.txt archive.zip</code></p>
      </td>
      <td>
        <p>

          Search inside a .zip archive.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>tar xf archive.tar</code></p>
      </td>
      <td>
        <p>

          Extract tar file Linux.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>tar xvzf archive.tar.gz</code></p>
      </td>
      <td>
        <p>

          Extract a tar.gz file Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>tar xjf archive.tar.bz2</code></p>
      </td>
      <td>
        <p>

          Extract a tar.bz2 file Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>tar ztvf file.tar.gz | grep blah</code></p>
      </td>
      <td>
        <p>

          Search inside a tar.gz file.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>gzip -d archive.gz</code></p>
      </td>
      <td>
        <p>

          Extract a gzip file Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>zcat archive.gz</code></p>
      </td>
      <td>
        <p>

          Read a gz file Linux without decompressing.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>zless archive.gz</code></p>
      </td>
      <td>
        <p>

          Same function as the <code>less</code> command for .gz archives.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>zgrep 'blah' /var/log/maillog*.gz</code></p>
      </td>
      <td>
        <p>

          Search inside .gz archives on Linux, search inside of compressed log files.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>vim file.txt.gz</code></p>
      </td>
      <td>
        <p>

          Use vim to read .txt.gz files (my personal favorite).

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>upx -9 -o output.exe input.exe</code></p>
      </td>
      <td>
        <p>

          UPX compress .exe file Linux.

        </p>
      </td>
    </tr>

    </tbody>
</table>
</div>


### Linux Compression Commands

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
        <p><code>zip -r file.zip /dir/*</code></p>
      </td>
      <td>
        <p>

          Creates a .zip file on Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>tar cf archive.tar files</code></p>
      </td>
      <td>
        <p>

          Creates a tar file on Linux.  

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>tar czf archive.tar.gz files</code></p>
      </td>
      <td>
        <p>

          Creates a tar.gz file on Linux.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>tar cjf archive.tar.bz2 files</code></p>
      </td>
      <td>
        <p>

          Creates a tar.bz2 file on Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>gzip file</code></p>
      </td>
      <td>
        <p>

          Creates a file.gz file on Linux.

        </p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Linux File Commands

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
        <p><code>df -h blah</code></p>
      </td>
      <td>
        <p>

          Display size of file / dir Linux.   

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>diff file1 file2</code></p>
      </td>
      <td>
        <p>

          Compare / Show differences between two files on Linux.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>md5sum file</code></p>
      </td>
      <td>
        <p>

          Generate MD5SUM Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>md5sum -c blah.iso.md5</code></p>
      </td>
      <td>
        <p>

          Check file against MD5SUM on Linux, assuming both file and .md5 are in the same dir.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>file blah</code></p>
      </td>
      <td>
        <p>

          Find out the type of file on Linux, also displays if file is 32 or 64 bit.

        </p>
      </td>
    </tr>  

    <tr>
      <td>
        <p><code>dos2unix</code></p>
      </td>
      <td>
        <p>

          Convert Windows line endings to Unix / Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>base64 < input-file > output-file</code></p>
      </td>
      <td>
        <p>

          Base64 encodes input file and outputs a Base64 encoded file called output-file.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>base64 -d < input-file > output-file</code></p>
      </td>
      <td>
        <p>

          Base64 decodes input file and outputs a Base64 decoded file called output-file.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>touch -r ref-file new-file</code></p>
      </td>
      <td>
        <p>

          Creates a new file using the timestamp data from the reference file, drop the -r to simply create a file.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>rm -rf</code></p>
      </td>
      <td>
        <p>
          Remove files and directories without prompting for confirmation.
        </p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Samba Commands

Connect to a Samba share from Linux.

{% highlight bash %}
$ smbmount //server/share /mnt/win -o user=username,password=password1
$ smbclient -U user \\\\server\\share
$ mount -t cifs -o username=user,password=password //x.x.x.x/share /mnt/share
{% endhighlight %}


### Breaking Out of Limited Shells

Credit to G0tmi1k for these (or wherever he stole them from!).

The Python trick:
{% highlight python %}
python -c 'import pty;pty.spawn("/bin/bash")'
{% endhighlight %}

{% highlight bash %}
echo os.system('/bin/bash')
{% endhighlight %}

{% highlight bash %}
/bin/sh -i
{% endhighlight %}

### Misc Commands

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
        <p><code>init 6</code></p>
      </td>
      <td>
        <p>

          Reboot Linux from the command line.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>gcc -o output.c input.c</code></p>
      </td>
      <td>
        <p>

          Compile C code.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>gcc -m32 -o output.c input.c</code></p>
      </td>
      <td>
        <p>

          Cross compile C code, compile 32 bit binary on 64 bit Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>unset HISTORYFILE</code></p>
      </td>
      <td>
        <p>

          Disable bash history logging.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>rdesktop X.X.X.X</code></p>
      </td>
      <td>
        <p>

          Connect to RDP server from Linux.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>kill -9 $$</code></p>
      </td>
      <td>
        <p>

          Kill current session.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>chown user:group blah</code></p>
      </td>
      <td>
        <p>

          Change owner of file or dir.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>chown -R user:group blah</code></p>
      </td>
      <td>
        <p>

          Change owner of file or dir and all underlying files / dirs - recersive chown.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>chmod 600 file</code></p>
      </td>
      <td>
        <p>

          Change file / dir permissions, see [Linux File System Permissons](#linux-file-system-permissions) for details.

        </p>
      </td>
    </tr>

    </tbody>
</table>
</div>


Clear bash history:

    {% highlight bash %}
      $ ssh user@X.X.X.X | cat /dev/null > ~/.bash_history
    {% endhighlight %}

### Linux File System Permissions

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Value</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>

    <tr>
      <td>
        <p><code>777</code></p>
      </td>
      <td>
        <p>

          <code>rwxrwxrwx</code> No restriction, global WRX any user can do anything.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>755</code></p>
      </td>
      <td>
        <p>

          <code>rwxr-xr-x</code> Owner has full access, others can read and execute the file.

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>700</code></p>
      </td>
      <td>
        <p>

          <code>rwx------</code> Owner has full access, no one else has access.  

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>666</code></p>
      </td>
      <td>
        <p>

          <code>rw-rw-rw-</code> All users can read and write but not execute.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>644</code></p>
      </td>
      <td>
        <p>

          <code>rw-r--r--</code> Owner can read and write, everyone else can read.

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>600</code></p>
      </td>
      <td>
        <p>

          <code>rw-------</code> Owner can read and write, everyone else has no access.

        </p>
      </td>
    </tr>


    </tbody>
</table>
</div>

### Linux File System

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Directory</th>
      <th>Description</th>
    </tr>
    </thead>
  <tbody>
    <tr>
      <td>
        <p><code>/</code></p>
      </td>
      <td>
        <p>

          / also know as "slash" or the root.   

        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>/bin</code></p>
      </td>
      <td>
        <p>

          Common programs, shared by the system, the system administrator and the users.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/boot</code></p>
      </td>
      <td>
        <p>

          Boot files, boot loader (grub), kernels, vmlinuz    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/dev</code></p>
      </td>
      <td>
        <p>

          Contains references to system devices, files with special properties.     

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc</code></p>
      </td>
      <td>
        <p>

          Important system config files.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/home</code></p>
      </td>
      <td>
        <p>

          Home directories for system users.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/lib</code></p>
      </td>
      <td>
        <p>

          Library files, includes files for all kinds of programs needed by the system and the users.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/lost+found</code></p>
      </td>
      <td>
        <p>

          Files that were saved during failures are here.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/mnt</code></p>
      </td>
      <td>
        <p>

          Standard mount point for external file systems.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/media</code></p>
      </td>
      <td>
        <p>

          Mount point for external file systems (on some distros).    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/net</code></p>
      </td>
      <td>
        <p>

          Standard mount point for entire remote file systems - nfs.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/opt</code></p>
      </td>
      <td>
        <p>

          Typically contains extra and third party software.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/proc</code></p>
      </td>
      <td>
        <p>

          A virtual file system containing information about system resources.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/root</code></p>
      </td>
      <td>
        <p>

          root users home dir.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/sbin</code></p>
      </td>
      <td>
        <p>

            Programs for use by the system and the system administrator.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/tmp</code></p>
      </td>
      <td>
        <p>

            Temporary space for use by the system, cleaned upon reboot.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/usr</code></p>
      </td>
      <td>
        <p>

            Programs, libraries, documentation etc. for all user-related programs.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/var</code></p>
      </td>
      <td>
        <p>

            Storage for all variable files and temporary files created by users, such as log files, mail queue, print spooler. Web servers, Databases etc.

        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>

### Linux Interesting Files / Dir's

Places that are worth a look if you are attempting to privilege escalate / perform post exploitation.    

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Directory</th>
      <th>Description</th>
    </tr>
    </thead>
  <tbody>
    <tr>
      <td>
        <p><code>/etc/passwd</code></p>
      </td>
      <td>
        <p>

          Contains local Linux users.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/shadow</code></p>
      </td>
      <td>
        <p>

          Contains local account password hashes.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/group</code></p>
      </td>
      <td>
        <p>

          Contains local account groups.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/init.d/</code></p>
      </td>
      <td>
        <p>

          Contains service init script - worth a look to see whats installed.    

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/hostname</code></p>
      </td>
      <td>
        <p>

          System hostname.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/network/interfaces</code></p>
      </td>
      <td>
        <p>

          Network interfaces.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/resolv.conf</code></p>
      </td>
      <td>
        <p>

          System DNS servers.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/profile</code></p>
      </td>
      <td>
        <p>

          System environment variables.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>~/.ssh/</code></p>
      </td>
      <td>
        <p>

          SSH keys.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>~/.bash_history</code></p>
      </td>
      <td>
        <p>

          Users bash history log.   

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/var/log/</code></p>
      </td>
      <td>
        <p>

          Linux system log files are typically stored here.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/var/adm/</code></p>
      </td>
      <td>
        <p>

          UNIX system log files are typically stored here.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/var/log/apache2/access.log</code></p>
        <p><code>/var/log/httpd/access.log</code></p>
      </td>
      <td>
        <p>

          Apache access log file typical path.  

        </p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>/etc/fstab</code></p>
      </td>
      <td>
        <p>

          File system mounts.  

        </p>
      </td>
    </tr>

  </tbody>
</table>
</div>
