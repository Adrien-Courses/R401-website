+++
title = "PrepareStatement"
weight = 5
+++

{{% notice style="tip" title="Ressources" icon="book" %}}
- [Créer un PreparedStatement sur une requête SQL simple](https://youtu.be/Cb7lh0-sNZI?list=PLzzeuFUy_Cnheztz2UEfV1UMeeowaevwt)
{{% /notice %}}

## Statement

```java
Connection conn = DriverManager.getConnection("url");

Statement stmt = con.createStatement();
String sql = "SELECT * FROM users WHERE name + 'name';"

ResultSet rs = stmt.executeQuery(sql)
```

Or, cette pratique de concaténer une requête SQL avec un paramètre (ici name) permet des Injections SQL.
Ces chaines paramétrés (e.i. Statement) doivent donc être utilisées de façon limité avec beaucoup de précaution. Une façon plus simple est d'éviter de les utiliser et préférer les `PrepareStatement`

## PrepareStatement

```java
PrepareStatement ps = con.createPrepareStatement("SELECT * FROM users");

ps.executeQuery(); // sans paramètre
(ps.executeUpdate();)
```

Les différences sont très subtiles :
- On créait une requête qui est définit à la création du `prepareStatement`, là où avec le statement on pouvait avoir plusieurs requêtes sql `stmt.executeQuery(sql)`, `stmt.executeQuery(sql2)`
- la requête SQL est paramétrable via le `?` puis préciser la valeur du paramètre

```java
PrepareStatement ps = con.createPrepareStatement("SELECT * FROM users WHERE name = ?");
ps.setString(1, "Paul"); // /!\ On commence à 1 et pas à 0

ResultSet rs = ps.executeQuery();

ps.setString(1, "Ines");
ResultSet rs2 = ps.executeQuery();
```