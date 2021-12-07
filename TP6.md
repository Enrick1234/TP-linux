# TP6 Stockage et sauvegarde

## 1. Ajout de disque

On ajoute un disque dur de 5go 

```
[toto@backup ~]$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0    8G  0 disk
├─sda1        8:1    0    1G  0 part /boot
└─sda2        8:2    0    7G  0 part
  ├─rl-root 253:0    0  6.2G  0 lvm  /
  └─rl-swap 253:1    0  820M  0 lvm  [SWAP]
sdb           8:16   0    5G  0 disk
sr0          11:0    1 1024M  0 rom
```

```
sdb           8:16   0    5G  0 disk
```

## 2. Partitioning

On crée un PV du nouveau disque

```
[toto@backup ~]$ sudo pvcreate /dev/sdb
[sudo] password for toto:
  Physical volume "/dev/sdb" successfully created.
```

Création du VG 

```
[toto@backup ~]$ sudo vgcreate backup /dev/sdb
  Volume group "backup" successfully created
```

Création d'un LV 

```
[toto@backup ~]$ sudo lvcreate -l 100%FREE backup -n lvbackup
  Logical volume "lvbackup" created.
```

Formater la partition

```
[toto@backup ~]$ mkfs -t ext4 /dev/backup/lvbackup
mke2fs 1.45.6 (20-Mar-2020)
Could not open /dev/backup/lvbackup: Permission denied
[toto@backup ~]$ sudo !!
sudo mkfs -t ext4 /dev/backup/lvbackup
[sudo] password for toto:
mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 1309696 4k blocks and 327680 inodes
Filesystem UUID: ac90419e-dd0a-4620-9b55-01dcf6050fc1
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736
```

Monter la partition 

```
[toto@backup mnt]$ sudo !!
sudo mkdir /backup
[toto@backup ~]$ sudo mount /dev/backup/lvbackup /backup
```

```
[toto@backup ~]$ df -h
Filesystem                   Size  Used Avail Use% Mounted on
devtmpfs                     387M     0  387M   0% /dev
tmpfs                        405M     0  405M   0% /dev/shm
tmpfs                        405M  5.6M  400M   2% /run
tmpfs                        405M     0  405M   0% /sys/fs/cgroup
/dev/mapper/rl-root          6.2G  2.1G  4.2G  34% /
/dev/sda1                   1014M  266M  749M  27% /boot
tmpfs                         81M     0   81M   0% /run/user/1000
/dev/mapper/backup-lvbackup  4.9G   20M  4.6G   1% /backup
```

```
[toto@backup /]$ ls -l
total 24
drwxr-xr-x.   3 toto toto 4096 Nov 30 11:42 backup
```

Partie 2 : Setup du serveur NFS sur backup.tp6.linux

Préparer les dossiers à partager

```
[toto@backup backup]$ mkdir db.tp6.linux web.tp6.linux
[toto@backup backup]$ ls
db.tp6.linux  lost+found  web.tp6.linux
```

Conf du serveur NFS

```
[toto@backup backup]$ cat /etc/idmapd.conf | grep Domainc/idmapd.conf
Domain = tp6.linux
```

Démarrez le service

```
[toto@backup backup]$ sudo systemctl start nfs-server.service
[toto@backup backup]$ sudo systemctl status nfs-server.service
● nfs-server.service - NFS server and services
   Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; disabled; vendor preset: disabled)
   Active: active (exited) since Tue 2021-11-30 12:41:51 CET; 2s ago
  Process: 2993 ExecStart=/bin/sh -c if systemctl -q is-active gssproxy; then systemctl reload gssproxy ; fi (code=e>
  Process: 2981 ExecStart=/usr/sbin/rpc.nfsd (code=exited, status=0/SUCCESS)
  Process: 2980 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
 Main PID: 2993 (code=exited, status=0/SUCCESS)

Nov 30 12:41:50 backup.tp6.linux systemd[1]: Starting NFS server and services...
Nov 30 12:41:51 backup.tp6.linux systemd[1]: Started NFS server and services.
[toto@backup backup]$ sudo systemctl enable nfs-server.service
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-server.service → /usr/lib/systemd/system/nfs-server.service.
```

Firewall

```
[toto@backup backup]$ sudo firewall-cmd --add-port=2049/tcp --permanent
success
[toto@backup backup]$ sudo firewall-cmd --reload
success
```

Port 

```
[toto@backup ~]$ sudo ss -lanpt | grep 2049
LISTEN 0      64           0.0.0.0:2049       0.0.0.0:*
ESTAB  0      0          10.5.1.13:2049     10.5.1.12:692
ESTAB  0      0          10.5.1.13:2049     10.5.1.11:741
LISTEN 0      64              [::]:2049          [::]:*
```

##  3. Setup des clients NFS : web.tp6.linux et db.tp6.linux

Sur les 2 machines

```dnf install nfs-utils```
```mkdir /srv/backup```

Conf /etc/idmapd.conf

```
[toto@db backup]$ cat /etc/idmapd.conf | grep Domain
Domain = tp6.linux
```

```sudo mount -t nfs 10.5.1.13:/backup/(db) and (web).tp6.linux /srv/backup```