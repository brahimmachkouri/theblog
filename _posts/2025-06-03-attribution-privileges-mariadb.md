---
layout: post
date: 2025-06-03 13:00:00
title: Attribution de privilèges MySQL/MariaDB
category: dev
tags: mysql mariadb
---

## Objectif

Accorder tous les privilèges à un utilisateur sur une base de données MariaDB, avec accès depuis n'importe quelle adresse IP (et pas uniquement `localhost`).

---

## Commande SQL

```sql
GRANT ALL PRIVILEGES ON nom_base.* TO 'nom_utilisateur'@'%' IDENTIFIED BY 'mot_de_passe';
FLUSH PRIVILEGES;
```

### Paramètres :

- `nom_base` : nom de la base de données
- `nom_utilisateur` : nom de l'utilisateur MariaDB
- `mot_de_passe` : mot de passe associé
- `@'%'` : autorise l'accès depuis toutes les adresses IP

---

## Exemple

```sql
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'%' IDENTIFIED BY 'MotDePasseFort123!';
FLUSH PRIVILEGES;
```

---

## Variantes utiles

### Accès depuis une IP précise

```sql
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'192.168.1.42' IDENTIFIED BY 'MotDePasseFort123!';
```

### Accès depuis un sous-réseau local

```sql
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'192.168.1.%' IDENTIFIED BY 'MotDePasseFort123!';
```

### Accès depuis localhost (par défaut, mais à expliciter si besoin)

```sql
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'localhost' IDENTIFIED BY 'MotDePasseFort123!';
```

---

## Finalisation

Toujours exécuter :
```sql
FLUSH PRIVILEGES;
```

---

## Astuce

Si le compte existe déjà avec une autre restriction (ex : uniquement localhost), il peut être nécessaire de créer explicitement une autre entrée avec `@'%'` ou de modifier l'existante avec :

```sql
CREATE USER 'webadmin'@'%' IDENTIFIED BY 'MotDePasseFort123!';
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'%';
FLUSH PRIVILEGES;
```

---

## Vérification des droits

```sql
SHOW GRANTS FOR 'webadmin'@'%';
```

---

## Sécurité

- Veille à utiliser des mots de passe forts.
- Pour la production, privilèges plus restrictifs recommandés : `SELECT`, `INSERT`, etc. 
- Utiliser `REVOKE` pour retirer les droits si besoin.

```sql
REVOKE ALL PRIVILEGES ON projet_web.* FROM 'webadmin'@'%';
```
