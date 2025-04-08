# README - Guide Essentiel TypeScript

Ce fichier résume les concepts clés, la syntaxe et les fonctionnalités importantes de TypeScript, un sur-ensemble typé de JavaScript qui se compile en JavaScript, de manière simple et précise.

## Concepts Clés

1.  **Typage Statique :**
    * La principale caractéristique de TypeScript est l'ajout de types statiques aux variables, paramètres de fonctions, valeurs de retour, etc.
    * Permet de détecter les erreurs de type au moment de la compilation, avant l'exécution.

    ```typescript
    let message: string = "Bonjour TypeScript !";
    let compteur: number = 0;
    let estValide: boolean = true;
    let liste: number[] = [1, 2, 3];
    let tuple: [string, number] = ["clé", 123];

    function afficherMessage(texte: string): void {
      console.log(texte);
    }
    ```

2.  **Types de Base :**
    * `string`, `number`, `boolean` : Les mêmes que JavaScript.
    * `void` : Indique qu'une fonction ne retourne pas de valeur.
    * `null` et `undefined` : Types spécifiques pour les valeurs nulles et indéfinies.
    * `any` : Désactive la vérification de type (à éviter autant que possible).
    * `unknown` : Similaire à `any`, mais nécessite une vérification de type avant utilisation.
    * `never` : Représente une valeur qui n'arrivera jamais (ex: une fonction qui lève toujours une exception).

3.  **Interfaces :**
    * Définissent la structure d'un objet, spécifiant les noms et les types de ses propriétés.
    * Peuvent également décrire des signatures de fonctions.

    ```typescript
    interface Utilisateur {
      nom: string;
      age?: number; // Le point d'interrogation indique une propriété optionnelle
      direBonjour: (message: string) => void;
    }

    let utilisateur: Utilisateur = {
      nom: "Alice",
      direBonjour: (msg) => console.log(`Bonjour, ${msg} de la part de ${utilisateur.nom}`)
    };
    ```

4.  **Classes :**
    * Similaires aux classes JavaScript ES6, mais avec des fonctionnalités de typage.
    * Peuvent avoir des propriétés typées, des constructeurs et des méthodes avec des types pour les paramètres et les valeurs de retour.
    * Gèrent les modificateurs d'accès (`public`, `private`, `protected`).

    ```typescript
    class Animal {
      nom: string;
      constructor(nom: string) {
        this.nom = nom;
      }
      seDeplacer(distanceEnMetres: number = 0): void {
        console.log(`${this.nom} s'est déplacé de ${distanceEnMetres}m.`);
      }
    }

    class Chien extends Animal {
      aboyer(): void {
        console.log("Wouaf ! Wouaf !");
      }
    }

    let monChien = new Chien("Rex");
    monChien.aboyer();
    monChien.seDeplacer(10);
    ```

5.  **Fonctions :**
    * Les types peuvent être ajoutés aux paramètres et à la valeur de retour des fonctions.

    ```typescript
    function multiplier(a: number, b: number): number {
      return a * b;
    }

    let addition: (x: number, y: number) => number = (a, b) => a + b;
    ```

6.  **Tableaux et Tuples :**
    * Les types des éléments d'un tableau peuvent être spécifiés.
    * Les tuples permettent de définir des tableaux avec un nombre fixe d'éléments dont les types sont connus.

    ```typescript
    let nombres: number[] = [1, 2, 3];
    let messages: Array<string> = ["hello", "world"];
    let coordonnees: [number, number] = [10, 20];
    let infoUtilisateur: [string, number, boolean] = ["Bob", 25, true];
    ```

7.  **Énumérations (`enum`) :**
    * Permettent de définir un ensemble de constantes nommées.

    ```typescript
    enum Couleur {
      Rouge, // 0 par défaut
      Vert,  // 1
      Bleu   // 2
    }

    let couleurChoisie: Couleur = Couleur.Vert;
    console.log(couleurChoisie); // 1
    console.log(Couleur.Bleu); // 2
    console.log(Couleur[0]); // "Rouge"
    ```

8.  **Génériques (`Generics`) :**
    * Permettent d'écrire du code réutilisable qui peut fonctionner avec différents types sans perdre la sécurité de type.

    ```typescript
    function premierElement<T>(liste: T[]): T | undefined {
      return liste[0];
    }

    let nombres = premierElement([1, 2, 3]); // type de nombres sera number | undefined
    let chaines = premierElement(["a", "b", "c"]); // type de chaines sera string | undefined
    ```

9.  **Union et Intersection Types :**
    * **Union Types (`|`) :** Permettent à une variable d'avoir plusieurs types possibles.
    * **Intersection Types (`&`) :** Combinent plusieurs types en un seul.

    ```typescript
    let resultat: string | number;
    resultat = "succès";
    resultat = 123;

    interface HasNom { nom: string; }
    interface HasAge { age: number; }

    type PersonneAvecNomEtAge = HasNom & HasAge;

    let personne: PersonneAvecNomEtAge = { nom: "Charlie", age: 35 };
    ```

10. **Compilation vers JavaScript :**
    * Le code TypeScript (`.ts` fichiers) doit être compilé en JavaScript (`.js` fichiers) pour être exécuté par les navigateurs ou Node.js.
    * Le compilateur TypeScript (`tsc`) est utilisé pour cette tâche. La configuration de la compilation se fait via un fichier `tsconfig.json`.

    ```bash
    tsc monFichier.ts        # Compile un fichier
    tsc                     # Compile tous les fichiers selon tsconfig.json
    ```

## Points Importants à Retenir

* **Sûreté de Type :** Le principal avantage de TypeScript est la détection précoce des erreurs grâce au typage statique.
* **Meilleure Maintenance et Lisibilité :** Les types rendent le code plus facile à comprendre, à maintenir et à refactoriser.
* **Outils Améliorés :** TypeScript offre une meilleure expérience de développement avec l'autocomplétion, la vérification des erreurs en temps réel dans les IDE.
* **Adoption Progressive :** Vous pouvez intégrer TypeScript progressivement dans un projet JavaScript existant.
* **Compilation Nécessaire :** N'oubliez pas que le code TypeScript ne s'exécute pas directement dans les navigateurs ou Node.js ; il doit être compilé en JavaScript.

## Outils Couramment Utilisés (Spécifiques à TypeScript)

* **Compilateur TypeScript (`tsc`) :** L'outil de base pour compiler le code TypeScript.
* **`tsconfig.json` :** Fichier de configuration pour le compilateur TypeScript, spécifiant les options de compilation.
* **Linters (ESLint avec des plugins TypeScript comme `@typescript-eslint/eslint-plugin`) :** Pour appliquer des règles de style et détecter des erreurs potentielles spécifiques à TypeScript.
* **Formatters (Prettier avec des configurations TypeScript) :** Pour un formatage de code cohérent.
* **Intégration IDE (VS Code, WebStorm, etc.) :** Offre une excellente prise en charge de TypeScript avec la vérification de type en temps réel, l'autocomplétion, etc.
* **Gestionnaires de Paquets (npm, yarn) :** Utilisés pour installer TypeScript lui-même (`npm install -g typescript` ou `yarn global add typescript`) et les définitions de types pour les librairies JavaScript (`@types/<nom_de_la_librairie>`).

Ce guide fournit une base solide pour comprendre et utiliser TypeScript. Explorez la [documentation officielle de TypeScript](https://www.typescriptlang.org/) pour des informations plus détaillées et des fonctionnalités avancées.