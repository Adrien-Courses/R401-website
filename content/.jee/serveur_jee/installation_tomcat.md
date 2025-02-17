+++
title = "Intégration Tomcat/Eclipse"
weight = 10
+++

> [!ressource] Ressources
> - [Intégration Tomcat/Eclipse](https://koor.fr/Java/TutorialJEE/jee_eclipse_tomcat.wp)

## Téléchargement
- Télécharger une version Eclipse Enterprise Edition
- Télécharger la version zip 10.1 de Tomcat [https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi)

## Installer Tomcat
1. De-zipper Tomcat dans le répertoire de votre choix (e.g. C:/java/tomcat)

## Configurer Eclipse
1. Aller dans l'onglet "Window" > "Show View" > "Servers"
2. Dans la vue "Servers" qui apparaît, cliquer droit et choisir "New" > "Server"
3. Dans la liste des serveurs, développer "Apache" et sélectionner "Tomcat v10.0 Server"
4. Puis sélectionner l'emplacement de votre serveur Tomcat (e.g. C:/java/tomcat)
5. Finish

![Tomcat Installation](../images/tomcat_installation.png)

## Tester la configuration
### Créer un projet web de test

1. File > New > Dynamic Web Project
2. Donner un nom au projet
3. Sélectionner Tomcat 10 comme runtime
   - Le 6.1 correspond à la version de l'API Servlet
4. Cliquer sur "Finish"


### Ajouter une page de test

1. Créer un fichier index.html dans WebContent ou src/main/webapp
2. Ajouter du contenu HTML basique (e.g. index.html)
   ![Dynamic web app structure](../images/dynamic_web_app_structure.png)
3. Déployer le projet sur Tomcat (glisser-déposer vers le serveur)
    - Dans l'onglet "Server" > Clic Droit > "Add and Remove" 
    - Ajouter (add) le projet créé
    ![Add and remove tomcat](../images/tomcat_add_and_remove.png)
4. Accéder à http://localhost:8080/nom-du-projet (e.g. http://localhost:8080/test)



