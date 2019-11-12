## Présentation

Code de jeux sous INSTEAD est écrit en langage Lua (5.1), c'est pourquoi, la connaissance de cette
la langue est utile, mais pas nécessaire. Le noyau du moteur est également écrit sur
lua, par conséquent, la connaissance de Lua peut être utile pour une meilleure compréhension de
les principes de son travail, bien sûr, si vous vous demandez ce
faire.

Au cours de son développement, INSTEAD a reçu de nombreux nouveaux
fonctions. Maintenant, avec son aide, vous pouvez faire des jeux de différents genres (de
jeux d'arcade, les jeux avec la saisie de texte). Ainsi, dans INSTEAD, vous pouvez exécuter
jeux écrits sur d'autres moteurs, mais le fondement de INSTEAD
reste le noyau initial, qui se concentre sur la création de
текстографических de jeux d'aventure. Dans cette documentation décrit
c'est cette base de la partie, l'étude de laquelle vous devez même
si vous voulez écrire quelque chose d'autre... Commencez votre recherche de
INSTEAD avec l'écriture d'un simple jeu!

En février 2017, après 8 ans de développement, INSTEAD (version 3.0)
sorti avec le soutien d'un nouveau noyau STEAD3. L'ancien noyau a reçu le nom de
STEAD2. INSTEAD de travail prend en charge les jeux écrites sur STEAD2, ainsi
et sur STEAD3. Ce guide décrit STEAD3.

Si vous avez des questions, vous pouvez visiter le site INSTEAD:

https://instead-hub.github.io

Ou se connecter au chat sur gitter:

https://gitter.im/instead-hub/instead

### L'histoire de la création

Quand nous disons "une aventure" la plupart des gens se produit
l'un des deux habituelles images. C'est un texte avec des boutons d'action
par exemple:

 Vous voyez en face de la table. Sur la table est la pomme. Que faire?

 1) Prendre une pomme
 2) de s'Éloigner de la table

Ou, beaucoup plus rarement, c'est le jeu classique avec la saisie de texte, où
jeu de gestion, vous devez y saisir clavier.

 Vous êtes dans la cuisine. Il ya un bureau.
 > regarder la table.
 Sur la table une pomme.

Les deux approches ont leurs avantages et inconvénients.

Si parler de la première approche, il est proche du genre de livres-jeux et
convient plus pour les textes littéraires, qui décrivent _события_,
se produisant avec le personnage principal, et n'est pas très utile pour la création de
de quêtes classiques, où le personnage principal explore _смоделированный_ dans
jeu _мир_ tout en vous déplaçant librement et en interagissant avec les objets
de ce monde.

La deuxième approche modélise le monde, mais exige beaucoup d'efforts de l'auteur
jeux, et, plus important encore, formé d'un joueur. En particulier,
quand nous avons affaire avec la langue russe.

Le projet INSTEAD a été créé pour l'écriture d'un autre type de jeux qui
combinent les avantages des deux approches, tout en essayant d'éviter les
leurs inconvénients.

Le monde du jeu sur INSTEAD est modélisé comme lors de la deuxième approche, c'est dans
le jeu il y a des endroits (la scène ou la salle) qui peut se rendre dans le chef de
le héros et les objets avec lesquels il communique (y compris les vivants
les personnages). Le joueur libre d'explorer le monde et manipule
des objets. En outre, l'action avec les objets ne sont pas énoncées dans la forme explicite
les éléments de menu, mais plutôt de rappeler les classiques graphiques quêtes
style 90-h.

En fait, INSTEAD il existe une multitude de imperceptibles à première vue
des choses, qui se concentrent sur le développement de l'approche choisie, et qui
rend le jeu maximal et différent des habituelles
"les quêtes". Ceci est confirmé par le fait que la
le moteur a été libéré une variété de grands jeux, l'intérêt qui
manifeste non seulement les amateurs de jeux en tant que tels, mais les gens ne sont pas
familier avec le genre.

Avant l'étude de ce guide, je vous recommande de jouer en
jeux classiques INSTEAD, pour comprendre de quoi il s'agit. D'autre
part, une fois que vous êtes là, vous êtes sans doute déjà fait.

La vérité, pas la peine d'étudier jusqu'à ce que le code de ces jeux, comme le vieux jeu très
souvent écrits неоптимально, avec les anciens
les structures. La version actuelle INSTEAD permet l'exécution de code
concise, simple et claire. Ce sujet et décrit dans ce
document.

Si vous êtes intéressé par l'histoire de la création du moteur, vous pouvez le lire
un article sur comment tout a commencé: https://instead-hub.github.io/article/2010-05-09-history/

### À quoi ressemble le classique INSTEAD jeu

Alors, à quoi ressemble le classique INSTEAD jeu?

_Главное окно_ de jeu contient un récit de partie, des informations sur
statique et dynamique de la scène, actifs de l'événement et de l'image
la scène (dans le graphique de l'interpréteur) avec de possibles transferts à d'autres
de la scène.

_Описательная partie сцены_ n'apparaît qu'une seule fois, lors de l'affichage
de la scène, ou sur demande explicite de l'inspection de la scène (dans le graphique de l'interpréteur --
_Статическая partie сцены_ contient des informations sur les objets
scène (d'habitude, c'est le décor) et s'affiche toujours. Cette partie de la
écrit par l'auteur du jeu.

_Динамическая partie сцены_ est composé de descriptions de scènes d'objets,
qui sont présents en ce moment sur la scène. Cette partie est générée
automatiquement et s'affiche toujours. D'habitude, il présente
les objets qui peuvent changer d'emplacement.

Le joueur peut d'objets disponibles sur n'importe quelle scène --
_инвентарь_. Le joueur peut interagir avec les objets de l'inventaire et
agir avec les objets de l'inventaire sur les autres objets de la scène ou de l'inventaire.

> Il est à noter que la notion d'inventaire est conditionnelle. Par exemple,
> dans "l'inventaire" peuvent se trouver des objets tels que "ouvrir",
> "explorer", "utiliser", etc.

_Действиями_ joueurs peuvent être:

- l'inspection de la scène;
- l'action sur l'objet de la scène;
- l'action sur l'objet de l'inventaire;
- l'action de l'objet de l'inventaire sur l'objet de l'inventaire;
- l'action de l'objet de l'inventaire sur l'objet de la scène;
- le passage à une autre scène.

### Comment créer un jeu

Le jeu est le répertoire dans lequel doit se trouver le script
(un fichier texte) main3.lua. (Notez la présence d'un fichier
main3.lua signifie que vous écrivez le jeu sur STEAD3!) Les autres ressources du jeu
(les scripts lua, le graphisme et la musique) doivent se trouver dans le cadre de ce
un catalogue. Tous les liens vers les ressources sont relativement à l'
catalogue -- le répertoire du jeu.

Au début du fichier main3.lua peut être défini par le titre, composé de
les balises (lignes d'un type spécial). Les tags doivent commencer par les caractères:
deux instrumentaux.

--

Deux moins c'est un commentaire du point de vue de Lua. En ce moment
il existe les balises suivantes.

La balise $Name: affiche le nom du jeu en UTF-8. Exemple
l'utilisation de la balise:

 -- $Name: la Plus intéressante du jeu!$

Ensuite, il faut (de préférence) pour spécifier la version du jeu:

 -- $Version: 0.5$

Et d'indiquer la paternité:

 -- $Author: Anonyme amateur de texte de l'aventure$

Pour plus d'informations sur le jeu, vous pouvez définir la balise Info:

 -- $Info: c'Est un remake d'un ancien jeu\ps ZX specturm$

Notez \n Info, ce sera un saut de ligne, quand vous
sélectionnez "Informations" INSTEAD.

Si vous développez un jeu dans Windows, assurez-vous que votre éditeur
prend en charge l'encodage UTF-8 _без BOM_. C'est ce codage doit être
utiliser lors de l'écriture du jeu!

Ensuite, il est généralement nécessaire de spécifier les modules requis par le jeu. Sur
les modules seront abordés séparément.

 require "fmt" -- certaines fonctions de mise en forme
 fmt.para = true -- activer le mode des paragraphes (retraits)

En outre, il vaut la peine de déterminer les gestionnaires par défaut:
game.act, game.use, game.inv, également sera expliqué ci-dessous.

 game.act = 'Ne fonctionne pas.';
 game.use = 'Cela ne fonctionne pas.';
 game.inv = 'Pourquoi moi?';

_Инициализацию_ jeu doit être décrit dans la fonction init, qui
appelé le moteur au début. Dans cette fonction pratique
initialiser l'état du joueur au début du jeu, ou d'autres
les étapes nécessaires à la configuration initiale du monde du jeu. D'ailleurs,
la fonction "init" peut-être en avez pas besoin.

 function init() -- allons ajouter à l'inventaire d'un couteau et du papier
 take 'couteau'
 take 'papier'
end

Une fois que le jeu проинициализирована, se fait un jeu de lancement. Vous
pouvez définir la fonction start(), qui est exécuté directement
avant de lancer le jeu. Cela signifie, par exemple, que dans le cas d'un téléchargement
savegame, start() sera appelée après l'enregistrement
lu


 function start(load) -- restaurer l'état?
 if load then
 dprint "Est le chargement de l'état."
else
 dprint "c'Est le début du jeu."
end
 -- nous n'avez rien à faire
end


Interpréteur graphique à la recherche de jeux disponibles dans le répertoire
games. Unix version de l'interpréteur à l'exception de ce qui regarde la page de catalogue
aussi des jeux dans le répertoire ~/.instead/games. Windows-version: Documents and
Settings/USER/Local Settings/Application Data/instead/games. Dans
Windows - et standalone-Unix-version du jeu est recherché dans le répertoire
./appdata/games, s'il existe.

Dans certains assemblages INSTEAD (Windows, Linux, si le projet est compilé avec
gtk etc.), vous pouvez ouvrir le jeu sur n'importe quel chemin à partir du menu "Sélection de jeux".
Ou, appuyer sur f4. Si le catalogue de jeux est présent une seule
le jeu, INSTEAD lancera automatiquement, ce qui est pratique si vous voulez
distribuer votre jeu avec le moteur.

Donc, vous mettez le jeu dans votre répertoire et lancez INSTEAD.

__Il est important!__

Lors de l'écriture du jeu, il est fortement recommandé d'utiliser l'indentation
pour la conception du code du jeu, comme cela est fait dans l'exemple de ce
de la direction, ce faisant, vous réduisez le nombre d'erreurs et faites votre
code visuellement!

Ci-dessous le minimum de modèle pour votre premier jeu:

``
-- $Name: Mon premier jeu$
-- $Version: 0.1$
-- $Author: auteur Anonyme de$

require "fmt"
fmt.para = true

game.act = 'Euh...';
game.use = 'Ne fonctionne pas.';
game.inv = 'Pourquoi moi?';

function init()
-- initialisation, si elle est nécessaire
end
``

### Base de débogage

En cours de débogage (vérifier l'intégrité de votre jeu) pratique pour
INSTEAD a été lancé avec l'option -debug, alors dans le cas d'erreurs
montre de plus amples informations sur le problème de la forme de la pile
des appels. L'option-debug, vous pouvez définir dans l'étiquette (si vous travaillez en
Windows), et pour les autres systèmes, je pense que vous savez comment transférer de la
paramètres de ligne de commande.

En outre, dans le mode debug se connecte automatiquement le débogueur. Vous
pouvez l'activer à l'aide des touches ctrl-d ou f7. Vous pouvez
attacher un débogueur et en spécifiant explicitement:

 require "dbg"

Dans le code de votre jeu.

Lors du débogage d'un jeu d'habitude vous devez enregistrer le jeu et le télécharger
l'état du jeu. Vous pouvez utiliser un mécanisme standard pour la sauvegarde du
par le menu (ou sur les touches f2/f3), ou utiliser rapide
enregistrer/charger (touche f8/f9).

En mode '-debug', vous pouvez redémarrer le jeu des touches
'alt-r'. En combinaison avec f8/f9 il donne la possibilité de voir rapidement
changements dans le jeu après son édition.

> Attention! Si vous redémarrez simplement INSTEAD, alors les chances
> verrez l'ancien état de jeu, comme il fonctionne par défaut en mode
> démarrage auto! Ou désactivez ce paramètre dans le menu
> (enregistrement automatique), ou explicitement, redémarrez le jeu après modifications.
> Redémarrage disponible par le menu (recommencer) ou 'alt-r' dans le mode
> '-debug' comme décrit ci-dessus.

En mode '-debug' Windows-version INSTEAD crée une fenêtre de console (en
Les versions d'Unix, si vous exécutez INSTEAD de la console, le résultat sera
envoyé à celle-ci) qui sera effectué à la sortie des erreurs. Sauf
outre, en utilisant la fonction 'print()' vous serez en mesure de produire ses
les messages de déboguer la sortie. Par exemple:

``
act = function(s)
 print ("Act is here! ");
...
end;
``

Ne vous inquiétez pas, quand vous lisez tout le manuel et commencez à écrire
votre jeu, vous, probablement, jetez un coup d'oeil sur cet exemple avec un grand
l'enthousiasme.

Vous pouvez également utiliser la fonction dprint(), qui envoie à la conclusion
dans la fenêtre du débogueur, et vous pourrez le voir lors de l'entrée dans le mode
le débogage.

``
act = function(s)
 dprint ("Act is here! ");
...
end;
``

Pendant le débogage, il est pratique d'étudier les fichiers de sauvegardes, qui
contiennent les variables d'état du jeu. Pour ne pas chercher à chaque fois les fichiers
sauvegardes de jeu, créez un répertoire saves dans le répertoire de votre jeu (dans le
le répertoire qui contient main3.lua) et le jeu sera maintenue
saves. Ce mécanisme sera aussi pratique pour transférer les jeux sur d'autres
les ordinateurs.

Peut-être (surtout si vous utilisez Unix) vous aimerez
l'idée de vérifier la syntaxe de vos scripts à travers le lancement du compilateur
"luac". Sous Windows c'est possible aussi, il suffit d'installer le
les fichiers exécutables de lua pour Windows
(http://luabinaries.sourceforge.net)/ et utiliser luac52.exe.

Vous pouvez vérifier la syntaxe et à l'aide de INSTEAD, pour ce
utilisez l'option -luac:

 sdl-instead -debug -luac <laisser sur le script.lua>

## Scène

_Сцена_ (ou chambre) est l'unité de jeu dans lequel le joueur
peut examiner tous les objets de la scène et d'interagir avec eux. Par exemple,
la scène peut être la pièce dans laquelle se trouve le héros. Ou site
la forêt, disponible pour l'observation.

Dans n'importe quel jeu doit être une scène appelée "main". C'est avec elle
et commencer votre jeu!

``
room {
 nam = 'main';
 disp = "pièce Principale";
 dsc = [[Vous dans la grande salle.]];
}
``

L'enregistrement implique la création d'un objet (comme presque toutes les entités INSTEAD
ce sont des objets de main de type room (salle de bains). L'attribut de l'objet nam stocke le nom de
la pièce 'main', par laquelle vous pouvez accéder à la salle de code. Chaque
objet possède un nom unique. Si vous essayez de créer deux
de l'objet avec le même nom, vous obtenez un message d'erreur.

Pour accéder à l'objet par le nom, vous pouvez utiliser la
enregistrement:

``
dprint (Objet: ", _'main')
``

Chaque objet du jeu il ya _атрибуты_ et _обработчики событий_. Dans
cet exemple possède deux attributs: nam et dsc. Les attributs sont séparés
un séparateur (dans cet exemple, -- un symbole de point-virgule ';').

D'habitude, les attributs peuvent être des chaînes de texte,
fonctions-les gestionnaires et les valeurs booléennes. Cependant, l'attribut nam
doit toujours être une chaîne de texte, s'il est défini.

En fait, vous ne pouvez pas spécifier un nom lors de la création de l'objet:

``
room {
 disp = "pièce Principale";
 dsc = [[Vous dans la grande salle.]];
}
``

Dans ce cas, le moteur lui-même donnera le nom de l'objet, et ce nom est un
le nombre. Comme vous ne savez pas ce nombre, vous ne pouvez pas accéder à
l'objet est clairement. Parfois, il est pratique de créer des objets sans nom, par exemple,
pour la décoration. Lors de la création de l'objet, même si elle est "sans nom", vous
pouvez créer une variable de référence à l'objet, par exemple:

``
myroom = room {
 disp = "Placard";
 dsc = [[Vous dans un placard.]];
}
``

La variable myroom dans ce cas, est synonyme de l'objet (lien
sur l'objet lui-même).

``
dprint (Objet: ", myroom)
``

Vous pouvez les suivre d'une façon ou d'appliquer
les deux. Par exemple, vous pouvez définir un nom et une variable de référence:

``
main_room = room {
 nam = 'main';
 disp = "pièce Principale";
 dsc = [[Vous dans la grande salle.]];
}
``
Il est important de comprendre que le moteur est en tout cas fonctionne avec les noms des objets, et
les variables référencées -- c'est juste un moyen de simplifier l'accès
utilisés à des objets. C'est pourquoi, pour notre premier match, nous sommes tenus de
spécifier l'attribut nam = 'main', pour créer une salle de main et
commence notre aventure!

Dans notre exemple, lors de l'affichage de la scène, en tant que tête de la scène
utilisé l'attribut 'disp'. En fait, si on n'a pas posé,
alors dans l'en-tête, nous verrions 'nam'. Mais le vietnam n'est pas toujours facile à faire
le titre de la scène, surtout si c'est une chaîne comme la 'main', ou si c'est
l'identifiant numérique de qui moteur a attribué à l'objet automatiquement.

Il y a plus intuitive de l'attribut 'title'. S'il est défini, lors de la
l'affichage de la pièce de titre figurera il.
title est utilisé lorsque le joueur se trouve _внутри_ de la pièce. Dans
tous les autres cas (lors de l'affichage de navigation dans cette salle)
utilisé 'disp' ou 'nam'.

``
mroom = room {
 nam = 'main';
 title = 'le Début de l'aventure';
 disp = "pièce Principale";
 dsc = [[Vous dans la grande salle.]];
}
``
L'attribut 'dsc' -- c'est la description de la scène qui s'affiche une fois lors de la
l'entrée en scène ou sur demande explicite de l'examen de la scène. Il n'y a pas de descriptions
les objets présents dans la scène.

Vous pouvez utiliser le caractère ',' au lieu de ';' pour séparer les
les attributs. Par exemple:

``
room {
 nam = 'main',
 disp = 'pièce Principale',
 dsc = 'êtes-Vous dans la grande salle.',
}
``
Dans cet exemple, tous les attributs -- chaîne. La chaîne peut être enregistrée
en simples ou doubles guillemets:

``
room {
 nam = 'main';
 disp = 'pièce Principale';
 dsc = "êtes-Vous dans la grande salle.";
}
``

Pour les longues descriptions commode d'utiliser l'enregistrement de l'aspect:


``
dsc = [[ une Très longue description... ]];
``

Ce faisant, les traductions de la ligne sont ignorés. Si vous voulez à la sortie
description de la scène étaient présents paragraphes -- utilisez le caractère '^'.

``
dsc = [[ le Premier paragraphe. ^^
Le Deuxième Paragraphe.^^

Le troisième paragraphe.^
Sur une nouvelle ligne.]];
``

Je recommande toujours d'utiliser la [[ et ]] pour 'dsc'.

Je vous rappelle encore une fois que le nom 'nam' de l'objet et de l'afficher (dans ce
le cas de ce que la scène va ressembler pour le joueur sous la forme d'inscriptions en haut)
vous pouvez (et souvent, il faut!) partager. Pour cela, il existe des attributs
'disp' et 'title'. 'title' arrive seulement les pièces et fonctionne comme
le spécificateur, quand un joueur se trouve à l'intérieur de cette chambre. Dans les autres
cas, utilisé 'disp' (si il existe).

Si 'disp' et 'title' n'est pas définie, il est considéré que l'affichage
égale nom.

'disp' et 'title' peuvent prendre la valeur false dans ce cas,
l'affichage ne sera pas.

## Objets

_Объекты_ -- c'est l'unité de la scène, qui interagissent avec le joueur.

``
obj {
 nam = 'table';
 dsc = 'Dans la chambre coûte {table}.';
 act = 'Euh... Juste une table...';
};
``

Le nom de l'objet "nam" est utilisé en cas de contact avec lui dans l'inventaire. Bien que,
dans notre cas, le bureau d'à peine y pénétrait. Si l'objet détecté
'disp', lors de la pénétration dans l'inventaire de son affichage
utilisée avec cet attribut. Par exemple:

``
obj {
 nam = 'table';
 disp = 'l'angle de la table';
 dsc = 'Dans la chambre coûte {table}.';
 tak = 'J'ai entrepris l'angle de la table';
 inv = 'Je tiens à l'angle de la table.';
};
``
Tout le bureau est venu à nous dans l'inventaire.

Vous pouvez masquer l'affichage de l'objet dans l'inventaire, si 'disp'
l'attribut est égal à 'false'.

'dsc' -- description de l'objet. Il affiche dans la partie dynamique
de la scène, en présence de l'objet dans la scène. Les accolades s'affiche
le fragment de texte qui servira de référence dans la fenêtre de INSTEAD. Si
les objets dans la scène de beaucoup, toutes les descriptions s'affichent l'une après l'autre,
à travers l'espace,

'act' -- c'est un gestionnaire de l'événement, qui est appelée lors de l'action
utilisateur (action sur un objet d'une scène, d'habitude -- clic de la souris sur
le lien). Sa fonction principale est de sortie (retour) de la ligne de texte
qui fera partie des événements de la scène, et le changement d'état de jeu
du monde.

## Ajouter des objets dans la scène

Pour mettre en scène des objets, il existe plusieurs manières.

Tout d'abord, lors de la création de la pièce, vous pouvez définir la liste des 'obj',
composé de noms d'objets:

``
obj { -- l'objet de nom, mais sans variable
 nam = 'boîte';
 dsc = [[Sur le plancher, je vois {boîte}.]];
 act = [[Lourd!]];
}

room {
 nam = 'main';
 disp = 'une Grande salle';
 dsc = [[Vous dans la grande salle.]];
 obj = { 'boîte' };
};
``
Maintenant, lors de l'affichage de la scène, nous allons voir l'objet de la "boîte" dans la dynamique de l'
partie.

Au lieu du nom de l'objet, vous pouvez utiliser la variable de référence, si
seulement il a été défini à l'avance:

``
apple = obj { -- l'objet à une variable, mais sans le nom de
 dsc = [[Ici, il ya {pomme}.]];
 act = [[Rouge!!]];
}

room {
 nam = 'main';
 disp = 'une Grande salle';
 dsc = [[Vous dans la grande salle.]];
 obj = { apple };
};
``

L'autre forme d'écriture est la construction with:

``
room {
 nam = 'main';
 disp = 'une Grande salle';
 dsc = [[Vous dans la grande salle.]];
}:with {
'lettres',
}
``
La construction with permet de se débarrasser de l'excès de niveaux d'imbrication dans
le code du jeu.

Deuxièmement, vous pouvez déclarer les objets à l'intérieur de l'obj ou with,
décrivant leur définition:

``
room {
 nam = 'main';
 disp = 'une Grande salle';
 dsc = [[Vous dans la grande salle.]];
}:with {
 obj {
 nam = 'boîte';
 dsc = [[Sur le plancher, je vois {boîte}.]];
 act = [[Lourd!]];
}
};
``
Il est facile à faire pour les objets du décor. Mais dans ce cas, vous n'
serez en mesure de créer des objets à la variable de référence. Heureusement, pour
les décors il n'est pas nécessaire.

Si dans la pièce sont placés plusieurs objets, séparez-les liens
par des virgules, par exemple:

 obj = { 'boîte', apple };

Vous pouvez insérer des sauts de ligne pour plus de clarté, quand les objets
beaucoup, par exemple, ainsi:

``
obj = {
'tabl',
'apple',
"knife',
};
``

Un autre mode de placement des objets est l'appel d'une fonction,
qui mettront les objets dans les pièces. Il sera examiné dans le
plus tard.

## Decoration

Les objets qui peuvent être déplacés d'une scène à une autre (ou
pénétrer dans l'inventaire), ont généralement le nom et/ou une variable de référence. Ainsi
comment donc vous pouvez toujours trouver un objet n'importe où et de travailler
avec lui.

Mais une grande partie du monde du jeu représentent des objets qui l'occupent
spécifique de l'emplacement et servent de décors.

De tels objets peuvent être très nombreux, et de plus, généralement, c'est
le même type d'objets comme les arbres ou d'autres objets.

Pour la création de décors, vous pouvez utiliser une variété d'approches.

### Un seul et même objet dans plusieurs pièces

Vous pouvez créer un objet, par exemple, 'arbre' et les placer dans
différentes pièces.

``
obj {
 nam = 'bois';
 dsc = [[il est {arbre}.]];
 act = [[Tous les arbres ont l'air semblables.]];
}

room {
 nam = 'Forêt';
 obj = { 'arbre' };
}

room {
 nam = 'Rue';
 obj = { 'arbre' };
}

``
### L'utilisation de la balise au lieu des noms de

Si vous n'aimez pas inventer des noms uniques pour les mêmes
des objets décoratifs, vous pouvez l'utiliser pour des objets
tags. Les balises sont définies par l'attribut tag et commencent toujours par le caractère '#':


``
obj {
 tag = '#fleurs';
 dsc = [[y {fleurs}.]]
}
``

Dans cet exemple, le nom de l'objet est généré automatiquement, mais
accéder à l'objet, vous serez en mesure de la balise. L'objet sera
recherchés dans le soutien à la pièce. Par exemple:

``
dprint(_'#fleurs') -- recherchons dans le courant de la chambre, le premier objet avec la balise '#fleurs'
``

Les tags, c'est dans un sens, synonyme de noms locaux, donc
il existe une autre entrée de créer l'objet avec le tag:


``
obj {
 nam = '#fleurs';
 dsc = [[y {fleurs}.]]
}
``

Si le nom de l'objet commence par le caractère '#', un obtient
la balise et générée automatiquement numérique nom.

### L'utilisation de l'attribut de la scène decor

Parce que les décors ne changent pas son emplacement, il est logique de faire
leur partie de la description de la scène, mais n'est pas dynamique. Cela se fait avec
l'aide de l'attribut de la scène 'decor'. decor affiche toujours et son
la fonction principale-la description du décor de la scène.

``
room {
 nam = 'Maison';
 dsc = [[Je suis à la maison.]];
 decor = [[Ici, je vois beaucoup de choses intéressantes. Par exemple, sur {#mur|mur}
 suspendus {#tableau|image}.]];
}: with {
 obj {
 nam = '#mur';
 act = [[le Mur comme un mur!]];
};
 obj {
 nam = '#image';
 act = [[Van Gogh?]];
}
}
``
Ici, nous voyons à la fois plusieurs étapes:

1. Dans un decor en forme le texte associé décrit le paysage;
2. Comme des liens permettent de conception avec explicite
 les objets auxquels ils se rapportent {nom de l'objet|texte};
3. Comme les noms des objets utilisés tags, pour ne pas penser au-dessus de leurs
l'originalité;
4. Les objets du décor en scène manquent les attributs de la dsc, car leur
 joue un rôle decor.

Bien sûr, vous pouvez combiner toutes les techniques entre les
toutes les proportions.

## Objets liés à d'autres objets

Les objets peuvent également contenir un attribut 'obj' (ou une structure
'with'). Dans ce cas, lors de la sortie des objets, au LIEU de déployer
les listes de manière cohérente. Cette technique peut être utilisée pour
création d'objets conteneurs ou tout simplement pour associer plusieurs
les descriptions de l'ensemble. Par exemple, mettons sur la table de la pomme.

``
obj {
 nam = 'pomme';
 dsc = [[Sur la table est {pomme}.]];
 act = 'Prendre quoi?';
};

obj {
 nam = 'table';
 dsc = [[la pièce est {table}.]];
 act = 'Euh... Juste une table...';
 obj = { 'pomme' };
};

room {
 nam = 'Maison';
 obj = { 'buffet' };
}
``

Dans ce cas, dans la description de la scène, nous verrons la description des objets 'table' et
'la pomme', ainsi que 'la pomme' -- associée à la table de l'objet et du moteur lors de la
l'impression de l'objet 'buffet' à la suite de son 'dsc' affichera successivement
"dsc" tous les sous-objets.

Aussi, il convient de noter qu'en termes l'objet de la 'table' (par exemple,
en le déplaçant de pièce en pièce), nous allons déplacer
et il est imbriqué dans un objet 'pomme'.

Bien sûr, cet exemple pourrait être écrit pour un autre, par exemple,
ainsi:


``
room {
 nam = 'Maison';
}:with {
 obj {
 nam = 'table';
 dsc = [[la pièce est {table}.]];
 act = 'Euh... Juste une table...';
 }: with {
 obj {
 nam = 'pomme';
 dsc = [[Sur la table est {pomme}.]];
 act = 'Prendre quoi?';
};
}
}
``
Choisissez celui qui pour vous est plus clair.

## Les attributs et les gestionnaires que les fonctions de la

La plupart des attributs et des gestionnaires peuvent être _функциями_. Ainsi,
par exemple:

``
disp = function()
 p 'pomme';
end
``

L'exemple n'est pas très réussie, de sorte que le plus simple serait d'écrire disp =
'la pomme', mais montre la syntaxe de la fonction d'enregistrement.

La tâche principale d'une telle fonction est -- il est de retour ligne ou булевого
des valeurs. Maintenant, nous considérons le retour de la ligne. Pour retourner une chaîne
vous pouvez utiliser explicitement un enregistrement sous la forme:

``
 return "pomme";
``

Ce faisant, la progression de l'exécution du code de la fonction s'arrête et il retourne
le moteur de jeu de la chaîne. Dans ce cas, la "pomme".

Une façon plus familière de sortie sont les fonctions de:

- p ("texte") -- affiche du texte et de l'espace;
- pn ("texte") -- affiche le texte à la ligne;
- pr ("texte") -- affiche le texte "en l'état".

> Si "p"/"pn"/"pr" est appelée avec un texte de paramètre, les supports peuvent être abaissé.

 pn "Pas de parenthèses!"

Toutes ces fonctions дописывают texte dans le tampon et au retour de la fonction
retournent de son moteur. Ainsi, vous pouvez progressivement
former une conclusion grâce à une exécution en série p/pn/pr. Gardez
à l'esprit que l'auteur a très rarement besoin de clairement mettre en forme le texte
surtout si c'est la description des objets, le moteur lui-même le met
les traductions nécessaires à des lignes et des espaces pour séparer les informations de différentes
genre et il le fait de manière uniforme.

Vous pouvez utiliser le '..' ou ',' collage de lignes. Alors '(' et ')'
obligatoires. Par exemple:

``
pn ("Ligne 1".." Ligne 2");
pn ("Ligne 1", "Ligne 2");
``

> La principale différence des attributs de gestionnaires d'événements est
> que les gestionnaires d'événements peuvent changer l'état du monde du jeu, et
> les attributs non. Par conséquent, si vous effectuez un attribut (par exemple, 'dsc')
> dans la forme de la fonction, n'oubliez pas que la tâche de l'attribut est le retour de la valeur et
> ne pas changer l'état du jeu! Le fait que le moteur fait appel à
> les attributs de ces moments dans le temps, qui d'habitude n'est pas clairement défini,
> et ne sont pas explicitement les processus de jeu!

__Il est important!__

> Une autre caractéristique des gestionnaires est le fait que vous ne
> besoin d'attendre les événements à l'intérieur d'un gestionnaire. Il ne devrait pas
> être de quelques cycles d'attente, ou de l'organisation des retards (pauses). L'affaire
> que la tâche du gestionnaire -- changer de terrain de jeu de l'état et de donner
> gestion de INSTEAD, qui visualise ces changements et encore
> passe en attente d'une action de l'utilisateur. Si vous avez besoin
> organiser des retards de sortie, vous devez utiliser le module
> "timer".

Les fonctions sont presque toujours des conditions et de travailler avec
les variables. Par exemple:

``
obj {
 nam = 'pomme';
 seen = false;
 dsc = function(s)
 if not s.seen then
 p 'Sur la table {quelque chose} est.';
else
 p 'Sur la table est {pomme}.';
end
end;
 act = function(s)
 if s.seen then
 p 'Est la pomme!';
else
 s.seen = true;
 p 'euh... Est-ce la même pomme!';
end
end;
};
``

Si l'attribut ou le gestionnaire d'une fonction, il est toujours _первый
аргумент_ fonction (s) -- l'objet lui-même. -Il y a, dans cet exemple, le 's'
c'est un synonyme de _'pomme'. Lorsque vous travaillez avec l'objet lui-même dans la fonction,
plus pratique d'utiliser une option et n'est pas explicite l'objet de
nom, comme lorsque vous renommez un objet, vous n'avez pas à réécrire
votre jeu. Oui et l'entrée sera plus courte.

Dans cet exemple, lors de l'affichage de la scène dans la dynamique de la scène sera
voir le texte: 'Sur la table quelque chose est'. Lors de l'interaction avec le 'quelque chose',
la variable 'seen' de l'objet 'pomme' sera installé dans true -- la vérité,
et nous verrons que c'était une pomme.

Comme on le voit, la syntaxe de l'instruction 'if' est assez évident. Pour
visibilité, quelques exemples.


 if <expression> then <action> end

 if have 'pomme' then
 p 'J'ai une pomme!'
end

 if <expression> then <actions> else <action sinon> end

 if have 'pomme' then
 p 'J'ai une pomme!'
else
 p 'je n'ai pas de pomme!'
end

 if <expression> then <actions> elseif <expression 2> then <action 2> else <else> end--, etc.

 if have 'pomme' then
 p 'J'ai une pomme!'
 elseif have 'prise' then
 p 'je n'ai pas de pomme, mais il y a prise!'
else
 p 'je n'ai ni pomme, ni les bouchons!'
end

L'expression de l'instruction if peut contenir logique "et" (and), ou"
(or), "la négation" (not) et les parenthèses ( ) pour définir les priorités. Enregistrement
type if <variable> then signifie que la variable n'est pas égale à
false. L'égalité est décrit comme le '==', l'inégalité '~='.

``
if not have 'pomme' and not have 'prise' then
 p 'je n'ai ni pomme, ni les bouchons!'
end

...
if w ~= apple then
 p 'n'Est pas une pomme.';
end
...

if time() == 10 then
 p '10 ème course est venu!'
end
``

__Il est important!__

Dans une situation où la variable n'a pas été déterminée, mais il est utilisé dans
la condition INSTEAD donnera l'erreur. Vous devez déterminer à l'avance
les variables que vous utilisez.


### Les variables de l'objet

L'entrée 's.seen' signifie que la variable 'seen' publiée dans l'objet
's' (c'est-à-dire 'la pomme'). Rappelez-vous, nous l'avons appelé le premier paramètre de la fonction
's' (self), et le premier paramètre -- c'est lui-même l'objet.

Les variables de l'objet doivent être définis à l'avance si vous allez
de les modifier. Comme nous l'avons fait avec la seen. Mais
les variables peuvent être nombreuses.


``
obj {
 nam = 'pomme';
 seen = false;
 eaten = false;
 color = 'red';
 weight = 10;
...
};
``

Toutes les variables de l'objet, lors de leur modification, tombent dans un fichier de sauvegarde
jeux.

Si vous ne voulez pas que la variable est tombé dans un fichier de sauvegarde, vous
pouvez déclarer des variables au sein du bloc spécial:


``
obj {
 nam = 'pomme';
{
 t = 1; -- cette variable ne pas tomber dans la conservation
 x = false; -- et celle-ci aussi
}
};
``
Normalement, vous ne devriez en faire autant. Cependant, il ya une situation où
cette astuce vous sera utile. Le fait que les tableaux et les tables de l'objet
sont toujours conservés. Si vous utilisez des tableaux pour stocker des
des valeurs, vous pouvez écrire:


``
obj {
 nam = 'pomme';
{
 text = { "un", "deux", "trois" }; -- jamais ne tombera pas dans le fichier de sauvegarde
}
...
};
``

Vous pouvez accéder à des variables de l'objet à travers le s -- si c'est lui-même
l'objet. ou de la variable de référence, par exemple:

``
apple = obj {
 color = 'red';
}
...
-- quelque part ailleurs
 apple.color = 'green'
``

Ou au nom de:


``
obj {
 nam = 'pomme';
 color = 'red';
}
...
-- quelque part ailleurs
 _'pomme'.color = 'green'
``
En fait, vous pouvez créer des variables de l'objet à la volée (sans
préalable de leur définition), même si elle n'est normalement pas de sens.
Par exemple:

``
apple 'xxx' (10) -- créé une variable xxx l'objet d'apple sur le lien
(_'pomme') 'xxx' (10) -- c'est la même chose, mais le nom de l'objet

``

### Les variables locales

En plus des variables d'un objet, vous pouvez utiliser les locaux et globaux
les variables.

Les variables locales sont créées à l'aide de l'appel du mot local:

``
act = function(s)
 local w = _'ampoule'
 w.light = true
 p [[J'ai appuyé sur le bouton et une ampoule allumée.]]
end
``

Dans cet exemple, la variable w n'existe que dans le corps
les fonctions de gestionnaire de l'act. Nous avons créé un lien de retour temporaire-la variable w,
ce qui fait référence à un objet 'ampoule' pour changer
la propriété d'une variable light de cet objet.

Bien sûr, nous pourrions écrire et:

 _'ampoule'.light = true

Mais imaginez, si nous avons besoin de faire quelques actions avec
l'objet, dans de tels cas, plus facile à utiliser une variable temporaire.

Les variables locales ne tombent dans le fichier-enregistrer et de jouer
le rôle des auxiliaires temporaires variables.

Les variables locales, vous pouvez créer et en dehors des fonctions, alors ce
la variable n'est visible que dans ce fichier lua et ne se trouve pas dans
un fichier de sauvegarde.

Un autre exemple d'utilisation des variables locales:

``
obj {
 nam = 'chaton';
 state = 1;
 act = function(s)
 s.state = s.state + 1
 if s.state > 3 then
 s.state = 1
end
 p [[Муррр!]]
end;
 dsc = function(s)
 local dsc = {
 "{Minou} ronronne.",
 "{Minou} joue.",
 "{Minou} облизывается.",
};
p(dsc[s.state])
end;
end
``

Comme on le voit, la fonction dsc nous avons déterminé un tableau de dsc. 'local' indique
qu'il agit dans les limites de la fonction dsc. Bien sûr, ce
d'exemple, on peut écrire ainsi:

``
dsc = function(s)
 if s.state == 1 then
 p "{Minou} ronronne."
 elseif s.state == 2 then
 p "{Minou} joue."
else
 p "{Minou} облизывается.",
end
end
``

### Variables globales

Vous pouvez également créer une variable globale:

``
global { -- définition des variables globales
 global_var = 1; -- le nombre de
 some_number = 1.2; -- le nombre de
 some_string = 'ligne';
 know_truth = false; -- la valeur booléenne
 array = {1, 2, 3, 4}; -- tableau
}
``

Une autre forme d'écriture, pratique pour les définitions:


``
global 'global_var' (1)
``

Les variables globales sont toujours à tomber dans un fichier-enregistrer.

À l'exception des variables globales, vous pouvez définir des constantes. La syntaxe
similaire à une variable globale:

``
const {
 A = 1;
 B = 2;
}
const 'Aflag' (false)
``

Le moteur sera de contrôler la préservation des constantes. Les constantes ne sont pas
entrent dans le fichier-enregistrer.

Parfois vous avez besoin d'utiliser une variable qui n'est pas définie comme
local (et visible dans tous vos lua les fichiers du jeu), mais ne doit pas tomber
dans un fichier de sauvegarde. Pour ces variables, vous pouvez utiliser
déclaration:


``
declare {
 A = 1;
 B = 2;
}
declare 'Z' (false)
``

La déclaration ne tombent pas dans le fichier de sauvegarde. L'une des propriétés importantes
des déclarations est que vous pouvez déclarer une fonction,
par exemple:

``
declare 'test' (function()
 p "Hello world!"
end)

global 'f' (test)

``

Dans ce cas, vous pouvez affecter la valeur de la fonction 'test' autres
les variables et l'état de ces variables peut être enregistrée dans un fichier
la conservation. Une декларированную fonction peut être utilisée
la valeur de la variable!

Vous pouvez déclarer précédemment certaines fonctions, par exemple:

``
declare 'dprint' (dprint)

``
Ce qui rend ces недекларированные de la fonction -- декларированными.

La déclaration de la fonction, en fait, c'est affecter la fonction du nom, merci
que pouvons-nous garder cette fonctionnalité comme une référence.


### La fonction auxiliaire

Vous pouvez écrire vos fonctions d'appui et de les utiliser à des
de votre jeu, par exemple:

``
function mprint(n, ...)
 local a = {...}; -- un tableau temporaire avec des arguments à la fonction
 p(a[n]) -- allons utiliser la n-ème élément du tableau
end
...
dsc = function(s)
 mprint(s).state, {
 "{Minou} ronronne.",
 "{Minou} joue.",
 "{Minou} облизывается." });
end;
``
Jusqu'à ce que ne pas prêter attention à cet exemple, si il vous semble difficile.

### Les valeurs de retour des gestionnaires

Si vous souhaitez montrer que l'action n'est pas remplie (gestionnaire n'est pas
fait rien d'utile), retourner la valeur false. Par exemple:

``
act = function(s)
 if then broken_leg
 return false
end
 p [[J'ai frappé du pied sur le ballon.]]
end
``
Cela permet d'afficher la description par défaut à l'aide de
gestionnaire de 'game.act'. Habituellement, la description par défaut contient
description synonyme d'action. Quelque chose comme:

 game.act = 'Euh... n'arrive Pas...';

Donc, si vous n'avez pas configuré le gestionnaire act ou ramené de lui false --
on croit que la réaction n'est pas et le moteur effectuera similaire à un gestionnaire de
l'objet 'game'.

Habituellement, il est inutile de renvoyer false de l'act, mais il existe
d'autres gestionnaires, qui sera décrit plus loin, pour lesquels
décrit le comportement est exactement le même.

En fait, en dehors de 'game.act' et 'act' -- attribut de l'objet existe
gestionnaire 'onact' de l'objet game, qui peut interrompre l'exécution
le gestionnaire 'act'.

Avant d'appeler un gestionnaire 'act' de l'objet, appelé onact de
game. Si le gestionnaire retourne false, l'exécution de 'act' est lâché.
'onact' pratique à utiliser, pour le contrôle
les événements dans la chambre ou dans un jeu, par exemple:

``
-- appelons onact de pièces, si elles sont
-- pour l'action sur n'importe quel objet

game.onact = function(s, ...)
 local r, v = std.call(ici), 'onact', ...)
 if v == false then -- si la valeur est false, la chaîne обрубаем
 return r, v
end
return
end

room {
 nam = 'shop';
 disp = 'Boutique';
 onact = function(s, w)
 p [[Dans le magasin ne peut pas voler!]]
 p ([[Même si c'est juste ]], w, '.')
 return false
end;
 obj = { 'des glaces', 'pain' };
}
``

Dans cet exemple, lorsque vous essayez de le "toucher" n'importe quel objet sera
un message sur l'interdiction de cette action.

Tout ce que décrit ci-dessus à l'exemple de 'act' est valable pour les autres
les gestionnaires: tak, inv, use, ainsi que lors de la navigation, qui sera
consultez de suite.

> Il est parfois nécessaire d'appeler la fonction de gestionnaire de la main.
> Pour ce faire, la syntaxe de l'appel de la méthode
> de l'objet. 'L'objet:méthode(paramètres)'. Par exemple:

 apple:act() -- appelez le gestionnaire 'act' de l'objet 'apple' (si il
 définie comme une fonction!). _'pomme':act() -- la même chose, mais
 nom, et non pas sur la variable de lien

Cette méthode fonctionne uniquement si la méthode de la
une fonction. Vous pouvez profiter d'un " std.call()' pour
l'appel d'un gestionnaire de la manière dont il fait lui-même INSTEAD. (Sera
décrit dans le futur).

## Inventaire

La version la plus simple de faire l'objet que vous pouvez emprunter -- déterminer
gestionnaire 'tak'.

Par exemple:

``
obj {
 nam = 'pomme';
 dsc = 'Sur la table est {pomme}.';
 inv = function(s)
 p 'J'ai mangé une pomme.'
 remove(s); -- supprimer la pomme de matériel
end;
 tak = 'Vous avez pris une pomme.';
};
``

Dans ce cas, l'action du joueur sur l'objet de la "pomme" (clic de souris sur
le lien dans la scène) -- la pomme sera retiré de la scène et posté dans
de l'inventaire. Lors de l'action du joueur sur l'inventaire (double clic sur
le nom de l'objet) -- un gestionnaire est appelé 'inv'.

Dans notre exemple, l'action du joueur sur la pomme dans l'inventaire -- la pomme
sera mangé.

Bien sûr, nous pourrions mettre en œuvre le code de prise de l'objet dans "act",
par exemple:

``
obj {
 nam = 'pomme';
 dsc = 'Sur la table est {pomme}.';
 inv = function(s)
 p 'J'ai mangé une pomme.'
 remove(s); -- supprimer la pomme de matériel
end;
 act = function(s)
take(s)
 p 'Vous avez pris une pomme.';
end
};
``

Si l'objet dans l'inventaire n'est pas déclaré, le gestionnaire d' 'inv', sera appelée 'game.inv'.

Si le gestionnaire 'tak' false, alors l'objet ne sera prise, par exemple:


``
obj {
 nam = 'pomme';
 dsc = 'Sur la table est {pomme}.';
 tak = function(s)
 p "Il червивое!"
 return false
end;
};
``

## Les transitions entre les scènes

Traditionnelles transitions dans INSTEAD ressemblent à des liens au-dessus de la description
de la scène. Pour la définition des transitions entre les scènes utilisé
l'attribut de la scène -- la liste des 'way'. Dans la liste sont définis de la pièce, sous la forme
les noms de pièces ou de variables de référence, comme la liste 'obj'. Par exemple:

``
room {
 nam = 'room2';
 disp = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 way = { 'main' };
};

room {
 nam = 'main';
 disp = 'pièce Principale';
 dsc = 'êtes-Vous dans la grande salle.';
 way = { 'room2' };
};
``

Dans ce cas, vous serez en mesure de passer entre les scènes, 'main' et 'room2'. Comme vous
rappelez-vous, 'disp' est peut-être la fonction, et vous pouvez générer des noms
les passages à la volée. Ou utiliser le titre, pour séparer le nom de scène
comme le titre et le nom de la transition:

``
room {
 nam = 'room2';
 disp = 'Dans la salle';
 title = 'Dans la salle';
 dsc = 'êtes-Vous dans une immense salle.';
 way = { 'main' };
};

room {
 nam = 'main';
 title = 'Dans la pièce principale';
 disp = 'Dans la pièce principale';
 dsc = 'êtes-Vous dans la grande salle.';
 way = { 'room2' };
};
``

Lors de la transition entre les scènes, le moteur entraîne le gestionnaire 'onexit' de
soutien de la scène et 'onenter' dans la scène où il va joueur. Par exemple:

``
room {
 onenter = 'Vous entrez dans la salle.';
 nam = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 way = { 'main' };
 onexit = 'Vous sortez de la salle.';
};
``

Bien sûr, comme tous les gestionnaires, 'onexit' et 'onenter' peuvent être
fonctions. Alors que le premier paramètre est (comme toujours) l'objet lui-même -
bains, et la deuxième c'est la salle où le joueur va aller (pour
'onexit') ou à partir de laquelle va sortir (pour 'onenter'). Par exemple:

``
room {
 onenter = function(s, f)
 if f^'main' then
 p 'Vous allez partir de la salle de main.';
end
end;
 nam = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 way = { 'main' };
 onexit = function(s, t)
 if t^'main' then
 p 'Je ne veux pas de retour!'
 return false
end
end;
};
``
Enregistrement de type:

 if f^'main' then

C'est la juxtaposition de l'objet nommé. C'est une alternative à des enregistrements:

 if f = = = = = _'main' then

Ou:

 if f.nam == 'main' then

Ou:

 if std.nomde(f) == 'main' then


Comme nous le voyons sur l'exemple de la onexit, ces gestionnaires, en ligne peuvent
renvoyer une valeur booléenne de statut. De même, le processeur de onact, nous
peut annuler la transition, en retournant false de onexit/onenter.


Vous pouvez demander le retour d'un statut et d'une autre manière, si cela semble
vous pratique:

``
 return "Je ne veux pas en arrière", false
``

Si vous utilisez la fonction 'p'/'pn'/'pr', puis il suffit de retourner la
le statut de l'opération à l'aide de la finale de 'return', comme indiqué dans
l'exemple ci-dessus.

__Il est important!__

> Il est à noter que lorsque le gestionnaire d' 'onenter' un pointeur sur
> la scène courante (here()) **n'est pas encore modifié**!!! Dans INSTEAD une
> les gestionnaires 'exit' (soins de la pièce) et 'enter' (coucher dans la salle),
> qui sont appelées déjà _после_ que la transition s'est produite. Ces
> les gestionnaires recommandés pour une utilisation toujours, quand il n'y a
> la nécessité d'interdire le passage.

Parfois, il ya un besoin pour le nom de la transition différente de
nom de la pièce, qui mène cette transition. Il existe plusieurs
façons de le faire. Par exemple, à l'aide de 'path'.

``
room {
 nam = 'room2';
 title = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 way = { path { 'Dans la pièce principale', 'main'} };
};

room {
 nam = 'main';
 title = 'une pièce Principale';
 dsc = 'êtes-Vous dans la grande salle.';
 way = { path {'Dans la salle', 'room2'} };
};
``

En fait, le 'path' crée une salle avec l'attribut 'disp', qui
est égal au premier paramètre, et une fonction spéciale 'onenter', ce qui
redirige le joueur dans une pièce donnée dans le deuxième paramètre 'path'.

Si vous spécifiez trois paramètres:

 way = { path {'#взал', 'Dans la salle', 'room2'} };

Le premier paramètre sera le nom (ou le tag, comme dans l'exemple
exemple) d'une telle pièce.

Une autre forme d'écriture avec explicite de l'attribut nam:

 way = { path { nam = '#взал', 'Dans la salle', 'room2'} };

Vous pouvez changer le nom de la transition, après évolution
au moins une fois, et vous avez appris que ces bains:

 way = { path {'#вдверь', 'porte', after = 'Le salon', 'room2'} };

Tous les paramètres, sauf le nom de la transition, peuvent être des fonctions.

Par conséquent, le 'path' vous permet de nommer les transitions de manière pratique.

Parfois, vous devrez peut-être activer et de désactiver les transitions. En
fait, il est souvent. L'idée de la navigation consiste en ce que le passage
visible même quand il n'est pas possible. Par exemple, imaginez la scène
en face de la maison de la porte d'entrée. Entrer dans la maison car la porte
fermé.

Pas beaucoup de sens pour cacher la transition de la "porte". Tout simplement dans la fonction 'onenter'
la scène de l'intérieur de la maison, nous vérifions, et si le héros a la clé? Et si la clé
non, disons que la porte est fermée, et nous interdisons la transition. C'est
améliore l'interactivité et simplifie le code. Si vous voulez faire
la porte de l'objet de la scène, placez-le dans la pièce, mais dans les 'act' gestionnaire
faites une inspection de la porte, ou donner la possibilité au joueur d'ouvrir la clé
(comment le faire - nous parlerons plus tard), mais le passage de laissez-faire
le joueur de façon habituelle, à travers la ligne de navigation.

Toutefois, il existe des situations où la transition n'est pas évidente et il
apparaît à la suite de certains événements. Par exemple, nous avons examiné la montre
et nous y avons vu le secret laz.

``
obj {
 nam = 'heures';
 dsc = [[Ici d'anciennes {montres}.]];
 act = function(s)
 enable '#montres'
 p [[Vous voyez que dans les heures il ya un passage secret!]];
end;
}

room {
 nam = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 obj = { 'heures' };
 way = { path { '#horloge', 'L'horloge', 'inclock' }:disable() };
};
``

Dans cet exemple, nous avons créé _отключенный_ la transition de l'appel
la méthode 'disable' de la pièce créée par le 'path'. La méthode 'disable'
tous les objets (et pas seulement de pièces), il met l'objet dans
l'état désactivé, ce qui signifie que l'objet cesse d'être
disponible à un joueur. Cette propriété déconnectée de l'objet
est-ce qu'elle peut _включить_ à l'aide de la 'enable()';

Ensuite, lorsque le joueur clique sur le lien qui décrit la montre, est appelée
gestionnaire 'act', qui, avec l'aide de la fonction 'enable()' qui fait la transition
visible.

Une option alternative n'est pas à l'arrêt, et la 'fermeture'
objet:

``
obj {
 nam = 'heures';
 dsc = [[Ici d'anciennes {montres}.]];
 act = function(s)
 open '#montre'
 p [[Vous voyez que dans les heures il ya un passage secret!]];
end;
}

room {
 nam = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 obj = { 'heures' };
 way = { path { '#horloge', 'L'horloge', 'inclock' }:close() };
};
``
Quelle est la différence? Désactivation d'un objet signifie que l'objet cesse
être disponible pour les joueurs. Si l'objet est investi d'autres objets
et ces objets ne sont pas disponibles.

La fermeture de l'objet rend inaccessible le contenu de cet objet, mais ne
l'objet lui-même.

Cependant, dans le cas de pièces, et la fermeture de bains les bains
conduisent à un résultat -- la transition sur eux n'est pas disponible.

Une autre option:


``
room {
 nam = 'inclock';
 dsc = [[J'en heures.]];
} close()

obj {
 nam = 'heures';
 dsc = [[Ici d'anciennes {montres}.]];
 act = function(s)
 open 'inclock'
 p [[Vous voyez que dans les heures il ya un passage secret!]];
end;
}

room {
 nam = 'Salle';
 dsc = 'êtes-Vous dans une immense salle.';
 obj = { 'heures' };
 way = { path { 'L'horloge', 'inclock' } };
};
``

Ici, nous fermons et ouvrons pas le passage, et dans une pièce où mène
de la transition. path ne montre pas de lui-même, si la pièce dans laquelle il mène
désactivé ou fermée.

## L'action des objets les uns sur les autres

Le joueur peut agir sur l'objet de l'inventaire sur d'autres objets. Pour
de cela, il clique sur un objet matériel, puis sur le sujet
de la scène. Ce faisant, un gestionnaire est appelé 'used' de l'objet, qui
agissent, et le gestionnaire d' 'use' de l'objet, qui agissent.

Par exemple:
``
obj {
 nam = 'couteau';
 dsc = 'Sur la table est {couteau}';
 inv = 'Vif!';
 tak = 'J'ai pris un couteau!';
 use = 'Vous essayez d'utiliser un couteau.';
};

obj {
 nam = 'table';
 dsc = 'Dans la chambre coûte {table}.';
 act = 'Euh... Juste une table...';
 obj = { 'couteau' };
 used = function(s)
 p 'Vous essayez de faire quelque chose avec une table...';
 return false
end;
};
``
Dans cet exemple, le gestionnaire used retourne false. Pourquoi? Si vous
rappelez-vous, le retour false signifie que le gestionnaire signale moteur sur la
que l'événement il n'est pas traité. Si nous ne renverrait false, la file d'attente
avant le gestionnaire 'use' de l'objet 'couteau' il ne serait pas atteint. En fait,
dans la réalité, habituellement, vous pourrez profiter ou use ou used, à peine
a de sens réaliser les deux types de gestionnaire pendant la durée de l'objet
sur le sujet.

Un autre exemple, quand il est commode de renvoyer false:

``
use = function(s, w)
 if w^'pomme' then
 p [[J'ai nettoyé la pomme.]]
 w.cut = true
return
end
 return false;
end
``
Dans ce cas, use de la pomme traite seulement d'une situation --
l'action sur la pomme. Dans d'autres cas, le gestionnaire renvoie false et
le moteur appelle la méthode par défaut: game.use.

Mais c'est mieux si vous пропишете l'action par défaut pour le couteau:

``
use = function(s, w)
 if w^'pomme' then
 p [[J'ai nettoyé la pomme.]]
 w.cut = true
return
end
 p [[Pas la peine de brandir un couteau!]]
end
``
Cet exemple illustre également le fait que le deuxième paramètre de use
est le sujet sur lequel nous agissons. La méthode 'used',
en conséquence, le deuxième paramètre -- c'est l'objet qui agit sur le
nous:

``
obj {
 nam = 'musorka';
 dsc = [[Dans le coin vaut {musorka}.]];
 used = function(s, w)
 if w^'pomme' then
 p [[J'ai jeté une pomme dans la poubelle.]]
remove(w)
return
end
 return false;
end
}
``

Comme vous vous en souvenez, avant d'appeler use appelé un gestionnaire de onuse
l'objet de la game, puis de l'objet 'joueur', et ensuite de soutien de la pièce. Vous
pouvez bloquer 'use', retour de l'une des méthodes suivantes
'onuse' -- false.

Utiliser l'option 'use' ou 'used' (ou les deux), c'est une question personnelle
de préférence, cependant, la méthode utilisée est appelée plus tôt et, par conséquent,
a la plus grande priorité.

## L'Objet "Joueur"

Le joueur dans le monde INSTEAD est représenté par un objet de type 'player'. Vous pouvez
la création de plusieurs joueurs, mais un joueur est présent par défaut.

Le nom de cet objet -- 'player'. La variable existe-lien pl,
ce qui indique à cet objet.

D'habitude, vous n'avez pas besoin de travailler avec cet objet directement. Mais parfois, c'est
peut-être pas nécessaire.

Par défaut, l'attribut 'obj', le joueur est un inventaire.
D'habitude, il est inutile de remplacer un objet de type player, cependant, vous
pouvez le faire:

``
game.player = player {
 nam = "Basile";
 room = 'cuisine'; -- de départ bains joueur
 power = 100;
 obj = { 'pomme' }; -- dans le même temps ajoutons une pomme dans l'inventaire
};
``

Dans INSTEAD il est possible de créer plusieurs joueurs et
basculer entre eux. Pour cela, utilisez la fonction 'change_pl()'. Dans
comme un paramètre transmettez la fonction de l'élément de type 'player'
(ou son nom). La fonction de bascule du joueur actuel, et lors de la
nécessaire, aura lieu le passage dans la pièce où se trouve le nouveau
un joueur.

La fonction de 'me()' renvoie toujours la valeur du joueur actuel. Par conséquent, dans
la plupart des jeux, me() == pl.

## L'Objet "Le Monde"

Le monde du jeu est représenté par un objet de type world. Le nom de l'objet
 'game'. Il existe une référence de la variable, qui est aussi appelé game.

Normalement, vous ne travaillez pas avec cet objet directement, mais parfois vous
pouvez appeler ses méthodes, ou de changer les valeurs des variables de ce
d'un objet.

Par exemple, la variable game.codepage contient l'encodage du code source
jeux, et par défaut est "UTF-8". Je ne recommande pas d'utiliser
le codage d'autres, mais parfois, le choix de l'encodage peut devenir
de la nécessité.

Variable game.player -- contient du joueur actuel.

En outre, comme vous le savez déjà, l'objet 'game' peut contenir
les gestionnaires par défaut: 'act', 'inv', 'use', 'tak', qui seront
appelés, si, à la suite de l'action de l'utilisateur ne seront pas trouvés
pas d'autres gestionnaires (ou tous retourné false). Par exemple, vous
pouvez écrire au début du jeu:

``
game.act = 'Ne fonctionne pas.';
game.inv = 'Hum.. une chose Étrange..';
game.use = 'Ne fonctionne pas...';
game.tak = 'Pas besoin de moi pour cela...';
``

Bien sûr, ils peuvent tous être des fonctions.

Aussi, l'objet de game peut contenir des gestionnaires: onact, ontak, onuse,
oninv, onwalk -- qui peuvent interrompre l'action, dans le cas d'un remboursement
false.

D'autre objet game, vous pouvez définir des gestionnaires: afteract, afterinv,
afteruse, afterwalk -- qui sont appelées en cas de succès
l'exécution de l'action.

## Attributs de listes

Les attributs des listes (tels que 'way' ou 'obj') permettent de travailler avec
son contenu à l'aide d'un ensemble de méthodes. Les attributs des listes appelés
stocker des listes d'objets. En fait, vous pouvez créer
les listes pour leurs propres besoins, et de les placer dans des objets, par exemple:

``
room {
 nam = 'réfrigérateur';
 frost = std.list { 'crème glacée' };
}
``

Bien que, généralement, il n'est pas nécessaire.
Voici la liste des méthodes des objets de type 'liste'. Vous pouvez les appeler
pour chacune des listes, bien que ce seront way et obj, par exemple:

 ways():disable() -- désactiver toutes les transitions

- disable() - désactive tous les objets de la liste;
- enable() - inclut tous les objets de la liste;
- close() - fermer tous les objets de la liste;
- open() - ouvrir tous les objets de la liste;
- add(objet|nom, [position]) - ajouter un objet;
- for_each(fonction, les arguments) - appeler pour chaque objet une fonction
des arguments;
- lookup(nom d'une balise ou d'un objet) - la recherche de l'objet dans la liste. Retourne
l'objet et l'index;
- srch(nom d'une balise ou d'un objet) - la recherche de l'objet visible dans la liste;
- empty() - retourne true si la liste est vide;
- zap() - effacer la liste;
- replace(qui, que) - remplacer l'objet dans la liste;
- cat(liste, [position]) - ajouter le contenu de la liste dans la liste actuelle
 de position;
- del(nom/objet) - supprimer un objet de la liste.

Il existe des fonctions qui retournent des objets-listes:

- inv([joueur]) - pour revenir à l'inventaire du joueur;
- objs([salle]) - pour revenir à la salle des objets;
- ways([salle]) - ramener les transitions de la pièce.

Bien sûr, vous pouvez accéder aux listes et directement:

``
 pl.obj:add 'couteau'

``

Les objets dans les listes sont stockées dans l'ordre dans lequel vous les avez
ajoutez. Cependant, si l'objet est présent attribut numérique pri, ce
il joue le rôle de _приоритета_ dans la liste. Si le pri n'est pas spécifié, la valeur de la
la priorité est considéré comme 0. Donc, si vous voulez un
l'objet a été le premier dans la liste, donnez-lui la priorité pri < 0. Si
la fin de la liste -- > 0.

``
obj {
 pri = -100;
 nam = 'truc';
 disp = 'le sujet Très important de l'inventaire';
 inv = [[Attention à ce sujet.]];
}

``

## Fonctions qui retournent des objets

Dans INSTEAD identifié certaines fonctions qui retournent des objets différents.
Lors de la description des fonctions utilisent les conventions suivantes sur les paramètres.

- dans les symboles [ ] décrit les paramètres facultatifs;
- 'que' ou 'où' signifie un objet (y compris la salle) spécifié dans la balise,
 le nom ou la variable de référence;

Ainsi, les fonctions de base:

- '_(que)' - obtenir l'objet;
- 'me()' retourne l'objet-joueur;
- 'here()' renvoie la scène;
- 'where(que)' renvoie la pièce ou de l'objet dans lequel se trouve
 un objet, si l'objet se trouve à plusieurs endroits, vous pouvez
 envoyer un deuxième paramètre -- une table Lua, qui seront ajoutés
 ces objets;
- 'inroom(que)' de même, where(), mais retourne la pièce dans laquelle
 se trouve l'objet (c'est important pour les objets dans les objets);
- 'from([où])' retourne le dernier de la salle, à partir de laquelle le joueur est passé à
donnée de la pièce. Le paramètre facultatif -- obtenir la dernière pièce
pas de soutien de la pièce, et pour prédéterminée;
- 'seen(que, [où])' retourne un objet ou un passage, si il
présent et de voir, il y a un second paramètre optionnel -- choisir
une scène ou un objet/une liste dans laquelle chercher;
- 'lookup(que, [où])' retourne un objet ou un passage, si il
il existe dans la scène ou l'objet de la liste;
- 'inspect(que)' renvoie l'objet, si elle est visible/disponible sur
 scène. La recherche est effectuée par ajout de transitions et d'objets, y compris, dans
 les objets d'un joueur;
- 'have(que)' retourne un objet dans l'inventaire et ne
désactivé;
- 'live(que)' renvoie l'objet, si il est présent parmi les vivants
 objets (décrite ci-dessous);

Ces fonctions sont principalement utilisés dans les conditions de la ou pour la recherche
l'objet de suivi de la modification. Par exemple, vous pouvez utiliser
'seen' pour la rédaction de conditions:

``
onexit = function(s)
 if seen 'monstre' then -- si la fonction 1 option
 --- les parenthèses ne faut pas écrire
 p 'Monstre bloque le passage!'
 return false
end
end
``

Ainsi, pour trouver l'objet dans la scène:
``
use = function(s, w)
 if w^'fenêtre' then
 local ww = lookup 'chien'
 if not then ww
 p [[il est où mon chien?]]
return
end
 place(ww, 'rue')
 p 'J'ai cassé la fenêtre! Mon chien a sauté sur la rue.'
return
end
 return false
end
``

Exemple avec la fonction 'have':

``
...
act = function(s)
 if have 'couteau' then
 p 'Mais j'ai même un couteau de!';
return
end
 take 'couteau'
end
...
``
> La question peut se poser, quelle est la différence entre les fonctions lookup et _ ()?
> Le fait que lookup() à la recherche de l'objet, et dans le cas où l'objet n'a pas été trouvé
> -- tout simplement rien ne sera de retour. Et une entrée d' _ () suppose que vous êtes exactement
> vous savez ce que vous obtenez. En d'autres termes, _ () est
> une inconditionnelle de l'obtention d'un objet par son nom. Cette fonction ne peut pas être
> s'occupe de _поиском_. Uniquement si le paramètre est défini de la balise,
> sera effectué une recherche parmi les objets disponibles. Si vous utilisez
> _ () sur un objet inexistant ou inaccessible balise -- vous obtenez une erreur!

## Les autres fonctions de la bibliothèque standard

Dans INSTEAD dans le module stdlib, qui toujours se connecte automatiquement
la fonction est définie, qui propose à l'auteur comme base de travail
l'outil le monde du jeu. Nous les examinerons dans ce chapitre.

Lors de la description des fonctions dans la plupart des fonctions sous l'option 'w'
on entend un objet ou une pièce définie nom de balise ou de
la variable de référence. [ wh ] - signifie un paramètre facultatif.

- include(fichier) - mettre le fichier dans le jeu;

 include "lib" -- comprendra le fichier lib.lua à partir du répertoire courant avec le jeu;

- loadmod(module) - connecter le module de jeu;

 loadmod "module" -- comprendra un module de module.lua à partir du répertoire courant;

- rnd(m) est un entier, la valeur de '1' à 'm';
- rnd(a, b) est un entier, la valeur de 'a' à 'b', où 'a'
 et 'b' entiers >= 0;
- rnd\_seed(que) - définir le grain du générateur de nombres aléatoires;
- p(...) - sortie de ligne dans le tampon de gestionnaire/attribut (avec un espace à la fin);
- pr(...) - la production d'une chaîne dans la mémoire tampon de gestionnaire/l'attribut "tel quel";
- pn(...) - sortie de ligne dans le tampon de gestionnaire/attribut (avec un saut de ligne à la fin);
- pf(fmt, ...) - la conclusion de formatage de chaîne de caractères dans la mémoire tampon du gestionnaire/attribut;

 local text = 'bonjour';
 pf("Chaîne de caractères: %q le Nombre: %d\n", text, 10);

- pfn(...)(...)... "chaîne" - la formation d'un simple gestionnaire;
 Cette fonction facilite la création de simples gestionnaires:

 act = pfn(walk, 'salle de bain'), J'ai décidé d'aller à la salle de bain.";
 act = pfn(enable, '#passage') "J'ai remarqué un trou dans le mur!";

- obj {} - la création d'un objet;
- stat {} - la création de statut;
- room {} - création d'une pièce;
- menu {} - création d'un menu;
- dlg {} - création d'un dialogue;
- me() - retourne l'actuel joueur;
- here() - renvoie la scène;
- from([w]) - renvoie la salle à partir de laquelle le passage dans
la scène courante;
- new(concepteur, args) - création d'un nouveau _динамического_
 de l'objet (qui sera décrit plus loin);
- delete(w) - suppression d'un objet dynamique;
- gamefile(fichier, [réinitialiser l'état?]) - de charger dynamiquement un fichier
 avec le jeu;

 gamefile("part2.lua", true) -- réinitialiser l'état du jeu (supprimer
 objets et variables), de charger part2.lua et de commencer à main de la pièce.

- player {} - créer un joueur;
- dprint(...) - sortie de débogage;
- visits([w]) - le nombre de visites de cette pièce (ou 0 si les visites n'ont pas été);
- visited([w]) - nombre de visites dans la pièce ou false, si les visites ne
a été;

 if not visited() then
 p [[Je suis là pour la première fois.]]
end

- walk(w, [булевое exit], [булевое enter], [булевое changer from]) - le passage
 dans la scène;

 walk('fin', false, false) -- une inconditionnelle de la transition (ignorer
onexit/onenter/exit/enter);

- walkin(w) - passage en sous-scène (sans appel à exit/onexit soutien de la chambre);
- grève surprise([w], [dofrom]) - le retour de la sous-scène (sans appel
entrée/onenter);
- walkback([w]) - synonyme de grève surprise([w], false);
- \_(w) - l'obtention de l'objet;
- for_all(fn, ....) - exécuter la fonction pour tous les arguments.

 for_all(enable, 'fenêtre', 'porte');

- seen(w, [où]) - la recherche de l'objet visible;
- lookup(w, [où]) - la recherche de l'objet;
- ways([où]) - obtenir la liste des conversions;
- objs([où]) - obtenir la liste des objets;
- search(w) - recherche disponible à un joueur de l'objet;
- have(w) - recherche d'objet dans l'inventaire;
- inroom(w) - retour de la pièce de séjour où se trouve l'objet;
- where(w, [le tableau]) - le retour de l'objet/les objets dans laquelle se trouve l'objet;

 local list = {}
 local w = where('pomme', list)
 -- si la pomme est en plus de place, 
 -- list contiendra un tableau de ces lieux.
 -- Si vous avez assez d'un seul emplacement, alors:
 where 'pomme' -- sera assez

- closed(w) - true si l'objet est fermé;
- disabled(w) - true si l'objet est éteint;
- enable(w) - activer l'objet;
- disable(w) - eteindre l'objet;
- open(w) - ouvrir l'objet;
- close(w) - fermer l'objet;
- actions(w, ligne, [valeur]) - retourne (ou place)
 le nombre d'actions de type t pour objet de w.

 if actions(w, 'tak') > 0 then -- l'objet de w a été pris au moins 1 fois;
 if actions(w) == 1 then -- act de l'objet w a été appelé 1 fois;

- pop(la balise) - retour à la dernière branche du dialogue;
- push(balise) - passage en suivant la branche d'un dialogue
- empty([w]) est vide si la branche du dialogue? (ou l'objet)
- lifeon(w) - ajouter un objet dans la liste des vivants;
- lifeoff(w) - enlever l'objet à partir de la liste des vivants;
- live(w) - l'objet de la vie?;
- change\_pl(w) - changer de joueur;
- player\_moved([pl]) - le joueur actif se déplace dans cette course?;
- inv([pl]) - obtenir la liste de l'inventaire;
- remove(w, [wh]) - supprimer un objet à partir de l'objet ou de la pièce; Supprime
 l'objet de listes obj et way (en laissant tous les autres, par exemple,
game.lifes);
- purge(w) - détruire l'objet (de toutes les listes); Supprime l'objet de
 _всех_ les listes dans lesquelles il est présent;
- replace(w, ww, [wh]) - remplacer un objet par un autre;
- place(w, [wh]) - placer un objet dans un objet/une pièce (en supprimant des
 ancien objet/pièce);
- put(w, [wh]) - placer l'objet sans la suppression de l'ancien emplacement;
- take(w) - l'enlèvement de l'objet;
- drop(w, [wh]) - jeter l'objet;
- path {} - créer une transition;
- time() - nombre de coups depuis le début du jeu.


__Il est important!__

En fait, beaucoup de ces fonctions peuvent également travailler non seulement avec
de bains et des objets, mais des listes. C'est - 'remove(apple,
inv())' fonctionne également comme 'remove(apple, me())"; d'Ailleurs,
remove(apple) marchera et supprime l'objet de ces endroits où il
est présent.

Voyons quelques exemples.

``
act = function()
 pn "Je vais dans la salle suivante..."
 walk (nextroom);
end

obj {
 nam = 'ma machine';
 dsc = 'Avant de la cabane est mon vieux {pick-up} Toyota.';
 act = function(s)
 walk 'inmycar';
end
};
``
__Il est important!__

> Après l'appel de 'walk' exécution du gestionnaire se poursuit jusqu'à son
> terminer. C'est pourquoi d'habitude, après le 'walk' doit toujours être
> 'return', si seulement ce n'est pas la dernière ligne de la fonction, bien que dans
> ce cas, il est sûr de livrer 'return'.

``
act = function()
 pn "Je vais dans la salle suivante..."
 walk (nextroom);
return
end
``

N'oubliez pas également que lors de l'appel 'walk' вызовутся des gestionnaires
'onexit/onenter/exit/enter" et si ils interdisent le passage, il n'est pas
se produire.


## Dialogues

Les dialogues -- c'est la scène d'un type spécial de 'dlg', contenant des objets --
de la phrase. Lors de l'entrée dans le dialogue, le joueur voit une liste des expressions qui peut
choisir, en obtenant une sorte de réaction de jeu. Par défaut, déjà sélectionnées
les phrases se cachent. L'épuisement de toutes les options, le dialogue se termine
la sortie de la précédente pièce (bien sûr, si le dialogue n'est pas en permanence
visibles des phrases, parmi lesquels on trouve généralement quelque chose comme 'Terminer
la conversation' ou 'Demander une fois de plus'). Lors de ce dialogue, tous les
les expressions cachées sont à nouveau visibles et le dialogue est remis à
l'état initial (si, bien sûr, l'auteur du jeu n'est pas spécialement
faisait des efforts pour le changement de boîte de dialogue).

Le passage dans la boîte de dialogue dans le jeu est comme le passage à la scène:

``
obj {
 nam = 'chef';
 dsc = 'Je vois {chef}.';
 act = function()
 walk 'povardlg'
end,
};
``

Bien que je recommande d'utiliser un 'walkin', comme dans le cas de 'walkin' n'est pas
sont appelées 'onexit/exit' soutien de la pièce, et le personnage avec lequel nous
nous pouvons parler normalement se trouver dans la même pièce, où et chef de la
le héros. C'est à dire:

``
obj {
 nam = 'chef';
 dsc = 'Je vois {chef}.';
 act = function()
 walkin 'povardlg'
end,
};
``

Si vous n'aimez pas le préfixe de phrases en forme de trait d'union, vous pouvez définir une variable de chaîne:

``
std.phrase_prefix = '+';
``

Et obtenir le préfixe " + " devant chaque phrase. Vous pouvez également
faire un préfixe de la fonction. L'entrée de la fonction dans ce cas sera
agir comme paramètre le numéro de la phrase. La mission de la fonction -- retourner
chaîne de préfixe.

Veuillez noter que le 'std.phrase_prefix' n'est pas enregistré si vous
besoin de remplacer son à la volée, vous devez le restaurer à son
l'état dans le 'start()' de la fonction main!

__Il est important!__

> Je vous recommande d'utiliser le module 'noinv' et définir la propriété 'noinv'
> dans les dialogues. Les dialogues auront l'air plus beau et vous обезопасите sa
> le jeu des erreurs imprévisibles et indésirables lors de l'utilisation de l'inventaire
> à l'intérieur d'un dialogue (comme d'habitude l'auteur n'implique pas de tels
> des choses). Par exemple:

``
require "noinv"
...
dlg {
 nam = 'Garde';
 -- dans les boîtes de dialogue n'est généralement pas besoin d'un inventaire
 noinv = true;
...
}
``

### Les phrases

Un concept central dans les dialogues est _фраза_. La phrase n'est pas juste
question-réponse que vous pouvez penser. La phrase est un arbre, et dans ce
sens, le dialogue en entier peut être réalisé seul
de la phrase. Par exemple:

``
dlg {
 nam = 'conversation';
 title = [[la Conversation avec le vendeur]];
 enter = [[J'ai demandé au vendeur.]];
 phr = {
 { 'Avez-vous les haricots?', '-- Non.'},
 { 'Avez-vous le chocolat?', '-- Non.'},
 { 'Avez-vous kvas?', '-- Oui',
 { 'Et combien il coûte?', '-- 50 roubles.' },
 { 'Et il fait froid?', '-- Réfrigérateur est tombé en panne.',
 { 'Prends les deux!', 'Seul.',
 { 'Donnez un!', function() p [[Ok!]]; take 'brassage'; end };
}
}
}
}
}
``
Comme on le voit dans l'exemple, la phrase est définie par l'attribut de phr et peut contenir
ramifiée dialogue. La phrase contient des élections, chacun
peut aussi contenir des élections et ainsi de suite...

La phrase a un format de paires: spécificateur -- une réaction. Dans le cas le plus simple,
c'est la ligne. Mais ils peuvent être et les fonctions. Généralement, la fonction arrive
la réaction, qui peut contenir du code de l'évolution du monde du jeu.

Le couple peut être simple:

 {'Question', 'Réponse }

Et peut contenir un tableau de paires:

 {'Question', 'Réponse',
 {'Sous-question 1', 'Sous-réponse1' },
 {'Sous-question 2', 'Sous-answer2' },
}

En fait, si vous regardez attentivement l'attribut de phr, vous
remarquez que le tableau de l'élection est imbriqué dans la phrase
phr, mais seulement initiale de la vapeur manquant:


``
dlg {
 nam = 'conversation';
 title = [[la Conversation avec le vendeur]];
 enter = [[J'ai demandé au vendeur.]];
 phr = {
 -- ici pourrait être la réponse de la question 1 niveau!
 -- 'La question principale', 'la réponse',
 { 'Avez-vous les haricots?', '-- Non.'},
 { 'Avez-vous le chocolat?', '-- Non.'},
 { 'Avez-vous kvas?', '-- Oui',
 { 'Et combien il coûte?', '-- 50 roubles.' },
 { 'Et il fait froid?', '-- Réfrigérateur est tombé en panne.',
 { 'Prends les deux!', 'Seul.',
 { 'Donnez un!', function() p [[Ok!]]; take 'brassage'; end };
}
}
}
}
}
``
En fait, c'est ainsi. Et vous pouvez ajouter 'la question Principale' et
'La principale réponse', mais que vous ne verrez pas cette question. L'affaire est dans le
le fait que lors de l'entrée dans la boîte de dialogue expression phr automatiquement révélé, ainsi
comme d'habitude il n'y a aucun sens dans les dialogues à partir d'une seule phrase.
Et beaucoup plus facile de comprendre le dialogue comme l'ensemble des élections, que seule
arborescente de la phrase. Ainsi que de phr il n'y a jamais initiale de la paire
question-réponse, mais nous entrons dans un tableau d'options, plus
il est entendu.

Quand nous parlons de ce que le dialogue est en fait réalisé une
l'expression, nous ne sommes pas tout à fait raison. Le fait est que nous avons affaire avec la phrase,
à l'intérieur de laquelle se trouvent d'autres expressions... Cela nous rappelle la situation avec
des objets. En effet, les phrases -- c'est des objets! Qui peuvent
se trouver à l'intérieur de l'un l'autre. Donc, un coup d'oeil sur le dialogue avec un regard neuf:

``
dlg {
 nam = 'conversation';
 title = [[la Conversation avec le vendeur]];
 enter = [[J'ai demandé au vendeur.]];
 phr = { -- c'est le type d'objet de la phrase, sans dsc et act
 -- c'est la 1ère phrase, à l'intérieur des phrases de la dsc et act
 { 'Avez-vous les haricots?', '-- Non.'},
 -- c'est la 2e phrase, à l'intérieur des phrases de la dsc et act
 { 'Avez-vous le chocolat?', '-- Non.'},
 -- c'est la 3e phrase, à l'intérieur des phrases avec le dsc et act
 { 'Avez-vous kvas?', '-- Oui',
 -- c'est la 1-j'ai une phrase à l'intérieur de la 3e phrases avec dsc et act
 { 'Et combien il coûte?', '-- 50 roubles.' },
 { 'Et il fait froid?', '-- Réfrigérateur est tombé en panne.',
 { 'Prends les deux!', 'Seul.',
 -- ici, act dans la forme de la fonction
 { 'Donnez un!', function() p [[Ok!]]; take 'brassage'; end };
}
}
}
}
}
``
Comme on le voit, le dialogue -- c'est la salle, et la phrase -- objets spéciaux!
Maintenant, vous expliquera la suite de l'exposé.

> Attention! Par défaut, lorsque le joueur clique sur l'une des questions
> la liste, le moteur reprend son dans la sortie et ensuite l'affiche
> la réponse. C'est fait pour que le dialogue avait l'air lié. Si
> vous souhaitez désactiver ce comportement, utilisez le réglage
> std.phrase_show:


``
std.phrase_show = false -- ne pas afficher la phrase-la question lors de la sélection
``

Ce paramètre s'applique à tous les dialogues, placez-le dans init() ou
start() de la fonction.

### Les attributs de phrases

Envisager l'option de la phrase:
``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
}
}
``

Si vous exécutez cette boîte de dialogue, puis, après la sélection, disons, rouge
la pilule, nous avons un autre choix de la pilule bleue. Mais notre
un dessein clairement pas dans cette! Il existe plusieurs façons de faire du dialogue
bon.

Tout d'abord, vous pouvez profiter de la pop() -- le retour de la précédente
le dialogue:

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 {'Rouge', function() p 'Tenez!'; pop() end; },
 {'Bleu', function() p 'Ici!'; pop() end; },
}
}
``
Ou, dans une autre inscription:

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 {'Rouge', pfn(pop) 'Tenez!' },
 {'Bleu', pfn(pop) 'Ici!' },
}
}
``
Mais il n'est pas trop à l'aise, en outre, que si ces phrases contiennent
de nouvelles phrases? Dans le cas où l'option offre un choix, et ce
le choix doit être le seul, vous pouvez définir de l'attribut de la phrase only:


``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
}
}
``
Dans ce cas, après la sélection des phrases les phrases du contexte actuel seront
fermés.

Autre chose de la situation, vous voulez que la phrase n'est pas réfugia après son
l'activation. Il se fait un travail de drapeau true:

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
 { true, 'Et quel est le meilleur?', 'Vous de choisir.' }, -- une expression
 -- ce qui ne sera jamais caché
}
}
``
Alternative enregistrement, explicite l'attribut always:

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
 { always = true, 'Et quel est le meilleur?', 'Vous de choisir.' }, -- une expression
 -- ce qui ne sera jamais caché
}
}
``

Un autre exemple. Et si nous voulons que l'expression a été montré(ou
caché) sur n'importe quel condition? Pour cela, il existe une fonction de gestionnaire
cond.

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
 { true, 'Et quel est le meilleur?', 'Vous de choisir.' }, -- une expression
 -- ce qui ne sera jamais caché
},
 { cond = function() return have 'pomme' end,
 'Voulez-vous une pomme?', 'Non merci.' };
}
``
Dans cet exemple, que si le joueur de pomme, semble branche
la boîte de dialogue 'Et que vous souhaitez la pomme?'.

Il est parfois commode d'effectuer une action au moment où les options
le niveau actuel(le contexte) le dialogue épuisé. Pour cela, utilisez
la fonction de gestionnaire onempty.


``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
 onempty = function()
 p [[Tu a fait son choix.]]
pop()
end;
},
 { cond = function() return have 'pomme' end,
 'Voulez-vous une pomme?', 'Non merci.' };
}
``
Notez que quand il y a une méthode onempty, réglage de la
le retour dans la branche n'est pas effectué, il est supposé que la méthode
onempty fera tout ce qu'il faut.

Tous les attributs peuvent être installés n'importe quelle phrase. Dans ce
le nombre et le niveau 1:

``
phr = {
 onempty = function()
 p [[Voici parlé.]]
walkout()
end;
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!' },
 {'Bleu', 'Ah!' },
 onempty = function()
 p [[Tu a fait son choix.]]
pop()
end;
},
 { cond = function() return have 'pomme' end,
 'Voulez-vous une pomme?', 'Non merci.' };
}
``

### Tags

Seulement que nous avons examiné les mécanismes de dialogues qui permettent d'
créer plutôt des dialogues difficiles. Cependant, ces fonds ne peuvent pas
suffire. Parfois, nous avons besoin de pouvoir accéder à des phrases à partir d'autres endroits
un dialogue. Par exemple, inclure de manière sélective, ou analyser leurs
l'état. Ainsi que de faire des conversions de certaines branches du dialogue dans d'autres.

Tout cela est possible pour les expressions dont il existe une balise. Créer une phrase avec la balise
très simple:


``
phr = {
 { '#quoi?', 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 {'#red', 'Rouge', 'Tenez!' },
 {'#bleu', 'Bleu', 'Ah!' },
},
}
``
Comme on le voit, la présence au début de la phrase de la ligne qui commence par le symbole
'#'signifie la présence de la balise.

Pour ces phrases fonctionnent méthodes standard, telles que seen ou
enable/disable. Par exemple, nous pourrions faire sans l'attribut only
comme suit:



``
phr = {
 { '#quoi?', 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 {'#red', 'Rouge', 'Tenez!'
 cond = function(s)
 return not closed('#bleu')
end
},
 {'#bleu', 'Bleu', 'Ah!',
 cond = function(s)
 return not closed('#rouge')
end
},
},
}
``

Tags, en outre, que les permettent de connaître et de modifier l'état spécifique
des phrases qui rendent possible les transitions entre les phrases. Pour ce faire, utilisez les
les fonctions push et pop.

push(où) -- fait la transition sur la phrase avec mémorisation de la position dans la pile.

pop([où]) - appelée sans paramètre, s'élève à 1 position
la pile de l'histoire. Vous pouvez spécifier la balise de la phrase qui doit être
dans l'histoire, dans ce cas, le remboursement sera fait sur elle.

Il faut noter que lorsque vous cliquez sur un push, nous passons d'une
la phrase, et sur la liste des phrases de cette phrase. Les divulguons, ainsi que
il est fait pour la phrase de la phr. Par exemple:


``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!', next = '#отаблетке' },
 { 'Bleu', 'Ah!', next = '#отаблетке' },
},
 { false, '#отаблетке',
 {'J'ai fait le bon choix?', 'Le temps nous le dira.'}
},
}
``
Ici, nous voyons plusieurs trucs:

- l'attribut next, au lieu de la description explicite de la réaction dans une fonction
push. next -- c'est un moyen facile d'enregistrer push.

- false au début de la phrase, qui rend la phrase est éteint. Elle se trouve à
l'état est éteint, jusqu'à ce que ne pas le faire explicite enable. Cependant, à l'intérieur de la phrase
nous pouvons aller, et afficher le contenu de l'élection. Alternative entrée
possible avec l'aide de l'attribut hidden:


``
 { hidden = true, '#отаблетке',
 {'J'ai fait le bon choix?', 'Le temps nous le dira.'}
},
``

Par conséquent, vous pouvez enregistrer les dialogues ne древовидно, et de façon linéaire. Encore
une autre caractéristique des transitions est que si la phrase n'est pas décrit
la réaction, lorsque le sera appelée en-tête de la phrase:

``
phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!', next = '#отаблетке' },
 { 'Bleu', 'Ah!', next = '#отаблетке' },
},
 { false, '#отаблетке', [[J'ai pris la pilule et le maître sourit sournoisement.]],
 {'J'ai fait le bon choix?', 'Le temps nous le dira.'},
 {'Que faire?', 'Tu es libre.'},
},
}
``
Lors de la sélection de pilules, sera appelée en-tête de la méthode de la phrase
'#отаблетке', puis sera soumis à la sélection.

Si vous aimez linéaire entrée, vous préférerez peut-être la prochaine
option:


``
dlg {
 nam = 'dialogue';
 phr = {
 { 'Que c'est chez vous?', 'La pilule. Les rouges et les bleus. Vous?',
 only = true,
 {'Rouge', 'Tenez!', next = '#отаблетке' },
 { 'Bleu', 'Ah!', next = '#отаблетке' },
}
}
}:with {
 { '#отаблетке', [[J'ai pris la pilule et le maître sourit sournoisement.]],
 {'J'ai fait le bon choix?', 'Le temps nous le dira.'},
 {'Que faire?', 'Tu es libre.'},
},
}
``
Le fait que l'attribut de phr le dialogue est le premier objet de la pièce. Mais
vous pouvez remplir les objets de la pièce de manière habituelle: une fois obj ou
with. De sorte que lors de la connexion dans la boîte de dialogue divulguée 1-j'ai la phrase, le reste
la phrase que vous ne verrez pas (attention, de la phrase '#отаблетке' est pas la peine
false), mais vous pouvez faire des sauts sur ces phrases.

### Méthodes

Comme vous le savez déjà, les objets dans INSTEAD peuvent être dans un état
ouvert/fermé et éteint/allumé. Comme il convient les phrases
le dialogue?

Pour les phrases ordinaires, après l'activation de la sélection de la phrase _закрывается_. Lors de la
vous vous reconnectez au dialogue de toutes les phrases _открываются_.

Pour les phrases avec always = true (vrai au début de la définition) -- ce
la fermeture ne se produit pas.

Pour les phrases avec hidden = true ou false au début de la définition) -- une expression
sera créée dans le désactiver. Elle ne sera pas visible jusqu'à ce que n'est pas
explicitement inclus.

Pour les phrases avec cond(), à chaque fois lors de la visualisation des phrases appelé ce
la méthode, et en fonction de la valeur de retour (true/n'est pas true) l'expression
s'allume ou s'éteint.

Sachant ce comportement, vous pouvez masquer/afficher et analyser les phrases
les fonctions types: disable / enable / empty / open / close /
closed / disabled et ainsi de suite...

Cependant, vous pouvez le faire dans la boîte de dialogue, ainsi que toutes les phrases
sont identifiés par des balises. Si vous souhaitez modifier les
état/analyser des phrases des autres pièces, vous pouvez:

- donner l'expression du nom { nam = 'nom' }...
- chercher la phrase sur la balise dans une autre pièce: local ph = lookup('#tag',
 'dialogue') et ensuite de travailler avec elle;

En ce qui concerne les fonctions de push/pop, vous pouvez les appeler explicitement comme
les méthodes de dialogue, par exemple:

``
 _'dialogue'push '#nouvelle'
``

Mais le mieux est de le faire dans la boîte de dialogue, par exemple, dans l'entrée.

En outre, il y a une méthode :reset qui remet la pile de navigation et
définit le départ de la phrase, par exemple:

``
enter = function(s)
 s:reset '#début'
end
``
> Il convient de noter que lorsque vous faites enable/disable/open/close
> une phrase, vous effectuez une action, c'est au-dessus de cette phrase, et non pas au-dessus de la
> les phrases inscrites à l'intérieur. Mais comme lors de la diffusion des phrases moteur
> s'arrête sur le off/fermé l'objet de la phrase et n'entrera pas
> à l'intérieur, cela suffit.


## Objets spéciaux

Dans STEAD3 il existe des objets qui remplissent
des fonctions spécifiques. Tous ces objets peuvent être divisés en deux
classe:

1. Les objets système @;
2. Substitution $.

Le système des objets, ce sont des objets dont le nom commence par le caractère
'@' ou '$'. Ces objets sont généralement créés dans le _модулях_. Ils ne sont pas détruits
lors de la mort du monde du jeu (par exemple, lors du chargement des gamefile, lors de la
le téléchargement de jeu de la conservation, et ainsi de suite). Exemples d'objets: @timer
@prefs, @snd.

Ces objets, en plus de ses fonctions spéciales, peuvent être
utilisé sur un lien, sans explicitement le principe de l'objet dans la scène ou
de l'inventaire, mais le mécanisme d'action de ces objets -- particulier.

### L'objet '@'

Habituellement, vous n'avez pas besoin de travailler avec de tels objets, mais comme un
exemple d'envisager la mise en œuvre 'liens'.

Laissez-nous voulons faire le lien, en cliquant sur laquelle nous nous déplaçons dans
une autre pièce. Bien sûr, nous pourrions ajouter un objet dans la scène, mais vaut la peine
- il faire dans ce cas le plus simple?

Comment on peut aider le système un objet?

``
obj {
 nam = '@walk';
 act = function(s, w)
 walk(w, false, false)
end;
}
room {
 nam = 'main';
 title = 'Début';
 decor = [[Démarrer {@walk marche|aventure}]];
}
``
Lorsque vous cliquez sur le lien "aventure" est appelée act de l'objet
'@walk' avec l'option "start".

En fait, dans la bibliothèque stdlib déjà l'objet
le nom de '@', qui vous permet de faire vos gestionnaires de liens suivants
ainsi:


``
xact.walk = walk

room {
 nam = 'main';
 title = 'Début';
 decor = [[Démarrer {@ walk marche|aventure}]];
}
``

Notez, sur la barre d'espace après le @. Cette entrée est la suivante:

- prend un objet '@' (un objet créé par la bibliothèque stdlib);
- prend act;
- appelle act avec les paramètres de la promenade et de la marche;
- act objet '@' regarde dans le tableau xact;
- walk définit la méthode qui sera appelée à partir d'un tableau xact;
- marche -- paramètre à cette méthode.

Un autre exemple:

``
xact.myprint = function(w)
 p (w)
end

room {
 nam = 'main';
 title = 'Début';
 decor = [[Clique {@ myprint "hello world";|sur}]];
}
``
### Substitution

Des objets dont le nom commence par le caractère '$' est également considéré comme système
les objets, mais ils fonctionnent différemment.

Si dans le texte de sortie rencontre "le lien" de la sorte:

``
{$my a b c|texte}

``
Ce est la suivante:

1. Est prise de l'objet $my;
2. Pris act de l'objet $my;
3. Appelé act: _'$my':(a, b, c, texte);
4. La chaîne retournée remplace toute la structure {...}.

Par conséquent, les objets jouent un rôle de substitution.

Pourquoi avez-vous besoin? Imaginez que vous avez développé un module qui
transforme l'écriture des formules du type de texte à l'image. Vous écrivez
l'objet $de math qui, dans son act méthode transforme le texte en graphique
l'image (sprite) et retourne dans le flux de texte. Alors
utiliser ce module est très simple, par exemple:

``
{$math|(2+3*x)/y^2}
``

## Les événements dynamiques

Vous pouvez définir des gestionnaires qui sont exécutées chaque fois,
quand le temps de jeu augmente de 1. D'habitude, cela a un sens pour les vivants
les personnages, ou de certains processus d'arrière-plan du jeu. L'algorithme de l'étape du jeu
ressemble à:
 - Le joueur clique sur le lien;
 - La réaction de 'act', 'use", 'inv', 'tak', l'inspection de la scène (clic sur le titre
 de la scène) ou le passage à une autre scène;
 - Dynamique de l'événement;
 - Conclusion d'un nouveau état de la scène.

Par exemple, nous le ferons Барсика vivant:

``
obj {
 nam = 'Barsik';
 { -- ne pas enregistrer un tableau de lf
 lf = {
 [1] = 'Barsik bouge j'ai un sein.',
 [2] = 'Barsik peeps éclatante.',
 [3] = 'Barsik мурлычит j'ai un sein.',
 [4] = 'Barsik tremble j'ai un sein.',
 [5] = 'Je sens la chaleur Барсика de sein.',
 [6] = 'Barsik bâtons sur la tête à cause de sinus et inspecte le terrain.',
};
};
 life = function(s)
 local r = rnd(5);
 if r > 2 then -- cela n'est pas toujours
return;
end
 r = rnd(#s.lf); -- le symbole # est le nombre d'éléments dans le tableau
 p(s).lf[r]); -- affichons l'une des 6 états Барсика
end;
....
``

Et voici le moment dans le jeu, quand Barsik se trouve chez nous dans le sein!

``
take 'Barsik' -- ajouter à l'inventaire
lifeon 'Barsik' -- pimenter Барсика!
``

N'importe quel objet (y compris la scène) peut avoir son gestionnaire 'life',
qui est appelée à chaque cycle d'horloge du jeu, si l'objet a été ajouté à la liste
les installations de la vie avec l'aide de 'lifeon'. N'oubliez pas de supprimer des objets vivants
à partir de la liste à l'aide de 'lifeoff', quand ils ne sont plus nécessaires. Il est possible de
faire, par exemple, dans le gestionnaire 'exit', ou de toute autre manière.


> Si votre jeu de la "vie" des objets, vous pouvez leur poser des
> explicite position dans la liste, lors de l'ajout. Pour cela, utilisez l'
> le deuxième paramètre numérique (entier nombre négatif) 'lifeon',
> plus le nombre est élevé, plus la priorité est élevée. 1 -- le plus haut. Ou vous
> pouvez utiliser l'attribut pri de l'objet. Certes, cet attribut
> aura une incidence sur la priorité de l'objet dans n'importe quelle liste.


Si vous avez besoin d'un processus d'arrière-plan dans une certaine pièce, exécutez-le dans
'enter' et supprimez le 'exit', par exemple:

``
room {
 nam = 'Dans le sous-sol';
 dsc = [[Ici foncé!]];
 enter = function(s)
lifeon(s);
end;
 exit = function(s)
lifeoff(s);
end;
 life = function(s)
 if rnd(10) > 8 then
 p [[J'entends des bruissements!]];
 -- parfois effrayer le joueur шорохами
end
end;
 way = { 'Maison' };
}
``

Si vous avez besoin de déterminer si le passage d'un joueur d'une scène à
l'autre, utilisez le 'player\_moved()'.

``
obj {
 nam = 'poche';
 on = false;
 life = function(s)
 if player_moved() then -- éteindre la lampe de poche lors de la navigation
 s.on = false
 p "J'ai éteint la lampe de poche."
return
end;
end;
...
}
``

Pour le suivi se produisent dans le temps des événements, utilisez le 'time()'
ou d'un rôle de la variable compteur. Pour déterminer l'emplacement de la
le joueur -- 'here()'. Pour la détermination du fait que l'objet est "vivant" --
'live()'.


``
obj {
 nam = 'dynamite';
 timer = 0;
 used = function(s, w)
 if w^'match' then -- allumette?
 if en direct(s) then
 return "est Déjà allumé!"
end
 p "J'ai mis le feu à la dynamite."
lifeon(s)
return
end
 return false -- si vous ne l'allumette
end;
 life = function(s)
 s.timer = s.timer + 1
 if s.timer == 5 then
lifeoff(s)
 if here() == where(s) then
 p [[la Dynamite a explosé à côté de moi!]]
else
 p [[J'ai entendu, comme la dynamite a explosé.]];
end
end
end;
...
}
``

Si 'life' gestionnaire renvoie le texte de l'événement, il est imprimé après
description de la scène.

Vous pouvez retourner à partir du gestionnaire de 'life' le deuxième code de retour ('true'
ou 'false'). Si vous retournez true -- ce sera un signe important
l'événement, qui permet d'imprimer à la description des objets de la scène, par exemple:

``
p 'entra Dans la pièce, le gardien de sécurité.'
return true
``

Ou:
``
return 'entra Dans la pièce, gardien de sécurité.', true
``

Si vous retournez false, alors la chaîne life méthodes échoue sur vous. C'est
l'aise de le faire lors de l'exécution walk à partir de la méthode life, par exemple:


``
life = function()
 walk 'theend'
 return false -- c'est la dernière life
end
``

Si vous voulez bloquer l' 'life' des gestionnaires dans une des chambres,
utilisez le module 'nolife'. Par exemple:

``
require "noinv"
require "nolife"

dlg {
 nam = 'Garde';
 noinv = true;
 nolife = true;
...
}
``

Il vaut la peine d'examiner la question du transfert du joueur de 'life'
le gestionnaire. Si vous allez utiliser la fonction 'walk' à l'intérieur
'life', alors vous devriez prendre en compte le comportement suivant.

Si 'life' transporte le joueur dans un nouveau lieu, alors on ne le suppose généralement
que vous:

1. Nettoyez la conclusion de réactions: game:reaction(false);
2. Nettoyez la conclusion vivants méthodes en ce moment: game:events(false,
false)
3. Faites walk.
4. Arrêtez la chaîne de life des appels à l'aide return false;

Certains points nécessitent des explications.

game:reaction() -- permet de prendre/modifier la conclusion de la réaction
de l'utilisateur, si le définir false, cela signifie perdre la réaction.

game:events() -- permet de prendre/modifier la conclusion life méthodes. Dans
comme les paramètres sont mis à jour prioritaires et non prioritaires
le message, en définissant false, false, nous avons supprimé toutes les sorties précédentes life
des méthodes.

La bibliothèque standard est déjà une fonction life_walk(), ce qui fait
les étapes. Il ne vous reste plus qu'à retourner false.

## Graphique

Interpréteur graphique INSTEAD analyse de l'attribut de la scène 'pic', et
qu'il perçoit comme le chemin de l'image, par exemple:

``
room {
 pic = 'gfx/home.png';
 nam = 'Maison';
 dsc = 'Je suis à la maison';
};
``

__Il est important!__

Utilisez les voies directes '/'. Aussi, il est fortement
il est recommandé d'utiliser des noms de répertoires et de fichiers uniquement
latines minuscules. Ce faisant, vous vous обезопасите son jeu de
les problèmes de compatibilité et de fonctionner sur tous les architecturales
les plates-formes où porté INSTEAD.

Bien sûr, le 'pic' peut-être une fonction, en élargissant les possibilités de développement.
Si la scène n'est pas définie par l'attribut 'pic', c'est par défaut de l'attribut
'game.pic'. Si n'est pas défini et il est, l'image ne s'affiche pas.

Prend en charge tous les formats les plus courants de l'image, mais je
vous recommande d'utiliser le 'png' et en important la taille) 'jpg'.

Vous pouvez l'utiliser comme images gif animés.

Vous pouvez intégrer des graphiques directement dans le texte, dans la
y compris dans l'inventaire, les transitions, les titres de pièces et de 'dsc' à l'aide de
la fonction 'fmt.img' (Pour ce faire, activez le module fmt).

Par exemple:

``
require "fmt"

obj {
 nam = 'pomme'
 disp = 'Pomme'..fmt.img('img/apple.png');
}
``

Ce n'est pas moins de, une image de la scène est toujours doit être rédigé dans la forme d'un 'pic'
l'attribut, et non pas l'insertion 'fmt.img' en 'dsc' de la pièce.

Le fait que l'image de la scène est multipliée par un autre algorithme.
Des photos 'fmt.img' évoluent en fonction des paramètres INSTEAD
(l'échelle de la conversation), et le 'pic' -- tient également compte de la taille de l'image.

En outre, les images 'pic' qui possèdent d'autres propriétés, par exemple,
la possibilité de suivre les coordonnées de clics de souris.

Si vous placez le 'fmt.img' à l'intérieur de { et }, obtenez un bandeau.

``
obj {
 nam = 'pomme';
 disp = 'pomme' ..img('img/apple.png');
 dsc = function(s)
 p ("Sur le plancher est {la pomme",fmt.img 'img/apple.png', "}");
 -- d'autres options:
 -- return "Sur le plancher est {la pomme"..fmt.img('img/apple.png').."}";
 -- p "Sur le plancher est {la pomme"..fmt.img('img/apple.png').."}";
 -- ou dsc = "Sur le plancher est {la pomme"..fmt.img('img/apple.png').."}";
end;
}
``

INSTEAD prend en charge l'habillage des images du texte. Si l'image
inséré à l'aide de la fonction 'fmt.imgl'/'fmt.imgr', elle sera
située à la gauche/droite.

__Il est important!__

> Les images insérées dans le texte à l'aide de 'fmt.imgl/fmt.imgr' ne peuvent pas être
> le lien!!! Utilisez-les uniquement à des fins décoratives.

Pour les travaux de retrait autour de l'image, utilisez le 'pad', par exemple:

``
fmt.imgl 'pad:16,picture.png' -- retrait de 16 de chaque bord
fmt.imgl 'pad:0 16 16 4,picture.png' -- marges: haut de 0, à droite 16, en bas de 16 à gauche 4
fmt.imgl 'pad:0 16,picture.png' -- marges: haut de 0, à droite 16, en bas de 0, à gauche 16
``

Vous pouvez utiliser les pseudo-fichiers pour les images de rectangles et de
les zones vides:

``
dsc = fmt.img 'blank:32 x 32'..[[Ligne vide de l'image.]];
dsc = fmt.img 'boîte:32 x 32,red,128'..[[Ligne rouge semi-transparent carré.]];
``

INSTEAD peut traiter des images composites, par exemple:

``
pic = 'gfx/mycat.png;gfx/milk.png@120,25;gfx/fish.png@32,32';
``

Par conséquent, une composante de l'image est un ensemble de chemins de
des images, séparées par le caractère ';'. Le deuxième et le suivi des
les composants peuvent contenir des postfix sous la forme de
@x\_координата,y\_координата%, où la coordonnée 0,0 correspond à gauche
l'angle supérieur de toute l'image. Commune de la taille de l'image est égal à
la taille totale d'un premier composant composite d'images, c'est, d'abord
le composant (dans notre exemple -- gfx/mycat.png) joue le rôle de la toile, et
les composants en surimpression sur cette toile.

La superposition se produit à l'angle supérieur gauche durant
de l'image. Si vous avez besoin de la superposition était relativement
centre durant des images, utilisez avant de coordonnées préfixe
"c", par exemple:

``
pic = 'gfx/galaxy.png;gfx/star.png@c128,132';
``

Souscrivez en fonction de la formation de la voie composite images, vous pouvez
générer une image basée sur le jeu de l'état.

Si vous êtes dans son jeu привязываетесь les coordonnées
l'image, ou à leur taille, faites-le, c'est relativement original
la taille des images. Lors de la mise à l'échelle des thèmes sous la valeur de consigne de joueur
la résolution, INSTEAD sera lui-même effectuer la conversion de coordonnées (lors de la
cet coordonnées pour les jeux semblent comme si le jeu est lancé sans
mise à l'échelle). Cependant, il peut y avoir des petites erreurs de calcul.

Si vous n'avez pas les fonctions décrites dans ce chapitre, apprenez le module
"sprite", qui offre la possibilité de
graphique prsentation. Mais je, il ne recommande pas de le faire dans sa
le premier jeu.

## Musique

Pour la musique et les sons que vous aurez besoin d'un module snd.

``
require "snd"

``

L'interpréteur perd dans le cycle actuel de la musique, qui est donnée par
avec l'aide de la fonction: 'snd.music(chemin d'accès au fichier audio)'.

__Il est important!__

Utilisez les voies directes '/'. Aussi, il est fortement
il est recommandé d'utiliser des noms de répertoires et de fichiers uniquement
latines minuscules. Ce faisant, vous vous обезопасите son jeu de
les problèmes de compatibilité et de fonctionner sur tous les architecturales
les plates-formes où porté INSTEAD.

Prend en charge la plupart des formats de musique, mais fortement
il est recommandé d'utiliser le format 'ogg', car c'est
prise en charge de la meilleure façon dans toutes les versions INSTEAD (pour
les différentes plates-formes).

__Il est important!__

Il faut faire preuve de prudence lors de l'utilisation de трекерной de la musique
comme dans certaines distributions Linux, peut-être un problème lors de la
la lecture de certains fichiers (erreurs dans la liasse des bibliothèques SDL_mixer
et libmikmod).

Aussi, si vous utilisez 'mid' fichiers, soyez prêts à ce que
le joueur entendra seulement dans la version Windows INSTEAD (car
la plupart des cas, les versions d'Unix SDL_mixer recueillies sans le soutien de
"timidity").

Comme la fréquence des fichiers de musique utilisez des multiples de la fréquence
11025.

``
room {
 pic = 'gfx/street.png';
 entrée = function()
 snd.music 'mus/rain.ogg'
end;
 nam = 'dans la rue';
 dsc = 'dans La rue, il pleut.';
};
``

'snd.music()' sans le paramètre renvoie le nom de la piste.

Dans la fonction 'snd.music()' vous pouvez envoyer un deuxième paramètre -- le nombre de
проигрываний (cycles). Pour obtenir le compteur, vous pouvez à l'aide de
'snd.music()' sans les options -- la deuxième valeur de retour. 0 --
signifie un cycle perpétuel de. 1..n-le nombre de проигрываний. -1 --
la lecture de la piste actuelle est fini.

Pour annuler la lecture de la musique, vous pouvez utiliser
'snd.stop\_music()'

Pour jouer de la musique:


``
snd.music_playing()
``

Vous pouvez définir des temps de montée et de décroissance de la musique, avec l'aide de l'appel:


``
snd.music_fading(o, [i])
``

Ici o le temps (en ms). pour l'amortissement et i - le temps (en ms). pour la montée
de la musique. Si vous spécifiez un paramètre -- les deux temps sont considérés comme
la même. Après l'appel, les paramètres vont influencer
lecture de tous les fichiers de musique.

Pour la lecture des sons, utilisez 'snd.play()'. Vivement
il est recommandé d'utiliser le format 'ogg', bien que la plupart des
courants avec les formats audio fonctionnera également.

La différence entre la musique et le fichier audio est que le moteur
surveille les processus de la lecture de la musique et sauve/restaure
actuel jouable de la piste. En sortant de jeux et en le téléchargeant de nouveau joueur
entend la même musique que j'ai entendu lors de la sortie. Les sons
signifie généralement à court terme des effets, et le moteur ne stocke pas et n'est pas
récupère les événements sonores. Ainsi, si un joueur n'a eu le temps d'attente de l'entendre
bruit du coup de feu et a quitté le jeu, après le téléchargement d'un fichier de sauvegarde, il n'est pas
entendrait le son (ou la fin) de nouveau.

Toutefois, si l'on considère que 'snd.play()' permet d'exécuter
зацикленные les sons, la différence entre la musique et les sons est déjà
pas univoque.

Donc, la définition de la fonction: 'snd.play(fichier, [canal], [cycle])', où:

- le fichier -- le chemin d'accès et\ou le nom d'un fichier;
- le canal -- le numéro de canal [0..7]; Si vous ne spécifiez pas, il dépasserait le premier gratuit.
- cycle -- nombre de проигрываний 1..n, 0 -- boucle.

Pour arrêter la lecture d'un son, vous pouvez utiliser " snd.stop()'. Pour
arrêter le son dans un canal 'snd.stop(canal)'.

__Il est important!__

> Si vous utilisez зацикленные sons, vous aurez à
> restaurer leur état d'exécuter de nouveau à l'aide de
> 'snd.sound()') dans la fonction start()'

Par exemple:

``
global 'wind_blow' (false)
...
function start()
 if then wind_blow
 snd.play('snd/wind.ogg', 0)
end
end
``

Si vous n'avez pas assez les fonctions décrites audio,
utilisez une description complète du module "snd".

## Mise en forme et la décoration de la sortie

Habituellement, INSTEAD lui-même s'occupe de la mise en forme et la décoration
sortie. Par exemple, sépare statique de la scène de la dynamique. Alloue
en italique les actions du joueur. Déplace le focus sur le changement dans le texte et
etc. Modules comme "fmt" améliorer la qualité de la sortie du jeu sans
des efforts supplémentaires de la part de l'auteur.

Par exemple:

``
require 'fmt'
fmt.para = true -- activer l'indentation des paragraphes
``

Et votre jeu sera beaucoup plus agréable. Si vous avez besoin d'une sorte de
traitement automatique de la sortie de texte, vous pouvez activer le module
"fmt" et déterminer la fonction 'fmt.filter'. Par exemple:


``
require "fmt"
fmt.filter = function(s state)
 -- s -- conclusion
 -- state -- true si c'est le rythme du jeu (la conclusion de la scène)
 if then state
 return 'Cette ligne sera ajouté au début de la sortie\n'..s;
end
 return s
end
``

Beaucoup de bons jeux sur INSTEAD ne s'occupent de par sa conception,
outre la pagination du texte de 'dsc' sur les paragraphes à l'aide de caractères '^^',
alors, pensez, et si vous avez envie de faire la décoration de sa
le jeu manuellement?

Cependant, parfois, c'est tout de même nécessaire.

> Attention! Par défaut, tous les paramètres et les sauts de ligne,
> les espaces et tabulations sont coupées à partir de la sortie de gestionnaires. Ainsi
> comme d'habitude, ils n'ont pas de sens, et même dangereux. Dans de rares cas,
> l'auteur peut avoir besoin de plus de contrôle sur la sortie, alors il
> peut définir std.strip\_call à false dans init() ou start(),
> par exemple:

``
std.strip_call = false

obj {
 dsc = [[Là que réside {pomme}.^^^^]] -- maintenant, les transferts de lignes
 -- ne seront pas coupés, bien que cette envie étrange
}
``

> Mais le plus souvent cette mise en forme manuelle témoigne de la mauvaise
> le style. Pour la prsentation de la scène est préférable d'utiliser decor et/ou
> de substitution $.

### Mise en forme

Vous pouvez faire une simple mise en forme du texte à l'aide de fonctions:

- fmt.c(chaîne) - placer au centre;
- fmt.r(chaîne) - placer sur la droite;
- fmt.l(ligne) - placer à gauche;
- fmt.top(chaîne) - en haut de la chaîne;
- fmt.bottom(chaîne) - en bas de la chaîne;
- fmt.middle(chaîne) - le milieu de la ligne (par défaut).

Par exemple:
``
room {
 nam = 'main';
 title = 'bienvenue';
 dsc = fmt.c 'bienvenue!'; -- si la fonction 1 seul paramètre,
 -- crochets, vous pouvez omettre;
}
``

Ces fonctions ne touchent pas seulement le texte, mais sur l'image,
insérées à l'aide de "fmt.img()'.

Il convient de noter que si vous utilisez plusieurs fonctions
mise en forme, il suppose qu'ils sont sur des lignes différentes
(paragraphes). Dans le cas contraire, le résultat n'est pas défini. Cassez
le texte en paragraphes des caractères '^' ou 'pn()'.

INSTEAD lors de la sortie supprime les espaces supplémentaires. Cela signifie que peu importe
combien d'espaces vous insérez entre les mots, toujours lors de la sortie ils
ne seront pas pris en compte pour le calcul de la distance entre les mots. Parfois, il est
peut devenir un problème.

Vous pouvez créer des _неразрывные строки_ avec l'aide de:
fmt.nb(chaîne). Par exemple, le module "fmt" utilise indissociables de la chaîne
pour créer un retrait en début de paragraphe. Aussi, 'fmt.nb' peut
s'avérer utile pour la sortie de service. On peut dire que
la totalité de la chaîne-l'option 'fmt.nb' est perçu le moteur comme un grand
mot.

Un autre exemple. Si vous utilisez un trait de soulignement de texte, 
les intervalles entre les mots ne seront soulignées. Lors de l'utilisation
'fmt.nb' espaces seront également soulignées.

INSTEAD ne prend pas en charge l'affichage des tables, mais pour la sortie de simples
les données de la table, vous pouvez utiliser 'fmt.tab()'. Cette fonction
utilisé pour le positionnement absolu dans la chaîne (tabulation).

 fmt.tab(position, [centre])

_Позиция_, c'est un texte ou une valeur numérique. Si vous configurez le numérique
l'option, il est considéré comme la position en pixels. Si elle est définie dans
la forme de la chaîne 'nombre%', il est perçu comme
la position exprimée en pourcentage de la largeur de la fenêtre de sortie de la scène.

Facultatif une valeur de chaîne de _центр_ définit la position suivante
pour le 'fmt.tab' un mot, qui sera publiée sur le numéro de déplacement en
ligne. Les positions peuvent être les suivantes:

- left;
- right;
- center.

Par défaut, il croit que l'option "left".

Ainsi, par exemple:

``
room {
 nam = 'main';
 disp = 'Début';
 -- le placement de 'Début!' au centre de la chaîne
 dsc = fmt.tab('50%', 'center')..'Début!';
}
``

Bien sûr, n'est pas très bon exemple, parce que la même chose peut être fait
faire avec l'aide de 'fmt.c()'. Un bon exemple.

 dsc = function(s)
 p(fmt.tab '0%')
 p "de Gauche";
 p(fmt.tab '100%', 'right')
 p "de Droite";
end

En fait, la seule situation où l'application de la 'fmt.tab()'
justifiée -- c'est la conclusion de données tabulaires.

Il convient de noter que dans une situation où nous écrivons quelque chose comme:

``
-- placement 'Fois' au centre de la chaîne
dsc = fmt.tab('50%', 'center')..'Fois tous les deux ou trois!';
``

Seulement le mot 'Fois' est placé au centre de la ligne, le reste des mots
seront ajoutés à la droite de ce mot. Si vous voulez centrer 'Fois
deux ou trois!' comme une seule unité, utilisez 'fmt.nb()'.

``
-- placement 'une Fois les deux ou trois!' au centre de la chaîne
dsc = fmt.tab('50%', 'center')..fmt.nb ('Fois les deux ou trois!');
``

Dans INSTEAD il existe également la possibilité d'effectuer une simple verticale
la mise en forme. Pour ce faire, utilisez la tabulation verticale:

 fmt.y(position, [centre])

Comme dans le cas de fmt.tab _позиция_, c'est un texte ou numérique
le paramètre. Ici, il est perçu comme la position de la chaîne, exprimée en
pixels ou en pourcentage de la hauteur de la zone de la scène. Par exemple, 100% --
correspond à la limite inférieure de la zone de la scène. 200% -- correspond à la
la limite inférieure de la deuxième page de sortie (deux fois la hauteur de la zone de sortie
la scène).

Facultatif une valeur de chaîne de _центр_ définit la position à l'intérieur
la ligne sur laquelle se fait le positionnement:

- top (sur le bord supérieur);
- middle (au centre);
- le fond (sur le bord inférieur -- valeur par défaut).

Il convient de noter que 'fmt.y' est la totalité de la ligne. Si
la barre de rencontrer plusieurs fmt.y, agir sera le dernier
tabulations.

``
-- le placement 'CHAPITRE I - le centre de la scène
dsc = fmt.y('100%').."CHAPITRE I";
``
_Если la position indiquée taquet, déjà occupée par une autre chaîne, la tabulation est ignorée._

Par défaut, la partie statique de la scène est séparée de la dynamique d'un double saut de ligne. Si cela ne vous convient pas, vous pouvez remplacer 'std.scene_delim', par exemple:

 std.scene_delim = '^' -- simple saut de ligne

Vous ne pouvez pas changer cette variable dans les gestionnaires, car elle n'est pas
enregistré, mais vous pouvez demander pour un jeu entier, ou
restaurer manuellement dans la fonction start()'.

Si vous n'est absolument pas à l'aise avec ce que INSTEAD forme la conclusion
(séquence абзацов de texte), vous pouvez substituer la fonction
'game.display()', qui par défaut est la suivante:


``
game.display = function(s state)
 local r, l, av, pv
 local reaction = s:reaction() or nil -- une réaction
 r = std.here()
 if then state -- rythme de jeu?
 reaction = iface:em(reaction) -- italique
 av, pv = s events()
 av = iface:p(av) -- affiche "importants" life
 pv = iface:em(pv) -- la conclusion de fond life
 l = s.player:look() -- objects [and scene] -- objets et de la scène
end
 l = std.par(std.scene_delim,
 reaction or false, av or false, l or false,
 pv or false) or "
 return l
end;
``

Le fait que je vous ai amené ici, ce code ne signifie pas que je recommande
remplacer cette fonction. Au contraire, je suis totalement contre cette
une forte liaison à la mise en forme du texte. Cependant, parfois
la situation se produit lorsque le contrôle complet de la séquence
la sortie est nécessaire. Si vous écrivez votre premier jeu, il suffit de sauter
de ce texte.

### Decoration

Vous pouvez changer le style de police du texte à l'aide de combinaisons de fonctions:

- fmt.b(string) - le texte en gras;
- fmt.em(chaîne) - italique;
- fmt.u(chaîne) - texte barré;
- fmt.st(ligne) - texte barré.

Par exemple:
``
room {
 nam = 'Intro';
 title = false;
 dsc = function(s)
 p ('Vous êtes dans la salle: ')
 p (fmt.b(s))
end;
}
``

> À l'aide de la fonction 'fmt.u' et 'fmt.st' sur les lignes contenant des espaces
> vous obtenez des sauts de lignes dans ces lieux. D'éviter cela, vous pouvez
> convertir le texte dans le _неразрывную строку_:

 fmt.u(fmt.nb maintenant le texte sans laissez-passer" )

Strictement parlant, INSTEAD ne prend pas en charge la production simultanée de plusieurs polices dans la scène de la fenêtre (sans compter les divers caractères, donc si vous voulez plus de contrôle flexible de sortie, vous pouvez faire ce qui suit:

- Utiliser les graphiques insertion 'fmt.img()';
- Utiliser le module 'fonts', qui met en œuvre le rendu
 différentes polices grâce à un module 'sprite';
- Utiliser un autre moteur, de sorte que les chances sont que vous utilisez INSTEAD non conforme.

## Constructeurs et héritage

__Attention!__

Si vous écrivez votre premier match, il serait mieux si elle était
simple. Pour un simple jeu, les informations de ce chapitre n'est pas
besoin. De plus, 90% des jeux sur INSTEAD n'utilise pas les choses
décrits dans ce chapitre!

Si vous écrivez un jeu dans lequel de nombreux types d'objets identiques, peut-être
vous aurez envie d'en faciliter la création. Il est possible de faire l'un des
méthodes suivantes:

- Créer votre constructeur;
- Créer une nouvelle classe d'objets.

### Concepteurs

Le constructeur -- c'est la fonction qui crée pour vous un objet et remplit
ses attributs de sorte que vous en avez besoin. Prenons l'exemple. Par exemple, dans le
votre jeu sera beaucoup de fenêtres. Besoin de créer des fenêtres de n'importe quelle fenêtre, vous pouvez
briser. Nous pouvons écrire un constructeur 'window'.

``
window = function(v)
 v.window = true
 v.broken = false
 if v.dsc == nil then
 v.dsc = 'Ici a {fenêtre}.'
end
 v.act = function(s)
 if s.broken then
 p [[Fenêtre brisée.]]
else
 p [[En dehors de la fenêtre sombre.]]
end
end
 if v.used == nil then
 v.used = function(s, w)
 if w^'marteau' then
 if s.broken then
 p [[la Fenêtre est déjà brisé.]]
else
 p [[J'ai cassé la fenêtre.]]
 s.broken = true;
end
return
end
 return false
end
end
 return obj(v)
end
``
Comme on le voit, l'idée des concepteurs est simple. Il vous suffit de créer une fonction,
qui reçoit sur une entrée de la table des attributs {} que constructeur
peut-дозаполнить les bons attributs. Ensuite, ce tableau est transmis
le constructeur obj/room/dlg et retourne l'objet.

Maintenant, pour créer des fenêtres, il est devenu facile:

``
window {
 dsc = [[Ici, il est {la fenêtre}.]];
}
``

Ou, puisque la fenêtre est généralement un objet statique, vous pouvez le créer
directement dans 'obj'.

``
obj = { window {
 dsc = 'Dans le mur est un {fenêtre}.';
}
};
``

De notre fenêtre sera prêt used méthode et la méthode act. Vous pouvez
vérifier le fait que l'objet de la fenêtre -- il suffit de vérifier le symptôme de la fenêtre:

``
use = function(s, w)
 if w.window then
 p [[Action sur la fenêtre.]]
return
end
 return false
end
``

L'état de "faiblesse" de la fenêtre, c'est l'attribut de broken.

Comment mettre en œuvre l'héritage dans les concepteurs?

En fait, dans l'exemple ci-dessus est déjà utilisé
l'héritage. En effet, en effet, le constructeur 'window" provoque
un autre constructeur 'obj', ainsi obtenir toutes les propriétés de l'ordinaire
d'un objet. Aussi, 'window' définit le signe de la variable 'window',
le jeu que nous puissions comprendre que nous avons affaire à une fenêtre. Par exemple:

Pour illustrer le mécanisme d'héritage de la création d'une classe d'objets
'treasure', les. trésors.

``
global { score = 0 }
treasure = function()
 local v = {}
 v.disp = 'trésor'
 v.treasure = true
 v.points = 100
 v.dsc = function(s)
 p ('Ici, il ya {', std.dispof(s), '}.')
end;
 v.inv = function(s)
 p ('c'Est le même ', std.dispof(s), '.');
end;
 v.tak = function(s)
 score = score + s.points; -- augmenter la facture
 p [[mains Tremblantes j'ai enlevé les trésors.]];
end
 return obj(v)
end
``

Et maintenant, sur la base de allons créer de l'or, le diamant et le coffre.

``
gold = function(dsc)
 local v = treasure();
 v.disp = 'or';
 v.gold = true;
 v.points = 50;
 v.dsc = dsc;
 return v
end

diamond = function(dsc)
 local v = treasure();
 v.disp = 'diamant';
 v.diamond = true;
 v.points = 200;
 v.dsc = dsc;
 return v
end

chest = function(dsc)
 local v = treasure();
 v.disp = 'coffre';
 v.chest = true
 v.points = 1000;
 v.dsc = dsc;
 return v
end
``

Maintenant, dans le jeu, vous pouvez créer des trésors à travers les concepteurs:

``
diamond1 = diamond("Dans la boue, j'ai remarqué {diamant}.")
diamond2 = diamond(); -- il y aura une description par défaut du diamant
gold1 = gold("Dans le coin, j'ai remarqué l'éclat {or}.");

room {
 nam = 'cave';
 obj = {
diamond1,
gold1,
 chest("Et je vois {coffre}!")
};
}
``

En fait, comme c'est d'écrire des fonctions d'études et de réaliser
le principe d'héritage, ne dépend que de vous. Choisissez la plus simple
et de façon intuitive.

Lors de la rédaction des constructeurs il est parfois utile de faire appel
le gestionnaire, comme le fait INSTEAD. Pour cela, utilisez
'std.call(un objet, une méthode, paramètres)', dans ce cas, la fonction retourne
la réaction de l'attribut sous la forme d'une chaîne. Prenons l'exemple d'une modification de
'window', qui est que vous pouvez définir sa
la réaction de l'inspection de la fenêtre, qui sera exécuté après la norme
un message indiquant qu'il a cassé la fenêtre (si elle est cassée).

``
window = function(nam, dsc, what)
 local v = {} -- crée une table vide
 -- remplissez-le
 v.window = true
 v.what = what
 v.broken = false
 if dsc == nil then
 v.dsc = 'Ici, il ya {fenêtre}'
end
 v.act = function(s)
 if s.broken then
 p [[Fenêtre brisée.]]
end
 local r, v = place.call(s, 'what')
 if v then -- gestionnaire ait été exécuté?
p(r)
else
 p [[En dehors de la fenêtre sombre.]]
end
end
 return obj(v)
end
``

Par conséquent, nous pouvons, lors de la création de la fenêtre d'un troisième paramètre, 
pour identifier la fonction ou de la chaîne qui va de la réaction dans le temps
l'inspection de la fenêtre. Ce faisant, le message indiquant que la boîte est cassé (si elle
vraiment cassé), vous êtes en face de cette réaction.


### La classe d'objets

Concepteurs d'objets ont été largement utilisés dans STEAD2. DANS STEAD3
obj/dlg/room sont implémentés comme des classes d'objets. Classe d'entités en pratique
créer pour les cas où le comportement de l'objet à créer n'est pas
cela rentre dans le standard des objets obj/room/dlg et que vous voulez changer
les méthodes de la classe. En modifiant la méthode de la classe, par exemple, vous pouvez généralement
changer l'apparence de l'objet dans la scène. À titre d'exemple,
envisager la création d'une classe "conteneur". Le conteneur peut stocker une
d'autres objets, être fermé et ouvert.

``
-- create own class container
cont = std.class({ -- créons une classe de cont
 __cont_type = true; -- pour déterminer le type de l'objet
 display = function(s) -- переопределяем la méthode de présentation de l'objet
 local d = std.obj.display(s)
 if s:closed() or #s.obj == 0 then
 return d
end
 local c = s.cont or 'à l'Intérieur:' -- spécificateur de contenu
 local empty = true
 for i = 1, #s.obj do
 local o = s.obj[i]
 if o:visible() then
 empty = false
 if i > 1 then c = c .. ', ' end
 c = c..'{'..std.nomde(o)..'|'..std.dispof(o)..'}'
end
end
 if empty then
 return d
end
 c = c .. '.'
 return std.par(std.space_delim, d, c)
end;
}, std.obj) -- nous наследуемся de la norme de l'objet

``
Après cela, vous pouvez créer des conteneurs ainsi:

``
cont {
 nam = 'boîte';
 dsc = [[y {boîte}.]];
 cont = 'Dans la boîte: ';
}: with {
 'pomme', 'poire';
}
``

Lorsque le conteneur est ouvert, vous verrez une description de la boîte et est également
le contenu de la boîte dans la forme d'une chaîne de liens: Dans la boîte: pomme, poire.
dsc des objets de la pomme et de la poire seront également présentés, si elles sont posées.

Si vous souhaitez cacher dsc objets lors de l'ouverture du conteneur, laissant
seuls les noms des objets, vous pouvez effectuer la modification suivante:

``
-- remplacer la fonction d'affichage de n'importe quel objet
-- si l'objet à l'intérieur du conteneur, de ne pas l'appeler dsc
std.obj.display = function(self)
 local w = self:where() -- où l'objet?
 if not std.is_obj(w, 'cont') then -- si ce n'est dans le conteneur
 local d = std.call(self, 'dsc')
 return d
end
end
``

Malheureusement, la description détaillée des classes au-delà de ce
guide, ces choses seront décrits dans un autre guide pour
les développeurs de modules. Et tandis que, pour votre premier match, vous n'avez pas
écrire ses propres classes d'objets.

## Conseils

### Les sauts de fichiers

Lorsque votre jeu est grand, son code entier dans 'main3.lua' -- une mauvaise idée.

Pour séparer le texte des jeux sur les fichiers vous pouvez utiliser
'include'. Vous devez utiliser le 'include' dans un contexte mondial
de sorte que, pendant le temps de chargement 'main3.lua' chargs et tout
les autres fragments de jeu, par exemple.

``
-- main3.lua
include "episode1" -- .lua, vous pouvez omettre
include "npc"
include "start"

room {
 nam = 'main';
....
``

Exactement comment casser le code source sur les fichiers dépend de vous. Je
utilise des fichiers selon les épisodes du jeu (qui est généralement faible
reliés entre eux), mais vous pouvez créer des fichiers qui stockent séparément
de la pièce, des objets, des dialogues, etc. c'Est une question de convenance personnelle.

Ont également la possibilité de charger dynamiquement du jeu (avec
la possibilité de доопределять objets). Pour ce faire, vous
pouvez utiliser la fonction 'gamefile':


``
...
act = function() gamefile ("episode2") end -- .lua, vous pouvez omettre
...
``

> Attention! Si votre jeu défini par une fonction init(), puis à
> подгружаемых parties, elle doit également être présent! Sinon
> le cas, après le chargement d'un fichier, c'est l'actuel fonction init(),
> ce qui n'est généralement pas souhaitable.

'gamefile()' permet également de télécharger un nouveau fichier et oublier la pile
des précédents téléchargements, en lançant ce nouveau fichier comme une
jeu. Pour ce faire, définissez le deuxième paramètre de la fonction en tant que 'true'. Gardez à l'
l'esprit que les modules existants restent et l'expérience de l'opération gamefile
dans les deux cas. 'gamefile()' peut être utilisé que dans
les gestionnaires.


``
 act = function() gamefile ("episode3.lua", true); end;
``

Dans la deuxième variante 'gamefile()' vous pouvez utiliser pour la décoration
multilingues jeux ou jeux-recueils, où en fait à partir de l'interpréteur
exécution de l'auto-jeu.

### À la carte

Le comportement par défaut de l'objet de l'inventaire est que le joueur
doit faire deux clics de la souris. Cela est nécessaire parce que chaque
l'objet de l'inventaire peut être utilisé sur un autre objet ou de la scène
de l'inventaire. Après un second déclic se produit jeu de rythme de jeu. Parfois
ce comportement peut être indésirable. Vous voudrez peut-être
faire un jeu dans lequel la mécanique de jeu est différente de la classique
INSTEAD de jeux. Alors vous pouvez avoir besoin de la carte.

Menu -- c'est un élément d'inventaire qui se déclenche au premier clic. Lors de la
cette carte peut indiquer au moteur que l'action n'est pas un jeu
tact. Ainsi, en utilisant le menu, vous pouvez créer dans la zone de
l'inventaire de jeu de gestion de n'importe quelle complexité. Par exemple, il existe
le module "proxymenu", qui met en œuvre un jeu de gestion dans le style de quêtes
sur la ZX-Spectrum. Dans le jeu de la "maison" leur gestion, ce qui introduit
plusieurs modificateurs de l'action, etc.

Donc, vous pouvez faire un menu dans le domaine de l'inventaire, en définissant les objets
le type de 'menu'. Dans ce cas, le gestionnaire de menu ('act') sera appelée
après un clic de la souris. Si le moteur renvoie false, 
l'état du jeu ne change pas. Par exemple, la mise en œuvre de la poche:

``
menu {
 state = false;
 nam = 'poche';
 disp = function(s)
 if s.state then
 return fmt.u('poche'); -- soulignons actif poche
end
 return 'poche';
end;
 gen = function(s)
 if s.state then
 s:open(); -- afficher tous les objets dans la poche
else
 s:close(); -- cacher tous les objets dans la poche
end
 return s
end;
 act = function(s)
 s.state = not s.state -- modifier l'état
 s:gen(); -- d'ouvrir ou de fermer la poche
end;
}: with {
 obj {
 nam = 'couteau';
 inv = 'ceci Est un couteau';
};
}

function init()
 take 'poche':gen()
end
``

### Le statut d'un joueur

Parfois, le désir affiche un statut. Par exemple,
nombre de points de jeu, l'état d'un héros ou, enfin, le temps
de la journée. INSTEAD ne fournit pas d'autres domaines de la sortie, à l'exception
de la scène et de l'inventaire, par conséquent, la façon la plus simple de sortie du statut de
est la conclusion, dans la zone d'inventaire.

Ci-dessous une implémentation du statut du joueur sous la forme d'un texte qui
apparaît dans l'inventaire, mais ne peut pas être sélectionné, c'est, semble
tout simplement en tant que texte.

``
global {
 life = 10;
 power = 10;
}

stat { -- stat -- l'objet "statut"
 nam = 'statut';
 disp = function(s)
 pn ('la Vie: ', life)
 pn ('Puissance ', power)
end
};

function init()
 take 'statut'
end
``

### walk des gestionnaires onexit et onenter

Vous pouvez faire un 'walk' gestionnaire 'onenter' et
'onexit'. Par exemple, 'path' est implémenté comme un gestionnaire
'onenter', qui transporte le joueur dans une autre pièce.

Il est recommandé de retourner des onexit/onenter false dans le cas où vous
faites walk de ces gestionnaires.

### Le codage du code source du jeu

Si vous ne voulez pas afficher le code source de ses jeux, vous pouvez
encoder le code source à l'aide de l'option de ligne de commande
'-encode':


 sdl-instead -encode <chemin du fichier> [chemin de sortie]

Et utiliser le fichier codé à l'aide des include/gamefile.
Cependant, pour ce faire, vous devez écrire au début de la main3.lua:

``
std.dofile = std.doencfile
``

L'un fichier 'main3.lua', vous devez laisser ouvert. Cette
façon, le schéma est le suivant ('game.lua' -- codé
le fichier):


``
-- $Name: Mon fermé le jeu!$
std.dofile = std.doencfile
include "game"; -- personne n'apprend que de subir!
``

__Il est important!__

> Ne pas utiliser la compilation de jeux avec le 'luac' car 'luac' crée
> платформозависимый code! Cependant, la compilation de jeux peut être
> utilisé pour rechercher les erreurs dans le code.


### Emballage des ressources

Vous pouvez emballer les ressources du jeu (graphismes, de la musique, des thèmes) dans un fichier
ressources '.fid', pour ce faire, placez toutes les ressources dans le répertoire 'data'
et lancez INSTEAD:

 sdl-instead -idf <chemin d'accès au data>

Dans ce cas, dans le répertoire doit créera un fichier
'data.fid'. Placez-le dans le répertoire du jeu. Maintenant les ressources dans le jeu
forme de fichiers individuels, vous pouvez supprimer (bien sûr, en laissant
les fichiers d'origine).

Vous pouvez compresser au format '.fid' ensemble du jeu:

 sdl-instead -idf <chemin d'accès au jeu>

Jeux au format 'idf' vous pouvez exécuter comme les jeux normaux 'instead' (comme
si c'étaient les catalogues) ainsi que de la ligne de commande:

 sdl-instead game.idf

### Basculer entre les joueurs

Vous pouvez créer un jeu avec de multiples personnages et de temps en temps
basculer entre eux (voir 'change_pl()'). Mais vous pouvez également
utiliser cette astuce pour avoir la possibilité de basculer
entre les différents types de matériel.

### L'utilisation de paramètres de gestionnaire

L'exemple de code.

``
obj {
 nam = 'pierre';
 dsc = 'Sur le bord est {pierre}.';
 act = function()
 remove 'pierre';
 p ', J'ai poussé la pierre, il est tombé et s'est envolé vers le bas...';
end
``

Le gestionnaire d'act pourrait sembler plus facile:

``
 act = function(s)
remove(s);
 p ', J'ai poussé la pierre, il est tombé et s'est envolé vers le bas...';
end
``

### Spéciales statuts des gestionnaires

À partir du gestionnaire de revient habituellement à un texte, sous la forme de retour texte
le message". Ou à l'aide de fonctions p()/pr()/pn()/pf(). En outre,
il ya des statuts, qui peuvent être utiles lors de l'élaboration
jeux.

L'état de retour false:

 return false

Ce statut signifie que le gestionnaire n'a pas rempli sa fonction et doit
être ignoré. Normalement le moteur dans ce cas appelera
par défaut.

Vous pouvez aussi retrouver un statut spécial:

 return true, false

Dans ce mode, перерисуется uniquement la zone d'inventaire (mais pas
la scène). Ce statut est utile pour la mise en œuvre de la carte dans
le champ d'inventaire.

Il y a encore un statut spécial: std.nop(). Il peut être
utilisé simplement comme un appel de fonction à la fin du gestionnaire ou
en collaboration avec return.

 return std.nop()
 -- ... ou ...
std.nop()
 -- suivant la fin de la fonction ou de retour

Dans ce cas, le contenu de la scène restera le même que dans le passé
rythme de jeu (même chaîne de réaction reste de l'ancienne). Ce statut de pratique
utilisé conjointement avec le module theme, lorsque vous souhaitez modifier
du jeu à la volée et de redessiner l'image avec de nouveaux paramètres
les thèmes.

### Minuterie

Pour les événements asynchrones, liées au temps réel, à INSTEAD
il ya la possibilité d'utiliser la minuterie. En fait, vous devez
bien réfléchir dans le jeu d'aventure de l'utiliser
la minuterie. D'habitude, le joueur n'est pas trop favorable. Avec
d'autre part, la minuterie, il est possible de l'utiliser pour la gestion
de la musique ou dans les accessoires afin de.

Pour une utilisation de la minuterie, vous devez vous connecter à un module de "timer".

 require "timer"

Minuterie programmable à l'aide de l'objet 'timer'.

- timer:set(ms) -- définir l'intervalle de temps en millisecondes;
- timer:stop() -- arrêter la minuterie.

Lors du déclenchement de la minuterie, un gestionnaire est appelé game.timer. Si
game.timer retourne la valeur false, la scène n'est pas redessiné. Dans
sinon, la valeur de s'affiche comme une réaction.

Vous pouvez faire des locaux pour la pièce de gestionnaires 'timer'. Si
la salle annoncé le gestionnaire d' 'timer', il est appelé à la place
'game.timer'. Si elle retourne false -- appelé game.timer.

Par exemple:

``
game.timer = function(s)
 if time() > 10 then
 return false
end
 snd.play 'gfx/beep.ogg';
 p ("Timer:", time())
end

function init()
 timer set(1000) -- une fois par seconde
end
``

``
room {
 enter = function(s)
timer set(1000);
end;
 timer = function(s)
timer:stop();
 walk 'комната2';
end;
 nam = 'Vérification de la minuterie';
 dsc = [[Attendre.]];
}
``

L'état de la minuterie se trouve dans un fichier de sauvegarde, par conséquent, vous n'avez pas
il faut prendre soin de sa restauration.

Vous pouvez retourner à partir de la minuterie le statut spécial:

 return true, false

Dans ce mode, перерисуется uniquement la zone d'inventaire. Il est possible de
utiliser pour les statuts comme heures.

En outre, dans INSTEAD, il existe la possibilité de suivre les intervalles
temps en millisecondes. Pour ce faire, utilisez la fonction
instead.ticks(). La fonction renvoie le nombre de millisecondes écoulées
après le début du jeu.


### Lecteur de musique

Vous pouvez écrire pour le jeu de votre lecteur de musique en créant la base de l'objet, par exemple:

``
-- joue les pistes dans un ordre aléatoire
require "snd"
obj {
{
 tracks = {"mus/astro2.mod
"mus/aws_chas.xm",
"mus/dmageofd.xm",
"mus/doomsday.s3m"};
};
 nam = 'lecteur';
 life = function(s)
 if not snd.music_playing() then
 local n = s.tracks[rnd(#s.tracks)]
 snd.music(n, 1);
end
end;
}:lifeon();
``

Voici un exemple plus complexe de l'appareil. Changer de piste seulement si
il a pris fin ou a plus de 2 minutes et le joueur a passé des
la salle. Dans chaque piste, vous pouvez spécifier le nombre de проигрываний (0 -
zatsiklenniy piste):

``
require "timer"
global { track_time = 0 };

obj {
 nam = 'player';
 pos = 0;
{
 playlist = { '01 Frozen sun.ogg', 0,
 '02 Thinking.ogg', 0,
 '03 Melancholy.ogg', 0,
 '04 Everyday happiness.ogg', 0,
 '10 Good morning again.ogg', 1,
 '15 [Bonus track] The end (demo cover).ogg', 1
};
};
 tick = function(s)
 if snd.music_playing() and ( track_time < 120 or not player_moved() ) then
return
end
 track_time = 0
 if s.pos == 0 then
 s.pos = 1
else
 s.pos = s.pos + 2
end
 if s.pos > #s.playlist then
 s.pos = 1
end
 snd.music('mus/'..s.playlist[s.pos], s.playlist[s.pos + 1]);
end;
}

game.timer = function(s)
 track_time = track_time + 1
music_player:tick();
end

function init()
timer set(1000)
end
``

### Objets vivants

Si votre héros ont besoin d'un ami, d'une façon peut devenir une méthode
'life' de ce personnage, qui a toujours transfère l'objet à l'emplacement
joueur:

``
obj {
 nam = 'cheval';
 dsc = 'à Côté de moi il y a {cheval}.';
 act = [[Mon cheval.]];
 life = function(s)
 if player_moved() then
place(s);
end
end;
}

function init()
 lifeon 'cheval'; -- la fois оживим cheval
end
``

### Appel du menu

Vous pouvez appeler à partir du menu de jeux INSTEAD avec l'aide de la fonction
'istead.menu()'. Si en tant que paramètre à définir: 'save', 'load' ou
'quit', vous serez convoqué correspondant à la section du menu.

### La création dynamique d'objets

Des objets ordinaires et de la pièce ne permet pas de créer "à la volée". Habituellement, vous
créez-les dans l'espace mondial lua fichier. Cependant, il existe
les jeux dans lesquels le nombre d'objets inconnue à l'avance, ou le nombre de
les objets de grande et ils sont ajoutés au cours du jeu.

Dans INSTEAD il existe un moyen de créer n'importe quel objet à la volée. Pour cela
vous aurez besoin d'écrire _конструктор_ de votre objet, et
utiliser la fonction 'new'.

Le constructeur doit être déclarée.

Donc, vous pouvez utiliser la fonction 'new' et 'supprimer' pour la création de
et la suppression des objets dynamiques. Exemples:

``
declare 'box' (function()
 return obj {
 dsc = [[Ici est {boîte}.]];
 tak = [[J'ai pris la boîte.]];
}
end)

local o = new (box);
take(o);
``

``
declare 'box' (function(dsc)
 return obj {
 dsc = dsc;
 tak = [[J'ai pris la boîte.]];
}
end)
take(new(box, 'Dans un coin de la peine {boîte}'))
``

'new' perçoit le premier argument -- comme задекларированную
la fonction constructeur, et tous les autres paramètres -- comme arguments
le concepteur. Le résultat de l'exécution du constructeur doit être
l'objet.

``
function myconstructor()
 local v = {}
 v.disp = 'l'objet de test'
 v.act = 'Test de réaction'
 return obj(v)
end
``

> Attention! Lors de la création de l'objet, le concepteur doit s'appuyer uniquement
> sur des informations obtenues à partir des paramètres! Le fait que la création d'une
> de l'objet lors de l'amorçage se produit au début, lorsque l'environnement de la paix
> n'est pas encore chargée complètement! En tant que paramètres sont pris en charge
> pour les types simples: les lignes de la table, les nombres, les booléens
> valeurs. Недекларированные les fonctions et les listes ne fonctionnent pas.

Si vous voulez détruire un objet par son nom ou une référence à une variable,
utilisez:

``
 purge(o) -- enlever de toutes les listes
 delete(o) -- la libération de l'objet
``

Dans ce cas, delete c'est la suppression de l'objet de INSTEAD, mais pas l'équivalent
remove() ou purge(). D'habitude, n'a pas de sens de faire un delete. Seulement
si l'objet n'est nécessaire dans le jeu, ou vous allez
recréer l'objet du même nom, a le sens de la libérer de
l'aide de delete().

Plus pratiquement un exemple:

``
-- déclarons constructeur
-- path

declare 'make_path' (function(v) return path(v) end)

-- dynamique de la transition
-- créé un nouvel objet
-- et mettez-le dans ways()

put( new (make_path, { 'passage', 'комната2'}, ways())
``

### L'interdiction d'enregistrer des jeux

Il peut être parfois nécessaire d'interdire à un joueur de faire des économies en
jeu. Par exemple, si il s'agit de scènes où un élément important est
le cas, ou pour des jeux courts, dont la perte doit être fatal
et d'exiger le redémarrage du jeu.

Pour le contrôle de la fonction d'enregistrement, vous utilisez l'attribut 'instead.nosave'.

Par exemple:

instead.nosave = true -- interdire la conservation

Si vous voulez interdire l'enregistrement n'est pas partout, mais dans certaines scènes,
décorez 'instead.nosave' dans la forme de la fonction, ou changez l'état de
l'attribut à la volée -- il pénètre dans le sauve.

``
-- interdire
-- conservation dans les pièces qui contiennent un attribut nosave.
instead.nosave = function()
 return here().nosave
end
``

Il convient de noter que l'interdiction de la conservation ne signifie pas l'interdiction de la
enregistrement automatique. Pour la gestion de автосохранением utilisez
même l'attribut 'instead.noautosave'.

> Vous pouvez stocker explicitement le jeu avec l'aide de l'appel:
> 'instead.autosave([numéro d'emplacement])'; Si l'emplacement n'est pas spécifié, 
> le jeu sera sauvegardée sous un slot 'enregistrement automatique'. Gardez à l'esprit que
> reste en état d' __après__ termine la quantum jeux.

### Définition d'un type d'objet

Dans INSTEAD, il existe deux façons de déterminer le type de l'objet.
Le premier - avec l'aide de la fonction std.is_obj(variable [type]).

Par exemple:
``
a = room {
 nam = 'objet';
};

dprint(std.is_obj(a)) -- affiche true
dprint(std.is_obj('objet')) -- affiche false
dprint(std.is_obj(a, 'room')) -- affiche true
dprint(std.is_obj(a.obj, 'list')) -- affiche true
``

std.is_obj() pratique pour déterminer le type de пременной ou de l'argument
de la fonction.

La deuxième méthode est lié à l'utilisation d'une méthode de type:

``
a = room {
 nam = 'objet';
};

dprint(a:type 'room') -- affiche true
``

## Thèmes pour sdl-instead

Interpréteur graphique prend en charge le mécanisme de ce. _Тема_
représente le répertoire avec le fichier 'theme.ini' à l'intérieur.

Le thème qui est le minimum requis -- c'est un sujet
'default'. Ce thème est toujours chargé en premier. Tous les autres thèmes
héritent de lui et peuvent être partiellement ou totalement la remplacer
les paramètres. Le choix des thèmes est effectuée par l'utilisateur via le menu
les paramètres, mais personne le jeu peut contenir votre propre thème et
par conséquent, une incidence sur votre apparence. Dans ce cas, dans le répertoire
le jeu doit être votre fichier " theme.ini'. Toutefois,
l'utilisateur est libre de désactiver ce mécanisme, ce
l'interpréteur va alerter sur la violation de la création
l'auteur du jeu.

La syntaxe de 'theme.ini' est très simple.

 <option> = <valeur>
ou

 ; le commentaire

Les valeurs peuvent être des types suivants: chaîne de caractères, la couleur, le nombre.

La couleur est définie sous la forme #rgb, où r, g et b composantes de couleur dans
forme hexadécimale. En outre, certaines couleurs de base
reconnus par leurs noms. Par exemple: noir vert jaunâtre, ou
violet.

Les paramètres peuvent être:

- scr.w = largeur de l'espace de jeu en pixels (nombre)
- scr.h = hauteur de l'espace de jeu en pixels (nombre)
- scr.col.bg = couleur d'arrière-plan
- scr.gfx.scalable = [0/1/2] (0 - n'est pas évolutive thème 1 -
 évolutive, 2 - évolutive sans lissage), à partir de
 version 2.2.0 en option [4/5/6]: 4 - n'a pas
 évolutive (avec pas évolutifs des polices), 5 - évolutive,
 avec pas évolutifs des polices, 6 - évolutive sans lissage, avec
 pas de polices évolutives
- scr.gfx.bg = chemin à l'image de l'image de fond (ligne)
- scr.gfx.cursor.x = coordonnée x du centre du curseur (nombre)
- scr.gfx.cursor.y = coordonnée y du centre du curseur (nombre)
- scr.gfx.cursor.normal = chemin d'accès à l'image-le curseur (ligne)
- scr.gfx.cursor.use = chemin à l'image-le curseur du mode d'utilisation (ligne)
- scr.gfx.use = chemin d'accès à l'image-indicateur de mode d'utilisation (ligne)
- scr.gfx.pad = taille de l'indentation à skroll-bars et les bords de la carte (nombre)
- scr.gfx.x, scr.gfx.y scr.gfx.w, scr.gfx.h = coordonnées, la largeur et
 la hauteur de la fenêtre de l'image. La zone dans laquelle se trouve l'image
 de la scène. L'interprétation dépend du mode de localisation (nombre)
- win.gfx.h - synonyme de la scr.gfx.h (pour la compatibilité)
- scr.gfx.icon = le chemin vers le fichier de l'icône du jeu (OS dépendant de l'option peut
 ne pas fonctionner correctement dans certains cas)
- scr.gfx.mode = mode de positionnement (ligne fixe, embedded ou
 float). Définit le mode d'image. embedded -- l'image est
 une partie du contenu de la fenêtre principale, les paramètres de la scr.gfx.x, scr.gfx.y
 scr.gfx.w sont ignorés. float -- image se trouve sur ces
 les coordonnées (scr.gfx.x, scr.gfx.y) et s'adaptent à la taille de
 scr.gfx.w x scr.gfx.h si le dépasse. fixed -- l'image est
 partie de la scène à la fois en mode embedded, mais ne скроллируется avec
 le texte et se trouve directement au-dessus de lui. Disponibles de modification
 le mode float avec des modificateurs 'left/right/center/middle/bottom/top',
 indiquant que c'est de placer l'image dans le domaine de la
 scr.gfx. Par exemple: float-top-left;
- win.scroll.mode = [0/1/2/3], le mode de défilement du volet de la scène. 0 - non
 défilement automatique, 1 - défilement sur un changement dans le texte, 2
 le défilement sur le changement, seulement si le changement n'est pas visible, 3 - toujours
 à la fin;
- win.x, win.y, win.w, win.h = coordonnées, la largeur et la hauteur de la principale
 de la fenêtre. La zone dans laquelle se trouve la description de la scène (nombre)
- win.fnt.name = le chemin vers la police (la chaîne). Ici, la police
 peut contenir une description de tous les styles de caractère, par exemple:
 {sans,sans-b,sans-i,sans-bi}.ttf (les glyphes pour regular,
 bold, italic et bold-italic). Vous pouvez baisser les polices
 et le moteur lui-même génère sur une base normale de police, par exemple:
 {sans, sans-i}.ttf (définis uniquement regular et bold);
- win.align = center/left/right/justify " (l'alignement du texte dans la fenêtre
de la scène);
- win.fnt.size = taille de la police de la fenêtre principale (taille)
- win.fnt.height = interligne comme un nombre à virgule
 la virgule (1.0 par défaut)
- win.gfx.up, win.gfx.down = chemin d'accès aux fichiers-images скорллеров
 vers le haut/vers le bas de la fenêtre principale (ligne)
- win.up.x, win.up.y, win.down.x, win.down.y = coordonnées changeurs
 (coordonnée ou -1)
- win.col.fg = couleur du texte de la fenêtre principale (couleur)
- win.col.link = couleur des liens de la fenêtre principale (couleur)
- win.col.alink = la couleur des liens actifs de la fenêtre principale (couleur)
- win.ways.mode = top/bottom (définir l'emplacement de la liste de raccourcis, de
 défaut top -- dessus de la scène)
- inv.x, inv.y, inv.w, inv.h = coordonnées, la hauteur et la largeur de la zone de
 de l'inventaire. (nombre)
- inv.mode = ligne de mode de l'inventaire (horizontal ou vertical). Dans
le mode horizontal d'inventaire dans une rangée peuvent être quelques
sujets. À la verticale, chaque ligne de l'inventaire contient
un seul sujet. Il y avait des modifications
(-left/right/center). Vous pouvez définir le mode disabled si votre
le jeu n'avez pas besoin de l'inventaire;
- inv.col.fg = couleur du texte de l'inventaire (couleur)
- inv.col.link = couleur des liens d'inventaire (couleur)
- inv.col.alink = la couleur des liens actifs de l'inventaire (couleur)
- inv.fnt.name = chemin d'accès du fichier, la police de l'inventaire (chaîne)
- inv.fnt.size = taille de la police de l'inventaire (taille)
- inv.fnt.height = interligne comme un nombre à virgule
 la virgule (1.0 par défaut)
- inv.gfx.up, inv.gfx.down = chemin d'accès aux fichiers-images скорллеров
 vers le haut/vers le bas pour l'inventaire (chaîne)
- inv.up.x, inv.up.y, inv.down.x, inv.down.y = coordonnées changeurs
 (coordonnée ou -1)
- menu.col.bg = arrière-plan (couleur)
- menu.col.fg = couleur du texte des menus (couleur)
- menu.col.link = couleur des liens du menu (couleur)
- menu.col.alink = la couleur des liens actifs à la carte (couleur)
- menu.col.alpha = transparence du menu 0-255 (nombre)
- menu.col.border = la couleur de la bordure du menu (couleur)
- menu.bw = épaisseur de la bordure du menu (nombre)
- menu.fnt.name = chemin d'accès du fichier, la police du menu (chaîne)
- menu.fnt.size = taille de la police du menu (taille)
- menu.fnt.height = interligne comme un nombre à virgule
 la virgule (1.0 par défaut)
- menu.gfx.button = chemin du fichier de l'image de l'icône de menu (barre)
- menu.button.x, menu.button.y = coordonnées du bouton de menu (nombre)
- snd.click = chemin du fichier de clic (chaîne)
- include = nom du thème (le dernier composant dans le chemin d'accès au répertoire) (la chaîne)

En outre, le titre, le thème peut inclure des commentaires
les balises. Actuellement, il n'existe qu'une seule balise: $Name:
contenant de l'UTF-8 la ligne avec le nom du thème. Par exemple:

``
; $Name:Nouveau thème de$
; modification de l'objet book
include = book -- utiliser le thème du Livre
scr.gfx.h = 500 -- remplacer dans l'un paramètre
``

> L'interpréteur recherche des thèmes dans le répertoire themes. Unix version
> en ce répertoire, regarde aussi dans le répertoire ~/.instead/themes/
> La version Windows -- Documents and Settings/USER/Local
> Settings/Application Data/instead/themes

En outre, la nouvelle version INSTEAD prennent en charge le mécanisme de multiples
autant dans un jeu. En donnant la possibilité au joueur de menus
Au LIEU de choisir l'apparence, prévue par l'auteur
jeux. Pour ce faire, tous les sujets doivent se trouver dans le jeu dans un sous-répertoire
themes. À son tour, chaque thème -- c'est un sous-répertoire dans le répertoire
themes. Dans chaque sous-répertoire doit se trouve votre fichier
theme.ini et les ressources du thème (images, polices, sons). Lors de cette
nécessairement des thèmes du répertoire themes/default - ce thème
chargé par défaut. Format de fichier theme.ini nous venons de
examiné. Cependant, les chemins d'accès aux fichiers de ressources dans le theme.ini sont écrits ne
par rapport à la racine du jeu, et relativement à l'annuaire
les thèmes. Cela signifie que d'habitude, ils ne contiennent que le nom du fichier lui-même
sans le chemin d'accès au répertoire. Par exemple:

``
mygame/
themes/
default/
theme.ini
bg.png
widescreen/
theme.ini
main3.lua
``

theme.ini

``
scr.gfx.bg = bg.png
; ...
``

Dans ce cas, tous les thèmes sont hérités de thèmes
themes/default. Soutenu par le mécanisme d'inclusion. Dans ce cas, INSTEAD
essaie d'abord de trouver éponyme, le thème du jeu, et si tel n'est pas
se trouve, il sera chargé du thème des sujets INSTEAD (si elle
il existe). Ensuite, dans le theme.ini, vous pouvez uniquement modifier les paramètres
qui nécessitent une modification.

## Modules

Une fonctionnalité supplémentaire est souvent réalisé dans INSTEAD sous la forme d'
les modules. Pour utiliser le module, vous devez
écrire:

``
require "nom du module"
``

Ou:

``
loadmod "nom du module"
``

Si le module est fourni avec le jeu.

Une partie des modules inclus INSTEAD, mais il ya ceux qui vous
pouvez télécharger séparément et le mettre dans le répertoire du jeu. Vous pouvez
remplacer n'importe quel module de son, si vous mettez le dans un répertoire avec
jeu sous le même nom de fichier que le standard.

Le module est en fait 'lua' un fichier nommé: 'nom_module.lua'.

Voici les principaux modules standard, avec indication de la
les fonctionnalités qu'ils offrent.

- 'base' — le module de débogage;
- 'click' — module d'intercepter les clics de la souris sur l'image de la scène;
- 'prefs' — module de configuration (le magasin de données de configuration);
- 'snapshots' — module de support de снапшотов (pour les restaurations de situations de jeu);
- 'fmt' — module de prsentation de sortie;
- 'theme' — un thème à la volée;
- 'noinv' - le module d'inventaire;
- 'key" - un module de traitement de l'événement de déclenchement de la touche;
- 'timer' - timer.
- 'sprite' — module pour travailler sur des sprites;
- 'snd' module audio;
- 'nolife' – module de verrouillage méthodes life;

Exemple de chargement d'un module:

``
--$Name: Mon jeu!$
require "fmt"
require "click"
``

> Des modules supplémentaires qui n'entrent pas dans la norme
> la livraison, vous pouvez télécharger à partir de [référentiel
> les modules](https://github.com/instead-hub/stead3-modules). Tout simplement
> télécharger le module et placez-le dans le répertoire
> le jeu. Incluez un tel module à l'aide de loadmod().

### Module keys

Vous pouvez intercepter les événements du clavier à l'aide du module "keys".

D'habitude, l'interception des touches est logique de l'utiliser pour l'organisation de saisie de texte.

Pour définir les touches, vous devez suivre
définissez la fonction keys:filter(press key). Cette fonction doit
retourner true si vous voulez traquer cette
l'événement. Par exemple:

``
require "keys"

function keys:filter(press key)
 return press -- nous attrapons les frappes de touche
end
``

Dans l'exemple, nous rendons simplement l'option press, qui est égal à true
les événements de la touche et false -- relevé. Key est transmis
le nom symbolique de la touche (sous forme de chaîne).

D'habitude, nous devons choisir quelles touches nous voulons intercepter:

``
require "keys"

function keys:filter(press key)
 if key == '0' or key == '1' or key == 'z' then
 return press -- nous attrapons touches z, 1 et 0
end
end
``

Donc, keys:filter vous permet de sélectionner les événements de clavier. Et eux-mêmes
les événements entrent en jeu dans la forme d'un appel à un gestionnaire de 'onkey' pour le soutien
de la pièce ou, s'il n'est pas spécifié dans la salle) pour l'objet 'game'.

Le gestionnaire de onkey agit comme d'habitude, le gestionnaire STEAD3. Il peut
renvoyer false et a alors pensé que la touche n'a pas été traitée
jeu. Soit il peut faire quelque chose d'action.

_Внимание:_ Si le jeu va traiter _все_ les événements de touche, même
ces combinaisons qui sont utilisés par INSTEAD seront traitées
le jeu n'est pas l'interprète. Par exemple, si le jeu va intercepter
appuyez sur la touche "escape" (et ne pas renvoyer false à partir du gestionnaire), la touche
"escape" cesse d'être traitées par l'interpréteur INSTEAD (escape --
appel du menu).

Voici un exemple simple affichage des noms symboliques de touches:

``
require "keys"

function keys:filter(press key)
 return press -- attraper toutes les frappes
end

game.onkey = function(s press, key)
 dprint("pressed: ", key)
 p("Pressé: ", key)
 return false -- donner de traiter les touches de l'interpréteur INSTEAD
end
``

Cet exemple peut être utilisé afin de déterminer la symbolique
le nom spécifique de la touche.

Lors de l'écriture des jeux d'arcade est souvent utile de ne pas recevoir un événement de
le clavier, et d'interroger son (généralement dans un minuteur). Pour ce faire, vous
pouvez utiliser la fonction keys:state(le nom de la touche).

Cette fonction renvoie true pour la touche et false -- pour
essorée, par exemple:

``
require "timer"
require "keys"

game.timer = function(s) -- montrons l'état de la touche curseur de droite
 dprint("state of 'right' key: ", keys:state 'right')
 p("l'État de la touche 'droite':", keys:state 'right')
end

timer set(30)
``

### Le module click

Vous pouvez le suivre dans son jeu de clics sur l'image de la scène ainsi que de
l'arrière-plan. Pour cela, utilisez le module "click". Aussi, vous pouvez
suivre l'état de la souris à l'aide de la fonction:

``
instead.mouse_pos([x, y])
``

Qui renvoie les coordonnées du curseur. Si vous définissez les paramètres (x, y),
vous pouvez déplacer le curseur à la position spécifiée (toutes les coordonnées
sont calculées par rapport à l'angle supérieur gauche de la fenêtre INSTEAD).

``
require "click"
function click:filter press, btn, x, y, px, py)
 dprint(press, btn, x, y, px, py)
 return press and px -- nous attrapons seulement cliquant sur l'image
end
room {
 nam = 'main';
 pic = "box:320x200,red";
 onclick = function(s press, btn, x, y, px, py)
 pn("Vous avez cliqué sur l'image: ", px, ", ", py)
 pn("les coordonnées Absolues: ", x, ", ", y)
 p (Touche": ", btn)
end;
}
``

_Внимание!_ Dans INSTEAD par défaut, le filtre est activé clics de la souris, qui
amortit rapides clics. C'est fait pour exclure l'effet des clats
les touches de la souris. Dans certains cas, un filtre peut être
indésirable. Dans ce cas, utilisez la fonction
instead.mouse\_filter(), qui peut être utilisé pour
la définition de la valeur actuelle du filtre de la souris et de l'installation de la nouvelle (en ce
notamment l'arrêt), par exemple:

``
function start()
 dprint("Mouse filter delay: ", instead.mouse_filter())
 instead.mouse_filter(0) -- arrêté du filtre
end
``

Ou alors:

``
old_filter = instead.mouse_filter(0) -- arrêté
...
instead.mouse_filter(old_filter) -- restauré
``

### Module theme

Le module theme vous permet de modifier les paramètres de thème à la volée.

> Gardez à l'esprit que la modification des paramètres sujets à la volée pour les jeux
> qui ne contiennent pas de votre propre thème -- la source de potentiels
> les problèmes! Le fait que votre jeu dans un tel cas doit être prête
> travailler avec toutes les autorisations et les paramètres de sorte qu'il est extrêmement difficile
> réaliser. Donc, si vous allez changer les paramètres du thème de code
> -- créez votre propre thème et de l'activer dans le jeu!

Dans ce cas, l'enregistrement des modifications du thème dans le fichier d'enregistrement n'est pas
pris en charge. Auteur du jeu lui-même doit restaurer les paramètres de thèmes
la fonction start(), comme cela se fait lors de l'utilisation avec le module de sprites.

Pour modifier les paramètres agissant des thèmes suivants
fonctions:

``
-- réglage de la fenêtre de sortie
theme.win.geom(x, y, w, h)
theme.win.color(fg, link, alink)
theme.win.font(name, size, height)
theme.win.gfx.up(pic, x, y)
theme.win.gfx.down(pic, x, y)

-- configuration du matériel
theme.inv.geom(x, y, w, h)
theme.inv.color(fg, link, alink)
theme.inv.font(name, size, height)
theme.inv.gfx.up(pic, x, y)
theme.inv.gfx.down(pic, x, y)
theme.inv.mode(mode)

-- menu de configuration
theme.menu.bw(w)
theme.menu.color(fg, link, alink)
theme.menu.font(name, size, height)
theme.menu.gfx.button(pic, x, y)

-- configuration de l'image
theme.gfx.cursor(norm, use, x, y)
theme.gfx.mode(mode)
theme.gfx.pad(pad)
theme.gfx.bg(bg)

-- réglage du son
theme.snd.click(name);
``

Il existe la possibilité de lire les paramètres actuels de sujets:

 theme.get 'le nom de la variable thème';

La valeur de retour est toujours sous forme de texte.

 theme.set ('le nom de la variable thèmes', valeur);

Vous pouvez réinitialiser la valeur du paramètre de sujets sur ce qui a été
installé dans un thème du jeu:


``
theme.reset 'le nom de la variable';
theme.win.reset();
``

Il y a une fonctionnalité pour votre thème choisi.

``
theme.name()
``

La fonction renvoie une chaîne -- le nom du répertoire du thème. Si le jeu utilise
propre fichier " theme.ini', la fonction retourne un point. C'est pratique pour
déterminer si le mécanisme de vos jeux:

``
if theme.name() ~= '.' then
 error "Please enable own theme mode in menu!"
end
``

Si le jeu utilise le mécanisme de multiples le nom du thème
commence par un point, par exemple:

``
if theme.name() == '.default' then
 -- nos intégré le thème default
elseif theme.name() == 'default' then
 -- thème standard default dans INSTEAD
end
``
Exemple d'utilisation:

``
theme.gfx.bg "dramatic_bg.png
theme.win.geom (0,0, theme.get 'scr.w', theme.get 'scr.h');
theme.inv.mode 'disabled'
``

Obtenir les dimensions du thème actuel:

``
theme.scr.w() -- largeur
theme.scr.w() -- hauteur
``

### Module de sprite

Le module de sprite permet de travailler avec des graphiques.
Pour activer le module écrire:

 require "sprite"

Les sprites ne peuvent pas entrer dans un fichier de sauvegarde, par conséquent, la restauration
l'état des sprites -- la tâche de l'auteur du jeu. En général, cela
utilise la fonction init() et/ou start(). start() est appelée après
le téléchargement d'un jeu, donc dans cette fonction, vous pouvez utiliser
les variables du jeu.

En fait dans le module sprite mis en œuvre de deux modules: les sprites et les
les pixels. Mais comme ils sont utilisés ensemble, ils sont placés dans un
le module. Commençons avec des sprites:

#### Sprites

Pour créer un sprite, utilisez la méthode de sprite.new, par exemple:

``
 declare 'my_spr' (sprite.new 'gfx/bird.png')
 local heart = sprite.new 'heart.png'
 local blank = sprite.new (320, 200) -- un sprite vide 320x200
``

De créé спрайтового objet les méthodes suivantes:

- :alpha(alpha) - crée un sprite avec une transparence alpha
 (255 - n'est pas transparent). Il est très lente de la fonction;
- :dup() - crée une copie du sprite;
- :scale(xs, ys, [smooth]) -- mise à l'échelle des sprite, pour les reflets
 utilisez l'échelle de -1.0 (lentement! pas pour le temps réel). Crée
 le nouveau sprite.
- :rotate(angle, [smooth]) -- la rotation du sprite à l'angle de l'
 degrés (lentement! pas pour le temps réel). Crée un nouveau sprite.
- :size() -- Retourne la largeur et la hauteur du sprite en pixels.
- :draw(fx, fy, fw, fh, dst\_spr, x, y, [alpha]) -- Dessin
 le domaine de la src du sprite dans le domaine de la dst sprite (tâche alpha fortement
 ralentit l'exécution de la fonction).
- :draw(dst_spr, x, y, [alpha]) -- le Dessin du sprite, attache
 l'option; (tâche alpha ralentit l'exécution de la fonction).
- copy(fx, fy, fw, fh, dst\_spr, x, y) -- Copie
 rectangle fw-sur-fh de sprite dans le sprite
 dst\_spr à l'aide de coordonnées [x,y] (dessin - substitution). Il existe
 une courte présentation (comme :draw).
- :compose(fx, fy, fw, fh, dst\_spr, x, y) -- Dessin - avec
 la transparence des deux sprites). Il ya une courte présentation
 (comme :draw).
- :fill(x, y, w, h, [col]) -- le Remplissage de la couleur du sprite.
- :fill([col]) -- le Remplissage de la couleur du sprite.
- :pixel(x, y, col, [alpha]) -- le Remplissage des pixels du sprite.
- :pixel(x, y) -- la Prise d'un pixel sprite (renvoie les quatre
 composant de la couleur).
- :colorkey(color) -- Définit un sprite couleur, qui agit
 le rôle de l'arrière-plan transparent. Dans ce cas, lors de la prochaine exécution d'une opération
 copy, de en question, le sprite seront copiés les
 les pixels dont la couleur ne correspond pas avec la couleur d'arrière-plan transparent.

Comme les "couleurs" de méthodes reçoivent une ligne du type: 'green', 'red',
'yellow' ou '#333333', '#2d80ff'...

Exemple:

``
 local spr = sprite.new(320, 200)
 spr:fill 'blue'
 local spr2 = sprite.new 'fish.png'
 spr2:draw(spr, 0, 0)
``

En outre, il existe la possibilité de travailler avec des polices. La police est créée
avec l'aide d'un sprite.fnt(), par exemple:

 local font = sprite.fnt('sans.ttf', 32)

De l'objet créé défini les méthodes suivantes:

- :height() -- la hauteur de la police en pixels;
- :text(text, col, [style]) -- la création d'un sprite à partir d'un texte, col - ici
 et plus loin - la couleur dans le format texte (au format '#rrggbb' ou
 'le texte le nom de la couleur').
- :size(text) -- calcule la taille, qui occupera le texte
 sprite, sans la création de sprite.

Vous peut également être utile à la fonction:

sprite.font_scaled_size(size)

Elle retourne la taille de la police compte tenu de la mise à l'échelle, qui
expose le joueur dans les paramètres INSTEAD. Si vous êtes dans son jeu voulez
prendre en compte cette configuration, utilisez cette fonction pour déterminer,
la taille de la police.

Exemple:
``
 local f = sprite.fnt('sans.ttf', 32)
 local spr = sprite.new('box:320x200,black')
 f:text("BONJOUR!", 'white'):draw(spr, 0, 0)
``

Maintenant, considérez les options du module de l'application de sprite.

#### La fonction pic

La fonction de pic peut retourner le sprite. Vous pouvez vous former à chaque fois
nouveau sprite (ce qui ne sera pas très efficace), ou vous pouvez le retourner
à l'avance dédié sprite. Si un sprite est modifiée, alors
ces modifications seront répercutées dans l'image du jeu. Ainsi, changer de sprite
par la minuterie, vous pouvez faire de l'animation:

``
require "sprite"
require "timer"
local spr = sprite.new(320, 200)

function game:timer()
 local col = { 'red', 'green', 'blue'}
 col = col[rnd(3)]
spr:fill(col)
 return false est Important! Ainsi, la scène ne sera pas modifiée
end

game.pic = function() return spr end -- fonction: comme
-- le sprite est un objet (pas de chaîne)

function start()
timer set(30)
end

room {
 nam = 'main';
 decor = [[l'HYPNOSE!]];
}
``

#### Le rendu en arrière-plan

La fonction sprite.scr() renvoie un sprite de fond. Vous pouvez effectuer
le rendu dans le sprite dans n'importe quel gestionnaire, par exemple, dans un minuteur. Ce
le plus la réalisation de changements de fond à la volée, sans l'utilisation d'un module theme.
Par exemple:

``
--$Author: Andrew Lobanov

require 'sprite'
require 'theme'
require 'timer'

declare {
 x = 0,
 y = 0,
 dx = 10,
 dy = 10,
}

const {
 w = theme.scr.w(),
 h = theme.scr.h(),
}

instead.fading = false

local bg, red, green

function init()
 theme.set('scr.col.bg', '#000000')
 theme.set('win.col.fg', '#aaaaaa')
 theme.set('win.col.link', '#ffaa00')
 theme.set('win.col.alink', '#ffffff')

 bg = sprite.new(w, h)
bg:fill('black')
 red = sprite.new(w, h)
red:fill('#ff0000')
 red = red:alpha(128)
 green = sprite.new(w, h)
green:fill('#00ff00')
 green = green:alpha(64)
bg:copy(sprite.scr())
timer set(25)
end

function game:timer()
bg:copy(sprite.scr())
 red:draw(sprite.scr(), x, 0, 128)
 green:draw(sprite.scr(), 0, y, 64)
 x = x + dx
 if x >= w ou x == 0 then
 dx = -dx
end
 y = y + dy
 if y >= h or y == 0 then
 dy = -dy
end
 return false est Important!
end

room {
 nam = 'main',
 disp = 'Test. Test? Test!',
 decor = 'Lorem ipsum';
}

``

_Внимание!_ L'interpréteur INSTEAD dans le mode d'utilisation de l'objet sur le
l'objet se traduit dans le mode "pause". Cela signifie qu'en ce moment
lorsque vous sélectionnez un objet de l'inventaire (le curseur se change en forme de roue dentée)
l'événement timer cessent d'être traités jusqu'à ce que le joueur n'est pas
applique le sujet. Ceci est fait afin de ne pas briser le rythme de
jeux. Si votre création un tel comportement est
un obstacle (par exemple, vous n'aimez pas le fait que l'animation d'arrière-plan
s'arrête), vous pouvez le modifier à l'aide de l'appel:

instead.wait_use(false)

Comme d'habitude, placez un appel à init() ou start() de la fonction.

#### Substitution

Vous pouvez créer votre système de l'objet de la substitution, et de former
les graphiques dans la sortie du jeu à l'aide de l'img, par exemple:

``
require "sprite"
require "timer"
require "fmt"

obj {
 nam = '$spr';
{
 [carré] = sprite.new 'boîte:32 x 32,red';
};
 act = function(s, w)
 return fmt.img(s[w])
end
}

room {
 nam = 'main';
 decor = [[Maintenant, nous allons insérer un sprite: {$spr|place}.]];
}

``

#### direct mode

Dans INSTEAD, il existe un mode d'accès direct aux graphiques. Dans le sujet il
défini par le paramètre:

 scr.gfx.mode = direct

Cette option peut être pré-réglée à thème.ini, ou profiter de la
le module theme. Ou (mieux), une fonction spéciale:

sprite.direct(true)

Si la mode a réussi à activer -- la fonction retourne true. sprite.direct()
sans l'option -- retourne le mode (true -- si direct
est activé.)

Dans ce mode, le jeu dispose d'un accès direct à travers la fenêtre, et peut effectuer des
rendu dans la procédure de la minuterie. L'écran présenté à la spéciale du sprite:

sprite.scr()

Par exemple:


``
require "sprite"
require "timer"
require "theme"

sprite.direct(true)

local stars = {}
local w, h
local colors = {
"red",
"green",
"blue",
"white",
"yellow",
"cyan",
"gray",
"#002233",
}
function game:timer()
 local scr = sprite.scr()
 scr:fill 'black'
 for i = 1, #stars do
 local s = stars[i]
 rcs:pixel(s).x, s.y, colors[s.dy])
 s.y = s.y + s.dy
 if s.y >= h then
 s.y = 0
 s.x = rnd(w) - 1
 s.dy = rnd(8)
end
end
end

function start()
 w, h = theme.scr.w(), theme.scr.h()

 w = std.tonum(w)
 h = std.tonum(h)

 for i = 1, 100 do
 table.insert(stars, { x = rnd(w) - 1, y = rnd(h) - 1, dy = rnd(8) })
end
timer set(30)
end
``

Un autre exemple:

``
require "timer"
require "sprite"
require "theme"

local spr = sprite

declare {
 fnt = false, ball = false, ballw = 0,
 ballh = 0, bg = false, line = false,
 G = false, by = false, bv = false,
 bx = false, t1 = false,
}

function init()
 fnt = spr.fnt(theme.get 'win.fnt.name', 32);
 ball = fnt:text("INSTEAD 3.0", 'white', 1);
 ballw, ballh = ball:size();
 bg = spr.new 'box:640x480,black';
 line = spr.new 'box:320x8,lightblue';
spr.direct(true)
end

function start()
timer set(20)
 G = 9.81
 by = -ballh
 bv = 0
 bx = 320
 t1 = instead.ticks()
end

function phys()
 local t = timer:get() / 1000;
 bv = bv + G * t;
 by = by + bv * t;
 if by > 400 then
 bv = - bv
end
end

function game:timer(s)
 local i
 for i = 1, 10 do
phys()
end
 if instead.ticks() - t1 >= 20 then
 bg:copy(spr.scr(), 0, 0);
 ball:draw(spr.scr(), (640 - ballw) / 2, by - ballh/2);
 line:draw(spr.scr(), 320/2, 400 + ballh / 2);
 t1 = instead.ticks()
end
end

``

_Внимание!_ direct mode peut être utilisé pour créer des
des jeux d'arcade. Dans certains cas, vous pouvez masquer le pointeur
de la souris. Par exemple, quand le jeu est contrôlé uniquement avec le clavier.

Pour ce faire, utilisez la fonction instead.mouse\_show()

``
instead.mouse_show(false)
``

Le menu de l'interpréteur INSTEAD le pointeur de la souris sera toujours
visible.

#### Utilisation de sprite avec le module theme

Dans la fonction start et les gestionnaires, vous pouvez modifier les paramètres de thème, 
notamment, en utilisant des graphiques sprites, par exemple:

``
require "sprite"
require "theme"

function start() -- remplacer l'arrière-plan sur le sprite
 local spr = sprite.new(800, 600)
 spr:fill 'blue'
 spr:fill (100, 100, 32, 60, 'red')
 theme.set('scr.gfx.bg', spr)
end

``
En utilisant cette technique, vous pouvez appliquer une image de fond
les statuts, les contrôles ou tout simplement de changer le substrat.


#### Pixels

Le module prend en charge les sprites aussi à travailler avec des pixels graphiques. Vous
pouvez créer des objets -- ensembles de pixels, modifier et
dessiner des images dans des sprites.

La création de pixels est fonction de pixels.new().

Exemples:

 local p1 = pixels.new(320, 200) -- créé 320x200 pixels
 local p2 = pixels.new 'gfx/apple.png' -- créé des pixels à partir de
 -- image
 local p3 = pixels.new(320, 200, 2) -- créé 320x200 pixels,
 -- qui est le rendu dans le sprite -- seront смасштабированы à
 -- 640x400

L'objet de pixels possède les méthodes suivantes:
> lors de la description des symboles utilisés: r, g, b, a --
> les composants d'un pixel: rouge, vert, bleu, et la transparence. Tous les
> les valeurs de 0 à 255). les coordonnées x, y du coin supérieur gauche, w, h
> -- la largeur et la hauteur de la zone.

- :size() -- retourner la taille et l'échelle (comme les 3 valeurs);
- :clear(r, g, b, [a]) -- nettoyage rapide de pixels;
- :fill(r, g, b, [a]) -- remplissage (avec transparence);
- :fill(x, y, w, h, r, g, b, [a]) -- le remplissage d'une zone (avec transparence);
- :val(x, y, r, g, b, a) - définir la valeur d'un pixel
- :val(x, y) -- obtenir les composantes r, g, b, a
- :pixel(x, y, r, g, b, a) -- dessiner un pixel (en tenant compte de
 la transparence de l'existant pixel);
- :line(x1, y1, x2, y2, r, g, b, a) -- la ligne;
- :lineAA(x1, y1, x2, y2, r, g, b, a) -- la ligne AA;
- :circle(x, y, radius, r, g, b, a) -- la circonférence;
- :circleAA(x, y, radius, r, g, b, a) -- le cercle avec AA;
- :poly({x1, y1, x2, y2, ...}, r, g, b, a) -- le polygone;
- :polyAA({x1, y1, x2, y2, ...}, r, g, b, a) -- un polygone avec AA;
- :blend(x1, y1, w1, h1, pixels2, x, y) -- dessiner une zone de pixels
 un autre objet de pixels, la forme complète;
- :blend(pixels2, x, y) -- un court formulaire;
- :fill\_circle(x, y, radius, r, g, b, a) -- baignée de cercle;
- :fill\_triangle(x1, y1, x2, y2, x3, y3, r, g, b, a) -- baignée de
le triangle;
- :fill\_poly({x1, y1, x2, y2, ...}, r, g, b, a) -- baignée de polygone;
- copy(...) -- comme mélange, mais pas à dessiner et à le copier (rapidement);
- :scale(xscale, yscale, [smooth]) -- mise à l'échelle dans un nouvel objet
pixels;
- :rotate(angle, [smooth]) -- la rotation dans un nouvel objet pixels;
- :draw_spr(...) -- comme draw, mais dans le sprite, pas de pixels;
- :copy_spr(...) -- copy, mais dans le sprite, pas de pixels;
- :compose_spr(...) -- la même chose, mais dans un mode de composition;
- :dup() -- créer une copie de pixels;
- :sprite() -- créer un sprite de pixels.

Aussi, il ya la possibilité de travailler avec des polices:

- pixels.fnt(fnt(la police.ttf, taille) - créer des polices;

Dans ce cas, de l'objet créé le "police" il existe une méthode text:

- :text(texte, couleur(comme dans les sprites), le style) -- créer des pixels avec le texte.

Par exemple:

``
 local fnt = pixels.fnt("sans.ttf", 64)
 local t = fnt:text("HELLO, INSTEAD!", 'black')
pxl:copy_spr(sprite.scr())
 pxl2:draw_spr(sprite.scr(), 100, 200);
 t:draw_spr(sprite.scr(), 200, 400)
``

Un autre exemple (l'auteur de cet exemple, André Lobanov):

``
require "sprite"
require "timer"

sprite.direct(true)

declare 'pxl' (false)
declare 't' (0)

function game:timer()
 local x, y, i
 t = t + 1
 for x = 0, 199 do
 for y = 0, 149 do
 i = (x * x + y * y + t)
 pxl:val(x, y, 0, i, i / 2)
end
end
pxl:copy_spr(sprite.scr())
end

function start(load)
 pxl = pixels.new(200, 150, 4)
timer set(20)
end

``

Lors de la procédure de génération à l'aide de pixels pratique d'utiliser des bruits
Perlin. Dans INSTEAD il existe des fonctions:

- instead.noise1(x) - 1D le bruit de Perlin;
- instead.noise2(x, y) - 2D bruit de Perlin;
- instead.noise3(x, y, z) - 3D bruit de Perlin;
- instead.noise4(x, y, z, w) - 4D bruit de Perlin;

Toutes ces fonctions renvoient une valeur dans l'intervalle [-1; 1] et l'entrée
reçoivent les coordonnées de la virgule flottante.

### Le module snd

Nous avons déjà examiné les fonctionnalités de base audio. Module
snd a encore quelques fonctions audio.

Vous pouvez télécharger le son et le garder en mémoire jusqu'à ce qu'il
vous avez besoin d'un.

``
require 'snd'
local wav = snd.new 'bark.ogg'
``
Outre le chargement de fichiers, vous pouvez charger le fichier audio à partir d'un tableau de lua:

``
local wav = {}
for i = 1, 10000 do
 table.insert(wav, rnd() * 2 - 1) -- des valeurs aléatoires de -1 à 1
end

function start()
 local p = snd.new(22050, 1, wave) -- la fréquence, le nombre de canaux et de son
p:play()
end
``
Le son est défini dans нормированном format: [-1 .. 1]

Le son de perdre de la méthode play([chan], [loop]), où chan -- canal
(0 - 7), loop - cycles (0 - infini).

Les autres fonctions du module:

- snd.stop([channel]) – pour arrêter la lecture du canal sélectionné ou
 tous les canaux. Le deuxième paramètre, vous pouvez définir la durée de l'atténuation du son
 dans mme. lors de son приглушении;

- snd.playing([channel]) – apprendre la lecture d'un son sur n'importe quel
 le canal ou sur le canal sélectionné; si vous sélectionnez un canal,
 la fonction retourne хандл проигрываемого au moment de son ou
 nil. Attention! Le son de clic n'est pas prise en compte et prend généralement 0 canal;

- snd.pan chan, l, r) – la tâche de points de vue panoramique. Le canal, le volume de la
 gauche[0-255], le volume de la droite[0-255] des canaux. Vous devez appeler
 avant de procéder à la lecture de son, a eu pour effet;

- snd.vol(vol) – le volume sonore (musique et effets) de 0 à 127.

Une autre possibilité intéressante -- la génération des sons à la volée (jusqu'à ce que
situé dans le pilote statut):

``
require "snd"

function cb(hz, len (data)
 for i = 1, len do
 data[i] = rnd() * 2 - 1
end
end
function start()
snd.music_callback(cb)
end
``

### Module prefs

Ce module vous permet d'enregistrer les paramètres de jeu. En d'autres termes,
les informations enregistrées ne dépend pas de l'état du jeu. Un tel mécanisme
vous pouvez utiliser, par exemple, pour la mise en œuvre d'un système d'avances ou de
le compteur du nombre de passages du jeu.

Intrinsèquement prefs c'est l'objet de toutes les variables qui seront
conservés.

Enregistrer les paramètres:

``
prefs:store()
``

Les paramètres sont enregistrés automatiquement lorsque vous enregistrez le jeu, mais vous pouvez
contrôler ce processus, provoquant des prefs:store().

Détruire le fichier avec les paramètres:
``
prefs:purge()
``
Chargement de la configuration se fait automatiquement lorsque vous démarrez le jeu
(avant d'appeler la fonction start()), mais vous pouvez lancer le processus de téléchargement et
manuellement:

``
prefs:load()
``

Exemple d'utilisation:

``
-- $Name: Test du module prefs$
-- $Version: 0.1$
-- $Author: instead$

-- connectez le module click
require "click"
-- connectez le module prefs
require "prefs"

-- établissons la valeur initiale du compteur
prefs.counter = 0;

-- définit une fonction de suivi du nombre de "clics"
game.onclick = function(s)
 -- augmenter le compteur
 prefs.counter = prefs.counter + 1;
 -- conservons le compteur
prefs:store();
 -- affichons le message
 p("À ce moment, fait ", préf.counter , de" clics");
end;

-- ajoute une image pour produire des clics
game.pic = 'box:320x200,black';

room {
 nam = 'main',
 title = "Bains de clics",
 -- faisons la fixation statique une partie de la description
 -- ajoute la description de la scène
 decor = [[ Cet essai a été écrit spécifiquement
 pour vérifier le fonctionnement du module <<préf>>.
]];
};

``
Veuillez noter que lorsque vous démarrez le jeu à nouveau, le nombre de
effectuées de clics n'est pas remis à zéro!

### Module de snapshots

Le module de snapshots offre la possibilité de récupérer
pré-enregistrés l'état du jeu. À titre d'exemple, vous pouvez
conduire la situation, lorsqu'un joueur effectue dans un jeu d'action, conduisant à une
la perte. Le module permet à l'auteur de jeux d'écrire du code pour que le joueur
revenir à la pré-enregistrée dans l'état de jeu.

Pour la création de снапшота, utilisez la fonction: snapshots:make(). Dans
tant que paramètre peut être défini sur le nom de l'emplacement.

_Внимание!!!_ Sous marine sera créé après la fin du quantum de jeu,
parce que dans ce cas, la garantie de la cohérence
enregistré état du jeu.

Le téléchargement de снапшота est snapshots:restore(). Comme
le paramètre peut être défini sur le nom de la fente.

La suppression de снапшота se fait à l'aide de snapshots:remove(). Il faut
supprimer des instantanés, car ils prennent de la place dans les fichiers
la conservation.

Exemple d'utilisation:

``
require "snapshots"

room {
 nam = 'main';
 title = 'Jeu';
 onenter = function()
 snapshots:make() -- créé un point de restauration
end;
 decor = [[{#red|Rouge} ou {#noir|noir}?]];
}: with {
 obj {
 nam = '#red';
 act = function()
 p [[Vous avez gagné!]]
end;
};
 obj {
 nam = '#black';
 act = function()
 walk 'end'
end;
}
}

room {
 nam = 'fin';
 title = 'Fin';
}: with {
 obj {
 dsc = [[{Rejouer?}]];
 act = function()
 snapshots:restore() -- récupéré
end;
}
}
``

## Méthodes des objets

Tous les objets STEAD3 il existe des techniques qui sont utilisés lors de la
la mise en œuvre de la bibliothèque standard et, généralement, ne sont pas utilisés par l'auteur
le jeu directement. Cependant, il est parfois utile de connaître la composition de ces méthodes, bien que
pour ne pas le nommer vos variables et des méthodes noms ont déjà
les méthodes existantes. Voici la liste des méthodes avec une brève
description.

### L'objet (obj)

- :with({...}) - définition de la liste obj;
- :new(...) - le constructeur;
- :actions(type, [valeur]) - définir/lire le nombre d'événements de l'objet
 d'un type donné;
- :inroom([{}]) - dans quelle chambre (bain) se trouve l'objet;
- :where([{}]) - dans lequel l'objet (les objets) est l'objet;
- :purge() - supprimer l'objet de toutes les listes;
- :remove() - supprimer l'objet de tous les objets/pièces/matériel;
- :close()/:open() - fermer/ouvrir;
- :disable()/:enable() - activer/désactiver;
- :closed() -- renvoie true si fermé;
- disabled() -- renvoie true si éteint;
- :empty() -- renvoie true si vide;
- :save(fp, n) est une fonction de conservation;
- :display () est une fonction d'affichage dans la scène;
- :visible() -- renvoie true si visible;
- :srch(w) -- la recherche de l'objet visible;
- :lookup(w) -- la recherche de n'importe quel objet;
- :for_each(fn, ...) -- un itérateur sur les objets;
- :lifeon()/:lifeoff () - ajouter/supprimer de la liste des vivants;
- :live() -- renvoie true si dans la liste des vivants.

### Bains (room)

Outre les méthodes obj, ajoutés à des méthodes suivantes:

- :from() -- où sont-ils venus dans la pièce;
- :visited() -- a-t-bains visitée auparavant?;
- :visits() -- le nombre de visites (0 -- si ce n'était pas);
- :scene() -- affichage de la scène (pas d'objets);
- :display() -- l'affichage des objets de la scène;

### Les dialogues (dlg)

Outre les méthodes room, ajoutés à des méthodes suivantes:

- push(expression) - passer à la phrase de mémorisation dans la pile;
- reset(phrase) -- la même chose, mais avec une décharge de la pile;
- pop([phrase]) -- retour sur la pile;
- :select([phrase]) -- le choix de soutien de la phrase;
- :ph\_display() -- l'affichage de la phrase;

### Le monde du jeu (l'objet du jeu)

Outre les méthodes obj, ajoutés à des méthodes suivantes:

- :time([v]) -- installer/de prendre le nombre de graduations;
- :lifeon([v])/:lifeoff([v]) - ajouter/supprimer un objet de la liste
 vivants, ou activer/désactiver la liste de monde (si vous n'avez pas
l'argument);
- :live([v]) -- vérifier l'activité vivante;
- :set\_pl(pl) -- basculer d'un joueur;
- :life() -- itération les installations de la vie;
- :step() -- rythme de jeu;
- :lastdisp([v]) -- installer/prendre la dernière sortie;
- :display(state) -- affichage de la sortie;
- :lastreact([v]) -- d'installer/de prendre la dernière réaction;
- :reaction([v]) -- installer/prendre la réaction actuelle;
- events(pre, bg) -- set/prendre de l'événement les installations de la vie;
- :cmd(cmd) -- l'exécution de la commande INSTEAD;

### Le joueur (player)

Outre les méthodes obj, ajoutés à des méthodes suivantes:

- :moved() -- le joueur qui a fait le déplacement dans le courant de tact de jeu;
- :need_scene([v]) -- besoin d'un rendu de la scène dans cette course;
- :inspect(w) -- trouver un objet (visible) de la scène en cours ou à vous-même
plein;
- :have(w) -- la recherche dans l'inventaire;
- :useit(w) -- utiliser un objet;
- :useon(w, ww) -- utiliser un objet sur le sujet;
- :call(m, ...) -- l'appel de la méthode du joueur;
- :action(w) -- l'action sur l'objet (act);
- :inventory() -- retourner l'inventaire (liste par défaut, c'est obj);
- :take(w) -- prendre l'objet;
- :walk/walkin/walkout -- transitions;
- :go(w) -- l'équipe d'aller (vérifie la disponibilité de navigation);

## Postface

C'est tout, ici, la documentation se termine. Mais, peut-être, commence
le plus intéressant -- votre histoire!

J'ai fait la première version INSTEAD en 2009. En ce moment, je n'aurais jamais
n'a pas pensé que mon jouet (et le moteur) survivront autant
les modifications. Maintenant, quand j'écris c'est la postface, à la cour de 2017 et
le texte de l'aventure existe encore. Cependant, leur impact sur la
la culture est encore minime. Et bonne aventure -- encore
très peu.

À mon avis, de текстографических jeux un énorme potentiel. Ils ne sont pas
de telles interactifs, ils ne privent de votre vie en échange éternellement
insatisfait du désir, ne vous forcent les jours s'asseoir pour
le moniteur de l'irritation ou de la malbouffe de la nervosité... Ils peuvent prendre
le meilleur du monde de la littérature et des jeux informatiques. Et ce genre de
la plus grande partie à but non lucratif -- même plus.

L'histoire de la INSTEAD, à mon avis, une bonne preuve. A
publié de nombreux jeux, qui peut certainement appeler
excellente! Leurs auteurs peuvent à la retraite, mais qu'ils ont créées eux
les œuvres déjà vivent leur vie, en se reflétant dans l'esprit des gens,
qui y jouent ou se souviennent. Laissez-le "tirage" de ces jeux n'est pas si grande,
mais ce que j'ai vu entièrement "justifié" la totalité de l'effort sur
le moteur de. Je sais que c'est du temps bien dépensé. J'ai même
de la force, et fait le moteur est encore mieux, libérant STEAD3. J'espère qu'il
aimez et vous.

Donc, si vous avez lu jusqu'ici, je ne peux que vous souhaiter
ajouter votre première histoire. La créativité -- c'est la liberté. :)

Merci et bonne chance. Pierre Obliques, en mars 2017.

