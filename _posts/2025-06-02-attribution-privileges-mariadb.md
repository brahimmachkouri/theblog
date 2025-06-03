---
layout: post
date: 2025-06-02 13:00:00
title: Attribution de privilèges MySQL/MariaDB
category: dev
tags: mysql mariadb
---

Accorder des privilèges à un utilisateur sur une base de données MariaDB/MySQL, avec gestion des accès depuis différentes adresses IP et bonnes pratiques de sécurité.

---

### Syntaxe moderne (MySQL 8.0+ / MariaDB 10.2+)

La bonne pratique actuelle consiste à séparer la création d'utilisateur de l'attribution des privilèges :

```sql
-- 1. Créer l'utilisateur
CREATE USER 'nom_utilisateur'@'%' IDENTIFIED BY 'mot_de_passe';

-- 2. Accorder les privilèges
GRANT ALL PRIVILEGES ON nom_base.* TO 'nom_utilisateur'@'%';

-- 3. Appliquer les changements
FLUSH PRIVILEGES;
```

#### Paramètres

- `nom_base` : nom de la base de données
- `nom_utilisateur` : nom de l'utilisateur MariaDB/MySQL
- `mot_de_passe` : mot de passe associé
- `@'%'` : autorise l'accès depuis toutes les adresses IP

> 💡 **Pourquoi cette approche ?** Elle offre plus de flexibilité, une meilleure gestion des erreurs et respecte le principe de séparation des responsabilités.

---

### Exemple pratique

Créons un utilisateur pour une application e-commerce :

```sql
-- Étape 1 : Créer l'utilisateur
CREATE USER 'ecommerce_user'@'%' IDENTIFIED BY 'SecurePass2025!';

-- Étape 2 : Créer la base de données (si nécessaire)
CREATE DATABASE IF NOT EXISTS boutique_en_ligne;

-- Étape 3 : Accorder les privilèges
GRANT ALL PRIVILEGES ON boutique_en_ligne.* TO 'ecommerce_user'@'%';

-- Étape 4 : Appliquer les changements
FLUSH PRIVILEGES;

-- Étape 5 : Vérifier que tout fonctionne
SHOW GRANTS FOR 'ecommerce_user'@'%';
```

#### Résultat attendu

```bash
+--------------------------------------------------------------------+
| Grants for ecommerce_user@%                                       |
+--------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `ecommerce_user`@`%`                       |
| GRANT ALL PRIVILEGES ON `boutique_en_ligne`.* TO `ecommerce_user`@`%` |
+--------------------------------------------------------------------+
```

---

### Gestion des utilisateurs existants

#### Vérifier l'existence d'un utilisateur

```sql
SELECT User, Host FROM mysql.user WHERE User = 'webadmin';
```

#### Recréer un utilisateur (MariaDB 10.1.3+ / MySQL 8.0+)

```sql
DROP USER IF EXISTS 'webadmin'@'%';
CREATE USER 'webadmin'@'%' IDENTIFIED BY 'MotDePasseFort2025!';
GRANT ALL PRIVILEGES ON boutique_en_ligne.* TO 'webadmin'@'%';
FLUSH PRIVILEGES;
```

#### Modifier un utilisateur existant

```sql
-- Changer le mot de passe (syntaxe moderne)
ALTER USER 'webadmin'@'%' IDENTIFIED BY 'NouveauMotDePasse2025!';
FLUSH PRIVILEGES;
```

---

### Variantes de restrictions d'accès

#### Accès depuis une IP précise

```sql
CREATE USER 'webadmin'@'192.168.1.42' IDENTIFIED BY 'MotDePasseFort2025!';
GRANT ALL PRIVILEGES ON boutique_en_ligne.* TO 'webadmin'@'192.168.1.42';
FLUSH PRIVILEGES;
```

#### Accès depuis un sous-réseau local

```sql
CREATE USER 'webadmin'@'192.168.1.%' IDENTIFIED BY 'MotDePasseFort2025!';
GRANT ALL PRIVILEGES ON boutique_en_ligne.* TO 'webadmin'@'192.168.1.%';
FLUSH PRIVILEGES;
```

#### Accès depuis localhost uniquement

```sql
CREATE USER 'webadmin'@'localhost' IDENTIFIED BY 'MotDePasseFort2025!';
GRANT ALL PRIVILEGES ON boutique_en_ligne.* TO 'webadmin'@'localhost';
FLUSH PRIVILEGES;
```

#### Accès depuis plusieurs sources

```sql
-- Créer plusieurs entrées pour le même utilisateur
CREATE USER 'webadmin'@'localhost' IDENTIFIED BY 'MotDePasseFort123!';
CREATE USER 'webadmin'@'192.168.1.%' IDENTIFIED BY 'MotDePasseFort123!';

GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'localhost';
GRANT ALL PRIVILEGES ON projet_web.* TO 'webadmin'@'192.168.1.%';
```

---

### Privilèges granulaires (recommandé pour la production)

Au lieu d'utiliser `ALL PRIVILEGES`, accordez uniquement les droits nécessaires :

#### Privilèges de base pour une application web

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON projet_web.* TO 'webadmin'@'%';
```

#### Privilèges étendus pour un administrateur

```sql
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER, INDEX 
ON projet_web.* TO 'webadmin'@'%';
```

#### Privilèges en lecture seule

```sql
GRANT SELECT ON projet_web.* TO 'readonly_user'@'%';
```

#### Liste des privilèges principaux

- `SELECT` : lecture des données
- `INSERT` : insertion de nouvelles données
- `UPDATE` : modification des données existantes
- `DELETE` : suppression des données
- `CREATE` : création de tables/bases
- `DROP` : suppression de tables/bases
- `ALTER` : modification de structure
- `INDEX` : gestion des index
- `GRANT OPTION` : possibilité d'accorder des privilèges à d'autres

---

### Configuration réseau requise

Pour permettre les connexions distantes, vérifiez la configuration de MariaDB/MySQL :

#### Fichier de configuration (`/etc/mysql/my.cnf` ou `/etc/my.cnf`)

```ini
[mysqld]
# Commenter ou modifier cette ligne pour autoriser les connexions distantes
# bind-address = 127.0.0.1
bind-address = 0.0.0.0

# Ou spécifier une IP précise
# bind-address = 192.168.1.10
```

#### Redémarrer le service

```bash
sudo systemctl restart mysql
# ou
sudo systemctl restart mariadb
```

---

### Vérification et maintenance

#### Vérifier les droits accordés

```sql
SHOW GRANTS FOR 'webadmin'@'%';
```

#### Lister tous les utilisateurs

```sql
SELECT User, Host, authentication_string FROM mysql.user;
```

#### Vérifier les connexions actives

```sql
SHOW PROCESSLIST;
```

---

### Révocation des privilèges

#### Révoquer des privilèges spécifiques

```sql
REVOKE INSERT, UPDATE ON projet_web.* FROM 'webadmin'@'%';
```

#### Révoquer tous les privilèges

```sql
REVOKE ALL PRIVILEGES ON projet_web.* FROM 'webadmin'@'%';
```

#### Supprimer un utilisateur

```sql
DROP USER 'webadmin'@'%';
FLUSH PRIVILEGES;
```

---

### Bonnes pratiques de sécurité

#### Mots de passe

- Utilisez des mots de passe forts (12+ caractères, mixte majuscules/minuscules/chiffres/symboles)
- Changez régulièrement les mots de passe
- Utilisez des gestionnaires de mots de passe

#### Principe du moindre privilège

- N'accordez que les privilèges strictement nécessaires
- Évitez `ALL PRIVILEGES` en production
- Créez des utilisateurs spécialisés par fonction

#### Restrictions réseau

- Limitez l'accès aux IP nécessaires plutôt qu'utiliser `%`
- Utilisez des VPN ou des tunnels SSH pour les accès distants
- Configurez un pare-feu approprié

#### Exemple d'architecture sécurisée

```sql
-- Utilisateur application (lecture/écriture limitée)
CREATE USER 'app_user'@'192.168.1.%' IDENTIFIED BY 'AppPassword123!';
GRANT SELECT, INSERT, UPDATE, DELETE ON myapp.* TO 'app_user'@'192.168.1.%';

-- Utilisateur admin (accès complet mais local uniquement)
CREATE USER 'db_admin'@'localhost' IDENTIFIED BY 'AdminPassword456!';
GRANT ALL PRIVILEGES ON myapp.* TO 'db_admin'@'localhost';

-- Utilisateur lecture seule pour les rapports
CREATE USER 'reporter'@'%' IDENTIFIED BY 'ReportPassword789!';
GRANT SELECT ON myapp.* TO 'reporter'@'%';

FLUSH PRIVILEGES;
```

---

### Dépannage courant

#### Erreur "Access denied"

1. Vérifiez que l'utilisateur existe pour le bon host
2. Vérifiez le mot de passe
3. Contrôlez les privilèges accordés
4. Vérifiez la configuration réseau

#### Connexion impossible depuis l'extérieur

1. Vérifiez `bind-address` dans la configuration
2. Contrôlez les règles de pare-feu
3. Vérifiez que le port 3306 est ouvert
4. Testez avec `telnet server_ip 3306`

#### Commandes de diagnostic

```sql
-- Voir la configuration actuelle
SHOW VARIABLES LIKE 'bind_address';

-- Voir les tentatives de connexion
SHOW STATUS LIKE 'Connections';

-- Voir les erreurs de connexion
SHOW STATUS LIKE 'Aborted_connects';
```
