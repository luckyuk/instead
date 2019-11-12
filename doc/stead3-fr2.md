## Informations Générales

Le moteur, au contraire, est écrit en langage Lua (5.1), donc sachant
cette langue est utile, mais pas nécessaire. Le moteur de base écrit dans
lua, de sorte que le Lua de la connaissance peut être utile pour mieux comprendre les principes
de l'opération, bien sûr, si vous êtes intéressé à ce faire.

Au cours de son développement, la PLACE se charge de nouvelles fonctions. Maintenant, vous pouvez faire
jeux de différents genres (à partir de arcade, jeux de texte). Et aussi, au LIEU de vous
peut exécuter des jeux écrits dans d'autres moteurs, mais la Fondation de la PLACE
reste de la carotte d'origine, qui est axé sur la création de textes et de graphiques
jeux d'aventure. Cette documentation décrit le ce des compétences de base est
nécessaire, même si vous voulez écrire quelque chose d'autre... donc, nous allons Commencer à apprendre
Au LIEU de cela par l'écriture d'un simple jeu!

Au LIEU de cela 3.0 a été publié en février 2017, après 8 ans de développement.
La Version 3.0 prend en charge un nouveau moteur d'exécution nommé STEAD3. L'ancien moteur d'exécution est maintenant connu
comme STEAD2. Au LIEU prend en charge les jeux écrite, soit STEAD2 ou STEAD3.

Ce manuel va vous apprendre tout ce que vous devez savoir à propos de l'écriture de jeux pour
STEAD3. D'autres ressources peuvent vous être utiles, tels que la PLACE site web:

https://instead-hub.github.io

Nous avons également une salle de chat sur gitter:

https://gitter.im/instead-hub/instead

### L'histoire de la création

Quand nous disons "le texte de l'aventure" la plupart des gens là-bas l'un des deux familiers
les images. C'est soit du texte, des boutons d'action, par exemple:

 Vous voyez une table en face de vous. Il y a une pomme sur la table. Que faire?

 1) Prendre la pomme
 2) s'éloigner de la table

Ou beaucoup moins, c'est un jeu classique avec une saisie de texte, où le jeu de contrôle a été
nécessaire d'introduire des actions avec le clavier.

 Vous dans la cuisine. Il y a une table.
 > inspecter la table.
 Il y a une pomme sur la table.

Les deux approches ont leurs avantages et leurs inconvénients.

Si nous parlons de la première approche, il est proche de ce genre de livres, de jeux
et plus commode pour les textes littéraires qui décrivent les événements, ce qui est
qui se passe avec le personnage principal, et pas très facile à créer classique quêtes,
où le personnage principal explore modélisé dans l'univers du jeu, en se déplaçant librement
sur elle et l'interaction avec les objets de ce monde.

La deuxième approche, les modèles du monde, mais exige un effort considérable de
le jeu de l'auteur, et, plus important, plus prêt du joueur. Surtout quand on
traitez avec la langue russe.

Au LIEU de cela, le projet a été créé pour en écrire d'autres types de jeux
combiner les avantages des deux approches, tout en essayant d'éviter
leurs inconvénients.

Le monde du jeu, au contraire, est modélisé comme la seconde approche, qui est, dans
le jeu il y a des endroits (scènes ou de chambres) qui peuvent avoir accès aux principales
le héros et les objets avec lesquels il interagit (y compris la vie
caractères). Le joueur est libre d'explorer le monde et de manipuler des objets.
Par ailleurs, les actions avec des objets n'est pas indiqué explicitement les éléments de menu, mais
n'est pas sans rappeler le classique graphique quêtes dans le style des années 90.

En fait, au LIEU de cela il existe de nombreux invisible au premier coup d'œil, les choses qui sont
visant le développement de l'approche choisie, et qui rend le jeu aussi
dynamique et différent de l'habituel "texte des quêtes". Ceci est confirmé par
y compris le fait que le moteur a été libéré beaucoup de grands jeux, le
l'intérêt de montrer non seulement les fans de jeux de mots en tant que tels, mais les gens ne sont pas
familier avec ce genre.

Avant de lire ce guide, je vous recommande de jouer classique à la PLACE de jeux
comprendre ce qui se passait. D'autre part, puisque vous êtes ici, vous
probablement fait.

Cependant, pas la peine d'en examiner le code de ces jeux, parce que les vieux jeux
sont, très souvent, se comportent de façon inefficace, avec des dessins. Version actuelle
Au LIEU de cela vous permet de mettre en œuvre un code plus court, plus simple et plus facile. À propos de cette
et que dit ce document.

Si vous êtes intéressé par l'histoire du moteur, vous pouvez lire
un article sur comment tout a commencé:

https://instead-hub.github.io/article/2010-05-09-history/

### Look et la sensation d'un type à la PLACE de jeu

Donc, Qu'est typique de la PLACE de jeu ressemble?

_Main jeu window_ contiennent un récit, des informations sur la statique et
les parties dynamiques de la scène, active et une image de la scène (dans le
graphique interprète) avec des transitions possibles vers d'autres lieux.

_Descriptive partie de la scene_ apparaît qu'une seule fois, lors de l'affichage de la scène, ou explicite
l'inspection de la scène (dans le graphique interprète -- Staticheskaya partie
la scène contient des informations sur la statique des objets de la scène (généralement des paysages) et est
toujours affiché. Cette pièce écrite par l'auteur du jeu.

_Dynamic partie de la scene_ est composé de descriptions d'objets dans la scène,
qui sont présents en ce moment sur scène. Cette partie est générée automatiquement
et affiche toujours. Habituellement, il présente des objets qui peuvent changer sa
emplacement.

Le joueur est disponible sur les fonctionnalités disponibles sur toute la scène-de l'Inventaire. L'
le joueur peut interagir avec les objets de l'inventaire et à la loi sur les objets de
l'inventaire sur d'autres objets dans la scène ou de l'inventaire.

> Il est à noter que le "stock" est un conventionalism. Par exemple,
> l'équipement peut être de tels objets comme "ouvert", "examiner", "utiliser", etc.

_Actions_ du joueur peut être:

- l'inspection de la scène;
- l'effet sur l'objet de la scène;
- l'effet sur l'inventaire de l'objet;
- l'action objet de l'inventaire sur l'inventaire de l'objet;
- le déroulement de la scène d'objets;
- passer à une autre scène.

### La façon de démarrer un nouveau projet de jeu

Au LIEU de cela permettra de traiter n'importe quel répertoire sur votre ordinateur comme un projet de jeu si il
contient un fichier texte nommé "main3.lua". La présence de ce fichier signifie que
c'est un STEAD3 projet. Tous les autres fichiers dont vous avez besoin pour votre jeu, comme par exemple
Lua scripts, les images et la musique doivent être stockés dans le répertoire du jeu comme
bien. Tout moment vous faites référence à une ressource externe dans votre code, il devrait être
par rapport au haut-niveau répertoire du jeu.

Le "main3.lua" fichier doit commencer par un bloc de commentaire contenant une liste de balises. Les balises sont des paires de noms et de chaînes de caractères qui fournissent de l'information au sujet de votre jeu à la PLACE.

Toute séquence de caractères commençant par un double tiret sur la même ligne est un Lua commentaire. Au LIEU de cela, les balises sont des lignes de commentaires de la forme suivante:

 -- $TagName: Une chaîne de caractères UTF-8$

$Name de la balise contient le nom du jeu. Voici un exemple:

 -- $Nom: le jeu le Plus intéressant!$

C'est une bonne pratique à suivre le Nom de la balise avec quelques autres. Voici comment spécifier le jeu de numéro de version:

 -- $Version: 0.5$

Les gens qui jouent de votre jeu peut être intéressé par qui il a écrit:

 -- $Auteur: Anonyme fan de texte aventures$

Il est souvent utile d'inclure une description de votre jeu. Nous donnons ici
un exemple de multi-ligne de la balise. Vous spécifiez les sauts de ligne à l'aide de la "\n" échapper
séquence:

 -- $Info: C'est un remake d'un classique\nom nzx Spectre de jeu.$

Si vous êtes un utilisateur de Windows, assurez-vous que votre éditeur de texte, vous pouvez enregistrer les fichiers
codé en UTF-8 _without un BOM (byte order marqueur)_.

Après le préambule contenant vos mots-clés, vous devriez liste tous les modules externes
requis par le jeu. Voici un exemple de ce à quoi cela pourrait ressembler. Nous allons vous expliquer ce que ces lignes de dire plus tard:

 exiger "fmt" -- certaines fonctions de mise en forme
 fmt.para = true -- enable les paragraphes (retraits)

Après cela, il est généralement utile de définir les gestionnaires par défaut. Nous n'avons pas
couvert ce que les gestionnaires sont pourtant, il est donc très bien si vous ne savez pas exactement ce que
ces lignes. Voici comment vous définissez la valeur par défaut "loi", "utiliser", et
"inv" gestionnaires:

 jeu.act = 'ne fonctionne Pas.';
 jeu.utilisation = 'Ça n'aide pas.';
 jeu.inv = 'Pourquoi?';

Quand le jeu commence, il va mettre le jeu initial de l'état par l'appel de la "init"
fonction. Vous pouvez utiliser cette fonction pour initialiser le joueur ou d'accomplir un
spécial à partir de tâches. La fonction peut ne pas être nécessaire en fonction de votre jeu.

 la fonction init() -- Mis le couteau à papier et de papier dans l'inventaire du joueur
 prendre "le couteau"
 prendre "papier"
fin

Le moteur de jeu, seuls les appels de la fonction init() lorsqu'un nouveau jeu est lancé. Il ne sera pas
appelé lorsque vous chargez un jeu qui a été précédemment enregistré. Pour effectuer un peu de
actions lorsque le jeu est chargé, utilisez la fonction start ().

``
la fonction start(is_loaded) -- pour restaurer l'état d'origine?
 si is_loaded alors
 dprint "Jeu chargé."
d'autre
 dprint "Nouveau jeu a commencé."
fin
 -- nous n'avons pas besoin de faire quelque chose
fin
``

Si vous comporter à la fois une fonction init() _et_ une fonction start (), puis les
moteur de jeu va appeler init() tout d'abord, et ensuite appeler la méthode start().

Le graphique interprète cherche jeux disponibles dans le répertoire de jeux. L'
La version Unix de l'interprète en outre, le catalogue des scans aussi des jeux en
le répertoire ~/.au lieu de cela/de jeux. Windows version: Documents et
Settings/USER/Local Settings/Application Data/plutôt/jeux. Dans Windows
autonome-Unix-version du jeu sont recherchés dans le répertoire
./appdata/jeux, si elle existe.

Dans certaines assemblées, à la PLACE (dans Windows, sous Linux si le projet est construit avec
gtk, etc.), vous pouvez ouvrir le jeu de toute façon à partir du menu "Choix de
jeux".Ou, appuyez sur f4. Si dans le répertoire du jeu, il y est un seul de la
jeu, au LIEU de cela, il sera lancé automatiquement, ce qui est pratique si vous voulez
distribuer votre jeu avec le moteur.

Si vous mettez le jeu dans votre répertoire et exécuter à la PLACE.

__Important!__

Lors de l'écriture de jeux, il est fortement recommandé d'utiliser l'indentation pour le codage
des jeux, comme dans l'exemple de la direction, ce qui vous permettra de réduire le
nombre d'erreurs et de rendre votre code graphiquement!

Ci-dessous est un minimum de modèle pour votre premier jeu:

``
-- $Nom: Mon premier jeu$
-- $Version: 0.1$
-- $Auteur: Anonyme$

exiger "fmt"
fmt.para = true

jeu.act = 'Hm...';
jeu.utilisation = " ne fonctionne Pas.';
jeu.inv = 'Pourquoi moi?';

la fonction init()
 -- initialisation si c'est nécessaire
fin
``

### Les bases de débogage

Lors du débogage (vérifier la santé de votre jeu) il est commode de
Au LIEU de cela a commencé avec option de débogage, puis, en cas d'erreur
affiche des informations plus détaillées sur le problème sous la forme d'une pile
les appels. L'option de débogage peut être réglé dans le raccourci (si vous travaillez en
Windows) et pour les autres systèmes, je pense que vous savez déjà comment passer
options de ligne de commande.

En outre, le mode de débogage, le débogueur se connecte automatiquement. Vous pouvez
l'activer avec la touche ctrl-d ou f7. Vous pouvez connecter le débogueur et explicitement:

 exiger "dbg"

Dans le code de votre jeu.

Lorsque vous déboguez un jeu, vous avez généralement besoin d'enregistrer fréquemment le jeu et charger
l'état du jeu. Vous pouvez utiliser le mécanisme standard d'enregistrement via le menu
(ou via les touches F2/F3), ou utilisez la barre de sauvegarde/chargement (appuyez sur F8/F9).

En Mode 'debug', vous pouvez redémarrer le jeu avec les touches [ALT]+R. En combinaison avec la touche F8/F9
cela vous permet de voir rapidement les modifications apportées au jeu après il change.

> Attention! Au LIEU de la fonction d'enregistrement automatique est utile lors de la lecture des jeux, mais peut
> obtenir de la manière quand vous êtes dans le milieu de développement. Lors du redémarrage de
> Au LIEU de cela, le comportement par défaut est de reprendre là où vous l'avez laissée par le rechargement
> le jeu précédent de l'état. Vous pouvez désactiver cette fonctionnalité dans le menu
> paramètres sous "enregistrement automatique". Vous pouvez également explicitement réinitialiser le jeu à chaque fois
> vous modifier son code source. Alors que dans le mode de débogage (en commençant par le -debug
> drapeau comme décrit ci-dessus), vous pouvez appuyer sur [ALT]+R ou sélectionnez "démarrer" à partir d'
> le menu.

En Mode 'debug' version de Windows au LIEU de cela crée une fenêtre de console (sous Unix
version, si vous commencez PLUTÔT à partir de la console, la sortie bedirected à
c') qui sera mis en œuvre par la sortie d'erreur. En outre, à l'aide de la
la fonction " print ()", vous pouvez générer vos messages avec la sortie de débogage. Pour
exemple:

``
act = fonction(s)
 print ("la Loi est ici! ");
...
fin;
``

Ne vous inquiétez pas si vous lisez tous les conseils et commencer la rédaction de votre jeu,
vous aurez plus de chances de prendre un coup d'oeil à cet exemple avec un grand enthousiasme.

Vous pouvez également utiliser la fonction dprint(), qui envoie la sortie dans le débogueur
de la fenêtre, et vous pouvez voir lorsque vous êtes connecté en mode debug.

``
act = fonction(s)
 dprint ("la Loi est ici! ");
...
fin;
``

Lors du débogage, il est utile d'examiner le fichier de sauvegarde, qui contiennent les
les variables d'état du jeu. Ne pas chercher à chaque fois les fichiers de sauvegarde, de créer un
enregistre répertoire dans le répertoire de votre jeu ( le répertoire qui contient
main3.lua) et le jeu va persister sauve. Ce mécanisme sera également utile
pour le transfert de jeux à d'autres ordinateurs.

Il est possible (surtout si vous utilisez des systèmes Unix), vous aimez l'idée
de vérifier la syntaxe de votre script par le compilateur "luac". Dans Windows
il est également possible, vous avez seulement besoin d'installer d'exécuter le fichier lua pour Windows
(http://luabinaries.sourceforge.net)/ et de l'utilisation luac52.exe.

Vous pouvez vérifier la syntaxe et de l'utiliser à la PLACE, pour ce faire, utilisez le paramètre-luac:

 sdl-au lieu-debug-luac <chemin d'accès au script.>

## Scène

Scène (ou chambre), est une unité de jeu dans lequel le joueur peut examiner tous les
les objets de la scène et d'interagir avec eux. Par exemple, la scène peut être un
la salle dans laquelle le héros est. Ou de la parcelle de forêt disponible pour l'observation.

Dans tout jeu devrait être une scène appelée "main". Il démarre et votre jeu!

``
chambre {
 nam = "principal";
 disp = "pièce Principale";
 dsc = [[Vous êtes dans une grande salle.]];
}
``

L'enregistrement des moyens de création d'un objet (comme presque toutes les entités. les objets) principale
type de pièce (chambre). L'attribut de l'objet stocke le nom de nam salle "principale", qui
vous pouvez accéder à la salle à partir de votre code. Chaque objet a son propre nom. Si vous
essayez de créer deux objets avec le même nom, vous recevrez un message d'erreur
message.

Pour accéder à l'objet par son nom, vous pouvez utiliser l'entrée suivante:

 dprint("Objet: ", _'principal')

Chaque objet a _attributes_ et _event handlers_. Dans cet exemple, a deux
attributs: nam et dsc. Les attributs sont séparés le délimiteur (dans ce
exemple-un symbole, le point-virgule ';').

Généralement, les attributs peuvent être des chaînes de texte, les fonctions des gestionnaires et Boolean
des valeurs. Toutefois, l'attribut nam devrait toujours être une chaîne de texte, si
spécifié.

En fait, vous ne pouvez pas spécifier le nom lors de la création de l'objet:

``
chambre {
 disp = "pièce Principale";
 dsc = [[Vous êtes dans une grande salle.]];
}
``

Dans ce cas, le moteur lui-même donnera le nom de l'objet, et ce
le nom est un type de numéro. Parce que vous ne connaissez pas ce numéro, vous pouvez
contacter l'objet clairement. Il est parfois commode de créer sans nom
les objets, par exemple, pour la décoration. Lorsque l'objet est créé, même si il
est anonyme, vous ne parvenez pas à créer une variable de l'objet de référence, par exemple:

``
myroom = chambre {
 disp = "Placard";
 dsc = [[Vous dans le placard.]];
}
``

Myroom variable dans ce cas devient un synonyme de l'objet (lien sur l'objet
lui-même).

 dprint("Objet: ", myroom)

Vous pouvez vous en tenir à une méthode ou d'appliquer à la fois. Par exemple, vous pouvez spécifier
le nom et la variable lien:

``
main_room = chambre {
 nam = "principal";
 disp = "pièce Principale";
 dsc = [[Vous êtes dans une grande salle.]];
}
``

Il est important de comprendre que le moteur est en tout cas de travail avec
les noms d'objet, les variables sont juste des références-c'est juste une façon de simplifier
l'accès pour les objets fréquemment utilisés. Donc, pour notre premier match, nous nous devons de préciser
l'attribut nam = 'main' pour créer la pièce principale et nous commençons notre aventure!

Dans notre exemple, lors de l'affichage de la scène, le titre de la scène sera utilisée la
l'attribut 'disp'. En fait, si nous n'avions pas demandé c'est dans le titre nous voir
'nam'. Mais nam n'est pas toujours commode de faire le titre de la scène,
surtout si c'est une chaîne comme "principal" ou si c'IDENTIFIANT numérique que le moteur
affecté à l'objet automatiquement.

Il est plus intuitif de l'attribut "title". Si elle est définie, lorsque l'affichage
chambre comme le titre l'indique il. le titre est utilisé lorsque le joueur est à l'intérieur
l'une des chambres. Dans tous les autres cas (lors de l'affichage des transitions dans cette salle)
utiliser le 'disp' ou 'nam'.

``
mroom = chambre {
 nam = "principal";
 title = 'Début de l'aventure';
 disp = "pièce Principale";
 dsc = [[Vous êtes dans une grande salle.]];
}
``

L'attribut " asn " est la description de la scène qui s'affiche une fois quand
entrer en scène, ou lorsque explicite de l'examen de la scène. Il n'y a pas
les descriptions des objets présents dans la scène.

Vous pouvez utiliser le symbole ',' au lieu de ';' pour séparer les attributs. Par exemple:

``
chambre {
 nam = "main",
 disp = 'pièce Principale',
 dsc = 'Vous êtes dans une grande salle.',
}
``

Dans cet exemple, tous les attributs -- une chaîne de caractères. La chaîne peut être écrit dans
des guillemets simples ou doubles:

``
chambre {
 nam = "principal";
 disp = 'pièce Principale';
 dsc = "Vous êtes dans une grande salle.";
}
``

Pour les longues descriptions, il est commode d'utiliser les éléments suivants:

 dsc = [[ Très longue description... ]];

Les retours à la ligne sont ignorés. Si vous voulez à la sortie de la description de l'
la scène a été assisté par les paragraphes -- utiliser le '^'symbole.

``
dsc = [[, Premier alinéa. ^^
Le Deuxième Paragraphe.^^

Troisième paragraphe.^
Sur une nouvelle ligne.]];
``

Je vous recommande de toujours utiliser la [[ et ]] de 'dsc.

Permettez-moi de vous rappeler à nouveau que le nom 'nam' de l'objet et de l'afficher (dans ce
le cas de la façon dont la scène va ressembler pour le joueur dans la forme de
lettrage en haut), vous pouvez (et souvent en faut!) part. Pour cela, il ya
les attributs 'disp' et 'title'. 'titre' est seulement dans les chambres et travaille comme poignée
lorsque le joueur est à l'intérieur de cette chambre. Dans d'autres cas, utilisez la fonction "disp" (si elle existe).

Si "disp" et "titre" n'est pas spécifié, il est considéré que l'affichage
est égal nom.

'disp' et 'title' peut être définie sur false dans ce cas, l'affichage ne sera pas.

## Objets

_Objects_ -- ce une scène de l'interaction avec le joueur.

``
obj {
 nam = 'table';
 dsc = 'Dans le {table}.';
 act = 'Hm... Juste une table...';
};
``

Nom de l'objet "nam" est utilisé quand il arrive à votre inventaire. Même si dans notre cas,
le tableau est à peine là. Si l'objet est défini 'disp', quand il entre dans
l'inventaire de l'affichage, il rendra l'utilisation de cet attribut. Par exemple:

``
obj {
 nam = 'table';
 disp = 'table d'angle;
 dsc = 'Dans le {table}.';
 tak = 'j'ai pris le coin de la table';
 inv = 'je tiens le coin de la table.';
};
``

Encore, le tableau est venu à nous dans l'inventaire.

Vous pouvez cacher des objets dans l'inventaire, si 'disp' l'attribut est "faux".

'asn' -- description de l'objet. Il sera affiché dans la dynamique
une partie de la scène dans la présence de l'objet dans la scène. Les accolades affiche
un fragment du texte à un lien dans la fenêtre à la PLACE. Si les objets de la
scène beaucoup, toutes les descriptions sont affichés l'un après l'autre, à l'aide de la
la barre d'espace

"loi" est un gestionnaire d'événement qui est appelée lorsque l'action de l'utilisateur (action sur
objet dans la scène, en général -- cliquez sur la souris sur le lien). Ses principaux
la tâche de la sortie (retour) de la ligne de texte, qui fera partie de la scène des événements,
et le changement d'état du monde de jeu.

## Ajout d'objets à la scène

Il y a plusieurs façons pour vous, l'auteur, pour remplir une scène avec des objets.

Tout d'abord, lorsqu'une pièce est créée, vous pouvez affecter une liste contenant les noms de
les objets de la chambre 'obj' attribut:

``
obj { -- un objet avec un nom, mais sans une variable
 nam = "boîte";
 dsc = [[je vois un {box} sur le sol.]];
 act = [[Dur!]];
}

chambre {
 nam = "principal";
 disp = "Big room";
 dsc = [[Vous êtes dans une grande salle.]];
 obj = {case''};
};
``

Maintenant, lors du rendu de la scène, nous allons voir que l'objet "boîte", une partie dynamique.

Plutôt que de se référer à la boîte en son nom, nous aurions pu utiliser une variable
de référence. Ceci est possible tant que l'objet est déclaré plus tôt dans votre
code source:

``
apple = obj { -- variable objet, mais sans nom
 dsc = [[il y a {Apple}.]];
 act = [[Rouge!!]];
}

chambre {
 nam = "principal";
 disp = "Big room";
 dsc = [[Vous êtes dans une grande salle.]];
 obj = { apple };
};
``

En tant que syntaxique de commodité, vous pouvez affecter des objets de la pièce à la fin dans un
":avec" bloc. Cela vous permet de supprimer un niveau d'indentation, et le groupe
des objets à la fin d'une salle de définition:

``
chambre {
 nam = "principal";
 disp = "Big room";
 dsc = [[Vous êtes dans une grande salle.]];
}:avec {
"boîte",
}
``

Vous pouvez même déclarer vos objets directement dans une salle de définition. Voici
un exemple de comment vous pouvez le faire:

``
chambre {
 nam = "principal";
 disp = "Big room";
 dsc = [[Vous êtes dans une grande salle.]];
}:avec {
 obj {
 nam = "boîte";
 dsc = [[je vois un {box} sur le sol.]];
 act = [[Dur!]];
}
};
``

Ceci est utile pour des objets et des paysages. Mais dans ce cas, vous serez en mesure
pour créer des objets avec une variable de lien. Heureusement, pour les décorations de ne pas
ont pour.

Si la pièce est placée à quelques les objets, séparer les liens avec des virgules, pour
exemple:

 obj = {"boîte", apple };

Vous pouvez insérer des sauts de ligne pour plus de clarté, lorsque de nombreux objets, par exemple:

``
obj = {
'table',
'pomme',
"couteau",
};
``

Une autre façon de placer des objets est l'appel des fonctions dont les objets sont
placé dans la salle. Ce point sera discuté plus loin.

## Objets utilisés comme décoration

Les objets qui finissent par être déplacés dans différentes scènes ou dans et
de l'inventaire du joueur pendant le jeu sont généralement donnés un nom ou
affectée à une variable. Nommage d'un objet nous permet de consulter et de travailler avec
des objets de notre code chaque fois que le joueur (ou notre éditeur de code) qui se passe de l'être.

D'autre part, plusieurs ou la plupart de notre monde du jeu peuvent consister en des objets qui
servir à aucune autre fin que de la décoration. Ces objets existent seulement pour enrichir
nos environnements avec descriptif de contenu pour le joueur.

De tels objets peuvent être nombreuses, et de plus, il est courant pour eux d'être
doubles les uns des autres. Une forêt peut être remplie par de nombreuses instances
de la même arborescence, les rues de la ville peuvent contenir le même poteau d'éclairage sur chaque bloc.
Nous utilisons une approche différente pour créer des objets tels que nous
pour les objets uniques que le joueur peut manipuler.

### Le même objet dans plusieurs salles

En utilisant notre exemple de forrested zone, vous pouvez créer un objet unique et
le placer dans plusieurs pièces comme suit:

``
obj {
 nam = "arbre";
 dsc = [[Il y a un {arbre}.]];
 act = [[Tous ces arbres se ressemblent.]];
}

chambre {
 nam = "Forêt";
 obj = {arbre };
}

chambre {
 nam = "Rue";
 obj = {arbre };
}

``

### Utiliser des balises au lieu de noms

Un objet décoratif ne peut apparaître que dans une seule pièce. Pour des objets comme ceux-ci,
vous pouvez leur attribuer un nom local à l'aide de la " tag " attribut. Par l'attribution d'une
tag, vous n'avez pas à venir avec un nom unique à l'échelle mondiale. Les chaînes attribué
pour le 'tag' attribut ressembler à un nom, mais sont précédées par un '#' symbole.
Même ainsi, vous référer à l'objet de son étiquette sans #:

``
obj {
 tag = '#fleurs";
 dsc = [[il y a {fleurs}.]]
}
``

Tous les objets ont des noms si vous définissez un ou pas. Dans cet exemple, le
objet étiqueté comme "fleurs" sera donné un nom généré automatiquement.
Se référant à un objet de son étiquette n'est possible que dans la salle de cours.
Par exemple:

``
-- recherche de la salle de cours pour le premier objet étiqueté comme '#fleurs
dprint(_'#flowers")
``

Un moyen de penser les balises, c'est qu'ils sont des noms locaux. Pour plus de commodité,
vous pouvez identifier un objet par l'attribution d'une étiquette à l' 'nam' attribut. Rappelez-vous que
les balises de toujours commencer par un caractère " # " alors que les noms ne sont pas.

``
obj {
 nam = '#fleurs";
 dsc = [[il y a {fleurs}.]]
}
``

Par l'attribution d'une étiquette à l' 'nam' attribut, l'objet de nom réel sera
généré automatiquement derrière les coulisses.

### L'attribut de l'utilisation de la scène le décor

Depuis, le paysage ne change pas sa position, il est logique de faire partie
de la description de la scène et non dynamiques. Ceci est fait attribut de la scène
'décor'. le décor est toujours affichée et la fonction principale-description de la
décor de la scène.

``
chambre {
 nam = "Maison";
 dsc = [[je suis à la maison.]];
 décor = [[, je vois beaucoup de choses intéressantes. Par exemple, {#mur|mur}
 la pendaison {#photo|image}.]];
}: avec {
 obj {
 nam = '#wall";
 act = [[le Mur comme un mur!]];
};
 obj {
 nam = '#modèle';
 act = [[van Gogh?]];
}
}
``

Ici, nous voyons plusieurs techniques:

1. Un texte décrit le paysage;
2. Comme les références sont utilisées conception avec une mission claire objets auxquels ils
 appartiennent {le nom de l'objet|texte};
3. Que les noms des objets sont des balises, de ne pas penser à leur unicité;
4. Les objets de la scène dans la scène n'a pas d'attributs dsc, comme leur
 le rôle de décor.

Bien sûr, vous pouvez combiner toutes les techniques entre elles tout
les proportions.

## Objets associés à d'autres objets

Les objets peuvent également contenir l'attribut 'obj' (ou design 'avec'). Dans ce cas,
lors de la sortie d'objets, au LIEU d'être déployé des listes de manière cohérente. Cette
la technique peut être utilisée pour créer des objets conteneurs ou tout simplement pour lier un peu
les descriptions. Par exemple, mettre sur la table une Pomme.

``
obj {
 nam = 'Pomme';
 dsc = [[, Sur la table, {Apple}.]];
 act = 'Prendre quoi?";
};

obj {
 nam = 'table';
 dsc = [[la chambre est un {table}.]];
 act = 'Hm... Juste une table...';
 obj = { 'Apple' };
};

chambre {
 nam = "Maison";
 obj = { 'table' };
}
``

Ainsi, dans la description de la scène, nous allons voir les descriptions des objets de la table " et
"Pomme", car "Apple" est associée à un objet de la table et le moteur en
la sortie de l'objet "table" après " asn " sera systématiquement "dsc" et tous les
ses objets imbriqués.

Aussi, il convient de noter que, en termes d'objet "table" (par exemple, le déplacement
de salle en salle), nous allons déplacer automatiquement et investi dans un objet
"Pomme".

Bien sûr, cet exemple peut aussi être écrite différemment, par exemple,
donc:

``
chambre {
 nam = "Maison";
}: avec {
 obj {
 nam = 'table';
 dsc = [[la chambre est un {table}.]];
 act = 'Hm... Juste une table...';
 }: avec {
 obj {
 nam = 'Pomme';
 dsc = [[, Sur la table, {Apple}.]];
 act = 'Prendre quoi?";
};
}
}
``

Vous pouvez choisir la manière la plus claire pour vous.

## Définition des attributs et des gestionnaires d'événements avec des fonctions

Jusqu'à maintenant, nous avons attribué des chaînes à la plupart des attributs des objets
et les chambres. Vous pouvez également affecter des _functions_ d'attributs. Par exemple:

``
disp = function()
 p 'Pomme';
fin
``

Ce n'était pas un très bon exemple, mais cela montre bien que la syntaxe devrait ressembler
comme. Nous pouvons ainsi ont écrit "disp = "Pomme". L'objectif principal de ces fonctions est de retourner une chaîne ou une valeur booléenne.

Jetons un coup d'oeil comment retourner quelque chose. Une façon de retourner une chaîne de caractères est par un
déclaration explicite comme suit:

 retour "Pomme";

Lorsqu'une déclaration de ce type est rencontré, la fonction de sortie avec la
compte tenu de la valeur de retour. Dans ce cas, ce serait la chaîne, "Apple".

Le moyen le plus courant pour la sortie des chaînes en utilisant les trois suivantes intégré
fonctions:

- p ("texte") -- texte de sortie et de l'espace blanc;
- pn ("texte") -- texte de sortie avec saut de ligne;
- pr ("texte") -- output texte-est.

Ces fonctions n'ont pas de texte de sortie directement. Le moteur maintient un presse-papiers
où le texte est stocké. Le contenu de cette presse-papiers est envoyé au moteur
lorsque votre fonction retourne. Cela vous permet de construire un bloc de
texte pour revenir en appelant p/pn/pr plusieurs fois. Dans la plupart des cas, vous ne devriez pas
vous soucier de l'endroit où les sauts de ligne ou de tout autre problème de mise en forme.
Cela est particulièrement vrai des descriptions. Le moteur de placer du texte et ajouter
l'espacement de façon intelligente basée sur ce que vous présentez pour le joueur.

Comme avec toutes les fonctions, vous pouvez omettre les parenthèses s'il ya seulement un
valeur unique pour la fourniture de:

 pn "Sans crochets!"

Il est probable que vous aurez souvent à concaténer plusieurs chaînes.
Vous pouvez soit utiliser '..' ou ',' à cette fin, et dans ce cas, les parenthèses
seront nécessaires:

 pn ("Chaîne 1".." Ligne 2");
 pn ("Chaîne 1" "chaine 2");

> La principale différence entre les attributs et les gestionnaires d'événements, c'est que seul événement
> les gestionnaires sont en mesure de modifier l'état de l'univers du jeu. Lors de la rédaction d'un
> l'attribut comme 'asn', n'oubliez pas que votre but est de retourner une valeur et
> ne pas affecter ce qui se passe dans le jeu! L'oubli de cette règle conduira à
> comportement imprévisible.

__Important!__

> Il y a quelque chose à garder à l'esprit lors de l'écriture de gestionnaires d'événements ainsi. Ils
> doit s'exécuter rapidement. Les gestionnaires d'événements sont appelés en réponse à l'utilisateur
> l'action, et le moteur s'attend à revenir immédiatement de sorte qu'il peut
> actualiser l'interface utilisateur. Si vous souhaitez insérer des retards de contrôle de la
> > sortie que les utilisateurs peuvent voir, vous devrez utiliser le "timer" du module.


Fonctions presque toujours de retour dynamique des résultats qui sont basés sur le contenu
de variables. Par exemple:

``
obj {
 nam = 'Pomme';
 vu = false;
 dsc = fonction(s)
 si s pas.vu alors
 p '{quelque Chose} est sur la table.';
d'autre
 p 'Un {Apple} se trouve sur la table.';
fin
fin;
 act = fonction(s)
 si s.vu alors
 p " C'est une Pomme!';
d'autre
 s.vu = true;
 p 'Euh... C'est une Pomme!';
fin
fin;
};
``
Lorsqu'une fonction est attribuée à un attribut, son premier paramètre se réfère toujours
à l'objet lui-même. Nous avons généralement le nom de cette variable " s " pour "soi". Vous
pourrait aussi écrire au nom de l'objet (\_'Pomme'), mais lors de l'écriture de ces
fonctions, il est généralement préférable d'utiliser le paramètre " s " que de faire un
appel explicite à le nom de l'objet. Cela vous évitera d'avoir à les réécrire
si jamais vous avez besoin de renommer l'objet.

Dans cet exemple, nous faisons des descriptions de texte dynamique. La première fois que le joueur
les rencontres de cet objet, il sera dénommé "quelque Chose". Une fois qu'ils
interract avec elle, le " vu " variable devient vrai. Après cela, ils vont voir
que l'objet est un "Apple".

La syntaxe de 'si' états est facile à lire. Voici quelques exemples pour
clarté:

 si <expression>, puis <actions> fin

 si vous avez de "Pomme" puis
 p 'j'ai une pomme!'
fin

 si <expression>, puis <actions> else <actions autrement> fin

 si vous avez de "Pomme" puis
 p 'j'ai une pomme!'
d'autre
 p 'je n'ai pas de pommes!'
fin

 si <expression> alors <action> elseif <expression 2> alors <action 2>
 d'autre <sinon> end -- etc.

 si vous avez de "Pomme" puis
 p 'j'ai une pomme!'
 elseif ont "fork" alors
 p 'je n'ai pas de pommes, mais il y a cette fourchette dans mon inventaire!'
d'autre
 p " je n'ai pas une pomme ou une fourchette.
fin

L'_expression_ d'une instruction "if" est une valeur booléenne composé de vrai/faux
termes séparés par "et", "ou", "non", et la parenthèse de contrôle de l'évaluation
de priorité. Expressions contenant une seule variable (c'est à dire: "si <variable>`)
il suffit de vérifier que la variable n'est pas égale à la valeur `false`. L'égalité
opérateur '==', et l'opérateur d'inégalité est '~='.

``
si ce n'est pas de "Pomme" et pas "fork" alors
 p " je n'ai pas une pomme ou une fourchette!'
fin

...
si w ~= pomme puis
 p 'Ce n'est pas une Pomme.';
fin
...

si le temps() == 10 alors
 p: "C'est de la 10ème tour!'
fin
``

__Important!__

Vous devez définir les variables avant de les utiliser. Si vous utilisez une variable dans un
expression de condition sans définir d'abord, il va générer une erreur.

### Les variables d'objet

De l'entrée.vu "signifie que la variable" vu " est placé dans l'objet 's'
(c'est à dire "Pomme"). Rappelez-vous, nous avons appelé le premier paramètre de la fonction
's' (auto) comme premier paramètre -- est l'objet lui-même.

Les variables d'objet doit être défini à l'avance si vous êtes à modifier.
Quelque chose comme nous l'avons fait avec la vu. Mais les variables peuvent être nombreux.

``
obj {
 nam = 'Pomme';
 vu = false;
 mangé = false;
 couleur = 'rouge';
 poids = 10;
...
};
``

Toutes les variables d'un objet quand il change, entrez le fichier de sauvegarde du jeu.

Si vous ne voulez pas que la variable a été dans un fichier de sauvegarde, vous pouvez déclarer ces
les variables dans un bloc spécial:

``
obj {
 nam = 'Pomme';
{
 t = 1; -- cette variable ne sera pas obtenir de l'enregistrer
 x = false; -- et c'est trop
}
};
``

Normalement, vous n'avez pas besoin de le faire. Cependant, il y a une situation dans laquelle
cette technique sera utile. Le fait que les tableaux et les tables de l'objet sont
toujours sauvé. Si vous utilisez des tableaux pour stocker des valeurs inaltérables, vous pouvez écrire:

``
obj {
 nam = 'Pomme';
{
 texte = { "un", "deux", "trois" }; -- ne jamais aller pour un fichier de sauvegarde
}
...
};
``

Vous pouvez accéder aux variables d'un objet via s-si c'est lui-même objet. ou
la variable de référence, par exemple:

``
apple = obj {
 couleur = 'rouge';
}
...
quelque part dans un autre endroit
 apple.couleur = "vert"
``

Ou par nom:

``
obj {
 nam = 'Pomme';
 couleur = 'rouge';
}
...
quelque part dans un autre endroit
 _"Pomme".couleur = "vert"
``

En fait, vous pouvez créer des variables de l'objet à la volée (sans pré-définir
eux), bien que généralement il ne fait aucun sens. Par exemple:

``
apple 'xxx' (10) -- créer une variable xxx, l'objet d'apple sur le lien
(_'Pomme') 'xxx' (10) -- même, mais le nom de l'objet
``

### Les variables locales

En plus des variables d'objet, vous pouvez utiliser des variables locales et globales.

Les variables locales sont créées à l'aide de office word local:

``
act = fonction(s)
 local w = _'ampoule'
 w.lumière = true
 p [[j'ai appuyé sur le bouton et l'ampoule allumée.]]
fin
``

Dans cet exemple, la variable " w " n'existe que dans le corps de la fonction de gestionnaire
loi. Nous avons créé un renvoi temporaire de la variable "w", qui fait référence à l'objet
la "lumière" à la fonction de changement de variable de la lumière de cet objet.

Bien sûr, on pourrait écrire:

 _'ampoule'.lumière = true

Mais imaginez si nous avons besoin d'exécuter plusieurs actions avec l'objet de ces
cas est plus facile d'utiliser une variable temporaire.

Les variables locales ne sont jamais affichées dans le fichier-enregistrer et de jouer le rôle de
temporaire de variables auxiliaires.

Les variables locales peuvent être créées en dehors des fonctions, alors cette variable est
visible uniquement à l'intérieur d'un fichier lua et manque un fichier de sauvegarde.

Un autre exemple d'utilisation de variables locales:

``
obj {
 nam = 'chaton';
 etat = 1;
 act = fonction(s)
 s.etat = s.état + 1
 si s.etat > 3 alors
 s.etat = 1
fin
 p [[Ronronner!]]
fin;
 dsc = fonction(s)
 local dsc = {
 "L'{chaton} ronronne.",
 "L'{chaton} est de jouer.",
 "L'{chaton} est léché.",
};
p(dsc[s.état])
fin;
fin
``

Comme vous pouvez le voir, dsc, nous avons déterminé la matrice de l'asn. "local" indique qu'il s'
fonctionne au sein de l'asn. Bien sûr, cet exemple vous pouvez l'écrire comme:

``
dsc = fonction(s)
 si s.etat == 1 alors
 p "{le Chaton} est ronronner."
 elseif s.etat == 2 alors
 p "{le Chaton} est de jouer."
d'autre
 p "{le Chaton} est léché.",
fin
fin
``

### Variables globales

Vous pouvez également créer une variable globale:

``
global { -- définition des variables globales
 global_var = 1; -- nombre
 some_number = 1.2; -- nombre chaîne_quelconque = 'chaîne';
 know_truth = false; -- une valeur Booléenne
 tableau = {1, 2, 3, 4}; -- tableau
}
``

Une autre forme, pratique pour les définitions uniques:

 global 'global_var' (1)

Les variables globales toujours obtenir le fichier-enregistrer.

En plus des variables globales, vous pouvez définir des constantes. Syntaxe similaire à
les variables globales:

``
const {
 A = 1;
 B = 2;
}
const 'Aflag' (false)
``

Le moteur de contrôle de la cohérence des constantes. Les constantes ne sont pas
entrez le fichier-enregistrer.

Parfois, vous avez besoin de travailler avec une variable qui n'est pas défini aslocal (et
visible dans l'ensemble de vos fichiers lua), mais ne devrait pas obtenir dans le fichier de sauvegarde. Pour
ces variables, vous pouvez utiliser la Déclaration suivante:

``
déclarer {
 A = 1;
 B = 2;
}
déclarer 'Z' (false)
``

La Déclaration n'est pas stocké dans le fichier de sauvegarde. L'un des plus importants
propriétés de déclarations, c'est que vous pouvez déclarer des fonctions par exemple:

``
déclarer 'test' (function()
 p "Hello world!"
fin)

mondial " f " (test)
``

Dans de tels cas, vous pouvez affecter la valeur de la fonction 'test' autres variables
et l'état de ces variables peuvent être stockés dans l'filesave. C'est, une
déclaré fonction peut être utilisée comme valeur de la variable!

Vous pouvez déclarer défini précédemment fonction, par exemple:

 déclarer "dprint' (dprint)

Faisant de telles fonctions non déclarées -- déclaré.

La Déclaration de la fonction, en fait, la cession du nom de la fonction,
merci de ce que nous pouvons retenir de cette fonctionnalité comme un lien.

### Des fonctions d'assistance

Vous pouvez écrire vos fonctions d'assistance et de les utiliser à partir de votre jeu, par exemple:

``
fonction mprint(n, ...)
 local a = {...}; -- tableau temporaire avec les arguments de la fonction
 p(a[n]) -- obtenir le n-ième élément du tableau
fin
...
dsc = fonction(s)
 mprint(s).l'état {
 "L'{chaton} ronronne.",
 "L'{chaton} est de jouer.",
 "L'{chaton} est léché." });
fin;
``
Ne faites pas attention à cet exemple, si il vous semble difficile.

### La valeur de retour des gestionnaires d'

Si vous voulez montrer que l'action n'est pas exécutée (gestionnaire n'a rien
utile), renvoie false. Par exemple:

``
act = fonction(s)
 si broken_leg alors
 return false
fin
 p [[j'ai des coups de pied le ballon.]]
fin
``

Cela affiche la description par défaut est spécifié à l'aide d'un gestionnaire de jeu.loi".
Habituellement, la valeur par défaut description contient la description de l'irréalisable action.
Quelque chose comme:

 jeu.act = 'Hm... ne fonctionne Pas...';

Donc, si vous ne définissez pas le gestionnaire de loi ou renvoyées par les faux -- il est
cru qu'il n'y a pas de réaction et le moteur va effectuer la même
gestionnaire de l'objet "jeu".

Habituellement, il n'ya pas de sentiment de fausse déclaration de la loi, mais il y a d'autres
les gestionnaires, qui sera discuté plus loin, pour qui le comportement décrit
est exactement le même.

En fait, outre la de jeu.loi " et " loi " -- l'attribut d'objet existe gestionnaire
'onact" du jeu de l'objet, ce qui peut interrompre l'exécution de gestionnaire de "loi".

Avant d'appeler le gestionnaire de " loi " d'un objet est appelée onact de jeu. Si
le gestionnaire renvoie la valeur false, l'exécution de la loi est passé. 'onact'
pratique à utiliser pour le contrôle des événements dans le jeu, par exemple:

``
-- appelé onact chambres, si elles sont
-- pour les actions sur n'importe quel objet

jeu.onact = fonction(s, ...)
 local r, v = std.appel(ici(), 'onact', ...)
 si v == false then -- si la valeur est false, couper la chaîne
 retour r, v
fin
de retour
fin

chambre {
 nam = 'boutique';
 disp = 'Boutique';
 onact = fonction(s, w)
 p [[Dans le magasin, vous ne pouvez pas voler!]]
 p ([[, Même si c'est seulement ]], w, '.')
 return false
fin;
 obj = { 'ice cream', 'pain' };
}
``

Dans cet exemple, lors de la tentative de "toucher" tout élément, sera afficher un message
d'interdire cette action.

Tout ce qui est décrit ci-dessus, sur l'exemple de la " loi " s'applique pour les autres
gestionnaires: tak, inv, de l'utilisation et de transitions, comme il sera discuté plus tard.

> Parfois, vous devez appeler la fonction de gestionnaire manuellement.
> Il utilise la syntaxe d'un appel de méthode
> objet. Objet:méthode(paramètres)'. Par exemple:

 apple:la loi() -- gestionnaire d'appel 'loi' de l'objet 'apple' (si il défini comme un
 la fonction!). _"Pomme": loi() -- même, mais de nom, pas une référence à une variable

Cette méthode fonctionne uniquement si la méthode appelée conçu comme une caractéristique. Vous pouvez utiliser
'std.call()' pour un gestionnaire est invoquée dans la façon dont il fait lui-même à la PLACE.
(Qui sera décrit dans le futur).

## Inventaire

La façon la plus simple de créer un objet qui peut être ramassé à assigner un
gestionnaire à l' 'tak' attribut, qui est l'abréviation de "prendre". Par exemple:

``
obj {
 nam = 'Pomme';
 dsc = " Sur la table est {Apple}.';
 inv = fonction(s)
 p 'j'ai mangé la Pomme.'
 supprimer le(s); -- supprimer une Pomme à partir de l'inventaire
fin;
 tak = 'Vous avez pris la Pomme.';
};
``

Dans ce cas, le joueur objet est "Apple" (cliquez sur le lien dans la scène) -- la
Apple est retiré de la scène et a ajouté à l'inventaire. Lorsque le joueur utilise
l'inventaire (double-cliquez sur le nom de l'objet) -- l'appel du gestionnaire
'inv'.

Dans notre exemple, lorsque le joueur utilise la Pomme dans l'inventaire d'Apple-être
mangé.

Bien sûr, on pourrait mettre en œuvre le code de l'objet "d'agir", par exemple:

``
obj {
 nam = 'Pomme';
 dsc = " Sur la table est {Apple}.';
 inv = fonction(s)
 p 'j'ai mangé la Pomme.'
 supprimer le(s); -- supprimer une Pomme à partir de l'inventaire
fin;
 act = fonction(s)
prendre(s)
 p 'Vous avez pris la Pomme.';
fin
};
``
Si l'objet dans l'inventaire n'est pas déclaré un gestionnaire pour le "inv", seront appelés " le jeu.inv'.

Si le gestionnaire est 'tak' retourne la valeur false, l'élément ne sera pas pris, par exemple:

``
obj {
 nam = 'Pomme';
 dsc = " Sur la table est {Apple}.';
 tak = fonction(s)
 p "Il est véreux!"
 return false
fin;
};
``

## Transitions

Le traditionnel transitions en PLACE apparaissent comme des liens ci-dessus la description
de la scène. Pour déterminer les transitions entre les scènes est utilisé attribut de la scène --
liste de "façon". Dans la liste sont déterminés par la chambre dans la forme de la pièce
les noms ou les références à des variables comme la liste 'obj'. Par exemple:

``
chambre {
 nam = 'room2';
 disp = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 chemin = { 'main' };
};

chambre {
 nam = "principal";
 disp = 'pièce Principale';
 dsc = 'Vous êtes dans une grande salle.';
 chemin = { 'room2' };
};
``

Avec cela, vous serez en mesure d'aller, entre les scènes, 'main' et 'room2'. Comme vous
rappelez-vous, "disp" peut être une fonction, et vous pouvez générer des noms de transitions sur
la mouche. Ou utilisez le titre, pour séparer le nom de la scène comme le titre et la façon dont
la transition de nom:

``
chambre {
 nam = 'room2';
 disp = 'hall';
 title = 'hall';
 dsc = 'Vous êtes dans une salle immense.';
 chemin = { 'main' };
};

chambre {
 nam = "principal";
 title = "la salle principale';
 disp = 'la salle principale';
 dsc = 'Vous êtes dans une grande salle.';
 chemin = { 'room2' };
};
``

Lors du passage entre les pièces du moteur appelle le gestionnaire pour le "onexit' de
de la scène actuelle et la "onenter" dans la scène où est le joueur. Pour
exemple:

``
chambre {
 le onenter = 'Vous entrez dans la salle.';
 nam = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 chemin = { 'main' };
 onexit = 'Vous sortir de la pièce.';
};
``

Bien sûr, comme tous les gestionnaires, 'onexit" et "le onenter" peuvent être des fonctions.
Alors le premier paramètre est (comme toujours) l'objet lui-même et de chambre à la
la deuxième chambre est l'endroit où le joueur va aller (pour "onexit") ou à partir de laquelle est
va laisser (pour "le onenter'). Par exemple:

``
chambre {
 onenter = fonction(s, f)
 si f^ "main", puis
 p 'Vous allez à la salle principale.';
fin
fin;
 nam = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 chemin = { 'main' };
 onexit = fonction(s, t)
 si t^un'main' alors
 p " je ne veux pas revenir en arrière!'
 return false
fin
fin;
};
``

Écrit:

 si f^ "main", puis

Ce mappage du nom de l'objet. Cette alternative dossiers:

 si f = = _ "main", puis

Ou:

 si f.nam = = "main", puis

Ou:

 si les mst.nameof(f) = = "main", puis

Comme vous pouvez le voir, par exemple, onexit, ces gestionnaires autre que la ligne de retour
un Boolean valeur d'état. De même, le processeur onact, nous pouvons annuler la
de transition, en retournant false de onexit/le onenter.

Vous pouvez également retourner une autre façon, si il semble que vous à l'aise:

 retour "je ne veux pas revenir en arrière", false

Si vous utilisez la fonction 'p'/'pn'/'pr', puis il suffit de retourner le statut de
transaction avec la finale de "retour", comme illustré dans l'exemple ci-dessus.

__Important!__

> Il est à noter que lors de l'appel du gestionnaire le onenter' pointeur vers
> scène actuelle (ici()) **pas encore changé**!!! De là, au LIEU
> les gestionnaires "exit" (quitter la salle) et "enter" (entrée de la salle),
> qui sont appelés déjà _posle_ comment la transition s'est passé. Ces
> les gestionnaires sont recommandés lorsqu'il n'y a pas de
> besoin d'interdire le passage.

Parfois, il ya un besoin de nommer la transformation diffère du nom de
la pièce dans laquelle cette transition conduit. Il y a sever toujours à le faire. Pour
exemple, à l'aide de 'chemin'.

``
chambre {
 nam = 'room2';
 title = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 chemin = { path { 'pièce principale', 'principal'} };
};

chambre {
 nam = "principal";
 title = 'pièce Principale';
 dsc = 'Vous êtes dans une grande salle.';
 chemin = { path {'salle', 'room2'} };
};
``

En fait, 'chemin' crée une chambre avec l'attribut "disp", qui égale à la
premier paramètre, et la fonction spéciale "le onenter", qui redirige le lecteur
à la chambre indiquée par le deuxième argument de la "voie".

Si vous spécifiez trois paramètres:

 chemin = { path {'#hall', 'chambre', 'room2'} };

Le premier paramètre est le nom (ou balise, comme dans l'exemple) pour un
chambre.

Forme Alternative d'entrée, avec la mission explicite de l'attribut nam:

 chemin = { path { nam = '#hall', 'chambre', 'room2'} };

Vous pouvez modifier le nom de la transition, après la transition s'est produite à
moins une fois, et vous le savez, qu'est-ce que cette pièce:

 chemin = { path {'#udvari', 'porte', après = 'salon', 'room2'} };

Tous les paramètres, à l'exception de la transition de nom, peut-être des fonctions.

Ainsi, le 'chemin' vous permet d'appeler un des transitions de façon pratique.

Parfois, vous devrez peut-être activer et de désactiver les transitions. Reallyit n'est pas souvent
nécessaire. L'idée de transitions est que la transition visible, même quand
c'est impossible. Par exemple, imaginez la scène en face de la maison par l'
porte d'entrée. Pour entrer dans la maison car la porte est fermée.

Il fait peu de sens pour masquer la transition. Juste à la fonction de " l'
onenter " la scène à l'intérieur de la maison, nous vérifions si une touche de caractère? Et si
la clé n', de parler du fait que la porte est fermée, et d'interdire la
de transition. Il augmente l'interactivité et simplifie le code. Si vous voulez
faire la porte de l'objet dans la scène, la place dans la chambre, mais dans la "loi"
handlerd o les portes d'inspection, ou de permettre à un joueur de l'ouvrir avec une clé (comment
le faire - nous verrons plus tard), mais le passage lui-même faire l'
joueur de la manière habituelle par le biais de la ligne de transitions.

Cependant, il ya des moments où la transition n'est pas évidente et il semble que
à la suite de certains événements. Par exemple, une horloge et a vu un tunnel secret derrière
c'.

``
obj {
 nam = "horloge";
 dsc = [[vous voyez un vieux {horloge}.]];
 act = fonction(s)
 activer '#horloge"
 p [[Vous voyez que la montre est un passage secret!]];
fin;
}

chambre {
 nam = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 obj = { 'horloge' };
 chemin = { path { '#watch', 'watch', 'inclock' }:disable() };
};
``

Dans cet exemple, nous avons créé _disabled_ de transition, en appelant la méthode "désactiver"
de la pièce créée à l'aide de la "voie". La méthode 'désactiver' avoir tous les éléments (pas
chambres seulement), elle se traduit par l'objet dans l'état désactivé, ce qui signifie que le
objet cesse d'être à la disposition du joueur. Une propriété remarquable désactivé
la facilité, c'est qu'il peut _enabled_ avec " enable()';

De plus, lorsque le joueur clique sur le lien qui décrit la montre, appelé
gestionnaire, "loi", à l'aide de la fonction " enable()' permet une transition visible.

L'alternative n'est pas à l'arrêt et de fermeture de l'objet:

``
obj {
 nam = "horloge";
 dsc = [[vous voyez un vieux {horloge}.]];
 act = fonction(s)
 ouvrir "#horloge"
 p [[Vous voyez que la montre est un passage secret!]];
fin;
}

chambre {
 nam = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 obj = { 'horloge' };
 chemin = { path { '#watch', 'watch', 'inclock' }:close() };
};
``

Quelle est la différence? La désactivation d'un objet signifie que l'objet cesse d'être
à la disposition du joueur. Si l'objet est imbriqué avec d'autres objets, ces
les objets deviennent inaccessibles. La fermeture de l'usine rend indisponible l'
le contenu de cet objet, mais non pas de l'objet lui-même.

Toutefois, dans le cas de chambres et de fermeture des chambres et des handicapés, salle de conduire à l'
même résultat: la transition n'est pas disponible.

Une autre option:

``
chambre {
 nam = 'inclock';
 dsc = [[j'en heures.]];
}:close()

obj {
 nam = "horloge";
 dsc = [[vous voyez un vieux {horloge}.]];
 act = fonction(s)
 ouvrir "inclock'
 p [[Vous voyez que la montre est un passage secret!]];
fin;
}

chambre {
 nam = 'Hall';
 dsc = 'Vous êtes dans une salle immense.';
 obj = { 'horloge' };
 chemin = { path { 'watch', 'inclock' } };
};
``

Ici, nous fermer et ouvrir n'a pas bougé, et la chambre, qui est la transition. chemin
montre lui-même si la pièce dans laquelle il dirige handicapés ou fermé.

## L'effet des objets les uns sur les autres

Le joueur peut utiliser un objet de l'inventaire sur d'autres objets. Pour qu'il clique sur le
de la souris sur l'élément, puis pour la scène. Lorsque ce gestionnaire est appelé "utilisé"
tout objet dont la fonction, et le gestionnaire de l '"utilisation" de l'objet qui s'appliquent.

Par exemple:

``
obj {
 nam = "couteau";
 dsc = " Sur la table est un {couteau}';
 inv = 'Pointu!';
 tak = 'j'ai pris le couteau!';
 utilisation = 'Vous essayez d'utiliser le couteau.';
};

obj {
 nam = 'table';
 dsc = 'Dans le {table}.';
 act = 'Hm... Juste une table...';
 obj = { 'couteau' };
 utilisé = fonction(s)
 p 'Vous essayez de faire quelque chose avec une table...';
 return false
fin;
};
``

Dans cet exemple, utilisé gestionnaire retourne false. Pourquoi? Si vous vous souvenez, le retour
false signifie que le gestionnaire de charge du moteur à propos de ce cas, il n'est pas
traités. Si nous aurait renvoyé la valeur false, la file d'attente à la gestionnaire de l '"utilisation" de l'objet
"couteau" ne serait tout simplement pas venir. En fait, la réalité est en général, l'utilisation ou la
l'utilisation ou utilisé, il est peu probable que cela a du sens de faire à la fois le gestionnaire au cours de l'
l'action de l'objet sur le sujet de.

Un autre exemple, quand il est commode de fausse déclaration:

``
utilisation = fonction(s, w)
 si w^'Apple', puis
 p [[j'ai nettoyé la Pomme.]]
 w.couper = true
de retour
fin
 return false;
fin
``

Dans ce cas, l'utilisation d'Apple ne gère qu'une situation-effet sur Apple. Dans
dans d'autres cas, la fonction renvoie la valeur false et le moteur d'appel par défaut:
jeu.utiliser.

Mais c'est mieux si vous vous inscrivez à l'action, à défaut d'un couteau:

``
utilisation = fonction(s, w)
 si w^'Apple', puis
 p [[j'ai nettoyé la Pomme.]]
 w.couper = true
de retour
fin
 p [[il n'est Pas nécessaire de brandir un couteau!]]
fin
``

Cet exemple illustre également le fait que le deuxième argument u utilisation est la
sujet sur lequel nous agissons. La méthode "utilisé" en conséquence, le deuxième argument -- 
c'est l'entité qui agit sur nous:

``
obj {
 nam = "corbeille";
 dsc = [[Dans le coin est un {corbeille}.]];
 utilisé = fonction(s, w)
 si w^'Apple', puis
 p [[j'ai jeté la Pomme dans la corbeille.]]
supprimer(w)
de retour
fin
 return false;
fin
}
``

Comme vous vous en souvenez, avant d'appeler utilisation onuse gestionnaire est appelé à partir du jeu
l'objet, l'objet "joueur", et puis ma salle de cours. Vous pouvez bloquer
"l'usage" de retour de l'une des méthodes suivantes 'onuse' -- faux.

Utiliser "utiliser" ou "utilisé" (ou les deux) est une question de préférence personnelle, cependant, l'
la méthode utilisée est appelée plus tôt et, par conséquent, a une priorité plus élevée.

## Le "Joueur" De L'Objet

Joueur dans le monde, au LIEU représenté par un objet de type "joueur". Vous pouvez
pour créer plusieurs joueurs, mais un joueur est présent par défaut.

Le nom de cet objet est un "joueur". Il y a une variable de référence pl qui
les points de cet objet.

Généralement, vous n'avez pas besoin de travailler avec cet objet directement. Mais parfois, il
peut être nécessaire.

Par défaut, l'attribut 'obj', le joueur représente l'inventaire. Habituellement,
il ne fait aucun sens pour remplacer un objet de type joueur, cependant, vous pouvez le faire
c':

``
jeu.joueur = joueur {
 nam = "Basilic";
 chambre = 'cuisine'; -- la salle de départ du joueur
 puissance = 100;
 obj = { 'Apple' }; -- nous allons lui donner une Pomme
};
``

Au LIEU d'avoir la capacité de créer de multiples acteurs et de basculer entre les
eux. C'est le " change_pl()'. En tant que paramètre à passer à la fonction requise d'un
l'objet de type "joueur" (ou son nom). La fonction de commutation du courant
joueur, andnecessary, se déplace dans la pièce où le nouveau joueur.

Le " moi()' renvoie toujours le joueur actuel. Par conséquent, dans la plupart des jeux et
moi() == pl.

## Le "Monde" De L'Objet

Le monde du jeu est représenté par un objet de type monde. Le nom de cette
objet de la "game". Il y a une variable de référence, également appelé jeu de.

Habituellement, vous n'avez pas à travailler avec cet objet directement, mais parfois, vous pouvez appeler
ses méthodes, ou de modifier les valeurs d'une variable dans cet objet.

Par exemple, la variable de jeu.page de codes contient l'encodage de la source
code jeux, et par défaut à "UTF-8". Je ne recommande pas l'utilisation d'autres
encodages, mais parfois, le choix de l'encodage peut être nécessité.

La variable de jeu.joueur-contient le joueur actuel.

En outre, comme vous le savez déjà, l'objet de la "game" peut contenir par défaut
des gestionnaires: "loi", 'inv', 'utilisation', 'tak', qui sera appelée si les actions de la
l'utilisateur ne sont pas pas trouvé d'autres gestionnaires (ou tous retourné false). Pour
exemple, vous pouvez écrire au début du jeu:

 jeu.act = 'ne fonctionne Pas.';
 jeu.inv = 'hmm ... chose Étrange...';
 jeu.utilisation = " ne fonctionne Pas...';
 jeu.tak = 'je n'ai pas besoin de ce...';

Bien sûr, elles peuvent être toutes les fonctions.

Aussi, le jeu de l'objet peut contenir des gestionnaires: onact, ontak, onuse, oninv, onwalk
-- ce qui peut interrompre l'action, en cas de fausse déclaration.

Encore, le jeu de l'objet, vous pouvez définir des gestionnaires d'événements: afteract, afterinv, afteruse,
afterwalk -- qui sont invoquées en cas de succès à effectuer le
d'action.

## Liste Des Attributs

Liste des attributs (comme 'moyen' ou 'obj') vous permettent de travailler avec son contenu
avec un ensemble de méthodes. Attributs-une liste conçu de garder une liste d'objets. Dans
fait, vous pouvez créer des listes pour leurs propres besoins, et de les placer dans des objets, pour
exemple:

``
chambre {
 nam = 'réfrigérateur';
 gel = std.liste { 'ice cream' };
}
``

Bien que généralement il n'est pas nécessaire. Voici la liste des méthodes pour les objets de
de type "liste". Vous pouvez les appeler pour toutes les listes, bien que ces derniers seront généralement
moyen et obj, par exemple:

 des moyens():disable() -- disable toutes les transitions

- désactiver() - désactive tous les objets dans la liste;
- enable() - permet à tous les objets de la liste.
- fermer() - fermer tous les objets de la liste.
- open() - ouvrir la liste des objets;
- add(objet|nom [position]) - ajout d'un objet;
- for_each(fonction, args) à appeler pour chaque objet en fonction des arguments;
- recherche(nom/tag ou d'un objet) est à la recherche de l'objet dans la liste. Renvoie l'objet 
 et l'index;
- srch(nom/tag ou de l'objet) - la recherche d'un objet visible dans la liste;
- vide (): retourne true si la liste est vide;
- zap() - effacer la liste;
- remplacer(quoi, quoi?) - remplacer l'objet dans la liste;
- chat(liste, [position]) - ajouter le contenu de la liste dans la liste actuelle
position;
- del(nom/objet) - supprimer un objet de la liste.

Il y a des fonctions qui renvoient des listes d'objets:

- inv([joueur]) - de retour de l'inventaire du joueur;
- objs([où]) - le retour des objets de la chambre;
- des moyens([chambre]) - retour des transitions de la chambre.

Bien sûr, vous pouvez vous référer aux listes directement:

 pl.obj:ajouter "couteau"

Les objets dans les listes sont stockées dans l'ordre dans lequel ils ajouter. Cependant,
si l'objet est présent attribut numérique pri il joue le rôle de priorité
dans la liste. Si le pri n'est pas spécifié, la valeur de priorité 0 est considéré comme.
Donc, si vous voulez un peu de l'objet a été le premier sur la liste, de donner la priorité pri <
0. Si la fin de la liste -- > 0.

``
obj {
 pri = -100;
 nam = 'truc';
 disp = 'Très important';
 inv = [[Attention avec ce sujet.]];
}
``

## Fonctions qui retournent des objets

Au LIEU de définir un certain nombre de fonctions qui retournent des objets différents. Dans le
description de la fonction utilise les paramètres suivants de l'accord.

- les caractères [ ] décrire les paramètres facultatifs;
- "quoi" ou " où " - désigne un objet (y compris la chambre) est spécifié par le
 nom de balise ou d'une variable de référence;

Ainsi, la fonction principale:

- '_() ' - obtenir l'objet;
- moi()' renvoie à l'objet en cours devant le joueur;
- "ici()' renvoie à la scène actuelle.
- "où ()' renvoie à une pièce ou un objet qui est l'objet spécifié si l'
 l'objet se trouve dans plusieurs endroits, vous pouvez passer dans un deuxième paramètre -- Lua
 le tableau qui sera ajouté à ces objets;
- 'un coffre ()' similaire où(), mais les retours de la salle dans laquelle l'installation 
 se trouve (ce qui est important pour les objets les objets);
- "à partir de([où])' retourne la dernière salle, le joueur va dans la pièce donnée.
 Paramètre facultatif -- à la dernière chambre, pas pour la salle de cours, et pour un
donné;
- vu(ce que [où])' retourne un objet de transition ou, s'il s'agit de présenter et de
 pouvez le voir, il est un second paramètre optionnel -- sélectionnez la scène ou
 objet/liste dans laquelle effectuer la recherche;
- "recherche(ce qui, [où])' retourne un objet ou de transition, si ce n'est dans 
 la scène ou l'objet/liste;
- "inspecter ()' renvoie à l'objet s'il est visible et disponible sur scène. L'
 la recherche est effectuée pour les transitions et les objets, y compris, dans l'objet de l'
 le joueur;
- "ai - ()' renvoie à l'objet si il est dans l'inventaire et n'a pas été désactivée;
- "live ()' renvoie à l'objet si il est présent parmi les vivants, les objets (
 décrit ci-dessous);

Ces fonctions sont principalement utilisés dans les conditions, ou à l'objet de la recherche de
une autre modification. Par exemple, vous pouvez utiliser " vu " pour la rédaction des termes:

``
onexit = fonction(s)
 si on se place 'monstre' then -- si une fonction a 1 paramètre
 --- Je n'ai pas faim ... 
 p 'le Monstre qui est dans le chemin!
 return false
fin
fin
``

Et aussi, pour trouver l'objet dans la scène:

``
utilisation = fonction(s, w)
 si w^ "fenêtre", puis
 local ww = recherche de 'chien'
 si pas ww alors
 p [[où est mon chien?]]
de retour
fin
 lieu(ww, 'rue')
 p 'j'ai cassé la fenêtre! Mon chien a sauté sur la rue.'
de retour
fin
 return false
fin
``

Exemple avec "avoir":

``
...
act = fonction(s)
 si vous avez un 'couteau' ensuite
 p', Mais j'ai un couteau!';
de retour
fin
 prendre "le couteau"
fin
...
``

> La question peut se poser, quelle est la différence entre la fonction de recherche et d' _
> ()? Le fait que lookup() regarde l'objet et si l'objet n'est pas
> trouvé-n'a tout simplement pas de retour. Et le record est de _ () suppose que vous venez de
> vous savez que l'élément que vous obtenez. En d'autres termes, _ () est inconditionnel
> la réception de l'objet par son nom. Cette fonction n'est pas, en général, fait
> la recherche. Seulement si le tag> seront recherchés à partir de la
> les objets. Si vous utilisez _ () sur un objet inexistant ou indisponible, vous allez
> obtenez une erreur!

## Les autres fonctions de la bibliothèque standard

Dans la stdlib module à la PLACE, qui toujours se connecte automatiquement défini le
les fonctions offertes par l'auteur comme le principal outil de travail pour travailler avec les
jeu monde. Laissez-nous prendre en compte dans ce Chapitre.

Dans la description des fonctions la plupart des fonctions en vertu du paramètre
"w" se réfère à un objet ou d'une salle, spécifié par un nom, d'une étiquette ou de liaison variable. [ wh
] - indique un paramètre facultatif.

- inclure(fichier) fichier à inclure dans le jeu;
 inclure "lib" -- va inclure le fichier lib.lua à partir du répertoire courant avec le jeu;

- loadmod(module) pour connecter le module du jeu;

 loadmod "module" -- comprendra le module.lua à partir du répertoire courant;
- rnd(m) - une valeur entière aléatoire entre 1 et 'm';
- rnd(a, b) - un nombre entier aléatoire de la valeur de 'a' à 'b', où 'a'
 et 'b' sont des entiers >= 0;
- rnd\_seed () - mettre la graine du générateur de nombres aléatoires;
- p (...) en sortie une chaîne de caractères dans la mémoire tampon de gestionnaire ou de l'attribut (avec un espace à la
fin);
- pr (...) en sortie une chaîne de caractères dans la mémoire tampon de gestionnaire ou de l'attribut "comme est";- pn (...) 
 sortie une chaîne de caractères dans la mémoire tampon de gestionnaire ou de l'attribut (avec un saut de ligne à la fin);
- pf(fmt, ...) - sortie de la chaîne mise en forme de la mémoire tampon de gestionnaire ou de l'attribut;

 local de texte = 'bonjour';
 pf("Chaîne de caractères: %q: %d\n", texte, 10);

- pfn(...)(...)... "ligne" - la création d'un simple gestionnaire; Cette fonctionnalité simplifie 
 la création de simples gestionnaires:

 act = pfn(à pied, 'salle de bains') "j'ai décidé d'aller à la salle de bain.";
 act = pfn(activer, '#transition") "j'ai remarqué un trou dans le mur!";

- obj {} - créer un objet;
- stat {} - créer état;
- salle de {} - créer une pièce;
- menu {} - créer un menu;- dlg {} - créer un dialogue;
- moi() - renvoie le lecteur;
- ici() - retourne la scène actuelle.
- à partir de([w]) - retourne la pièce à partir de laquelle la transition actuelle de votre scène.
- nouveau(constructeur, arguments) - crée un nouveau dinamicheskogo objet (pour être 
 décrit plus loin);
- supprimer(w) - supprime la dynamique de l'objet;
- gamefile(fichier, [réinitialiser?]) - charger dynamiquement le fichier avec le jeu;

 gamefile("part2.lua", true) -- reset le jeu de l'etat (suppression de
 les objets et les variables), la charge part2.lua, et de commencer par la pièce principale.

- joueur {} - créer un joueur;- dprint(...) - la sortie de débogage;
- visites([w]) - le nombre de visites à la salle de bain (ou 0 si les visites);
- visité([w]) - le nombre de visites à la chambre, ou false si pas de visites;

 si non visitées() puis
 p [[c'est ma première fois.]]
fin

- marche(w, [Boolean exit], [enter Boolean], [Boolean pour changer de]) - 
 la transition de la scène;

 -- saut inconditionnel (ignorer onexit/le onenter/sortie/entrée);
 à pied('end', false, false)

- walkin(w) est une transition dans la scène (sans faire appel à exit/onexit actuelle);
- débrayage([w], [dofrom]) - retour de la sous-scène (sans appel
 entrer/le onenter);
- walkback([w]) - un synonyme de débrayage([w], false);
- \_(w) - la réception de l'objet;
- for_all(fn, ....) - exécuter la fonction pour tous les arguments;

 for_all(activer, 'fenêtre', 'porte');

- vu(w, [où]) - recherche de l'objet visible;
- recherche(w, [où]) est un objet de recherche;
- des moyens([où]) - obtenir la liste de transitions;
- objs([où]) - obtenir la liste des objets;
- la recherche(w) - recherche pour le joueur de l'objet;
- avez(w) - rechercher des éléments dans l'inventaire;
- un coffre(w) - le retour de la chambre/les chambres, dans lequel réside l'objet;
- où(w, [table]) - de retour de l'objet/les objets dont l'objet réside;

 liste locale = {}
 local w = où('Pomme', liste)
 -- si la Pomme est dans plus d'un endroit, puis liste contiendra une 
 -- tableau de ces lieux. Si vous avez seulement besoin d'un seul endroit, puis:
 où 'Apple' -- sera suffisant

- fermé(w) - vrai si l'objet est fermé;
- désactivé(w) - vrai si l'objet est éteint;
- activer(w) est à inclure un objet;
- désactiver(w) - off de l'objet;
- ouvrir(w) - ouvert;
- fermer(w) - proche de l'objet;
- actions(w, string, [valeur]) - retourne (ou ensembles)
 le nombre d'actions de type t à un objet w. 

 si les actions(w, 'tak') > 0 then -- objet w a été pris au moins 1 heure;
 si les actions(w) == 1 then -- loi sur w a été appelé 1 fois;

- pop(tag) - pour revenir à la dernière branche de la boîte de dialogue;
- pousser(tag) - la transition vers la prochaine branche de dialogue
- vide([w]) - vide de la branche droite du dialogue? (ou l'objet)
- lifeon(w) - ajouter un objet à la liste de la vie;
- lifeoff(w) - pour supprimer un objet de la liste des vivants;
- live(w) - objet est vivant?;
- modifier\_pl(w) - changement d'un joueur;
- joueur\_moved([pl]) est l'actuel joueur déplacé de cette manière?;
- inv([pl]) - obtenir une liste de l'inventaire;
- supprimer(l, [wh]) - supprimer l'objet de l'objet ou de la pièce; Supprime
 objet obj de la liste et de façon (en laissant tout le reste, par exemple, de jeu.mortes);
- purge(w) - destruction de l'objet (à partir de toutes les listes); Supprime l'objet de
 Vseh les listes dans lesquelles il est présent;
- remplacer(w, ww, [wh]) consiste à remplacer un objet à un autre;
place(w, [wh]) est de mettre l'objet dans l'objet/chambre (suppression de
 l'ancien objet/chambres);
- mettre(w, [wh]) - mettre l'objet sans le retirer de l'ancien emplacement;
- prendre(w) - ramasser un objet;
- déposer(w, [wh]) - de jeter de l'objet;
- chemin {} - créer une transition;
- le temps () est le nombre de déplacements depuis le début du jeu.

__Important!__

En fait, beaucoup de ces fonctions sont également en mesure de travailler non seulement avec des chambres et des
des installations, mais aussi avec des listes. Qui est, " supprimer(apple inv())' fonctionne comme
'supprimer(apple, moi())"; Toutefois, supprimer(apple) aussi le travail et la suppression de l'objet
à partir des lieux où il est présent.

Considérons quelques exemples.

``
loi = function()
 pn: "je vais à la chambre d'à côté..."
 à pied (nextroom);
fin

obj {
 nam = "ma voiture";
 dsc = 'en Face de la cabine est mon ancien {ramassage} Toyota.';
 act = fonction(s)
 marcher inmycar';
fin
};
``

__Important!__

> Après un appel à "marcher" l'exécution du gestionnaire d'continue jusqu'à ce qu'il
> terminée. Donc, habituellement, après la "marche" est toujours suivi
> "retour", sauf si c'est la dernière ligne de la fonction, bien que dans
> ce cas, il est prudent de faire "retour".

``
loi = function()
 pn: "je vais à la chambre d'à côté..."
 à pied (nextroom);
de retour
fin
``

Ne pas oublier également que lorsque vous appelez "marcher" seront appelés gestionnaires
'onexit/le onenter/sortie/entrée" et si ils interdisent la transition, il n'est pas
va se passer.

## Dialogue

Dialogues-la scène d'un type spécial 'dlg" contenant des objets --phrase. Lorsque
entrer dans le dialogue que le joueur voit une liste des expressions qui peuvent choisir,
obtenir une sorte de jeu de la réaction. La valeur par défaut est déjà sélectionné expression
caché. L'épuisement de toutes les options, la conversation se termine à la sortie de la
salle précédente (bien sûr, si dans le dialogue, il est constamment visible
des phrases, qui se produit généralement quelque chose comme "Terminer la conversation" ou " Demander
une fois de plus'). En entrant de nouveau dans la boîte de dialogue, tous l'expression cachée à nouveau devenir
visible et le dialogue est remis à l'état initial (à moins, bien sûr, la
auteur du jeu spécialement faire un effort pour changer la forme d'un
le dialogue).

La transition dans le dialogue du jeu est de savoir comment le passage à la scène:

``
obj {
 nam = "cook";
 dsc = 'je vois un {cuire}.';
 loi = function()
 marcher povardlg'
fin
};
``

Mais je recommande d'utiliser 'walkin', comme dans le cas de "walkin' a pas appelé
'onexit/sortie de" la salle de cours, et le personnage avec qui on peut parler,
habituellement, pour être dans la même pièce, où le héros principal. C'est:

``
obj {
 nam = "cook";
 dsc = 'je vois un {cuire}.';
 loi = function()
 walkin 'povardlg'
fin
};
``

Si vous n'aimez pas le préfixe de phrases sous la forme d'un trait d'union, vous pouvez spécifier une variable de type string:

 std.phrase_prefix = '+';

Et obtenir un préfixe '+' devant chaque phrase. Vous pouvez également faire un préfixe
fonction. La fonction, dans ce cas, sera à entrer un paramètre le nombre de
le membre de phrase. Le but de la fonction) pour renvoyer une chaîne de préfixe.

Veuillez noter que la " std.phrase_prefix' n'est pas enregistré si vous vous avez besoin de remplacer
à la volée, vous devez restaurer l'état dans des "start ()" fonction
manuellement!

__Important!__

> Je vous recommande d'utiliser le module 'noinv" et le "noinv" dans les boîtes de dialogue. L'
> boîtes de dialogue va regarder de plus belle et vous permettra de protéger votre jeu à partir de
> des erreurs et des réactions inattendues lors de l'utilisation de l'équipement à l'intérieur de la boîte de dialogue (comme
> d'habitude, l'auteur n'implique pas de telles choses). Par exemple:

``
exiger "noinv"
...
dlg {
 nam = 'Garde';
 -- dans les dialogues ne nécessitent généralement pas d'inventaire
 noinv = true;
...
}
``

### Expression

Le concept Central dans le dialogue est l'expression. L'expression n'est pas juste la question-
la réponse que vous pourriez le penser. La phrase est un arbre, et, dans le sens de la
tout le dialogue peut être mis en œuvre que de la phrase. Par exemple:

``
dlg {
 nam = 'conversation';
 titre = [[la Conversation avec le vendeur]];
 entrée = [[j'ai demandé au vendeur.]];
 phr = {
 {"Vous avez des haricots?', '-- No.'},
 {"Vous avez du chocolat?', '-- No.'},
 {"Vous avez une infusion?', 'Oui',
 {"Combien vaut-il?', '-- 50 roubles.' },
 { 'Il fait froid?', '-- Réfrigérateur a été brisé.',
 {"Prendre les deux!', 'Gauche.',
 {"Donnez-moi un seul!', function() p [[OK!]]; prendre 'brew'; à la fin };
}
}
}
}
}
``

Comme vous pouvez le voir, la phrase est spécifié par l'attribut de la phr et peut contenir
ramifiée dialogue. La phrase contient des élections, chaque de ce qui peut également
contenir des choix et ainsi de suite...

L'expression est dans le format de paires: descripteur: réaction. Dans le plus simple
cas, c'est un de ligne. Mais ils peuvent aussi avoir des fonctions. Généralement, la fonction est
la réaction, qui peut contenir du code de changer le monde du jeu.

La vapeur peut être aussi simple que:

 {'Question', 'Réponse }

Et peut contenir un tableau de paires:

 {'Question', 'Answer',
 {"Sous-question1', 'Sous-answer1' },
 {"Sous-question2', 'Sous-answer2' },
}

En fait, si vous regardez attentivement, à l'attribut de la phr, vous remarquez que le tableau
de choix est également incorporé dans la peine principale phr, mais seulement l'original
paire est manquant:

``
dlg {
 nam = "conversation"; title = [[la Conversation avec le vendeur]];
 entrée = [[j'ai demandé au vendeur.]];
 phr = {
 -- il pourrait être question réponse 1 niveau!
 -- 'Le problème principal', 'Main',
 {"Vous avez des haricots?', '-- No.'},
 {"Vous avez du chocolat?', '-- No.'},
 {"Vous avez une infusion?', 'Oui',
 {"Combien vaut-il?', '-- 50 roubles.' },
 { 'Il fait froid?', '-- Réfrigérateur a été brisé.',
 {"Prendre les deux!', 'Gauche.',
 {"Donnez-moi un seul!', function() p [[OK!]]; prendre 'brew'; à la fin };
}
}
}
}
}
``

En fait, il est. Et vous pouvez ajouter la Principale question " et "La réponse", mais vous
ne verrez pas cette question. La chose que lors de l'entrée en dialogue dans la phrase
phr s'ouvre automatiquement, donc comme d'habitude il n'y a pas de point dans les dialogues d'un
seule phrase. Et il est beaucoup plus facile de comprendre le dialogue comme un ensemble de
élection que comme le seul arbre de la phrase. Donc, il n'y a jamais une phr de la première
paire question-réponse, mais nous immédiatement nous nous trouvons dans le tableau de
des options plus claires.

Lorsque nous parlons de ce que le dialogue réellement mis en œuvre une phrase, nous sommes
pas tout à fait droit. Le fait que nous sommes face à la phrase,qui est situé
à l'intérieur de l'autre phrase... Elle nous rappelle la situation des objets. En effet,
les expressions sont des objets! Qui peut être à l'intérieur les uns des autres. Alors, jetez un oeil à
le dialogue à partir d'une nouvelle perspective:

``
dlg {
 nam = 'conversation';
 titre = [[la Conversation avec le vendeur]];
 entrée = [[j'ai demandé au vendeur.]];
 phr = { -- est une phrase sans dsc et de la loi sur les
 -- est la 1ère phrase à l'intérieur de la phrase avec l'asn et l'acte
 {"Vous avez des haricots?', '-- No.'},
 --est la 2ème phrase dans une phrase avec l'asn et l'acte
 {"Vous avez du chocolat?', '-- No.'},
 - c'est une 3ème phrase dans une phrase avec l'asn et l'acte
 {"Vous avez une infusion?', 'Oui',
 -- est la 1ère phrase à l'intérieur de la 3ème phrase avec l'asn et l'acte
 {"Combien vaut-il?', '-- 50 roubles.' },
 { 'Il fait froid?', '-- Réfrigérateur a été brisé.',
 {"Prendre les deux!', 'Gauche.',
 -- agissent ici comme une fonction
 {"Donnez-moi un seul!', function() p [[OK!]]; prendre 'brew'; à la fin };
}
}
}
}
}
``

Comme vous pouvez le voir, le dialogue -- c'est une pièce, la partie de la phrase, des objets spéciaux! Maintenant
vous comprendrez de la présentation ultérieure.

> Attention! Par défaut, lorsque le joueur clique sur une des questions de l'
> liste, le moteur se répète dans la conclusion, puis affiche la réponse.
> C'est fait pour s'assurer que le dialogue semble lié. Si vous voulez
> désactiver ce comportement, utilisez l'option std.phrase_show:

``
std.phrase_show = false -- ne pas afficher le mot de passe multiterme question lors du choix d'
``

Ce paramètre affecte tous les dialogues, la mettre dans l'init() ou la fonction start ().

### Les attributs des phrases

Considérons la phrase:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
}
}
``

Si vous exécutez cette boîte de dialogue, après que vous sélectionnez, pour ainsi dire, rouge pilules, nous allons avoir un autre
le choix de la pilule bleue. Mais notre idée, évidemment pas à cette! Il y a
plusieurs façons de faire du dialogue correct.

Tout d'abord, vous pouvez utiliser le protocole pop() -- le retour à la précédente, le niveau de dialogue:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 {'Rouge', function() p 'Tenir!'; pop() end; },
 {'Bleu', function() p 'Ici!'; pop() end; },
}
}
``

Ou, dans un autre billet:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 {"Rouge", pfn(pop) " Hold up!' },
 {'Bleu', pfn(pop) 'Ici!' },
}
}
``

Mais il n'est pas trop commode, en outre, que si ces phrases contiennent une nouvelle
une phrase? Dans le cas où l'option offre un choix, et ce choix doit
la seule chose que vous pouvez demander à partir de l'expression de l'attribut uniquement:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
}
}
``

Dans ce cas, après le choix de l'expression, tous les mots de l'actuel
contexte sera fermé.

Une autre commune de la situation, vous voulez que la phrase n'était pas caché après son
l'activation. Ceci est fait par la mise en drapeau de vrai:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
 { true, 'Et qui est le meilleur?', 'Vous choisissez.' }, -- une expression
 -- qui ne sera jamais caché
}
}
``

Une autre notation, avec pour mission explicite de l'attribut toujours:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
 { toujours = true, 'Et qui est le meilleur?', 'Vous choisissez.' }, -- une expression
 -- qui ne sera jamais caché
}
}
``

Un autre exemple. Que faire si nous voulons que la phrase a été présenté(ou caché) sur toute
condition? C'est la fonction de gestionnaire cond.

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
 { true, 'Et qui est le meilleur?', 'Vous choisissez.' }, -- une expression
 -- qui ne sera jamais caché
},
 { cond = function() rendement de l'avoir de "Pomme" fin
 "Voulez-vous une Pomme?', "Merci, non". };
}
``

Dans cet exemple, seulement quand le joueur a une Pomme, semblent direction de la boîte de dialogue " avez-vous
veux une Pomme?'.

Il est parfois pratique pour effectuer une action au moment où les options
niveau actuel(contexte) de la dialogue est épuisé. À cette fin, l'
fonction de gestionnaire onempty.

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
 onempty = function()
 p [[Vous avez fait votre choix.]]
pop()
fin;
},
 { cond = function() rendement de l'avoir de "Pomme" fin
 "Voulez-vous une Pomme?', "Merci, non". };
}
``

Veuillez noter que lorsqu'il y a une méthode onempty, retour automatique à la
la branche précédents n'est pas effectuée, il est supposé que la méthode onempty va
faire tout ce dont vous avez besoin.

Tous les attributs ci-dessus peut être réglé à n'importe quelle phrase. En fait, au 1er niveau:

``
phr = {
 onempty = function()
 p [[fin de la conversation.]]
débrayage()
fin;
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!' },
 {'Bleu', 'Ici!' },
 onempty = function()
 p [[Vous avez fait votre choix.]]
pop()
fin;
},
 { cond = function() rendement de l'avoir de "Pomme" fin
 "Voulez-vous une Pomme?', "Merci, non". };
}
``

### Tags

Nous avons examiné les mécanismes de dialogues, qui permettent déjà de créer assez
complexe des boîtes de dialogue. Toutefois, ces fonds peuvent ne pas être assez. Parfois, nous avons besoin de
pouvoir se référer à des phrases à partir d'autres endroits le dialogue. Par exemple, pour
activer de manière sélective, ou de les analyser de l'état. Et de rendre les transitions de l'un
des branches de dialogue dans d'autres.

Tout cela est possible pour les phrases qui ont le tag. Créer une phrase avec un tag
très simple:

``
phr = {
 {'#?', 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 {'#rouge', 'Rouge', 'Tenir!' },
 {'#bleu', 'Bleu', 'Ici!' },
},
}
``

Comme vous pouvez le voir, la présence au début de la phrase chaîne, en commençant
avec le symbole '#' - indique la présence de la balise.

Pour de telles phrases emploie des méthodes standard, tels que la vue ou l'activer/la désactiver. Pour
exemple, nous pourrions le faire sans l'attribut uniquement dans les cas suivants:

``
phr = {
 {'#?', 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 {'#rouge', 'Rouge', 'Tenir!'
 cond = fonction(s)
 de retour n'est pas fermée('#bleu')
fin
},
 {'#bleu', 'Bleu', 'Ici!',
 cond = fonction(s)
 de retour n'est pas fermée('#rouge')
fin
},
},
}
``

Les balises, sauf que vous permettre d'apprendre et de changer le statut d'un particulier
des phrases, ce qui rend possible les transitions entre les phrases. À cette fin, l'
les fonctions push et pop.

push(de) - permet le passage de la phrase au sujet de se souvenir de la position
dans la pile.

pop([où]) -- appelée sans paramètre, remonte en position 1 dans le
pile de l'histoire. Vous pouvez spécifier une balise spécifique phrase qui doit être dans l'histoire,
dans ce cas, le remboursement sera crédité sur son.

Il convient de noter que lorsque vous cliquez sur une commande, nous nous déplaçons pas un de la phrase,
et la liste de mots de la phrase. C'est, divulguer, ainsi que cela se fait
pour la principale expression de la phr. Par exemple:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!', next = '#aboutpill' },
 { 'Bleu', 'Ici!', next = '#aboutpill' },
},
 { false, '#aboutpill',
 {'Ai fait le bon choix?', "Le temps nous le dira.'}
},
}
``

Ici, nous voyons plusieurs techniques:

- attribut suivant, au lieu de descriptions explicites de réaction en fonction
avec push. la prochaine est une manière simple pour enregistrer pousser.

- faux au début de la phrase rend la phrase off. Elle est verrouillée
jusqu'à ce que vous n'explicite de l'activer. Cependant, à l'intérieur de la phrase, nous pouvons aller et de montrer
le contenu des élections. D'entrée Alternative possible à l'utilisation de la
attribut caché:

 { hidden = true, '#aboutpill',
 {'Ai fait le bon choix?', "Le temps nous le dira.'}
},

Ainsi, il est possible d'enregistrer des conversations n'est pas un arbre, et linéaire. Plus
une des caractéristiques des transitions, c'est que si la phrase n'est pas décrit à la
réaction, lorsque la transition est déclenchée par la phrase titre:

``
phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!', next = '#aboutpill' },
 { 'Bleu', 'Ici!', next = '#aboutpill' },
},
 { false, '#aboutpill', [[j'ai pris la pilule et le magicien sourit sournoisement.]],
 {'Ai fait le bon choix?', "Le temps nous le dira.'},
 {'Quoi?', "Vous êtes libre.'},
},
}
``

Lors du choix d'une tablette, ce qui sera appelé la méthode d'en-tête de phrase '#aboutpill',
puis sera présenté choix.

Si vous aimez linéaire, vous pourriez préférer l'option suivante:

``
dlg {
 nam = 'dialogue';
 phr = {
 { 'Qu'avez-vous?', "Des pilules. Le rouge et le bleu. Vous avez quoi?",
 seulement = true,
 {"Rouge", " Hold!', next = '#aboutpill' },
 { 'Bleu', 'Ici!', next = '#aboutpill' },
}
}
}: avec {
 { '#aboutpill', [[j'ai pris la pilule et le magicien sourit sournoisement.]],
 {'Ai fait le bon choix?', "Le temps nous le dira.'},
 {'Quoi?', "Vous êtes libre.'},
},
}
``

Le fait que l'attribut de la phr définit le premier objet de la pièce. Mais vous
peut combler les objets de l'espace de la manière habituelle: par la mise en obj ou avec. Depuis
entrer dans le dialogue révèle la 1ère phrase, puis le reste de la phrase, vous
pas voir (attention à la phrase "#aboutpill' n'en vaut pas itfalse), mais vous
sera en mesure de faire des transitions sur ces phrases.

### Méthodes

Comme vous le savez déjà, les objets d'une PLACE peut être en mesure d'ouvrir/fermé/activé
sur. Cela correspond à la phrase de dialogue?

Pour les expressions courantes, après l'activation du choix de la phrase _closes_. Lorsque
re-entrer dans le dialogue, les phrases obtenir _opened_.

Pour les phrases avec toujours = true (vrai ou au début de la définition) --
cette clôture n'a pas lieu.

Pour les phrases avec hidden = true (ou faux au début de la définition)
-- la phrase va être créé comme désactivé. Il ne sera pas visible jusqu'à ce que
est explicitement permis.

Pour les phrases avec cond(), chaque fois que vous naviguez sur des phrases est appelé cette méthode,
et en fonction de la valeur de retour (vrai/faux) phrase s'active ou se désactive.

Sachant ce comportement, vous pouvez masquer/afficher et d'analyser la phrase standard
fonctions: désactiver / activer / vide / ouvrir / fermer / fermeture / désactivé et donc
sur...

Cependant, pour ce faire, vous pouvez seulement dans le dialogue, comme toutes les phrases
faire identifier par des balises. Si vous souhaitez modifier la condition/analyser des phrases à partir de
les autres chambres, vous pouvez:

- donner l'expression nom est { nam = 'nom' }...
- recherche de la phrase sur la balise dans l'autre pièce: local ph = lookup('#tag',
 "dialogue"), et ensuite travailler avec elle;

À l'égard des fonctions de push/pop, vous pouvez l'appeler explicitement que les méthodes
de dialogue, par exemple:

 _'dialogue':push '#new'

Mais il vaut mieux le faire dans le dialogue, par exemple, dans l'entrée.

En outre, il existe une méthode :reset, ce qui remet la pile et définit les
de départ de la phrase, par exemple:

 entrée = fonction(s)
 s:reset "#start"
fin

> Il est à noter que lorsque vous faites un activer/désactiver/ouvrir/fermer la phrase,
> puis vous effectuez l'action exactement au-dessus de cette phrase, pas plus de phrases
> inclus à l'intérieur. Mais depuis le montrant phrases d'arrêt du moteur sur marche/arrêt / fermé
> facilité d'expression et ne sera pas inclus à l'intérieur, c'est assez.

## Objets spéciaux

Dans STEAD3 il y a des objets qui exécutent des fonctions spécifiques. Tous ces
les objets peuvent être divisés en deux classes:

1. Les objets du système de @;
2. La Substitution.

Système d'objets sont des objets dont le nom commence par le caractère '@' ou '$'.
De tels objets sont généralement modulaire. Ils ne sont pas détruits à la mort de la
monde du jeu (par exemple, lors du téléchargement du fichier de jeu, lors du chargement du jeu de
enregistrer, et ainsi de suite). Exemples d'objets: @minuterie, @préf., @snd.

Ces objets, en plus de leurs fonctions spéciales, vous pouvez utiliser le lien
sans explicitement de mettre l'objet sur la scène ou de l'action, mais la
mécanisme d'action de ces objets sont spéciales.

### L'objet '@'

Généralement, vous n'avez pas besoin de travailler avec de tels objets, mais à titre d'exemple, envisager de
la mise en œuvre de "liens".

Supposons que l'on souhaite faire un lien, en cliquant sur lequel nous allons passer dans l'autre
chambre. Bien sûr, on pourrait ajouter l'objet à la scène, mais il vaut la peine de nous le faire
dans un tel cas?

Comment on peut aider le système un objet?

``
obj {
 nam = '@marcher";
 act = fonction(s, w)
 marche(w, false, false)
fin;
}
chambre {
 nam = "principal";
 title = 'Accueil';
 décor = [[Démarrer {@commencer à marcher|aventure}]];
}
``

Lorsque vous cliquez sur le lien "aventure" méthode sera appelée loi-objet
'@marcher " avec le paramètre "start".

En fait, dans le standard de la bibliothèque stdlib est déjà l'objet de nom de fichier avec
'@', qui vous permet de faire vos gestionnaires les liens suivants suit:

``
xact.pied = marche

chambre {
 nam = "principal";
 title = 'Accueil';
 décor = [[Démarrer {@ commencer à marcher|aventure}]];
}
``

Remarque l'espace après le @. Cette entrée est le suivant:

- prend un objet par " @ " (cet objet est créé par la bibliothèque stdlib);
- prend sa loi;
- causes de loi avec les paramètres de la marche et de départ;
- loi sur l'objet de la '@' regarde le tableau xact;
- pied de détection d'une méthode qui sera appelée à partir de la matrice de xact;
- start est un paramètre de cette méthode.

Un autre exemple:

``
xact.myprint = function(w)
 p (w)
fin

chambre {
 nam = "principal";
 title = 'Accueil';
 décor = [[Push {@ myprint "hello world"|le bouton}]];
}
``

### Recherche

Les objets dont le nom commence par le caractère'$', sont également considérés comme
le système des objets, mais ils fonctionnent différemment.

Si le texte de sortie se trouve sur le lien:

 {$ma a b c|texte}

Le suivant se produit:

1. L'objet $mon;
2. Pris acte de l'objet $mon;
3. Loi est appelée: _'$ma'(a, b, c, texte);
4. La chaîne renvoyée remplace la totalité de la structure {...}.

Ainsi, les objets jouent le rôle d'un joker.

Pourquoi est-il nécessaire? Imaginez que vous avez développé un module, qui tourne à écrire
à partir d'un texte à afficher dans le graphique. Vous écrivez objet $de mathématiques qui, dans son acte
méthode convertit le texte d'une image graphique (sprite) et le renvoie dans le texte
flux de données. Ensuite, pour utiliser ce module très simple exemple:

{$math|(2+3*x)/y^2}

## Événements dynamiques

Vous pouvez définir des gestionnaires d'exécuter à chaque fois que le temps de jeu par incréments de
un. Habituellement, il a un sens pour la vie des personnages, ou de l'arrière-plan
les processus du jeu. L'algorithme de l'étape jeux ressemble à ceci:
 - Le joueur clique sur le lien;
 - Réaction, "loi", "utiliser", 'inv', 'tak', l'inspection de la scène (cliquez sur 
 sur le nom de scène) ou passer à une autre scène;
 - Dynamique des événements;
 - Sortie d'un nouvel état de la scène.

Par exemple, faire un léopard des neiges vivant:

``
obj {
 nam = 'Barsik';
 {- ne pas enregistrer le tableau lf
 lf = {
 [1] = 'Barsik est en mouvement dans mon sein.',
 [2] = 'Barsik semble hors de sa poitrine.',
 [3] = 'Barsik ronronner dans mon sein.',
 [4] = 'Barsik des frissons dans mon sein.',
 [5] = 'je me sens Barsik de la chaleur dans sa poitrine.',
 [6] = 'Barsik colle sa tête hors de sa poche et regarde autour de lui.',
};
};
 la vie = fonction(s)
 local r = rnd(5);
 si r > 2 then -- ce n'est pas toujours
retour;
fin
 r = rnd (s#.lf); -- le symbole # est le nombre d'éléments dans le tableau
 p(s).lf[ r]); -- dériver l'un des 6 Membres de la panthère des neiges
fin;
....
``

Et voici le point dans le jeu, lorsque le léopard des neiges se fait dans ton sein.

 prendre "snow leopard" -- l'ajouter à votre inventaire
 lifeon 'Barsik' -- pour relancer le chat!

N'importe quel objet (y compris l'étape) peut avoir son propre gestionnaire, "la vie", appelée à chaque
tique de la partie si l'objet a été ajouté à la liste des objets en direct à l'aide de la
'lifeon'. N'oubliez pas de supprimer des objets en direct à partir de la liste à l'aide de la
'lifeoff" quand ils ne sont plus nécessaires. Il est possible de le faire, pour
exemple, dans le gestionnaire de "sortie" ou par toute autre méthode.

> Si votre jeu est beaucoup de "vivre" des objets, vous pouvez les poser
> une position claire dans la liste, en ajoutant. Pour cela, utilisez
> deuxième paramètre numérique (entier non négatif) 'lifeon',
> plus le chiffre est plus la priorité est élevée. 1-le plus haut. Ou vous
> vous pouvez utiliser l'attribut de la pri de l'objet. Cependant, cet attribut
> affectera la priorité de l'objet dans la liste.

Si vous avez besoin d'un processus d'arrière-plan dans certains chambre, et de l'envoyer en "entrée" et de supprimer les
la "sortie", par exemple:

``
chambre {
 nam = 'Dans le sous-sol;
 dsc = [[il fait sombre Ici!]];
 entrée = fonction(s)
lifeon(s);
fin;
 sortie = fonction(s)
lifeoff(s);
fin;
 la vie = fonction(s)
 si rnd(10) > 8 alors
 p [[j'ai entendu quelque chose bruissement!]];
 -- de temps en temps pour faire fuir le lecteur fait du bruit
fin
fin;
 chemin = { 'Maison' };
}
``

Si vous avez besoin pour déterminer si le transfert du joueur à partir d'une scène à l'
un autre, l'utilisation de " joueur\_moved()'.

``
obj {
 nam = 'torche';
 sur = false;
 la vie = fonction(s)
 si player_moved() then -- mettre de la torche de transitions
 s.sur = false
 p "j'ai éteint la lampe de poche."
de retour
fin;
fin;
...
}
``

Pour suivre le déplacement d'événements, l'utilisation de 'time()' ou un auxiliaire de la variable compteur. Pour
déterminer l'emplacement joueur -- 'ici()'. Pour déterminer que l'objet est
"alive", - le live()'.

``
obj {
 nam = 'dynamite';
 timer = 0;
 utilisé = fonction(s, w)
 si w^'match' then -- match?
 si vivre(s), puis
 de retour "c'est Déjà allumé!"
fin
 p "j'ai allumé la dynamite."
lifeon(s)
de retour
fin
 retourne false si pas de match
fin;
 la vie = fonction(s)
 s.timer = s.minuterie + 1
 si s.timer == 5 alors
 lifeoff(s) si ici() == où(s), puis
 p [[la Dynamite a explosé juste à côté de moi!]]
d'autre
 p [[j'ai entendu une explosion quelque part.]];
fin
fin
fin;
...
}
``

Si la " vie " gestionnaire retourne le texte de l'événement, il est imprimé après la description
de la scène.

Vous pouvez retourner à partir d'un gestionnaire pour la " vie " deuxième code de retour ('true' ou
"faux"). Si vous retournez vrai-c'est un signe important de l'événement, qui sera
affiche pour décrire les objets dans la scène, par exemple:

 p 'entré de la garde.'
 return true

Ou:

 retour " que vous avez entré de la chambre, le garde de sécurité.', vrai

Si vous retourne la valeur false, la chaîne de la vie de méthodes échoue sur vous. Il est facile à faire
lorsque vous effectuez une marche de la vie, par exemple:

 la vie = function()
 marcher theend'
 return false -- c'est la dernière de la vie
fin

Si vous souhaitez bloquer la " vie " des gestionnaires dans certaines chambres, l'utilisation du module
'nolife'. Par exemple:

``
exiger "noinv"
besoin de "nolife"

dlg {
 nam = 'Garde';
 noinv = true;
 nolife = true;
...
}
``

Nous devons également tenir compte de la transition de l'acteur de "la vie" de gestionnaire. Si
vous allez utiliser la fonction "marche" à l'intérieur de'life', alors vous devriez
considérons le problème suivant.

Si la " vie " prend le lecteur à un nouvel emplacement, il est généralement supposé que vous:

1. Claire réactions de sevrage: jeu:réaction(false);
2. Nettoyer retrait vivre méthodes du moment: jeu:les événements(false, false)
3. Faire de la marche.
4. Arrêt de la chaîne de la vie de l'appel à l'aide return false;

Il faudrait éclaircir certains points.

jeu:réaction () - vous permet de prendre ou de modifier la sortie de la réaction
l'utilisateur, si défini à false, ce qui signifie pour réinitialiser la réaction.

jeu:les événements () -- permet de prendre/modification / annulation de la vie de méthodes. Inas
les choix sont faits en priorité et non pas un des critères de priorité faux nous avons annulé la
toute la production de la vie précédente méthodes.

La bibliothèque standard a une fonction life_walk(), ce qui rend le décrit
actions. Il suffit de le retourner false.

## Graphisme

Graphique interprète PLUTÔT qu'analyse l'attribut de la scène 'pic', voit
comme le chemin d'accès à l'image, par exemple:

 chambre {
 pic = " gfx/home.png';
 nam = 'Accueil';
 dsc = 'je suis à la maison';
};

__Important!__

Utiliser d'une manière directe '/'. Aussi, il est fortement recommandé d'utiliser l'
les noms de répertoires et de fichiers que le Latin caractères minuscules. De cette façon, vous
permettra de protéger votre jeu à partir de problèmes de compatibilité et il fonctionne sur tous les
des plates-formes architecturales où porté à la PLACE.

Bien sûr, 'pic' peut être une fonction, d'élargir les possibilités de la
développeur. Si la scène actuelle n'a pas défini l'attribut 'pic' de l'attribut
est prise de jeu.pic'. Si l'image n'est pas affichée.

Prend en charge tous les formats d'image courants, mais je vous recommande d'utiliser 'png' et
où () "jpg".

Vous pouvez utiliser que des images, des fichiers GIF animés. Assurez-vous qu'ils sont des fichiers GIF, et pas WEBP format ou format PNG, qui sont pas pris en charge.

Vous pouvez incorporer des images dans le texte de l'inventaire,
des transitions, des titres, des chambres et des " asn " à l'aide de la fonction 'esf.img' (cela
allumez le module de l'esf).

Par exemple:

``
exiger "fmt"

obj {
 nam = 'Pomme'
 disp = 'Pomme'..esf.img ("img/apple.png');
}
``

Que, au moins, la photo de la scène doit toujours être faite dans la forme de 'pic'
attribut, pas de l'insertion de l'esf.img' de 'dsc.

Le fait que l'image de la scène des échelles sur les autres algorithmes. Photos
'esf.img' obtenir mis à l'échelle en conformité avec les paramètres de la PLACE (l'échelle
thème), et le 'pic' -- prend également en compte la taille de l'image.

En outre, les images 'pic' ont d'autres propriétés, par exemple, la capacité
pour suivre les coordonnées de clics de souris.

Si vous mettez 'esf.img' à l'intérieur de { et }, vous obtenez un lien graphique.

`
obj {
 nam = 'Pomme';
 disp = 'Pomme' ..img ("img/apple.png');
 dsc = fonction(s)
 p ("le sol est {Apple",esf.img 'img/apple.png', "}");
 -- d'autres options:
 -- retour "Sur le plancher se situe {Apple"..de l'esf.img ("img/apple.png').."}";
 -- p "Sur le plancher se situe {Apple"..de l'esf.img ("img/apple.png').."}";
 -- ou dsc = "le sol est {Apple"..de l'esf.img ("img/apple.png').."}";
fin;
}
`

Au LIEU de cela soutient habillage des images avec du texte. Si l'image est insérée à l'aide de
la fonction 'esf.imgl'/'esf.imgr", il sera situé à gauche/à droite.

__Important!__

> Les Images insérées dans le texte à l'aide de 'esf.imgl/fmt.imgr' ne peut pas être
> les liens!!! Les utiliser uniquement à des fins décoratives.

Définir l'espacement autour d'une image, utilisez le " pad " exemple:

 -- indentation 16 de chaque bord
 fmt.imgl " pad:16,image.png'
 -- marges: haut 0, à droite 16, bas 16, à gauche 4
 fmt.imgl " pad:0 16 16 4,une image.png'
 -- marges: haut 0, à droite 16, en bas à 0, à gauche 16
 fmt.imgl " pad:0 16,une image.png'

Vous pouvez utiliser des pseudo-fichiers pour les images et les rectangles vides domaines:

``
dsc = fmt.img 'vide:32x32'..[[Ligne vide de l'image.]];
dsc = fmt.img 'de la boîte:32 x 32,rouge,128'..[[Ligne rouge semi-transparent carré.]];
``

PLUTÔT de processus composé d'images, par exemple:

 pic = " gfx/mycat.png;gfx/milk.png@120,25;gfx/fish.png@32,32';

Ainsi, l'image composite est un ensemble de chemins d'accès aux images, séparées par des ';'. L'
deuxième et les suivantes composants peuvent contenir des Postfix sous la forme
@x\coordonner,y\coordonner%, dont les coordonnées 0,0 correspond à gauche
le coin supérieur de l'image. La taille totale de l'image est considéré comme égal
le montant total de la première composante de l'image composite, qui est,
le premier composant (dans notre exemple-gfx/mycat.png) joue le rôle de la toile,
et d'autres composantes sont superposées sur cette toile.

La superposition est pour le coin supérieur gauche de la overlaypictures. Si vous avez besoin d'
pour superposer était le centre de la superposition d'images, utilisez avant de les coordonner
le préfixe "c", par exemple:

 pic = " gfx/galaxy.png;gfx/star.png@c128,132';

Ayant comme fonction la formation de la voie du composite photos, vous
peut générer une image en fonction de l'état de la partie.

Si vous êtes dans le jeu lié à toutes les coordonnées des images ou de leurs tailles, de le faire dans
rapport à l'original de la taille d'une image. Mise à l'échelle thème précis
playerthe résolution au LIEU de cela, il va convertir les coordonnées (avec le
coordonnées pour le jeu ressemble le jeu est en cours d'exécution sans mise à l'échelle).
Cependant, il peut y avoir une petite erreur de calcul.

Si vous n'avez pas assez de fonctionnalités décrites dans ce Chapitre, j'examine
le module "sprite", qui offre plus de possibilités pour la conception graphique. Mais
Je recommande fortement de ne pas le faire dans son le premier jeu.

## Musique

Pour travailler avec de la musique et de sons, vous aurez besoin du module snd.

 exiger "snd"

L'interprète joue dans le cycle de la musique actuelle qui est défini à l'aide de la fonction: 'snd.musique de musique(fichier)'.

__Important!__

Utiliser d'une manière directe '/'. Aussi, il est fortement il est recommandé d'utiliser l'
les noms de répertoires et de fichiers que le Latin caractères minuscules. De cette façon, vous
permettra de protéger votre jeu à partir de problèmes de compatibilité et il fonctionne sur tous les
des plates-formes architecturales où porté à la PLACE.

Prend en charge la plupart des formats de musique, mais il est fortement il est recommandé d'utiliser l'
format ogg " comme il est pris en charge de la meilleure façon dans toutes les versions, à la PLACE (pour
différentes plates-formes).

__Important!__

La prudence devrait être exercée lors de l'utilisation du tracker musique comme certains Linux
les distributions peuvent être un problème lors de la lecture de certains fichiers (erreur dans le bundle
les bibliothèques SDL_mixer et libmikmod).

Aussi, si vous utilisez le 'milieu' des fichiers, être préparé pour le fait que le joueur
les entendrez uniquement dans la version Windows à la PLACE (comme dans la plupart des cas, Unix
la version de SDL_mixer compilé sans le support "timidité").

Comme la fréquence des fichiers de musique, utiliser une fréquence multiple de 11025.

``
chambre {
 pic = " gfx/rue.png';
 entrée = function()
 snd.de la musique de mus et de la pluie.ogg'
fin;
 nam = 'dans la rue';
 dsc = 'il pleut à l'extérieur.';
};
``

'snd.la musique () sans paramètre renvoie le nom de la piste.

Dans la fonction " snd.(musique)", vous pouvez passer à la deuxième paramètre est le nombre de diffusion
(les cycles). Pour obtenir le compteur en cours vous pouvez utiliser 'snd.la musique()' n'a pas de paramètres
-- la deuxième valeur de retour. 0 -- c'est le cycle éternel. 1..n-le nombre de
play-backs. -1 -- la lecture de la piste en cours est terminé.

Pour annuler la musique, vous pouvez utiliser 'snd.stop\_music()'

Afin de savoir si la musique est:

snd.music_playing()

Vous pouvez régler le temps de montée et de l'atténuation de la musique, en appelant:

 snd.music_fading(s [i])

Ici o est le temps en millisecondes. pour l'atténuation et je est le temps dans
millisecondes. une hausse de la musique. Si vous spécifiez un seul paramètre -- les deux fois
sont considérés comme identiques. Après l'appel, les paramètres s'appliquent à la lecture de
tous les fichiers de musique.

Pour la lecture des sons à l'aide de snd.play()'. Très il est recommandé d'utiliser l'
format ogg", bien que la plupart des communes de formats audio fonctionnera également.

La distinction entre la musique et de fichiers son, c'est que le moteur de la surveillance de la
processus de la lecture de la musique et sauve/restaure actuellement joué piste.
Quitter le jeu et de le télécharger encore une fois, le joueur entendre la même musique que
vous avez entendu lorsque vous quittez. Sons indiquent en général des effets à court terme, et la
moteur de ne pas stocker ou restaure le son des événements. Ainsi, si un joueur n'a pas
le temps d'écouter le son de la prise de vue et à gauche du jeu, après le téléchargement de l'
fichier, enregistrez-il pas entendre le son (ou la fin) de nouveau.

Cependant, si vous considérez le fait que " snd.play()' permet d'exécuter en boucle
les sons, la différence entre la musique et le son devient pas aussi clairs.

Ainsi, la définition de la fonction: 'snd.jouer(fichier [canal], [boucle])', où:

- fichier-chemin et / ou du nom du fichier audio;
- canal -- numéro de canal [0..7]; Si non spécifié, obtenir gratuit.
- cycle -- le nombre de lectures 1..n, 0 -- boucle.

Pour arrêter la lecture, vous pouvez utiliser 'snd.stop()'. Pour arrêter le son dans un canal donné
'snd.stop(canal)'.

__Important!__

> Si vous utilisez la boucle des sons, vous devrez réinitialiser leur état (courir à nouveau
> avec " snd.le son (la)') dans la fonction " start()'

Par exemple:

``
global 'wind_blow' (false)
...
la fonction start()
 si wind_blow alors
 snd.jouer('snd/vent.ogg', 0)
fin
fin
``

Si vous n'êtes pas assez décrit ici les fonctions pour travailler avec la bonne utilisation de l'
description complète du module "snd".

## La mise en forme de la sortie

Habituellement, au LIEU des offres avec mise en forme et la conception de sortie. Par exemple, l'
étape sépare les parasites de la dynamique. Faits saillants de les mettre en italique les actions de la
joueur. Déplace le focus vers le changement dans le texte et etc des Modules comme "fmt"
améliorer la qualité de la sortie du jeu, sans effort supplémentaire de la part
de l'auteur.

Par exemple:

 require 'fmt'
 fmt.para = true -- enable indentation des paragraphes

Et votre jeu sera beaucoup mieux. Si vous avez besoin d'un traitement automatisé de
le texte affiché à l'écran, vous pouvez activer le module "fmt" et définir la fonction
'esf.filtre". Par exemple:

``
exiger "fmt"
fmt.filtre = fonction(s, état)
 -- s -- sortie
 -- state -- true si c'est le temps de jeu (étage de sortie)
 si l'état alors
 retour "de Cette chaîne sera ajoutée au début de sortie\n'..s;
fin
 de retour s
fin
``

Un bon jeu, au contraire, ne pas faire votre conception en plus à fendre le texte
"asn" paragraphes en utilisant les caractères"^^', alors, pensez, et si vous voulez
la conception de votre jeux manuellement?

Parfois, cependant, il est toujours nécessaire.

> Attention! Par défaut, tous les de fuite et de départ des retours à la ligne,
> les espaces et tabulations sont coupées à partir de la sortie des gestionnaires. Donc
> comme d'habitude, ils sont inutiles et même nuisibles. Dans de rares cas,
> l'auteur peut voulez plus de contrôle sur le résultat, il
> peut-être demander std.bande\_call comme faux dans init() ou sur démarrer(),
> par exemple:

``
std.strip_call = false

obj {
 dsc = [[Il y a {Apple}.^^^^]] --- maintenant la ligne
 -- ne seront pas coupés, même si c'est bizarre
}
``

> Mais généralement ce type de formatage manuel indique un mauvais
> le style. Pour la phase de conception, il est préférable d'utiliser le décor et/ou
> de substitution.

### Mise en forme

Vous pouvez faire simple mise en forme du texte en utilisant les fonctions:

- fmt.c(string) - la place dans le centre.
- fmt.r(string) - droit de la place;
- fmt.l(ligne) - la place de la gauche;
- fmt.haut(ligne) - ligne d'en haut;
- fmt.bas(string) - la ligne de fond;
- fmt.moyen(ligne) - milieu de la ligne (par défaut).

Par exemple:

``
chambre {
 nam = "principal";
 title = "Bienvenue";
 dsc = fmt.c 'Bienvenue!'; - si une fonction a 1 seul paramètre,
 -- vous pouvez omettre les parenthèses;
}
``

Ces caractéristiques affectent non seulement le texte mais aussi des images, inséré à l'aide de
"fmt.img()'.

Il convient de noter que si vous utilisez plusieurs fonctions de mise en forme, il est supposé
qu'ils appartiennent à différentes lignes (les paragraphes). Sinon, le résultat est
undefined. Ou d'abus le texte en paragraphes avec '^' ou 'pn()'.

Au LIEU de cela, dans le calcul supprime les espaces supplémentaires. Cela signifie que peu importe
combien de places vous insérer entre les mots, sont tout de même à la conclusion
ils ne seront pas comptabilisés pour le calcul de la distance entre les mots. Parfois
il pourrait être un problème.

Vous pouvez créer _non-rupture line_ à l'aide de: esf.nb(ligne). Par exemple, le module
"fmt" utilise une ligne continue de créer de l'indentation au début de
les paragraphes. En outre, de l'esf.nb " peut être utile à la sortie de caractères spéciaux. Nous
peut dire que l'ensemble de la chaîne de paramètre 'esf.nb " est perçu par le moteur
un grand mot.

Un autre exemple. Si vous utilisez le trait de soulignement de texte, les espaces entre les mots sont
non soulignés. Lors de l'utilisation de 'esf.nb " les lacunes seront également mis en évidence.

Au LIEU de cela pas en charge l'affichage des tables, mais la conclusion est simple
tableaux de données vous pouvez utiliser 'esf.onglet()'. Cette fonction utilisée pour absolue
le positionnement de la ligne (délimité par des tabulations).

 fmt.onglet(position, [centre])

_Position_, est un texte ou un paramètre numérique. Si vous spécifiez un paramètre numérique,
elle est considérée comme la position en pixels. Si elle est définie dans un paramètre de chaîne
"nombre%', il est perçu comme position, exprimée en pourcentage de la largeur
de la fenêtre de sortie de la scène.

Le paramètre facultatif centre indique la position dans la suite de'fmt.l'onglet'
mot qui sera placé à l'offset spécifié dans la ligne. Les Positions peuvent être
comme suit:

- à gauche;
- droit;
- centre.

Par défaut, il est considéré que l'option "gauche".

Ainsi, par exemple:

``
chambre {
 nam = "principal";
 disp = "Start";
 -- l'emplacement de la "Start!" dans le milieu de la ligne
 dsc = fmt.onglet('50%', 'centre')..'Démarrer!';
}
``

Bien sûr, pas un très bon exemple, que la même chose pourrait faire à l'aide de 'esf.c()'. Un
plus d'exemple de réussite.

 dsc = fonction(s)
 p(esf.l'onglet '0%')
 p "Gauche";
 p(esf.l'onglet '100%', 'droit')
 p Droit;
fin

En fait, la seule situation où l'utilisation de l'esf.onglet()' justifiée-est de la
sortie de données de la table.

Il convient de noter que, dans une situation où nous écrire quelque chose comme:

 -- l'emplacement du "Temps" sur la ligne du centre
 dsc = fmt.onglet('50%', 'centre')..'un, deux, trois!';

Seul le mot "Temps" est placé dans le centre de la ligne, le reste des mots
sera ajouté à droite de ce mot. Si vous voulez le centre de la 'Fois
deux, trois! " comme une unité, utilisez 'esf.nb()'.

 -- placer le "un, deux, trois!" sur la ligne de centre de l'
 dsc = fmt.onglet('50%', 'centre')..de l'esf.nb ("un, deux, trois!');

À la PLACE, il est également possible d'effectuer une simple mise en forme verticale. Faire
cela, utilisez l'onglet vertical:

 fmt.y(position, [centre])

Comme dans le cas de la fmt.position de la tabulation est un texte ou un paramètre numérique. Ici, il est
perçue comme une position de la ligne exprimée en pixels ou en pourcentage de la
hauteur de la région de la scène. Par exemple, 100% -- correspond à la plus faible
région frontière de la scène. 200% correspond -- à la limite inférieure de la
seconde page de la sortie (deux fois la hauteur de la sortie volet de la scène).

Le paramètre facultatif centre spécifie la position withinline sur lequel vous
position:

- haut (bord supérieur);
- milieu (centre);
- bas (pour le bas-la valeur par défaut).

Il convient de noter que " fmt.y' œuvres entièrement pour la ligne. Si la ligne
rencontrer un peu de la fmt.y, de la loi sur le dernier onglet.

 -- l'emplacement de la "CHAPITRE I" - centre de la scène
 dsc = fmt.y('100%').."CHAPITRE I";

_Esli position spécifiée par l'onglet, est déjà occupé par une autre ligne, l'onglet est ignorée._

Par défaut, la partie statique de la scène est séparée de la dynamique double
de retour à la ligne. Si vous ne vous convient pas, vous pouvez remplacer " std.scene_delim', par exemple:

 std.scene_delim = '^' -- seule ligne

Vous ne pouvez pas modifier cette variable dans le gestionnaires, comme il n'est pas en reste, mais vous
pouvez le régler pour l'ensemble du jeu, ou de le réparer manuellement dans la fonction start()'.

Si vous avez absolument n'aime pas la façon PLUTÔT les formes conclusion (suite de
les paragraphes), vous pouvez remplacer la fonction de jeu.affichage ()", qui, par défaut
se présente comme suit:

``
jeu.affichage = fonction(s, état)
 local r, l, av, pv
 réaction locale = s:réaction() ou nil -- réaction
 r = std.ici()
 si l'état -- le rythme du jeu?
 réaction = iface:em(réaction) -- la police en italique
 av, pv = s:événements)
 av = iface:em(av) - la sortie de "importants" de la vie
 pv = iface:em(pv) - sortie arrière-plan de la vie
 l = s.joueur:look() -- les objets [scène] -- les objets et de scènes
fin
 l = std.par(std.scene_delim,
 réaction ou faux, av ou fausse, l ou faux
 pv ou faux) ou "
 de retour de l
fin;
``

Le fait que je l'ai amené ici, ce code ne veut pas dire que je recommande
annuler cette fonction. Au contraire, j'ai catégoriquement contre une forte
la liaison à la mise en forme du texte. Cependant, parfois, il y a une situation où
le plein contrôle sur la séquence de sortie nécessaires. Si vous écrivez votre premier jeu,
sautez ce texte.

### Faire

Vous pouvez modifier le style de police du texte à l'aide des combinaisons de caractéristiques:

- fmt.b(string) - le texte en gras;
- fmt.em(string) - italique;
- fmt.u(string) - le texte est souligné.
- fmt.st(string) - texte barré.

Par exemple:

``
chambre {
 nam = 'Intro';
 title = false;
 dsc = fonction(s)
 p ('Vous êtes dans une pièce: ')
 p (esf.b(s))
fin;
}
``

> Utilisation de la fonction 'esf.u " et " de l'esf.st' sur des chaînes de caractères contenant des espaces
> vous obtiendrez des lignes brisées en ces lieux. Ce qui pour l'éviter, 
> à son tour le texte en _non-rupture string_:

 fmt.u(esf.nb "l'intégrale" )

Strictement parlant, au LIEU de cela ne prend pas en charge l'affichage simultané de différents
polices de caractères dans la fenêtre de la scène (sauf pour les différents styles de police), donc si vous
besoin d'un plus flexible de contrôle de sortie, vous pouvez effectuer les opérations suivantes:

- Utilisez le graphique insérer " de l'esf.img()';
- Utiliser le module "polices", qui met en œuvre le rendu des polices de caractères différentes à 
 les frais de module 'sprite';
- Utiliser un autre moteur, parce que la plupart probablement vous utilisez à la PLACE à d'autres fins.

## Les constructeurs et héritage

__Attention!__

Si vous écrivez votre premier jeu, il serait mieux si elle était simple. Pour un
simple jeu, les informations de ce Chapitre n'ont pas besoin. En outre, 90% disent
ce n'est pas une bonne décrites dans ce Chapitre!

Si vous écrivez un jeu, où de nombreux objets similaires mayyou voulez
simplifier leur création. Vous pouvez procéder de l'une des manières suivantes:

- Créer votre concepteur;
- Créer une nouvelle classe d'entités.

### Les concepteurs

Constructeur-une fonction qui crée l'objet pour vous et remplit son
les attributs que vous en avez besoin. Prenons un exemple. Par exemple, dans votre
le jeu sera beaucoup de Fenêtres. Vous avez besoin de créer une fenêtre, une fenêtre peut être
pause. Nous pouvons écrire le constructeur 'fenêtre'.

``
fenêtre = function(v)
 v. fenêtre = true
 v. cassé = false
 si v. dsc == nil alors
 v. dsc = 'il est un {window}.'
fin
 v. act = fonction(s)
 si s.cassé, alors
 p [[Fenêtre brisée.]]
d'autre
 p [[il fait sombre à l'extérieur.]]
fin
fin
 si v. utilisés == nil alors
 v. utilisés = fonction(s, w)
 si w^'marteau', puis
 si s.cassé, alors
 p [[la Fenêtre est déjà cassé.]]
d'autre
 p [[je me suis cassé la fenêtre.]]
 s.cassé = true;
fin
de retour
fin
 return false
fin
fin
 retour obj(v)
fin
``

Comme vous pouvez le voir, l'idée des concepteurs est simple. Vous venez de créer le
fonction qui reçoit en entrée de la table avec les attributs {}, qui est
le constructeur peut remplir les caractéristiques désirées. Ensuite, ce tableau est passé
constructeur obj/chambre/dlg et retourne l'objet résultant.

Maintenant, créez une fenêtre a été facile:

 fenêtre {
 dsc = [[il y a un {window}.]];
}

Ou, parce que la fenêtre est généralement un objet statique, vous pouvez le créer
directement dans 'obj'.

 obj = { window {
 dsc = 'Dans le mur est, il est un {window}.';
}
};

Notre fenêtre sera prêt, la méthode et de la loi sur la méthode. Vous pouvez
pour vérifier le fait que l'objet est une fenêtre-vérifier simplement l'attribut de la fenêtre:

 utilisation = fonction(s, w)
 si w.fenêtre, puis
 p [[l'Action.]]
de retour
fin
 return false
fin

L'état de "faiblesse" de la fenêtre, cet attribut est cassé.

Comment mettre en œuvre l'héritage dans les constructeurs?

En fait, dans l'exemple ci-dessus est déjà utilisé l'héritage. En effet, l'
concepteur de "fenêtre" à cause de l'autre constructeur 'obj', donc l'obtention de tous les
les propriétés classiques de l'objet. Aussi, la "fenêtre" définit une variable
l'attribut "fenêtre", dans le jeu, nous avons pu comprendre que nous avons affaire à
d'une fenêtre. Par exemple:

Pour illustrer le mécanisme d'héritage, de créer une classe d'objets 'trésor',
de ceux-ci. trésors.

``
global { score = 0 }
trésor = function()
 local v = {}
 v. disp = 'trésor'
 v. trésor = true
 v. points = 100
 v. dsc = fonction(s)
 p ("il y a {', std.dispof(s), '}.')
fin;
 v. inv = fonction(s)
 p ("Il est", std.dispof(s), '.');
fin;
 v. tak = fonction(s)
 score = score + s.points; -- d'augmenter le score
 p [[main Tremblante, j'ai pris le trésor.]];
fin
 retour obj(v)
fin
``

Maintenant, nous allons créer de l'or, le diamant et le coffre au trésor.

``
or = function(dsc)
 local v = trésor();
 v. disp = 'or';
 v. or = true;
 v. points = 50;
 v. dsc = dsc;
 retour v
fin

diamant = function(dsc)
 local v = trésor();
 v. disp = 'diamant';
 v. diamant = true;
 v. points = 200;
 v. dsc = dsc;
 retour v
fin

poitrine = function(dsc)
 local v = trésor();
 v. disp = 'poitrine';
 v. poitrine = true
 v. points = 1000;
 v. dsc = dsc;
 retour v
fin
``

Maintenant, dans le jeu, vous pouvez créer un trésor à l'aide de constructeurs:

``
diamond1 = diamant("Dans la boue, j'ai remarqué {le diamant}.")
diamond2 = diamant(); -- voici la description du diamant
or1 = or("Dans le coin j'ai remarqué que les paillettes {or}.");

chambre { nam = "caverne";
 obj = {
diamond1,
or1,
 poitrine("je peux voir {poitrine}!")
};
}
``

En fait, la façon d'écrire les fonctions constructeur et mettre en oeuvre l'héritage
principe, ne dépend que de vous. Choisir l'option la plus simple et intuitif.

Lors de l'écriture des constructeurs, il est parfois utile de faire un gestionnaire d'appel de la
façon dont il le fait à la PLACE. Pour ce faire, utilisez 'std.appel(objet, méthode, options)', ce
la fonction retournera la réaction de l'attribut comme une chaîne de caractères. Par exemple,
envisager une modification de la "fenêtre", ce qui fait que vous pouvez définir votre propre la
réaction à la fenêtre d'inspection, qui sera exécutée après la norme
des postes de ce qui est la fenêtre brisée (si il est cassé).

``
fenêtre = function(nam, dsc, quoi)
 local v = {} -- crée un tableau vide
 -- le remplir
 v. fenêtre = true
 v. ce = que
 v. cassé = false
 si dsc == nil alors
 v. dsc = 'il est un {window}'
fin
 v. act = fonction(s)
 si s.cassé, alors
 p [[Fenêtre brisée.]]
fin
 local r, v = place.appel(s, "quoi")
 si v puis -- le gestionnaire est exécuté?
p(r)
d'autre
 p [[il fait sombre à l'extérieur.]]
fin
fin
 retour obj(v)
fin
``

Ainsi, nous pouvons créer une fenêtre pour spécifier le troisième paramètre, où à définir
une fonction ou une chaîne de caractères qui sera en même temps l'inspection
de la fenêtre. Le message que la fenêtre a été brisée (si c'est vraiment cassé), vous
voir l'avant de cette réponse.

### La classe d'entités

Les constructeurs des objets sont largement utilisés dans STEAD2. DANS STEAD3 obj/dlg/salle
sont implémentées comme des classes d'objet. Classe d'objets est de créer pour ceux
des occasions où le comportement de l'objet créé n'est pas inscrit dans la norme
objets obj/chambre/dlg et que vous souhaitez modifier les méthodes de la classe. La modification d'une
méthode de classe, par exemple, vous pouvez généralement à changer la façon dont l'objet de recherche dans
de la scène. Considérez, par exemple, la création d'une classe "conteneur". Le conteneur
peut stocker d'autres objets à être ouvert et fermé.

``
-- créer propre classe conteneur
cont = std.class({ -- créer la classe cont
 __suite_type = true; -- pour déterminer le type de l'objet
 affichage = fonction(s) -- substituables méthode d'affichage de l'objet
 local d = std.obj.affichage(s)
 si s:fermé() ou s#.obj == 0 alors
 de retour d
fin
 local c = s.cont ou " à l'Intérieur:' -- descripteur de contenu
 local vide = true
 pour i = 1, #s.obj faire
 local o = s.obj[i]
 si o:visible() puis
 vide = false
 si i > 1 alors c = c .. ', ' fin
 c = c..'{'..std.nameof(s)..'|'..std.dispof(s)..'}'
fin
fin
 si vide alors
 de retour d
fin
 c = c .. '.'
 retour std.par(std.space_delim, d, c)
fin;
} std.obj) -- nous héritons de l'objet par défaut
``

Après cela, vous pouvez créer des conteneurs comme ceci:

``
cont {
 nam = "boîte";
 dsc = [[il y a {box}.]];
 cont = "boite": ';
}: avec {
 'Pomme', 'poire';
}
``

Lorsque le conteneur est ouvert, vous pouvez voir une description de la boîte, et le contenu
de la case dans la ligne de liens: Dans la boîte: la Pomme, la poire. dsc objets d'Apple et
de poire sont aussi indiquées si elles sont ensemble.

Si vous souhaitez masquer dsc objets lors de l'ouverture du conteneur, ne laissant que l'
les noms des objets, vous pouvez effectuer la modification suivante:

``
-- remplacer la fonction d'affichage de n'importe quel objet
-- si l'objet est à l'intérieur du récipient, de ne pas l'appeler dsc
std.obj.affichage = function(auto)
 local w = self:où () - où en est l'objet?
 si pas de mst.is_obj(w, 'cont') then -- si ce n'est dans un conteneur
 local d = std.appel(self, 'asn')
 de retour d
fin
fin
``

Malheureusement, une description détaillée des classes est au-delà de la portée de cette
manuel, ces choses seront abordés dans un autre guide développeurs du module. Dans
entre-temps, pour votre première partie, vous n'avez pas besoin d'écrire votre propre objet
des classes.

## Conseils utiles

### Diviser des fichiers

Quand votre jeu est grande, le lieu de son code entièrement en 'main3.lua' -- mauvaise idée.

Pour séparer le texte des fichiers de jeu vous pouvez utiliser l'option "inclure". Vous devez utiliser
"inclure" dans un contexte global, de sorte que lors du chargement 'main3.lua " est chargé et
tous les fragments restants du jeu, par exemple.

``
-- main3.lua
inclure "episode1" -- .lua peut être omis
inclure des "pnj"
inclure "démarrer"

chambre {
 nam = "principal";
....
``

Comment diviser un fichier texte source dépend de vous. J'utilise les fichiers en conformité
avec les épisodes du jeu (qui est habituellement bénigne liée), mais vous pouvez
créer des fichiers pour les stocker séparément les chambres, les objets, les dialogues, etc. Est une question
de la convenance personnelle.

Il y a aussi la possibilité de charger dynamiquement des parties du jeu (avec le
capacité à redéfinir les objets). Pour ce faire, vous pouvez utiliser la gamefile':

``
...
loi = function() gamefile ("episode2") end -- .lua peut être omis
...
``

> Attention! Si votre jeu définit une fonction init(), 
> chargement des pièces, il faut être présent! Autrement
> cas, après avoir téléchargé le fichier, est appelée la fonction init ()
> qui n'est généralement pas souhaitable.

'gamefile()' permet également de télécharger un nouveau fichier et oublier stackprevious
téléchargements en lançant ce nouveau fichier comme un jeu indépendant. Pour ce faire, définissez
le deuxième paramètre comme "vrai". Gardez à l'esprit que les modules existants restent et
survivre à l'opération gamefile dans les deux cas. 'gamefile()' ne peut être utilisé dans
les gestionnaires.

 loi = function() gamefile ("episode3.lua", true); end;

Dans la deuxième option 'gamefile()' peut être utilisé pour la conception multilingue jeux, ou
jeu collections, la coquille de l'exécution d'un jeu autonome.

### Menu

Le comportement par défaut de l'élément de l'inventaire, c'est que le playerneed à faire
deux clics de souris. Cela est nécessaire car chaque tout élément de l'inventaire peut être
utilisé sur un autre objet ou de la scène de l'inventaire. Après la seconde cliquez sur se produit
jeux de le battre jeux. Parfois, ce comportement peut être indésirable. Vous pourriez
voulez faire un jeu dans lequel les mécanismes de jeu diffère de la classique à la PLACE
jeux. Alors vous pouvez avoir besoin de menu.

Le Menu est un élément de l'inventaire, qui est déclenché sur le premier clic. Lorsque
ce menu peut informer le moteur de l'action n'est pas un jeu de tact. Ainsi,
en utilisant les menus, vous pouvez créer dans la zone de gestion des stocks d'un jeu de
la complexité. Par exemple, il y a le module "proxymenu" qui met en œuvre le contrôle
le style de jeu des quêtes sur la ZX-spectrum. Dans le jeu "Manoir" de son
de contrôle, qui introduit un peu d'action modificateurs, etc.

Ainsi, vous pouvez faire le menu de l'inventaire, l'identification des objets de type
'menu'. Dans ce cas, le gestionnaire de menu ("loi") sera appelée après un clic
de la souris. Si le gestionnaire renvoie la valeur false, l'état de la partie ne change pas.
Par exemple, la mise en œuvre de la poche:

``
menu {
 etat = false;
 nam = "poche";
 disp = fonction(s)
 si s.etat
 de retour de l'esf.u('poche'); -- souligner active poche
fin
 de retour de "poche";
fin;
 gen = fonction(s)
 si s.etat
 s:open(); -- afficher tous les éléments dans la poche
d'autre
 s:close(); -- masquer tous les éléments dans la poche
fin
 de retour s
fin;
 act = fonction(s)
 s.etat = s pas.état -- changement d'état
 s:gen (); - pour ouvrir ou fermer la poche
fin;
}: avec {
 obj {
 nam = "couteau";
 inv = 'Ceci est le couteau';
};
}

la fonction init()
 prendre la "poche": gen()
fin
``

### Le statut du joueur

Parfois, il ya un désir affiche du statut. Par exemple,le nombre de
les points de l'état du héros ou enfin le temps des jours. Au LIEU de cela ne fournit pas de
tous les autres points de retrait, à l'exception des scènes et de l'inventaire, par conséquent, l'
la plus simple façon de sortie de l'état est à la sortie de la zone de l'équipement.

Ci-dessous est une mise en œuvre du statut de joueur comme un texte qui apparaît dans la
l'inventaire, mais ne peut pas être sélectionné, c'est, semble juste comme texte.

``
global {
 la vie = 10;
 puissance = 10;
}

stat { -- stat-état de l'objet
 nam = 'statut';
 disp = fonction(s)
 pn ('la Vie: ', la vie)
 pn ("Alimentation: ', puissance)
fin
};

la fonction init()
 prendre du "statut"
fin
``

### à pied de gestionnaires pour le onenter et onexit

Vous pouvez faire "marcher" les gestionnaires "le onenter" et "onexit'. Par exemple,
'chemin' mis en œuvre avec un gestionnaire de la "onenter", qui met le joueur dans la
l'autre pièce.

Il est recommandé de retour d'onexit/false dans le onenter cas, si vous ne
la marche de ces gestionnaires.

### Encodage du code source du jeu

Si vous ne voulez pas afficher le code source de leurs jeux, vous pouvez encoder
le code source à l'aide de l'option de ligne de commande le 'code':

 sdl-au lieu de coder <chemin du fichier> [chemin de sortie]

Et l'utilisation de l'encodage du fichier en utilisant la normale include/gamefile. Toutefois, pour que cette
vous devez écrire au début de main3.lua:

 std.dofile = std.doencfile

Le fichier principal 'main3.lua " doit être laissée ouverte. Par la façon, le schéma
se présente comme suit ('de jeu.lua' -- fichier encodé):


 -- $Nom: Mon jeu fermé!$
 std.dofile = std.doencfile
 include "jeu"; -- personne ne sait comment faire pour passer!

__Important!__

> Ne pas utiliser la suite avec "luac', comme 'luac' crée
> dépend de la plateforme de code! Toutefois, les éléments suivants peuvent être
> utilisé pour rechercher des erreurs dans votre code.

### Boxe ressources

Vous pouvez package les ressources du jeu (graphismes, de la musique, des thèmes) dans le fichier
ressources".fid' placer toutes les ressources dans le répertoire "data" et de commencer à
Au LIEU de cela:

 sdl-au lieu-idf <chemin d'accès aux données>

Ainsi, dans un répertoire sera créé le fichier de données.fid'. Le placer dans
le répertoire du jeu. Maintenant les ressources jeu de séparer les fichiers peuvent être supprimés
(bien sûr, laissant lui-même les fichiers d'origine).

Vous pouvez emballer dans format.fid' l'ensemble du jeu:

 sdl-au lieu-idf <chemin d'accès au jeu>

Jeu 'idf' peut être exécuté comme un jeu normal "à la place" (comme si c'était un dossier)
et aussi à partir de la ligne de commande:

 sdl-à la place de jeu.idf

### La commutation entre les joueurs

Vous pouvez créer un jeu avec de multiples personnages et de temps en temps pour passer
entre eux (voir " change_pl()'). Mais vous pouvez également utiliser cette astuce dans le but
pour être en mesure de basculer entre les différents types de stocks.

### Utiliser les paramètres du gestionnaire d'

L'exemple de code.

 obj {
 nam = "pierre";
 dsc = 'Sur le bord de mensonges {rock}.';
 loi = function()
 supprimer 'pierre';
 p 'j'ai poussé la pierre, il est tombé et a volé vers le bas...';
fin

Loi gestionnaire pourrait regarder de plus simple:

 act = fonction(s)
supprimer le(s);
 p 'j'ai poussé la pierre, il est tombé et a volé vers le bas...';
fin

### Statut particulier des gestionnaires d'

Maître renvoie le texte dans le formulaire de retour "des messages texte." Ou avec
les fonctions p()/pr()/pn()/pf(). En outre, il y a des statuts qui
peut être utile lors du développement du jeu.

L'état de retour est faux:

 return false

Ce statut signifie que le gestionnaire n'a pas rempli sa fonction et needsto
être ignoré. Généralement, le moteur dans ce cas, il va appeler le gestionnaire par
par défaut.

Vous pouvez également retourner un statut spécial:

 retourner true, false

Dans ce mode, seul le matériel (mais pas de la scène) sera redessinée. Ce statut est utile
pour la mise en œuvre de menu le domaine de l'inventaire.

Il y a un autre statut spécial: std.nop(). Il peut être utilisé comme un
appel de fonction à la fin de son maître ou avec le retour.

 retour std.nop()
 -- ... ou ...
std.nop()
 -- ensuite, la fin de la fonction ou de retour

Dans ce cas, le contenu de la scène restera le même que lastthe battre
jeu (même chaîne de caractères d'une réaction de vieux). Ce statut idéalement être utilisé dans
conjointement avec le module de thème, lorsque vous avez besoin de changer le design de la
jeu à la volée et le rafraîchissement de l'image avec les nouveaux paramètres de thème.

### Minuterie

Pour les événements asynchrones lié au temps réel. il est possible d'utiliser la minuterie.
En fait, vous devez bien penser que dans le jeu d'aventure à
utilisation de la minuterie. Habituellement, il n'est pas perçu trop favorablement. Avec l'autre main,
la minuterie peut être utilisée pour contrôler la musique ou à des fins de conception.

Pour utiliser la minuterie, vous devez connecter le module "timer".

 besoin de "timer"

La minuterie est programmée à l'aide de l'objet "timer".

- minuterie:set(MS) -- régler le minuteur d'intervalle en millisecondes;
- minuterie:stop() -- l'arrêt de la minuterie.

Lorsque le compte à rebours incendies, appelle un gestionnaire de jeu.minuterie. Si jeu.minuterie renvoie la valeur false,
la scène n'est pas redessiné. Dans le cas contraire, la valeur de retour est sortie que le
réponse.

Vous pouvez faire local pour la chambre des maîtres-timer". Si la chambre a déclaré le gestionnaire
"timer", elle est appelée au lieu'game.timer". Si elle retourne false -- le jeu est
appelé.minuterie.

Par exemple:

``
jeu.timer = fonction(s)
 si time() > 10 alors
 return false
fin
 snd.jouer " gfx/bip.ogg';
 p ("Timer:", time())
fin

la fonction init()
 minuterie:ensemble(1000) -- temps en seconde
fin
``

``
chambre {
 entrée = fonction(s)
minuterie:ensemble(1000);
fin;
 timer = fonction(s)
minuterie:stop();
 marcher комната2';
fin;
 nam = 'minuterie Test';
 dsc = [[Attendre.]];
}
``

Lorsque le minuteur arrive à la sauvegarde de fichiers, de sorte que vous ne avez-vous pas besoin de prendre soin de
le restaurer.

Vous pouvez y revenir à partir de la minuterie statut spécial:

 retourner true, false

Dans ce mode, seule la zone d'inventaire sera redessinée. Il est possible d'utiliser de
les statuts de comme heures.

En outre, au LIEU de cela, il ya la possibilité de suivre les intervalles de temps dans
millisecondes. Pour ce faire, utilisez la fonction à la place.les tiques(). La fonction
renvoie le nombre de millisecondes écoulées depuis le début du jeu.

### Lecteur de musique

Vous pouvez écrire votre jeu lecteur de musique, créé sur la base d'un objet vivant,
par exemple:

``
-- lecture des pistes dans un ordre aléatoire
exiger "snd"

obj {
{
 pistes = {"mus/astro2.mod",
"mus/aws_chas.xm",
"mus/dmageofd.xm",
"mus/doomsday.s3m"};
};
 nam = 'joueur';
 la vie = fonction(s)
 si pas de snd.music_playing() puis
 local n = s.pistes[rnd (s#.les pistes)]
 snd.de la musique(n, 1);
fin
fin;
}:lifeon();
``

Ci-dessous est un exemple plus complexe joueur. Modifier la piste si c'est terminé ou
il a été plus que 2 minutes, et le joueur va de chambre en chambre. Dans chaque
piste, vous pouvez spécifier le nombre de lectures (0 - une boucle de la piste):

``
besoin de "timer"
global { track_time = 0 };

obj {
 nam = 'joueur';
 pos = 0;
{
 playlist = { '01 Congelés soleil.ogg', 0,
 '02 Pensée.ogg', 0,
 '03 Mélancolie.ogg', 0,
 '04 tous les jours de bonheur.ogg', 0,
 '10 de Bon matin, à nouveau.ogg', 1,
 '15 [Bonus track] La fin (démo de couverture).ogg', 1
};
};
 tique = fonction(s)
 si snd.music_playing() et ( track_time < 120 ou pas player_moved ()), puis
de retour
fin
 track_time = 0
 si s.pos == 0 alors
 s.pos = 1
d'autre
 s.pos = s.pos + 2
fin
 si s.pos > #s.liste de lecture, puis
 s.pos = 1
fin
 snd.musique ("mus/'..s.la liste de lecture[s.pos], s.la liste de lecture[s.pos + 1]);
fin;
}

jeu.timer = fonction(s)
 track_time = track_time + 1
music_player:tick();
fin

la fonction init()
minuterie:ensemble(1000)
fin
``

### Les objets vivants

Si votre héros a besoin d'un ami, l'un des moyens peut être une méthode de la " vie " de ce
caractère, qui se déplace toujours l'objet à l'emplacement du joueur:

``
obj {
 nam = "cheval";
 dsc = 'je suis debout à Côté de {le cheval}.';
 act = [[Mon cheval.]];
 la vie = fonction(s)
 si player_moved() puis
place(s);
fin
fin;
}

la fonction init()
 lifeon "cheval"; -- immédiatement relancer le cheval
fin
``

### Menu

Vous pouvez appeler à partir du menu de jeu au LIEU d'utiliser la fonction'istead.menu()'.
Si l'option demander: "enregistrer", la "charge" ou "quitter", elle ne doit être causé par l'
la section sur le menu.

### Dynamique de création de l'objet

Les objets ordinaires et les chambres ne peuvent pas être créés "à la volée". En général, vous créez
dans l'espace de noms global fichier lua. Cependant, il ya des jeux dans lesquels le
nombre d'objets est inconnu à l'avance, ou le nombre d'objets importants et qu'ils
sont ajoutées à mesure que le jeu progresse.

À la PLACE, il existe un moyen de créer n'importe quel objet à la volée. Pour cela, vous aurez
besoin d'écrire le constructeur de votre objet et l'utilisation de la fonction "nouveau".

Le constructeur doit être déclaré.

Ainsi, vous pouvez utiliser les fonctions 'nouvelle' et 'supprimer' pour la création et la suppression de
les objets dynamiques. Exemples:

``
déclarer "boîte" (function()
 retour obj {
 dsc = [[Il y a {box}.]];
 tak = [[j'ai ramassé une boîte.]];
}
fin)

local o = new (boîte);
prendre(o);
``

``
déclarer "boîte" (function(dsc)
 retour obj {
 dsc = dsc;
 tak = [[j'ai ramassé une boîte.]];
}
fin)
prendre(nouveau(case " Dans le coin {box}'))
``

les "nouveaux" prend en premier argument est déclarée comme une fonction constructeur, et tous les
d'autres paramètres comme les arguments du constructeur. Le résultat de l'constructeur doit
être l'objet.

``
fonction myconstructor()
 local v = {}
 v. disp = 'test de l'objet"
 v. act = 'Test de réponse"
 retour obj(v)
fin
``

> Attention! Lorsque l'objet est créé le constructeur ne devrait se prévaloir
> sur des informations provenant de paramètres! Le fait que la création de
> objet lorsque le chargement se produit au début, lorsque l'environnement du monde
> pas encore complètement chargé! En tant que paramètres sont pris en charge
> les types simples: les chaînes de caractères, des tableaux, des chiffres, des Booléens
> valeurs. Non déclarées de fonctions et de listes ne fonctionnera pas.

Si vous voulez détruire l'objet par son nom ou sur le lien-utilisation variable:

 purge(o) -- enlever de toutes les listes
 delete(o) - communiqué de l'objet

Dans ce cas, supprimez c'est la suppression de l'objet à partir de au LIEU de cela, plutôt que de
analogique remove() ou de la purge(). Généralement, il fait peu de sens pour effectuer la suppression.
Seulement si le sujet ne sera jamais nécessaire dans un jeu, ou allez-vous
recréer un objet portant le même nom, il est logique de le libérer avec l'aide de
supprimer().

Un plus dans la pratique exemple:

``
-- déclare le constructeur de chemin de

déclarer "make_path' (function(v) voie de retour(v) à la fin)

-- dynamique de transition
-- créer un nouvel objet
-- et le mettre dans une façons()

mettre( nouveau (make_path, { 'transition', 'комната2'}, des moyens())
``

### L'interdiction de l'enregistrement d'un jeu

Parfois, vous pouvez interdire au joueur de faire de la conservation dans le jeu. Pour
exemple, si nous parlons des scènes où un élément important est la cas, ou
pour les petits jeux où le perdant doit être fatale et qui nécessitent le redémarrage de l'
jeu.

Pour contrôler la fonction de sauvegarde utilise l'attribut " à la place.nosave'.

Par exemple:

au lieu de cela.nosave = true -- désactiver la préservation de

Si vous voulez éviter d'enregistrer non pas partout, mais dans certaines scènes, faire
"à la place.nosave' à la vue de la fonction ou de modifier la condition de l'attribut sur la
mouche -- il arrive à un fichier de sauvegarde.

``
-- interdire
- enregistrer les chambres qui contiennent le nosave attribut.
au lieu de cela.nosave = function()
 de retour ici().nosave
fin
``

Il convient de noter que l'interdiction de la conservation ne signifie pas l'interdiction
La sauvegarde automatique. Pour contrôler la fonction d'enregistrement automatique, utilisez le même attribut
"à la place.noautosave'.

> Vous pouvez enregistrer explicitement à un jeu avec un appel à:
> "à la place.enregistrement automatique([numéro de l'emplacement])'; Si le numéro de l'emplacement n'est pas précisé, 
> le jeu sera sauvegardé dans la fente "AutoSave". Gardez à l'esprit que
> enregistre l'état de __après__ l'achèvement de l'étape en cours de jeu.

### Définition d'un type d'objet

À la PLACE, il y a deux façons de déterminer le type de l'objet. La première
à l'aide de la fonction std.is_obj(variable, [type]).

Par exemple:

``
a = salle {
 nam = 'objet';
};

dprint(std.is_obj(a)) -- imprime vrai
dprint(std.is_obj('objet')) -- imprime faux
dprint(std.is_obj(a, "room")) -- imprime vrai
dprint(std.is_obj(un.obj, 'list')) -- imprime vrai
``

std.is_obj() est utile pour déterminer le type de la variable ou de l'argument
fonction.

La deuxième méthode est d'utiliser la méthode de type:

``
a = salle {
 nam = 'objet';
};

dprint(a:type "chambre") -- imprime vrai
``

## Sujets de sdl-au lieu

Graphique interprète prend en charge le moteur de thème. Tema est un répertoire, le fichier
'thème.ini' à l'intérieur.

Le thème, qui est le minimum requis-est le thème "par défaut". Cette rubrique
est toujours chargé en premier. Tous les autres sujets héritent de celui-ci et peut partiellement ou
pour le remplacer complètement les paramètres. Les thèmes sont choisis par l'utilisateur par l'intermédiaire de
les réglages du menu, mais le jeu peut contenir son propre sujet et donc l'influence
sur votre apparence. Dans ce cas, le répertoire, le jeu doit être un fichier
'thème.ini'. Toutefois, l'utilisateur est libre de désactiver ce mécanisme, tandis que la
interprète avertir à propos de la violation de la créatrice du concept, l'auteur de
le jeu.

La syntaxe de thème.ini' est très simple.

 <paramètre> = <valeur>
ou

 commentaire

Les valeurs peuvent être des types suivants: chaîne de caractères, la couleur, le nombre.

La couleur est spécifiée dans le formulaire n ° rgb où r, g et b composantes de couleur dans
hexadécimal. En outre, certains de base colorsrecognized par leurs noms.
Exemple: yellowgreen, ou le violet.

Paramètres peut prendre les valeurs:

- scr.w = largeur de l'espace de jeu, en pixels (nombre)
- scr.h = hauteur de l'espace de jeu, en pixels (nombre)
- scr.col.bg = couleur d'arrière-plan
- scr.gfx.scalable = [0/1/2] (0 - n'est pas à l'échelle thème 1 -
 évolutive 2 évolutive sans lissage), depuis
 la version 2.2.0 disponible en option [4/5/6]: 4 - entièrement modulable (non évolutif 
 les polices de caractères), 5 évolutive, avec les polices vectorielles, 6 sur l'échelle de sans lissage, 
 avec pas d'une police vectorielle
- scr.gfx.bg = chemin d'accès à l'arrière-plan de l'image (string)
- scr.gfx.le curseur.x = la coordonnée x du curseur centre (nombre)
- scr.gfx.le curseur.y = la coordonnée y du curseur centre (nombre)
- scr.gfx.le curseur.normal = chemin d'accès au curseur de fichier image (string)
- scr.gfx.le curseur.utilisation = chemin d'accès au curseur de l'image utilisation du fichier (chaîne de caractères)
- scr.gfx.utilisation = chemin vers l'image-indicateur d'utilisation (string)
- scr.gfx.pad = rembourrage pour les barres de défilement de menu et les bords (nombre)
- scr.gfx.x, de la scr.gfx.y, de la scr.gfx.w, la scr.gfx.h = coordonnées, la largeur et la 
 la hauteur de la fenêtre des images. La région dans laquelle est situé l'image de la scène. 
 L'interprétation dépend du mode de la visite (nombre)
- gagner.gfx.h - synonyme de la scr.gfx.h (pour la compatibilité)
- scr.gfx.icône = chemin d'accès au fichier de l'icône du jeu (OS dépendant de l'option,
 peut fonctionner correctement dans certains cas)
- scr.gfx.mode = mode de localisation (ligne fixe, incorporé ou flottant). Définit le mode de 
 de l'image. intégré de l'image est une partie du contenu de la principale 
 de la fenêtre, les paramètres de la scr.gfx.x, de la scr.gfx.y, de la scr.gfx.w sont ignorés. 
 float-l'image qui se trouve aux coordonnées spécifiées (scr.gfx.x, 
 la scr.gfx.y) et évolue en fonction de la taille de la scr.gfx.w x scr.gfx.si h qui la dépasse. 
 fixe l'image fait partie d'une scène en mode imbriqué, mais ne défile pas 
 avec un texte et situé directement au-dessus d'elle. La disposition des modifications de mode 
 modificateurs float '(gauche/droite/centre/milieu/bas/haut", spécifie comment 
 placer une image dans le domaine de la scr.gfx. Par exemple: float-en haut à gauche;
- gagner.de défilement.mode = [0/1/2/3] en mode de défilement de la région de la scène. 0 - pas de
 la fonction de défilement automatique 1 - faites défiler jusqu'à modifier le texte, 2 faites défiler jusqu'à modifier uniquement si le 
 le changement n'est pas visible, 3 - toujours à la fin;
- gagner.x, win.y gagner.w, gagner.h = coordonnées, la largeur et la hauteur de la principale 
 de la fenêtre. Région qui est une description de la scène (nombre)
- gagner.fnt.nom = chemin d'accès au fichier de police de caractères (string). Ci-après, la police de caractères peut contenir 
 une description de tous les styles, par exemple: {sans,sans-b et 
 sans-je,sans-bi}.ttf (le style de la police définie sur normal, gras, italique et 
 gras-italique). Vous pouvez omettre certains des visages, et le moteur lui-même va 
 générer basé sur le style normal, par exemple: {sans, sans-i}.ttf (
 définir uniquement régulière et en italique);- gagnant.align = center/gauche/droite/à justifier (texte 
 l'alignement dans la fenêtre de la scène);
- gagner.fnt.taille = la taille de la police de la fenêtre principale (taille)
- gagner.fnt.hauteur = espacement de ligne comme le flottant du point décimal (1.0 est 
valeur par défaut)
- gagner.gfx.jusqu', gagner.gfx.bas = les chemins vers les fichiers d'image skallerup haut/bas pour 
 la fenêtre principale (string)
- gagner.jusqu'.x, win.jusqu'.y gagner.vers le bas.x, win.vers le bas.y = coordonnées. (coordonner ou -1)
- gagner.col.fg = couleur du texte de la fenêtre principale (couleur)
- gagner.col.link = lien de la couleur de la fenêtre principale (couleur)
- gagner.col.alink = couleur du lien actif pour la fenêtre principale (couleur)- win.façons.mode 
 = haut/bas (pour spécifier l'emplacement de la liste de raccourcis par défaut à haut -- sur 
 haut de la scène)
- inv.x, inv.y, inv.w, inv.h = coordonnées, la largeur et la hauteur de la région 
 de l'inventaire. (nombre)
- inv.mode = chaîne de mode de l'inventaire (horizontale ou verticale). En horizontal 
 mode d'inventaire dans une ligne peut être de plusieurs éléments. En mode vertical, chaque ligne 
 de l'inventaire ne contient qu'une seule chose. Il y a des modifications à (gauche/
 droite/centre). Vous pouvez définir le mode désactivé si le jeu n'est pas 
 besoin d'un inventaire;
- inv.col.fg = couleur du texte des outils (couleur)- inv.col.link = lien outils de couleurs
(couleur)
- inv.col.alink = couleur du lien actif pour l'inventaire (la couleur)
- inv.fnt.nom = chemin d'accès au fichier de police de l'inventaire (en ligne)
- inv.fnt.taille = la taille de la police de l'inventaire (taille)
- inv.fnt.hauteur = espacement de ligne comme le flottant le point décimal
 (1.0 est par défaut)
- inv.gfx.jusqu', inv.gfx.bas = les chemins vers les fichiers d'image skallerup haut/bas de 
 équipement (en ligne)
- inv.jusqu'.x, inv.jusqu'.y, inv.vers le bas.x, inv.vers le bas.y = coordonnées.
 (coordonner ou -1)
- menu.col.bg = arrière-plan (couleur)
- menu.col.fg = texte menu color (couleur)- menu.col.link = lien de menu couleur
(couleur)
- menu.col.alink = lien actif menu color (couleur)
- menu.col.alpha = la transparence du menu de 0 à 255 (nombre)
- menu.col.border = frontière menu color (couleur)
- menu.bw = épaisseur du bord de menu (nombre)
- menu.fnt.nom = chemin d'accès au fichier de police de menu (string)
- menu.fnt.taille = la taille de la police du menu (taille)
- menu.fnt.hauteur = espacement de ligne comme le flottant le point décimal
 (1.0 est par défaut)
- menu.gfx.bouton = le chemin d'accès au fichier de l'image de l'icône de menu (string)
- menu.bouton.x, menu.bouton.y = coordonnées des boutons de menu (nombre)
- snd.cliquez sur = le chemin de la cliquez sur le fichier son (string)
- inclure = nom du thème (le dernier composant du chemin d'accès au répertoire) (chaîne de caractères)

En outre, l'objet de l'en-tête peut contenir des commentaires avec des balises. Pour le moment
il n'y a qu'une seule balise: $Nom:, contenant les chaînes UTF-8 avec le nom du thème.
Par exemple:

``
; $Nom:Nouveau thème$
; la modification des thèmes livre
citons = le livre-pour utiliser le thème du Livre
la scr.gfx.h = 500 -- remplacer un paramètre
``

> L'interprète des recherches pour les thèmes suivants dans les "thèmes" du répertoire. La version Unix
> en plus de ce répertoire, analyse également le répertoire ~/.au lieu de cela/themes/
> Version de Windows -- Documents and Settings/USER/Local
> Settings/Application Data/plutôt/thèmes

En outre, la nouvelle version à la PLACE du soutien de plusieurs personnes dans un seul jeu.
Permettant au joueur via le menu standard au LIEU de choisir un
apparence, fourni par l'auteur du jeu. Pour cela, tous les fils doivent être en
le jeu dans un sous-répertoire de thèmes. À son tour, chaque thème, est un sous-répertoire dans
le répertoire des thèmes. Dans chacun de ces sous-répertoire doit être un fichier de thème.ini et
thème de ressources (images, polices, sons). Dans ce casea thèmes catalogue
themes/default - ce thème sera chargé par défaut. Le format du thème
les fichiers.ini nous venons de considérer. Cependant, les chemins d'accès aux ressources
thème.fichier ini ne sont pas écrites par rapport à la racine du répertoire du jeu, et
relatif au répertoire courant thème. Cela signifie qu'ils contiennent généralement
seul le nom du fichier sans le chemin d'accès au répertoire. Par exemple:

``
mygame/
thèmes/
par défaut/
thème.ini
bg.png
grand écran/
thème.ini
main3.lua
``

thème.ini

``
la scr.gfx.bg = bg.png
; ...
``

Dans ce cas, tous les thèmes du jeu sont héritées de l'
themethemes/par défaut. Mécanisme de prise en charge comprennent. Dans le même temps, au LIEU
essaie d'abord de trouver le thème du jeu, et si ces sujets ne sont pas de la volonté
téléchargé le thème de la norme thèmes de la PLACE (si elle existe). En outre,
dans le thème.ini vous ne pouvez modifier ces paramètres en fonction des modifications étaient nécessaires.

## Les Modules

Des fonctionnalités supplémentaires sont souvent mis en œuvre au LIEU de la forme de modules. Pour
l'utilisation d'un module, vous devez écrire:

 besoin de "nom du module"

Ou:

 loadmod "nom du module"

Si le module est livré avec le jeu.

Modules inclus au LIEU de cela, mais il ya ceux que vous pouvez télécharger séparément
et le mettre dans le répertoire du jeu. Vous pouvez remplacer n'importe quel module standard comme
son si vous le mettez dans le répertoire du jeu sous le même nom que le fichier de
standard.

Le module est en fait " lua " nom du fichier: 'nom_du_module.lua'.

Suivants sont à la base de modules standard, indiquant la fonctionnalité
ils fournissent.

- 'dbg" module de débogage.
- "clic" — module d'intercepter les clics de la souris sur l'image de la scène;
- 'prefs' paramètres de module (entrepôt de données paramètres);
- "instantanés" — un plugin pour soutenir les snapshots (pour les pots-de-vin de jeu 
les situations);
- 'fmt' module de rétractation;
- "thème" — le thème de bureau à la volée;
- 'noinv' module pour faire fonctionner l'équipement;
- la touche" - module de traitement des événements de l'opération de touches;
- minuterie de la minuterie;
- 'sprite' — module pour travailler avec des sprites;
- 'snd' module audio;
- 'nolife' module méthodes de verrouillage de la vie;

Exemple de charger les modules:

``
--$Nom: Mon jeu!$
exiger "fmt"
exiger ", cliquez sur"
``

> Quelques modules supplémentaires qui ne sont pas inclus dans la norme
> l'offre, vous pouvez le télécharger à partir de [référentiel
> modules](https://github.com/instead-hub/stead3-modules). Juste
> télécharger le module désiré et de le mettre dans le répertoire
> le jeu. Activer ce module en utilisant la loadmod().

### Module de touches

Vous pouvez intercepter les événements de clavier à l'aide du module de touches.

Généralement, l'interception des touches de sens que pour la saisie de texte.

Pour spécifier ce que les touches que vous souhaitez surveiller définir la fonction
clés:filtre presse, de la clé). Cette fonction doit retourner true si vous voulez
suivre cet événement. Par exemple:

``
exiger des "clés"

touches de fonction:filtre presse, de la clé)
 appuyez sur la touche retour -- capture en appuyant sur les touches
fin
``

Dans l'exemple nous suffit de retourner le paramètre, appuyez sur ce qui est vrai pour
keypress les événements et les faux-serrant. La clé est transmis le nom symbolique d'un
clé (comme une chaîne de caractères).

Habituellement, nous avons besoin de choisir ce qu'clés que nous envisageons:

``
exiger des "clés"

touches de fonction:filtre presse, de la clé)
 si touche == '0' ou key == '1' ou key == 'z' alors
 appuyez sur la touche retour -- capture les frappes de touches z, 1 et 0
fin
fin
``

Donc, les clés:filtre permet de choisir les événements de clavier. Et doevents venir en
jouer dans la forme de l'appel de gestionnaire onkey " pour la salle de cours, ou (si pas
spécifié dans la salle) de l'objet "jeu".

Le onkey gestionnaire agit en tant que gestionnaire normal STEAD3. Il peut retourner false et
ensuite, il est considéré que la clé n'a pas été traitée de jeu. Ou il peut effectuer
un peu d'action.

_Внимание:_ Si le jeu prendra en charge tous les principaux événements, même ceux combinaisons
qui sont utilisés par l'INTERPRÉTEUR sont traitées le jeu, pas l'interprète.
Par exemple, si le jeu va attraper appuyez sur la touche echap (et renvoie la valeur false à partir de la
gestionnaire), en appuyant sur"echap" ne seront plus traitées par l'interprète
Au LIEU d'échappement -- l'appel du menu).

Ci-dessous est un exemple simple pour afficher la symbolique des noms de touches:

``
exiger des "clés"

touches de fonction:filtre presse, de la clé)
 appuyez sur la touche retour -- attraper tous les cliquant
fin

jeu.onkey = fonction(s, appuyez sur la touche)
 dprint("pressé: ", clé)
 p("Pressé: ", clé)
 return false -- permettre de gérer les clés de l'interprète à la PLACE
fin
``

Cet exemple peut être utilisé pour déterminer le symbolique, le nom de l'
les touches.

Lors de l'écriture de jeux d'arcade peut être utile de ne pas obtenir de l'événement du clavier et de la
le scanner (généralement de la minuterie). Pour ce faire, vous pouvez utiliser la fonction
touches:état(nom de la clé).

Cette fonction retourne true pour les pressés et les faux-pressées, par exemple:

``
besoin de "timer"
exiger des "clés"

-- affichage de l'état de la touche le curseur vers la droite
jeu.timer = fonction(s)
 dprint("état de droit" la clé: ", les clés:état de "droit")
 p("Comme les touches "droite":", des clés:état de "droit")
fin

minuterie:ensemble(30)
``

### Module, cliquer

Vous pouvez suivre dans votre jeu, clique sur l'image de la scène, et le fond.
Pour ce faire, utilisez le module "clic". Aussi, vous pouvez garder une trace de l'état de la souris
par la fonction:

 au lieu de cela.mouse_pos([x, y])

Qui renvoie les coordonnées du curseur. Si vous définissez les paramètres (x, y),
vous pouvez déplacer le curseur à la position spécifiée (toutes les coordonnées sont
calculé par rapport au coin supérieur gauche de la fenêtre au LIEU de cela).

``
exiger ", cliquez sur"
fonction cliquez sur:filtre(presse, btn, x, y, px, py)
 dprint(presse, btn, x, y, px, py)
 de retour de la presse et px -- seul hic cliquant sur l'image
fin
chambre {
 nam = "principal";
 pic = "boîte:320 x 200,rouge";
 onclick = function(s, appuyez sur, btn, x, y, px, py)
 pn("Vous avez cliqué sur l'image: ", px, ", ", py)
 pn("coordonnées Absolues: ", x, ", ", y)
 p("Bouton:" btn)
fin;
}
``

_Внимание!_ Vous pouvez à la PLACE utiliser le filtre par défaut clics de souris, ce qui
s'éteint clics. Ceci est fait pour éliminer les effets de rebond
les boutons sur une souris. Dans certains cas, le filtre peut être indésirable. Dans ce
cas, utilisez la fonction à la place.souris\_filter(), qui peut être utilisé pour
déterminer les valeurs de filtre actuelles de la souris et d'installer de nouvelles ( le nombre
- off), par exemple:

``
la fonction start()
 dprint("Souris filtre de retard: ", à la place.mouse_filter())
 au lieu de cela.mouse_filter(0) -- désactiver le filtre
fin
``

Ou ceci:

``
old_filter = à la place.mouse_filter(0) -- éteindre
...
au lieu de cela.mouse_filter(old_filter) -- restauré
``

### Le module thème

Le module thème vous permet de modifier les paramètres de thème à la volée.

> Gardez à l'esprit que le fait de changer les paramètres de thème à la volée pour les jeux qui
> ne contient pas son objet propre d'une source de problèmes potentiels! Le fait
> que votre jeu doit être prête à travailler avec toutes les autorisations et paramètres
> le fait qu'il est extrêmement difficile à réaliser. Donc, si vous allez à
> changer les paramètres du thème de code -- créer votre propre thème et de le transformer
> dans un jeu!

Dans ce cas, l'enregistrement des modifications sujet pour enregistrer un fichier non pris en charge. L'auteur
le jeu doit restaurer les paramètres de thème dans la fonction start () comme vous le faites
lorsque vous travaillez avec le module de sprites.

Pour modifier les paramètres du thème en cours, utilisez les fonctions suivantes:

``
-- configuration de la fenêtre de sortie
thème.gagner.geom(x, y, w, h)
thème.gagner.couleur(fg, lien, alink)
thème.gagner.police de caractères(le nom, la taille et la hauteur)
thème.gagner.gfx.jusqu'(pic, x, y)
thème.gagner.gfx.vers le bas(image, x, y)

-- configuration de l'inventaire
thème.inv.geom(x, y, w, h)
thème.inv.couleur(fg, lien, alink)
thème.inv.police de caractères(le nom, la taille et la hauteur)
thème.inv.gfx.jusqu'(pic, x, y)
thème.inv.gfx.vers le bas(image, x, y)
thème.inv.mode(mode)

-- menu de configuration
thème.menu.bw(w)
thème.menu.couleur(fg, lien, alink)
thème.menu.police de caractères(le nom, la taille et la hauteur)
thème.menu.gfx.bouton(image, x, y)

-- configuration graphique
thème.gfx.curseur(norme, usage, x, y)
thème.gfx.mode(mode)
thème.gfx.pad(pad)
thème.gfx.bg(bg)

-- paramètres de son
thème.snd.cliquez sur(nom);
``

Il y a la possibilité de lire les paramètres actuels:

 thème.le 'get' nom de la variable thèmes';

La valeur de retour est toujours sous forme de texte.

 thème.set ('nom de la variable thème", valeur);

Vous pouvez réinitialiser la valeur du paramètre du sujet, qui a été installé dans
l'intégré dans le thème du jeu:

 thème.réinitialiser la variable "nom";
thème.gagner.reset();

Il y a une fonction, afin de connaître le courant du thème sélectionné.

thème.nom()

La fonction retourne une chaîne de caractères -- le nom du répertoire du thème. Si le jeu
les utilisations propres le fichier " theme.ini', la fonction retournera un point. C'est
utile pour déterminer si le mécanisme de sa propre pour ces jeux:

``
si le thème.nom() ~= '.' alors
 message d'erreur "Veuillez activer propre thème de la mode dans le menu!"
fin
``

Si le jeu utilise le mécanisme de multiples thèmes, puis le nom du thème commence
avec un point, par exemple:

``
si le thème.nom() == '.par défaut", puis
 -- nos intégré dans le thème par défaut
elseif thème.nom() == 'par défaut', puis
 -- la norme thème par défaut.
fin
``

Exemple d'utilisation:

``
thème.gfx.bg "dramatic_bg.png";
thème.gagner.geom (0,0, thème.obtenir 'la scr.w' thème.obtenir 'la scr.h');
thème.inv.en mode "désactivé"
``

Obtenir les dimensions du thème:

``
thème.la scr.(w) -- largeur
thème.la scr.w () - hauteur
``

### Le module de sprite

Le sprite module permet de travailler avec des images graphiques.Pour permettre à l'e-mail
module:

 besoin de "sprite"

Les Sprites ne peut pas entrer dans le fichier de sauvegarde de sorte que l'état de recouvrement des sprites-la
la tâche de l'auteur du jeu. Généralement, on utilise les fonctions init() ou
start(). start() est appelée après le téléchargement du jeu, dans cette fonction, vous
pouvez utiliser les variables du jeu.

En effet, dans le module sprite met en œuvre deux modules: les sprites et les pixels. Mais
depuis qu'ils sont utilisés ensemble, ils sont placés dans un seul module. Commençons avec
les sprites:

#### Sprites

Pour créer un sprite, utilisez le sprite.de nouvelles, par exemple:

``
déclarer "my_spr' (sprite.nouveaux " gfx/oiseau.png')
locaux de coeur = sprite.nouveau coeur.png'
local vide = sprite.nouveau (320, 200) -- un sprite vide de 320 x 200
``

Avoir créé un objet sprite possède les méthodes suivantes:

- :alpha(alpha) - crée une nouvelle image-objet avec l'opacité alpha
 (255 signifie transparent). C'est un très lente de la fonction;
- :dup() - crée une copie du sprite;
- :l'échelle(xs, ys, [lisse]) -- échelles de sprite pour les réflexions à l'aide de l'échelle -1.0 (
 lent! pas en temps réel). Crée une nouvelle image-objet.
- :rotate(angle [lisse]) -- tourne le sprite par le biais d'un angle spécifié en 
 les degrés (lentement! pas en temps réel). Crée une nouvelle image-objet.
- :size () - Renvoie la largeur et la hauteur de l'image-objet en pixels.
- :draw(fx, fy, fw, fh, dst\_spr, x, y, [alpha]) -- Dessin le domaine de la src 
 le sprite du sprite de la zone de l'heure d'été (job alpha beaucoup ralentit l'exécution 
 de la fonction).
- :draw(dst_spr, x, y, [alpha]) -- le dessin de la sprite, recadrée en option; (job 
 alpha ralentit l'exécution de la fonction).
- :copie(fx, fy, fw, fh, dst\_spr, x, y) -- la Copie rectangle fw-fh de l' 
 sprite dans le sprite de l'heure d'été\_spr par des coordonnées [x,y] (dessin de substitution). 
 Il y a une version raccourcie (comme :tirage).
- :composer(fx, fy, fw, fh, dst\_spr, x, y) -- le Dessin donné, avec l' 
 la transparence des deux sprites). Il y a une version raccourcie
 (comme dans :tirage).
- :remplir(x, y, w, h, [col]) -- Remplissez sprite avec une couleur.
- :remplir([col]) -- Remplissez sprite avec une couleur.
- :le pixel(x, y, col, [alpha]) de Remplissage des pixels d'une image-objet.
- :le pixel(x, y) -- la capture du pixel de l'image-objet (retour en quatre couleurs 
composant).
- :couleur-clé(couleur) -- Définit le sprite de couleur, qui joue le rôle d'un 
 arrière-plan transparent. Ainsi, lors de l'utilisation ultérieure :copie de la présente 
 sprite seront copiés uniquement les pixels dont la couleur ne correspond pas à l' 
 arrière-plan transparent.

Comme la "couleur" des méthodes de recevoir des chaîne de caractères: 'vert', 'rouge', 'jaune' ou '#333333',
'#2d80ff'...

Exemple:

``
 local spr = sprite.nouveau(320, 200)
 spr:remplissez le 'bleu'
 local spr2 = sprite.de nouveaux poissons.png'
 spr2:draw(spr, 0, 0)
``

En outre, il ya la possibilité de travailler avec des polices. La police est
créé avec l'aide de sprite.fnt(), par exemple:

 une police locale = sprite.fnt('sans.ttf', 32)

Ont créé l'objet définit les méthodes suivantes:

- :hauteur() -- hauteur de la police en pixels;
- :texte(texte, col, [style]) -- créer un sprite à partir du texte de col - ici la couleur 
 dans le format texte (dans le format "#rrvvbb " ou "la couleur du texte').
- :la taille(texte) -- calcule la taille du texte à l'occupera sprite, sans 
 la création d'un sprite.

Vous pouvez également trouver utile de:

sprite.font_scaled_size(taille)

Il renvoie la taille de la police en tenant compte de la mise à l'échelle de mettre le joueur dans
les paramètres de la PLACE. Si vous êtes dans le jeu que vous souhaitez remarque ce paramètre utiliser
cette fonction afin de déterminer la taille de la police.

Exemple:

 local f = sprite.fnt('sans.ttf', 32)
 local spr = sprite.nouvelle ("boîte:320 x 200,noir)
 f:text("BONJOUR!", 'blanc'):tirage au sort(spr, 0, 0)

Maintenant, considérons l'application du module de sprite.

#### La fonction pic

Une fonction peut renvoyer une photo pour un sprite. Vous pouvez façonner chaque fois qu'un nouveau sprite
(ce qui n'est pas très efficace), ou peut-retour préaffectés sprite. Si l'
sprite modifications, ces modifications seront prises en compte dans le prochain cadre de la
jeu. Donc, changer le sprite de la minuterie pour faire l'animation:

``
besoin de "sprite"
besoin de "timer"
local spr = sprite.nouveau(320, 200)

la fonction de jeu:timer()
 local col = { 'rouge', 'vert', 'bleu'}
 col = col[rnd(3)]
spr:remplir(col)
 return false -- Important! Ainsi, la scène ne sera pas changé
fin

jeu.pic = function() retour spr end -- fonction: en tant que
-- sprite est un objet spécial (pas de chaîne)

la fonction start()
minuterie:ensemble(30)
fin

chambre {
 nam = "principal";
 décor = [[l'HYPNOSE]];
}
``

#### Le rendu en arrière-plan

La fonction sprite.scr() retourne le sprite de fond. Vous pouvez effectuer le rendu
ce sprite en tout gestionnaire, par exemple, dans le timer. La plupart cherchent à
modifier l'arrière-plan à la volée, sans avoir à utiliser le module de thème. Pour
exemple:

``
--$Auteur: Andrew Lobanov 

require 'sprite'
besoin d'un thème"
require 'minuterie'

déclarer {
 x = 0,
 y = 0,
 dx = 10,
 dy = 10,
}

const {
 w = thème.la scr.w(),
 h = thème.la scr.h(),
}

au lieu de cela.la décoloration = false

local bg, rouge, vert

la fonction init()
 thème.set('scr.col.bg', '#000000')
 thème.set('gagner.col.fg', '#aaaaaa')
 thème.set('gagner.col.lien', '#ffaa00')
 thème.set('gagner.col.alink', '#ffffff') 

 bg = sprite.nouveau(w, h)
bg:remplir ("noir")
 rouge = sprite.nouveau(w, h)
rouge:fill('#ff0000')
 rouge = rouge:alpha(128)
 vert = sprite.nouveau(w, h)
vert:fill('#00ff00')
 vert = vert:alpha(64)
bg:copie(sprite.scr())
minuterie:set(25)
fin

la fonction de jeu:timer()
bg:copie(sprite.scr())
 rouge:draw(sprite.scr(), x, 0, 128)
 vert:draw(sprite.scr(), 0, y, 64)
 x = x + dx
 si x >= w ou x == 0 alors
 dx = -dx
fin
 y = y + dy
 si y >= h ou y == 0 alors
 dy = -dy
fin
 return false -- Important!
fin

chambre {
 nam = "main",
 disp = 'Test. Test? Test de!',
 décor = 'Lorem ipsum';
}
``

_Caution!_ Interprète au LIEU d'utiliser l'élément de l'objet se transforme lui-même
dans le mode "pause". Cela signifie que, au moment où l'élément sélectionné
à partir de l'inventaire (le curseur de changement de vitesse) les événements de la minuterie ne seront pas traitées
jusqu'à la jusqu'à ce que le joueur s'appliquent le sujet. Ceci est fait afin de ne pas
casser le rythme du jeu. Si votre idée créative c'est un obstacle (par exemple,
vous n'aimez pas le fait que l'animation d'arrière-plan se fige), vous pouvez modifier
en appelant:

au lieu de cela.wait_use(faux)

Comme d'habitude, le lieu de l'appel dans le init() ou la fonction start ().

#### Recherche

Vous pouvez créer votre système objet de substitution, et de faire des sorties graphiques
du jeu, avec img, par exemple:

``
besoin de "sprite"
besoin de "timer"
exiger "fmt"

obj {
 nam = '$spr';
{
 ["carré"] = sprite.nouveau "de la boîte:32 x 32,rouge';
};
 act = fonction(s, w)
 de retour de l'esf.img(s[w])
fin
}

chambre {
 nam = "principal";
 décor = [[Maintenant nous insérer le sprite: {$spr|place}.]];
}
``

#### mode direct

À la PLACE, il y a un accès direct à la carte. Dans le sujet il est
spécifiée à l'aide du paramètre:

 la scr.gfx.mode = direct

Cette option peut être pré-série thématique.ini, ou de l'utilisation du module de thème. Ou (mieux) spécial
fonction:

sprite.direct(vrai)

Si le régime a réussi à comprendre: la fonction renvoie true.
sprite.direct() sans paramètre -- retourne le mode actuel (vrai-si
direct inclus).

Dans ce mode, le jeu dispose d'un accès direct à l'ensemble de la fenêtre et peut effectuer
le rendu dans la procédure de la minuterie. L'écran présente un spécial
sprite:

sprite.scr()

Par exemple:

``
besoin de "sprite"
besoin de "timer"
besoin de "thème"

sprite.direct(vrai)

des stars locales = {}
local w, h
couleurs locales = {
"rouge",
"vert",
"bleu",
"blanc",
"jaune",
"cyan",
"gris",
"#002233",
}

la fonction de jeu:timer()
 local scr = sprite.scr()
 scr:remplissez le "noir"
 pour i = 1, #étoiles ne s = étoiles[i]
 scr:le pixel(s).x, s.y, couleurs[s.dy])
 s.y = s.y + s.dy
 si s.y >= h alors
 s.y = 0
 s.x = rnd(w) - 1
 s.dy = rnd(8)
fin
fin
fin

la fonction start()
 w, h = thème.la scr.w(), thème.la scr.h()

 w = std.tonum(w)
 h = std.tonum(h)

 pour i = 1, 100
 table.insert(étoiles, { x = rnd(w) - 1, y = rnd(h) - 1, dy = rnd(8) })
fin
minuterie:ensemble(30)
fin
``

Un autre exemple:

``
besoin de "timer"
besoin de "sprite"
besoin de "thème"

local spr = sprite

déclarer {
 fnt = false, la balle = false, ballw = 0,
 ballh = 0, bg = false, ligne = false,
 G = false, par = false, bv = false,
 bx = false, t1 = false,
}

la fonction init()
 fnt = spr.fnt(thème.obtenez de gagner.fnt.nom', 32);
 balle = fpn:texte("au LIEU de 3.0", "blanc", 1);
 ballw, ballh = ballon:taille();
 bg = spr.nouvelle boîte:640 x 480,noir";
 ligne = spr.nouvelle boîte:320x8,lightblue';
spr.direct(vrai)
fin

la fonction start()
minuterie:set(20)
 G = 9.81
 par = -ballh
 bv = 0
 bx = 320
 t1 = à la place.les tiques()
fin

fonction phys()
 local t = timer:get() / 1000;
 bv = bv + G * t;
 en = en + bv * t;
 si par > 400 puis
 bv = - bv
fin
fin

la fonction de jeu:minuteur(s)
 local je
 pour i = 1, 10 ne
phys()
fin
 si à la place.les tiques() - t1 >= 20 alors
 bg:copie(spr.scr(), 0, 0);
 balle:draw(spr.scr(), (640 - ballw) / 2, par ballh/2);
 ligne:tirage au sort(spr.scr(), 320/2, 400 + ballh / 2);
 t1 = à la place.les tiques()
fin
fin
``

_Caution!_ le mode direct peut être utilisé pour créer un simple jeux d'arcade. Dans certains
cas, vous souhaiterez peut-être supprimer le pointeur de la souris. Par exemple, lorsque le jeu est
contrôlé uniquement avec le clavier.

Pour ce faire, utilisez la fonction à la place.souris\_show()

au lieu de cela.mouse_show(faux)

Sur le menu de l'interprète à la PLACE du pointeur de la souris sera toujours
visible.

#### À l'aide du sprite avec le module thème

En début et dans le gestionnaire, vous pouvez modifier les paramètres de thème avec comme
les graphiques de la sprites, par exemple:

``
besoin de "sprite"
besoin de "thème"

la fonction start() -- remplacer l'arrière-plan de sprite
 local spr = sprite.nouveau(800, 600)
 spr:remplissez le 'bleu'
 spr:remplir (100, 100, 32, 60, 'rouge')
 thème.set('scr.gfx.bg', spr)
fin
``

En utilisant cette technique, vous pouvez appliquer de l'image d'arrière-plan des statuts,
les contrôles, ou tout simplement de changer le substrat.

#### Pixels

Le sprite module prend également en charge le travail avec des graphiques en pixels. Vous pouvez créer
objets: les ensembles de pixels, de les modifier et de dessiner les sprites.

La création de pixels est une fonction de pixels.new().

Exemples:

``
local p1 = pixels.nouveau(320, 200) -- a créé un 320 x 200 pixels
local p2 = pixels.nouveaux " gfx/apple.png' -- créé pixels de l'image
locaux p3 = pixels.nouveau(320, 200, 2) -- créé un 320x200 pixels,
-- que lorsque vous rendre à un sprite-sera réduite à
-- 640x400
``

Les pixels de l'objet possède les méthodes suivantes:

> dans la description des symboles utilisés: r, g, b, a --
> les composants d'un pixel: rouge, vert, bleu, et de la transparence. Tous
> les valeurs de 0 à 255). les coordonnées x, y du coin supérieur gauche, w, h
> -- largeur et la hauteur de la zone.

- :size () - renvoie la taille et de l'échelle (3 valeurs);
- :claire(r, g, b, [a]) -- ce qui facilite le nettoyage de pixels;
- :remplissage(r, g, b, [a]) -- remplir (avec transparence);
- :remplir(x, y, w, h, r, g, b, [a]) -- la zone de remplissage (avec transparence);
- :val(x, y, r, g, b, a) - ensemble de valeur de pixel
- :val(x, y) -- pour obtenir les composantes r, g, b, a
- :le pixel(x, y, r, g, b, a) - dessiner un pixel (en tenant compte de la transparence 
 de l'existant pixel);
- :line(x1, y1, x2, y2, r, g, b, a)--;
- :lineAA(x1, y1, x2, y2, r, g, b, a) -- la ligne AA;
- :le cercle(x, y, rayon, r, g, b, a) - cercle;
- :circleAA(x, y, rayon, r, g, b, a) est le cercle de AA;
- :poly({x1, y1, x2, y2, ...}, r, g, b, a) - polygone;
- :polyAA({x1, y1, x2, y2, ...}, r, g, b, a) - polygone avec AA;
- :mélange de(x1, y1, w1, h1, pixels2, x, y) -- dessiner une zone de pixels dans un autre objet 
 pixels, en pleine forme;
- :mélange(pixels2, x, y) - court formulaire;
- :remplissez le\_circle(x, y, rayon, r, g, b, a) -- cercle rempli;
- :remplissez le\_triangle(x1, y1, x2, y2, x3, y3, r, g, b, a) -- triangle plein;
- :remplissez le\_poly({x1, y1, x2, y2, ...}, r, g, b, a) -- rempli de polygone;
- :copie (...) - comme un mélange, mais pas à dessiner et à copier (rapidement);
- :l'échelle, xscale, yscale, [lisse]) -- l'échelle de l'objet nouveau pixels;
- :rotate(angle [lisse]) -- le tour du nouvel objet en pixels;
- :draw_spr(...) -- comme dessiner, mais dans le sprite, pas de pixels;
- :copy_spr(...) -- la copie, mais dans le sprite, pas de pixels;
- :compose_spr(...) -- même chose, mais en mode composition;
- :dup() -- double de pixels;
- :sprite() -- créer un sprite avec des pixels.

Aussi, il est possible de travailler avec des polices:

- pixels.fnt(fnt(de la police.ttf, taille): création d'une police;

Dans ce cas, l'objet créé est "font" il est la méthode de texte:

- :texte(le texte, la couleur(comme dans les sprites), style) -- créer des pixels de texte;

Par exemple:

``
local fnt = pixels.fnt("sans.ttf", 64)
local t = fpn:texte("HELLO, PLACE!", "noir")
pxl:copy_spr(sprite.scr())
pxl2:draw_spr(sprite.scr(), 100, 200);
t:draw_spr(sprite.scr(), 200, 400)
``

Un autre exemple (par exemple, Andrei Lobanov):

``
besoin de "sprite"
besoin de "timer"

sprite.direct(vrai)

déclarer "pxl' (false)
déclarer 't' (0)

la fonction de jeu:timer()
 local x, y, i
 t = t + 1
 pour x = 0, 199 ne
 pour y = 0 à 149 ne
 i = (x * x + y * y + t)
 pxl:val(x, y, 0, i, i / 2)
fin
fin
pxl:copy_spr(sprite.scr())
fin

la fonction start(charge)
 pxl = pixels.nouveau(200, 150, 4)
minuterie:set(20)
fin
``

Lorsque la procédure de génération à l'aide de pixels, il est pratique d'utiliser le bruit de Perlin. Dans
Au LIEU de cela, il existe des fonctions:

- à la place.noise1(x) - 1D le bruit de Perlin;
- à la place.noise2(x, y) - 2D bruit de Perlin;
- à la place.noise3(x, y, z) est un 3D le bruit de Perlin;
- à la place.noise4(x, y, z, w) - 4D bruit de Perlin;

Toutes ces fonctions renvoient une valeur dans l'intervalle [-1; 1] et à l'entrée
obtenir les coordonnées de la virgule flottante.

### Module snd

Nous avons déjà discuté les fonctionnalités de base de travail avec le son. Module
snd a certaines fonctions pour travailler avec le son.

Vous pouvez charger le son et de le garder en mémoire tant qu'il vous faut.

 require 'snd'
 local wav = snd.nouveau 'écorce.ogg'

En plus de télécharger des fichiers, vous pouvez charger le son à partir d'un tableau de lua:

``
local wav = {}
pour i = 1, 10000 ne
 table.insert(wav, rnd() * 2 - 1) -- des valeurs de -1 à 1
fin

la fonction start()
 -- la fréquence, le nombre de canaux et de son
 local p = snd.nouveau(22050, 1, wave)
p:jouer()
fin
``

Le son est spécifié dans le format normalisé: [-1 .. 1]

Le son, vous pouvez méthode :play([chan], [boucle]), où chan-canal
(0 - 7), boucle - cycles (0 - infini).

Autres fonctions du module:

- snd.stop([channel]) – pour arrêter la lecture du canal sélectionné ou tous les
 les canaux. Le second paramètre, vous pouvez définir le temps de décroissance du son dans 
 MME quand il est coupé;
- snd.lecture([channel]) – pour savoir si le son est joué sur n'importe quel canal 
 ou sur le canal sélectionné; si le canal spécifique de la fonction 
 sera de retour poignée actuellement joué son ou nul. Attention! Le son 
 d'un clic n'est pas pris en compte et prend habituellement de 0 canal;
- snd.pan(chan, l, r) – réglez le panoramique. Le canal, le volume à gauche[0-255], le volume 
 de la droite[0-255] canaux. Vous devez appeler avant de jouer le son d'avoir 
effet;
- snd.vol(volume) – règle le volume du son (musique et effets) de 0 à 127.

Une autre possibilité intéressante -- son de la génération à la volée (pourtant il est en
situation expérimentale):

``
exiger "snd"

la fonction cb(hz, len, données)
 pour i = 1, len n'
 data[i] = rnd() * 2 - 1
fin
fin
la fonction start()
snd.music_callback(cb)
fin
``

### Le module préf.

Ce module vous permet d'enregistrer les paramètres de jeu. En d'autres termes, les sauvés
l'information ne dépend pas de l'état du jeu. Un tel mécanisme peut être
utilisé, par exemple, de mettre en œuvre un système de succès ou le comte de la
le nombre de passages de jouer.

Essentiellement préf est un objet, toutes les variables qui seront sauvés.

Enregistrer les paramètres:

préf:store()

Les paramètres sont automatiquement enregistrés lorsque vous enregistrez le jeu, mais vous pouvez
le contrôle de ce processus, ce qui entraîne la préf:store().

Détruire le fichier de configuration:

préf:purge()

Pour télécharger les paramètres automatiquement lorsque vous démarrez le jeu (avant d'appeler
la fonction start ()), mais vous pouvez lancer le téléchargement et le manuellement:

préf:load()

Exemple d'utilisation:

``
-- $Nom: module de Test préf.$
-- $Version: 0.1$
-- $Auteur: au lieu de$

-- plug-in, cliquez
exiger ", cliquez sur"
-- le plug-in préf.
exiger "prefs"

- de définir la valeur de départ du compteur
préf.compteur = 0;

-- définir une fonction permettant de comptabiliser le nombre de "clics"
jeu.onclick = function(s)
 -- incrémenter compteur
 préf.compteur = prefs.compteur + 1;
 -- persistante contre
préf:store();
 -- message d'impression
 p("actuellement ", préf.compteur de" clics");
fin;

-- ajout d'une image pour produire des clics
jeu.pic = 'boîte:320 x 200,noir";

chambre {
 nam = "main",
 title = "maison de clics",
 - rendre les parties statiques de la description
 -- ajout de la description de la scène
 décor = [[ Cet essai a été écrit spécifiquement
 afin de vérifier le fonctionnement du module <<préf>>.
]];
};
``

Veuillez noter qu'après avoir commencé le jeu à nouveau, le nombre de clics
ne pas réinitialiser!

### Module instantanés

Module instantanés offre la possibilité de restaurer la partie préalablement sauvegardée
état. Comme un exemple, il est possible de ramener la situation lorsque le joueur
effectue dans l'action du jeu, menant à la perte. Le module permet à l'auteur de
écrire le code du jeu pour que le joueur sera de retour à la
l'état du jeu.

Pour créer un instantané d'utilisation: instantanés:faire(). Dans le paramètre peut être réglé à l'
nom de la fente.

_Caution!!!_ Instantané sera généré après avoir terminé l'étape en cours de
jeu, parce que dans ce cas, la garantie de la cohérence de la partie sauvegardée
état.

Est téléchargement instantané des instantanés:restore(). En tant que paramètre peut être défini sur le nom
de la fente.

Instantané de suppression est effectuée à l'aide d'instantanés:remove(). Devrait supprimer
inutile instantanés, car ils prennent de l'espace supplémentaire dans les fichiers enregistrer.

Exemple d'utilisation:

``
exiger des "instantanés"

chambre {
 nam = "principal";
 title = "Jeu";
 le onenter = function()
 instantanés:make () - créé un point de restauration
fin;
 décor = [[{#rouge|Rouge} ou {#noir|noir}?]];
}: avec {
 obj {
 nam = '#rouge';
 loi = function()
 p [[Vous avez gagné!]]
fin;
};
 obj {
 nam = '#noir";
 loi = function()
 marche "fin"
fin;
}
}

chambre {
 nam = 'fin';
 title = 'Fin';
}: avec {
 obj {
 dsc = [[{Replay?}]];
 loi = function()
 instantanés:restore() -- récupéré
fin;
}
}
``

## Méthodes de l'objet

Tous les objets STEAD3 il y a des méthodes qui sont utilisées dans la mise en œuvre de
la bibliothèque standard et ne sont généralement pas utilisés par l'auteur des jeux directement.
Cependant, il est parfois utile de connaître la composition de ces méthodes,
serait de ne pas le nom de vos variables et méthodes de noms déjà de l'
les méthodes existantes. Ci-dessous une liste de méthodes avec une brève description.

### Objet (. obj)

- :avec({...}) - liste obj;
- :nouvelle (...) constructeur;
- :les actions(type, [valeur]) - définir/lire le numéro de l'événement objet de la présélection 
type;
- :un coffre([{}]) - dans lequel la chambre (la chambre) se trouve l'objet;
- :d'où([{}]) - ce que l'objet (les objets) est un objet;
- :purge() - supprime l'objet de toutes les listes.
- :remove() - supprimer l'objet de tous les objets/les chambres/équipements;
- :close ()/open() - fermer/ouvrir;
- :disable()/:enable() - désactiver/activer;
- :fermé() -- renvoie true si elle est fermée;
- :désactivé() -- renvoie true si le paramètre est désactivé;
- :empty() -- renvoie true si vide;- :save(fp, n) -- une fonction de sauvegarde;
- :afficher() -- la fonction d'affichage de la scène;
- :visible() -- renvoie true si elle est visible;
- :srch(w) - la recherche de l'objet visible;
- :lookup(w) - recherche de n'importe quel objet;
- :for_each(fn, ...) -- itérateur sur les objets;
- :lifeon()/:lifeoff() -- ajouter/supprimer de la liste des vivants;
- :live() -- renvoie true si la liste des vivants.

### Chambre (la chambre)

Outre les méthodes de l'obj, a ajouté l'une des méthodes suivantes:

- :à partir de() -- où je suis entré dans la chambre;
- :visited() -- il y a une pièce que vous avez visité plus tôt?;
- :visites() -- le nombre de visites (0 sinon);
- :scene() -- scène d'affichage (pas d'objets);
- :afficher() -- affichage des objets dans la scène;

### Les boîtes de dialogue (dlg)

Outre les méthodes de la salle, a ajouté l'une des méthodes suivantes:

- :push(une phrase) - aller à la phrase, il se souvenir de la pile;
- :reset(une phrase) - même, mais de réinitialiser la pile;
- :pop([phrase]) -- le retour de la pile;
- :sélectionnez([phrase]) -- sélectionner la phrase;
- :ph\_display() -- affichage de la phrase;

### Le monde du jeu (jeu de l'objet)

Outre les méthodes de l'obj, a ajouté l'une des méthodes suivantes:

- :le temps([v]) -- set/get le nombre de jeu de cycles;
- :lifeon([v])/:lifeoff([v]) - ajouter/supprimer un objet de la liste des vivants, ou 
 activer/désactiver les inscriptions en ligne à l'échelle mondiale (si non spécifié argument);
- :live([v]) -- vérification de l'activité de la durée de vie de l'objet;
- :set\_pl(pl) -- changement de joueur
- :vie (la) - l'itération de vivre objets;
- :step() -- le tact du jeu;
- :lastdisp([v]) - set/get la dernière sortie;
- : (affichage d'état) -- affichage de la sortie;
- :lastreact([v]) - set/get la plus récente de la réaction;
- réaction: ([v]) -- set/get réponse actuelle;
- :les événements(pré, bg) -- set/get événements de vivre objets;
- :cmd(cmd) -- la commande à la PLACE;

### Player (joueur)

Outre les méthodes de l'obj, a ajouté l'une des méthodes suivantes:

- :déplacé (a) - le joueur fait un mouvement dans le cycle actuel du jeu;
- :need_scene([v]) - nécessité de rendre la scène dans ce cycle;
- :inspecter(w) - trouver un objet (visible) dans la scène actuelle ou vraiment;
- :ont(w) - recherche dans l'inventaire;
- :useit(w) - utiliser l'élément;
- :useon(w, ww) -- utilisez le point sur le sujet;
- :appelez le(m, ...) -- l'appel d'une méthode d'un joueur;
- de l'action: (w) - action sur le sujet (la loi);
- :inventaire () - renvoie l'inventaire (liste, valeur par défaut est obj);
- :prendre(w) -- prendre l'objet;
- :marche/walkin/débrayage -- transitions;
- :aller(w) - l'équipe de go (vérification de la disponibilité des transitions);

## Épilogue

C'est ainsi que la documentation se termine. Mais peut-être que commence le plus intéressant
- votre histoire!

J'ai fait la première version de la PLACE en 2009. À l'époque, je n'ai jamais pensé
que mon jouet (et le moteur) aurait survécu à de nombreux changements. Texte
aventures existent encore, comme je l'ai écris cet épilogue en 2017. De même, leur impact
sur notre culture reste minime, et bien écrit aventures sont loin et quelques
entre.

Les jeux mettant en vedette un mélange de texte et de graphismes ont un énorme potentiel dans mon
de l'opinion. Même s'ils sont moins interractive, ils sont moins exigeants
le joueur. Bénéficiant d'un texte de l'aventure ne sera pas consommer les jours de votre vie passée
en face de l'écran la souffrance, de la frustration ou insalubres que votre anxiété
les désirs sont laissé insatisfait. Textuelle jeux d'emprunter à partir de la meilleure que les deux
les mondes de liturature et les jeux d'ordinateur ont à offrir. En bonus, la
une grande partie des jeux de ce genre peut être apprécié sans coût.

L'histoire de la PLACE, c'est, à mon avis, la preuve de cette affirmation. De nombreux jeux ont
été publié pour la PLACE qui peut en toute sécurité être appelée la grande! Leurs auteurs peuvent
à la retraite, mais ils ont créé des œuvres sont déjà de vivre leur vie, qui se reflète dans
la conscience des gens, qui jouent entre eux ou de se rappeler. Laissez la "circulation"
de ces jeux n'est pas si grand, mais de ce que j'ai vu complètement "justifié" tous les
les efforts dépensés sur le moteur. Je sais que c'est du temps bien dépensé. J'ai donc trouvé une puissance, et
faites le moteur encore mieux, libérant STEAD3. J'espère qu'il et comme vous.

Donc, si vous avez lu jusqu'ici, je ne peux que vous souhaiter ajouter votre première histoire.
La créativité -- c'est la liberté. :)

Merci à vous et bonne chance. Peter Inclinaison, de Mars 2017.

