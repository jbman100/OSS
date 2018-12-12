<a href="http://skutnik.iiens.net/cours/1A/OSS">Retour au sommaire</a>

## Déplacements

Dans le cadre du cours, on travaille sur un système UNIX. Dans un tel système, __les fichiers sont organisées en arbre__, dont la racine (`root`) est notée `/`. Quand un terminal est ouvert, __il se situe à un endroit précis du système de fichier__, comme le montre le 'prompt':

```
jb@pc253-09:~/Desktop $ 
	    ^^^^^^^^^
```

`~` est en fait le dossier de base de l'utilisateur: en général `/home/[nom de l'utilisateur]`, raccourci à juste `~`. Ici, je me trouve donc à `/home/jb/Desktop`. __Pour voir ce qu'il y a là où on se trouve, il faut lancer `ls`__:

```
jb@pc253-09:~/Desktop $ ls
Dossier1  Dossier2  fichier1.c  fichier2.txt
```

__Pour se déplacer dans le système, on utilise `cd [destination]`__.

```
jb@pc253-09:~/Desktop $ cd Dossier1/
	    ^^^^^^^^^
jb@pc253-09:~/Desktop/Dossier1 $ 
	    ^^^^^^^^^^^^^^^^^^
```

Le prompt indique un chemin différent.

### Notion de chemin

Le système de fichier est un arbre orienté. Cela veut dire qu'__il existe une unique route partant de la racine et allant jusqu'au fichier: c'est le chemin de ce fichier__. 

#### Chemin absolu

Le chemin absolu est le chemin partant de la racine. Dans l'exemple précédent, `fichier1.c` est dans `Desktop`, qui est dans `~`: son chemin est donc

```
~/Desktop/fichier1.c		    or ~ est /home/jb/

=> /home/jb/Desktop/fichier1.c	    Chemin absolu
```

Le premier caractère est `/` qui est la racine du système de fichiers.

#### Chemin relatif

Regardons le contenu d'un dossier, en utilisant les options `-a` et `-l`, pour 'all' et 'list':

```
jb@pc253-09:~/Desktop $ ls -a -l
total 16
drwxr-xr-x  4 jean-baptiste.skutnik student 4096 Dec 18 11:45 .		<- Ces fichiers sont
drwx------ 18 jean-baptiste.skutnik student 4096 Dec 18 11:38 ..	<- apparus !
drwxr-xr-x  2 jean-baptiste.skutnik student 4096 Dec 18 11:45 Dossier1
drwxr-xr-x  2 jean-baptiste.skutnik student 4096 Dec 18 11:45 Dossier2
-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier2.txt
```

Pour faciliter les mouvements, il existe deux dossiers spéciaux dans tous les répertoires:

- `.` est une référence au dossier lui-même
- `..` est une référence au dossier précédent

Par exemple:

```
jb@pc253-09:~/Desktop/Dossier1 $ cd .
	    ^^^^^^^^^^^^^^^^^^
jb@pc253-09:~/Desktop/Dossier1 $
	    ^^^^^^^^^^^^^^^^^^
```

```
jb@pc253-09:~/Desktop/Dossier1 $ cd ..
	    ^^^^^^^^^^^^^^^^^^
jb@pc253-09:~/Desktop $ 
	    ^^^^^^^^^
```

On utilise alors ces dossiers pour créer des __chemins relatifs à la position du terminal__:

```
jb@pc253-09:~/Desktop/Dossier1 $ cd ../Dossier2
			     ^
jb@pc253-09:~/Desktop/Dossier2 $
			     ^
```

`cd` est retourné dans le dossier précédent, puis dans `Dossier2`.

## Fichiers et droits

Regardons plus en détail le résultat de `ls -al` (on regroupe les options):

```
-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
°^^^^^^^^^  * ~~~~~~~~~~~~~~~~~~~~~ -------
```

Beaucoup d'informations sont présentées.

- Le caractère souligné par `°` est le type du fichier;
- La partie soulignée en `~` est le nom de l'utilisateur détenteur du fichier;
- La partie soulignée en `-` est le groupe d'utilisateurs possédant ce fichier;
- Le chiffre souligné par `*` est le nombre de fichiers que contient ce ficher (utile uniquement pour les dossiers);
- La chaine de caractères soulignée par `^` représente les __droits d'accés__.

#### Droits d'accés

Tout fichier possède trois types de droits:

- Les droits du propriétaire, les lettres: `rwx------`;
- Les droits du groupe: `---rwx---`;
- Les droits des autres: `------rwx`.

Où les lettres signifient:

- `r`: read
- `w`: write
- `x`: execute

Ainsi dans l'exemple précédent:

```
-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
 ^^^ Le propriétaire a les droits de lecture et d'écriture

-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
    ^^^ Le groupe a le droit de lecture

-rw-r--r--  1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
       ^^^ Les autres ont le droit de lecture
```

En mémoire, cela est codé sur 3 bits:

```
En binaire: 100 - r - 4 en base 10
En binaire: 010 - w - 2 en base 10
En binaire: 001 - x - 1 en base 10
```

Ce qui fait que l'on note les droits en chiffres, de 0 à 7:

```
7 - 4+2+1 - rwx
6 - 4+2+0 - rw-
5 - 4+0+1 - r-x
4 - 4+0+0 - r--
3 - 0+2+1 - -wx
2 - 0+2+0 - -w-
1 - 0+0+1 - --x
0 - 0+0+0 - ---
```

Ainsi, des droits en `666` codent `rw-rw-rw-`, lecture et écriture pour tout le monde. __Pour les modifier, on utilise `chmod`__:

```
jb@pc253-09:~/Desktop/Dossier1 $ ls -al
total 8
drwxr-xr-x 2 jean-baptiste.skutnik student 4096 Dec 18 12:51 .
drwxr-xr-x 4 jean-baptiste.skutnik student 4096 Dec 18 12:51 ..
-rw-r--r-- 1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
 ^^^^^^^^^
jb@pc253-09:~/Desktop/Dossier1 $ chmod 666 fichier1.c 

jb@pc253-09:~/Desktop/Dossier1 $ ls -al
total 8
drwxr-xr-x 2 jean-baptiste.skutnik student 4096 Dec 18 12:51 .
drwxr-xr-x 4 jean-baptiste.skutnik student 4096 Dec 18 12:51 ..
-rw-rw-rw- 1 jean-baptiste.skutnik student    0 Dec 18 11:45 fichier1.c
 ^^^^^^^^^
```

#### Les ID

Chaque utilisateur et groupe ont un `ID`:

- Pour les utilisateurs: c'est le `User ID` soit `UID`;
- Pour les groupes: c'est le `Group ID` soit `GID`.

Ce sont des codes qui représentent les ustilisateurs ou groupes. Pour les voir, on utilise la commande `id`.
