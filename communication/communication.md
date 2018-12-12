<a href="http://skutnik.iiens.net/cours/1A/OSS">Retour au sommaire</a>

# Communication inter-processus

## Sommaire

1. [Signaux](#signaux)
2. [FIFO](#fifo)


## <a name="signaux"></a>Signaux

### Description

Les signaux sont des évènements que les processus peuvent s'envoyer. Selon le signal, le programme réagit de façon différente:

- `Ign`: Il peut l'ignorer;
- `Term`: Il peut se terminer;
- `Core`: Il peut se terminer en sauvegardant son état;
- `Stop`: Il peut s'endormir;
- `Spe`: Il peut effectuer une action spéciale.

Les différents signaux sont:

| Nom	      | Numéro | Origine				    | Action    |
|-------------|--------|--------------------------------------------|-----------|
| `SIGHUP`    | 1      | Mort du terminal ou processus parent	    | `Term`	|
| `SIGINT`    | 2      | Interruption depuis le clavier		    | `Term`	|
| `SIGQUIT`   | 3      | Sortie depuis le clavier		    | `Core`	|
| `SIGILL`    | 4      | Instruction illégale			    | `Core`	|
| `SIGABRT`   | 6      | Signal arrêt depuis `abort(3)`		    | `Core`	|
| `SIGBUS`    | 7      | Erreur de bus (mauvais accès mémoire)	    | `Core`	|
| `SIGFPE`    | 8      | Exeception `float` (?)			    | `Core`	|
| `SIGKILL`   | 9      | __Signal de KILL__			    | `Term`	|
| `SIGSEGV`   | 11     | Erreur de segmentation			    | `Core`	|
| `SIGPIPE`   | 13     | `pipe` cassée ou invalide		    | `Term`	|
| `SIGALRM`   | 14     | Signal de `alarm(2)`			    | `Term`	|
| `SIGTERM`   | 15     | Signal de terminaison			    | `Term`	|
| `SIGUSR1`   | 10     | Signal utilisateur 1	        	    | `Term`	|
| `SIGUSR2`   | 12     | Signal utilisateur 2			    | `Term`	|
| `SIGCHLD`   | 17     | Enfant mort ou arrêté			    | `Ign`	|
| `SIGCONT`   | 18     | Signal de redémarrage			    | `Cont`	|
| `SIGSTOP`   | 19     | __Signal d'arrêt__			    | `Stop`	|
| `SIGTSTP`   | 20     | Stop écrit en `tty`			    | `Stop`	|
| `SIGTTIN`   | 21     | Entrée `tty` pour processus en arrière plan| `Stop`	|
| `SIGTTOU`   | 22     | Sortie `tty` pour processus en arrière plan| `Stop`	|

Pas besoin de tout apprendre, les plus importants sont en gras. __Les signaux `SIGKILL` et `SIGSTOP` ne peuvent être arrêtés, bloqués, ou ignorés__.

L'envoi d'un signal est quasi instantané. Sa réception est elle plus hasardeuse: le programme écoute les signaux à ceratains moments de son exécution et peut mettre du temps à le reçevoir.

- __De façon peu intuitive, `kill` envoie un signal, et `signal` reçoit un signal__.

### Utilisation

## `kill`

```c
int kill(pid_t pid, int sig);
```

##### Fonction
- Envoie le signal `sig` au processus de PID `pid`.

##### Arguments
- `pid_t pid`: un PID;
- `int sig` Un entier correspondant à un signal.

##### Retour
- Rien en cas de succès, `-1` en cas d'échec. `errno` est mis à jour.

## `signal`

```c
void (*signal(int sig, void (*func)(int)))(int);
```

##### Fonction
- Associe au signal `sig` l'action `void (*func)(int))`. Cet argument __infect__ est en fait un pointeur vers une fonction, qui sera appelée lors de la réception du signal.
- En donnant `SIG_IGN` en tant que deuxième argument, le signal sera ignoré;
- En donnant `SIG_DFL` en tant que deuxième argument, le signal sera géré comme par défaut.

##### Arguments
- `int sig` Un entier correspondant à un signal;
- `void (*func)(int)`: Un pointeur vers une fonction.

##### Retour
- Le gestionnaire précédent en cas de succès, `SIG_ERR` en cas d'échec. `errno` est mis à jour.

##### Exemple

```c
void terminator(int signo) {
    printf("RECEIVED SIGPIPE\n");

    if (signo != SIGPIPE) // Dans le cas où il y a eu une c*uille;
	return;
    
    /* Actions à faire dans ce cas */
    ...
}

// Dans le main

if (signal(SIGPIPE, terminator) == SIG_ERR) // Teste le résultat de signal
    printf("Il y a eu une erreur\n");
else
    printf("terminator sera lancé lorsque SIGPIPE sera reçu\n");
```

## `pause`

```c
int pause(void);
```

##### Fonction
- Pause le programme jusqu'à la réception d'un signal non ignoré.

## <a name="fifo"></a>Les __`FIFO`__

### Description

`FIFO` vient de 'first in first out'. C'est un modèle de file: __il faut le voir comme un tuyau__. Un `FIFO` à un sens: __les informations rentrent du coté des 'écrivains' et sortent du coté des 'lecteurs'__. Il existe une taille maximale: une fois atteinte, les écrivains ne peuvent rien mettre tant que les lecteurs ne lisent pas les informations.

__On s'intéresse ici au `pipe`__, un type de `FIFO`. Elle permet de partager des données avec d'autres processus qui ont eux aussi ouvert le `pipe`.

- Il existe deux types de `pipe`: les `pipe` nommés, pouvant connecter divers processus, et les `pipe` anonymes, connectant uniquement des processus apparentés.
    + Les `pipe` nommés sont créés avec la commande `mkfifo`, __hors du programme__.
    + Les `pipe` anonymes sont créés avec la fonction `pipe` de `<unistd.h>`.
- Le `pipe` s'ouvre comme un flux classique.
- Les lecteurs sont bloqués losque la `pipe` est vide.
- Les écrivains sont bloqués losque la `pipe` est pleine.
- __Si on essaye d'écrire dans une `pipe` sans lecteur, on reçoit un `SIGPIPE`__. C'est un bon moyen de savoir si un autre programme est arrêté: il faut par contre mettre à jour le gestionnaire de `SIGPIPE` car il a pour effet de terminer le processus.

Une fois ouvert, __on lit et écrit au `pipe` comme un flux classique__.

## `pipe`

```c
int pipe(int fildes[2]);
```

##### Fonction
- Crée un `pipe`: met dans `fildes[0]` le fd de la sortie et dans `fildes[1]` le fd de l'entrée.

##### Arguments
- `int fildes[2]`: Un tableau d'`int` de taille `2`.

##### Retour
-  `0` si tout s'est bien passé; `-1` sinon et `errno` est mis à jour.
