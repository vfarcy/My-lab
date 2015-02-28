###### July 14th, 2014

Comment rediriger un nom de domaine vers un blog statique hébergé sur Github ?
------------------------

Github autorise l'hébergement de pages web statiques et offre la possibilté d'y accéder à partir d'un nom de domaine du type `sousdomaine.domaine.tld`. C'est gratuit (hors le coût du nom de domaine) et pratique.

C'est le moyen que j'utilise pour héberger ce blog : son repository public est chez github : https://github.com/vfarcy/hexoskelet, le code du site est stocké sous la branche `master`, la version statique générée est sous la branche `gh-pages` à laquelle vous accédez via la redirection http://blog.farcy.me.

Il existe de nombreux tutoriaux qui indiquent la marche à suivre, mais ils sont souvent anciens, ou bien compliqués. Je vous résume la méthode que j'ai utilisée avec succès.

Vous avez deux choses à faire. D'abord, vous devez créer un enregistement `CNAME` qui pointe vers votre adresse github (du type votrenom.github.io) à partir du panneau de contrôle de votre registar. Ensuite, vous devez créer un fichier CNAME dans votre repository qui contient l'adresse du type `sousdomaine.domaine.tld`.

Dans la pratique, après avoir créé l'enregistrement `CNAME` (attention, certains registars réclament une adresse du type `sousdomaine.domaine.tld.`, **vous noterez le point final**), vous laisserez suffisamment de temps pour que le DNS se propage. Vous pouvez vérifier la progression de la propagation en utilisant un service comme whatismydns.com. Chez moi, une vingtaine de minutes ont suffi pour obtenir une propagation quasi totale.

Quand cette première étape est passée, il vous reste à créer chez Github un fichier `CNAME` comportant le même sous domaine `sousdomaine.domaine.tld`, ** cette fois sans le point final !**, et à patienter (encore ...) une quinzaine de minutes. Attention, si vos pages sont hébergées dans un repository Github du  type `login.github.io`, alors le fichier `CNAME` doit se trouver sous la branche `master` tandis qu'il doit se trouver sous la branche `gh-pages` si votre repository est créé sous `login.github.com\nomdurepos`.

Pour finir, si vous utilisez le générateur de blog hexo.io, vous pouvez utiliser le plugin `hexo-generator-cname` (https://github.com/leecrossley/hexo-generator-cname) qui va créer automatiquement le fichier `CNAME` au moment du déploiment du site : `hexo deploy`.

###### December 14th, 2014

Comment bien concevoir un programme arduino  ?
------------------------
TBC ...

Attention, il y a un [bug](https://github.com/Dynalon/mdwiki/issues/150) MDwiki qui ne prend pas en compte la nouvelle numérotation alphanumérique des Gist. 

[gimmick:gist](9132644)

Multi-tasking arudino, the right way
------------------------
TBC ...



