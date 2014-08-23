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


Mise en place de l'environnement de travail après avoir vérifié que la voiture radiocommandée, tout juste reçue, fonctionne correctement. La portée est extrêmement limitée (deux mètres ?) mais la voiture réagit bien aux 4 commandes : avant, arrière, gauche et droite. Les commandes de direction sont "tout ou rien", il est impossible de doser la vitesse.

Je prépare l'environnement de travail, et installe quelques programmes indispensables; notamment Github windows et l'IDE arduino, la solution que je retiens pour "piloter" la radio-commande. Sur la plan matériel, je fais place nette sur mon espace de travail afin de minimiser les risques de court-circuit et de mauvaises manipulations avec le matériel (fer à souder, prises électriques, ...). 

J'ouvre ensuite la télécommande et procède au repérage des composants, des boutons et du circuit imprimé.

Je commence par souder deux fils d'alimentation (rouge et noir) et un premier fil jaune (il y en aura 4 au total) qui devrait (à valider plus tard) commander une direction (gauche / droite) ou un mouvement (avant / arrière). La méthode de repérage que j'ai adoptée est la suivante : chaque bouton poussoir dispose de 4 points de soudure sur le circuit imprimé. Chaque bouton est connecté via deux pairs : une pair est toujours à la masse tandis que l'autre est au +VCC (3,3 V) quand le bouton n'est pas actionné. Le fil jaune est soudé à l'une des pattes qui compose la pair reliée +VCC du bouton poussoir (il passe à 0 V quand il est actionné).

La prochaine étape consistera à valider la solution en écrivant un petit programme arduino qui commande l'état piloté par le fil jaune, ce qui nécessitera auparavant un peu de câblage et de soudure.

###### Le 23 août 2014
Validation de la faisabilité (en cours de rédaction)
------------------------

Le prototype matériel est câblé et fonctionne. L'exécution d'un programme Arduino très simple a permis de tester avec succès les 4 commandes : marche avant, marche arrière, à gauche et à droite. Voyons plus en détail le montage électronique et le programme Arduino.

### Le montage ###


Faire une brève présentation de l'Arduino.

<a href="http://www.amazon.fr/gp/product/B00CF2REXC/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00CF2REXC&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00CF2REXC&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00CF2REXC" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


Faire une brève présentation du kit RC.

<a href="http://www.amazon.fr/gp/product/B00FFRXZIW/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00FFRXZIW&linkCode=as2&tag=farcy.me-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00FFRXZIW&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=farcy.me-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=farcy.me-21&l=as2&o=8&a=B00FFRXZIW" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


Faire une brève présentation du composant ULN 2803.

<a href="http://www.amazon.fr/gp/product/B00JWHW0KU/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00JWHW0KU&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00JWHW0KU&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00JWHW0KU" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


Voici une photo de l'ensemble sur laquelle on voit les deux éléments principaux : l'Arduino et la télécommande dépouillée.

<Insérer une photo>

Le câblage de l'ensemble est présenté sur ce Fritzing :

### Le programme ###


