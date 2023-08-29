# Usine de robots
Qui n'a jamais rêvé de se fabriquer un robot pour exterminer l'humanité faire le café ?

Dans un fichier index.php, qui contiendra tout en haut votre logique PHP, puis le document HTML à proprement parler, vous allez développer un programme capable de créer un nouveau robot à chaque rafraîchissement de la page. Le robot saluera alors l'utilisateur et se présentera. Exemple :

« Salut, humain. Je suis VX-2345. »

Le nom du robot est généré aléatoirement au chargement de la page. Il se constituera toujours de deux lettres majuscules, d'un tiret et de quatre nombres.

## Bonus 1
Après s'être présenté, le robot annoncera la date et l'heure actuelles au format suivant :

« Nous sommes le 30 07 2020, il est 11:59:30. »

## Bonus 2
Après avoir annoncé la date et l'heure, le robot choisira un nombre aléatoire compris entre 1 et 10, et vous dira si c'est un nombre pair ou impair. Exemple :

« J'ai choisi le nombre 3. C'est un nombre impair. »

## Bonus 3
Notre robot découvre le sens de l'humour. Il lui prendra alors l'idée de dire son nom à l'envers. Un robot du nom de VX-2345 dira alors :

« Mon nom à l'envers s'écrit 5432-XV. Ah. Ah. Ah. »

## Bonus 4
Oups ! Il s'avère que notre robot a une chance sur trois de devenir un robot tueur diabolique.

Deux fois sur trois, le robot dira « Vous voulez un café ? »

Une fois sur trois, le robot dira « Extermination ! Extermination ! »

## Bonus 5
Si ce n'est encore pas fait à ce stade, vous allez repenser votre code pour faire de votre robot un objet.

Vous aurez une classe Robot dont vous créerez une instance au chargement de la page. Toutes les caractéristiques et actions de notre robot deviendront alors des propriétés et des méthodes.

<!-- Exemples de corrigées types -->

Bien sûr, voici comment vous pourriez implémenter ce programme en PHP :

```php
<?php
class Robot {
    private $name;

    public function __construct() {
        $this->generateName();
    }

    private function generateName() {
        $letters = range('A', 'Z');
        shuffle($letters);
        $name = implode(array_slice($letters, 0, 2)) . '-' . sprintf('%04d', mt_rand(0, 9999));
        $this->name = $name;
    }

    public function greet() {
        echo "Salut, humain. Je suis { $this->name}.<br>";
        $this->displayDateAndTime();
        $this->checkEvenOdd();
        $this->reverseName();
        $this->senseOfHumor();
    }

    private function displayDateAndTime() {
        echo "Nous sommes le ". date("d m Y") . ", il est " . date("H:i:s") . ".<br>";
    }

    private function checkEvenOdd() {
        $number = mt_rand(1, 10);
        echo "J'ai choisi le nombre $number. C'est un nombre " . ($number % 2 === 0 ? "pair" : "impair") . ".<br>";
    }j

    private function reverseName() {
        $reversedName = strrev($this->name);
        echo "Mon nom à l'envers s'écrit $reversedName. Ah. Ah. Ah.<br>";
    }

    private function senseOfHumor() {
        $randomNumber = mt_rand(1, 3);
        if ($randomNumber === 1) {
            echo "Vous voulez un café ?<br>";
        } else {
            echo "Extermination ! Extermination !<br>";
        }
    }
}

$robot = new Robot();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robot Factory</title>
</head>
<body>
    <?php
    $robot->greet();
    ?>
</body>
</html>
```

Dans cet exemple, j'ai créé la classe `Robot` avec des méthodes pour générer un nom aléatoire, saluer, afficher la date et l'heure, vérifier si un nombre est pair ou impair, afficher le nom à l'envers et montrer un sens de l'humour aléatoire. La page HTML appelle la méthode `greet()` de l'objet `Robot` pour afficher le comportement souhaité à chaque rafraîchissement.Pour créer la page `homepage.phtml` avec un formulaire permettant à l'utilisateur de donner un nom au robot et de saisir un comportement, voici comment vous pourriez le faire :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robot Homepage</title>
</head>
<body>
    <h1>Créer votre propre robot</h1>
    <form method="post" action="process.php">
        <label for="robotName">Nom du robot :</label>
        <input type="text" id="robotName" name="robotName"><br>

        <label for="robotBehavior">Comportement du robot :</label>
        <textarea id="robotBehavior" name="robotBehavior" rows="4" cols="50"></textarea><br>

        <input type="submit" value="Créer le robot">
    </form>
</body>
</html>
```

Assurez-vous de créer un fichier `process.php` dans le même répertoire que `homepage.phtml` pour gérer les données soumises par le formulaire. Voici comment vous pourriez le mettre en place :

```php
<?php
class Robot {
    // ... (les méthodes et propriétés de la classe Robot restent inchangées)
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $robotName = $_POST['robotName'];
    $robotBehavior = $_POST['robotBehavior'];

    $robot = new Robot();
    $robot->setName($robotName);
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robot Created</title>
</head>
<body>
    <?php if (isset($robot)) : ?>
        <h1>Robot créé avec succès !</h1>
        <p>Nom du robot : <?php echo $robot->getName(); ?></p>
        <p>Comportement du robot : <?php echo $robotBehavior; ?></p>
    <?php else : ?>
        <h1>Erreur lors de la création du robot</h1>
    <?php endif; ?>
</body>
</html>
```

Assurez-vous que `process.php` et `homepage.phtml` se trouvent dans le même répertoire et que votre serveur Web est configuré pour exécuter des fichiers PHP et PHTML. Dans ce scénario, le formulaire de `homepage.phtml` soumet les données à `process.php`, où un nouvel objet `Robot` est créé avec le nom saisi et le comportement du robot est affiché sur la page après la soumission.

Bien sûr, voici comment vous pourriez réorganiser la classe `Robot` pour qu'elle prenne en compte les informations saisies dans le formulaire :

```php
<?php
class Robot {
    private $name;
    private $behavior;

    public function __construct($name, $behavior) {
        $this->name = $name;
        $this->behavior = $behavior;
    }

    public function greet() {
        echo "Salut, humain. Je suis {$this->name}.<br>";
        $this->displayDateAndTime();
        $this->checkEvenOdd();
        $this->reverseName();
        $this->senseOfHumor();
    }

    private function displayDateAndTime() {
        echo "Nous sommes le " . date("d m Y") . ", il est " . date("H:i:s") . ".<br>";
    }

    private function checkEvenOdd() {
        $number = mt_rand(1, 10);
        echo "J'ai choisi le nombre $number. C'est un nombre " . ($number % 2 === 0 ? "pair" : "impair") . ".<br>";
    }

    private function reverseName() {
        $reversedName = strrev($this->name);
        echo "Mon nom à l'envers s'écrit $reversedName. Ah. Ah. Ah.<br>";
    }

    private function senseOfHumor() {
        $randomNumber = mt_rand(1, 3);
        if ($randomNumber === 1) {
            echo "Vous voulez un café ?<br>";
        } else {
            echo "Extermination ! Extermination !<br>";
        }
    }

    public function getName() {
        return $this->name;
    }

    public function getBehavior() {
        return $this->behavior;
    }
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $robotName = $_POST['robotName'];
    $robotBehavior = $_POST['robotBehavior'];

    $robot = new Robot($robotName, $robotBehavior);
}
?>
```

Cette version de la classe `Robot` reçoit le nom et le comportement du robot lors de sa création. Dans le fichier `process.php`, les données du formulaire sont utilisées pour instancier un objet `Robot` avec le nom et le comportement saisis. Les méthodes de la classe restent essentiellement les mêmes, mais la construction de l'objet est basée sur les données du formulaire.

Bien sûr, pour générer aléatoirement le nom du robot si l'utilisateur ne fournit pas de nom, vous pouvez ajuster le code comme suit :

```php
<?php
class Robot {
    private $name;
    private $behavior;

    public function __construct($name = null, $behavior) {
        if ($name === null) {
            $this->generateRandomName();
        } else {
            $this->name = $name;
        }
        $this->behavior = $behavior;
    }

    private function generateRandomName() {
        $letters = range('A', 'Z');
        shuffle($letters);
        $name = implode(array_slice($letters, 0, 2)) . '-' . sprintf('%04d', mt_rand(0, 9999));
        $this->name = $name;
    }

    // ... (autres méthodes restent inchangées)
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $robotName = $_POST['robotName'];
    $robotBehavior = $_POST['robotBehavior'];

    $robot = new Robot($robotName, $robotBehavior);
}
?>
```

Avec cette modification, la classe `Robot` accepte un argument optionnel `$name` dans son constructeur. Si l'utilisateur ne fournit pas de nom lors de la création de l'objet, la méthode `generateRandomName()` sera appelée pour générer un nom aléatoire. Si un nom est fourni, il sera utilisé comme nom du robot.


<!-- Fonctions PHP -->
PHP (Hypertext Preprocessor) est un langage de programmation populaire utilisé pour développer des applications web dynamiques. Il dispose d'un grand nombre de fonctions intégrées qui permettent d'effectuer diverses opérations. Voici une liste de certaines fonctions PHP couramment utilisées, avec leurs définitions et des exemples :

1. **Fonctions de base** :

   - **echo** : Affiche une ou plusieurs chaînes de caractères.
     ```php
     echo "Bonjour, PHP!";
     ```

   - **print** : Affiche une chaîne de caractères. (similaire à echo, mais retourne toujours 1)
     ```php
     print("Bonjour, PHP!");
     ```

   - **isset** : Vérifie si une variable est définie et non nulle.
     ```php
     if (isset($nom)) {
         echo "La variable \$nom est définie.";
     }
     ```

   - **empty** : Vérifie si une variable est vide.
     ```php
     if (empty($liste)) {
         echo "La liste est vide.";
     }
     ```

   - **count** : Retourne le nombre d'éléments d'un tableau.
     ```php
     $nombres = [1, 2, 3, 4, 5];
     echo "Nombre d'éléments : " . count($nombres);
     ```

2. **Fonctions de manipulation de chaînes de caractères** :

   - **strlen** : Retourne la longueur d'une chaîne de caractères.
     ```php
     $texte = "Hello, world!";
     echo "Longueur : " . strlen($texte);
     ```

   - **strpos** : Cherche la position d'une sous-chaîne dans une chaîne.
     ```php
     $phrase = "Bonjour à tous!";
     $position = strpos($phrase, "à");
     echo "Position : $position";
     ```

   - **substr** : Retourne une sous-chaîne à partir d'une chaîne.
     ```php
     $texte = "abcdef";
     $partie = substr($texte, 2, 3);
     echo "Sous-chaîne : $partie";
     ```

3. **Fonctions de gestion des tableaux** :

   - **array_push** : Ajoute un ou plusieurs éléments à la fin d'un tableau.
     ```php
     $fruits = ["pomme", "banane"];
     array_push($fruits, "orange");
     ```

   - **array_pop** : Supprime et retourne le dernier élément d'un tableau.
     ```php
     $dernier_fruit = array_pop($fruits);
     ```

   - **array_merge** : Fusionne deux tableaux en un seul.
     ```php
     $tableau1 = ["a", "b"];
     $tableau2 = ["c", "d"];
     $nouveau_tableau = array_merge($tableau1, $tableau2);
     ```

4. **Fonctions mathématiques** :

   - **rand** : Génère un nombre aléatoire.
     ```php
     $nombre_aleatoire = rand(1, 100);
     ```

   - **sqrt** : Calcule la racine carrée d'un nombre.
     ```php
     $racine = sqrt(25);
     ```

   - **round** : Arrondit un nombre décimal.
     ```php
     $nombre_arrondi = round(3.14159, 2);
     ```

5. **Fonctions de manipulation de dates et heures** :

   - **date** : Formate une date/heure selon le format spécifié.
     ```php
     $date_actuelle = date("Y-m-d H:i:s");
     ```

   - **strtotime** : Convertit une date textuelle en timestamp.
     ```php
     $timestamp = strtotime("2023-08-21");
     ```

   - **time** : Retourne le timestamp actuel.
     ```php
     $timestamp_actuel = time();
     ```

Il y a bien sûr de nombreuses autres fonctions disponibles en PHP pour diverses tâches telles que la manipulation de fichiers, les opérations sur les chaînes de caractères, la gestion des bases de données, etc. Cette liste ne couvre qu'une fraction des fonctions disponibles. Vous pouvez consulter la documentation officielle de PHP pour une liste exhaustive et à jour : [Documentation PHP](https://www.php.net/manual/fr/funcref.php).

<!-- Les tableaux -->
Bien sûr, voici une liste de fonctions PHP spécifiques aux tableaux et aux chaînes de caractères, accompagnée d'exemples pour chacune d'entre elles :

### Fonctions pour les tableaux :

1. **array_push** : Ajoute un ou plusieurs éléments à la fin d'un tableau.
   ```php
   $fruits = ["pomme", "banane"];
   array_push($fruits, "orange");
   ```

2. **array_pop** : Supprime et retourne le dernier élément d'un tableau.
   ```php
   $dernier_fruit = array_pop($fruits);
   ```

3. **array_merge** : Fusionne deux tableaux en un seul.
   ```php
   $tableau1 = ["a", "b"];
   $tableau2 = ["c", "d"];
   $nouveau_tableau = array_merge($tableau1, $tableau2);
   ```

4. **array_slice** : Extrait une portion d'un tableau.
   ```php
   $nombres = [1, 2, 3, 4, 5];
   $portion = array_slice($nombres, 2, 2);
   ```

5. **array_search** : Recherche une valeur dans un tableau et renvoie sa clé.
   ```php
   $position = array_search("banane", $fruits);
   ```

6. **array_key_exists** : Vérifie si une clé existe dans un tableau associatif.
   ```php
   $personne = ["nom" => "Dupont", "âge" => 30];
   if (array_key_exists("âge", $personne)) {
       echo "Clé 'âge' existe.";
   }
   ```

### Fonctions pour les chaînes de caractères :

1. **strlen** : Retourne la longueur d'une chaîne de caractères.
   ```php
   $texte = "Hello, world!";
   echo "Longueur : " . strlen($texte);
   ```

2. **strpos** : Cherche la position d'une sous-chaîne dans une chaîne.
   ```php
   $phrase = "Bonjour à tous!";
   $position = strpos($phrase, "à");
   ```

3. **substr** : Retourne une sous-chaîne à partir d'une chaîne.
   ```php
   $texte = "abcdef";
   $partie = substr($texte, 2, 3);
   ```

4. **str_replace** : Remplace toutes les occurrences d'une sous-chaîne par une autre.
   ```php
   $texte = "Bonjour à tous!";
   $nouveau_texte = str_replace("Bonjour", "Salut", $texte);
   ```

5. **strtolower** : Convertit une chaîne en minuscules.
   ```php
   $majuscules = "HELLO";
   $minuscules = strtolower($majuscules);
   ```

6. **strtoupper** : Convertit une chaîne en majuscules.
   ```php
   $minuscules = "hello";
   $majuscules = strtoupper($minuscules);
   ```

7. **trim** : Supprime les espaces au début et à la fin d'une chaîne.
   ```php
   $texte = "   Bonjour   ";
   $texte_nettoye = trim($texte);
   ```

Ces exemples illustrent certaines des fonctions les plus couramment utilisées pour les tableaux et les chaînes de caractères en PHP. N'oubliez pas de consulter la documentation officielle de PHP pour une liste complète des fonctions et leurs détails : [Documentation PHP - Tableaux](https://www.php.net/manual/fr/ref.array.php) et [Documentation PHP - Chaînes de caractères](https://www.php.net/manual/fr/ref.strings.php).

<!-- Fpnctions importantes -->

Absolument, voici une liste de fonctions PHP très importantes avec des exemples pour chacune d'entre elles :

1. **Fonctions pour les chaînes de caractères** :

   - **strlen** : Retourne la longueur d'une chaîne de caractères.
     ```php
     $texte = "Hello, world!";
     echo "Longueur : " . strlen($texte);
     ```

   - **strpos** : Cherche la position d'une sous-chaîne dans une chaîne.
     ```php
     $phrase = "Bonjour à tous!";
     $position = strpos($phrase, "à");
     ```

   - **str_replace** : Remplace toutes les occurrences d'une sous-chaîne par une autre.
     ```php
     $texte = "Bonjour à tous!";
     $nouveau_texte = str_replace("Bonjour", "Salut", $texte);
     ```

   - **strtolower** : Convertit une chaîne en minuscules.
     ```php
     $majuscules = "HELLO";
     $minuscules = strtolower($majuscules);
     ```

   - **strtoupper** : Convertit une chaîne en majuscules.
     ```php
     $minuscules = "hello";
     $majuscules = strtoupper($minuscules);
     ```

2. **Fonctions pour les tableaux** :

   - **count** : Retourne le nombre d'éléments d'un tableau.
     ```php
     $nombres = [1, 2, 3, 4, 5];
     echo "Nombre d'éléments : " . count($nombres);
     ```

   - **array_push** : Ajoute un ou plusieurs éléments à la fin d'un tableau.
     ```php
     $fruits = ["pomme", "banane"];
     array_push($fruits, "orange");
     ```

   - **array_pop** : Supprime et retourne le dernier élément d'un tableau.
     ```php
     $dernier_fruit = array_pop($fruits);
     ```

   - **array_merge** : Fusionne deux tableaux en un seul.
     ```php
     $tableau1 = ["a", "b"];
     $tableau2 = ["c", "d"];
     $nouveau_tableau = array_merge($tableau1, $tableau2);
     ```

   - **array_key_exists** : Vérifie si une clé existe dans un tableau associatif.
     ```php
     $personne = ["nom" => "Dupont", "âge" => 30];
     if (array_key_exists("âge", $personne)) {
         echo "Clé 'âge' existe.";
     }
     ```

3. **Fonctions pour les dates et heures** :

   - **date** : Formate une date/heure selon le format spécifié.
     ```php
     $date_actuelle = date("Y-m-d H:i:s");
     ```

   - **strtotime** : Convertit une date textuelle en timestamp.
     ```php
     $timestamp = strtotime("2023-08-21");
     ```

   - **time** : Retourne le timestamp actuel.
     ```php
     $timestamp_actuel = time();
     ```

4. **Fonctions pour la gestion des fichiers** :

   - **file_get_contents** : Lit le contenu d'un fichier dans une chaîne.
     ```php
     $contenu = file_get_contents("fichier.txt");
     ```

   - **file_put_contents** : Écrit une chaîne dans un fichier.
     ```php
     $contenu = "Contenu à écrire";
     file_put_contents("nouveau_fichier.txt", $contenu);
     ```

   - **fopen** et **fclose** : Ouvrent et ferment un fichier pour la lecture/écriture.
     ```php
     $fichier = fopen("fichier.txt", "r");
     // ... manipulations du fichier ...
     fclose($fichier);
     ```

5. **Fonctions pour la manipulation des variables** :

   - **isset** : Vérifie si une variable est définie et non nulle.
     ```php
     if (isset($nom)) {
         echo "La variable \$nom est définie.";
     }
     ```

   - **empty** : Vérifie si une variable est vide.
     ```php
     if (empty($liste)) {
         echo "La liste est vide.";
     }
     ```

   - **unset** : Détruit une variable.
     ```php
     $variable = "Contenu";
     unset($variable);
     ```

Ces exemples couvrent certaines des fonctions PHP les plus importantes pour les chaînes de caractères, les tableaux, les dates et heures, la gestion des fichiers et des variables. Il existe bien sûr beaucoup d'autres fonctions disponibles pour répondre à différents besoins de développement.

<!-- SESSION ET CCOOKIE -->

En PHP, les sessions et les cookies sont des mécanismes importants pour gérer l'état et la persistance des données entre les différentes requêtes d'un utilisateur à un site web. Ils sont souvent utilisés pour créer des expériences utilisateur personnalisées et pour stocker des informations temporaires ou permanentes.

1. **Session en PHP :**
Une session en PHP est une façon de stocker des données côté serveur pour un utilisateur spécifique, de manière à ce que ces données soient disponibles pendant toute la durée de la visite de l'utilisateur sur le site web. Les informations stockées dans une session sont accessibles à travers différentes pages et requêtes effectuées par cet utilisateur. Les sessions sont souvent utilisées pour stocker des données sensibles comme les informations de connexion, les préférences utilisateur, etc.

**Exemple d'utilisation de sessions :**
Supposons que vous souhaitez stocker le nom d'utilisateur d'un utilisateur connecté tout au long de sa visite sur votre site :

```php
// Démarre la session
session_start();

// Stocke le nom d'utilisateur dans la session
$_SESSION['username'] = 'utilisateur123';
```

Sur une autre page du site, vous pouvez accéder au nom d'utilisateur stocké dans la session :

```php
// Démarre la session
session_start();

// Accède au nom d'utilisateur depuis la session
if (isset($_SESSION['username'])) {
    echo "Bienvenue, " . $_SESSION['username'];
} else {
    echo "Utilisateur non connecté.";
}
```

2. **Cookie en PHP :**
Un cookie en PHP est un petit fichier de données stocké sur l'ordinateur de l'utilisateur. Les cookies sont envoyés par le serveur et stockés dans le navigateur de l'utilisateur. Ils sont utilisés pour stocker des informations qui doivent persister entre les sessions et même après que l'utilisateur a quitté le site.

**Exemple d'utilisation de cookies :**
Imaginons que vous souhaitez stocker la langue préférée d'un utilisateur afin de lui afficher le contenu dans cette langue lors de ses prochaines visites :

```php
// Crée un cookie pour stocker la langue préférée
$expiry = time() + 60 * 60 * 24 * 30; // Expiration dans 30 jours
setcookie('lang', 'fr', $expiry);

// Plus tard, sur une autre page, récupérez la langue préférée depuis le cookie
if (isset($_COOKIE['lang'])) {
    $lang = $_COOKIE['lang'];
    echo "Langue préférée : " . $lang;
} else {
    echo "Langue par défaut.";
}
```

Il est important de noter que les cookies ont des limites en termes de taille et que les utilisateurs peuvent les désactiver ou les supprimer, tandis que les sessions sont gérées côté serveur et sont plus sécurisées pour stocker des données sensibles.

En résumé, les sessions et les cookies en PHP sont des outils puissants pour gérer l'état et stocker des informations temporaires ou permanentes pour les utilisateurs sur un site web. Le choix entre les deux dépend des besoins spécifiques de votre application et des niveaux de sécurité requis.

Bien sûr, voici une comparaison plus détaillée entre les sessions et les cookies en PHP :

**1. Sessions en PHP :**

- **Stockage :** Les données de session sont stockées côté serveur. Le serveur crée un identifiant unique de session pour chaque utilisateur, et les données sont associées à cet identifiant.
- **Durée de vie :** Les données de session restent en mémoire tant que la session est active, généralement jusqu'à ce que l'utilisateur ferme le navigateur ou que la session expire en raison de l'inactivité.
- **Taille :** Les sessions peuvent stocker une quantité plus importante de données, car elles sont stockées côté serveur.
- **Sécurité :** Les données de session ne sont pas accessibles directement par l'utilisateur, ce qui les rend plus sécurisées pour stocker des informations sensibles.
- **Exemple d'utilisation :** Stocker des informations de connexion, des paniers d'achats temporaires, des préférences utilisateur, etc.

**2. Cookies en PHP :**

- **Stockage :** Les cookies sont stockés côté client, c'est-à-dire dans le navigateur de l'utilisateur.
- **Durée de vie :** Les cookies peuvent être persistants (restant même après la fermeture du navigateur) ou de session (disparus après la fermeture du navigateur).
- **Taille :** Les cookies ont une limite de taille (généralement quelques kilo-octets), ce qui restreint la quantité de données qu'ils peuvent contenir.
- **Sécurité :** Les cookies sont visibles et accessibles par l'utilisateur, ce qui peut les rendre vulnérables si des données sensibles y sont stockées sans cryptage approprié.
- **Exemple d'utilisation :** Mémoriser des préférences utilisateur (comme la langue), suivre les activités de l'utilisateur, garder les utilisateurs connectés (à l'aide de cookies persistants), etc.

**Résumé :**

En résumé, les sessions en PHP sont plus adaptées pour stocker des données sensibles et importantes, car elles sont gérées côté serveur et ne sont pas accessibles directement par l'utilisateur. Les sessions sont également pratiques pour stocker de grandes quantités de données. D'autre part, les cookies sont utiles pour stocker de petites quantités d'informations, telles que des préférences utilisateur, et pour suivre les activités de l'utilisateur au fil du temps. Cependant, en raison de leur nature côté client, les cookies doivent être utilisés avec prudence pour éviter toute exposition de données sensibles.

Ce code PHP gère le processus d'upload d'une image depuis un formulaire HTML vers un répertoire spécifique (`$target_dir`). Il effectue diverses vérifications pour s'assurer que le fichier téléchargé est une image valide et respecte certaines conditions.

Voici une explication ligne par ligne de ce code :

```php
$target_dir = "uploads/"; // Répertoire où les images téléchargées seront stockées
$target_file = $target_dir . basename($_FILES["pictures"]["name"]); // Chemin complet du fichier cible
$uploadOk = 1; // Variable pour suivre si le téléchargement est possible
$imageFileType = strtolower(pathinfo($target_file, PATHINFO_EXTENSION)); // Extension du fichier image

if (isset($_POST["validate"])) {
    $check = getimagesize($_FILES["pictures"]["tmp_name"]); // Vérifier si le fichier est une image valide
    if ($check !== false) {
        echo "File is an image - " . $check["mime"] . ".";
        $uploadOk = 1; // Le téléchargement est possible car le fichier est une image
    } else {
        echo "File is not an image.";
        $uploadOk = 0; // Le téléchargement n'est pas possible car le fichier n'est pas une image
    }
}

if (file_exists($target_file)) {
    echo "Sorry, file already exists.";
    $uploadOk = 0; // Le téléchargement n'est pas possible car le fichier existe déjà
}

if ($_FILES["pictures"]["size"] > 500000) {
    echo "Sorry, your file is too large.";
    $uploadOk = 0; // Le téléchargement n'est pas possible car le fichier est trop volumineux
}

if (
    $imageFileType != "jpg" && $imageFileType != "png" && $imageFileType != "jpeg"
    && $imageFileType != "gif"
) {
    echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
    $uploadOk = 0; // Le téléchargement n'est pas possible car le format de fichier n'est pas autorisé
}

if ($uploadOk == 0) {
    echo "Sorry, your file was not uploaded.";
} else {
    if (move_uploaded_file($_FILES["pictures"]["tmp_name"], $target_file)) {
        echo "The file " . htmlspecialchars(basename($_FILES["pictures"]["name"])) . " has been uploaded.";
    } else {
        echo "Sorry, there was an error uploading your file.";
    }
}
```

En résumé, ce code effectue les actions suivantes :

1. Il détermine le chemin complet du fichier cible en combinant le répertoire de destination avec le nom du fichier envoyé.
2. Il vérifie si le formulaire a été soumis pour le traitement.
3. Il utilise la fonction `getimagesize()` pour vérifier si le fichier est une image valide.
4. Il vérifie si le fichier existe déjà dans le répertoire de destination.
5. Il vérifie si la taille du fichier est inférieure à 500 000 octets (500 Ko).
6. Il vérifie si l'extension du fichier est autorisée (JPG, JPEG, PNG ou GIF).
7. En fonction des vérifications précédentes, il affiche des messages d'erreur ou réussite concernant le téléchargement du fichier.

Assurez-vous d'intégrer ce code dans un formulaire HTML approprié pour permettre aux utilisateurs de télécharger des images.

Bien sûr, voici un exemple de code PHP pour charger et envoyer une image dans une base de données, en vérifiant également la taille minimale du fichier image. Dans cet exemple, nous supposons que vous utilisez une base de données MySQL et que vous avez une table appelée "images" avec les colonnes "id" (auto-incrément), "nom" (nom du fichier) et "image" (type BLOB pour stocker l'image).

```php
<?php
// Vérifier si le formulaire a été soumis
if (isset($_POST['submit'])) {
    // Vérifier si un fichier a été téléchargé
    if (isset($_FILES['image']) && $_FILES['image']['error'] === UPLOAD_ERR_OK) {
        // Vérifier le type de fichier (assurez-vous qu'il s'agit bien d'une image)
        $mimeTypes = ['image/jpeg', 'image/png', 'image/gif'];
        $uploadedMimeType = $_FILES['image']['type'];
        if (!in_array($uploadedMimeType, $mimeTypes)) {
            echo "Le fichier n'est pas une image valide.";
            exit();
        }

        // Vérifier la taille minimale du fichier (en octets)
        $minFileSize = 1024; // 1 Ko
        if ($_FILES['image']['size'] < $minFileSize) {
            echo "La taille de l'image est inférieure à la taille minimale autorisée.";
            exit();
        }

        // Charger le fichier dans une variable
        $imageData = file_get_contents($_FILES['image']['tmp_name']);

        // Connexion à la base de données (remplacez les valeurs par vos propres informations)
        $servername = 'localhost';
        $username = 'nom_utilisateur';
        $password = 'mot_de_passe';
        $dbname = 'nom_base_de_donnees';

        $conn = new mysqli($servername, $username, $password, $dbname);

        // Vérifier la connexion à la base de données
        if ($conn->connect_error) {
            die("La connexion à la base de données a échoué : " . $conn->connect_error);
        }

        // Préparer et exécuter la requête SQL pour insérer l'image
        $stmt = $conn->prepare("INSERT INTO images (nom, image) VALUES (?, ?)");
        $stmt->bind_param("sb", $_FILES['image']['name'], $imageData);
        
        if ($stmt->execute()) {
            echo "Image téléchargée avec succès.";
        } else {
            echo "Erreur lors du téléchargement de l'image : " . $stmt->error;
        }

        // Fermer la connexion à la base de données
        $stmt->close();
        $conn->close();
    } else {
        echo "Erreur lors du téléchargement du fichier.";
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Téléchargement d'image</title>
</head>
<body>
    <form action="" method="POST" enctype="multipart/form-data">
        <input type="file" name="image" accept="image/*">
        <button type="submit" name="submit">Télécharger</button>
    </form>
</body>
</html>
```

N'oubliez pas d'ajuster les informations de connexion à la base de données (serveur, nom d'utilisateur, mot de passe et nom de la base de données) ainsi que le schéma de votre table "images" selon vos besoins.


Bien sûr, voici comment vous pouvez afficher une image stockée en tant que photo sur une page HTML en utilisant le code PHP et HTML ensemble :

```php
<!DOCTYPE html>
<html>
<head>
    <title>Affichage d'image</title>
</head>
<body>
    <?php
    // Connexion à la base de données (remplacez les valeurs par vos propres informations)
    $servername = 'localhost';
    $username = 'nom_utilisateur';
    $password = 'mot_de_passe';
    $dbname = 'nom_base_de_donnees';

    $conn = new mysqli($servername, $username, $password, $dbname);

    // Vérifier la connexion à la base de données
    if ($conn->connect_error) {
        die("La connexion à la base de données a échoué : " . $conn->connect_error);
    }

    // Récupérer les données de l'image depuis la base de données
    $imageId = 1; // L'identifiant de l'image que vous souhaitez afficher
    $sql = "SELECT nom, image FROM images WHERE id = $imageId";
    $result = $conn->query($sql);

    if ($result->num_rows > 0) {
        $row = $result->fetch_assoc();
        $imageName = $row['nom'];
        $imageData = $row['image'];

        // Afficher l'image en tant que photo
        echo "<h2>Image : $imageName</h2>";
        echo "<img src='data:image/jpeg;base64," . base64_encode($imageData) . "' alt='$imageName'>";
    } else {
        echo "Aucune image trouvée.";
    }

    // Fermer la connexion à la base de données
    $conn->close();
    ?>
</body>
</html>
```

Assurez-vous de remplacer les informations de connexion à la base de données (`$servername`, `$username`, `$password`, `$dbname`) avec vos propres valeurs. Le code ci-dessus récupère les données de l'image de la base de données en fonction de l'identifiant de l'image (`$imageId`). Ensuite, il affiche l'image en tant que photo à l'aide de la balise `<img>` avec la source définie comme une URL de données encodée en base64.

Notez que ce code est basé sur l'hypothèse que vous avez déjà stocké des images dans votre base de données conformément à la logique précédemment fournie.

En PHP, les superglobales sont des variables prédéfinies qui sont disponibles dans tous les contextes du script (dans les fonctions, les classes, etc.). Elles contiennent des informations spécifiques à différentes facettes de l'exécution du script, telles que les données de formulaire, les informations de session, les en-têtes HTTP, etc. Voici une liste des superglobales en PHP, avec leurs définitions et utilisations :

1. **$GLOBALS :**
   - **Définition :** Une superglobale qui contient toutes les variables globales.
   - **Utilité :** Utilisée pour accéder aux variables globales de n'importe où dans le script, même à l'intérieur des fonctions.

2. **$_SERVER :**
   - **Définition :** Contient des informations sur le serveur et l'environnement d'exécution.
   - **Utilité :** Utilisée pour récupérer des informations telles que l'URL, les en-têtes HTTP, le nom du serveur, etc.

3. **$_GET :**
   - **Définition :** Contient les données passées via la méthode HTTP GET (URL).
   - **Utilité :** Utilisée pour récupérer les valeurs des paramètres d'URL.

4. **$_POST :**
   - **Définition :** Contient les données envoyées via la méthode HTTP POST (formulaire).
   - **Utilité :** Utilisée pour récupérer les valeurs des champs de formulaire.

5. **$_REQUEST :**
   - **Définition :** Contient les données de $_GET, $_POST et $_COOKIE combinées.
   - **Utilité :** Utilisée pour accéder aux données des requêtes sans se soucier de la méthode HTTP utilisée.

6. **$_FILES :**
   - **Définition :** Contient les informations sur les fichiers téléchargés via les formulaires.
   - **Utilité :** Utilisée pour manipuler les fichiers téléchargés, tels que leur nom, type et emplacement temporaire.

7. **$_SESSION :**
   - **Définition :** Contient les variables de session créées et stockées pour l'utilisateur.
   - **Utilité :** Utilisée pour stocker et récupérer des données entre différentes pages pendant une session utilisateur.

8. **$_COOKIE :**
   - **Définition :** Contient les valeurs des cookies envoyés par le client au serveur.
   - **Utilité :** Utilisée pour accéder aux cookies stockés sur l'ordinateur de l'utilisateur.

9. **$_ENV :**
   - **Définition :** Contient les variables d'environnement de l'hôte serveur.
   - **Utilité :** Utilisée pour accéder aux variables d'environnement du système.

10. **$_GLOBALS :** (Obsolète à partir de PHP 7.0)
    - **Définition :** Contient une référence à toutes les variables globales.
    - **Utilité :** Utilisée pour accéder aux variables globales depuis n'importe où dans le script.

11. **$HTTP_RAW_POST_DATA :** (Obsolète à partir de PHP 5.6)
    - **Définition :** Contient les données brutes de la requête POST.
    - **Utilité :** Utilisée pour accéder aux données POST brutes sans utiliser $_POST.

12. **$argc et $argv :**
    - **Définition :** $argc contient le nombre d'arguments passés au script en ligne de commande, et $argv contient un tableau des arguments eux-mêmes.
    - **Utilité :** Utilisées pour récupérer les arguments passés à un script PHP exécuté en ligne de commande.

Ces superglobales offrent une manière pratique de récupérer et manipuler diverses informations, qu'il s'agisse de données de formulaire, d'informations de serveur, de variables de session, etc. Utilisez-les judicieusement pour accéder aux données dont vous avez besoin dans vos scripts PHP.


