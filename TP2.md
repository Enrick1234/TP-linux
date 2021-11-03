# TP2 : Manipulation de services



## Intro


### Nommer la machine


On change le nom de la machine 

<img src="image tp2/1/nom machine.PNG" alt="jndjqs"/>

et de manière définitive dans /etc/hostname

<img src="image tp2/1/nom def machine.PNG">

### Config réseau


ping 1.1.1.1

<img src="image tp2/1/ping 1.1.1.1.PNG">

ping ynov.com

<img src="image tp2/1/ping ynov.PNG">

ping IP_VM

<img src="image tp2/1/ping ordi.PNG">


## PARTIE 1


### Installation du serveur


Avec une commande ```sudo apt install openssh-server``` on installe le paquet openssh-server


### Lancement du service SSH


On lance le serveur avec ```sudo systemctl start sshd```
et on verifie que le service est actif avec ```sudo systemctl status sshd```

<img src="image tp2/1/etat server.PNG">


### Etude du service SSH


On a déja vu le statut du service juste ci-dessus

On affiche les processus liés au service ssh avec la commande ```ps -e```

<img src="image tp2/1/ps ssh status.PNG">

On affiche le port utilisé par le service ssh avec la commande ```sudo ss -lanpt```

<img src="image tp2/1/ss ssh port.PNG">

On affiche les logs du service ssh avec la commande ```journalctl -u ssh```

<img src="image tp2/1/ssh lastlog.PNG">

On se connecte a notre serveur depuis notre PC avec ```ssh user@IP```

<img src="image tp2/1/connexion pc externe.PNG">


### Modification de la configuration du server


On peut modifier le service dans le fichier /etc/ssh/sshd_config. </p>
On peut par exemple modifié le port d'écoute qui est sur 22 de base et le mettre par exemple sur 6666

<img src="image tp2/1/cat port modifié.PNG">

<img src="image tp2/1/ss port modifié.PNG">

On se connecte sur le nouveau port avec ```ssh user@IP -p XXXX``` XXXX pour le numéro de port dans mon cas 6666



## Partie 2



### Installation du serveur et lancement du service 


On installe le paquet vsftpd avec ```sudo apt install vsftpd``` puis on le lance ```sudo systemctl start vsftpd```


### Etude du service FTP


On affiche le statut du service 

<img src="image tp2/2/system status.PNG">

On affiche les processus liés au service ssh avec la commande ```ps -e```

<img src="image tp2/2/ps vstpd.PNG">

On affiche le port utilisé par le service ssh avec la commande ```sudo ss -lanpt```

<img src="image tp2/2/ss -l vstpd.PNG">

On affiche les logs du service ssh avec la commande ```journalctl -u vsftpd```

<img src="image tp2/2/journal.PNG">

On se connecte a notre serveur... A défaut d'avoir réussi par un navigateur web. Je suis passé directement par l'explorateur de fichier windows avec ```ftp://IP``` </p>
On peut alors telecharger et uploader un fichier facilement comme un simple transfert de fichier au sein d'un meme PC.

<img src="image tp2/2/ftp.PNG">

Preuve upload avec ls

<img src="image tp2/2/preuve ls upload.PNG">

ligne de log pour le download et l'upload

<img src="image tp2/2/download upload log.PNG">


### Modification de la configuration serveur

On peut encore changer le port d'écoute que l'on va mettre sur 6667 cette fois-ci. On a du rajouter la ligne(listen_port) car absente de base

<img src="image tp2/2/port changed.PNG">

<img src="image tp2/2/preuve port changed.PNG">

On peut se reconnecter avec ```ftp://IP:XXXX``` avec XXXX le numéro du port dans mon cas 6667







ps -e
ss -lanpt
journalctl -u

nc -l -p 6668
nc 192.168.56.112

nc -l -p 6668 >> file.txt
nc 192.168.56.112 >> file.txt



