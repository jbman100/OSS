<a href="http://skutnik.iiens.net/cours/1A/OSS">Retour au sommaire</a>

# Processus

## Processus UNIX

- Il existe plusieurs méthodes pour créer des processus. On peut créer des processus lourds, copies complètes des processus les ayant créés, ou des processus légers, partageant des éléments avec le père.
- Il faut bien comprendre que __dès l'appel de la fonction de clonage, il existe deux processus. La seule différence est le retour de cette fonction__: c'est comme ça que l'on distingue le père du fils.

### Processus lourd - `fork`

```c
pid_t fork(void);
```

##### Fonction
- `fork` crée un nouveau processus en clonant le processus qui l'utilise. Le nouveau processus est le processus fils.

##### Retour
- __Retourne `0` dans le processus fils__.
- __Retourne le PID du fils dans le processus père__.
- Retourne `-1` en cas d'échec. `errno` est mis à jour.

### Processus léger

Il n'existe apparemment pas de moyens de les créer. le cours conseille de le faire en assembleur.
