+++
title = "TP3 Servlet et DP Factory"
weight = 30
+++

> [!ressource] Ressource
> - https://github.com/Adrien-Courses/R401-TP-JSF-abstract-factory-pattern.git

Dans ce TP nous nous intéressons aux :
- [Redirections avec Servlet]({{< relref "presentation_layer/servlet_jsp/servlet_redirection" >}})
- [Design Pattern Factory et Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)

> http://localhost:8080/R401-TP-JSF-abstract-factory-pattern/

## 1. Design pattern Abstract Factory
Nous avons plusieurs type de composant `HTML5` et `Mobile`
- Les composants HTML5 sont classiques : bouton, text et select
- Les composant Mobiles sont identique sauf qu'ils appliquent un style couleur = rouge

```java
public class ButtonComponent extends AbstractUIComponent {
	public String render() {
		return "<button name='" + id + "'>" + value + "</button>";
	}
}

public class MobileButtonComponent extends AbstractUIComponent {
	@Override
	public String render() {
		return "<button name='" + id + "' style='color:red;'>" + value + "</button>";
	}
}
```

En s'appuyant sur le Design Pattern Factory créer permettre la création des composants
- `HTML5ButtonComponent`, `MobileButtonComponent`
- `HTML5InputTextComponent`, `MobileInputTextComponent`
- `HTML5SelectComponent`, `MobileSelectComponent`

## 2. Créer le formulaire
Dans la méthode `ComponentServlet.doGet()` coder le formulaire qui en fonction du paramètre `renderKit` utilisera l'une ou l'autre des objets en factory

```java
@WebServlet("/formProcessor")
public class ComponentServlet extends HttpServlet {
    @Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO
    }
}
```

## 3. Coder la redirection
Une fois le formulaire soumis, redirigez vers la page `results.jsp`
- créer la page
- effectuer la redirection

Ici, on ne se préoccupe pas de transférer les données entre le formulaire et la page *results*, ni le thème (html5 ou Mobile)


## 4. Les sessions pour transmettre des informations
Dans la page de résultat on souhaite
- afficher les informations soumise dans le formulaire
- avoir le même thème que dans le formulaire

En vous appuyant sur la notion de session réalisez ceci

<!-- 

partie complémentaire avec session

1. passer renderKit en session 

Pour ce faire dans le formulaire ne pas oublier d'avoir un champ caché
<input type="hidden" name="renderKit" value=" + renderKit + ">

Pour que le doPost() récupère la valeur qui est en paramètre

et la transmette au /result qui est en JSP (ca aurait été pareil si nous étions en servlet)


On remarquera que c'est la même chose pour les button
on s'appuie que l'attribut "name" pour recup la valeur 
"<button name='" + id + "'>" + value + "</button>";

doGet
usernameInput.setId("username");

doPost
String username = request.getParameter("username");

-->


<!-- results.jsp
<%@ page contentType="text/html" pageEncoding="UTF-8" %>
<%@ page import="fr.adriencaubel.factory.UIComponentFactory,fr.adriencaubel.factory.AbstractUIComponent" %>
<%
    // Retrieve session attributes
    String username = (String) session.getAttribute("username");
    String country = (String) session.getAttribute("country");
    String renderKit = (String) session.getAttribute("renderKit");

    // Get the UI component factory
    UIComponentFactory factory = UIComponentFactory.getFactory(renderKit);

    // Create read-only input component for username
    AbstractUIComponent usernameDisplay = factory.createComponent("input");
    usernameDisplay.setId("usernameDisplay");
    usernameDisplay.setValue(username);

    // Create back button component
    AbstractUIComponent backButton = factory.createComponent("button");
    backButton.setId("backButton");
    backButton.setValue("Back to Form");
%>
<!DOCTYPE html>
<html>
<head>
    <title>Results</title>
</head>
<body>
    <h2>Form Submission Results</h2>


    <%= usernameDisplay.render().replace("/>", "readonly />") %>

    <br/>Country Selected: <%= country %>

    <form action="formProcessor" method="get">
        <%= backButton.render() %>
    </form>
</body>
</html>

-->