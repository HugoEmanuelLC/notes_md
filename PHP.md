# PHP APIRest

## Terminal

* composer require
* composer update
* composer init
* composer require slim/slim "^4.0"
* composer start



## Classes

```php
// Définition d'une classe
<?php
    class Utilisateur {
        // Propriétés
        public $nom;
        public $email;

        // Méthodes
        public function afficherInfos() {
            echo "Nom : " . $this->nom . "<br>";
            echo "Email : " . $this->email . "<br>";
        }
    }
?>
```

```php
// Création d'objets (instanciation)
<?php
    $utilisateur1 = new Utilisateur();
    $utilisateur1->nom = "Alice";
    $utilisateur1->email = "alice@example.com";

    $utilisateur2 = new Utilisateur();
    $utilisateur2->nom = "Bob";
    $utilisateur2->email = "bob@example.com";

    $utilisateur1->afficherInfos();
    $utilisateur2->afficherInfos();
?>
```


```php
// Propriétés (attributs)
/*
Visibilité :
    public : Accessible depuis n'importe où.
    private : Accessible uniquement à l'intérieur de la classe.
    protected : Accessible à l'intérieur de la classe et des classes héritées.

Méthodes (fonctions)
    Visibilité : (identique aux propriétés)
    $this : Mot-clé qui fait référence à l'objet courant.
*/ 

// Constructeur
<?php
    class Utilisateur {
        public $nom;
        public $email;

        public function __construct($nom, $email) {
            $this->nom = $nom;
            $this->email = $email;
        }

        // ...
    }

    $utilisateur = new Utilisateur("Charlie", "charlie@example.com");
?>

// Héritage
<?php
    class Admin extends Utilisateur {
        public $niveau;

        public function __construct($nom, $email, $niveau) {
            parent::__construct($nom, $email); // Appel du constructeur parent
            $this->niveau = $niveau;
        }

        public function afficherInfos() {
            parent::afficherInfos(); // Appel de la méthode parent
            echo "Niveau : " . $this->niveau . "<br>";
        }
    }

    $admin = new Admin("David", "david@example.com", "1");
    $admin->afficherInfos();
?>
```

#### Encapsulation
* L'encapsulation consiste à regrouper les propriétés et les méthodes d'un objet et à contrôler leur accès. Elle permet de protéger les données d'un objet et de garantir son intégrité.

```php
<?php
    class CompteBancaire {
        private $solde;

        public function __construct($soldeInitial) {
            $this->solde = $soldeInitial;
        }

        public function deposer($montant) {
            if ($montant > 0) {
                $this->solde += $montant;
            }
        }

        public function getSolde() {
            return $this->solde;
        }
    }

    $compte = new CompteBancaire(1000);
    $compte->deposer(500);
    echo "Solde : " . $compte->getSolde(); // Accès via une méthode publique
?>
```


#### Polymorphisme
* Le polymorphisme permet à des objets de classes différentes de répondre de manière différente à un même appel de méthode.

```php
<?php
    interface Forme {
        public function calculerAire();
    }

    class Rectangle implements Forme {
        private $largeur;
        private $hauteur;

        public function __construct($largeur, $hauteur) {
            $this->largeur = $largeur;
            $this->hauteur = $hauteur;
        }

        public function calculerAire() {
            return $this->largeur * $this->hauteur;
        }
    }

    class Cercle implements Forme {
        private $rayon;

        public function __construct($rayon) {
            $this->rayon = $rayon;
        }

        public function calculerAire() {
            return pi() * pow($this->rayon, 2);
        }
    }

    function afficherAire(Forme $forme) {
        echo "Aire : " . $forme->calculerAire() . "<br>";
    }

    $rectangle = new Rectangle(5, 10);
    $cercle = new Cercle(3);

    afficherAire($rectangle); // Affiche l'aire du rectangle
    afficherAire($cercle); // Affiche l'aire du cercle
?>
```


#### Interfaces
* Les interfaces définissent un ensemble de méthodes qu'une classe doit implémenter.

```php
<?php
    interface Logger {
        public function log(string $message);
    }

    class FileLogger implements Logger {
        public function log(string $message) {
            file_put_contents('log.txt', $message . PHP_EOL, FILE_APPEND);
        }
    }

    class DatabaseLogger implements Logger {
        public function log(string $message) {
            // Logique pour enregistrer dans la base de données
        }
    }

    function utiliserLogger(Logger $logger, string $message) {
        $logger->log($message);
    }

    $fileLogger = new FileLogger();
    utiliserLogger($fileLogger, "Message de log");
?>
```


#### Classes abstraites
* Les classes abstraites ne peuvent pas être instanciées. Elles servent de modèles pour les classes enfants.

```php
<?php
    abstract class Vehicule {
        abstract public function demarrer();

        public function arreter() {
            echo "Véhicule arrêté<br>";
        }
    }

    class Voiture extends Vehicule {
        public function demarrer() {
            echo "Voiture démarrée<br>";
        }
    }

    $voiture = new Voiture();
    $voiture->demarrer();
    $voiture->arreter();
?>
```


#### Bonnes pratiques
* Respectez les conventions de nommage (PSR-4).

* Utilisez l'héritage et le polymorphisme pour rendre votre code plus flexible.

* Appliquez l'encapsulation pour protéger vos données.

* Utilisez des interfaces et des classes abstraites pour définir des contrats.



## Framework Slim PHP

```php
<?php
    require '../vendor/autoload.php';

    $app = new \Slim\App();

    // Middleware global (appliqué à toutes les routes)
    $app->add(function ($request, $response, $next) {
        error_log("Middleware global: Avant la route");
        $response = $next($request, $response);
        error_log("Middleware global: Après la route");
        return $response;
    });

    // Middleware de route (appliqué uniquement à /users)
    $authenticate = function ($request, $response, $next) {
        $token = $request->getHeaderLine('Authorization');
        if ($token !== 'valid_token') {
            return $response->withStatus(401); // Non autorisé
        }
        $response = $next($request, $response);
        return $response;
    };

    // Route GET /users
    $app->get('/users', function ($request, $response) {
        $users = [['id' => 1, 'name' => 'Alice'], ['id' => 2, 'name' => 'Bob']];
        $response->getBody()->write(json_encode($users));
        return $response->withHeader('Content-Type', 'application/json');
    })->add($authenticate); // Ajout du middleware à la route

    // Route GET /products/{id}
    $app->get('/products/{id}', function ($request, $response, $args) {
        $id = (int) $args['id'];
        $product = ['id' => $id, 'name' => 'Product ' . $id];
        $response->getBody()->write(json_encode($product));
        return $response->withHeader('Content-Type', 'application/json');
    });

    // Route POST /orders
    $app->post('/orders', function ($request, $response) {
        $data = $request->getParsedBody();
        $order = ['id' => 1, 'product' => $data['product'], 'quantity' => $data['quantity']];
        $response->getBody()->write(json_encode($order));
        return $response->withHeader('Content-Type', 'application/json')->withStatus(201);
    });

    $app->run();
?>
```


##### Explications et points clés

### Routage :
Les deux frameworks utilisent des méthodes comme get, post, put, delete pour définir les routes.
Les paramètres de route sont définis différemment ({id} en Slim, :id en Express.js).

### Middlewares :
Les middlewares sont ajoutés avec add (Slim) ou use (Express.js).
next() est crucial pour passer le contrôle au middleware suivant ou à la route.
L'ordre d'execution est inversé.

### Gestion des données :
Slim utilise $request->getParsedBody() pour récupérer les données JSON.
Express.js utilise express.json() et req.body.

### Réponses :
Les deux frameworks utilisent des méthodes pour envoyer des réponses JSON et définir les codes de statut HTTP.

### Fichiers de configuration:
Slim utilise Composer et son fichier composer.json pour la gestion des dépendances.
Express.js utilise npm et son fichier package.json pour la gestion des dépendances.

### Démarrage du serveur :
Slim utilise $app->run() pour démarrer l'application.
Express.js utilise app.listen() pour démarrer le serveur.


Ces exemples vous donnent une base solide pour commencer à développer des API avec Slim et Express.js. N'hésitez pas à les adapter et à les étendre en fonction de vos besoins.


### Slim Terminal

* composer create-project slim/slim-skeleton ./

* composer create-project slim/slim-skeleton [app-name]

* composer start