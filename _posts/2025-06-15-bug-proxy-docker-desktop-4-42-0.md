---
layout: post
date: 2025-06-15 13:00:00
title: Bug Proxy dans Docker Desktop 4.42.0
category: système
tags: docker
---

![BugProxy]({{ "/assets/images/docker-proxy-bug.png" | relative_url }})


Il y a un problème de détection de proxy dans la version 4.42.0 de Docker Desktop qui touche toutes les plateformes.

La solution pour l'instant est d'aller dans Resources > Proxies, et activer la configuration proxy manuelle.