
******
#TP 4 - Utilisateurs, groupes et permissions
******
##Exercice 1. Gestion des utilisateurs et des groupes

1. Commencez par créer deux groupes groupe1 et groupe2
la commande pour créer groupe est `sudo groupadd groupenom`. dans  notre cas,  le codage est  le suivant:

```linux
sudo groupadd groupe1
sudo groupadd groupe2
```
Et après la création de chaque groupe, on récupère le résultat de list de groupe en utilisant la commande suivant:
```linux
cat /etc/group
```
2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
Afin de créer des utilisateurs et leur dossier personnel, il faut utiliser la commande `useradd` avec l'option `-m` et -s pour definir bash pour shell.
```linux
 sudo useradd -m u1 -s /bin/bash
 sudo useradd -m u2 -s /bin/bash
 sudo useradd -m u3 -s /bin/bash
 sudo useradd -m u4  -s /bin/bash
```
pour vérifier toutes les créations d’utilisateur, on accède au  fichier /etc/passwd. On conserve des lignes suivantes :
```linux
u1:x:1001:1003::/home/u1:/bin/bash
u2:x:1002:1004::/home/u2:/bin/bash
u3:x:1003:1005::/home/u3:/bin/bash
u4:x:1004:1006::/home/u4:/bin/bash
```

3. Ajoutez les utilisateurs dans les groupes créés :
- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2
on utilise la commande `usermod` avec l’option -a -G pour  ajouter des utilisateurs existants  à un groupe  secondaire existant.
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


4. Donnez deux moyens d’afficher les membres de groupe2 :
 		 	 	 							
 les fichiers /etc/gshadow  et /etc/group Contiennent la liste des utilisateurs appartenant aux différents groupes, et les mots de passe de groupes. Quand on les affiche on obtient les résultats suivants:	

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


5. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4.
La commande chgrp (change group) permet de modifier le groupe propriétaire  d’un fichier/dossier:
```linux
sudo chgrp -R  groupe1 /home/u2
sudo chgrp -R groupe1 /home/u1
sudo chgrp  -R groupe2 /home/u3
sudo chgrp -R groupe2 /home/u4
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
pour faire cela nous avons utilisé la commande ‘ usermod -g ‘ pour modifier le groupe primaire  des utilisateurs u1, u2, u3 et u4. pour vérifier cela nous avons affiché le répertoire /ect/gshadow.

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
 on active le stickybit  pour que seul le propriétaire ait  le droit de renommer ou supprimer  ce fichier
```
sudo chmod +t  /home/groupe1
sudo chmod +t   /home/groupe2
```



9. Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
Non car l'utilisateur u1 n'est pas activé, n'ayant pas de mot de passe. La commande su u1 censée permettre la connection ne fonctionne alors pas.
```linux
su u1
Password: 
su: Authentication failure
```


10. Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte. 
on utilise la commande  passwd pour donner un mot de passe à u1.

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
on utilise la commande ‘id u1’ et on trouve que  l’uid  et le gid de u1 sont respectivement 1001 et 1001 
```linux
  id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
````

12. Quel utilisateur a pour uid 1003 ?

``linux  
cat /etc/passwd
u3:x:1003:1002::/home/u3:/bin/sh 
``` 
c’est l’utilisateur u3 qui a l’id 1003 car on a regardé la  3eme colonne  du fichier  /etc/passwd.



13. Quel est l’id du groupe groupe1 ?
le groupe1 a  l’id 1001 ,on regarde la  3eme colonne  du fichier  /etc/group

``linux  
cat  /etc/group
groupe1:x:1001:u1,u2,u4
```

14. Quel groupe a pour guid 1002 ? ( Rien n’empêche d’avoir un groupe dont le nom serait 1002...).
 c’est le groupe 2 qui a le guid 1002 . le group user id est  la  3eme colonne  du fichier  /etc/group.
``linux  
cat  /etc/group
groupe2:x:1002:u2,u3,u4
```


15. Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
etant donné que u3 n’a qu’un seul groupe à  la prochaine fois que l’on va se connecter il se retrouve toujours dans le groupe 2. 
```linux
cat  /etc/group
groupe1:x:1001:u1,u2,u4
groupe2:x:1002:u2,u3,u4

$ sudo gpasswd -d u3 groupe2
[sudo] password for user: 
Removing user u3 from group groupe2
```



16. Modifiez le compte de u4 de sorte que :
— il expire au 1 er juin 2020 on utilise l’option -E pour modifier la date d’expiration .
```linux 
~$ sudo chage -E 2020/06/01 u4
[sudo] password for user: 
```
— il faut changer de mot de passe avant 90 jours, l’option  -x pour modifier la durée de validité du mot de passe 
```linux
sudo passwd -x 90 u4
passwd: password expiry information changed.
```

— il faut attendre 5 jours pour modifier un mot de passe, l’option  -n pour le nombre de jour  avant lesquels on ne peut pas modifier le mot de passe.
```linux
sudo passwd -n 5 u4
```
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe , l’option -w pour  la durée d’avertissement avant  expiration du mot de passe 
```linux 
sudo passwd  -w 14 u4
```
— le compte sera bloqué 30 jours après expiration du mot de passe,  l’option -i pour la durée de  non blocage du compte après expiration .
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
user@server:$ sudo mkdir test 
user@server:~$ sudo echo "coucou la famille" > test/fichier.txt
user@server:~$ ls -R -l
-rw-rw-r-- 1 user user   18 mars  17 18:17 fichier.txt
drwxr-xr-x 2 root root 4096 mars  17 17:45 test
````
Pour créer un dossier test dans le dossier home, on utilise sudo mkdir test. On peut ensuite créer un fichier fichier.txt en tapant sudo echo "coucou la famille" > test/fichier.txt . On a drwxr-xr-x  sur test et -rw-rw-r-- sur fichier1. On peut le voir grâce à la commande ls -R -l.


2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

```linux 
 sudo chmod  ugo-rwx test/fichier.txt
---------- 1 root root 0 mars  18 14:45 fichier.txt
sudo echo " le coronavirus c'est la merde" > fichier.txt
cat fichier.txt
 le coronavirus c'est la merde
```

On retire tous les droits en entrant chmod ugo-rwx test/fichier.txt, puis on vérifie que les droits ont bien été retirés. On constate qu'avec sudo (exécution en tant que root), on peut toujours lire et écrire dans fichier.txt

3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

sudo chmod  u+wx test/fichier.txt
 ls -R -l
--wx------ 1 root root 0 mars  18 14:45 fichier.txt
echo "echo Hello" > fichier.txt
cat fichier.txt
echo Hello

On redonne les droits en écriture et exécution avec la commande chmod u+wx test/fichier.txt. On exécute ensuite la commande echo “echo Hello” > test/fichier.txt . Cela remplace le contenu de fichier.txt  par “echo Hello”. Les droits ont été respectés car nous n'avons fait qu'écrire avec cette opération.

4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

```linux
 .user@server:~$  ./fichier.txt
bash: ./fichier.txt: Permission denied
user@server:~$ sudo  ./fichier.txt
Hello
```
 On remarque que l'exécution ne fonctionne pas sans le “sudo”. Cela signifie que l'exécution fait aussi appel au droit de lecture, qui est refusé ici.


5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test

```linux
user@server:~/test$ sudo chmod u-r /home/user/test 
user@server:~/test$ ls 
ls: cannot open directory '.': Permission denied
user@server:~/test$ cat fichier.txt
echo hello
```
 On peut retirer les droits en lecture pour le répertoire actuel, ici test, en écrivant “chmod u-r .” . On a en conséquence une permission refusée pour afficher le contenu du répertoire via ls, ou encore pour exécuter ou afficher le contenu du fichier.txt . On en déduit que changer les permissions du répertoire a un impact sur les permissions des fichiers de ce répertoire. On accorde les droits “chmod u+r .”.


6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?
```linux 
user@server:~/test$  chmod ugo-w /home/user/test 
user@server:~/test$  chmod ugo-w /home/user/test/nouveau 
user@server:~/test$ echo "laaal"> nouveau 
-bash: nouveau: Permission denied
user@server:~/test$  chmod ugo+w /home/user/test 
user@server:~/test$ echo "laaal"> nouveau 
-bash: nouveau: Permission denied
user@server:~/test$ rm nouveau
rm: remove write-protected regular empty file 'nouveau'? y
user@server:~/test$ ls
faby  fichier  fichier.txt  mama  sstest
user@server:~/test$ 

On retire le droit d'écriture dans “nouveau” avec “chmod ugo-w nouveau” une fois placés dans test et “chmod ugo-w”  . On ne peut alors plus modifier le fichier “nouveau”, meme apres réattribution du droit de lecture  mais on peut le supprimer , la suppression ne nécessite donc pas les droits en écriture du fichier.


7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
``linux
user@server:~$  chmod ugo-x /home/user/test 
user@server:~$ cd test
-bash: cd: test: Permission denied
user@server:~$ ls /home/user/test 
ls: cannot access '/home/user/test/fichier.txt': Permission denied
ls: cannot access '/home/user/test/faby': Permission denied
ls: cannot access '/home/user/test/sstest': Permission denied
ls: cannot access '/home/user/test/mama': Permission denied
ls: cannot access '/home/user/test/fichier': Permission denied
faby  fichier  fichier.txt  mama  sstest
```

Une fois les droits en exécution retirés par la même méthode que précédemment pour le répertoire test, on ne peut ni créer, ni supprimer,ni modifier de fichier. On ne peut non plus se déplacer jusqu'à ce répertoire mais on peut en lister le contenu. Le droit d'exécution pour un répertoire est donc nécessaire pour pouvoir interagir avec son contenu, car on ne peut même pas s'y rendre si le droit n'est pas accordé.

8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?

```linux
user@server:~$ chmod ugo+x /home/user/test 
user@server:~$  cd test
user@server:~/test$  chmod ugo-x /home/user/test 
user@server:~/test$ cd sstest
-bash: cd: sstest: Permission denied
user@server:~/test$ rm mama
rm: cannot remove 'mama': Permission denied
user@server:~/test$ cd ..
user@server:~$ 
```

On ne peut pas modifier, créer ou supprimer un fichier dans le répertoire, ni se déplacer vers sstest ou lister le contenu de sstest. On doit donc posséder le droit d'exécution sur le répertoire courant pour pouvoir accéder à quoi que ce soit contenu dedans. On peut en revanche revenir en arrière avec cd .. .


9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
```linux

user@server:~$  chmod ugo+x /home/user/test 
user@server:~$  chmod g=r /home/user/test/fichier.txt 
user@server:~$ ls -R -l
-rwxr----- 1 user user   11 mars  19 14:25 fichier.txt
 
```
On donne le droit à une personne du groupe de lire seulement le fichier.txt en entrant “chmod g=r /home/user/test/fichier.txt ” une fois dans le répertoire test.


10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
```linux
user@server:~$ umask 077 
user@server:~$ umask -S 
u=rwx,g=,o=
user@server:~$ touch monde 
user@server:~$ ll monde
-rw------- 1 user user 0 mars  18 19:05 monde


user@server:~$ umask 077 
user@server:~$ umask -S 
u=rwx,g=,o=
user@server:~$ mkdir corona
user@server:~$  ll corona
drwx------ 2 user user 4096 mars  18 19:10 ./

```

11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
```linux
user@server:~$ umask 022
user@server:~$ umask -S
u=rwx,g=rx,o=rx
user@server:~$ touch la_boule
user@server:~$ ll la_boule 
-rw-r--r-- 1 user user 0 mars  18 19:44 la_boule

```


12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
```linux
user@server:~$ umask 033
user@server:~$ umask -S
u=rwx,g=r,o=r
user@server:~$ touch femme 
user@server:~$ ll femme 
-rw-r--r-- 1 user user 0 mars  18 19:49 femme
user@server:~$ mkdir vie 
user@server:~$ ll vie
total 8
drwxr--r-- 2 user user 4096 mars  18 19:49 ./
drwxr-xr-x 7 user user 4096 mars  18 19:49 ../
user@server:~$ umask -S
u=rwx,g=r,o=r

```



13. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :

- chmod u=rx,g=wx,o=r fic :   chmod 534 fic
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---: chmod 602 fic
- chmod 653 fic en sachant que les droits initiaux de fic sont 711: chmod u-x,g+r,o+w fic
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x--- : chmod 520 fic

14. Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd?
```linux 
user@server:~$  whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1ssl.gz /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
user@server:~$ ls -R -l /usr/bin/passwd
-rwsr-xr-x 1 root root 67992 août  29  2019 /usr/bin/passwd
user@server:~$ ls -R -l /etc/passwd
-rw-r--r-- 1 root root 1827 mars  13 15:23 /etc/passwd
```
 oui car il a pour utilisateur courant le root donc on a tout les droits




Pour les plus rapides :
15. Access Control Lists (ACL) : suivez le tutoriel de cette page :https://doc.ubuntu-fr.org/acl.
16. Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.



