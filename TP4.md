******
#TP 4 - Utilisateurs, groupes et permissions
******
##Exercice 1. Gestion des utilisateurs et des groupes
1. Commencez par créer deux groupes groupe1 et groupe2
la commande pour créer groupe est `sudo groupadd groupenom`. Cas notre cas,  le codage est au-dessous.
```linux
sudo groupadd groupe1
sudo groupadd groupe2
```
Et après la création de chaque groupe, on récupère le résultat de list de groupe en utilisant la commande suivant:
```linux
cat /etc/group
```
2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
Afin de créer des utilisateurs et leur dossier personnel, il faut utiliser la commande `useradd` avec l'option `-m`.
```linux
useradd -m u1
useradd -m u2
useradd -m u3
useradd -m u4
```
pour vérifier toutes les créations d’utilisateur, on va à le fichier /etc/passwd. On conserve des lignes suivant:
```linux
u1:x:1001:1003::/home/u1:/bin/sh
u2:x:1002:1004::/home/u2:/bin/sh
u3:x:1003:1005::/home/u3:/bin/sh
u4:x:1004:1006::/home/u4:/bin/sh
```

3. Ajoutez les utilisateurs dans les groupes créés :
- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2
on utilise la commande `usermod` avec l’option -a -G pour  ajouter des utilisateurs à un groupe 
```linux
sudo usermod -a -G groupe1 u1
sudo usermod -a -G groupe1 u2
sudo usermod -a -G groupe1 u4
sudo usermod -a -G groupe2 u2
sudo usermod -a -G groupe2 u3
sudo usermod -a -G groupe2 u4
```

 avec la commande  ```cat /etc/group ```on affiche les utilisateurs des groupe 1 et 2 d’ou on a le resultat ci dessus :

```linux 
groupe1:x:1001:u1,u2,u4
groupe2:x:1002:u2,u3,u4
```


4. Donnez deux moyens d’afficher les membres de groupe2
```linux
sudo cat /etc/gshadow
groupe1:!::u1,u2,u4
groupe2:!::u2,u3,u4
```

```linux 
cat /etc/group 
groupe1:x:1001:u1,u2,u4
groupe2:x:1002:u2,u3,u4
```


5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
```linux
sudo chgrp groupe1 /home/u2
sudo chgrp groupe1 /home/u1
sudo chgrp groupe2 /home/u3
sudo chgrp groupe2 /home/u4
```

```resultats
ls -l ..
total 20
drwxr-xr-x 2 u1   groupe1 4096 mars  13 14:22 u1
drwxr-xr-x 2 u2   groupe1 4096 mars  13 14:22 u2
drwxr-xr-x 2 u3   groupe2 4096 mars  13 14:22 u3
drwxr-xr-x 2 u4   groupe2 4096 mars  13 14:23 u4
drwxr-xr-x 4 user user    4096 févr.  6 10:25 user
```


6. Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2
- groupe2 pour u3 et u4
pour faire ce la nous avons utiliser usermod  pour modifier le groupe primaire  des utilisateurs u1, u2, u3 et u4. pour verifier cela nous avons affiché le repètoire /ect/gshadow.

```linux
sudo usermod -g groupe1 u1
sudo usermod -g groupe1 u2
sudo usermod -g groupe2 u3
sudo usermod -g groupe2 u4
sudo cat /etc/gshadow
```
```resultats
groupe1:!::u1,u2,u4
groupe2:!::u2,u3,u4
u1:!::
u2:!::
u3:!::
u4:!::



7. Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
création des répertoires: la commande mkdir avec sudo.
```
sudo mkdir /home/groupe1
sudo mkdir /home/groupe2
```

sudo chgrp groupe1 groupe1  ##
sudo chmod g+rwx groupe1/ ##
sudo chmod o-rwx groupe1/ ##
sudo chgrp groupe2 groupe2 ##
sudo chmod g+rwx groupe2/ ##
sudo chmod o-rwx groupe2/ ##

pour vérifier le résultat, la commande `ls -l` montre le résultat suivant, on peut voir la 6e colonne de la table les propriétaires de groupe de chaque dossier.
```
root@server:/home# ls -list
total 28
393653 4 drwxrwx--- 2 root groupe2 4096 mars  13 15:38 groupe2
278756 4 drwxrwx--- 2 root groupe1 4096 mars  13 15:38 groupe1
278752 4 drwxr-xr-x 2 u4   groupe2 4096 mars  13 14:33 u4
278747 4 drwxr-xr-x 2 u3   groupe2 4096 mars  13 14:33 u3
262390 4 drwxr-xr-x 2 u2   groupe1 4096 mars  13 14:33 u2
262386 4 drwxr-xr-x 2 u1   groupe1 4096 mars  13 14:17 u1
262299 4 drwxr-xr-x 5 user user    4096 mars  13 10:07 user
```


8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
```
sudo chmod -R 755  /home/groupe1
sudo chmod -R 755  /home/groupe2
```
si on veut supprimer le droit de renommer ou supprimer un fichier de ces dossiers, on peut justement modifier la permission. r=4，w=2，x=1, `755`signifique -rwxr-xr-x.


9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
Non car l'utilisateur u1 n'est pas activé, n'ayant pas de mot de passe. La commande su u1 censée permettre la connection ne fonctionne alors pas.
```linux
su u1
Password: 
su: Authentication failure
```


10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.

```sudo  passwd u1
New password: 
Retype new password: 
passwd: password updated successfully
```
``` su u1
Password: titi
$ 
```




11. Quels sont l’uid et le gid de u1 ?
```linux
  id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
````


12. Quel utilisateur a pour uid 1003 ?
``linux  
cat /etc/passwd
u3:x:1003:1002::/home/u3:/bin/sh 
``` 
c’est l’utilisateur u3 qui a l’id 1003


13. Quel est l’id du groupe groupe1 ?
``linux  
cat  /etc/group
groupe1:x:1001:u1,u2,u4
```



14. Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...)
``linux  
cat  /etc/group
groupe2:x:1002:u2,u3,u4
```


15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
```linux
cat  /etc/group
groupe1:x:1001:u1,u2,u4
groupe2:x:1002:u2,u3,u4

$ sudo gpasswd -d u3 groupe2
[sudo] password for user: 
Removing user u3 from group groupe2

cat  /etc/group
groupe1:x:1001:u1,u2,u4
groupe2:x:1002:u2,u4

```



16. Modifiez le compte de u4 de sorte que :
— il expire au 1 er juin 2020
```linux 
~$ sudo chage -E 2020/06/01 u4
[sudo] password for user: 
```
— il faut changer de mot de passe avant 90 jours
```linux
sudo passwd -x 90 u4
passwd: password expiry information changed.
```

— il faut attendre 5 jours pour modifier un mot de passe
```linux
sudo passwd -n 5 u4
```
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
```linux 
sudo passwd  -w 14 u4
```
— le compte sera bloqué 30 jours après expiration du mot de passe
```linux
sudo passwd -i 30 u4
```
```linux
sudo chage -l u4
Last password change					: mars 13, 2020
Password expires					: juin 11, 2020
Password inactive					: juil. 11, 2020
Account expires						: juin 01, 2020
Minimum number of days between password change		: 5
Maximum number of days between password change		: 90
Number of days of warning before password expires	: 14
```



17. Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
```linux
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
```
Chaque utilisateur possède un shell par défaut, qui sera lancé à l'ouverture
d'une invite de commande. Le shell par défaut est précisé dans le fichier de configuration /etc/passwd dans le dernier champ de la ligne correspondant à l'utilisateur.


18. à quoi correspond l’utilisateur nobody ?
 l’utilisateur nobody est  un utilisateur qui  n’a aucun fichier , qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les autres utilisateurs ont.
Il est courant de lancer des démons en tant que nobody, spécialement pour des serveurs, de façon à limiter les dommages qui pourraient être occasionnés par un utilisateur malicieux qui aurait réussi à prendre leur contrôle. 


19. Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
pendant 	15 minutes 
Quelle commande permet de forcer sudo à oublier votre mot de passe ?
sodu -k  d’apres le manuel.
******

Exercice 2. Gestion des permissions

1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?
```linux 
user@server:/home$ sudo mkdir test 
user@server:/home/test$ sudo touch fichier.txt
user@server:~$ sudo echo "coucou la famille" > fichier.txt
user@server:~$ ls -R -l
-rw-rw-r-- 1 user user   18 mars  17 18:17 fichier.txt
drwxr-xr-x 2 root root 4096 mars  17 17:45 test
````

2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?


3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test
6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?


7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.


10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.


11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :
- chmod u=rx,g=wx,o=r fic
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
- chmod 653 fic en sachant que les droits initiaux de fic sont 711
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd?
Pour les plus rapides :
15. Access Control Lists (ACL) : suivez le tutoriel de cette page :https://doc.ubuntu-fr.org/acl.
16. Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.



