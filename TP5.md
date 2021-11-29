# TP5 P'tit cloud perso

[I. Setup DB](#i-setup-db)
  - [1. Install MariaDB](#1-install-mariadb)
  - [2. Conf MariaDB](#2-conf-mariadb)
  - [3. Test](#3-test)


## I. Setup DB

### 1. Install MariaDB

```sudo dnf install mariadb-server```

On lance le service maria

```sudo systemctl start mariadb```
```sudo systemctl enable mariadb```
```sudo systemctl status mariadb```

```
[toto@db ~]$ sudo systemctl status mariadb
[sudo] password for toto:
● mariadb.service - MariaDB 10.3 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2021-11-25 15:29:55 CET; 1min 23s ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
  Process: 1236 ExecStartPost=/usr/libexec/mysql-check-upgrade (code=exited, status=0/SUCCESS)
  Process: 910 ExecStartPre=/usr/libexec/mysql-prepare-db-dir mariadb.service (code=exited, status=0/SUCCESS)
  Process: 867 ExecStartPre=/usr/libexec/mysql-check-socket (code=exited, status=0/SUCCESS)
 Main PID: 948 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 30 (limit: 4943)
   Memory: 106.6M
   CGroup: /system.slice/mariadb.service
           └─948 /usr/libexec/mysqld --basedir=/usr

```

```
[toto@db ~]$ sudo ss -lanpt
State         Recv-Q        Send-Q               Local Address:Port               Peer Address:Port         Process
LISTEN        0             128                        0.0.0.0:22                      0.0.0.0:*             users:(("sshd",pid=865,fd=5))
ESTAB         0             36                       10.5.1.12:22                     10.5.1.1:61617         users:(("sshd",pid=1611,fd=5),("sshd",pid=1608,fd=5))
LISTEN        0             128                           [::]:22                         [::]:*             users:(("sshd",pid=865,fd=7))
LISTEN        0             80                               *:3306                          *:*             users:(("mysqld",pid=948,fd=21))

```

-C'est l'user toto qui lance le process

```
[toto@db ~]$ ps -ef | grep maria
toto        1658    1612  0 15:33 pts/0    00:00:00 grep --color=auto maria
```

-Configuration du firewall

```
[toto@db ~]$ sudo firewall-cmd --add-port=3306/tcp --permanent
success
[toto@db ~]$ sudo firewall-cmd --reload
success
```
## 2. Conf MariaDB

On execute la commande ```mysql_secure_installation```

Les question

```Enter current password for root (enter for none):```

Sert a rentrer un mdp root afin que personne d'autres ne puisse se connecter a MariaDB sans autorisation. Il est conseillé de répondre oui 

```Remove anonymous users? [Y/n]```

Par défaut, une installation MariaDB a un utilisateur anonyme, permettant à n'importe qui pour se connecter à MariaDB sans avoir à créer un compte utilisateur. Il est conseillé de répondre oui

```Disallow root login remotely? [Y/n]```

Il est préferable que root ne puisse se connecter qu'a partir du localhost. Il est donc conseillé de répondre oui

```Remove test database and access to it? [Y/n]```

Une base de donné par defaut auquel tout le monde peut avoir accès. Il est conseillé de la supprimé.

```Reload privilege tables now? [Y/n]```

Permet de faire prendre effet les modifications


On se connecte à la base de données pour la création d'un utilisateur

```sudo mysql -u root -p```

Installation sur la machine web.tp5.linux de la commande mysql

```sudo dnf install mysql```

On teste la connexion avec :

```sudo mysql -p -h 10.5.1.12 -P 3306 -u nextcloud nextcloud```

```
[toto@web ~]$ sudo mysql -p -h 10.5.1.12 -P 3306 -u nextcloud nextcloud
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 5.5.5-10.3.28-MariaDB MariaDB Server

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
```
mysql> SHOW TABLES;
Empty set (0.00 sec)
```

On installe Apache

```sudo dnf install httpd```
```sudo systemctl start httpd```
```sudo systemctl enable httpd```

Les processus

```
[toto@web ~]$ ps -ef | grep httpd
root        3352       1  0 10:04 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      3354    3352  0 10:04 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache      3355    3352  0 10:04 ?        00:00:03 /usr/sbin/httpd -DFOREGROUND
apache      3357    3352  0 10:04 ?        00:00:02 /usr/sbin/httpd -DFOREGROUND
apache      3591    3352  0 10:08 ?        00:00:02 /usr/sbin/httpd -DFOREGROUND
apache      3671    3352  0 10:10 ?        00:00:03 /usr/sbin/httpd -DFOREGROUND
toto        5774    5548  0 17:09 pts/0    00:00:00 grep --color=auto httpd
```

Le port d'écoute

```
[toto@web ~]$ sudo ss -lanpt | grep httpd
LISTEN 0      128                *:80              *:*     users:(("httpd",pid=3671,fd=4),("httpd",pid=3591,fd=4),("httpd",pid=3357,fd=4),("httpd",pid=3355,fd=4),("httpd",pid=3352,fd=4))
```

Les processus sont lancés sous l'utilisateur apache

Ouverture du firewall

```sudo firewall-cmd --add-port=80/tcp --permanent```
```sudo firewall-cmd --reload```

Test 

```
[toto@web ~]$ curl http://10.5.1.11
<!DOCTYPE html>
<html>
<head>
        <script> window.location.href="index.php"; </script>
        <meta http-equiv="refresh" content="0; URL=index.php">
</head>
</html>
```

On installe maintenant PHP en suivant les commandes

## II. Conf Apache


```
[toto@web conf.d]$ cat mon_nuage.conf
<VirtualHost *:80>
  # on précise ici le dossier qui contiendra le site : la racine Web
  DocumentRoot /var/www/nextcloud/html/  

  # ici le nom qui sera utilisé pour accéder à l'application
  ServerName  web.tp5.linux  

  <Directory /var/www/nextcloud/html/>
    Require all granted
    AllowOverride All
    Options FollowSymLinks MultiViews

    <IfModule mod_dav.c>
      Dav off
    </IfModule>
  </Directory>
</VirtualHost>
```

Création du dossier de la racine web

```sudo mkdir /var/www/nextcloud/html/```


Configuration de PHP

```
[Date]
; Defines the default timezone used by the date functions
; http://php.net/date.timezone
;date.timezone = "Europe/Paris"
```

## III. Install NextCloud

On récupère NextCloud

```curl -SLO https://download.nextcloud.com/server/releases/nextcloud-21.0.1.zip````

On l'extrait et on le deplace

```unzip nextcloud-21.0.1.zip```
```mv nextcloud /var/www/nextcloud/html/```
```rm nextcloud-21.0.1.zip```

Modification fichier hosts

```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost


10.5.1.11 web.tp5.linux
```

On test et ça fonctionne

<img src="image tp5/1.png">




