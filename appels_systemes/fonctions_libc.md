<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

## `fopen`

```c
FILE *fopen(const char *pathname, const char *mode);
```

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

```c
int fclose(FILE *stream);
```

##### Fonction
- Ferme un flux passé en argument.

##### Arguments
- `FILE *stream`: pointeur vers le dexcripteur de flux à fermer.

##### Retour
- Un entier correspondant au statut, `0` si aucune erreur ne s'est produite. Dans le cas contraire, `errno` est mis à jour.

## `fwrite`

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

##### Fonction
- Écrit dans le flux correspondant à `stream` `nmemb` éléments de taille `size` bytes, lus dans `ptr`.

##### Arguments
- `const void *ptr`: La zone de mémoire à lire;
- `size_t size`: La taille des éléments à copier;
- `size_t nmemb`: Le nombre d'éléments à copier;
- `FILE *stream`: Le flux où écrire.

##### Retour
- Un entier correspondant au nombre de bytes écrits, `0` si une erreur s'est produite.

## `fread`

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

##### Fonction
- Lit dans le flux correpondant à `stream` `nmemb` éléments de taille `size` bytes, et les stocke dans `ptr`.

##### Arguments
- `void *ptr`: La zone mémoire de destination;
- `size_t size`: La taille des éléments à lire;
- `size_t nmemb`: Le nombre d'éléments à copier;
- `FILE *stream`: Le flux où lire.

##### Retour
- Un entier correspondant à la taille de la mémoire lue, et `0` si une erreur s'est produite.

## `fseek`

```c
int fseek(FILE *stream, long offset, int whence);
```

##### Fonction
- Repositionne dans le flux correpondant à `stream` de `offset` par rapport à `whence`: si `whence` est `SEEK_END` et `offset` est `0`, il va à `0` `bytes` de la fin.

##### Arguments
- `FILE *stream`: le descripteur de flux;
- `long offset`: Le décalage depuis `whence`;
- `int whence`: un entier spécial défini dans un `enum`, pouvant être:
    - `SEEK_SET`: Depuis le début;
    - `SEEK_CUR`: Depuis la position du curseur;
    - `SEEK_END`: Depuis la fin.

##### Retour
- `0` si tout s'est bien passé et `-1` si une erreur s'est produite. `errno` est mis à jour.

## `ftell`

```c
long ftell(FILE *stream);
```

##### Fonction
- Donne la position du curseur dans le flux `stream`.

##### Arguments
- `FILE *stream`: Le flux à considérer.

##### Retour
- Un `long` correspondant à la position du curseur dans le fichier et `-1` si une erreur s'est produite. `errno` est mis à jour.

