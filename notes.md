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

###### August 8th, 2014

La communauté d’entraide de Grosbill 
------------------------

J'ai dernièrement acheté chez Grobill un SSD pour remplacer le disque dur d'un portable qui commençait à donner des signes de faiblesse.

Quelques jours après cet achat, je reçois deux mails de Grosbill : l'un m'invitant à répondre à un questionnaire de satisfaction sur la qualité de l'environnement d'achat (accueil, disponibilité, ..), l'autre m'informant, que suite à mon achat, je suis maintenant inscrit à la communauté d'entraide de Grosbill.

Rien à dire sur l'acte d'achat lui même effectué en magasin. Le service était bon.

Par contre, qu'est ce que la communauté d'entraide à laquelle je suis inscrit **par défaut** suite à mon achat et dont j'ignorais l'existence ?

Le mail explique que cette communauté permet à tout moment aux clients Grosbill d'interroger d'autres utilisateurs, de partager des conseils et astuces et de trouver des réponses à des questions déjà résolues. Un lien permet de rejoindre la communauté, et un autre permet de s'en **désinscrire**.

Monsieur Grosbill, je souscris parfaitement à l'idée de créer une communauté qui permet à vos clients d'échanger entre eux sur les produits qu'ils ont acheté. En revanche, j'aurais souhaité avoir la possibilité de décider de m'y inscrire, plutôt que d'y avoir été inscrit, **par défaut**, par vos soins. A la lecture de votre mail, j'ai plutôt retenu que vous sous-traitiez a vil prix, à vos clients, votre service clientèle après vente, démarche à laquelle, en revanche, je ne souscris pas du tout.

J'ai donc cliqué sur le lien pour me désinscrire, ce qui m'a amené vers une page me confirmant que je ne recevrai alors plus d'alertes au sujet du produit acheté. Un comble, je m’apprêtais à être spammé.

Cette expérience me rappelle l'époque à laquelle (c'est corrigé depuis), encore **par défaut**, Grosbill "proposait" un service d'échange à neuf à l'achat. Il fallait bien veiller à **décocher** l'option lors de la validation du panier d'achat, sous peine d'avoir une surprise lors du passage en caisse. 

###### December 14th, 2014

Comment bien concevoir un programme arduino  ?
------------------------
TBD


