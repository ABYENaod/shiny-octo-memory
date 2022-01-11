# shiny-octo-memory
Algorithme de recommandation de contenu sur java 

La recherche de film sur cette algorithme peut etre réalisé de deux manières differentes.

Premierement, soit uniquement en effectuant une recherche par titre, en écrivant celui ci dans la zone de texte superieur,
On lance cette recherche avec le bouton situé sur la droite de la premiere ligne droite.

Deuxiemement, en recherchant un film grace a son/ses genre(s), on peut en préciser 3 au maximum, qui aura pour resultat les films correspondants aux genres demandés.
On lance egalement cette recherche avec le bouton situé sur la droite de la seconde ligne.

L'affichage sera réalisé par des boutons qui afficheront, le titre ainsi que sa date de sortie, qui seront situés dans la partie blanche de l'affichage.


PS: 
Initialement présent dans le code fonctionnel;
La recherche de titre est réalisé sans rating,
La recherche par genre est réalisé avec rating,

Nous pourrons alors basculer lors de la présentation pour montrer la diffence de temps de calculs entre le moment ou on utilise le rating pour classer les film et quand on ne l'utilise pas



DESCRIPTION PROGRAMME
Partie SQL:
La base du programme utilisé correspond à l'exemple donné en classe sur la recherche 
des robots. La classe SQL est divisée en 2 méthodes, la recherche par genre et la 
recherche par titre.
Ces 2 méthodes utilisent la même base qui est la connexion à Mariadb grâce au driver 
Maria. Driver qui doit être implémenté dans le dossier registre du projet, ce programme 
suit la construction suivante :
Connexion -> création requête -> envoie requête -> récupération des résultats de la 
requête,
Le tout exécuté grâce à la méthode try.
A chacune de ses étapes, il existe une méthode catch qui va de pair avec le try qui nous 
permet de récupérer l'erreur s’il y en a une sur la partie SQL.
La requête SQL est sensiblement construite pareille dans ses 2 parties, elle revient à 
prendre les titres des films qui sont contenues dans la table movies qui ont leurs genres 
qui est identique au genre demandé, ou au titre, cependant la présence des "%" permet 
de demander si la chaîne de caractères entrée par l'utilisateur se trouve dans le titre ou 
les genre, peut importe où, du film renvoyé, cela permet de faire une recherche plus large 
et moins sensible à l'inexactitude de l'utilisateur, cette technique nous permet aussi de 
contourner un problème de conception de la base de données qui est que la colonne 
genre possède plusieurs informations à l'intérieur d'une seul case du 
type: "genre1|genre2|genre3|etc…".
Nous réalisons ensuite les jointures entre movies et ratings, car nous avons besoin de 
cette dernière pour trier les résultats par ordre décroissant de moyenne de note, ce qui 
correspond à la fin de notre requête. (Il y a une deuxième version sans ratings, qui permet 
l’algo d'être plus rapide dans sa recherche, elle sont présente en commentaire dans la 
partie SQL)
Cette requête et créé sous forme d'un String à l'intérieur de java, On a donc créé une 
variable string qui stock la requête au fur et à mesure de sa construction qui suit 3 grande 
étape :
• Création de la requête jusqu'à la rentrée du premier genre ou du titre.
• Conversion du genre ou du titre pour qu'il rentre au format '%titre/genre%' dans la 
String de la requête.
• Étape intermédiaire seulement pour la recherche par genre : ajout de genre 2 et 
genre 3 si ceux s’ils ne sont pas nuls.
• Finalisation de la requête avec le tri par note ou non selon le choix dans la partie 
SQL.
La dernière étape de cette méthode correspond à récupérer les résultats de la requête, 
afin de le faire, nous utilisons une boucle While qui a pour condition ress.next() qui devient 
0 si il n'y a plus de réponse a donner et 1 autrement, à l'intérieur de cette classe nous 
prenons les résultats et les stockons dans une linkedlist relier au programme de 
l'affichage. On a également limité le nombre de films ajoutés à cette liste.
Partie Affichage
Notre affichage est réalisé sur un grand panneau composé de plusieurs zones, une zone 
secondaire qui est situé sur les bords ouest, sud ainsi qu’est, ce sont les 3 jPanel utilisés 
en BorderLayout dont on a changé la couleur en gris, présents afin de réaliser un 
affichage plus sympa pour le visuel, ainsi que de 2 zones principales, qui sont pour l’une, 
la partie centrale, encore une fois réalisée à l'aide d’un BorderLayout cette fois ci centré, 
qui va nous permettre d’afficher nos résultats sur cette partie avec des jButton, pour 
l’autre encore une fois réalisé à l'aide d’un BorderLayout nord de couleur gris, sera la 
partie qui va permettre à l'utilisateur de réaliser sa recherche soit, uniquement par titre, 
ou alors uniquement par genre. Elles sont disposées dans 2 tableaux insérés dans le 
panneau supérieur. La méthode utilisée aussi bien pour l’un que pour l’autre est similaire. 
Il y a pour chaque recherche possible, une à trois zone(s) de texte (jtextField), suivant si 
on recherche un film par son titre ou par son genre. Il y a également au bout à droite de 
chacun de ces 2 tableaux présent dans la partie haute, un bouton qui va servir de liaison, 
et qui permet au programme de fonctionner en envoyant le contenu des zones de texte 
selon la recherche demandée à la partie SQL. Pour rappel, chaque bouton correspond à 
une recherche distincte. Pour disposer ces objets, on a simplement dû ajouter dans 
l’ordre les objets en partant de la gauche vers la droite.
Pour ce qui est de l’affichage des résultats, nous avons créé une LinkedList afin de 
pouvoir les utiliser dans l’affichage prévu au centre de la fenêtre. Avec l’aide d’une 
méthode, afficheResultat(), et d’une boucle, nous créons à chaque film présent dans cette 
liste un jButton afin de pouvoir l’afficher.
Partie Liaison Bouton
On a mis en place deux écouteursClics qui permettent aux deux boutons de réaliser la 
communication entre la class SQL et la class fenetreSQL et de lancer la recherche de 
film à partir du texte écrit dans la zone approprié à chaque recherche. Ces liaisons sont 
réalisées grâce aux méthodes getters présentes dans la class fenetreSQL, aux méthodes 
RechercheTitre ou RechercheGenre écrites dans la class SQL. Ce qui va permettre à la 
class SQL de remplir la LinkedList avec les films correspondants, cette liste qui va être 
utilisée par la méthode afficheResultat écrite dans la class fenetreSQL, afin de faire 
apparaître les titres qui correspondent à la recherche dans l’affichage sous forme de 
boutons comme expliqué précédemment
