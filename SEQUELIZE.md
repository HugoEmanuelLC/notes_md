# README - Guide Essentiel Sequelize

Ce fichier résume les concepts clés et les opérations importantes pour utiliser Sequelize, un ORM pour Node.js, de manière simple et précise.

## Concepts Clés

1.  **Modèles (Models) :**
    * Représentent vos tables de base de données sous forme de classes JavaScript.
    * Définis avec `sequelize.define('NomDuModèle', { /* Attributs */ })`.
    * Chaque instance d'un modèle correspond à une ligne dans la table.

    ```javascript
    const { DataTypes, Sequelize } = require('sequelize');
    const sequelize = new Sequelize('database', 'username', 'password', {
      dialect: 'sqlite' // Exemple pour SQLite
    });

    const User = sequelize.define('User', {
      firstName: DataTypes.STRING,
      lastName: DataTypes.STRING,
      email: {
        type: DataTypes.STRING,
        unique: true,
        allowNull: false
      }
    });

    module.exports = User;
    ```

2.  **Connexion à la Base de Données :**
    * Utilisation de l'objet `Sequelize` pour établir la connexion.
    * Configuration du dialecte (MySQL, PostgreSQL, SQLite, etc.) et des informations d'identification.

    ```javascript
    const { Sequelize } = require('sequelize');
    const sequelize = new Sequelize('nom_de_la_base', 'nom_utilisateur', 'mot_de_passe', {
      host: 'localhost',
      dialect: 'mysql'
    });
    ```

3.  **Opérations CRUD (Créer, Lire, Mettre à Jour, Supprimer) :**
    * Sequelize fournit des méthodes statiques sur les modèles pour interagir avec la base de données.

    * **Créer :**
        * `Model.create({ /* Valeurs des colonnes */ })`: Crée et enregistre une nouvelle entrée.

        ```javascript
        const newUser = await User.create({
          firstName: 'John',
          lastName: 'Doe',
          email: 'john.doe@example.com'
        });
        console.log(newUser.toJSON());
        ```

    * **Lire (Récupérer) :**
        * `Model.findAll({ /* Options de requête */ })`: Récupère tous les enregistrements correspondants.
        * `Model.findOne({ /* Options de requête */ })`: Récupère le premier enregistrement correspondant.
        * `Model.findByPk(id)`: Récupère un enregistrement par sa clé primaire.

        ```javascript
        const allUsers = await User.findAll();
        const oneUser = await User.findOne({ where: { email: 'john.doe@example.com' } });
        const userById = await User.findByPk(1);
        ```

    * **Mettre à Jour :**
        * `Model.update({ /* Nouvelles valeurs */ }, { where: { /* Conditions */ } })`: Met à jour les enregistrements correspondants.

        ```javascript
        const [updatedCount] = await User.update(
          { lastName: 'Smith' },
          { where: { email: 'john.doe@example.com' } }
        );
        console.log('Nombre d\'utilisateurs mis à jour:', updatedCount);
        ```

    * **Supprimer :**
        * `Model.destroy({ where: { /* Conditions */ } })`: Supprime les enregistrements correspondants.

        ```javascript
        const deletedCount = await User.destroy({
          where: { id: 1 }
        });
        console.log('Nombre d\'utilisateurs supprimés:', deletedCount);
        ```

4.  **Clause `where` (Conditions de Recherche) :**
    * Utilisée dans les opérations de lecture, mise à jour et suppression pour spécifier les conditions.
    * Peut utiliser des opérateurs Sequelize (`Op`) pour des conditions plus complexes.

    ```javascript
    const { Op } = require('sequelize');

    const usersOver18 = await User.findAll({
      where: {
        age: { [Op.gt]: 18 } // age > 18
      }
    });

    const usersJohnOrJane = await User.findAll({
      where: {
        [Op.or]: [
          { firstName: 'John' },
          { firstName: 'Jane' }
        ]
      }
    });
    ```

5.  **Associations (Relations entre les Tables) :**
    * Définissent comment les modèles sont liés (un-à-un, un-à-plusieurs, plusieurs-à-plusieurs).
    * Utilisation de méthodes comme `belongsTo`, `hasOne`, `hasMany`, `belongsToMany`.

    ```javascript
    // Exemple : User hasMany Post
    User.hasMany(Post, { foreignKey: 'userId', as: 'posts' });
    Post.belongsTo(User, { foreignKey: 'userId', as: 'author' });

    // Récupérer un utilisateur avec ses posts (eager loading)
    const userWithPosts = await User.findOne({
      where: { id: 1 },
      include: [{ model: Post, as: 'posts' }]
    });
    ```

## Points Importants à Retenir

* **Asynchrone :** La plupart des opérations Sequelize retournent des Promises. Utilisez `async/await` ou `.then()` et `.catch()` pour gérer les résultats.
* **Modèles dans des Fichiers Séparés :** Il est recommandé de définir chaque modèle dans son propre fichier (ex: `models/user.js`, `models/post.js`).
* **Configuration de la Connexion :** Centralisez la configuration de votre base de données dans un fichier dédié (ex: `config/database.js`).
* **Sequelize CLI (Optionnel mais Utile) :** Un outil en ligne de commande pour gérer les migrations de la base de données et la génération de modèles.
* **Documentation Officielle :** La [documentation de Sequelize](https://sequelize.org/master/) est votre ressource principale pour des informations détaillées.

Ce guide fournit une base solide pour commencer à utiliser Sequelize. Explorez la documentation pour des fonctionnalités plus avancées comme les transactions, les hooks, les validations et les scopes.