<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

# Processus

Il existe plusieurs méthodes pour créer des processus. On peut créer des processus lourds, copies complètes des processus les ayant créés, ou des processus légers, partageant des éléments avec le père.

## Processus lourd - `fork`

```c
pid_t fork(void);
```

##### Fonction
- `fork` crée un nouveau processus en clonant le processus qui l'utilise. Le nouveau processus est le processus fils.

##### Retour
- __Retourne `0` dans le processus fils__.
- __Retourne le PID du fils dans le processus père__.
- Retourne `-1` en cas d'échec. `errno` est mis à jour.
