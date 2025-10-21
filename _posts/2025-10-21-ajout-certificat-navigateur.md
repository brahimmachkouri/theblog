---
layout: post
date: 2025-10-21 11:00:00
title: Certificat - Installation d'un certificat .p12 pour un navigateur
category: système
tags: certificat windows
---

# ⚖️ Guide d'installation d'un certificat `.p12` dans Windows 11 (utilisateur courant / navigateur web)

## 🎯 Objectif
Installer un certificat client au format `.p12` pour une **utilisation dans les navigateurs web** tels que Edge, Chrome ou Firefox, pour **l'utilisateur courant**.

---

## 🥉 Étape 1 : Comprendre le fichier `.p12`

Un fichier `.p12` contient :
- 🔐 Le **certificat utilisateur**
- 🔑 La **clé privée**
- 🧾 Éventuellement la **chaîne de certification** (autorité intermédiaire / racine)

Ce fichier est **protégé par un mot de passe**, que tu devras saisir lors de l’importation.

---

## 🦯o Étape 2 : Importation via l’interface graphique

1. **Double-clique** sur le fichier `moncertificat.p12`
2. Sélectionne **Utilisateur actuel**  
   > car le certificat doit être disponible uniquement pour ton compte Windows
3. Clique sur **Suivant**
4. Clique sur **Parcourir**, puis sélectionne ton fichier `.p12`
5. Entre le **mot de passe** du certificat
6. Coche :
   - ✅ *Marquer cette clé comme exportable*
   - ✅ *Inclure toutes les propriétés étendues*
7. Sélectionne :
   - **Placer tous les certificats dans le magasin suivant**
   - Clique sur **Parcourir** → choisis **Personnel**
8. Clique sur **Suivant** puis **Terminer**
9. Un message “**L’importation a réussi**” s’affiche ✅

---

## 🌐 Étape 3 : Vérification dans le navigateur

### Microsoft Edge / Google Chrome
1. Ouvre les **Paramètres**
2. Recherche **certificats**
3. Va dans :
   ```
   Paramètres → Confidentialité et sécurité → Sécurité → Gérer les certificats
   ```
4. Onglet **Personnel** → tu dois voir ton certificat listé.

### Mozilla Firefox
> Firefox n’utilise **pas** le magasin Windows par défaut.

1. Menu ☰ → **Paramètres**
2. Onglet **Vie privée et sécurité**
3. Descends jusqu’à **Certificats**
4. Clique sur **Afficher les certificats**
5. Onglet **Vos certificats** → **Importer**
6. Sélectionne ton fichier `.p12` → entre le mot de passe

---

## ⚙️ Étape 4 : Installation via PowerShell (option automatisée)

Si tu préfères la méthode en ligne de commande :

```powershell
$password = Read-Host -AsSecureString "Mot de passe du fichier .p12"
Import-PfxCertificate -FilePath "C:\Users\<ton_user>\Documents\moncertificat.p12" -CertStoreLocation "Cert:\CurrentUser\My" -Password $password
```

- 📍 `Cert:\CurrentUser\My` = magasin “Personnel” de l’utilisateur
- 🔒 Nécessite **aucun privilège administrateur**

---

## 🧾 Étape 5 : Vérification via PowerShell

```powershell
Get-ChildItem Cert:\CurrentUser\My | Format-List Subject, Issuer, NotAfter
```

Tu verras ton certificat avec :
- **Subject** (ton nom / identifiant)
- **Issuer** (autorité émettrice)
- **NotAfter** (date d’expiration)

---

## ✅ Résumé

| Étape | Action | Détail |
|-------|---------|--------|
| 1 | Double-cliquer sur `.p12` | Lancer l’assistant d’importation |
| 2 | Choisir “Utilisateur actuel” | Installation pour ton compte |
| 3 | Entrer le mot de passe | Déverrouille le fichier |
| 4 | Choisir “Personnel” | Emplacement correct pour navigateur |
| 5 | Vérifier via navigateur | Chrome/Edge = magasin Windows, Firefox = import manuel |

---

## 💡 Astuce
Tu peux **exporter** ce certificat vers un autre poste Windows ou macOS :
1. Ouvre `certmgr.msc`
2. Dossier **Personnel → Certificats**
3. Clique droit → **Toutes les tâches → Exporter**
4. Coche *“Exporter la clé privée”* → choisis le format `.pfx`
5. Protège-le avec un mot de passe fort 🔐


