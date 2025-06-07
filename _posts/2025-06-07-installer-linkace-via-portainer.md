---
layout: post
date: 2025-06-07 13:00:00
title: Installation de LinkAce via Portainer
category: dev
tags: mysql mariadb
---

![LinkAce](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/linkace.png)

## Installation de LinkAce via Portainer

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

Ensuite, il faut programmer un cron toutes les minutes afin de déclencher le traitement de l'import par lots :

```bash
crontab -e
```

Puis saisir cette ligne dans le fichier : 
```
* * * * * /usr/local/bin/docker run --rm linkace/linkace php artisan key:generate --show >> /tmp/linkace_keygen.log 2>&1
```

Veillez à bien indiquer le bon chemin de l'exécutable docker sur votre système.