# Apprenant 1

## Problème de routage :

En lançant votre application PHP sur un serveur local, on s'aperçoit, en premier lieu, qu'il y a une erreur située à la ligne 12 du fichier `CoreController.php`.

Dans un second temps, on découvre que la page d'accueil n'est pas correctement chargée et qu'une page d'erreur 404 (`Not Found`) la remplace.

En testant les différentes routes configurées dans le fichier `public/index.php`, on s'aperçoit d'ailleurs que c'est le cas pour l'intégralité des routes.

En effet, la variable globale `$match`, censée contenir les informations sur la route courante a pour valeur `false`.

En remontant à sa définition dans le fichier `public/index.php`, on s'aperçoit qu'il n'y a pas de problème particulier avec le code source. Simplement, certains ajustements à appliquer pour adopter de meilleures pratiques (Cf. section "**Remarques et bonnes pratiques**", ci-dessous).

En remontant le code source du fichier racine `public/index.php`, on découvre que la variable super globale `$_SERVER` ne contient pas de valeur pour la clé `BASE_URI`. Aucun chemin de base est donc définit ce qui explique le problème mais pas sa résolution.

Pour corriger le problème, il suffit de créer et compléter un fichier de configuration du serveur `.htaccess` à la racine le dossier `public`. Il va ainsi permettre de définir dynamiquement la variable d'environnement `BASE_URI` représentant la partie de l'URL qui correspond à la racine du projet.

Une fois le fichier correctement configuré, le site complet fonctionne à merveille !

Conservez à l'esprit le cheminement suivi pour retrouver le problème de routage propre à votre solution PHP. Je suis parti d'un indice donné à travers un message d'erreur pour découvrir la source de problème de manière plus précise avant de m'apercevoir qu'il s'agissait d'un soucis de configuration du serveur.

## Remarques et bonnes pratiques :

Voici quelques remarques concernant le code source que j'ai parcouru accompagné de proposition de bonnes pratiques que je invite fortement à appliquer à l'avenir !

### Décharger les fichiers :

* Pour éviter de surcharger vos fichiers, je vous suggère de stocker certains traitements ou tableaux dans des fichiers dédiés.

* Dans le cas de votre solution, cette approche serait la bienvenue pour le mappage des routes à définir dans un fichier `routes.php`, par exemple.

* Même remarque pour les listes des contrôles d'accès (ou Access Control Lists : `$acl`) qui surchargent le constructeur de la classe `CoreController`.

### Remarques en vrac :

* Le fichier `style.css` censé être chargé depuis certaines vues HTML est manquant. Par ailleurs, il est préférable de stocker les fichiers CSS, JavaScript ainsi que les médias dans un répertoire `assets` situé à la racine du dossier `public` afin d'y avoir accès depuis nos vues à l'aide de la variable super globale `$_SERVER['BASE_URI']` qui nous permet de retrouver l'URL pointant vers la racine du projet.

Étant donné que la classe `CoreController` n'a pas vocation à être instanciée et sert uniquement de moule à ses classes fille contrôleurs, il est pertinent de la définir  comme étant une **classe abstraite**.

Par convention, il est préférable de nommer les routes en `skewer-case` et non en `snake_case` ou `camelCase`. Dans votre solution, c'est uniquement le cas pour la route `main-home`.

Enfin, le fichier de configuration du serveur stockant les identifiants de connexion à votre base de données, `app/config.ini`, ne doit pas être partagé sur GitHub. Il doit donc être ajouté au fichier `.gitignore` afin que ses modifications et sa publication soient ignoré.