# Appels systèmes et flux

## Appels systèmes

### Explications

Lors de son lancement, un programme est lu puis exécuté par le processeur. Supposons que ce soit une méthode qui n'affiche rien: alors rien n'est écrit à la fin du programme. Pas très utile. On va alors dire au programme de sauvegarder ses informations dans la mémoire, ou sur un terminal, pour ne pas l'avoir lancé en vain.

Mais comment faire pour __lire ou écrire un fichier sauvegardé sur un disque dur__, alors que le programme est chargé en mémoire vive ? On __passe par le système d'exploitation, l'OS__, en faisant un __appel système__. Il y a deux méthodes:

- Faire des __appels systèmes directement__, comme des gros cochons: cependant, cela ne __marche que sous UNIX__, et est très __peu performant__ si mal fait;
- Utiliser __`libc`__: ensemble de fonctions standard pour le `C`, codées pour tout __type de système d'exploitation__, et qui contrôlent ce que fait l'utilisateur pour __effectuer au mieux les appels systèmes__.

Il est clair que la deuxième option est préférable. Donc nous allons faire les deux. Il faut savoir qu'un appel système fait passer le programme en un 'mode' spécial pour effectuer sa tâche, ce qui stoppe le programme le temps de l'exécution de cet appel: il faut donc les utiliser à bon escient.

### Utilisation générale

Le cours donne un exemple d'__appel système classique, non `libc`__:

```c
#include <stdio.h>
#include <string.h>	//pour strerror
#include <errno.h>	//pour errno

int main (int argc , char *argv[]) {
    ...
    if (unAppelSysteme (...) == -1) {
	fprintf (stderr, "%s:unAppelSysteme a planté: %s\n", argv[0], strerror(errno));
	return 1 ;		// ou exit(1)
    }
    ...
    return 0 ;
}
```

Que faut il en retenir ?

- Les appels systèmes __non libc__ renvoient des entiers. Plus particulièrement, __les appels systèmes classiques renvoient `-1` en cas d'erreur__, pratique pour tester si il y a eu une erreur en route.
- __Il existe deux librairies `errno.h` et `string.h` qui s'utilisent avec les appels systèmes__. Lors d'un plantage, `errno` est mis à jour; __`strerror(errno)` renvoie une `string` avec un message d'erreur qui correspond au problème__.

## Les flux: qu'est-ce ?

__Les flux sont les entrées/sorties d'un programme__. Ce sont des fichiers que le programme peut lire ou écrire, via le `kernel`. Comme sous UNIX, tout est un fichier, on peut ainsi écrire dans diverses choses: fichiers textes, terminaux, CD/DVD .. Il suffit de savoir quel fichier ouvrir. __Chaque lecture ou écriture fait un appel système__.

### Ouverture de flux avec des appels systèmes

Avec les appels systèmes classiques, __un `int` nommé ` fd` représente un flux__. Les fonctions à savoir utiliser sont:

```c
// Ouvrir un flux, renvoie un fd correspondant:
int open(const char *pathname, int flags);

// Fermer le flux croorespondant à fd:
int close(int fd);

// Ecrire dans le flux:
ssize_t write(int fd, const void *buf, size_t count);

// Lire dans le flux:
ssize_t read(int fd, void *buf, size_t count);

// Se déplacer dans le flux:
off_t lseek(int fd, off_t offset, int whence);

// Copier des flux:
int dup(int oldfd);
int dup2(int oldfd, int newfd);
```

Toutes ces fontions se trouvent dans `<unistd.h>`.

## `open`

> `int open(const char *pathname, int flags)`

##### Fonction
- Ouvre un flux vers le fichier passé en argument.

##### Arguments
- `const char *pathname`: Le nom du fichier;
- `int flags`: un entier spécial défini dans un `enum`, pouvant être:
    - `O_RDONLY`: lecture seule;
    - `O_WRONLY`: lecture seule;
    - `O_RDWR`: lecture et écriture;
    - `O_TRUNC`: effacement avant écriture;
    - `O_CREAT`: création si non-existent;
    - `O_APPEND`: écriture en fin de fichier.

#### Retour
- Un entier `fd` correspondant au flux créé, `-1` sinon.

## `close`

> `int close(int fd)`

##### Fonction
- Ferme un flux passé en argument.

##### Arguments
- `int fd`: l'entier correspondant au flux.

##### Retour
- Un entier correspondant au statut, `-1` si une erreur s'est produite.

## `write`

> `ssize_t write(int fd, const void *buf, size_t count)`

##### Fonction
- Écrit dans un flux passé en argument.

##### Arguments
- `int fd`: l'entier correspondant au flux;
- `const void *buf`: un pointeur vers la mémoire à écrire dans le fichier;
- `size_t count`: la taille de la mémoire à écrire.

##### Retour
- Un entier correspondant au nombre de bytes écrits, `-1` si une erreur s'est produite.

## `read`

> `ssize_t read(int fd, void *buf, size_t count)`

##### Fonction
- Lit dans le flux correpondant à `fd` `count` bytes, et les stocke dans `buf`.

##### Arguments
- `int fd`: le descripteur de flux;
- `void *buf`: un pointeur vers la zone mémoire où stocker les infos lues;
- `size_t count`: la taille de la mémoire à lire.

##### Retour
- Un entier correspondant à la taille de la mémoire lue, et `-1` si une erreur s'est produite.

## `lseek`

> `off_t lseek(int fd, off_t offset, int whence)`

##### Fonction
- Repositionne dans le flux correpondant à `fd` de `offset` par rapport à `whence`: si `whence` est `SEEK_END` et `offset` est `0`, il va à `0` `bytes` de la fin.

##### Arguments
- `int fd`: le descripteur de flux;
- `off_t offset`: Le décalage depuis `whence`;
- `int whence`: un entier spécial défini dans un `enum`, pouvant être:
    - `SEEK_SET`: Depuis le début;
    - `SEEK_CUR`: Depuis la position du curseur;
    - `SEEK_END`: Depuis la fin.

##### Retour
- Un entier correspondant à la position du curseur dans le fichier, et `-1` si une erreur s'est produite.

## `dup` et `dup2`

> `int dup(int oldfd)`

> `int dup2(int oldfd, int newfd)`

##### Fonction
- Copie les flux. `dup` crée un nouveau `fd` qui pointe vers le même flux que `oldfd`, alors que `dup2` ferme `newfd` et le fait pointer vers le même flux que `oldfd`.

##### Arguments
- `int oldfd`: le descripteur de flux à copier;
- `int newfd`: le flux à écraser et faire pointer vers `oldfd`.

##### Retour
- Un entier correspondant au `fd` créé, et `-1` si une erreur s'est produite.
