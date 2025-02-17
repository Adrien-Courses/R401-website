+++
title = "Exercice 1"
weight = 10
+++

> [!ressource] Ressources
> - [https://formations.github.io/javaee/tp/servlet.html#_lancement_de_l_application](https://formations.github.io/javaee/tp/servlet.html#_lancement_de_l_application)

## Prérequis
- Installer un [serveur web tomcat (ou tomEE)]({{< ref "installation_tomcat" >}})

## Exercice a
>  L’objectif est de créer un servlet qui écrit dans la log système lorsqu’il est accédé en GET. 

1. Créer une nouvelle classe de servlet, `SysoutServlet`
  - Hériter de HttpServlet.
  - Implémenter la méthode doGet() : le corps de celle-ci fait un appel à la méthode System.out.println().
2. Modifier le descripteur de déploiement `web.xml` pour lier ce servlet au fragment d’URL `/sysout.`
3. Accéder à `http://localhost:8080/<NomProjet>/sysout` via GET.

**Ne pas oublier <NomProjet> dans l'URL, e.g. http://localhost:8080/exercice1/sysout**

## Exercice b
> L’objectif est de créer un servlet configuré avec les annotations - et non pas dans le descripteur de déploiement. 

1. Copier-coller la classe ci-dessus en `AnnotatedServlet`.
2. Configurer le servlet via les annotations pour qu’il soit accessible en GET via le fragment `/annotate`.
3. Accéder à `http://localhost:8080/annotate`.

## Exercice c
>  L’objectif est de créer un servlet qui redirige l’utilisateur vers un site externe 

1. Créer une nouvelle classe de servlet, `RedirectServlet`.
2. Implémenter la méthode `doGet()` pour rediriger l’utilisateur vers un site externe, p.e. `https://www.iut-rodez.fr/fr`.
3. Configurer le servlet soit par annotations, soit dans le descripteur de déploiement pour être accessible via `/redirect`.
4. Accéder à `http://localhost:8080/redirect`.

On peut également utiliser l'en-tête HTTP pour réaliser cette redirection

1. Remplacer l’implémentation de la méthode `doGet()`:
  -  Ajouter le header Location avec l’URL précédente
  -   Renvoyer un code statut HTTP de redirection
2. Configurer le servlet pour être accessible via `/redirect2`.
3. Accéder à `http://localhost:8080/redirect2`.
