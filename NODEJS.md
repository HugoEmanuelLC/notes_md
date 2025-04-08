# NodeJs

* nvm install <version> // mise à jour de node, choix de la version sur le site nodejs.org
* nvm list // versions de nodes
* nvm use <version> // quelle version on veut travailler

##### Gestionnaire pm2

* npm install pm2 -g
    * // PM2 est un gestionnaire de processus pour les applications Node.js en production.
    * // Il permet de maintenir les applications en ligne, de les redémarrer en cas de panne et de gérer les logs.

* ecosystem.config.mjs ou ecosystem.config.cjs
    * // est le fichier de configuration de PM2.

* pm2 start ecosystem.config.cjs
    * // démarrer de l'app en local
    * // à la place de npm run dev
    * // utile pour la preparation en production

* pm2 logs 
    * // gerer les logs

* pm2 stop ecosystem.config.cjs
    * // stop application

* pm2 restart ecosystem.config.cjs
    // redémarrer de l'app

* pm2-runtime ecosystem.config.cjs
    * // en production
    * fichier Dockerfile
        * RUN npm install pm2 -g
        * CMD ["pm2-runtime", "ecosystem.config.cjs"]
            * // pm2-runtime est une version allégée de pm2 prévue pour les environnements de production en conteneurs Docker.


### EXPRESS JS

- node app.js OR nodemon app.js
    * start server
    * localhost:4000

* npm run dev 
    * à la place de "nodemon app.js"
    ```js package.json
    "scripts": {
        "dev": "nodemon ./src/app.js"
    },
    ```

* type module 
    ```js  
    // package.json
        {
            "type": "module",
        }

    // ex app.js ou autre fichier
        import {} from "dotenv/config"; 
        // à la place de
        const dotenv = require('dotenv')
        dotenv.config()
    ```

- npm i express

- npm i dotenv
    
    * fichier .env  // variables de environement (caches)
    * process.env.NAME_VARIABLE

- npm i cors

    * ```js
        const dotenv = require('dotenv')

        dotenv.
        process.env.PORT
      ```

- npm i nodemon -D 
    * seulement pour le developpement 
    ```js package.json
    "devDependencies": {
        "nodemon": "^3.1.4"
    }
    ```

