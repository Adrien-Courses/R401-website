+++
title = "Copie défensive / Synchronisation (2)"
weight = 61
+++

Juste ici je met des petits exos

## Cas relation bidirectionnelle

```java
@Entity
public class Commande {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "commande", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<LigneDetail> ligneDetails = new ArrayList<LigneDetail>();
}

@Entity
public class LigneDetail {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;


    @ManyToOne
    private Commande commande;
}
```

### Supprimer une ligne d'une commande (NOT WORKING)
```java
@Test
public void testRemoveLigneDetail() {
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();
    
    // Récupérer la commande id=2
    Commande commande = session.find(Commande.class, 2L);

    //  Récupérer une ligne associée à la commande (ici la première ligne)
    LigneDetail ligneDetail = commande.getLigneDetails().get(0);

    // NOT WORKING      
    session.delete(ligneDetail); 

    transaction.commit();
}
```

### Supprimer une ligne d'une commande (WORKING)
Dans le code precedent, la ligne était toujours associée à la commande.
Nous devons donc supprimer également cette association 
```java
@Test
public void testRemoveLigneDetailWorking() {
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();
    
    Commande commande = session.find(Commande.class, 3L);
    
    LigneDetail ligneDetail = commande.getLigneDetails().get(0);
    commande.getLigneDetails().remove(ligneDetail); // ADD THIS
            
    session.delete(ligneDetail); 
    
    transaction.commit();
}
```

Mais, ceci n'est pas parfait
> By using the bidirectional add sync methods, we can ensure that the persist entity state transition is going to be propagated properly. Without synchronizing both sides of the JPA association, it’s not guaranteed that the entity state will be properly synchronized with the database. [source](https://vladmihalcea.com/jpa-bidirectional-sync-methods/)

```java
@Test
public void testRemoveLigneDetailWorking() {
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();
    
    Commande commande = session.find(Commande.class, 3L);
    
    LigneDetail ligneDetail = commande.getLigneDetails().get(0);
    commande.getLigneDetails().remove(ligneDetail); // ADD THIS
    ligneDetail.setCommande(null); // ADD THIS

    session.delete(ligneDetail); 
    
    transaction.commit();
}
```

Ainsi :
- La ligne est supprimée de l'objet commande
- La commande est supprimée de l'objet ligne


### Créer une commande avec le même Id
Que se passe-t-il si on crée un nouvel objet commande qui a le même id qu'un déjà sauvegardé en base de données et qu'on y ajoute une nouvelle ligne ?

Actuellement en base de données
```
mysql> select * from Commande;
+----+
| id |
+----+
|  1 |
+----+

mysql> select * from LigneDetail;
+-------------+----+
| commande_id | id |
+-------------+----+
|           1 |  1 |
+-------------+----+
```

```java
@Test
public void testAjouterLigneDetailANouvelleCommande() {
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();

    // Création d'un nouvel objet commande et on y associe id=1
    Commande commande = new Commande();
    commande.setId(1L);

    
    List<LigneDetail> ligneDetails = new ArrayList<LigneDetail>();
    LigneDetail ligneDetail = new LigneDetail();
    ligneDetail.setCommande(commande); // On side
    ligneDetails.add(ligneDetail);
    
    
    commande.setLigneDetails(ligneDetails); // Other side 

    session.saveOrUpdate(commande);

    transaction.commit();
    sessionFactory.close();
}
```

```
mysql> select * from Commande;
+----+
| id |
+----+
|  1 |
+----+

mysql> select * from LigneDetail;
+-------------+----+
| commande_id | id |
+-------------+----+
|           1 |  1 |
|           1 |  2 |
+-------------+----+

Et seulement le SQL suivant a été exécuté
[Hibernate] 
    insert 
    into
        LigneDetail
        (commande_id) 
    values
        (?)
```

### Créer une commande avec find()
Maintenant si on souhaite faire la même chose mais en utilisant `find()` alors ça ne fonctionne pas

```java
@Test
public void testFindCommandeEtAjouterLigneDetail() {
    // Begin a transaction
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();

    Commande commande = session.find(Commande.class, 1L);

    List<LigneDetail> ligneDetails = new ArrayList<LigneDetail>();
    LigneDetail ligneDetail = new LigneDetail();
    ligneDetail.setCommande(commande);
    ligneDetails.add(ligneDetail);
    
    
    commande.setLigneDetails(ligneDetails);


    session.saveOrUpdate(commande);

    transaction.commit();
    sessionFactory.close();
}

jakarta.persistence.RollbackException: Error while committing the transaction
Caused by: org.hibernate.HibernateException: A collection with orphan deletion was no longer referenced by the owning entity instance: fr.adriencaubel.hibernatetesting.Commande.ligneDetails
```

Pour que ça fonctionne nous devons ajouter 

```java
@Test
public void testFindCommandeEtAjouterLigneDetailWorking() {
    // Begin a transaction
    Session session = sessionFactory.openSession();
    Transaction transaction = session.beginTransaction();

    Commande commande = session.find(Commande.class, 1L);

    LigneDetail ligneDetail = new LigneDetail();
    ligneDetail.setCommande(commande);
    commande.getLigneDetails().add(ligneDetail);

    // Save the updated Commande object
    session.saveOrUpdate(commande);
    transaction.commit();
    sessionFactory.close();
} 
```