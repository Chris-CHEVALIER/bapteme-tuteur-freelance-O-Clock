# Apprenant 4

# Fautes d'innatention et de syntaxe :

Une erreur apparaît dès que l'on se rend sur la solution web PHP signifiant que le fichier `App\Controller\ErrorController.php` n'existe pas. Le dispatcher ne peut donc pas charger sa méthode `err404()` lorsque la route n'est pas retrouvée.

Dans la même idée, les fichiers `CoreController.php` et `MainController.php` sont également manquants. Ceux-ci sont appelés à de nombreuses reprises par votre code source mais sans succès.

Même si les contrôleurs `StudentsController` et `TeachersController` sont, eux, bien présents, ils comportent plusieurs erreurs d'innatention et de syntaxe :

1. Pour définir une instance d'une classe `Students` ou `Teachers`, on écrit `$studentsModel = new Students();` ou `$studentsModel = new Teachers();`. Il ne faut donc pas oublier les parenthèses (vides ou non en fonction de la définition du constructeur) et le point virgule obligatoire à la fin de chaque instruction PHP.

2. Dans les méthodes `$students` et `$teachers`, respectivement issues des classes `StudentsController` et `TeachersController`, une variable `studentsModel` et `teachersModel` est définie à la *ligne 19* des deux fichiers. Vous y faites référence, par la suite, avec les noms `$studentsdModel` et `$teachersdModel`. Eh oui, un `d` s'est glissé dans les noms de ces deux variables ! Elles ne sont donc pas reconnues par PHP. Ce genre d'erreur de syntaxe apparaît à nouveau dans le fichier `StudentsController.php` avec un `s` en trop inséré dans le nom du modèle => `Studentss`.

3. Les classes sont censées étendre et ainsi hériter de la classe `CoreController` qui n'existe pas... À cause de cet oubli, il n'est pas possible d'appeler une méthode show() qui n'existe pas puisque censée être définie dans cette classe parente `CoreController`.

4. Les méthodes `students()` et `teachers()` respectivement issues des classes `StudentsController` et `TeachersController` ne sont pas censées s'appeler elles-mêmes de cette manière. Sauf, si l'on souhaite obtenir de la récursivité pour boucler sur ces méthodes, par exemple.

5. La méthode `setName()` est appelée sur un objet de type `Teachers` mais sans succès étant donnée que celle-ci n'est pas définie dans la classe modèle en question.

6. Enfin, plusieurs variables sont également utilisées sans être déclarées au préalable. C'est notamment le cas de `$studentsid`, `$studentsdFromDb` ou encore `$teachersdFromDb`.

Un modèle est censé être une représentation sous la forme d'une classe d'une table de données. Ainsi, dans le modèle `Students`, il manque la propriété `$teacher_id` pour coller à la table `student` correspondante.

Pour améliorer vos productions, je vous invite à vous relire régulièrement et à prendre le temps d'écrire du code propre afin d'éviter les fautes d'innatention dont votre solution fait l'objet à de nombreuses reprises. Par ailleurs, n'hésitez pas à vous reposer sur les outils que propose votre éditeur de texte ou votre IDE qui vous indiquera les erreurs de syntaxe éventuelles en soulignant le texte problématique avec un message d'erreur plus explicite.

### Bonnes pratiques :

Par convention, il est préférable de nommer une classe au singulier.
Par exemple, `Students` devient `Student`.
Personnellement, j'ajoute un `s` à mes variables seulement lorsque je sais qu'elles vont stocker plusieurs éléments. C'est le cas des tableaux, par exemple. Il est alors pertinent de nommer un tableau stockant plusieurs utilisateurs `$users`.

Même remarque concernant le nommage des contrôleurs.
Par exemple, `StudentsController` devient `StudentController`.

Par convention, il est préférable de nommer les routes en `skewer-case` et non en `snake_case` ou `camelCase`. Dans votre solution, c'est uniquement le cas pour la route `main-home`.

Enfin, le fichier de configuration du serveur stockant les identifiants de connexion à votre base de données, `app/config.ini`, ne doit pas être partagé sur GitHub. Il doit donc être ajouté au fichier `.gitignore` afin que ses modifications et sa publication soient ignoré.

### Remarques en vrac : 

En outre, le fichier de configuration du serveur `.htaccess` doit être ajouté à la racine le dossier `public`. pour permettre de définir dynamiquement la variable d'environnement `BASE_URI` représentant la partie de l'URL qui correspond à la racine du projet. Autrement, aucune page n'est accessible en fonction de l'arborescence du serveur.