<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

# Communication inter-processus

## Signaux

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

```
