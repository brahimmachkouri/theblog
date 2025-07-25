---
layout: post
date: 2021-11-05 11:29:00
title: Upgrade Zalman VE-300 firmware to an iodd 2531 firmware
category: materiel
tags: zalman iodd firmware
---

![Zalman VE-300]({{ "/assets/images/iodd.jpg" | relative_url }})

Here is a working solution to get the latest IODD firmware on a Zalman VE-300.

- Download and extract the file [zalman_ve300_to_iodd.7z](https://github.com/brahimmachkouri/ioddfirmware/raw/master/zalman_ve300_to_iodd.7z).
- Run the firmware writer iODD2531_user_fw_writer_02(R1288F)_x86.EXE as administrator. Warning : it's the firmware for FAT/exFAT. If you want to apply this for NTFS, search for the firmware with a "N" at the end : R1288N (N stands for NTFS, and in R1288F, F stands for FAT)
- Run the EDA tool as administrator
- Drag-and-drop the crosshair of the EDA tool on the disabled "Update" button of the firmware writer
- Activate the "enabled" checkbox in the EDA tool. On eda  form, in the "Modify window" tab, uncheck the "WS_DISABLED" check box and click on "Set the window text" button : the "Update" button should now be enabled. 
- With your VE-300 device connected, press the "Update" button. The old updater is not so picky about the installed firmware on the device as the recent one.
- After finishing the update, power off the VE-300 and reconnect it. Now you have a "iodd2531" device with an outdated firmware.
- Run the latest firmware writer iODD_2531_user_fw_writer_04(R1600F)_x86.EXE. Since you have a valid iodd firmware on your device, the updater is happy with it. Just press the "Update" button. Maybe you'll find newer versions of the 2531 firmware in this page : http://iodd.kr/wordpress/support/download/#tab-e72d0adcf55af0bdf71
- After finishing the update, power off the VE-300 and reconnect it.
