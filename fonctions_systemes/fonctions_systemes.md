<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

# Fonctions systèmes

## Exécuter des programmes

### Méthode classique

```c
int execve(const char *path, char *const argv[], char *const envp[])
```

##### Fonction

- Exécute un programme annexe.

##### Arguments

- `const char *path`: le chemin vers le programme à exécuter;
- `char *const argv[]`: les arguments à donner au programme;
- `char *const envp[]`: l'environnement à donner au programme: des variables externes pouvant influer sur son comportement.

##### Retour

- __Rien en cas de succès__, `-1` sinon.

## Quitter

```c
int _exit(int status);
```

##### Fonction

- Libère la mémoire allouée, envoie les signaux adéquats et termine le processus en renvoyant le statut `status`.

##### Arguments

- `int status`: le statut à renvoyer. Il est de bon goût d'utiliser les status `EXIT_SUCCESS` et `EXIT_FAILURE`, définis en fonction du système.

##### Retour

- Rien.


