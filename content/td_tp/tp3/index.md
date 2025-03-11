+++
title = "TP3 Compte Bancaire"
weight = 20
+++

> [!ressource] Ressource
> https://github.com/Adrien-Courses/R401-TP-Compte-Bancaire

## Objectifs
Nous souhaitons créer un ensemble d'endpoint pour la gestion des comptes bancaire
- nous prêterons attention à l'anti-pattern [Anemic Domain Model](https://martinfowler.com/bliki/AnemicDomainModel.html)

http://localhost:8080/R401-TP-Compte-bancaire-0.0.1-SNAPSHOT/hello

## Développement 
L'organisation de nos packages est la suivante
- le package `domain` contiendra toute la logique métier
- le package `application` contiendra la logique application (e.g. enregistrer en base de données)
- le package `presentation` contiendra les points d'entrée de notre application REST

```
📦 bank-api
 ┣ 📂 src
 ┃ ┣ 📂 main
 ┃ ┃ ┣ 📂 java/com/fr.adriencaubel/bank
 ┃ ┃ ┃ ┣ 📂 application
 ┃ ┃ ┃ ┃ ┣ 📜 CompteBancaireService.java
 ┃ ┃ ┃ ┃ ┣ 📜 TransactionService.java
 ┃ ┃ ┃ ┣ 📂 domain
 ┃ ┃ ┃ ┃ ┣ 📂 model
 ┃ ┃ ┃ ┃ ┃ ┣ 📜 CompteBancaire.java
 ┃ ┃ ┃ ┃ ┃ ┣ 📜 Transaction.java
 ┃ ┃ ┃ ┃ ┣ 📂 repository
 ┃ ┃ ┃ ┃ ┃ ┣ 📜 FakeCompteBancaireRepository.java
 ┃ ┃ ┃ ┃ ┃ ┣ 📜 FakeTransactionRepository.java
 ┃ ┃ ┃ ┣ 📂 presentation
 ┃ ┃ ┃ ┃ ┣ 📜 CompteBancaireController.java
 ┃ ┃ ┃ ┃ ┣ 📜 TransactionController.java
 ┃ ┃ ┃ ┣ 📜 RestService.java
```

### Récupérer de l'argent
Coder la fonctionnalité permettant de récupérer de l'argent.

### Afficher un compte bancaire
Pouvoir afficher un compte bancaire en fonction de l'IBAN.

### Effectuer un transfert
Permettre d'effectuer un transfert entre deux comptes.
- En plus du transfert, une transaction est créée

## Développement avancé
### Les transfert depuis une date
Adapter le code pour pouvoir récupérer les transferts d'un utilisateur (iban) depuis une certaine date. 
- Remonter la liste des transferts et la somme totale des transactions entrantes et sortantes

### Permettre un découvert autorisé
Actuellement, si un utilisateur tente de retirer plus que son solde, la transaction échoue. Permettre aux clients de définir un découvert autorisé.
- Adapter le Model
- Rajouter l'endpoint `comptes/{iban}/decouvert?limit=500` 