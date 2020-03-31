---
date: 2020-03-31T01:00:00+06:00
lastmod: 2020-03-31T01:00:00+06:00
title: Visual Studio to Mac Connection Issue Caused by macOS update 10.15.14
authors: ["foysalahamed"]
categories:
  - Programming
tags:
  - xamarin
  - c#
  - bug
  - macOS
  - iOS
slug: visual-studio-to-mac-connection-issue-caused-by-macos-update-10_15_14
comments: false
toc: false
draft: false
---
Updating my macOS to **10.15.14** killed my full two days. I am using **Visual Studio 2019 + Windows 10** to develop a Xamarin Forms app. After updating my macOS, I started to face a connection issue when Visual Studio 2019 try to connect to my Local Mac Mini from the Windows machine I am currently working on.

> An error occurred while generating the SSH keys. Please check that the environment is properly configured. Details: cat: /Users/cc/Library/Caches/Xamarin/XMA/Keys/9a3289b9-d636-49b2-b0e4-5ef721a64fcd: No such file or directory

This issue is caused by an update in macOS, from **10.15.4**. It causes a bug in Visual Studio's SSH layer which makes the SCP usage to not work.

This is a temporary workaround until Microsoft release the new update.

- Copy the content of file `C:\Users\xx\AppData\Local\Xamarin\MonoTouch\id_rsa.pub`.
- Paste it into mac `~/.ssh/authorized_keys` in a New Line.
- Download the SCP binary from the link [**hosted in Xamarin official GitHub repo**](https://github.com/xamarin/xamarin-macios/files/4386337/scp.zip) to your macOS and decompress it.
- Disable read-only security on OS X Volume / SIP:
  - Reboot the system and hold down `Command+R (⌘+R)` keys simultaneously when you hear the startup chime; this will boot macOS into Recovery Mode.
  - Once in Recovery mode, open a Terminal window from the Utilities drop-down menu at the top of the screen.
  - Type in the Terminal: `csrutil disable`.
  - Hit Enter, and you’ll see a message saying that **System Integrity Protection has been disabled and that the Mac needs to restart for changes to take effect.**
  - Restart the machine (enter `reboot` in the Terminal, or use the Apple menu to find the Restart option).
- Now run these commands in macOS Terminal:

```sh
sudo mount -uw /
sudo cp /usr/bin/scp /usr/bin/scp.bak
sudo cp ~/Downloads/scp /usr/bin/scp
```

- Enable read-only security on OS X Volume / SIP:
  - Reboot into Recovery Mode again (⌘+R at system chime).
  - Open a Terminal and enter: `csrutil enable`.
  - Reboot.

Try now and it should work.
