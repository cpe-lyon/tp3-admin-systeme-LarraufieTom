LARRAUFIE
TOM 
3ICS

# TP 3 - commandes linux

## Exercice 1. Commandes de base Commencez par mettre à jour votre système avec les commandes vues dans le cours. Donnez les commandes répondant aux questions suivantes : 

On commence par mettre à jour notre système.
Pour cela on utilise la commande **sudo apt full-upgrade**
>tom@serveur:~$ sudo apt full-upgrade

####  1. Quels sont les 5 derniers paquets installés sur votre machine ?
Pour retrouver les derniers paquets installés sur notre machine, on utilise les fichiers d'historique **dpkg.log** se trouvent dans **/var/log**.

Ensuite on utilise une commande pour afficher l'historique et pour afficher les 5 derniers seulement.
La commande est la suivante:
```cat dpkg.log | tail -5```


#### 2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ? 
Pour compter le nombre de lignes d'une recherche, on utilise la commande **wc -l**
Donc pour compter le nombre de paquets installés, on utilise les commandes suivantes:

Avec dpkg: **/var/log$ dpkg -l | grep "^ii" | wc -l**
>**tom@serveur:/var/log$ dpkg -l | grep "^ii" | wc -l
514**

Avec apt: **apt list --installed | wc -l**
>tom@serveur:/var/log$ apt list --installed | wc -l
WARNING: apt does not have a stable CLI interface. Use with caution in scripts.
515


#### 3. Combien de paquets sont disponibles en téléchargement ? 
Pour compter le nombre de paquets disponible en téléchargement on utilise la commande **apt list** qui permet d'afficher la liste de tout les paquets téléchargeable.
On ajoute a cela la commande **wc -l** qui permet de compter le nombre de ligne retourner par la commande précédente.

**tom@serveur:/var/log$ apt list | wc -l**

>WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

64118

#### 4. Créer un alias “maj” qui met à jour le système
On entre dans le fichier .bashrc pour pouvoir créer un nouvel alias.
> nano .bashrc

Puis on ajoute l'alias **maj** qui va permettre de mettre a jour le système.
```alias=nano apt full-upgrade```

Ensuite, pour que notre commande soit prise en compte, nous devons quitter le terminal et le relancer ou saisir la commande suivante: **source ~/.bashrc**

Nous pouvons ensuite utiliser notre alias:
>tom@serveur:~$ maj
[sudo] password for tom:
Lecture des listes de paquets... Fait
Construction de l'arbre des dépendances
Lecture des informations d'état... Fait
Calcul de la mise à jour... Fait
0 mis à jour, 0 nouvellement installés, 0 à enlever et 0 non mis à jour.

#### 5. A quoi sert le paquet fortunes ? Installez-le. 
On commence par installer fortunes.
**tom@serveur:~$ sudo apt install fortunes**

Les fortunes sont de petits messages, des citations, des proverbes, etc. affichés à chaque connexion en mode console (terminal)

#### 6. Quels paquets proposent de jouer au sudoku ? 
Le paquet qui permet de jouer au sudoku est le suivant:
**sudo apt install sudoku**

#### 7. Lister les derniers paquets installés explicitement avec la commande apt install
Pour regarder quels sont les derniers paquets installés explicitement avec la commande **apt install**, nous devons rechercher dans l'historique avec les mots clés "apt install" avec la commande suivante.
**grep "apt install" /var/log/apt/history.log**

La commande **grep "apt install"** recherche tout les paquets qui contiennent cette chaîne de caractère. La cherche est effectué dans le fichier history.log qui ce trouve dans **/var/log/apt/history.log**



## Exercice 2. 

#### A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles) ? Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.
Pour trouver quel paquet a installé la commande **ls** on entre la commande suivante:
**which -a ls | xargs dpkg -S 2> /dev/null**

On utilise **xargs** car la commande dpkg ne peut pas récupéré en entrée les donnée de sortie de la commande précédente.
Car cette commande entre des valeurs en paramètres uniquement.
**-S** lance une recherche sur les paquets installés.
**2> /dev/null** redirige les erreurs dans ce répertoire qui est un répertoire de suppression. 



## Exercice 3. 
#### Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande. 

nous pouvons exécuter une instruction en fonction du résultat de l'instruction précédente:
-**[instruction A] && [instruction B]**  
si le résultat de l'instruction A retourne 0 , l'instruction B est exécuté.
-**[instruction A] || [instruction B]** 
si le résultat de l'instruction A ne retourne pas 0 , l'instruction B est exécuté.

Voici le script:
```bash
#!/bin/bash

(dpkg -l | grep "$1" ) 1>/dev/null && echo "INSTALLÉ" || echo "NON INSTALLÉ"
```



## Exercice 4. 
#### Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment afficher ce qu’elle retourne ? 
```console
tom@serveur-VirtualBox:~$ dpkg -L coreutils
/.
/bin
/bin/cat
/bin/chgrp
/bin/chmod
/bin/chown
/bin/cp
/bin/date
/bin/dd
/bin/df
/bin/dir
/bin/echo
/bin/false
/bin/ln
/bin/ls
/bin/mkdir
/bin/mknod
.
.
tom@serveur-VirtualBox:~$
```

## Exercice 5. 
#### aptitude Installez le paquet emacs à l’aide de la version graphique d’aptitude. 

On commence par installer aptitude

**tom@serveur-VirtualBox:~$ sudo apt install aptitude**

Puis on lance aptitude

**tom@serveur-VirtualBox:~$  aptitude**

Ensuite on entre dans les paquets non installé.
Puis **maj + /** nous ouvre une barre de recherche et l'on cherche **emacs**, on peut cliquer sur **n** pour passer les pages.

On clique sur **+** pour sélectionner tout les paquets d'emacs.

On clique sur **g** pour installé les packages.


## Exercice 6. 
#### Installation d’un paquet par PPA Certains logiciels ne figurent pas dans les dépôts officiels. C’est le cas par exemple de la version ”officielle” de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt personnel” ou PPA. 1. Installer la version Oracle de Java (avec l’ajout des PPA) sudo add-apt-repository ppa:linuxuprising/java sudo apt update sudo apt install oracle-java11-installer 2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?

On installe la version Oracle de java
```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java12-installer
```
Puis on appuis sur **OK**
Puis sur **YES**

**Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?**

>tom@serveur:~$ cd /etc/apt/sources.list.d
tom@serveur:/etc/apt/sources.list.d$ ls
linuxuprising-ubuntu-java-disco.list
tom@serveur:/etc/apt/sources.list.d$ nano linuxuprising-ubuntu-java-disco.list


Le nouveau fichier qui a été crée contient un lien vers une plateforme de mise à jour afin de faire la maintenance de la version d'oracle installé.

Il va se connecter sur le lien puis rentrer dans le main.


># deb-src http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main

