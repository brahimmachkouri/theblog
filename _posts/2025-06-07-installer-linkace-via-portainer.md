---
layout: post
date: 2025-06-07 13:00:00
title: Installation de LinkAce via Portainer
category: dev
tags: mysql mariadb
---

![LinkAce]({{ "/assets/images/linkace.png" | relative_url }})

[LinkAce](https://www.linkace.org/) est une application web libre et open-source qui permet de gérer, organiser et archiver tes favoris (liens/bookmarks) de manière centralisée, auto-hébergée et collaborative.

Voici le docker-compose pour la Stack dans Portainer :

```yaml
version: "3.7"

services:
  linkace:
    image: linkace/linkace:latest
    container_name: linkace
    depends_on:
      - db
      - redis
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - linkace-app-data:/app/storage
    environment:
      - APP_KEY=base64:<clé_à_générer>
      - APP_URL=http://localhost:8080
      - DB_CONNECTION=mysql
      - DB_HOST=db
      - DB_PORT=3306
      - DB_DATABASE=linkace
      - DB_USERNAME=linkace
      - DB_PASSWORD=motdepassefort
      - REDIS_HOST=redis
      - REDIS_PASSWORD=unautremdpfort
      - APP_DEBUG=true
      - APP_TIMEZONE=Europe/Paris

  db:
    image: mariadb:11.5
    container_name: linkace-db
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=motdepassefort
      - MYSQL_DATABASE=linkace
      - MYSQL_USER=linkace
      - MYSQL_PASSWORD=motdepassefort
    command: mariadbd --character-set-server=utf8mb4 --collation-server=utf8mb4_bin
    volumes:
      - linkace-db-data:/var/lib/mysql

  redis:
    image: bitnami/redis:7.4
    container_name: linkace-redis
    restart: unless-stopped
    environment:
      - REDIS_PASSWORD=unautremdpfort

volumes:
  linkace-app-data:
  linkace-db-data:
```

Pour générer la clé : 
```bash
docker run --rm linkace/linkace php artisan key:generate --show 
```

### Import

Il faut d'abord générer un Cron Token. Aller dans System Settings (http://127.0.0.1:8080/settings/system) et cliquer sur le bouton **Generate Token**.

Il est possible d'importer des liens à partir d'un export Firefox par exemple (bookmarks.html).

Aller dans Import (http://127.0.0.1:8080/import) afin d'aller chercher le fichier bookmarks.html, puis cliquer sur le bouton **Start Import**.

Ensuite, il convient de planifier, directement sur la machine hôte (celle qui exécute Docker, que ce soit votre ordinateur ou un serveur distant), une tâche cron toutes les minutes, par exemple, afin de lancer le traitement d’importation par lots.

Pour créer un nouveau cron : 
```bash
crontab -e
```

Puis saisir cette ligne dans le fichier : 
```
* * * * * wget -qO- http://localhost:8080/cron/{Mettre_ici_le_Cron_Token} >> /tmp/linkace_import.log 2>&1
```
