+++
title = "Architecture JEE"
weight = 10
+++

![architecture_jee](architecture_jee.png)

On distingue habituellement deux familles de composants :
- **Les composants web** : Servlets et JSP (Java Server Pages). Il s'agit de la partie chargée de l'interface avec l'utilisateur (on parle de logique de présentation).
- **Les composants métier** : EJB (Enterprise Java Beans). Il s'agit de composants spécifiques chargés des traitements des données propres à un secteur d'activité (on parle de logique métier ou de logique applicative) et de l'interfaçage avec les bases de données. 

> [!affirmation] Objectif de cette séparation
> L'architecture J2EE permet ainsi de séparer la couche présentation, correspondant à l'interface homme-machine (IHM), la couche métier contenant l'essentiel des traitements de données : *Separation of Concern*