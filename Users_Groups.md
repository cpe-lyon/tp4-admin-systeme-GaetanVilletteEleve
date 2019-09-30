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
<code>
chmod 1755 /home/groupe1
</code></br>

### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
<code>
  su u1
</code></br>
Non il est impossible de se connecter en tant que u1 car il n'y a pas de mot de passe pour cet utilisateur.</br>

### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
<code>
  sudo passwd u1
  new password: *espace*
  su u1
</code></br>

### Quels sont l’uid et le gid de u1 ?
<code>
id u1
  uid = 1001
  gid = 1001
</code></br>

### Quel utilisateur a pour uid 1003 ?
<code>
getent passwd "1003" | cut -d: -f1
</code></br>
uid 1003 = u3</br>

### Quel est l’id du groupe groupe1 ?
<code>
getent group groupe1 | cut -d: -f1
</code></br>
guid du groupe1 = 1001</br>

### Quel groupe a pour guid 1002 ?
<code>
getent group "1002" | cut -d: -f1
</code></br>
guid 1002 = groupe2

### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.






























