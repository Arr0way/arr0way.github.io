---
layout: blog_item
title:  "Security Harden CentOS 7"
date:   2015-03-25 00:52:10
author: Arr0way
categories: [Security-Hardening] 
---

![OpenSCAP Guide - Securing CentOS 7](http://i.imgur.com/z6hxOZG.png)

* list element with functor item
{:toc}


## Security Harden CentOS 7

This HowTo walks you through the steps required to **security harden CentOS 7**, it's based on the [OpenSCAP](http://www.open-scap.org/page/Main_Page) benchmark, unfortunately the current version of OpenSCAP that ships with CentOS does not offically support CentOS CPE's. But there is a "workaround" that will allow OpenSCAP + OpenSCAP workbench to run on CentOS, I'll document this in a separate post.  

## Based on a Minimal Install 

To follow this guide you will need a minimal CentOS 7 install, ideally using the Kickstart file below or copying it's partition layout. Installing CentOS 7 using a minimal installation reduces the attack surface and ensures you *only install software that you require*. 

This guide only covers the base system + SSH hardening, I will document specific service hardening separately such as HTTPD, SFTP, LDAP, BIND etc... 

In the section related to removing unrequired services, if you installed a minimal centos 7 install, you'll likely have nothing to remove or disable - I've included this section for completeness. 


## Issues with Security Hardening 

After hardening a system you may run into issues, hardening a system will make it more restrictive, especially SELinux or filesystem related permission hardening. When hardening a system for a specific task I recommend creating a duplicate virtual machine you can use for troubleshooting should you run into a issue that you *think* is related to security hardening, you'll be able to confirm by running it on the Vanilla system. 

Obviously **don't expose the Vanilla (un-hardened) system to the network!** 

## Why use OpenSCAP ? 

After a lot of research I decided to use OpenSCAP over other security hardening benchmarks / guides, here is my reasoning for doing so:

- It's open, free and actively worked on
- It has an audit tool, essential to verify each system 
- OpenSCAP has a GUI called, workbench
- OpenSCAP Workbench supports remote audits via SSH
- OpenSCAP Workbench allows you to customize your scan, should you not agree with all hardening checks 

If you don't get on with workbench or auditing from the command line, Nessus has functionality for authenticated SCAP scans. 


## Kickstart 

I've provided the following RHEL kickstart file below, it's a minimal install with a heavy partition scheme, allowing for stricter mount options.  


{% highlight bash %}

#version=RHEL7

install
# System authorization information
auth --enableshadow --passalgo=sha512

# Use CDROM installation media
cdrom
# Accept EULA
eula --agreed

services --enabled=NetworkManager,sshd
reboot

# Run the Setup Agent on first boot
#firstboot --enable
ignoredisk --only-use=sda
# Keyboard layouts
keyboard --vckeymap=us --xlayouts='us'
# System language
lang en_US.UTF-8
# SELinux
selinux --enforcing
# Network information
network  --bootproto=dhcp --device=eno16777736 --onboot=on --ipv6=off
network  --hostname=default-vm
# Root password
rootpw --iscrypted HASHGOESHERE
# System timezone
timezone Europe/London --isUtc --ntpservers=prime.transformers
# System bootloader configuration
bootloader --location=mbr --boot-drive=sda
# Partition clearing information
clearpart --all --drives=sda
ignoredisk --only-use=sda
# LVM

# Disk partitioning information
part pv.18 --fstype="lvmpv" --ondisk=sda --size=8004
part pv.11 --fstype="lvmpv" --ondisk=sda --size=8004
part /boot --fstype="ext4" --ondisk=sda --size=1000
volgroup lg_data --pesize=4096 pv.18
volgroup lg_os --pesize=4096 pv.11
logvol /  --fstype="xfs" --size=4000 --name=lv_root --vgname=lg_os
logvol /home  --fstype="xfs" --size=2000 --name=lv_home --vgname=lg_data
logvol /tmp  --fstype="xfs" --size=1000 --name=lv_tmp --vgname=lg_os
logvol /var  --fstype="xfs" --size=2000 --name=lv_var --vgname=lg_os
logvol /var/tmp  --fstype="xfs" --size=1000 --name=lv_var_tmp --vgname=lg_os
logvol /var/www  --fstype="xfs" --size=5000 --name=lv_var_www --vgname=lg_data
logvol /var  --fstype="xfs" --size=1000 --name=lv_var --vgname=lg_os
logvol /var/log  --fstype="xfs" --size=1500 --name=lv_var_log --vgname=lg_os
logvol /var/log/audit  --fstype="xfs" --size=500 --name=lv_var_log_audit --vgname=lg_os
logvol /var/tmp  --fstype="ext4" --size=500 --name=lv_var_tmp --vgname=lg_os 
logvol swap  --fstype="swap" --size=1000 --name=lv_swap --vgname=lg_data

%packages
@core
 %end

%post
%end

{% endhighlight %}

## Secure Partition Mount Options 

Your millage will vary here, for example if you have a website that uses cgi-bin executables you won't be able to use the **noexec** mount options, but you can and should use it on /tmp and /var/tmp as this is typically the first place an attacker will attempt to write and execute from when performing privilege escalation. 

Your /etc/fstab file should look something like: 

{% highlight bash %} 

#
# /etc/fstab
# Created by anaconda on Sat Oct 11 14:28:47 2014
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/lg_os-lv_root /                       xfs     defaults        1 1
UUID=d73c5d22-75ed-416e-aad2-8c1bb1dfc713 /boot                   ext4    defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_data-lv_home /home                   xfs     defaults        1 2
/dev/mapper/lg_os-lv_tmp /tmp                    xfs     defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_os-lv_var /var                    xfs     defaults,nosuid        1 2
/dev/mapper/lg_os-lv_var_tmp /var/tmp                xfs     defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_os-lv_var_tmp /var/log                xfs     defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_os-lv_var_tmp /var/log/audit                xfs     defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_data-lv_var_www /var/www                xfs     defaults,nosuid,noexec,nodev        1 2
/dev/mapper/lg_data-lv_swap swap                    swap    defaults        0 0

{% endhighlight %} 


## Install NTP

NTP is required for a number of compliance audits and is general good practice.

{% highlight bash %}
yum install ntp ntpdate
chkconfig ntpd on
ntpdate pool.ntp.org
/etc/init.d/ntpd start
{% endhighlight %} 

## Configure System for AIDE

Pre-linking binaries (arguably) improved execution time, however this cause issues with AIDE, so it must be disabled.

Open /etc/sysconfig/prelink and make sure the line <code>Set PRELINKING=no</code> is present, if you're writing a script: 

{% highlight bash %}
# Disable prelinking altogether
#
if grep -q ^PRELINKING /etc/sysconfig/prelink
then
  sed -i 's/PRELINKING.*/PRELINKING=no/g' /etc/sysconfig/prelink
else
  echo -e "\n# Set PRELINKING=no per security requirements" >> /etc/sysconfig/prelink
  echo "PRELINKING=no" >> /etc/sysconfig/prelink
fi
{% endhighlight %}

Disable previous prelink changes to binaries: 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Disable previous prelink changes to binaries</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">/usr/sbin/prelink -ua</span>
        </p>
        </p>
      </div>
    </div>
</section>

## Install AIDE

Install AIDE - Advanced Intrusion Detection Environment

{% highlight bash %}

yum install aide -y && /usr/sbin/aide --init && cp /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz && /usr/sbin/aide --check 

{% endhighlight %}

Configure periodic execution of AIDE, runs every morning at 04:30

{% highlight bash %}
echo "05 4 * * * root /usr/sbin/aide --check" >> /etc/crontab
{% endhighlight %}

## Prevent Users Mounting USB Storage

{% highlight bash %}
echo "install usb-storage /bin/false" > /etc/modprobe.d/usb-storage.conf
{% endhighlight %}

## Enable Secure (high quality) Password Policy 

The following command will Enable **SHA512** instead of using *MD5*:

{% highlight bash %}
authconfig --passalgo=sha512 —update
{% endhighlight %}
 
<code>vi /etc/security/psquality.conf</code> 

{% highlight bash %}

# Configuration for systemwide password quality limits
# Defaults:
#
# Number of characters in the new password that must not be present in the
# old password.
difok = 5
#
# Minimum acceptable size for the new password (plus one if
# credits are not disabled which is the default). (See pam_cracklib manual.)
# Cannot be set to lower value than 6.
minlen = 14
#
# The maximum credit for having digits in the new password. If less than 0
# it is the minimum number of digits in the new password.
dcredit = 1
#
# The maximum credit for having uppercase characters in the new password.
# If less than 0 it is the minimum number of uppercase characters in the new
# password.
ucredit = 1
#
# The maximum credit for having lowercase characters in the new password.
# If less than 0 it is the minimum number of lowercase characters in the new
# password.
lcredit = 1
#
# The maximum credit for having other characters in the new password.
# If less than 0 it is the minimum number of other characters in the new
# password.
ocredit = 1
#
# The minimum number of required classes of characters for the new
# password (digits, uppercase, lowercase, others).
minclass = 4
#
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
maxrepeat = 3
#
# The maximum number of allowed consecutive characters of the same class in the
# new password.
# The check is disabled if the value is 0.
maxclassrepeat = 3
#
# Whether to check for the words from the passwd entry GECOS string of the user.
# The check is enabled if the value is not 0.
gecoscheck = 1
#
# Path to the cracklib dictionaries. Default is to use the cracklib default.
# dictpath =

{% endhighlight %}

## Secure /etc/login.defs Pasword Policy

Add the following to <code>/etc/login.defs</code> 

{% highlight bash %} 
PASS_MIN_LEN 14
PASS_MIN_DAYS 1
PASS_MAX_DAYS 60
{% endhighlight %}

## Set Last Logon/Access Notification

Open <code>/etc/pam.d/system-auth</code> and add the following line immediately after session required pam_limits.so: 

<code>session       required     pam_lastlog.so showfailed</code> 

## Max Password Login Attempts per Session 

Set the amount of password reprompts per session, by editing the <code>pam_pwquality.so</code> statement in <code>/etc/pam.d/system-auth</code> to <code>retry=3</code> or lower. 

## Set Deny For Failed Password Attempts

Blocks logins for failed authentication on accounts. 

Add the following lines immediately below the <code>pam_unix.so</code> statement in AUTH section of both <code>/etc/pam.d/system-auth</code> and <code>/etc/pam.d/password-auth</code>: 

{% highlight bash%}
auth [default=die] pam_faillock.so authfail deny=3 unlock_time=604800 fail_interval=900

auth required pam_faillock.so authsucc deny=3 unlock_time=604800 fail_interval=900
{% endhighlight %}

## Limit Password Reuse

Open /etc/pam.d/system-auth, append remember=24 to the pam_unix.so line - preventing users from reusing passwords, remembering 24 times is the DoD standard. 

The line should look like:

{% highlight bash %}
password sufficient pam_unix.so existing_options remember=24
{% endhighlight %}

## Verify /boot/grub2/grub.cfg Permissions

Set grub.conf to chmod 600:

{% highlight bash %}
sudo chmod 600/boot/grub2/grub.cfg
{% endhighlight %}

## Set Boot Loader Password 

The grub2 boot loader should have a superuser account and password protection enabled to protect boot-time settings.

To do so, select a superuser account and password and add them into the appropriate grub2 configuration file(s) under /etc/grub.d. Since plaintext passwords are a security risk, generate a hash for the pasword by running the following command: 

{% highlight bash %}
grub2-mkpasswd-pbkdf2
{% endhighlight %}

When prompted, enter the password that was selected and insert the returned password hash into the appropriate grub2 configuration file(s) under /etc/grub.d immediately after the superuser account. (Use the output from grub2-mkpasswd-pbkdf2 as the value of password-hash):

{% highlight bash %}
password_pbkdf2 superusers-accountpassword-hash 
{% endhighlight %}

<div class="note tip">
  <h5>Don't use common admin account names for the grub2 superuser</h5>
  <p>Avoid using common admin account names like, root, admin or administrator for the grub2 superuser account. To meet FISMA Moderate, the bootloader superuser account password <b>must differ</b> from the root credentials.</p>
</div>

{% highlight bash %}
grub2-mkconfig -o /boot/grub2/grub.cfg
{% endhighlight %}

<div class="note warning">
  <h5>Don't manually add the superuser account to grub.cfg </h5>
  <p>Do NOT manually add the superuser account and password to the grub.cfg file as the grub2-mkconfig command overwrites this file.</p>
</div>

## Require Authentication for Single User Mode

Require root password when entering single user mode, open <code>/etc/sysconfig/init</code> and add the line: 

{% highlight bash %}
SINGLE=/sbin/sulogin
{% endhighlight %}

## Disable Ctrl-Alt-Del Reboot Activation 

Prevernt ALT+CTRL+DEL from rebooting. 

Open <code>/etc/init/control-alt-delete.conf</code> and modify the existing line:

{% highlight bash %}
exec /sbin/shutdown -r now "Control-Alt-Delete pressed"
{% endhighlight %}

To:

{% highlight bash %}
exec /usr/bin/logger -p security.info "Control-Alt-Delete pressed"
{% endhighlight %}

## Enable Console Screen Locking 

Install the screen Package to allow console screen locking. 

{% highlight bash %}
sudo yum install screen
{% endhighlight %}

Users can now run <code>screen</code> and lock the console with <code>ctrl+a x</code>.

## Disable Zeroconf Networking

Zeroconf network typically occours when you fail to get an address via DHCP, the interface will be assigned a 169.254.0.0 address.

To prevernt this:

{% highlight bash %}
echo "NOZEROCONF=yes" >> /etc/sysconfig/network
{% endhighlight %}

## Disable IPv6 Support Automatically Loading

Open <code>/etc/modprobe.d/disabled.conf</code> and add the line: 

{% highlight bash %}
options ipv6 disable=1
{% endhighlight %}

## Disable Interface Usage of IPv6

Add the following to <code>/etc/sysconfig/network</code> 

{% highlight bash %}
NETWORKING_IPV6=no
IPV6INIT=no
{% endhighlight %}

## Disable Support for RPC IPv6

RPC services like NFSv4 attempt to start using IPv6 even if it's disabled in <code>/etc/modprobe.d</code>. To prevent this behaviour open <code>/etc/netconfig</code> and comment the following lines: 

{% highlight bash %}
udp6       tpi_clts      v     inet6    udp     -       -
tcp6       tpi_cots_ord  v     inet6    tcp     -       -
{% endhighlight %}


## Securing root Logins

Only allow root logins via local terminal: 

{% highlight bash %} 
echo "tty1" > /etc/securetty
chmod 700 /root
{% endhighlight %}

## Enable UMASK 077

Can causes issues on systems where users share files:

{% highlight bash %}
perl -npe 's/umask\s+0\d2/umask 077/g' -i /etc/bashrc
perl -npe 's/umask\s+0\d2/umask 077/g' -i /etc/csh.cshrc
{% endhighlight %}

## Prune Idle Users 

{% highlight bash %}
echo "Idle users will be removed after 15 minutes"
echo "readonly TMOUT=900" >> /etc/profile.d/os-security.sh
echo "readonly HISTFILE" >> /etc/profile.d/os-security.sh
chmod +x /etc/profile.d/os-security.sh
{% endhighlight %}

## Securing Cron 

{% highlight bash %}
echo "Locking down Cron"
touch /etc/cron.allow
chmod 600 /etc/cron.allow
awk -F: '{print $1}' /etc/passwd | grep -v root > /etc/cron.deny
echo "Locking down AT"
touch /etc/at.allow
chmod 600 /etc/at.allow
awk -F: '{print $1}' /etc/passwd | grep -v root > /etc/at.deny
{% endhighlight %}

## Sysctl Security 

<code>/etc/sysctl.conf</code> 

{% highlight bash %}
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.tcp_max_syn_backlog = 1280
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.tcp_timestamps = 0
{% endhighlight %}

## Deny All TCP Wrappers 

TCP wrappers can provide a quick and easy method for controlling access to applications linked to them. Examples of TCP Wrapper aware applications are sshd, and portmap. 

Below commands block all but SSH: 

{% highlight bash %}
echo "ALL:ALL" >> /etc/hosts.deny
echo "sshd:ALL" >> /etc/hosts.allow
{% endhighlight %}

## Basic iptables Firewall Rules

Basic iptables Firewall rules, set to denyall as the default. 

{% highlight bash %}
#Drop anything we aren't explicitly allowing. All outbound traffic is okay
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
:RH-Firewall-1-INPUT - [0:0]
-A INPUT -j RH-Firewall-1-INPUT
-A FORWARD -j RH-Firewall-1-INPUT
-A RH-Firewall-1-INPUT -i lo -j ACCEPT
-A RH-Firewall-1-INPUT -p icmp --icmp-type echo-reply -j ACCEPT
-A RH-Firewall-1-INPUT -p icmp --icmp-type destination-unreachable -j ACCEPT
-A RH-Firewall-1-INPUT -p icmp --icmp-type time-exceeded -j ACCEPT
# Accept Pings
-A RH-Firewall-1-INPUT -p icmp --icmp-type echo-request -j ACCEPT
# Log anything on eth0 claiming it's from a local or non-routable network
# If you're using one of these local networks, remove it from the list below
-A INPUT -i eth0 -s 10.0.0.0/8 -j LOG --log-prefix "IP DROP SPOOF A: "
-A INPUT -i eth0 -s 172.16.0.0/12 -j LOG --log-prefix "IP DROP SPOOF B: "
-A INPUT -i eth0 -s 192.168.0.0/16 -j LOG --log-prefix "IP DROP SPOOF C: "
-A INPUT -i eth0 -s 224.0.0.0/4 -j LOG --log-prefix "IP DROP MULTICAST D: "
-A INPUT -i eth0 -s 240.0.0.0/5 -j LOG --log-prefix "IP DROP SPOOF E: "
-A INPUT -i eth0 -d 127.0.0.0/8 -j LOG --log-prefix "IP DROP LOOPBACK: "
# Accept any established connections
-A RH-Firewall-1-INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# Accept ssh traffic. Restrict this to known ips if possible.
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
#Log and drop everything else
-A RH-Firewall-1-INPUT -j LOG
-A RH-Firewall-1-INPUT -j DROP
COMMIT
{% endhighlight %}


## Verify iptables Enabled

{% highlight bash %}
sudo systemctl enable iptables
systemctl start iptables.service
{% endhighlight %}


## Disable Uncommon Protocols 


The following Protocols will be disabled: 

- Datagram Congestion Control Protocol (DCCP) 
- Stream Control Transmission Protocol (SCTP)
- Reliable Datagram Sockets (RDS)
- Transparent Inter-Process Communication (TIPC) 

{% highlight bash %}
echo "install dccp /bin/false" > /etc/modprobe.d/dccp.conf
echo "install sctp /bin/false" > /etc/modprobe.d/sctp.conf
echo "install rds /bin/false" > /etc/modprobe.d/rds.conf
echo "install tipc /bin/false" > /etc/modprobe.d/tipc.conf
{% endhighlight %}

## Ensure Rsyslog is installed 

{% highlight bash %}
yum -y install rsyslog
{% endhighlight %}


## Enable Rsyslog

{% highlight bash %}
systemctl enable rsyslog.service
systemctl start rsyslog.service
{% endhighlight %}

## Auditd - Audit Daemon

### Enable auditd Service

{% highlight bash %}
systemctl enable auditd.service
systemctl start auditd.service
{% endhighlight %}

### Audit Processes Which Start Prior to auditd

Audit process which start before the Audit Daemon. 

Add the following line to <code>/etc/grub.conf</code>: 

{% highlight bash %}
kernel /vmlinuz-version ro vga=ext root=/dev/VolGroup00/LogVol00 rhgb quiet audit=1
{% endhighlight %}

### Auditd Number of Logs Retained

Open <code>/etc/audit/auditd.conf</code> and add or modify:

{% highlight bash %}
num_logs = 5
{% endhighlight %}

### Auditd Max Log File Size 

{% highlight bash %}
max_log_file = 30MB
{% endhighlight %}

### Auditd max_log_file_action

Open <code>/etc/audit/auditd.conf</code> and set this to rotate. 

{% highlight bash %}
max_log_file_action = rotate
{% endhighlight %}


### Auditd space_left

Configure auditd to email you when space gets low, open <code>/etc/audit/auditd.conf</code> and modify the following:

{% highlight bash %}
space_left_action = email 
{% endhighlight %}

### Auditd admin_space_left

Configure auditd to halt when auditd log space is used up, forcing the system admin to rectify the space issue. 

On some systems where monitoring is less important another action could be leveraged. 

{% highlight bash %}
admin_space_left_action = halt
{% endhighlight %}

### Auditd mail_acct

When space gets low auditd can send a email notification via email, to configure this and the following line to <code>/etc/audit/auditd.conf</code>: 

{% highlight bash %}
action_mail_acct = root
{% endhighlight %} 

### Configure auditd to use audispd plugin

Auditd does not have the functionality to send logs directly to an external log server, however the audispd plugin pass audit records to the local syslog server, to enable this open <code>/etc/audisp/plugins.d/syslog.conf</code> and set the active line to yes, then restart audispd daemon: 

{% highlight bash %}
sudo service auditd restart
{% endhighlight %}

### Auditd Rules: /etc/audit/audit.rules

Open <code>/etc/audit/audit.rules</code> and add the following lines to monitor various system files and activities: 

{% highlight bash %}
# audit_time_rules - Record attempts to alter time through adjtime
-a always,exit -F arch=b64 -S adjtimex -k audit_time_rules

# audit_time_rules - Record attempts to alter time through settimeofday
-a always,exit -F arch=b64 -S settimeofday -k audit_time_rules

# audit_time_rules - Record Attempts to Alter Time Through stime
-a always,exit -F arch=b64 -S adjtimex -S settimeofday -S clock_settime 
-k audit_time_rules

# audit_time_rules - Record Attempts to Alter Time Through clock_settime
-a always,exit -F arch=b64 -S clock_settime -k audit_time_rules

# Record Attempts to Alter the localtime File
-w /etc/localtime -p wa -k audit_time_rules

# Record Events that Modify User/Group Information
# audit_account_changes
-w /etc/group -p wa -k audit_account_changes
-w /etc/passwd -p wa -k audit_account_changes
-w /etc/gshadow -p wa -k audit_account_changes
-w /etc/shadow -p wa -k audit_account_changes
-w /etc/security/opasswd -p wa -k audit_account_changes

# Record Events that Modify the System's Network Environment
# audit_network_modifications
-a always,exit -F arch=ARCH -S sethostname -S setdomainname -k audit_network_modifications
-w /etc/issue -p wa -k audit_network_modifications
-w /etc/issue.net -p wa -k audit_network_modifications
-w /etc/hosts -p wa -k audit_network_modifications
-w /etc/sysconfig/network -p wa -k audit_network_modifications

#Record Events that Modify the System's Mandatory Access Controls
-w /etc/selinux/ -p wa -k MAC-policy

#Record Events that Modify the System's Discretionary Access Controls - chmod
-a always,exit -F arch=b32 -S chmod -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S chmod  -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - chown
-a always,exit -F arch=b32 -S chown -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S chown -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchmod
-a always,exit -F arch=b32 -S fchmod -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchmod -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchmodat
-a always,exit -F arch=b32 -S fchmodat -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchmodat -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchown
-a always,exit -F arch=b32 -S fchown -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchown -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchownat
-a always,exit -F arch=b32 -S fchownat -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchownat -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fremovexattr
-a always,exit -F arch=b32 -S fremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fsetxattr
-a always,exit -F arch=b32 -S fsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - lchown
-a always,exit -F arch=b32 -S lchown -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S lchown -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - lremovexattr
-a always,exit -F arch=b32 -S lremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S lremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - lsetxattr
-a always,exit -F arch=b32 -S lsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S lsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - removexattr
-a always,exit -F arch=b32 -S removexattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S removexattr -F auid>=500 -F auid!=4294967295 -k perm_mod-a always,exit -F arch=b32 -S fchmodat -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchmodat -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchown
-a always,exit -F arch=b32 -S fchown -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchown -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fchownat
-a always,exit -F arch=b32 -S fchownat -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fchownat -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fremovexattr
-a always,exit -F arch=b32 -S fremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S fremovexattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - fsetxattr
-a always,exit -F arch=b32 -S lsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S lsetxattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - removexattr
-a always,exit -F arch=b32 -S removexattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S removexattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Events that Modify the System's Discretionary Access Controls - setxattr
-a always,exit -F arch=b32 -S setxattr -F auid>=500 -F auid!=4294967295 -k perm_mod
-a always,exit -F arch=b64 -S setxattr -F auid>=500 -F auid!=4294967295 -k perm_mod

#Record Attempts to Alter Logon and Logout Events
-w /var/log/faillog -p wa -k logins 
-w /var/log/lastlog -p wa -k logins

#Record Attempts to Alter Process and Session Initiation Information
-w /var/run/utmp -p wa -k session
-w /var/log/btmp -p wa -k session
-w /var/log/wtmp -p wa -k session

#Ensure auditd Collects Unauthorized Access Attempts to Files (unsuccessful)
-a always,exit -F arch=b32 -S creat -S open -S openat -S open_by_handle_at -S truncate -S ftruncate -F exit=-EACCES -F auid>=500 -F auid!=4294967295 -k access
-a always,exit -F arch=b32 -S creat -S open -S openat -S open_by_handle_at -S truncate -S ftruncate -F exit=-EPERM -F auid>=500 -F auid!=4294967295 -k access
-a always,exit -F arch=b64 -S creat -S open -S openat -S open_by_handle_at -S truncate -S ftruncate -F exit=-EACCES -F auid>=500 -F auid!=4294967295 -k access
-a always,exit -F arch=b64 -S creat -S open -S openat -S open_by_handle_at -S truncate -S ftruncate -F exit=-EPERM -F auid>=500 -F auid!=4294967295 -k access

#Ensure auditd Collects Information on the Use of Privileged Commands
#
#  Find setuid / setgid programs then modify and uncomment the line below. 
#
##  sudo find / -xdev -type f -perm -4000 -o -perm -2000 2>/dev/null
#
# -a always,exit -F path=SETUID_PROG_PATH -F perm=x -F auid>=500 -F auid!=4294967295 -k privileged

#Ensure auditd Collects Information on Exporting to Media (successful)
-a always,exit -F arch=ARCH -S mount -F auid>=500 -F auid!=4294967295 -k export

#Ensure auditd Collects File Deletion Events by User
-a always,exit -F arch=ARCH -S rmdir -S unlink -S unlinkat -S rename -S renameat -F auid>=500 -F auid!=4294967295 -k delete

#Ensure auditd Collects System Administrator Actions 
-w /etc/sudoers -p wa -k actions

#Ensure auditd Collects Information on Kernel Module Loading and Unloading 
-w /sbin/insmod -p x -k modules
-w /sbin/rmmod -p x -k modules
-w /sbin/modprobe -p x -k modules
-a always,exit -F arch=b64 -S init_module -S delete_module -k modules

#Make the auditd Configuration Immutable 
-e 2
{% endhighlight %}


##Removal of Unrequired Services 

The section outlines software that should be removed, instruction for disabling the service is also documented.  

### Bulk Remove of Services

{% highlight bash %}
# Remove
yum remove xinetd
yum remove telnet-server
yum remove rsh-server
yum remove telnet
yum remove rsh-server
yum remove rsh
yum remove ypbind
yum remove ypserv
yum remove tftp-server
yum remove cronie-anacron
yum remove bind
yum remove vsftpd
yum remove httpd
yum remove dovecot
yum remove squid
yum remove net-snmpd
{% endhighlight %}


### Bulk Enable / Disable Services

{% highlight bash %}
#Disable / Enable
systemctl disable xinetd
systemctl disable rexec
systemctl disable rsh
systemctl disable rlogin
systemctl disable ypbind
systemctl disable tftp
systemctl disable certmonger
systemctl disable cgconfig
systemctl disable cgred
systemctl disable cpuspeed
systemctl enable irqbalance
systemctl disable kdump
systemctl disable mdmonitor
systemctl disable messagebus
systemctl disable netconsole
systemctl disable ntpdate
systemctl disable oddjobd
systemctl disable portreserve
systemctl enable psacct
systemctl disable qpidd
systemctl disable quota_nld
systemctl disable rdisc
systemctl disable rhnsd
systemctl disable rhsmcertd
systemctl disable saslauthd
systemctl disable smartd
systemctl disable sysstat
systemctl enable crond
systemctl disable atd
systemctl disable nfslock
systemctl disable named
systemctl disable httpd
systemctl disable dovecot
systemctl disable squid
systemctl disable snmpd
{% endhighlight %}


### Disable Secure RPC Client Service

Disable rpcgssd:

The rpcgssd service manages RPCSEC GSS contexts required to secure protocols that use RPC (most often Kerberos and NFS). The rpcgssd service is the client-side of RPCSEC GSS. If the system does not require secure RPC then this service should be disabled. The rpcgssd service can be disabled with the following command: 

{% highlight bash %}
systemctl disable rpcgssd
{% endhighlight %}

### Disable Secure RPC Server Service

Disable rpcsvcgssd:

{% highlight bash %}
systemctl disable rpcsvcgssd
{% endhighlight %}

### Disable RPC ID Mapping Service

Disable rpcidmapd.

The rpcidmapd service is used to map user names and groups to UID and GID numbers on NFSv4 mounts. If NFS is not in use on the local system then this service should be disabled. The rpcidmapd service can be disabled with the following command:

{% highlight bash %}
systemctl disable rpcidmapd
{% endhighlight %}

### Disable Network File Systems (netfs)

The netfs script manages the boot-time mounting of several types of networked filesystems, of which NFS and Samba are the most common. If these filesystem types are not in use, the script can be disabled, protecting the system somewhat against accidental or malicious changes to /etc/fstab and against flaws in the netfs script itself. The netfs service can be disabled with the following command: 

{% highlight bash %}
sudo systemctl disable netfs
{% endhighlight %}


### Disable Network File System (nfs)

{% highlight bash %}
systemctl disable nfs
{% endhighlight %}

### If you don't need SSH disable it 

{% highlight bash %}
systemctl disable sshd
{% endhighlight %}


### Disable SSH iptables Firewall rule
 
Only do this if you don't need SSH. 

{% highlight bash %}
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
{% endhighlight %}

<div class="note">
  <h5>Tips™ - You probable need to leave SSH alone</h5>
  <p>Unless you know you don't need SSH, leave SSH and it's iptables rule enabled.</p>
</div>


###Remove Rsh Trust Files

{% highlight bash %}
rm /etc/hosts.equiv
rm ~/.rhosts
{% endhighlight %}

### Disable Avahi Server Software

The avahi-daemon service can be disabled with the following command: 

{% highlight bash %}
systemctl disable avahi-daemon
{% endhighlight %}

### Disable the CUPS Service 

If you don't need CUPS, disable it to further reduce your attack surface: 

{% highlight bash %}
systemctl disable cups
{% endhighlight %}

### Disable DHCP Service

The dhcpd service should be disabled on any system that does not need to act as a DHCP server.

{% highlight bash %}
systemctl disable dhcpd
{% endhighlight %}

### Uninstall DHCP Server Package

If you don't need a DHCP client, remove it: 

{% highlight bash %}
yum erase dhcp
{% endhighlight %}

### Disable DHCP Client

Open <code>/etc/sysconfig/network-scripts/ifcfg-eth0</code> (if you have more interfaces, do this for each one) and make sure the address is statically assigned with the BOOTPROTO=none 

Example: 

{% highlight bash %}
BOOTPROTO=none

NETMASK=255.255.255.0
IPADDR=192.168.1.2
GATEWAY=192.168.1.1
{% endhighlight %} 

### Specify Additional Remote NTP Servers

Open <code>/etc/ntp.conf</code> and add the following line: 

{% highlight bash %}
server ntpserver
{% endhighlight %}

Use an internal NTP server if possible. 

### Enable Postfix 

{% highlight bash %}
systemctl enable postfix
{% endhighlight %}

### Remove Sendmail 

{% highlight bash %}
yum remove sendmail 
{% endhighlight %}

### Postfix Disable Network Listening 

Open, <code>/etc/postfix/main.cf</code> and ensure the following inet_interfaces line appears: 

{% highlight bash %}
inet_interfaces = localhost
{% endhighlight %}

### Configure SMTP Greeting Banner 

Change the greeting banner, the default banner discloses the SMTP server is Postfix. 


### Disable xinetd Service

{% highlight bash %}
sudo systemctl disable xinetd
{% endhighlight %}

### System Audit Logs Permissions 

System audit logs must have 0640 or less permissions set. 

{% highlight bash %}
sudo chmod 0640 audit_file
{% endhighlight %}

### System Audit Logs Must Be Owned By Root

{% highlight bash %}
sudo chown root/var/log
{% endhighlight %}

### Disable autofs 

{% highlight bash %}
chkconfig --level 0123456 autofs off
service autofs stop 
{% endhighlight %}

## Disable uncommon filesystems 

{% highlight bash %}
echo "install cramfs /bin/false" > /etc/modprobe.d/cramfs.conf
echo "install freevxfs /bin/false" > /etc/modprobe.d/freevxfs.conf
echo "install jffs2 /bin/false" > /etc/modprobe.d/jffs2.conf
echo "install hfs /bin/false" > /etc/modprobe.d/hfs.conf
echo "install hfsplus /bin/false" > /etc/modprobe.d/hfsplus.conf
echo "install squashfs /bin/false" > /etc/modprobe.d/squashfs.conf
echo "install udf /bin/false" > /etc/modprobe.d/udf.conf
{% endhighlight %}

## Disable core dumps for all users

<code>vi /etc/security/limits.conf</code> 
{% highlight bash %}
* hard core 0
{% endhighlight %} 

## Disable core dumps for SUID programs

Run <code>sysctl -w fs.suid_dumpable=0</code> and <code>fs.suid_dumpable = 0</code>. 

{% highlight bash %}
# Set runtime for fs.suid_dumpable
#
sysctl -q -n -w fs.suid_dumpable=0

#
# If fs.suid_dumpable present in /etc/sysctl.conf, change value to "0"
#     else, add "fs.suid_dumpable = 0" to /etc/sysctl.conf
#
if grep --silent ^fs.suid_dumpable /etc/sysctl.conf ; then
     sed -i 's/^fs.suid_dumpable.*/fs.suid_dumpable = 0/g' /etc/sysctl.conf
else
     echo "" >> /etc/sysctl.conf
     echo "# Set fs.suid_dumpable to 0 per security requirements" >> /etc/sysctl.conf
     echo "fs.suid_dumpable = 0" >> /etc/sysctl.conf
fi
{% endhighlight %} 

## Buffer Overflow Protection 

This section helps mitigate against Buffer Overflow attacks (BOF). 

### Enable ExecShield 

Helps prevent stack smashing / BOF. 

Enable on current kernel: <code>sysctl -w kernel.exec-shield=1</code> 

Add to /etc/sysctl.conf:  

{% highlight bash %}
kernel.exec-shield = 1
{% endhighlight %}

### Check / Enable ASLR

Set runtime for kernel.randomize_va_space <code>sysctl -q -n -w kernel.randomize_va_space=2</code> 

Add <code>kernel.randomize_va_space = 2</code> to /etc/sysctl.conf if it does not already exist.

### Enable XD or NX Support on x86 Systems 

Recent processors in the x86 family support the ability to prevent code execution on a per memory page basis. Generically and on AMD processors, this ability is called **No Execute (NX)**, while on Intel processors it is called **Execute Disable (XD)**. This ability can help prevent exploitation of buffer overflow vulnerabilities and should be activated whenever possible. Extra steps must be taken to ensure that this protection is enabled, particularly on 32-bit x86 systems. Other processors, such as Itanium and POWER, have included such support since inception and the standard kernel for those platforms supports the feature. 

Check bios and ensure XD/NX is enabled, not relevant for VM’s. 

## SELinux

### Confirm SELinux is not disabled

{% highlight bash %}
sed -i "s/selinux=0//gI" /etc/grub.conf
sed -i "s/enforcing=0//gI" /etc/grub.conf
{% endhighlight %}

### SELinux Targeted / Enforcing 

Open /etc/selinux/config and check for <code>SELINUXTYPE=targeted</code> or <code>SELINUXTYPE=enforcing</code>, depending on your requirements. 

### Enable the SELinux restorecond Service

The restorecond service utilizes inotify to look for the creation of new files listed in the /etc/selinux/restorecond.conf configuration file. When a file is created, restorecond ensures the file receives the proper SELinux security context. The restorecond service can be enabled with the following command:

Enable restorecond for all run levels:

<code>chkconfig --level 0123456 restorecond on</code>


Start restorecond if not currently running:

<code>service restorecond start</code>

### Check no daemons are unconfined by SELinux
Run: 

{% highlight bash %}
sudo ps -eZ | egrep "initrc" | egrep -vw "tr|ps|egrep|bash|awk" | tr ':' ' ' | awk '{ print $NF }’
{% endhighlight %}

This should return no output. 

## Prevent Log In to Accounts With Empty Password

{% highlight bash %}
sed -i 's/\<nullok\>//g' /etc/pam.d/system-auth
{% endhighlight %}


## Secure SSH 

### Allow Only SSH Protocol 2

Open <code>/etc/ssh/sshd_config</code> and ensure the following line exists:

{% highlight bash %}
Protocol 2
{% endhighlight %}
### Limit Users' SSH Access

Open <code>/etc/ssh/sshd_config</code> and add:

{% highlight bash %}
DenyUsers USER1 USER2
{% endhighlight %}
### Set SSH Idle Timeout Interval

To set an idle timeout interval, edit the following line in <code>/etc/ssh/sshd_config</code> as follows:

{% highlight bash %}
ClientAliveInterval interval
{% endhighlight %}
### Set SSH Client Alive Count

To ensure the SSH idle timeout occurs precisely when the ClientAliveCountMax is set, edit <code>/etc/ssh/sshd_config</code> as follows:

{% highlight bash %}
ClientAliveCountMax 0
{% endhighlight %}

### Disable SSH Support for .rhosts Files

SSH can emulate the behavior of the obsolete rsh command in allowing users to enable insecure access to their accounts via .rhosts files.

To ensure this behavior is disabled, add or correct the following line in <code>/etc/ssh/sshd_config</code>:
{% highlight bash %}
IgnoreRhosts yes
{% endhighlight %}

### Disable Host-Based Authentication

SSH's cryptographic host-based authentication is more secure than .rhosts authentication. However, it is not recommended that hosts unilaterally trust one another, even within an organization.

To disable host-based authentication, add or correct the following line in <code>/etc/ssh/sshd_config</code>:

{% highlight bash %}
HostbasedAuthentication no
{% endhighlight %}
### Disable SSH Root Login

Disable root logins via SSH, open <code>/etc/ssh/sshd_config</code> and ensure the following line exists:

{% highlight bash %}
PermitRootLogin no
{% endhighlight %}

### Disable SSH Access via Empty Passwords

Open <code>/etc/ssh/sshd_config</code>:

{% highlight bash %}
PermitEmptyPasswords no
{% endhighlight %}
### Enable SSH Warning Banner

Enable a warning banner (Renforce policy awareness).

Banner <code>/etc/issue</code>

### Do Not Allow SSH Environment Options

To ensure users are not able to present environment options to the SSH daemon, add or correct the following line in <code>/etc/ssh/sshd_config</code>:

{% highlight bash %}
PermitUserEnvironment no
{% endhighlight %}
### Use Only Approved Ciphers

Limit the ciphers to those algorithms which are FIPS-approved. Counter (CTR) mode is also preferred over cipher-block chaining (CBC) mode. The following line in <code>/etc/ssh/sshd_config</code> demonstrates use of FIPS-approved ciphers:

{% highlight bash %}
Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc
{% endhighlight %}

## Secure X Windows 

### Disable X Windows Startup By Setting Runlevel

Disable X windows system, further reducing your attack surface.

{% highlight bash %}
Add id:3:initdefault: to /etc/inittab.
{% endhighlight %}

### Remove the X Windows Package Group

{% highlight bash %}
yum groupremove "X Window System"
{% endhighlight %}

## Prompt OS update installation

A process for prompt installation of OS updates must exist

{% highlight bash %}
yum -y install yum-cron
chkconfig yum-cron on
{% endhighlight %}

Make sure yum-cron is set to "check only", I don't recommend installing updates automatically. 

If this was helpfull, click tweet below. 

Enjoy.
