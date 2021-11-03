# TP2-linux

## TP2 : Manipulation de services

### Intro

#### Nommer la machine

On change le nom de la machine 

<img src="image tp2/1/nom machine.PNG" alt="jndjqs"/>

et de manière définitive dans /etc/hostname

<img src="image tp2/1/nom def machine.PNG">

#### Config réseau

ping 1.1.1.1

<img src="image tp2/1/ping 1.1.1.1.PNG">

ping ynov.com

<img src="image tp2/1/ping ynov.PNG">

ping IP_VM

<img src="image tp2/1/ping ordi.PNG">



ps -e
ss -lanpt
journal -u

nc -l -p 6668
nc 192.168.56.112

nc -l -p 6668 >> file.txt
nc 192.168.56.112 >> file.txt



