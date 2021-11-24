# TP4 : Une distribution orientée serveur

- [TP4 : Une distribution orientée serveur](#tp4--une-distribution-orientée-serveur)
- [II. Checklist](#ii-checklist)
- [III. Mettre en place un service](#iii-mettre-en-place-un-service)


## II. Checklist

**[`/etc/sysconfig/network-scripts/ifcfg-enp0s8`](lien)**

-Résultat ip a:

```
[toto@node ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:85:ac:9f brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 77692sec preferred_lft 77692sec
    inet6 fe80::a00:27ff:fe85:ac9f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1b:c3:a8 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.150/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1b:c3a8/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

-Service ssh actif

```
[toto@node ~]$ sudo systemctl status sshd
[sudo] password for toto:
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2021-11-24 10:27:35 CET; 2h 25min ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 866 (sshd)
    Tasks: 1 (limit: 4943)
   Memory: 4.7M
   CGroup: /system.slice/sshd.service
           └─866 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes256-cbc>

Nov 24 10:27:35 node.tp4.linux systemd[1]: Started OpenSSH server daemon.
Nov 24 10:29:01 node.tp4.linux sshd[1483]: Accepted password for toto from 192.168.56.1 port 53928 ssh2
Nov 24 10:29:01 node.tp4.linux sshd[1483]: pam_unix(sshd:session): session opened for user toto by (uid=0)
Nov 24 10:49:54 node.tp4.linux sshd[1577]: Accepted password for toto from 192.168.56.1 port 62115 ssh2
Nov 24 10:49:54 node.tp4.linux sshd[1577]: pam_unix(sshd:session): session opened for user toto by (uid=0)
Nov 24 10:49:54 node.tp4.linux sshd[1577]: pam_unix(sshd:session): session closed for user toto
Nov 24 10:50:18 node.tp4.linux sshd[1603]: Accepted publickey for toto from 192.168.56.1 port 62141 ssh2: RSA SHA256:7Q>
Nov 24 10:50:18 node.tp4.linux sshd[1603]: pam_unix(sshd:session): session opened for user toto by (uid=0)
Nov 24 10:50:45 node.tp4.linux sshd[1633]: Accepted publickey for toto from 192.168.56.1 port 62161 ssh2: RSA SHA256:7Q>
Nov 24 10:50:45 node.tp4.linux sshd[1633]: pam_unix(sshd:session): session opened for user toto by (uid=0)
lines 1-21/21 (END)
```

-La clé privé

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

```

-Le cadenas

```
[toto@node .ssh]$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC3HnGFIaGjw39Dr/tk5DSZIT47SqcrP4FPN52p/0EAxqEgKlwyi4rXdxpC8H7yQLK65ztdTFgaP2VtuH3MR6Tfdotm7w7qP8E3TPsJskveSadqftZSRGomjzHPLm62qwgldBF53+vZcz24cdQ2vQBxjYQPWzj0YmYXgNGjYYo8CgICCOzVwO6UdQwLvhoCGjMGdKx+lKlBtn0hr66YTTZi6jzr63KwSHIwedXx9e/fw5cI3UCixRii5WiGJMnmr96QMtSdxTFh+V1WItA1tbQN7zgYfRfo/hhHpob2HNO6OGpNZBy2E/S9Qo/ucH5d0qnPjiWMOspO4C62bdt7voR6DR/lpOcENKKv+O4PLkbDkyMxHntDt5J83OMsWzK4xWVjNEDBwNmUPXvrpPD7mng9GI+uuEK8Oye7yudmeAbfpsL5SkBWFbTifCb4aaFOTuPHpWuNUfR4rGWTSxMR2HXzJkS+KdJQ4fOWOAeLDCa+Cu+lMQ21lhGfSe77TRTBvnJk4vj6Y6Np+M9CzkTW5+Lp7QPTIy1spVzWc065deugjkIh+S3NT6l2Z7F92EMuqt0rZyFPZzbWFfaTPZcg6f6SKLZ2vbczHLmVJjgwht6TysiGuRdG2gqoSx7gAJPiam5WSoI2UOWyaaZsGnar+xU+P/BaKwHdkGNloLGUg31etQ== enric@DESKTOP-V3E4NIC
```

-Preuve de connexion sans mot de passe

```
C:\Users\enric>ssh  toto@192.168.56.150
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Wed Nov 24 10:50:45 2021 from 192.168.56.1
```

Connexion internet ping ip 

```
[toto@node ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=116 time=19.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=116 time=19.4 ms
```

-Ping domaine

```
[toto@node ~]$ ping ynov.com
PING ynov.com (92.243.16.143) 56(84) bytes of data.
64 bytes from xvm-16-143.dc0.ghst.net (92.243.16.143): icmp_seq=1 ttl=49 time=17.6 ms
64 bytes from xvm-16-143.dc0.ghst.net (92.243.16.143): icmp_seq=2 ttl=49 time=17.6 ms
```

-Nom de la VM

````
[toto@node ~]$ hostname
node.tp4.linux
````

-Fichier de conf 

```
[toto@node ~]$ cat /etc/hostname
node.tp4.linux
```


## III. Mettre en place un service

### Install

-On commence par installer le service nginx avec la commande

```
sudo dnf install nginx
```

### Analyse du service

-Le processus

```
[toto@node ~]$ ps -ef | grep nginx
root        4370       1  0 11:27 ?        00:00:00 nginx: master process /usr/sbin/nginx
nginx       4371    4370  0 11:27 ?        00:00:00 nginx: worker process
toto        4625    4565  0 13:08 pts/0    00:00:00 grep --color=auto nginx
```

```
[toto@node ~]$ sudo ss -lanpt | grep nginx
LISTEN 0      128           0.0.0.0:80        0.0.0.0:*     users:(("nginx",pid=4371,fd=8),("nginx",pid=4370,fd=8))
LISTEN 0      128              [::]:80           [::]:*     users:(("nginx",pid=4371,fd=9),("nginx",pid=4370,fd=9))
```

-Le dossier ou se trouve la racine web ```/usr/share/nginx/html/```

-Les fichier peuvent etre lu mais pas modifié par l'utilisateur

```
[toto@node html]$ ls -l
total 20
-rw-r--r--. 1 root root 3332 Jun 10 11:09 404.html
-rw-r--r--. 1 root root 3404 Jun 10 11:09 50x.html
-rw-r--r--. 1 root root 3429 Jun 10 11:09 index.html
-rw-r--r--. 1 root root  368 Jun 10 11:09 nginx-logo.png
-rw-r--r--. 1 root root 1800 Jun 10 11:09 poweredby.png
```

### Visite du service web

-On configure le firewall

```sudo firewall-cmd --add-port=80/tcp --permanent```  ouvre le port 80

```sudo firewall-cmd --reload``` permet de "mettre a jour les paramètres modifés"

-Le service fonctionne bien


```
[toto@node html]$ curl 192.168.56.150:80
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
  <head>
    <title>Test Page for the Nginx HTTP Server on Rocky Linux</title>
```


### Modif de la conf du serveur web

**Changer le port d'écoute**

-On va changer le port d'écoute donc on ferme le précedent avec ```sudo firewall-cmd --remove-port=80/tcp --permanent```

-On change le port d'écoute 
```
   server {
        listen       8080 default_server;
        listen       [::]:8080 default_server;
        server_name  _;
        root         /usr/share/nginx/html;
```
-On ouvre le port correspondant

```sudo firewall-cmd --add-port=8080/tcp --permanent```

-On reload le firewall et restart nginx

-Le service tourne toujours

```
[toto@node html]$ sudo systemctl status nginx.service
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2021-11-24 13:19:58 CET; 1min 40s ago
```
-Le service a bien changé de port

```
[toto@node html]$ sudo ss -lanpt | grep nginx
LISTEN 0      128           0.0.0.0:8080      0.0.0.0:*     users:(("nginx",pid=4680,fd=8),("nginx",pid=4679,fd=8))
LISTEN 0      128              [::]:8080         [::]:*     users:(("nginx",pid=4680,fd=9),("nginx",pid=4679,fd=9))
```

-On peut le visiter

```
[toto@node html]$ curl http://192.168.56.150:8080
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
  <head>
    <title>Test Page for the Nginx HTTP Server on Rocky Linux</title>
```

**Changer l'utilisateur qui lance le service**

-On crée un nouvel user, "web"

```useradd toto -m -s /bin/sh -u 2000```

-On lui donne un password, on se mets en root pour ça

```passwd web```

-On modifie le fichier de conf nginx

```
[toto@node etc]$ cat /etc/nginx/nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user web;
```
```
[toto@node etc]$ ps -aux | grep nginx
root        4886  0.0  0.2 119160  2184 ?        Ss   14:19   0:00 nginx: master process /usr/sbin/nginx
web         4887  0.0  0.9 151820  7884 ?        S    14:19   0:00 nginx: worker process
```

**Changer l'emplacement de la racine Web**

-Le nouveau dossier 

```
[toto@node super_site_web]$ pwd
/var/www/super_site_web
```

-Contenu index.html

```
[toto@node super_site_web]$ cat index.html
<!doctype html>
<html>
    <head>
        <title>mon super site</title>
        <meta charset="UTF8">
    </head>
<header>
        <h1>Super site<h1>
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
```
-On modifie la racine dans le dossier de conf

```
server {
        listen       8080 default_server;
        listen       [::]:8080 default_server;
        server_name  _;
        root         /var/www/super_site_web;
```

-Tout fonctionne correctement

```
[toto@node www]$ curl http://192.168.56.150:8080
<!doctype html>
<html>
    <head>
        <title>mon super site</title>
        <meta charset="UTF8">
    </head>
<header>
        <h1>Super site<h1>
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
```



