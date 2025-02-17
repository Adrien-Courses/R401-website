+++
title = "TP1 Celsius/Fahrenheit"
weight = 10
+++

> [!ressource] Ressource
> https://github.com/Adrien-Courses/R401-TP-celsuis-fahrenheit


## Consignes
Nous souhaitons un formulaire permettant de convertir des Celsius and Fahrenheit.

1. Dessiner le Schéma MVC de l'application
2. Coder

<!---
2 vue : index qui contient le form et result le résultat

1 controller qui en plus d'appeler la conversion (Model) fera également la redirection
-->

URL : http://localhost:8080/R401-TP-celsius-fahrenheit/

## Complément
Nous allons également déployer le `war` (notre application) dans un serveur TomEE/Tomcat comme si nous étions en production

1. Créer le `war` du projet en lançant un `mvn clean install` et ouvrez le fichier `/target`
2. Copier le fichier `R401-TP-celsius-fahrenheit-1.0-SNAPSHOT.war` dans `repertoireTomEE/webapps/`
3. Renommer le `war` en `cf.war` (un nom plus simple)
4. Modifier le port `8080` en `7878` dans `repertoireTomEE/conf/server.xml`
5. Lancer le serveur TomEE `./bin/catalina.sh start`

Rendez vous sur `http://localhost:7878/cf/`