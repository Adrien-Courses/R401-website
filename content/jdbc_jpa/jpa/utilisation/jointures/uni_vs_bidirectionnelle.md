+++
title = "Retour mappedBy"
weight = 50
+++

{{% notice style="tip" title="Ressources" icon="book" %}}
- [What is the difference between Unidirectional and Bidirectional JPA and Hibernate associations?](https://stackoverflow.com/questions/5360795/what-is-the-difference-between-unidirectional-and-bidirectional-jpa-and-hibernat)
{{% /notice %}}

On revient brièvement sur l'annotation `@mappedBy` et son utilité.

> The main difference is that bidirectional relationship provides navigational access in both directions, so that you can access the other side without explicit queries. Also it allows you to apply cascading options to both directions.

- Via une relation bidirectionnelle nous aurons access au getter et setter des deux côtés de la relation (du parent -> l'enfant et de l'enfant -> le parent)
- La gestion des cascades devient implicite. 

Par exemple si dans notre classe `User` nous définissons
```java
@Entity
@Table(name = "T_Users")
public class User {

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private Set<Command> commands = new HashSet<>();
```

- `orphanRemoval = true` s'assure que si nous supprimons (en code java) une commande d'un utilisateur, alors la commande sera automatique supprimer en base de données
- `cascade = CascadeType.ALL` s'assure que si un utilisateur est supprimé alors toutes ses commandes le seront aussi, il en va de même si un utilisateur est créé avec plusieurs commandes.

```java
try {
    // Créer un nouvel utilisateur
    User user = new User("john_doe", "password123", 0);
    
    // Créer des commandes et les associer à l'utilisateur
    Command command1 = new Command();
    Command command2 = new Command();
    
    // Ajouter les commandes à l'utilisateur
    user.addCommand(command1);
    user.addCommand(command2);
    
    // Persister l'utilisateur, ce qui persiste également les commandes
    entityManager.persist(user);
    
    // Valider la transaction
    transaction.commit();
} catch (Exception e) {
    ...
} finally {
    // Fermer l'EntityManager
    entityManager.close();
    entityManagerFactory.close();
}
```