---
layout: post
date: 2021-11-01 12:50:00
title: Git Sheatcheet
category: developpement
tags: linux git 
---

How to check your Git configuration:

The command below returns a list of information about your git configuration including user name and email:

git config -l

How to setup your Git username:

With the command below you can configure your user name:

git config --global user.name "Fabio"

How to setup your Git user email:

This command lets you setup the user email address you'll use in your commits.

git config --global user.email "signups@fabiopacifici.com"

How to cache your login credentials in Git:

You can store login credentials in the cache so you don't have to type them in each time. Just use this command:

git config --global credential.helper cache

How to initialize a Git repo:

Everything starts from here. The first step is to initialize a new Git repo locally in your project root. You can do so with the command below:

git init

How to add a file to the staging area in Git:

The command below will add a file to the staging area. Just replace filename_here with the name of the file you want to add to the staging area.

git add filename_here
