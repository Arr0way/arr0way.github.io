---
layout: blog_item
title:  "ADB Commands Cheat Sheet - Flags, Switches & Examples Tutorial"
date:   2024-06-06 05:37:10
author: Arr0way
description: 'ADB commands cheat sheet - Learn the Android adb toolk with this Tutorial containing Flags, & adb Command Examples.'
categories: [cheat-sheet]
tags:
- Web
- Tools
---

## What is adb? 

The Android Debug Bridge (adb) is a programming tool used for the debugging of Android-based devices. The daemon on the Android device connects with the server on the host PC over USB or TCP, which connects to the client that is used by the end user over TCP. For hackers, this means we can interact with Android devices to add and remove packages, access debug logs and interact with either physical or virtual Android instances for mobile web app security testing. 

* list element with functor item
{:toc}

## adb Installation 

How to install adb on MacOS: 

{% highlight bash %}

brew install android-platform-tools

{% endhighlight %}

## adb Example Commands


### adb start / restart 

{% highlight bash %}
adb kill-server
adb start-server 
{% endhighlight %}

### adb reboot 

{% highlight bash %}
adb reboot
adb reboot recovery 
adb reboot-bootloader
{% endhighlight %}

### adb reboot with root 

The following restarts the android device with root permissions: 

{% highlight bash %}
adb root
{% endhighlight %}

### adb list devices:

Show adb devices:

{% highlight bash %}

adb devices

{% endhighlight %}

adb show devices with full information (product/model)

{% highlight bash %}
adb devices -l
{% endhighlight %}

adb connect to a device: 

{% highlight bash %}
adb connect IP-ADDRESS
{% endhighlight %}

### adb shell

Run commands on the android device shell via an adb shell: 

{% highlight bash %}
adb shell
{% endhighlight %}

### adb android version

Return the running Android version of the connected adb device: 

{% highlight bash %}

adb shell getprop ro.build.version.release 

{% endhighlight %}

### adb Logcat 

Commands related to adb logcat, allowing access to Android logs via adb.  

adb view logs: 

{% highlight bash %}
adb logcat
{% endhighlight %}

adb clear logs on device: 

{% highlight bash %}
adb logcat -c
{% endhighlight %}

adb dump log output to file on the local system: 

{% highlight bash %}
adb logcat -d > /tmp/foo.txt
{% endhighlight %}

adb full log dump (dumpstate, dumpsys and logcat output): 

{% highlight bash %}
adb bugreport > /tmp/full-adb-dump.txt
{% endhighlight %}


### adb copy files 

adb copy files from your system to your android phone: 

{% highlight bash %}
adb push [source] [destination]
{% endhighlight %}

adb copy files from phone to system: 

{% highlight bash %}

adb pull [device file location] /tmp/foo

{% endhighlight %}

### adb install package 

{% highlight bash %}

adb -e install /tmp/package.apk

{% endhighlight %}

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th><code>COMMAND</code></th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>-d</p></td>
        <td><p>directs command to the only connected USB device</p></td>
      </tr>
      <tr>
        <td><p>-e</p></td>
        <td><p>directs command to the only running emulator</p></td>
      </tr>
      <tr>
        <td><p>-s</p></td>
        <td><p>serial number</p></td>
      </tr>
      <tr>
        <td><p>-p</p></td>
        <td><p>product name or path</p></td>
      </tr>
    </tbody>
  </table>
</div>

Note the flag needs to come before the command. 

### Uninstalling app with adb 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb uninstall com.myAppPackage</p></td>
        <td><p>Uninstalls the app with the package name com.myAppPackage.</p></td>
      </tr>
      <tr>
        <td><p>adb uninstall &lt;app .apk name&gt;</p></td>
        <td><p>Uninstalls the app with the specified .apk file name.</p></td>
      </tr>
      <tr>
        <td><p>adb uninstall -k &lt;app .apk name&gt;</p></td>
        <td><p>Uninstalls the .apk file without deleting its data.</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb update app 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb install -r app-name.apk</p></td>
        <td><p>reinstall the app and keep its data on the device</p></td>
      </tr>
      <tr>
        <td><p>adb install –k app-name.apk /local/path/</p></td>
        <td><p>Installs the app and retains its data without deleting it</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb home button press

{% highlight bash %}
adb shell am start -W -c android.intent.category.HOME -a android.intent.action.MAIN
{% endhighlight %}

### adb take screenshot 

{% highlight bash %}
adb shell screencap -p /sdcard/screenshot.png
{% endhighlight %}

### adb screen record

{% highlight bash %}

adb shell screenrecord /sdcard/NotAbleToLogin.mp4

{% endhighlight %}

### ShPref

{% highlight bash %}

replace org.example.app with your application id

{% endhighlight %}

### adb restart app 

{% highlight bash %}
adb shell 'am broadcast -a org.foo.app.sp.CLEAR --ez restart true'
{% endhighlight %}


### adb emulate device 

How to emulate device in adb: 

{% highlight bash %}
adb shell wm size 2048x1536
adb shell wm density 288
{% endhighlight %}

adb reset to default: 

{% highlight bash %}
adb shell wm size reset
adb shell wm density reset
{% endhighlight %}

## adb print text 

Potentially useful for input validation based attacks:

{% highlight bash %}
adb shell input text 'payload'
{% endhighlight %}



## adb key events 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p><code>adb shell input keyevent 3</code></p></td>
        <td><p>home btn</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 4</code></p></td>
        <td><p>back btn</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 5</code></p></td>
        <td><p>call</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 6</code></p></td>
        <td><p>end call</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 26</code></p></td>
        <td><p>turn android device on and off. it will toggle device to on/off status.</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 27</code></p></td>
        <td><p>camera</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 64</code></p></td>
        <td><p>open browser</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 66</code></p></td>
        <td><p>enter</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 67</code></p></td>
        <td><p>delete (backspace)</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 207</code></p></td>
        <td><p>contacts</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 220 / 221</code></p></td>
        <td><p>brightness down/up</p></td>
      </tr>
      <tr>
        <td><p><code>adb shell input keyevent 277 / 278 / 279</code></p></td>
        <td><p>cut/copy/paste</p></td>
      </tr>
    </tbody>
  </table>
</div>


## Useful file system paths 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th><code>PATH</code></th>
        <th><p>DESCRIPTION</p></th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>/data/data/&lt;package&gt;/databases</p></td>
        <td><p>App databases</p></td>
      </tr>
      <tr>
        <td><p>/data/data/&lt;package&gt;/shared_prefs/</p></td>
        <td><p>Shared preferences</p></td>
      </tr>
      <tr>
        <td><p>/data/app</p></td>
        <td><p>APK installed by user</p></td>
      </tr>
      <tr>
        <td><p>/system/app</p></td>
        <td><p>Pre-installed APK files</p></td>
      </tr>
      <tr>
        <td><p>/mmt/asec</p></td>
        <td><p>Encrypted apps (App2SD)</p></td>
      </tr>
      <tr>
        <td><p>/mmt/emmc</p></td>
        <td><p>Internal SD Card</p></td>
      </tr>
      <tr>
        <td><p>/mmt/adcard</p></td>
        <td><p>External/Internal SD Card</p></td>
      </tr>
      <tr>
        <td><p>/mmt/adcard/external_sd</p></td>
        <td><p>External SD Card</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb show device information 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb get-state</p></td>
        <td><p>Print device state</p></td>
      </tr>
      <tr>
        <td><p>adb get-serialno</p></td>
        <td><p>Get the serial number</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys iphonesybinfo</p></td>
        <td><p>Get the IMEI</p></td>
      </tr>
      <tr>
        <td><p>adb shell netstat</p></td>
        <td><p>List TCP connectivity</p></td>
      </tr>
      <tr>
        <td><p>adb shell pwd</p></td>
        <td><p>Print current working directory</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys battery</p></td>
        <td><p>Battery status</p></td>
      </tr>
      <tr>
        <td><p>adb shell pm list features</p></td>
        <td><p>List phone features</p></td>
      </tr>
      <tr>
        <td><p>adb shell service list</p></td>
        <td><p>List all services</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys activity &lt;package&gt;/&lt;activity&gt;</p></td>
        <td><p>Activity info</p></td>
      </tr>
      <tr>
        <td><p>adb shell ps</p></td>
        <td><p>Print process status</p></td>
      </tr>
      <tr>
        <td><p>adb shell wm size</p></td>
        <td><p>Displays the current screen resolution</p></td>
      </tr>
      <tr>
        <td><p>dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp'</p></td>
        <td><p>Print current app's opened activity</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb show package info 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb shell list packages</p></td>
        <td><p>List package names</p></td>
      </tr>
      <tr>
        <td><p>adb shell list packages -r</p></td>
        <td><p>List package name + path to APKs</p></td>
      </tr>
      <tr>
        <td><p>adb shell list packages -3</p></td>
        <td><p>List third-party package names</p></td>
      </tr>
      <tr>
        <td><p>adb shell list packages -s</p></td>
        <td><p>List only system packages</p></td>
      </tr>
      <tr>
        <td><p>adb shell list packages -u</p></td>
        <td><p>List package names + uninstalled</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys package packages</p></td>
        <td><p>List info on all apps</p></td>
      </tr>
      <tr>
        <td><p>adb shell dump &lt;name&gt;</p></td>
        <td><p>List info on one package</p></td>
      </tr>
      <tr>
        <td><p>adb shell path &lt;package&gt;</p></td>
        <td><p>Path to the APK file</p></td>
      </tr>
    </tbody>
  </table>
</div>


### adb system settings 

e.g., adjust battery level, resolution etc 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb shell dumpsys battery set level &lt;n&gt;</p></td>
        <td><p>Change the battery level from 0 to 100</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys battery set status &lt;n&gt;</p></td>
        <td><p>Change the battery status to unknown, charging, discharging, not charging, or full</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys battery reset</p></td>
        <td><p>Reset the battery</p></td>
      </tr>
      <tr>
        <td><p>adb shell dumpsys battery set usb &lt;n&gt;</p></td>
        <td><p>Change the status of USB connection to ON or OFF</p></td>
      </tr>
      <tr>
        <td><p>adb shell wm size WxH</p></td>
        <td><p>Sets the resolution to WxH</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb device commands 

<div class="mobile-side-scroller">
  <table>
    <thead>
      <tr>
        <th>COMMAND</th>
        <th>DESCRIPTION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><p>adb reboot-recovery</p></td>
        <td><p>Reboot device into recovery mode</p></td>
      </tr>
      <tr>
        <td><p>adb reboot fastboot</p></td>
        <td><p>Reboot device into fastboot mode</p></td>
      </tr>
      <tr>
        <td><p>adb shell screencap -p "/tmp/screenshot.png"</p></td>
        <td><p>Capture screenshot</p></td>
      </tr>
      <tr>
        <td><p>adb shell screenrecord "/tmp/record.mp4"</p></td>
        <td><p>Record device screen</p></td>
      </tr>
      <tr>
        <td><p>adb backup -apk -all -f backup.ab</p></td>
        <td><p>Backup settings and apps</p></td>
      </tr>
      <tr>
        <td><p>adb backup -apk -shared -all -f backup.ab</p></td>
        <td><p>Backup settings, apps, and shared storage</p></td>
      </tr>
      <tr>
        <td><p>adb backup -apk -nosystem -all -f backup.ab</p></td>
        <td><p>Backup only non-system apps</p></td>
      </tr>
      <tr>
        <td><p>adb restore backup.ab</p></td>
        <td><p>Restore a previous backup</p></td>
      </tr>
      <tr>
        <td><p>adb shell am start|startservice|broadcast &lt;INTENT&gt;[&lt;COMPONENT&gt;] -a &lt;ACTION&gt;</p></td>
        <td><p>Start activity intent</p></td>
      </tr>
      <tr>
        <td><p>adb shell am start -a android.intent.action.VIEW -d URL</p></td>
        <td><p>Open URL</p></td>
      </tr>
      <tr>
        <td><p>adb shell am start -t image/* -a android.intent.action.VIEW</p></td>
        <td><p>Opens gallery</p></td>
      </tr>
    </tbody>
  </table>
</div>

### adb backup 

{% highlight bash %}

adb backup -f chrome.ab -apk com.android.chromer device

{% endhighlight %}

## Conclusion 

We hope this ADB cheat sheet was useful in covering the usage of the ADB tool for performing mobile app security testing against Android Apps. 

## Sources 

- [https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8](https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8)
- [https://developer.android.com/tools/adb](https://developer.android.com/tools/adb) 