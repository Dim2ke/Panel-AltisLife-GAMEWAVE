Panel Altis Life - GAMEWAVE
========================

Panel de gestion des utilisateurs, des dons et de la quasi totalité des fonctions d'un serveur Altis Life (Arma3)

Réutilisation et modification autorisée, merci de citer la source (<b>BloodMotion</b>) ainsi que le lien du Github :-)

<b>Demo (online guest access)</b> : http://admin.altislife.fr/login
<br>
<b>ID</b> : user
<br>
<b>MDP</b> : userpass
<br>

Installation :
========================

<b>Prérequis :</b>
<ul>
 <li>Activer le <b style="color:#D5001D;">mod rewrite</b> sur apache !</li>
 <li>Autoriser (par défaut normalement) l'<b style="color:#D5001D;">open short tag</b> pour PHP</li>
</ul>

<b style="color:D5001D;">La structure de la BDD est légèrement changée (deux colonnes en plus) afin de traiter les donateurs automatiquements :</b>

```sql
-- Exécuter ces deux requêtes dans la table "players"
ALTER TABLE `players` ADD `duredon` integer(1) default 0 NOT NULL;
ALTER TABLE `players` ADD `timestamp` integer(11) NULL;
```

C'est assez simple (n'hésitez pas à me contacter par mail / GitHub). Il vous suffit dé télécharger la dernière version du Git, de l'uploader dans votre FTP (local sur la machine hébergeant le serveur Arma, si vous n'avez pas défini de connexion distante SQL dans la configuration MySQL).

Par la suite, vous devez récupérer la base de donnée "users.sql" (à la racine du Git) et l'importer dans votre base de donnée "arma3life". Avec PHPmyAdmin ou Navicat (par exemple), vous n'avez qu'à créer une nouvelle table "users" dans la base de donnée "arma3life" et importer le fichier fourni ici même :)

Après l'import, vous devriez avoir à peu près ça dans votre table "users" :
![ScreenShot](http://tuk.fr/s/260715184512.png)

Pour configurer les accès BDD (serveur, utilisateur, password) : vous devez éditer le fichier "<b>bdd.php</b>" en remplacant les champs par vos identifiants de connexion base de donnée respectifs :

```php
$ip = "localhost";
$bdd = "arma3life";
$user = "nom_utilisateur";
$passwd = "mot_de_passe";
try{
    $DB = new PDO('mysql:host='.$ip.';dbname='.$bdd, $user, $passwd);
}
```

En ce qui concerne la liste des utilisateurs connectés (en live), vous devrez mettre l'adresse IP de votre serveur (ex: 37.X.X.X) dans le fichier <b>/ajax/refreshPlayer1.php</b> : 

```php
define( 'SQ_SERVER_ADDR', '37.X.X.X' );
define( 'SQ_SERVER_PORT', 2303 );
define( 'SQ_TIMEOUT',     1 );
define( 'SQ_ENGINE',      SourceQuery :: SOURCE );

```

Première connexion :
========================
Si tout fonctionne bien, vous devriez pouvoir vous connecter au panel, les identifiants par défaut son :
  - URL: celle de votre serveur (ex : http://admin.altislife.fr/)
  - ID : admin
  - MDP: admin

<b>Pensez à les changer en base de donnée !</b> (cryptage SHA-512)

<i>Attention ! Depuis le 28-08-2015, le cryptage est passé de SHA1 à SHA-512. Si vous mettez à jour la plateforme, tous les mots de passe entrés en base de donnée ne seront plus valables et devront être modifiés.</i>

C'est fini ! Pour le reste, allez bidouiller dans les fichiers, n'ayez pas peur, c'est du procédural (pas d'objet ni de classe) et donc pas forcément la manière la plus "propre". Cependant, ce panel est testé et utilisé par les plus gros serveurs (Fantasma, AltisLifefr.com, Renaissance, GAMEWAVE (Altislife.fr) etc ...)

Côté technique :
========================
  - Regex (JS / PHP)
  - Requêtes préparées
  - Vérification des GET
  - htaccess
  - Sessions
  - Boostrap v3
  - Niveau d'administration (visiteur [1], modérateur [2], admin[3])
