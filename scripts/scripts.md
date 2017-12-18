<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

# Scripts en bash

## Création

Un script bash est une liste de commandes à effectuer par `bash`. Il suffit d'écrire cette liste dans un fichier, de préférence de nom terminant par `.sh`. Ensuite, __il fait dire au terminal d'exécuter le fichier__. Il existe deux possibilités:

- On utilise `bash`: `bash [fichier]` qui va dire au programme `bash` d'exécuter `[fichier]`
- On ajoute `#!/bin/bash` en première ligne du fichier; puis on effectue `chmod +x [fichier]` et enfin `./[fichier]`. Ça a pour effet de dire au terminal d'utiliser `bash` pour lire le fichier. C'est préférable à l'option précédente.

