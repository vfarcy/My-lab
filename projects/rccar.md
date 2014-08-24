###### August 15th, 2014

Formula Zero : Building a programmable RC Car
------------------------

![](http://upload.wikimedia.org/wikipedia/en/thumb/1/1c/Car_collection.jpg/300px-Car_collection.jpg "Not yet done !")

The purpose of the project is to modify the remote control of a cheap RC car so it can be controlled by software. 

This enhanced remote controller will be able to interpret a very simple language, as follows :  

- Directions : FF = Forward, FR = Forward & Right, BB = Backward, etc.
- Stop : SS = Stop
- Duration in milliseconds
- Speed : ranges from 0 to 255

For example a very simple program to drive forward at full speed (255) for 100 ms will look like that : `FF, 255, 100`. Complex motions will be possible by sending longer programs. As a cherry on the cake, the remote controller will be accessible via a browser so it can be possible to (blindly !) monitor the car through an internet connection (a next step could be to  stream a video captured by a camera).


###### Le 21 août 2014
Préparatifs
------------------------

![](..\assets\IMG_20140821_203122.jpg "Le fil rouge sur le bouton rouge !")


Mise en place de l'environnement de travail après avoir vérifié que la voiture radiocommandée, tout juste reçue, fonctionne correctement. La portée est plutôt limitée (deux mètres ?) mais la voiture réagit bien aux 4 commandes : avant, arrière, gauche et droite. Les commandes de direction sont "tout ou rien", il est impossible de doser la vitesse.

Je prépare l'environnement de travail, et installe quelques programmes indispensables; notamment Github windows et l'IDE arduino, la solution que je retiens pour "piloter" la radio-commande. Sur la plan matériel, je fais place nette sur l'espace de travail afin de minimiser les risques de court-circuit et de mauvaises manipulations avec le matériel (fer à souder, prises électriques, ...). 

J'ouvre ensuite la télécommande et procède au repérage des composants, des boutons et du circuit imprimé.

Je commence par souder deux fils d'alimentation (rouge et noir) et un premier fil jaune (il y en aura 4 au total) qui devrait (à valider plus tard) commander une direction (gauche / droite) ou un mouvement (avant / arrière). La méthode de repérage que j'ai adoptée est la suivante : chaque bouton poussoir dispose de 4 points de soudure sur le circuit imprimé. Chaque bouton est connecté via deux pairs : une pair est toujours à la masse tandis que l'autre est au +VCC (3,3 V) quand le bouton n'est pas actionné. Le fil jaune est soudé à l'une des pattes qui compose la pair reliée +VCC du bouton poussoir (il passe à 0 V quand il est actionné).

La prochaine étape consistera à valider la solution en écrivant un petit programme arduino qui commande l'état piloté par le fil jaune, ce qui nécessitera auparavant un peu de câblage et de soudure.

###### Le 23 août 2014
Validation de la faisabilité (en cours de rédaction)
------------------------

![](..\assets\IMG_20140823_102706.jpg "Vue d'ensemble")

Le prototype matériel est câblé et fonctionne. L'exécution de programmes Arduino très simples a permis de tester avec succès l'arrêt, la marche avant, la marche arrière et la gauche / droite. Nous allons voir plus en détail le montage électronique et les programmes de test.

### Le montage ###

Le montage s'articule autour de trois composants principaux : le kit RC, l'Arduino et un circuit intégré (l'ULN 2803 "Darlington driver") qui relie l'Arduino à l'émetteur radio. 

*Le kit RC.*

Il s'agit d'un kit vendu dans un emballage de type canette de bière qui regroupe une voiture à l'échelle 1/54 (elle mesure quelques centimètres) et un émetteur radio. On peut l'acheter pour une dizaine d'euros.


<a href="http://www.amazon.fr/gp/product/B00FFRXZIW/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00FFRXZIW&linkCode=as2&tag=farcy.me-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00FFRXZIW&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=farcy.me-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=farcy.me-21&l=as2&o=8&a=B00FFRXZIW" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

L'émetteur radio est alimenté en standard sous 3 V par deux piles bâton de 1,5V chacune. Il dispose de quatre boutons poussoirs : deux pour la direction et deux pour le sens de la marche (avant / arrière). **Le principe du montage est que ces quatre boutons poussoirs seront dorénavant pilotés par l'Arduino**. Pour que cela soit possible, nous devons commencer par un peu de bricolage : ouvrir l'émetteur, couper les deux fils alimentation vers les deux piles de 1,5V et ponter l'alimentation vers le +VCC de l'Arduino (en 3,3 V, mais cela ne semble pas poser de problème à l'émetteur !). Dans une première version du montage, j'avais omis d'alimenter l'émetteur à partir de l'Arduino ce qui perturbait le bon fonctionnement de l'ensemble.


*L'Arduino.*

<a href="http://www.amazon.fr/gp/product/B00CF2REXC/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00CF2REXC&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00CF2REXC&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00CF2REXC" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

Nous utilisons quatre sorties de l'Arduino pour piloter l'émetteur (pas directement mais "à travers un composant ULN 2803 dont nous parlerons plus-tard). Deux sorties, celles correspondant au sens de la marche, avant ou arrière, sont en mode PWN. Cela permet de réguler la vitesse de déplacement. Les deux autres sorties, direction à gauche et direction à droite, sont quant à elle "tout ou rien" : le braquet est soit maximal, auquel cas on tourne "vite à fond", soit nul, auquel cas on remet les roues "droites".  


*Le circuit intégé "ULN 2803", aussi appelé "Darlington driver"*

Ce circuit à 18 pattes agit comme un "tampon" entre l'Arduino et la radio émettrice. Dix pattes sont utilisées :
- en entrée, 4 pattes sont connectées aux 4 broches de l'Arduino (avant, arrière, droite et gauche)
- en sortie, 4 pattes sont connectées aux 4 boutons poussoirs de l'émetteur pour commander les déplacements
- la masse
- VCC, (+3,3 V)   

Le principe de ce circuit est simple :
- Le 0 V doit être relié à la masse
- L'application d'une tension de 5 V sur une entrée commute la sortie correspondante à la masse
- L'application de 0 V sur une entrée déconnecte la sortie correspondante.

Le rôle de ce circuit est double : il transforme en 3 V (tension originale de l'émetteur) le 5 V des broches de l'Arduino et actionne jusqu'à 8 "interrupteurs" (4 suffisent dans notre application) capables de piloter une charge pouvant aller jusqu'à 40V ou 500mA. Pour notre montage, quatre des huit "interrupteurs" actionnent chacun un bouton poussoir de l'émetteur.
 
<a href="http://www.amazon.fr/gp/product/B00JWHW0KU/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00JWHW0KU&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00JWHW0KU&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00JWHW0KU" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

*Un aperçu de l'ensemble*

J'ai modélisé avec Fritzing le schéma correspondant au montage. Voici ce que cela donne :

![](..\assets\FormulaOne_bb.png "L'Arduino, l'UN2803 et les 4 boutons poussoirs ")

![](..\assets\FormulaOne_schéma.png "Implantation Formula One ")


### Les programmes ###

J'ai créé trois programmes pour valider le montage :

- Mise à zéro (vitesse nulle, roues dans l'axe),
- Marche avant, marche arrière,
- Battement alterné gauche / droite de la direction

Les sources sont disponibles sur [Github](https://github.com/vfarcy/InterRCCar.git) où il est possible de les [télécharger](https://github.com/vfarcy/InterRCCar/archive/master.zip). Je reprends ici deux programmes que je vais commenter : ForBackward.ino (marche avant) et RightLeft.ino (test de la direction).

.....

### Vidéos explicatives ###

Les deux vidéos suivantes reprennent en image les explications de cet article. N'hésitez pas à me contacter si certains points méritent des éclaircissements.

