+++
title = "JSF"
weight = 30
+++

JavaServer Faces (JSF) est un framework Java standard pour la création d'interfaces utilisateur web basées sur des composants.
- Basé sur des composants UI : Permet de construire des interfaces riches avec des composants (`<h:inputText>`, `<h:commandButton>`, etc.).
- Séparation de la logique et de la présentation : Les vues sont définies en XHTML, et la logique métier est gérée via des Beans managés.
- Support des bibliothèques tierces : Peut être enrichi avec des frameworks comme PrimeFaces et RichFaces pour des interfaces plus modernes.

```xml
<h:form>
    <h:outputLabel for="name" value="Nom :" />
    <h:inputText id="name" value="#{userBean.name}" />
    <h:commandButton value="Envoyer" action="#{userBean.submit}" />
</h:form>
```

## Comparaison JSF / JSP+servlet
Les deux code suivants sont identiques :
- le premier écrit avec JSF
- le second avec une JSP

### JSF
```xml
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <title>Liste des Utilisateurs</title>
    </h:head>
    <h:body>
        <h1>Liste des Utilisateurs</h1>
        <h:dataTable value="#{userBean.users}" var="user" border="1">
            <h:column>
                <f:facet name="header">Nom</f:facet>
                #{user.name}
            </h:column>
            <h:column>
                <f:facet name="header">Email</f:facet>
                #{user.email}
            </h:column>
        </h:dataTable>
    </h:body>
</html>
```

- `h:dataTable` crée un tableau dynamique.
- `value="#{userBean.users}"` récupère la liste d’utilisateurs depuis le Bean managé.
- `var="user"` permet de boucler sur chaque utilisateur.

```java
@ManagedBean
@RequestScoped
public class UserBean {
    private List<User> users;

    public UserBean() {
        users = UserDAO.getUsers(); // charge une liste d'utilisateurs
    }

    public List<User> getUsers() {
        return users;
    }
}
```

### JSP + Servlet
Avec JSP et Servlets, nous devons :
- Récupérer la liste des utilisateurs dans un Servlet (`UserServlet`).
- Stocker la liste dans la requête avec `request.setAttribute("users", userList);`.
- Parcourir la liste avec une boucle for dans `users.jsp`.

```jsp
    ...
    <table border="1">
        <tr>
            <th>Nom</th>
            <th>Email</th>
        </tr>
        <%
            List<User> users = (List<User>) request.getAttribute("users");
            for (User user : users) {
        %>
        <tr>
            <td><%= user.getName() %></td>
            <td><%= user.getEmail() %></td>
        </tr>
        <%
            }
        %>
    </table>
```

```java
@WebServlet("/users")
public class UserServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Récupérer la liste des utilisateurs depuis le modèle (UserDAO)
        List<User> userList = UserDAO.getUsers();
 
        // Ajouter la liste en tant qu'attribut de la requête
        request.setAttribute("users", userList);
 
        // Rediriger vers la JSP pour afficher les résultats
        request.getRequestDispatcher("users.jsp").forward(request, response);
    }
}
```

## MVC en JSF
JSF suit naturellement le pattern MVC, séparant :
- Modèle (Model) → Les Beans managés, qui contiennent les données et la logique métier.
- Vue (View) → Les pages XHTML, qui définissent l'interface utilisateur.
- Contrôleur (Controller) → Géré implicitement par JSF, en liant les actions de la vue aux Beans.

## Résumé
- **En JSF**
    - L'affichage est déclaratif grâce à `h:dataTable` et aux Beans managés.
    - Il n'y a pas besoin d’un Servlet pour gérer les requêtes.
    - Il suffit de lier `#{userBean.users}` à une liste dans le Bean.

- **En JSP + Servlets**
  - Il faut manuellement gérer les requêtes et réponses via HttpServletRequest.
  - L'affichage nécessite une boucle for en JSP.

