---
layout: post
date: 2023-01-10 16:57:26
title: Connexion à un répertoire samba partagé avec macos
category: macos 
tags: smb samba mac
---  

⚠️ Utilisez le VPN de l'établissement si votre serveur de fichier est hébergé en interne.

Depuis le menu Finder, cliquez sur "Aller" > "Se connecter au serveur..." (ou command+K):  
![image1]({{ "/assets/images/connect_samba_macos_img1.png" | relative_url }})


Dans le champ, saisissez "smb://login@adresseip", en remplaçant login par votre propre login, et adresseip par l'adresse IP de votre serveur (1) :
![image2]({{ "/assets/images/connect_samba_macos_img2.png" | relative_url }})

Pour ajouter le serveur aux serveurs favoris, cliquez sur le bouton '+' (2). Puis cliquez sur le bouton "Connecter" (3).

Cliquez une nouvelle fois sur "Se connecter" :
![image3]({{ "/assets/images/connect_samba_macos_img3.png" | relative_url }})

Lorsque la fenêtre suivante apparaît, entrez vos identifiants (1,2) et cliquez sur Enregistrer le mot de passe dans mon trousseau (3) pour éviter de le resaisir la fois prochaine.  
Cliquez sur le bouton "Connecter" (4) :
![image4]({{ "/assets/images/connect_samba_macos_img4.png" | relative_url }})

