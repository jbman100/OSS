<a href="http://skutnik.iiens.net/cours/1A/OSS">Retour au sommaire</a>

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

### Utilisation des appels systèmes

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

Toutes ces fontions se trouvent dans `<unistd.h>`. Pour plus de détails sur ces fonctions, voir <a href="http://skutnik.iiens.net/cours/1A/OSS/appels_systemes/fonctions.html">cette page</a>.

__L'utiliisation de flux classiques utilise ses propres concepts__:

- On peut mettre plusieurs options dans `open` en les séparant par des `|`: pour ouvrir un fichier en lecture seule, on peut mettre `O_RDONLY|O_CREATE`;
- On ne peut lire un fichier ouvert avec l'option `O_RDONLY` ou écrire dan un fichier ouvert avec l'option `O_WRONLY`.
- À un flux est associé un __curseur qui est décalé à chaque lecture ou écriture__: effectuer 3 fois `read(fd, &sortie, 10*sizeof(char))` va mettre dans `sortie` 10 `char` au premier appel, puis les 10 `char` suivant au second, et enfin les 10 `char` qui les suivent au troisième.
- Pour repositionner ce curseur, on utilise `lseek`: suivant `whence`, le curseur est décalé à `offset` de:
    - La fin si on donne `SEEK_END` à whence;
    - Le début si on donne `SEEK_SET`;
    - La position du curseur si on donne `SEEK_CUR`.
- `lseek` renvoie le nombre de bytes qui ont été sautés. __Donc pour calculer la taille d'un fichier, on effectue `int size = lseek(fd, 0, SEEK_END)`__, qui bouge le curseur à `0` bytes de la fin, et qui donne le nombre de bytes qui ont été passés depuis la position précédente. Il faut pour avoir la taille entère que le curseur soit au début du fichier.
