+++
title = "MVC2"
weight = 20
+++

> [!ressource]
> [Mise en place d'une architecture MVC à partir des API Servlet et JSP](https://youtu.be/7tP-BKZk1qY)

Le Modèle-Vue-Contrôleur (MVC) est un patron d’architecture très utilisé en JavaEE pour organiser une application web de manière modulaire et maintenable.

MVC 2 est une évolution de MVC 1, **qui repose entièrement sur des servlets et JSP (sans logique métier dans les JSP), améliorant ainsi la séparation des responsabilités.**

![](mvc2.png)

1. Une requête HTTP part d'un utilisateur
2. Elle arrive sur une Servlet (Contrôleur)
    - Le contrôleur traite les données envoyées
    - Modifie le Model en conséquence
    - Et redirige vers la vue


## Contrôleur
- Récupère et valide les entrées utilisateur.
- Déclenche les traitements du modèle et transmet les données à la vue.
- Structure l’application en actions organisées pour faciliter la compréhension du code

## Modèle
- Gère les fonctionnalités et l'état de l'application indépendamment de l'interface.
- Responsable de la persistance des données (ex: base de données, session).
- Applique la logique métier sans être couplé à l’environnement d’exécution.

## Vue
- Responsable de l’interface utilisateur et de la mise en forme des données.
- Ne doit pas contenir de logique métier pour éviter des problèmes de maintenabilité.
- Souvent gérée par un moteur de templates (ex: JSP, JSTL en Java).

