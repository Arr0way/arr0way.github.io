---
layout: blog_item
title: "Encrypted Notes App Solution (iOS, Android, MacOS, Linux, Windows)"
date:   2022-01-17 10:29:10
author: Arr0way
description: 'A platform independent E2E Encrypted Notes App Solution using open-source tools Markdown + Cryptomator.'
categories: [SecOps]
tags:

- 'SecOps'
---

* list element with functor item
{:toc}


## E2E Encrypted Notes App Solution 

People make a lot of notes, and personally I wanted to ensure (with a good degree of certainty) that organizations and/or others could not end up reading my notes. Therefore, I set to work on finding a platform independent E2E encrypted note taking app, to find there basically is not one. Arguably you could use Bitwarden or 1Password secure notes but that is not very practical or scalable (IMO). 

Note: if you are on iOS/MacOS only you could use Apple Notes and use the encrypted notes functionality, but then you are locked into that ecosytem. 

***Caveat:*** Only tested between Windows/MacOS/iOS however, there should be no reason this would not work on any other platform that Cryptomator supports.

## The Cobbled Together Encrypted Notes App Solution

### Requirements

1. Must be End-to-End Encrypted AES256 (and encrypt file names)
2. The end user must hold the encryption key
3. It must be possible to sync notes back and forth from mobile to desktop, and desktop to mobile
4. Somewhat scalable / possible to migrate in the future
5. Not be impossible to use once setup, or to difficult that it puts you off bothering to make notes

As there is no one app solution for the above the following system was implemented, which works but is a little clunky to say the least. 

1. Cryptomator for encryption (the desktop app has been audited by Cure53)
2. Password manager of choice for key management (if you lose the key it is game over, you will have lost all your notes)
3. Markdown editor or Obsidian for note taking 

## Desktop Setup: MacOS or Windows 

The desktop setup is relatively easy, setup a vault with Cryptomator and use a cloud provider for the encrypted vault location (to allow for syncing, unless you plan to manually sync). When the vault is unlocked (open) there will be a localhost volume mounted in MacOS this is where you need to place your Obsidian vault or markdown files. 

## Mobile Setup: iOS

Buy Cryptomator from the app store (it is a one off payment not a sub). Open your obsidian vault from your cloud storage provider and enter your password, optionally you could setup FaceID for the vault unlocking. Once the vault is mounted, it will appear in the iOS Files app, it will appear at the bottom of the files list as a Cryptomator icon.

There is an Obsidian iOS app, however it does not appear to be able to open the vault from any other location that the pre-determined locations the applications uses. For this reason on iOS I use iWriter to open and edit notes, again you'll need to buy this app but it is a one time fee. An additional caveat is that you cannot open the obsidian vault directory within iWriter and can only open one note at a time by navigating to ```Open Other``` > ```Browse``` > ```Cryptomator``` and then opening the note you wish to edit.  

If this all looks like a lot of work, it is. 

If anyone has a more eloquent solution, tweet me @Arr0way 

