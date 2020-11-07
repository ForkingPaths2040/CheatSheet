# Backend Build
COMMAND | DESCRIPTION
* npm init -y | Creates package.json
* npm i nodemon -D | Creates package-lock.json and node modules
* touch **.gitignore** | Instructs Git specifically what to ignore 
  * echo "**.DS_Store**" >> .gitignore
  * echo "**node_modules**" >> .gitignore
### Typical dependencies to install
* npm i body-parser | Node.js body parsing middleware
* npm i cors | CORS is a node.js package for providing a Connect/Express middleware that can be used to enable CORS with various options.
* npm i express | Web framework for node
* npm i mongoose | Elegant mongodb object modeling for node.js
* npm i morgan | HTTP request logger middleware for node.js
