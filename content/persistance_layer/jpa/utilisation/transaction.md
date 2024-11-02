+++
title = "Transactions"
weight = 30
+++

- `void begin()` 	Débuter la transaction
- `void commit()` 	Valider la transaction
- `void roolback()` 	Annuler la transaction
- `boolean isActive()` 	Déterminer si la transaction est active

```java
EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("my-persistence-unit");
EntityManager entityManager = entityManagerFactory.createEntityManager();

EntityTransaction transaction = entityManager.getTransaction();

try {
    transaction.begin();  // Begin transaction

    MyEntity entity = new MyEntity(1L, "Entity Name");
    entityManager.persist(entity);  // Persist entity to the database

    transaction.commit(); // commit
    System.out.println("Transaction committed successfully.");
} catch (Exception e) {
    if (transaction.isActive()) {
        transaction.rollback();
        System.out.println("Transaction rolled back due to error.");
    }
    e.printStackTrace();
} finally {
    entityManager.close()
}

entityManagerFactory.close();
```