# TP1-linux

## TP 1 : Are you dead yet ?

Pour rappel le but du TP était de trouver le moyen de détruire une machine sous linux/GNU et de 5 manière les plus différentes possible. Les outils à notre dispositions sont GOOOOOGLE, notre imagination et plein de clones... beaucoup de clones.

### Les 5 manières en questions

- ```sudo rm -r --no-preserve-root /*``` Cette commande permet de supprimer **tout** depuis la racine de maniere récursive. La machine freeze et ne répond plus.

- ```sudo nano /etc/shadow``` Ouvre le fichier ou est enregistré le mot de passe. Modifier une lettre, au niveau de l'user changera le mot de passe et il sera alors impossible de se connecter.

- ```sudo chmod -R 000 /``` Cette commande retire absolument tout les droit à tout le monde. Sans droit la machine est totalement inutile.

- Ceci n'est pas une commande. Mais reste efficace, cela consiste a télécharger une images, lourde de préférence et de la copier plusieurs milliers de fois. La machine ralentira et au redemmarage ne sera plus du tout fonctionelle (saturation de la mémoire)

- ```sudo mv / /*``` Renomme le fichier racine et cause des problèmes dans l'adresse des fichiers 

### Bonus   

- ```sudo dd if=/dev/zero of=/dev/sda``` Cette commande permet de remplacer tout les bits du disque dur par des 0, cela a pour effet de formater petit a petit le disque dur. Il est possible de stopper le processus avec ```Control + c```. Neanmoins si une partie importante est "touché" la machine devient completement inutilisable.
