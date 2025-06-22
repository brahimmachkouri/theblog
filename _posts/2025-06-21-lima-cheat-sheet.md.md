---
layout: post
date: 2025-06-21 12:30:00
title: Lima - Ubuntu cheat sheet
category: système
tags: virtualisation macos
---

![lima](https://raw.githubusercontent.com/brahimmachkouri/theblog/main/assets/images/lima2.png)

Lima (“**Li**nux virtual **ma**chines”) est un outil open-source qui permet de lancer facilement des machines virtuelles Linux sur macOS (et aussi Linux). Il vise à offrir une expérience “Linux-on-Mac” fluide pour les développeurs, tout en gardant une gestion simple et intégrée.

L'objectif est d’exécuter des outils Linux (Docker, Podman, Kubernetes, etc.) sur Mac dans un vrai environnement Linux, ce qui offre une compatibilité et un comportement identiques à un vrai serveur Linux.

## Installation

```bash
brew install lima
```

## 📦 Lister les templates Ubuntu disponibles

Il existe de nombreux templates disponibles (docker, podman, kubernetes, ubuntu, debian, fedora, opensuse, alpine, etc), mais ici, c'est Ubuntu qui nous intéresse :

```bash
limactl start --list-templates | grep ubuntu
```

✅ Affiche les templates Ubuntu disponibles localement.

✅ Exemples de templates :
  - `ubuntu` : Dernière LTS (ex: 22.04)
  - `ubuntu-lts` : Alias vers la dernière LTS stable
  - `ubuntu-20.04` : Ubuntu 20.04 LTS (Focal Fossa)
  - `ubuntu-22.04` : Ubuntu 22.04 LTS (Jammy Jellyfish)
  - `ubuntu-24.04` : Ubuntu 24.04 LTS (Noble Numbat)

---

## 🚀 Créer et démarrer une VM Ubuntu avec une version précise

```bash
# Ubuntu LTS actuelle (ex: 22.04)
limactl start --name=ubuntu-lts template://ubuntu-lts

# Ubuntu 20.04 (Focal Fossa) avec 2 CPU et 4 Go de RAM
limactl start --name=ubuntu20 --cpus=2 --memory=4 --disk=5 template://ubuntu-20.04 

# Ubuntu 22.04 (Jammy Jellyfish) en mode Virtualization.framework avec Rosetta
limactl start --name=ubuntu22 --vm-type=vz --rosetta template://ubuntu-22.04

# Ubuntu 24.04 (Noble Numbat) : 4 CPU, 8 Go de RAM, Virtualization.framework, Rosetta, montage virtiofs en écriture
limactl start --name=ubuntu24 --cpus=4 --memory=8 --vm-type=vz --rosetta --mount-type=virtiofs --mount-writable template://ubuntu-24.04 

# Version non-LTS (ex: 24.10) ou future
limactl start --name=ubuntu-dev template://ubuntu-25.04
```

🛠️ **Paramètres courants :**
- `--cpus=X` : Nombre de CPU alloué à la VM.
- `--memory=Y` : Taille de la mémoire en Go (ex: `8` pour 8 Go).
- `--disk=Z` : Taille du disque en Go (par défaut 100Go).
- `--vm-type={qemu, vz}` : Type de machine virtuelle (`vz` pour macOS Ventura+ utilisant Virtualization.framework, plus performant).
- `--rosetta` : Active Rosetta pour l'exécution de binaires x86_64 sur Mac Apple Silicon (nécessite `--vm-type=vz`).
- `--mount-type={9p, virtiofs, reverse-sshfs}` : Type de montage (performant : `virtiofs` avec `vz`).
- `--mount-writable` : Donne accès en écriture au montage du répertoire de l'hôte.

🐚 **Alternative interactive :**
```bash
limactl start
```
➡️ Choisir le template dans l'interface interactive.

---

## 🖥️ Accéder à la VM

La commande **lima** est un alias pour "limactl shell default". Le nom de l'instance par défaut ("default") peut être changé via la variable $LIMA_INSTANCE.

Exemple : 
```bash
export LIMA_INSTANCE="myubu24"
```

Le shell et le répertoire initial (workdir) dans l'instance peuvent être spécifiés via $LIMA_SHELL et $LIMA_WORKDIR.

```bash
# Commande standard
lima

# Commande standard pour les autres VM 
limactl shell <nom_vm>

# Exemple :
limactl shell myubu24

# Accès SSH avancé (pour scripts, par exemple)
ip=$(limactl shell myubu24 ip -4 addr show lima0 | grep inet | awk '{print $2}' | cut -d/ -f1)
ssh -o IdentitiesOnly=yes -i ~/.lima/_config/user -p 22 $(whoami)@$ip
```

---

## 🛑 Gérer la VM

```bash
limactl list         # Lister toutes les VM
limactl stop         # Arrêter une VM
limactl start        # Démarrer une VM arrêtée
limactl restart      # Redémarrer une VM
limactl delete       # Supprimer une VM (demande confirmation)
```

⚠️ `limactl delete` supprime les disques et la configuration de la VM.

---

## 📂 Copier des fichiers entre hôte (Mac) et VM

```bash
limactl cp <source> <destination>
```

### Depuis le Mac vers la VM
```bash
limactl cp ./mon_fichier.txt ubuntu24:~/
```

### Depuis la VM vers le Mac
```bash
limactl cp ubuntu24:~/rapport.pdf ./
```

### Copier un répertoire récursivement
```bash
limactl cp -r  :
```

---

## ⚙️ Personnaliser la configuration de la VM

### Editer la configuration post-création

```bash
limactl edit 
```

Exemple (fragment) :

```yaml
cpus: 4
memory: "8GiB"
disk: "50GiB"
mounts:
  - location: "~"
    writable: true
    #virtiofs: true  # Utiliser pour vz si nécessaire
```

Après modification, arrêter et redémarrer la VM :

```bash
limactl stop 
limactl start 
```

### Création avancée avec fichier de configuration YAML
1. Créez un fichier `config.yaml` avec le contenu souhaité :

```yaml
images:
  - location: "https://cloud-images.ubuntu.com/releases/24.04/release/ubuntu-24.04-server-cloudimg-amd64.img"
    arch: "x86_64"
mounts:
  - location: "~"
    writable: true
cpus: 4
memory: "8GiB"
vmType: "vz"
rosetta: true
mountType: "virtiofs"
```

2. Lancez Lima en spécifiant le fichier de configuration :

```bash
limactl start --name=ubuntu24-advanced --file config.yaml
```

---

## 📝 Vérifier la version Ubuntu dans la VM

Une fois connecté à la VM :
```bash
lsb_release -a
```

Ou :
```bash
cat /etc/os-release
```

Commande depuis le Mac sans se connecter via un shell :
```bash
limactl shell ubuntu24 -- lsb_release -d
```

---

## ℹ️ Infos rapides sur les fichiers de la VM "myubu24"

Supposons qu'une VM avec le nom "myubu24" existe, les fichiers sont dans ~/.lima :

```brew
.lima/
├── _config
│   ├── networks.yaml         # Config réseau
│   ├── user                  # Clé privée
│   └── user.pub              # Clé publique
└── myubu24
    ├── basedisk <--- 578 Mo  # Image disque de base (cloudimg) utilisée
    ├── cidata.iso            # Image cloud-init (seed ISO)
    ├── cloud-config.yaml     # Configuration de la VM
    ├── default_ep.sock
    ├── default_fd.sock
    ├── diffdisk <----100 Go  # Overlay du disque (état de la VM)
    ├── ha.pid
    ├── ha.sock
    ├── ha.stderr.log
    ├── ha.stdout.log         # Logs du superviseur/agent VM
    ├── lima-version
    ├── lima.yaml             # Config YAML de la VM
    ├── serialv.log           # Log de la console série
    ├── ssh.config
    ├── ssh.sock
    ├── vz-efi
    ├── vz-identifier
    └── vz.pid
```

## ℹ️ Infos sur les images

- **Source des images** : Lima utilise les [cloud-images Ubuntu](https://cloud-images.ubuntu.com/) au format QCOW2 (KVM/QEMU).
- **Mises à jour** : La première fois, l'image est téléchargée puis mise en cache.
- **Non conteneur** : Lima fonctionne avec des machines virtuelles complètes (QEMU ou Virtualization.framework), pas des conteneurs LXC.

---

## 💡 Astuces Lima + Ubuntu

- **Hostname par défaut** : `lima-<nom_vm>`
- **Utilisateur par défaut** : `limactl shell ubuntu24 whoami` (avec accès sudo sans mot de passe)
- **Répertoire monté par défaut** : le répertoire home de l'utilisateur Mac est monté à la racine `/Users/<nom_user>`.
- **Accès SSH avancé** : Voir `~/.lima/<nom_vm>/ssh.config` pour la configuration SSH complète.

---

## 🔗 Liens utiles

- [Documentation officielle de Lima](https://lima-vm.io/)
- [Liste des images cloud Ubuntu](https://cloud-images.ubuntu.com/)
- [Github de Lima](https://github.com/lima-vm/lima)

---

> **Résumé workflow :**  
> 1. `limactl start --name=... template://...` (avec options)  
> 2. `limactl shell ...` pour se connecter  
> 3. Utiliser la VM comme un vrai système Ubuntu  
> 4. `limactl stop ...` pour arrêter  

> **Pro tips :**  
> - Pour les Mac Apple Silicon : toujours privilégier `--vm-type=vz` et `--rosetta`.  
> - `--mount-type=virtiofs` offre les meilleures performances de partage de fichiers.  
> - Configurer les montages en écriture avec `--mount-writable`.

