---
layout: post
date: 2021-10-31 14:39:00
title: Compilation du driver cp210x sous Ubuntu (M5Paper)
category: materiel
tags: m5paper linux driver cp210x
---

![MPaper](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/m5paper.jpg)

Pour compiler le driver cp210x, il faut le télécharger ([Linux 3.x.x/4.x.x/5.x.x VCP Driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)), puis ouvrir le zip pour trouver le drivers écrit en C destiné à être compilé.

Il faut au préalable installer quelques paquets :
```
sudo apt install unzip make gcc linux-image-extra-virtual
```
Décompresser le fichier zip téléchargé :
```
unzip Linux_3.x.x_4.x.x_VCP_Driver_Source.zip
```
Puis procéder à la compilation :
```
make
```

Ensuite, copier le driver dans le bon répertoire :
```
sudo cp cp210x.ko /lib/modules/$(uname -r)/kernel/drivers/usb/serial/
```

Et enfin, charger les modules :
```
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/usb/serial/usbserial.ko
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/usb/serial/cp210x.ko
```

Résultat de la commande `lsmod | grep cp210` (les sizes peuvent être différentes) :
```
(Module                 Size  Used by)
cp210x                 36864  0
usbserial              53248  1 cp210x
```
