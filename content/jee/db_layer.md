+++
title = "DB Layer"
weight = 60
+++

De nombreuses applications ont besoin de communiquer avec des SGBD.

## Java Database Connectivity (JDBC)
JDBC est une spécification pour accéder aux bases de données relationnelles de manière commune et cohérente. Avec JDBC, vous pouvez exécuter des instructions SQL et obtenir des résultats sur différentes bases de données en utilisant des API communes. 

Un pilote spécifique à la base de données se situe entre l'appel JDBC et la base de données, traduisant les appels JDBC en appels API spécifiques au fournisseur de la base de données.

## L'API Java Persistence (JPA)
Un des problèmes de l'utilisation directe des API JDBC est que vous devez constamment mapper les données entre les objets Java et les données dans les colonnes des lignes de la base de données relationnelle. Des frameworks comme Hibernate ont simplifié ce processus en utilisant un concept connu sous le nom de Mapping Objet-Relationnel (ORM). L'ORM est intégré dans JEE sous la forme de l'API Java Persistence (JPA). 

JPA vous donne la flexibilité de mapper les objets aux tables dans la base de données relationnelle et d'exécuter des requêtes avec ou sans utiliser le langage SQL structuré.

```java
@Entity
@Table(name = "employees")
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "first_name", nullable = false)
    private String firstName;

    @Column(name = "last_name", nullable = false)
    private String lastName;
```