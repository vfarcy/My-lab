###### August 15th, 2014

Formula Zero : Pilotage d'un jouet radio-commandé
------------------------

![](http://upload.wikimedia.org/wikipedia/en/thumb/1/1c/Car_collection.jpg/300px-Car_collection.jpg "Not yet done !")

Jon Bennett présente sur [son blog](http://www.jbprojects.net/articles/programmable-rc/) un projet dont l'objectif est de piloter la télécommande d'une voiture radiocommandée. 

Les lignes qui suivent présentent une adaptation de ce projet que j'ai avant tout réalisé en vue de découvrir l'Arduino.

###### Le 21 août 2014
Préparatifs
------------------------

![](..\assets\IMG_20140821_203122.jpg "Le fil rouge sur le bouton rouge !")


Je viens de mettre en place mon environnement de travail après avoir vérifié le bon fonctionnement du jouet radio-commandé que j'ai reçu aujourd'hui. La portée est assez limitée (deux mètres ?) mais la voiture réagit bien aux 4 commandes : avant, arrière, gauche et droite. Les commandes de direction sont "tout ou rien", il est impossible de doser la vitesse.

J'ai installé les logiciels que j'utiliserai pour réaliser le projet; le client Github windows et l'IDE arduino. Sur la plan matériel, je fais place nette sur mon espace de travail afin de minimiser les risques de court-circuit et de mauvaises manipulations avec le matériel (fer à souder, prises électriques, ...). 

Assez joué, puisque le jouet fonctionne correctement, il est tant d'en comprendre le fonctionnement, de procéder au repérage des composants, des boutons et du circuit imprimé. La lecture sur blog de Jon Bennett me donne quelques orientations que je confronte aujourd'hui à la réalité.

Je procède à l'ouverture de la télé-commande et commence par souder deux fils d'alimentation (rouge et noir) et un premier fil jaune (il y en aura 4 au total) qui commanderont une direction (gauche / droite) ou un mouvement (avant / arrière). La méthode de repérage que j'ai adoptée est la suivante : chaque bouton poussoir dispose de 4 points de soudure sur le circuit imprimé. Chaque bouton est connecté via deux pairs : une pair est toujours à la masse tandis que l'autre est au +VCC (3,3 V) quand le bouton n'est pas actionné. Le fil jaune est soudé à l'une des pattes qui compose la pair reliée +VCC du bouton poussoir (il passe à 0 V quand il est actionné).

La prochaine étape consistera à valider la solution en écrivant un petit programme arduino qui commande l'état piloté par le fil jaune, ce qui nécessitera auparavant un peu de câblage et de soudure.

###### Le 23 août 2014
Validation de la faisabilité (en cours de rédaction)
------------------------

![](..\assets\IMG_20140823_102706.jpg "Vue d'ensemble")

Le prototype matériel est câblé et fonctionnel. L'exécution de programmes Arduino très simples permet de commander le déplacement de la voiture : marche avant, marche arrière, gauche et droite. Nous allons voir plus en détail le montage électronique à base d'Arduino et le programmes de test.

### Le montage ###

Le montage s'articule autour de trois composants principaux : le kit RC, l'Arduino et un circuit intégré (l'ULN 2803 "Darlington driver") qui relie l'Arduino à l'émetteur radio. 

*Le kit RC.*

Il s'agit d'un kit vendu dans un emballage de type canette de bière qui regroupe une voiture à l'échelle 1/54 (elle mesure quelques centimètres) et un émetteur radio. On peut l'acheter pour une dizaine d'euros.


<a href="http://www.amazon.fr/gp/product/B00FFRXZIW/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00FFRXZIW&linkCode=as2&tag=farcy.me-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00FFRXZIW&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=farcy.me-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=farcy.me-21&l=as2&o=8&a=B00FFRXZIW" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

L'émetteur radio est alimenté en standard sous 3 V par deux piles bâton de 1,5V chacune. Il dispose de quatre boutons poussoirs : deux pour la direction et deux pour le sens de la marche (avant / arrière). **Le principe du montage est que ces quatre boutons poussoirs pilotent l'Arduino**. Pour cela, nous devons commencer par un peu de bricolage : ouvrir l'émetteur, couper les deux fils alimentation vers les deux piles de 1,5V et les relier au +VCC de l'Arduino (en 3,3 V, mais cela ne semble pas poser de problème à l'émetteur !). Dans une première version du montage, j'avais omis d'alimenter l'émetteur à partir de l'Arduino ce qui perturbait le bon fonctionnement de l'ensemble.


*L'Arduino.*

<a href="http://www.amazon.fr/gp/product/B00CF2REXC/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00CF2REXC&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00CF2REXC&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00CF2REXC" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

Nous utilisons quatre sorties d'un Arduino Méga pour piloter l'émetteur. L'Arduino et l'émetteur sont connectés "à travers" un composant ULN 2803 dont nous parlerons plus-tard. Les sorties de l'Arduino, celles qui pilotent la marche avant et la marche arrière sont utilisées en mode PWN qui permet de doser la pusisance de l'accélération.Les deux autres sorties, direction à gauche et direction à droite, sont quant à elle configurées en mode "tout ou rien" : le braquet est soit maximal, auquel cas on tourne "vite à fond", soit nul, auquel cas on remet les roues "droites".  


*Le circuit intégé "ULN 2803", aussi appelé "Darlington driver"*

Ce circuit à 18 pattes agit comme un "tampon" entre l'Arduino et la radio émettrice. Dix pattes sont utilisées sur les 18 que comporte le composant :
- en entrée, 4 pattes sont connectées aux 4 broches de l'Arduino (avant, arrière, droite et gauche)
- en sortie, 4 pattes sont connectées aux 4 boutons poussoirs de l'émetteur pour commander les déplacements
- la masse
- VCC, (+3,3 V)   

Le principe de ce circuit est simple :
- Une patte est reliée à la masse, l'autre au 3,3 V,
- L'application d'une tension de 5 V (en provenance de l'Arduino)commute la sortie associée à la masse, 
- L'application de 0 V (en provenance de l'Arduino) sur une entrée déconnecte la sortie associée.

Le rôle de ce circuit est double : il transforme en 3 V (tension originale de l'émetteur) le 5 V des broches de l'Arduino et actionne jusqu'à 8 "interrupteurs" (4 suffisent dans notre application) capables de piloter une charge pouvant aller jusqu'à 40V ou 500mA. Pour notre montage, quatre des huit "interrupteurs" actionnent chacun un bouton poussoir de l'émetteur.
 
<a href="http://www.amazon.fr/gp/product/B00JWHW0KU/ref=as_li_tl?ie=UTF8&camp=1642&creative=19458&creativeASIN=B00JWHW0KU&linkCode=as2&tag=presqriensurp-21"><img border="0" src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00JWHW0KU&Format=_SL160_&ID=AsinImage&MarketPlace=FR&ServiceVersion=20070822&WS=1&tag=presqriensurp-21" ></a><img src="http://ir-fr.amazon-adsystem.com/e/ir?t=presqriensurp-21&l=as2&o=8&a=B00JWHW0KU" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

*Un aperçu de l'ensemble*

J'ai modélisé avec Fritzing le schéma correspondant au montage. Voici ce que cela donne :

![](..\assets\FormulaOne_bb.png "L'Arduino, l'UN2803 et les 4 boutons poussoirs ")

![](..\assets\FormulaOne_schéma.png "Implantation Formula One ")


### Les programmes ###

Je me suis très largement inspiré du programme initial de Jon Bennett que j'ai adapté pour valider le montage et le bon déplacement de la voiture au moyen d'un langage élémentaire dont un exemple est présenté ci dessous :

   
```c 
C[] = {
          {FORWARD_BIT,15,2000}, /*Write the car's journey here.  */
          {BACKWARD_BIT+LEFT_BIT,15,2000},
          {FORWARD_BIT,15,2000},   
          {BACKWARD_BIT+RIGHT_BIT,15,2000},         
          {BACKWARD_BIT+LEFT_BIT,15,2000},             
        };

```
    
Les instructions de déplacement, stockés dans un tableau `C[]`, sont codées dans des instructions d'un ligne commandant chacune une direction, une vitesse et une durée. Ainsi, le programme précédent, qui contient 4 lignes, commande à la voiture d'avancer à une vitesse de 5 (comprise entre 0 et 255) pendant 3 secondes (3000 ms), de reculer en braquant à gauche à une vitesse de 5 pendant 5 secondes, de reculer en ligne droite à une vitesse de 15 pendant 1,5 secondes et d'avancer en braquant à droite à une vitesse de 20 pendant 1,5 seconde. 

Les sources sont disponibles sur [Github](https://github.com/vfarcy/InterRCCar.git) où il est aussi possible de les [télécharger](https://github.com/vfarcy/InterRCCar/archive/master.zip). 

### Vidéos explicatives ###

Les deux vidéos suivantes reprennent en image les explications de cet article. N'hésitez pas à me contacter si certains points méritent des éclaircissements.

