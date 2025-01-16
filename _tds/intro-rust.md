---
title: Introduction à Rust
---

Rust est un langage de programmation multi-paradigme.

Ce mini-tutoriel est une introduction très basique et rapide à la syntaxe et aux règles du langage. Si vous voulez approfondir plus, plein de tutoriels bien faits existent sur le net, n'hésitez pas à les consulter.

Pendant ces TP vous travaillerez sur vos machines. Pour installer rust sur vos machines, vous pouvez lancer ces commandes sur une machine Ubuntu mise à jour (2025).
~~~bash
>>> sudo apt-get install curl
>>> sudo curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
>>> rustup update
~~~
Ensuite il vous suffit de rédiger votre code dans un fichier (par exemple exemple.rs) avec l'extension .rs. Puis pour compiler le fichier on peut utiliser la commande :
~~~bash
>>> rustc exemple.rs -o exemple.bin
~~~
Puis pour exécuter le binaire on lancera
~~~bash
>>> ./exemple.bin
~~~

## Structure générale d'un fichier de code
En rust, lors de l'execution d'un programme, on lance la fonction main(). Ainsi le coeur du code qu'on veut executer sera à l'intérieur de cette fonction. On note que tout ce qui suit '`//`' (sur la même ligne) sera ignoré par le compilateur.
~~~rust
fn main() {
//Mettre son code ici
}
~~~

## Données et variables

Pour pouvoir accéder aux données qu'un programme manipule on fait usage d'un nombre de **variables** de différents types. Une variable apparaît dans un langage de programmation sous son *nom de variable*, mais il ne s'agit de rien d'autre qu'une référence désignant l'adresse mémoire où sont stockées les données.

En Rust, comme dans de nombreux langages de programmation, il existe un nombre de règles simples sur les noms de variables qu'il faut respecter :

* Seules les lettres a -> z, les chiffres 0-9 et le caractère '`_`' sont autorisés.
* Le nom d'une variable doit toujours commencer par une lettre.

Le mot clef '`let`' et le signe '`=`' est utilisé afin d'affecter une valeur à une variable. 

~~~rust
let n = 5;
let message = "Bonjour";
let pi = 3.14;
let b = true;
~~~

Dans le cas où une variable à vocation à être modifié, on rajoutera le mot-clef '`mut`' au moment de sa définition. Anoter qu'on ne peut pas changer le type de la variable, mais seulement sa valeur.

~~~rust
let mut n = 5;
n = 6;
~~~

On peut affecter une valeur à **plusieurs variables simultanément**. 

~~~rust
let (x, y, z) = (1, 2.0, "Bonjour");
~~~

A noter que dans le cas où la variable est inutilisé, on peut la préfixer par un '`_`' pour ne pas avoir d'avertissement de la part du compilateur.

On peut indiquer de quel type est la variable qu'on a déclaré.


Si en Rust il n'est pas nécessaire d'écrire des lignes de code spécifiques pour définir le type des variables avant de pouvoir les utiliser, c'est une bonne habitude d'indiquer le type des variables qu'on utilise au moment de leur création afin de faciliter le deboggage, même si il suffit d'assigner une valeur à un nom de variable pour que celle-ci soit automatiquement créée avec le type qui correspond à la valeur fournie. On dit alors que Rust est un langage à **typage statique**, contrairement aux langages à **typage dynamique** comme c'est le cas de Python. On peut vérifier ceci avec l'opérateur `type`.


~~~rust
let n:i32 = 5;
let _message:&str = "Bonjour";
let _pi:f32 = 3.14;
let b:bool = true;
~~~

* **Fonction** `print!()`. Pour afficher une chaine de caractères, on peut utiliser la fonction `print!()`. Si à l'intérieur de cette chaîne, on veut afficher des valeurs contenus dans des variables on utilisera les caractères '`{`' et '`}`'.

~~~rust
print!("toto");
print!("toto a {n} ans.");
print!("toto a {} ans.", n);
~~~

On remarque que la deuxième méthode (c'est à dire `print!("toto a {} ans.", n);`) est plus robuste que la première (`print!("toto a {n} ans.");`) si l'on doit mettre autre chose qu'un nom de variable en entrée. On notera aussi la variante `println` qui fait automatiquement un passage à la ligne à la fin de l'affichage.
Vous trouverez plus d'informations sur les fonctions d'affichage en Rust ici : [rust-book-fr](https://doc.rust-lang.org/rust-by-example/hello/print.html).

* **Opérations arithmétiques sur les entiers et les flotants**
On peut utiliser les opérations usuels `+-*/` aussi bien sur les entiers que les flotants. A noter que `\` est une division euclidienne dans un contexte discret, et s'interprête comme la division usuelle. On peut également utiliser `%` dans le contexte discret, ce qui correspond au modulo anglo-saxon.

~~~rust
let n = (3 + 20) mod 5;
let f = 6./5. ;
let b = n > 1;
~~~

On peut également transformer des couples d'entiers ou de flotants en booléens avec les opérateurs d'ordre, d'égalité ou d'inégalité : '`<`', '`>`', '`=`', et '`!=`' de la manière usuelle. 

**:**{:.exercise}

 Soient deux points de l'espace $$A$$ et $$B$$.  Déclarez 4 variables $$x_A$$, $$x_B$$, $$y_A$$ et $$y_B$$ correspondant aux coordonnées réelles de ces deux points et affectez-leur des valeurs. Calculez la distance entre $$A$$ et $$B$$, donnée par la formule $$\sqrt{(x_B - x_A)^2 + (y_B - y_A)^2}$$, et affichez le résultat à l'écran sous la forme *La distance entre $$A$$ et $$B$$ est : .*

**Attention !** Vous devez utiliser pour cet exercice la méthode `variable.sqrt()` qui permet de calculer la racine carré de la variable `variable`.

## Contrôle du flux d'exécution 

Dans la plupart des programmes que vous allez écrire vous aurez besoin d'utiliser des instructions qui permettront au programme de suivre des chemins différents selon les circonstances. Pour ceci il est nécessaire de disposer d'instructions capables de *tester une certaine condition* et de modifier le comportement du programme en conséquence.

L'instruction qui est sans doute la plus utile afin de permettre un tel comportement est l'instruction `if`. Son fonctionnement sous Rust est très simple. Si la condition à droite du mot-clef '`if`' est vraie, alors le bloc d'instructions entre les accolades '`{`', et '`}`' qui suit la condition sera exécuté. Sinon, ce sera le bloc entre accolade après le `else` qui sera executé.

~~~rust
if age > 18 {
        print! ("Tu peux entrer.")
    } else {
        print!("Tu es trop petit.")
    }
~~~

A noter que lorsqu'on veut remettre un `if else` comme bloc pour un `else`, on ne met pas d'accolade

~~~rust
if age > 18 {
        print!("Tu peux entrer.")
    } else if a < b {
        print!("Tu es trop petit.")
    } else {print!("Tu as tout juste l'âge.")}
}
~~~

**:**{:.exercise}

Déclarez une variable et affectez lui un entier naturel. Testez en utilisant les instructions `if` et `else` si l'entier est pair ou impair et affichez le résultat.

**:**{:.exercise}

Déclarez trois variables `a`, `b` et `c` correspondant aux coefficients de l'équation quadratique $$ax^2 + bx + c = 0$$. Calculez et affichez le discriminant $$\Delta = b^2 - 4ac$$ de l'équation. Testez en utilisant les instructions '`if`',  et '`else`' la valeur du déterminant et calculez la ou les solutions de l'équation. Si le déterminant est négatif, affichez le message "*L'équation n'a pas de solutions*".

## Boucles

Très souvent dans nos programmes nous avons besoin de répéter un certain nombre d'instructions plusieurs fois. Python, comme probablement tous les autres langages que vous connaissez, possède dans ce but les instructions '`while`' et '`for`'.

* La boucle `while` permet d'itérer un bloc d'instructions tant qu'une condition reste vraie.  

~~~rust
let mut n = 0 ;
while n < 101 {
        n = n +1 ;
        print! ("{n}\n")
    }
~~~


La boucle `for` est très utile lorsque on veut répéter un bloc d'instructions un nombre de fois connu à l'avance. Si on veut par exemple imprimer tous les nombres de 0 à 10, voici comment on peut le faire à l'aide de l'instruction `for` et de la de la fonction `range`.

~~~rust
for i in 0..10 {
    print!("{i}\n");
}
~~~

Comme on va le voir un peu plus tard, la boucle `for` peut être utilisée très facilement pour parcourir de différentes structures de données.

**:**{:.exercise} 

Initialisez deux entiers : `a = 0` et `b = 15`.
Écrivez une boucle qui affiche et incrémente de 1 la valeur de `a` tant qu’elle reste inférieure
à celle de `b`.

Écrivez ensuite une autre boucle qui décrémente la valeur de `b` et affiche sa valeur seulement si elle est
impaire. Itérez tant que `b` est supérieur à 0.

**:**{:.exercise}

Affichez la somme des cubes de tous les multiples de 3 compris entre 0 et 99 inclus. Utilisez pour cela l'instruction `for`.

## Fonctions

Pour créer une fonction en Rust, on va sortir de la fonction '`main`' et tout simplement créer une nouvelle fonction analogue à la fonction mais qui aura un autre nom. 
Voici la nouvelle structure du code globale.

~~~rust
fn main() {
    fonction_qui_ne_fait_pas_grand_chose(); 
}

fn fonction_qui_ne_fait_pas_grand_chose() {
 print!("La fonction a été appelée!");
}
~~~

On indiquera la liste des paramètres entre parenthèses, en indiquant à chaque fois le type de l'argument après un deux-points '`:`' qui suit le nom de l'argument. Le corps de la fonction commence à la ligne suivante et doit être écrit avec un retrait de quelques espaces.
Voici une fonction qui imprime les `n` premiers termes  de la suite *Fibonacci*.

~~~rust
fn fibonacci(n:i32) {
 let mut fin:i32 = 1 ;
 let mut finplusun:i32 = 1;
 for _i in 1..n {
        print! ("{fin}\n");
        let a:i32 = finplusun;
        finplusun = fin + finplusun;
        fin =a;
 }
}
~~~

On peut bien sûr écrire une fonction qui nous renvoie quelque chose. Dans ce cas, il faut écrire quel est le type renvoyé attendu avec le mot-clef '``'. Ceci se fait avec le mot-clé `return`.
Voici une fonction qui renvoie la somme des carrés des entiers de 0 à `n`.

~~~rust
fn somme_carres(n:i32)->i32{
 let mut total:i32 = 0 ;
 for i in 1..n {
        total = total + i*i;
 }
 total
}
~~~

Vous trouverez plus d'informations sur les fonctions en Rust ici : [rust-book-fr](https://jimskapt.github.io/rust-book-fr/ch03-03-how-functions-work.html).

**:**{:.exercise}

Écrivez une fonction `table_de_multiplication(base, fin)` qui prend en paramètre un entier `base` et un entier `fin`
et affiche à l'écran les `fin` premiers éléments de la  table de multiplication de l'entier `base`.

Par exemple `table_de_multiplication(3, 10)` doit retourner `0 3 6 9 12 15 18 21 24 27`.

## Références

Par défaut une fonction ne modifiera pas une variable (même si elle est censée être modifiable avec le mot-clef '`mut`'), car cette dernière copie la donnée prise en entrée, et travaille sur la copie de la donnée sans modifier l'original. On peut tester la fonction suivante :
~~~rust
let mut n = 5;
fn succ(n:i32){
 n= n+1;
}
incr(n);
print!("{}",n);
~~~
On va donc devoir utiliser une stratégie similaire à celle des pointeurs dans le langage C. Si Rust a une gestion des adresses, et des pointeurs similaires à celle de C. Dans ce cours, on priviligiera l'utilisation des références. L'idée est la suivante, `n` n'est plus vraiment une variable, mais une case-mémoire qui contient un entier modifiable de 64 bits (initialisé à `5`). Le type de cette case est `&mut i32` (comprendre case contenant un entier modifiable de 32 bits). Et pour le définir, on utilisera l'expression '`&mut`' (qui joue le même rôle que le '`ref`' en ocaml).

~~~rust
let n : &mut i32= &mut 5;
~~~

Pour accéder à la valeur de la case mémoire (que ça soit pour lire la valeur ou réécrire dessus), on utilisera '`*n`' (Similaire au langage C, et analogue au '`!`' du Ocaml).
~~~rust
let ref mut n = 5;
*n= *n+1;
print!("{}",*n);
~~~

A présent, on peut utiliser une fonction qui va prendre en argument la case mémoire, en faisant attention que le type passé en argument change (cela devient le type de la case mémoire, c'est à dire `&mut type_dela_donnee_dans_la_case`).

~~~rust
let ref mut n = 5;
fn succ(n:&mut i32){
 *n= *n+1;
}
succ(n);
print!("{}",*n);
~~~

## Tableaux

Les tableaux sont des structures de données où l'accès à une donnée (pour peu qu'on connaisse son adresse) est peu coûteuse, mais dont la taille est définie à la création, et n'a pas vocation à changer.
On peut les créer en les déclarant explicitement :
~~~rust
let nombres = [2,5,13,-35,0] ;
~~~
Ou alors en donnant la taille, et une valeur par défaut avec la syntaxe `[expr; N]`.
~~~rust
let tableau_toto = ["toto";100] ;
~~~
On peut accéder au contenu d'une case grâce aux crochets.
~~~rust
let tableau_toto = ["toto";100] ;
let u = tableau_toto[99];
~~~

On peut retrouver la taille d'un tableau avec l'atribut `len`.
~~~rust
let tableau :[&str; 100] = ["toto";100] ;
let taille = tableau.len() ;
~~~

Et si le tableau est modifiable (avec le mot-clef `mut`), on peut modifier les cases de ce tableau (mais pas sa taille qui est dépendante du type) :
~~~rust
let mut tableau_toto :[&str; 100] = ["toto";100] ;
tableau_toto[99]= "joujou" ;
~~~


Si on veut le modifier à l'intérieur d'une fonction, il faut aussi utiliser des références, à noter que lorsqu'on accède à une case (aussi bien pour la lecture que l'écriture), on a pas besoin d'utiliser le caractère '`*`' :
~~~rust
let mut tableau_toto :&mut [&str; 100] = &mut ["toto";100] ;
tableau_toto[99]= "joujou" ;
~~~


**:**{:.exercise}

Définissez le tableau `tableau = [34, 0, -17, 5, 18, 9]`, puis triez et affichez le tableau.

**:**{:.exercise}

Modifier la fonction `fibonacci(n)` de la Section 5 afin qu'elle renvoie un tableau avec les `n` premiers termes de la suite Fibonacci.

# Exercices

Vous êtes prêts maintenant à écrire par vous mêmes des programmes un peu plus longs et compliqués.

## Crible d'Ératosthène

Le crible d'Ératosthène est un algorithme qui permet de trouver tous les nombres premiers qui sont inférieurs à un certain entier naturel $$N$$. Cet algorithme est dû au mathématicien grec Ératosthène de Cyrène qui est également connu pour être la première personne à avoir mesuré le méridien terrestre.

L'idée du crible est très simple. On commence par écrire la liste de tous les nombres de $$2$$ jusqu'à $$N$$. Ensuite on barre (on enlève de la liste) tous les multiples de $$2$$. On note ensuite le plus petit nombre non-barré de la liste, qui est donc le nombre 3, et on procède de façon similaire en enlevant tous ses multiples. On continue de la même façon jusqu'à atteindre le nombre $$N$$. Les nombres qui restent à la fin  sont exactement les nombres premiers plus petits ou égaux à $$N$$.

Vous pouvez voir une jolie animation de l'exécution de l'algorithme sur la page <https://fr.wikipedia.org/wiki/Crible d'Ératosthène>.

**Remarque**. En réalité, il suffit de tester uniquement les multiples des nombres de 2 à $$\sqrt{N}$$, puisque un nombre composé plus petit ou égal à $$N$$, a forcément un facteur plus petit ou égal à $$\sqrt{N}$$.

Vous devez maintenant programmer le crible d'Ératosthène en  Python. Écrivez une fonction `eratosthene(N)` qui prend comme paramètre un entier naturel $$N$$ et qui affiche à l'écran la liste de tous les nombres premiers plus petits ou égaux à $$N$$. Il existe plusieurs façons de coder cet algorithme en Python, vous êtes libres de faire à votre propre guise. 

## Recherche dichotomique

La recherche dichotomique est un algorithme très simple et efficace pour rechercher un élément dans un tableau trié. 

Imaginez par exemple que nous souhaitons retrouver le numéro de téléphone d'une personne dans un annuaire qui est trié par ordre alphabétique. La recherche séquentielle, c.-à-d. parcourir l'annuaire du début en comparant tous les noms avec celui dont on cherche le numéro de téléphone peut être très longue (surtout si le nom recherché se trouve à la fin de l'annuaire). Une approche bien plus efficace est d'ouvrir l'annuaire au milieu et commencer par regarder si le nom se trouve à cette page. Si ce n'est pas le cas, et si le nom dont on cherche se trouve plus loin, alors on recommence la recherche avec la deuxième moitié de l'annuaire. Si le nom se trouve avant, alors on recommence avec la première moitié. 

On peut voir qu'avec cette approche, on réduit à chaque étape la taille de l'annuaire à parcourir de la moitié. Cet algorithme fait partie alors des algorithmes dits *diviser pour régner* et a une complexité *logarithmique* en la taille de la tableau.

Pour cet exercice, on suppose que l'utilisateur possède une tableau croissante de nombres et on lui fournit un nombre qu'on suppose être dans la tableau. Le but est de retourner l'indice du nombre recherché dans la tableau.

Si la tableau fourni est `[1,3,4,6,10,14,15] et l'élément qu'on cherche est 10, alors le programme doit retourner 4 (souvenez-vous que, dans une tableau, les indices sont numérotés à partir de 0).

Écrivez une fonction `recherche_dichotomique(valeur, tableautrie)` qui prend en entrée une tableau trié de nombres et une valeur à rechercher dans la tableau et renvoie l'indice de la tableau correspondant à cette valeur.
