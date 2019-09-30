## Exercice 1. Gestion des utilisateurs et des groupes

### Commencez par créer deux groupes groupe1 et groupe2

<code>
  sudo addgroup groupe1
  sudo addgroup groupe2
</code></br>

### Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell

<code>
  sudo useradd -m u1 --shell /bin/bash
  sudo useradd -m u2 --shell /bin/bash
  sudo useradd -m u3 --shell /bin/bash
  sudo useradd -m u4 --shell /bin/bash
</code></br>

### Placez les utilisateurs dans les groupes :
- u1, u2, u4 dans groupe1</br>
- u2, u3, u4 dans groupe2</br>

<code>
  sudo usermod -g groupe1 u1
  sudo usermod -g groupe1 u2
  sudo usermod -g groupe1 u4
  sudo usermod -g groupe2 u3
  sudo usermod -a -G groupe2 u2
  sudo usermod -a -G groupe2 u4
</code></br>

### Donnez deux moyens d’afficher les membres de groupe2

Le premier moyen est de tapper la commande:</br>
<code>
cat /etc/group | grep groupe2
</code></br>
Le deuxième moyen est de tapper la commande:</br>
<code>sudo apt install members
members groupe2</code></br>

### Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
<code>
  sudo chown :groupe1 /home/u1
  sudo chown :groupe1 /home/u2
  sudo chown :groupe2 /home/u3
  sudo chown :groupe2 /home/u4
</code</br>

### Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2
- groupe2 pour u3 et u4
<code>
sudo usermod -g groupe1 /home/u1
sudo usermod -g groupe1 /home/u2
sudo usermod -g groupe2 /home/u3
sudo usermod -g groupe2 /home/u4
</code></br>

### Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
<code>
sudo mkdir /home/groupe1
chgrp groupe1 /home/groupe1
sudo mkdir /home/groupe2
chgrp groupe2 /home/groupe2
</code></br>

### Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?












