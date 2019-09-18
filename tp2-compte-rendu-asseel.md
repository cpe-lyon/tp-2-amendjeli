# Compte Rendu TP2 Asseel Mendjeli

## Variable d'environnement

### Exercice 1


#### 1) Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?

* Les commandes utilisées par l'utilisateur sont touts située dans les chemins contenu dans la variable **$PATH**

    >`printenv PATH`

    >/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

#### 2) Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?

* La variable d'environnement **HOME** et celle qui permet a cd sans argument de nous ramenenr dans notre repertoire personnel

#### 3) Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.

- La variable d'environnement **LANG** est utilisée par les différents programmes pour déterminer la langue des messages à afficher.

- La variable d'environnement **PWD** contient le chemin absolu vers le répertoire courant (permet de savoir où on est dans l'arborescence).

- La variable d'environnement **OLDPWD** contient le chemin absolu vers le répertoire courant précédent (permet de savoir d'où on vient)

- La variable d'environnement **SHELL** indique l'interpréteur shell utilisé par défaut.
 
#### 4) Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.

* > `MY_VAR="abcdef"`

    >`echo $MY_VAR`

    >abcdef

#### 5) Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.

*    >`bash`

    La commance **BASH** elle permet d'ouvrir un nouveau shell.

    >`echo $MY_VAR`
    
 **MY_VAR** n'affiche rien car apres la commance bash la variable etant local et non d'environnement, elle n'est plus accesible.

#### 6) Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.

*   >`export MYVAR`

    >`bash`

    >`echo $MY_VAR`
    
 **MY_VAR** s'affiche car apres la commance bash la variable etant une variable d'environnement, elle reste  accesible.


#### 7) Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.

*   >`NOMS="MENDJELI ROSSE"`

    >`export NOMS`

    >`echo $NOMS`

#### 8) Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.

*   >`echo "bonjour a vous deux, $NOMS !"`

    >bonjour a vous deux, MENDJELI ROSSE !

#### 9) Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ?

* Il n'y a aucune difference.

#### 10) Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)

*   >`echo '$HOME = '"$HOME"`

    >$HOME =/home/asseel


## Programmation Bash

### Exercice 2
#### Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.


* #!/bin/bash

    password="toto"

    #Demande à l'utilisateur un mot de passe

    echo "veuilleuz entrer le mot de pass : "

    read -s passwordentry

    if [[ $password = $passwordentry ]]; then
          echo "mot de passe bon"
    else
            echo "mauvais mot de passe "
    fi


### Exercice 3 : Expressions rationnelles
#### Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :

function is_number()
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ; then
return 1
else
return 0
fi
}
Il affichera un message d’erreur dans le cas contraire.

**Reponse :**
#!/bin/bash

function is_number()
{
        re='^[+-]?[0-9]+([.][0-9]+)?$'
        if ! [[ $1 =~ $re ]] ; then
                return 1
        else
                return 0
        fi
}

is_number $1
if [ $? = 0 ] ; then
        echo "$1 est un nombre réel"
else
        echo "$1 n'est pas un nombre réel"
fi

### Exercice 4 : Contrôle d’utilisateur
#### Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”,où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)

* #!/bin/bash

    name=$1
    file="${0##*/}"
    users=$(cut -d: -f1 /etc/passwd)

    if [ -z "name" ]; then
            echo ”Utilisation : nom_du_script nom_utilisateur”
    else
            EXIST=0
            for user in  $users
            do
                    if [ "$users" = "$name" ]; then
                            echo "ce nom d'utilisateur existe"
                    else
                            echo "ce nom d'utilisateur n'existe pas"
            done
    fi

### Exercice 5 : Factorielle
#### Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).

* #!/bin/bash
    val=1
    result=1

    while [ $val -lt $1 ]
    do
            ((val++))
            result=$(($result*$i))
    done

    echo $result

### Exercice 6 : Le juste prix
#### Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).

*   #!/bin/bash

    randomNumber=$((RANDOM%1000+1))

    while [ true ]
    do
            read -p "Entrez un chiffre entre 1 et 1000: " ans

            if [ -z "$ans" ]; then
                    continue
            else
                    if [[ $ans -gt $randomNumber ]]; then
                            echo "C'est moins !"
                            continue
                    elif [[ $ans -lt $randomNumber ]]; then
                            echo "C'est plus !"
                            continue
                    else
                            echo "Gagné !"
                            break
                    fi
            fi
    done

### Exercice 7. Statistiques
#### 1,2 : Écrivez un script qui prend en paramètres des entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers.


* function is_number()
    {
    re='^[+-]?[0-9]+([.][0-9]+)?$'
            if ! [[ $1 =~ $re ]] ; then
                    return 1
            else
                    return 0
            fi
    }

    max=-100
    min=100
    sum=0

    for var in "$@"
    do
            if [ "$var" -gt "100" ] || [ "$var" -lt "-100" ]; then
                    echo "entrez des nombres réelle entre 100 et -100"
                    exit 1
            fi

            if [ $var -gt $max ]; then
                    let max=$var
            fi

            if [ $var -lt $min ]; then
                    let min=$var
            fi

            let sum=sum+$var
    done

    let moyenne=$total/$#
    echo "max: $max; min: $min; moyenne: $moyenne"



