<a href="http://skutnik.iiens.net/cours/OSS">Retour au sommaire</a>

# Scripts en bash

## Création

Un script bash est une liste de commandes à effectuer par `bash`. Il suffit d'écrire cette liste dans un fichier, de préférence de nom terminant par `.sh`. Ensuite, __il fait dire au terminal d'exécuter le fichier__. Il existe deux possibilités:

- On utilise `bash`: `bash [fichier]` qui va dire au programme `bash` d'exécuter `[fichier]`
- On ajoute `#!/bin/bash` en première ligne du fichier; puis on effectue `chmod +x [fichier]` et enfin `./[fichier]`. Ça a pour effet de dire au terminal d'utiliser `bash` pour lire le fichier. C'est préférable à l'option précédente.

```shell
#!/bin/bash

if ! [ $(which ruby) ] ; then
    echo You do not have ruby installed !
    exit 1
fi

function check_for_gem() {
    gem=$(gem list | grep $1)

    echo "$1: $gem"
    if [ "$gem" != "" ] ; then
	echo "Gem $1 has been found"
    else
	echo -n "The gem $1 is not installed. Install ? [Y/n] "
	read answer

	if ! [ $answer == "n" ] ; then
	    gem install $1
	else
	    exit 1
	fi
    fi
}

check_for_gem commonmarker
check_for_gem github-markup

mkdir -p "$HOME"/.local/bin
cp ./mdtoml "$HOME"/.local/bin
chmod +x "$HOME"/.local/bin/mdtoml

echo
echo "mdtoml has been copied to /home/"$USER"/.local/bin/mdtoml"
echo "You may need to update your PATH. See:"
echo "https://wiki.archlinux.org/index.php/environment_variables"
echo
```

test test2
