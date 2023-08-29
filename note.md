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

<!-- Answers of HIGHFIVE -->
## robot_functions.php
```php
<?php
declare(strict_types=1);

require_once('request.php');

date_default_timezone_set("Africa/Porto-Novo");

function generateRandomRobotName()
{
      $letters = range('A', 'Z');
      $randomLetters = $letters[array_rand($letters)] . $letters[array_rand($letters)];
      $randomNumbers = rand(1000, 9999);
      return $randomLetters . '-' . $randomNumbers;
}

function greet(string $name)
{
      return "Salut humain je suis $name";
}

function robotBehavior()
{
      $fractions = mt_rand(1, 3);

      if ($fractions === 1) {
            return "Vous voulez un café ☕☕";
      } else {
            return "Extermination 😈😈!!! ... Extermination😈😈 !!! ....";
      }
}
?>
```
## generic_functions.php
```php
<?php
declare(strict_types=1);

      function getCurrentDateTime()
      {
            return "Nous sommes le " . date("d m Y") . ", il est " . date("H:i:s") . ".<br>";
      }
      function checkEvenOdd(int $number)
      {
            return ($number % 2 === 0) ? "Le nombre choisi est pair" : "Le nombre choisi est impair";
      }
      function chooseRandomNumberAndParity()
      {
            $number = rand(0, 10);
            checkEvenOdd($number);
            return "J'ai choisi le nombre $number";
      }
      function reverseName(string $name)
      {
            $reversedName = strrev($name);
            return "Mon nom à l'envers s'écrit $reversedName Ah😂. Ah😂. Ah😂.<br>";
      }

?>
```

## homepage.phtml
```php
<?php
require('generic_functions.php');
require('robot_functions.php');
require_once('request.php');

?>

<!DOCTYPE html>
<html lang="en">

<head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css" rel="stylesheet"
            integrity="sha384-4bw+/aepP/YC94hEpVNVgiZdgIC5+VKNBQNGCHeKRQN+PtmoHDEXuppvnDJzQIu9" crossorigin="anonymous">
      <link href='https://fonts.googleapis.com/css?family=Poppins' rel='stylesheet'>
      <title>homePage</title>
      <style>
            *,
            h1,
            h2,
            h3,
            h4,
            h5 {
                  font-family: 'Poppins';
                  font-size: 1rem;
            }
      </style>
</head>

<body class="container">

      <div class="row">
            <div class="col-md-6 mt-5">
                  <h1>Formulaire de robot</h1>
                  <form action="#" method="POST">
                        <div class="form-floating mb-3">
                              <input type="text" class="form-control" name="robotName"
                                    placeholder="Entrez votre pseudonyme" required>
                              <label for="robotName">Entrez un nom pour votre robot</label>
                        </div>
                        <div class="form-floating mb-3">
                              <input type="text" class="form-control" name="robotBehavior"
                                    placeholder="Entrez votre pseudonyme" required>
                              <label for="robotBehavior">Entrez le comportement de moralité du robot</label>
                        </div>
                        <button type="submit" name="valider" class="btn btn-primary fw-bold w-100 py-3">Submit</button>
                  </form>
            </div>
            <div class="col-md-6 mt-5 pt-4">
                  <p>
                        <?= greet($robotName); ?>
                  </p>
                  <p>
                        <?= getCurrentDateTime(); ?>
                  </p>
                  <p>
                        <?= chooseRandomNumberAndParity(); ?>
                  </p>
                  <p>
                        <?= reverseName($robotName); ?>
                  </p>
                  <p>
                        <?= robotBehavior(); ?>
                  </p>
                  <p>
                        <?= $robotName; ?>
                  </p>
                  <p>
                        <?= $robotBehavior; ?>
                  </p>

            </div>
      </div>

</body>

</html>
```

## request.php

```php
<?php
$robotName;
$robotBehavior;



if (isset($_GET['valider'])) {
      if (!empty($_GET['robotName']) && !empty($_GET['robotBehavior'])) {

            $robotName = $_GET['robotName'];
            $robotBehavior = $_GET['robotBehavior'];

      } else {

            $robotName = generateRandomRobotName();
      }
      $robotBehavior = robotBehavior();
}

$robotName = generateRandomRobotName();
$robotBehavior = robotBehavior();

?>
```
## index.php

```php
<?php 
require 'homepage.phtml'; 
?>
```

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