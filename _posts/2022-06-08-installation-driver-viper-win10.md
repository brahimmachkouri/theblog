---
layout: post
date: 2022-06-08 19:47:04
title: Install viper usb driver on Windows 10
category: materiel
tags: gamecube viper driver usb
---

Téléchargements :

[Viper Extreme USB Driver](https://github.com/brahimmachkouri/theblog/raw/main/files/ViperUSBWin7Driver.7z)  
[Viper USB Flasher](https://github.com/brahimmachkouri/theblog/raw/main/files/ViperFlasherUSB10.7z) 

Le driver est non signé, il est donc nécessaire de passer Windows en mode Test.

## Test Mode

### Activation 

En ligne de commande avec les droits admin :
```
bcdedit -set TESTSIGNING ON
```
Redémarrer l'OS. Si l'opération a réussi, il y a une indication sur le côté droit au bas de l'écran (watermark) : Test Mode.  

Pour installer le driver USB de la Viper, allumez la gamecube en ayant pris soin de brancher la Viper en USB au PC.
S'il est demandé d'installer le driver via Windows Update, refusez.
Installez le driver en spécifiant le répertoire dans lequel se trouve les fichiers décompressés du drivers.
Cliquez sur "Continuer" s'il est dit que le driver n'est pas certifié.

### Désactivation

En ligne de commande avec les droits admin :
```
bcdedit -set TESTSIGNING OFF
```
Redémarrer l'OS.

## Flashage :
Assurez vous que le switch est en position 2 on sur le viper extreme, et que le câble USB est bien branché, et la gamecube allumée. 
Lancez Viper Flasher, et chargez le fichier (File > Load Bios) dont l'extension est vgc.
Cliquez sur Write pour flasher le BIOS de la puce.
