+++
title = "Rest API via JAX-RS"
weight = 20
+++

## Inconvénients Servlet
Il est tout à fait possible de créer des applications REST en utilisant uniquement l’API Servlet. Cependant, nous utilisons généralement des API spécialisées comme JAX-RS (Jakarta RESTful Web Services), car elles offrent plusieurs avantages significatifs, notamment

### 1. Simplicité et Productivité
- Implémenter une API REST avec uniquement l’API Servlet oblige à gérer manuellement l’analyse des requêtes, le formatage des réponses et la négociation du contenu.
- JAX-RS (ex : Jersey, RESTEasy) fournit des annotations comme `@GET`, `@POST`, `@Path`, rendant le développement plus déclaratif et plus lisible.

### 2. Sérialisation automatique en JSON/XML
Les bibliothèques comme Jackson s’intègrent facilement avec JAX-RS et Spring pour convertir automatiquement les objets Java en JSON ou XML.

### 3. Négociation de contenu
- Avec les Servlets purs, il faut analyser manuellement les en-têtes comme "Accept" et "Content-Type".
- Avec JAX-RS, cette gestion est automatique, facilitant le support de plusieurs formats de réponse (JSON, XML...)

### 4. Meilleure gestion des routes et des paramètres
- Avec les Servlets, vous devez extraire manuellement les paramètres de chemin et paramètres de requête depuis `HttpServletRequest`.
- Avec JAX-RS, vous pouvez simplement utiliser :
   - `@PathParam`, `@QueryParam` (JAX-RS), ce qui rend le code plus propre et plus facile à comprendre.

## Implémentation JAX-RS
JAX-RS est la spécification, les deux implémentations les plus communes sont `Jersey` et `RESTeasy`

![](https://files.codingninjas.in/article_images/what-is-jax-rs-1-1664800150.webp)

Jersey comme RESTeasy utilisent les servlets

> RESTeasy is implemented as a ServletContextListener and a Servlet and deployed within a WAR file. 