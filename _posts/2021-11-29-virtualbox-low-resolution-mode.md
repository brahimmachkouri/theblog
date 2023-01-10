---
layout: post
date: 2021-11-29 11:34:00
title: Virtualbox low resolution mode
category: virtualisation
tags: virtualbox
---

You have to change the info.plist with admin rights. For example in terminal type:
```
sudo nano /Applications/VirtualBox.app/Contents/Resources/VirtualBoxVM.app/Contents/Info.plist
```

Then you can change the following key to false:
```
<key>NSHighResolutionCapable</key> <true/>
```
should now be :
```
<key>NSHighResolutionCapable</key> <false/>
```
