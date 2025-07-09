---
layout: post
date: 2025-06-16 13:00:00
title: Guide d'activation de Windows 11
category: système
tags: activation windows
---

![activation](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/activation.png)

> Ce guide pas-à-pas explique comment saisir une nouvelle clé de produit.

## Prérequis

- **Droits administrateur** sur la machine.
- **Connexion Internet** fonctionnelle (obligatoire pour l'activation en ligne).
- **Clé de produit Windows 11** valide (25 caractères, format `XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`).

---

![Activation1](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/activation1.png)

## Étape 1 : ouvrir les paramètres d'activation

![Recherche « activation » dans le menu Démarrer](activation1.png)

1. Cliquez sur le bouton **Démarrer** (ou appuyez sur la touche **Windows**).
2. Tapez **activation** dans la barre de recherche (flèche 1).
3. Cliquez sur **Ouvrir** sous *Paramètres d'activation* (flèche 2).

---

![Activation2](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/activation2.png)

## Étape 2 : vérifier l'état d'activation

![Fenêtre Système > Activation](activation2.png)

- La ligne **État d'activation** indique si Windows est **Actif**.
- Pour changer la clé, sélectionnez **Modifier la clé de produit** (flèche verte).

---

![Activation3](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/activation3.png)

## Étape 3 : saisir une nouvelle clé de produit

![Fenêtre « Saisir une clé de produit »](activation3.png)

1. Saisissez votre nouvelle clé (25 caractères).
2. Cliquez sur **Suivant** puis sur **Activer**.
3. Attendez la confirmation. Un symbole ✓ confirme la réussite.

---

## Dépannage

| Code d'erreur | Signification | Correctif rapide |
|---------------|--------------|------------------|
| `0xC004F050` | Clé invalide | Vérifier la clé et ressaisir. |
| `0xC004F074` | Serveur injoignable | Vérifier la connexion Internet et réessayer. |

Pour d'autres codes, ouvrez **Paramètres > Système > Activation > Résoudre les problèmes d'activation**.

---

## Pour aller plus loin

- [Documentation officielle Microsoft](https://support.microsoft.com/windows/activation)
- Commande avancée : `slmgr /dlv` dans un PowerShell *en tant qu'administrateur* pour un rapport détaillé.