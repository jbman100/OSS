# Fonctions systèmes

## Exécuter des programmes

### Méthode classique

> `int execve(const char *path, char *const argv[], char *const envp[])`

##### Fonction

Exécute un programme annexe.

##### Arguments

- `const char *path`: le chemin vers le programme à exécuter;
- `char *const argv[]`: les arguments à donner au programme;
- `char *const envp[]`: l'environnement à donner au programme: des variables externes pouvant influer sur son comportement.

##### Retour

__Rien en cas de succès__, `-1` sinon.


