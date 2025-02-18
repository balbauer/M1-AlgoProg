---
title: Les classes en Rust (suite) : Les arbres
---


## Une classe Ville

Téléchargez et sauvegardez le fichier [villes.txt](villes.txt). Ce fichier contient la liste des 200 plus grandes villes de France dans un ordre aléatoire. Chaque ligne de ce fichier contient les 5 informations suivantes :

* Nom de la ville
* Numéro du département
* Nombre d'habitants
* Superficie (en km²)
* Rang au niveau national (critère : nombre d'habitants)

Voici à quoi ressemblent les deux premières lignes de ce fichier :

~~~
Saint-Priest 69 40944 29.7 155
Versailles 78 85761 26.2 46
~~~

**:**{:.exercise} 

Créez une classe `Ville` ayant 5 champs : chacun de ces champs doit correspondre aux 5 informations concernant une ville comme décrit ci-dessus (nom, numéro de département, population, superficie, rang). Le constructeur prendra en paramètre un tuple, et initialisera les 5 champs avec les 5 premières entrées du tuple.

Ajoutez ensuite les 2 méthodes suivantes à votre classe :

* Une fonction `afficher_nom(self)` qui affiche la valeur du champ *nom*,

* Une fonction `afficher_ville(self)` qui affiche les valeurs des quatre premiers champs.

Testez votre classe en tapant avec le tuple `("Maisons-Alfort", 94, 51091, 5.4, 100).

Vous devez maintenant parcourir le fichier et créer un nouvel objet ville à partir des informations contenues dans chaque ligne du fichier. Pour cela, il vous suffit de copier-coller le code ci-dessous :

~~~python
>>> fichier = open("villes.txt", "r")
>>> for ligne in fichier :
...     liste = ligne.rsplit(" ")
...     ville = Ville(liste)
~~~

La première ligne indique que nous allons ouvrir le fichier `villes.txt` en mode *lecture* (d'où, le `"r"`, faisant référence à *read*). Nous pouvons ensuite parcourir le fichier ligne par ligne. La variable `ligne` contient à chaque itération la ligne que nous sommes en train de lire, vue comme une chaîne de caractères. 

La méthode `rsplit` de la classe `String` nous permet de *couper* une chaîne de caractères en mots. Cette méthode retourne une liste contenant les mots de la ligne. Dans notre cas, la taille de la liste créé est 5 pour chaque ligne. Vérifiez-le.

## Construire un arbre binaire de recherche

Notre but maintenant est d'insérer toutes ces villes dans un arbre binaire de recherche. Les noeuds de l'arbre seront les objets de la classe `Ville`. Dans un premier temps, la valeur qui nous permettra de créer un *ordre*, sera l'attribue rang. Ceci veut dire qu'on va comparer les villes selon leur rang. Par exemple on va dire que `Meulon < Paris`.

Commencez par créer une classe `Noeud` ayant le constructeur suivant :

~~~python
>>> class Noeud:
...    def __init__(self,liste):
...        self.gauche = None
...        self.droite = None
...        self.ville = Ville(liste)
~~~

Cette classe a donc trois attributs (comme attendu pour un ABR) : un fils gauche, un fils droite, et une *valeur* pouvant être comparée (ou ayant des champs pouvant être comparés), qui est ici un objet de type `Ville`.

**:**{:.exercise} 

Dotez votre classe `Noeud` d'une méthode `inserer(self, liste)` qui prendra comme argument une liste (de longueur 5 dans notre cas) et insérera l'objet `Ville` construit à partir de cette liste, dans l'arbre. Vous pouvez utiliser l'algorithme d'insertion vu en cours.

**:**{:.exercise} 

Écrivez une méthode  `afficher_arbre(self)` qui affichera les noms des villes sauvegardées dans l'arbre en suivant un **parcours infixe**.

Testez votre code :

~~~python
>>> fichier = open("villes.txt", "r")
>>> liste = ["Maisons-Alfort", 94, 51091, 5.4, 100]
>>> noeud = Noeud(liste)
>>> for ligne in fichier :
...     liste = ligne.rsplit(" ")
...     noeud.inserer(liste)

fichier.close()
noeud.afficherArbre()
~~~

Vous devez obtenir l'affichage suivant :

~~~
Paris
Marseille
Lyon
Toulouse
Nice
Nantes
Strasbourg
Montpellier
Bordeaux
...
~~~

**:**{:.exercise} 

Modifiez la méthode `afficher_arbre(self)`afin qu'elle affiche le nom, le numéro de département et la population de chaque ville.


~~~
Paris 75 2125851 105.4
Marseille 13 797491 240.6
Lyon 69 445274 47.9
Toulouse 31 390301 118.3
Nice 6 343123 71.9
Nantes 44 270343 65.2
Strasbourg 67 263941 78.3
Montpellier 34 225511 56.9
Bordeaux 33 215374 49.4
...
~~~

**:**{:.exercise} 

Nous avons maintenant besoin d'une méthode qui permet de rechercher une ville dans l'arbre à partir de son rang. Écrivez une méthode `rechercher(self, rang)` qui prend en entrée un entier correspondant au rang de la ville recherchée et qui renvoie l'objet contentant la ville (noeud) en question correspondant. Si l'objet n'est pas trouvé, la méthode doit renvoyer `None`.

Testez votre code :

~~~python
>>> unNoeud = noeud.rechercher(130)
>>> if unNoeud is not None :
...    unNoeud.ville.afficherNom()
... else :
...    print("Non trouvé")
Saint-Brieuc
~~~

**:**{:.exercise} 

Écrivez une méthode `compter_enfants(self)` qui compte le nombre d'enfants (0, 1 ou 2) d'un noeud de l'arbre.

Testez votre code :

~~~python
>>> unNoeud = noeud.rechercher(130)
>>> nombreEnfants = unNoeud.compterEnfants()
>>> print("Nombre d'enfants = ", nombreEnfants)
1
>>> unNoeud = noeud.rechercher(100)
>>> nombreEnfants = unNoeud.compterEnfants()
>>> print("Nombre d'enfants = ", nombreEnfants)
2
>>> unNoeud = noeud.rechercher(200)
>>> nombreEnfants = unNoeud.compterEnfants()
>>> print("Nombre d'enfants = ", nombreEnfants)
0
~~~

**:**{:.exercise} 

Modifiez la construction de votre arbre, afin que la comparaison se fasse maintenant à partir de la superficie d'une ville.

**:**{:.exercise} 

En utilisant cette nouvelle construction, écrivez une méthode `rechercheMax(self)` qui renvoie la ville ayant la plus grande superficie.
