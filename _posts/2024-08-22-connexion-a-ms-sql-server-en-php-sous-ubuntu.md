---
layout: post
date: 2024-08-22 11:40:00
title: Connexion à MS SQL Server en PHP sous Debian/Ubuntu
category: dev
tags: mssql php ubuntu
---

Remarque : la procédure suivante a été réalisée pour une version 7.2 de PHP. Il se peut que vous deviez adapter quelques points...

Pour la suite, on suppose que le serveur Microsoft SQL Server écoute les requêtes sur le port 1433 (par défaut) et qu’un utilisateur admindb est configuré pour l’accès à la base tournois qui ne contient ici qu’une seule table :

```
+------------+
|   users    |
+------------+
|    id      |
|    nom     |
+------------+
```
Paramètres :

```
IP SQL Server : 10.0.0.15
Database : tournois
Username : admindb
Password : HercuLE
```
Pour installer le client SQL Server sous Debian ou Ubuntu :

```
apt install freetds-common freetds-bin unixodbc tdsodbc php7.2-odbc
```
Redémarrer le démon PHP, puis remplir les fichiers de configuration :

Dans /etc/freetds/freetds.conf, ajouter :

```
[monserveur]
host = 10.0.0.15
port = 1433
tds version = 8.0
```
Dans /etc/odbcinst.ini, ajouter :

```
[FreeTDS]
Description = FreeTDS Driver
Driver = /usr/lib/x86_64-linux-gnu/odbc/libtdsodbc.so
Setup = /usr/lib/x86_64-linux-gnu/odbc/libtdsS.so
Dontdlclose = 1
```
Dans /etc/odbc.ini, ajouter :

```
[MSSQLSRV]
Driver = FreeTDS
Description = ODBC connection via FreeTDS
#Server = 10.0.0.15
Servername = monserveur
Database = tournois
Port = 1433
```
Tester la connexion au serveur SQL Server à l’aide des outils tsql & isql :

```
tsql -H 10.0.0.15 -p 1433 -U admindb -P HercuLE -D tournois
```
Si tout va bien, voici le résultat :

```
locale is "en_US.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
Setting tournois as default database in login packet
1>
```
Taper quit pour sortir.

Ensuite :

```
isql -v MSSQLSRV admindb HercuLE
```
donnera :

```
+---------------------------------------+
| Connected!                            |
|                                       |
| sql-statement                         |
| help [tablename]                      |
| quit                                  |
|                                       |
+---------------------------------------+
SQL>
```
Voici le code pour faire une requête en PHP :

```php
<?php
 
define('USER','admindb');
define('PASSWORD','HercuLE');
 
$conn   = odbc_connect("MSSQLSRV", USER, PASSWORD);
$query  = "select * from CT_USER";
$result = odbc_exec( $conn, $query );
 
while (odbc_fetch_row($result)) {
  echo odbc_result($result, "name"), "\n";
}
```
