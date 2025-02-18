---
title: Introduction à Rust
---

## Le type Option

Dans de nombreux langages de programmation, on a envie dans certains contexte d'ajouter la valeur `null` ou `None` qui représente l'absence de valeur, à un type déjà existant. En particulier dans un langage fortement typé comme Rust, il faut créer un nouveau type pour gérer ces situations. C'est le type option qui joue ce rôle. Paramétré par un type quelconque `type`, il peut-être vu comme un type somme contenant le type constant `None`, et le type `Some(type)`.
Pour extraire le contenu à l'intérieur de `Some`, on utilise `unwrap()` de la manière suivante :

~~~rust
let t = Some(3);
let u = t.unwrap();
~~~

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

On va créer une classe `Ville` ayant 5 champs : chacun de ces champs doit correspondre aux 5 informations concernant une ville comme décrit ci-dessus (nom, numéro de département, population, superficie, rang). Le constructeur prendra en paramètre un tuple, et initialisera les 5 champs avec les 5 premières entrées du tuple.


~~~rust
use std::io::{self};
use std::str::FromStr;
use std::fs;



#[derive(Copy, Clone)]
struct Ville <'a>{
    nom: &'a str,
    dep: i32,
    popu: i32,
    supfc: f32,
    rang:i32
}


fn main()-> io::Result<()>  {
    let contenu = fs::read_to_string("villes.txt").expect("Quelque chose s'est mal passé lors de la lecture du fichier");
    let mut parts = contenu.split('\n');
    let mut tableau_ville: [Ville;199] = [const { Ville{
    nom: "Zombiniville",
    dep: 3,
    popu: 23,
    supfc: 12.,
    rang:28} };199];
    for i in 0..199 {
    let mut ligne =((parts.next()).unwrap()).split_whitespace();
    tableau_ville[i] = convert_to_ville(ligne.next(), ligne.next(), ligne.next(), ligne.next(), ligne.next());
    }
    Ok(())
}


fn convert_to_ville<'a>(a:Option<&'a str>,b:Option<&'a str>,c:Option<&'a str>,d:Option<&'a str>,e:Option<&'a str>)-> Ville<'a>{
let (Some(nomm), Some(depp), Some(popuu), Some(supfci), Some(rangg))=(a,b,c,d,e)else { panic!("erreur") };
let val = Ville{nom :nomm , dep : <i32 as FromStr>::from_str(depp).unwrap(), popu : <i32 as FromStr>::from_str(popuu).unwrap(), supfc : <f32 as FromStr>::from_str(supfci).unwrap(), 
rang : <i32 as FromStr>::from_str(rangg).unwrap(),};
return val
}
~~~

La première ligne du main indique que nous allons ouvrir le fichier `villes.txt` et convertir son contenu en une longue chaîne de caractères. Puis nous chacune les lignes dans la file parts.
Puis pour chacune de ces ligne, on redécoupe la ligne en une plus petite file, et on défile en convertissant le résultat en une ville grâce à la fonction `convert_to_ville` définie plus bas.

## Construire un arbre binaire de recherche

Notre but maintenant est d'insérer toutes ces villes dans un arbre binaire de recherche. Les noeuds de l'arbre seront les objets de la classe `Ville`. Dans un premier temps, la valeur qui nous permettra de créer un *ordre*, sera l'attribue rang. Ceci veut dire qu'on va comparer les villes selon leur rang. Par exemple on va dire que `Meulon < Paris`.

Commencez par créer une classe `Noeud` correspondant aux arbres binaires vus en cours. On n'hésitera pas à se servir du type option pour exprimer qu'un ils peut correspondre à une arbre vide:

Cette classe a donc trois attributs (comme attendu pour un AB) : un fils gauche, un fils droite, et une *valeur* pouvant être comparée (ou ayant des champs pouvant être comparés), qui est ici un objet de type `Ville`.

**:**{:.exercise} 

Dotez votre classe `Noeud` d'une fonction `inserer(self, liste)` qui prendra comme argument une ville et l'insérera dans l'arbre. Vous pouvez utiliser l'algorithme d'insertion vu en cours.

**:**{:.exercise} 

Écrivez une fonction  `afficher_arbre(self)` qui affichera les noms des villes sauvegardées dans l'arbre en suivant un **parcours infixe**.

Testez votre code sur l'arbre auquel on a inséré toutes le villes du document :

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

Modifiez la fonction `afficher_arbre()`afin qu'elle affiche le nom, le numéro de département et la population de chaque ville.


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

Testez votre code.

**:**{:.exercise} 

Écrivez une méthode `compter_enfants(self)` qui compte le nombre d'enfants (0, 1 ou 2) d'un noeud de l'arbre.

Testez votre code.
**:**{:.exercise} 

Modifiez la construction de votre arbre, afin que la comparaison se fasse maintenant à partir de la superficie d'une ville.

**:**{:.exercise} 

En utilisant cette nouvelle construction, écrivez une méthode `recherche_max(self)` qui renvoie la ville ayant la plus grande superficie.
