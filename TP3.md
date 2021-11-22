# TP3 : A little script

## Script carte d'identité

**Fichier [`/srv/idcard/idcard.sh`](https://github.com/Enrick1234/TP-linux/blob/main/fichier%20tp3/idcard.md)**
 
``` 
Machine name: enrick-aspirees1531
IP: 192.168.1.19/24
OS Ubuntu and kernel version is 5.8.0-63-generic
Ram :766572  RAM restante sur 7,6Gi  RAM totale
Disque : 859G space left
Top 5 processes by RAM usage :
  16079   16077 java -Xmx2048M -jar server. 28.5 26.0
   1072     991 /usr/bin/lxqt-panel          1.0  0.1
    858     831 /usr/lib/xorg/Xorg -noliste  0.7  4.2
   1069     991 /usr/bin/pcmanfm-qt --deskt  0.7  0.0
   1074     991 /usr/bin/lxqt-runner         0.7  0.0
Listening ports :
LISTEN 0      4096   127.0.0.53%lo:53          0.0.0.0:*     users:(("systemd-resolve",pid=616,fd=13))
LISTEN 0      128          0.0.0.0:22          0.0.0.0:*     users:(("sshd",pid=840,fd=3))
LISTEN 0      5          127.0.0.1:631         0.0.0.0:*     users:(("cupsd",pid=26582,fd=7))
LISTEN 0      128             [::]:22             [::]:*     users:(("sshd",pid=840,fd=4))
LISTEN 0      5              [::1]:631            [::]:*     users:(("cupsd",pid=26582,fd=6))
LISTEN 0      4096               *:25565             *:*     users:(("java",pid=16079,fd=26))
parse error: Invalid numeric literal at line 1, column 6

Here's your random cat : 
```

## Script youtube-dl

**Fichier [`/srv/yt/yt.sh`](https://github.com/Enrick1234/TP-linux/blob/main/fichier%20tp3/script.md)**
**Log [`/var/log/yt/download.log`](https://github.com/Enrick1234/TP-linux/blob/main/fichier%20tp3/log.md)**

```
enrick@enrick-aspirees1531:/srv/yt$ bash yt.sh https://www.youtube.com/watch?v=Wch3gJG2GJ4
Video https://www.youtube.com/watch?v=Wch3gJG2GJ4 was downloaded
File path : /srv/yt/downloads/1 Second Video/1 Second Video.mp4
```

## MAKE IT A SERVICE

**Fichier script [`/srv/yt/yt-v2.sh`](https://github.com/Enrick1234/TP-linux/blob/main/fichier%20tp3/service.md)**
**Fichier service [`/etc/systemd/system/yt.service`](https://github.com/Enrick1234/TP-linux/blob/main/fichier%20tp3/systemservice)**

Malheuresement le service ne se lance pas tout fonctionne correctement lorsque je le lance à la main mais pas avec le service

Je mets l'erreur juste ci dessous

```
Warning: The unit file, source configuration file or drop-ins of yt1.service changed on disk. Run 'systemctl daemon-reload' to reload units.
● yt1.service - dl video yt
     Loaded: loaded (/etc/systemd/system/yt1.service; disabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Mon 2021-11-22 16:05:31 CET; 1h 45min ago
    Process: 32299 ExecStart=/usr/bin/sudo bash /usr/local/bin/yt1.service (code=exited, status=127)
   Main PID: 32299 (code=exited, status=127)

nov. 22 16:05:31 enrick-aspirees1531 systemd[1]: Started dl video yt.
nov. 22 16:05:31 enrick-aspirees1531 sudo[32299]:     root : TTY=unknown ; PWD=/ ; USER=root ; COMMAND=/usr/bin/bash /usr/local/bin/yt1.service
nov. 22 16:05:31 enrick-aspirees1531 sudo[32299]: pam_unix(sudo:session): session opened for user root by (uid=0)
nov. 22 16:05:31 enrick-aspirees1531 sudo[32300]: bash: /usr/local/bin/yt1.service: No such file or directory
nov. 22 16:05:31 enrick-aspirees1531 sudo[32299]: pam_unix(sudo:session): session closed for user root
nov. 22 16:05:31 enrick-aspirees1531 systemd[1]: yt1.service: Main process exited, code=exited, status=127/n/a
nov. 22 16:05:31 enrick-aspirees1531 systemd[1]: yt1.service: Failed with result 'exit-code'
```


