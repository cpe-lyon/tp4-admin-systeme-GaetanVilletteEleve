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
<code>
sudo gpasswd -d u3 groupe2
</code></br>
U3 n'est pas utilisateur de groupe2 on ne peut donc pas le retirer car groupe2 est l'unique groupe de u3 et ne peux pas ne pas faire partie d'un groupe.3

### Modifiez le compte de u4 de sorte que :
— il expire au 1er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe

<code>
 sudo usermod --expiredate 2020-06-01 u4
 sudo chage -m 90 u4
 sudo chage -W 14 u4
 sudo chage -I 30 u4
</code></br>

### Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
<code>
 cat /etc/passwd | grep "root" | cut -d: -f7
</code></br>

### à quoi correspond l’utilisateur nobody ?
L'utilisateur nobody  correspond au nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont. </br>

### Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?
La commande sudo conserve le mot de passe en mémoire pendant 15 minutes. La commande qui permet de forcer sudo à oublier son mot de passe est :</br>
<code>sudo -k</code></br>.

## Exercice 2. Gestion des permissions

### Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier cd ; sudo mkdir test ; cd test ; nano fichier

Les droits du dossier test sont à 755 ou wrx wrx wrx et les droits du fichier fichier sont à 664.</br>

### Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
On ne peut pas acceder, modifier ou lire le fichier en tant qu'utilisateur, par contre il est possible de le faire en tant que root.
Ainsi root à tout les droits sur tout les fichiers meme si ceux-ci sont modifiés.</br>

### Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
On voit que l'on peut écrire sur le fichier ainsi il possède au minimum les droits 400.</br>

### Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.
On ne peut pas exécuter le fichier car on a pas les droits d'éxécution en tant qu'utilisateur dessus.</br>
Cependant avec sudo il sera possible de l'éxécuter.</br>

### Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test
<code>sudo chmod u-r ../test
ls 
cat fichier</code><br>
Comme on ne peut pas lister le contenu du repertoire ni voir le contenu du fichier on en deduit que les droits d'un repertoire sont supérieurs à ceux des fichiers internes.</br>

### Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?
<code>touch nouveau
mkdir sstest
chmod u-w nouveau
chmod u-w ../test
nano nouveau</code></br>
Il y a une erreur disant que nous ne possedons pas les permissions de modification.</br>
<code>chmod u+w ../test
nano nouveau</code>
Il y a encore la meme erreur disant que nous ne possedons pas les permissions de modification. </br>
<code>rm nouveau
rm: remove write-protected regular file 'nouveau'? yes
</code></br>
Ainsi on peut en deduire qu'il n'y pas de transmission des droits pour les fichiers internes aux répertoires.</br>

### Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
<code>chmod u-x test
 nano test/nouveau</code></br>
Le repertoire test n'est pas accessible.</br>
<code> cd test/</code></br>
Impossible d'acceder au fichier ou au dossier</br>
fichier  sstest
Quand nous n'avaons pas les droits en execution pour les répertoires nous ne pouvons pas accéder aux fichiers internes à ce répertoire.</br>

### Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
<code>chmod u+x ../test
cd test
chmod u-x ../test
touch new</code></br>
Nous n'avons pas les droits de création de fichier.</br>
<code>rm fichier</code></br>
Nous n'avons pas les droits de suppression de fichier.</br>
<code>nano fichier</code></br>
Nous ne pouvons pas sauvegarder les changements du fichier fichier</br>

<code>cd sstest</code></br>
Il est impossible d'accéder à sstest</br>

<code>ls sstest</code></br>
Nous n'avons pas les permissions de lectures du répertoire.</br>
On en conclu qu'enlever les droit à un dossier applique le changement aux fichiers internes.</br>
Cependant on peut toujours retourner dans le répertoire courant car les droits sur celui-ci nous le permette.</br>

### Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
<code> chmod 640 fichier </code></br>

### Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
<code>umask 077 test
mkdir ndir
commande umask 077 dir</code></br>
On ne peut acceder ni au nouveau fichier ni au nouveau répertoire.</br>

### Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
<code> umask 011 test
umask 011 dir </code></br>

### Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
<code> umask 066 test
umask 066 dir </code></br>

### Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses)
<code>chmod u=rx,g=wx,o=r fic = chmod 534 = chmod -r-x--wx-r--
chmod uo+w,g-rx fic = chmod 706 = chmod -rwx-x-rw- </code></br>

### Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?




