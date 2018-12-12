<a href="http://skutnik.iiens.net/cours/1A/OSS">Retour au sommaire</a>

## Commandes basiques

```
man [programme]: afficher le manuel du programme
```

## Mouvements

```
ls [dossier]: lister le contenu du fichier
cd: Revient au dossier de base
cd [dossier]: changer de dossier
```

## Gestion de fichiers

```
rm [fichier]: effacer un fichier
rm -r [dossier]: effacer un dossier
cp [fichier] [destination]: copie un fichier
mv [fichier] [destination]: bouge (ou rennome) un fichier
mkdir [dossier]: crée le dossier de nom [dossier]
chmod [droits] [fichier]: change les droits de [fichier] à [droits]
```

## Affichage de fichiers

```
cat [fichier]: affiche le fichier
grep 
less [fichier]: affiche le fichier de façon lisible (essayez !)
head -n [nombre]: affiche les [nombre] premières lignes
tail -n [nombre]: affiche les [nombre] dernières lignes
```

## Processus

```
ps aux: affiche les processus actifs
kill [pid]: tue le processus de pid [pid]
<Ctrl-C>: Arrête le processus
<Ctrl-D>: Envoie EOF au processus; quitte le terminal
<Ctrl-Z>: Endort le terminal
```
