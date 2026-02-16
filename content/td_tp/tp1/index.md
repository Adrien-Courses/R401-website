+++
title = "TP1 Celsius/Fahrenheit"
weight = 10
+++

> [!ressource] Ressource
> https://github.com/Adrien-Courses/R401-TP-celsuis-fahrenheit


## Résultat à obtenir
Lorsque nous allons sur l'endpoint web `http://localhost:8810/convert?temperature=30` permettant de convertir des Celsius and Fahrenheit, nous souhaitons avoir l'affichage suivant

![pas de changement d'url](td_tp/tp1/images/tp1.png)


## Consignes

### 1. Dessiner le Schéma MVC de l'application
Avant de coder, faire le schéma MVC de l'application
- qui est le modele ?
- qui est le Contrôleur ?
- qui est la vue ?

### 2. Développer
Coder l'ensemble des 3 éléments
- La classe Modèle est déjà donnée
- La classe Contrôleur soit être complétée
- Le fichier `resultat.jsp` doit être codé dans `src/main/webapp/`

=> Vous pouvez vous inspirer du cours sur le [JPS]({{< relref "presentation_layer/servlet_jsp/jsp" >}})


### 3. Complément
Jusqu’à présent, nous avons utilisé tomcat-embed afin de simuler le fonctionnement d’un serveur Tomcat directement depuis notre application Java. Cette approche est très pratique pour comprendre le rôle du conteneur de servlets, manipuler le cycle de vie d’une application web et expérimenter localement sans installation supplémentaire.

Cependant, dans un contexte réel de développement et de mise en production, on ne démarre pas un serveur Tomcat depuis une méthode main. En pratique, l’application est empaquetée (généralement au format WAR) puis déployée sur un véritable serveur d’applications comme Tomcat ou TomEE, installé et configuré indépendamment du code.

Nous allons donc maintenant passer d’une approche “embarquée” à une approche standard en déployant une application java sur un serveur applicatif réel.

#### 3.1 Générer une archive `war`

La première étape consiste a généré un exécutable de notre application sous la forme d'un `war`. Pour ce faire ouvrir un invité de commande et saisir `mvn clean package` ou dans IntelliJ -> menu de gauche -> clean puis -> package

![complement.png](td_tp/tp1/images/complement.png)

Après avoir exécuté cette commande vous devriez avoir
- Build Success dans la console
- un fichier `.war` dans le dossier `target`

#### 3.2 Télécharger un serveur TomEE et installer le war

1. Rendez-vous sur https://tomee.apache.org/download.html et télécharger TomEE Webprofile ZIP
2. Dézipper le dossier dans `z:` par exemple
3. Copier/Coller le `.war` dans le dossier `z:/apache-tomee-webprofile-10.1.4/webapps`
4. Renommer le war en `cf.war` (ou autre)
5. Modifier le port `8080` en `8810` dans `z:/apache-tomee-webprofile-10.1.4/conf/server.xml`
6. Ouvrir une CLI en admin et 
   - `cd z:/apache-tomee-webprofile-10.1.4` 
   - puis lancer le serveur TomEE `./bin/catalina.sh start`


Rendez vous sur `http://localhost:7878/cf/` (ne pas oublier le `cf` de l'étape 4)

![complement.png](td_tp/tp1/images/complement2.png)
