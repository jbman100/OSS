<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

## `fopen`

> `FILE *fopen(const char *pathname, const char *mode)`

##### Fonction
- Ouvre un flux vers le fichier passé en argument. Les modes `w`, `w+`, `a` et `a+` créent le fichier s'il n'existe pas.

##### Arguments
- `const char *pathname`: Le nom du fichier;
- `const char *mode`: Une string pouvant être:
    - `r`: lecture seule;
    - `r+`: lecture et écriture, depuis le début;
    - `w`: écriture seule, le fichier est effacé;
    - `w+`: lecture et écriture, après effacement;
    - `a`: écriture en fin de fichier;
    - `a+`: lecture et écriture en fin de fichier.

#### Retour
- Un pointeur vers un objet `FILE` correspondant au flux créé, `NULL` sinon.

## `fclose`

> `int fclose(FILE *stream)`

##### Fonction
- Ferme un flux passé en argument.

##### Arguments
- `FILE *stream`: pointeur vers le dexcripteur de flux à fermer.

##### Retour
- Un entier correspondant au statut, `0` si aucune erreur ne s'est produite. Dans le cas contraire, `errno` est mis à jour.

## `fwrite`

> `ssize_t write(int fd, const void *buf, size_t count)`

##### Fonction
- Écrit dans un flux passé en argument.

##### Arguments
- `int fd`: l'entier correspondant au flux;
- `const void *buf`: un pointeur vers la mémoire à écrire dans le fichier;
- `size_t count`: la taille de la mémoire à écrire.

##### Retour
- Un entier correspondant au nombre de bytes écrits, `-1` si une erreur s'est produite.

## `fread`

> `ssize_t read(int fd, void *buf, size_t count)`

##### Fonction
- Lit dans le flux correpondant à `fd` `count` bytes, et les stocke dans `buf`.

##### Arguments
- `int fd`: le descripteur de flux;
- `void *buf`: un pointeur vers la zone mémoire où stocker les infos lues;
- `size_t count`: la taille de la mémoire à lire.

##### Retour
- Un entier correspondant à la taille de la mémoire lue, et `-1` si une erreur s'est produite.

## `fseek`

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
