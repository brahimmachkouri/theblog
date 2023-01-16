---
layout: post
date: 2023-01-16 10:01:15
title: utilisation de pyenv sous macOS
category: macos
tags: python dev
---  

Pour utiliser différentes versions de Python sous macOS, [**pyenv**](https://github.com/pyenv/pyenv) est LA solution.

Pour l'installer, il faut d'abord installer les Xcode Tools :  
```shell
xcode-select --install
```
Ainsi que quelques dépendances à l'aide de [**brew**](https://docs.brew.sh/Installation) :  
```shell
brew install openssl readline sqlite3 xz zlib
```

Ensuite, cloner le repository et lancer l'installation :  
```shell
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
cd ~/.pyenv/ && src/configure && make -C src
```

Il faut maintenant éditer le fichier .zshrc ou .bash_profile pour y ajouter ceci à la fin :
```bash
export PYENV_ROOT="$HOME/.pyenv" 
export PATH="$PYENV_ROOT/bin:$PATH" 
eval "$(pyenv init --path)" 
eval "$(pyenv init -)"
```
Pour que les modifications soient bien prises en compte, il faut s'assurer de bien fermer le terminal (pressez <kbd>⌘</kbd>+<kbd>Q</kbd>) et le relancer.

Désormais, pour installer des versions de Python, procédez comme ceci :  
```shell
pyenv install 3.11
pyenv install 2.7.18
```

Et pour le définir la version 3.11.1 comme version globale :  
```shell
pyenv global 3.11.1
```

Testez :
```bash
pyenv versions
python -V
```

Pour déclarer les versions 2 et 3 de Python en global :
```shell
pyenv global 3.11.1 2.7.18
```
Testez :  
```shell
python2
python3
```
