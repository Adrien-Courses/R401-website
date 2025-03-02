+++
title = "Formulaire et doPost()"
weight = 20
+++

Dans la section précédente, nous avons découvert la notion de Servlet et de page JSP. Afin d'enrichir nos applications, nous allons continuer notre étude en utilisant des formulaires et la méthode `doPost`.

## Création d'un formulaire en jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<body>
    <h1>Inscription</h1>
    <form action="inscription" method="post">
        <div>
            <label for="nom">Nom :</label>
            <input type="text" id="nom" name="nom">
        </div>
        <div>
            <input type="submit" value="S'inscrire">
        </div>
    </form>
</body>
</html>
```

- La valeur dans `action` sera l'endpoint interrogé par l'action `POST`
- `name` dans les input correspondent au nom des paramètres passés dans la requêtes

![alt text](post_url.png)

![alt text](post_param.png)

`curl 'http://localhost:8092/R401-EXO-formulaire/inscription' -X POST --data-raw 'nom=adrien'`

## Servlet 
```java
@WebServlet("/inscription")
public class InscriptionServlet extends HttpServlet {

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// Récupération des paramètres du formulaire
		String nom = request.getParameter("nom");

		request.setAttribute("message", "Inscription réussie pour " + nom);

		// Redirection vers la page de confirmation
		RequestDispatcher dispatcher = request.getRequestDispatcher("confirmation.jsp");
		dispatcher.forward(request, response);
	}
}
```

- le `setAttribut` nous permet d'ajouter un attribut à la requête qui sera ensuite récupérer dans la page de confirmation

## Page de confirmation
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<body>
    <h1>Confirmation</h1>
    <p>${message}</p>
</body>
```