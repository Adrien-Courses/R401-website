+++
title = "TP1 Correction"
weight = 11
+++

> [!ressource] Ressource
> https://github.com/Adrien-Courses/R401-TP-celsuis-fahrenheit **branche "final"**

Nous allons découper la correction en deux étapes
- un première pour définir le modèle MVC et la redirection
- la seconde pour passer le résultat à la page `result.jsp`

## 1. MVC et redirection
La Servlet est le point d'entrée de notre application et correspond au contrôleur, le Modèle est l'endroit où la logique métier est opérée; ici la classe `TemperatureConverter`. Et finalement la vue qui sera ici en `jsp`

### Créer la servlet
- Hériter de `HttpServlet`
- Annoter la classe de `@WebServlet("/convert")` (ou définir un fichier web.xml)
- Coder le corps de la méthode `doGet()`

```java
@WebServlet("/convert")
public class TemperatureConverterServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Récupérer le paramètre ?temperature=30
        String temperatureParam = request.getParameter("temperature")

		// Appeler la logique métier (ici la conversion)
		Double conversion = TemperatureConverter.convertCelsiusToFahrenheit(Double.parseDouble(temperatureParam));


		// Redirection vers la Vue : result.jsp
		// response.sendRedirect("result.jsp");
		RequestDispatcher requestDispatcher = request.getRequestDispatcher("result.jsp");
		requestDispatcher.forward(request, response);
	}
}
```

Pour rappel il existe deux façons d'effectuer une redirection (voir [Servlet Redirection]({{< relref "presentation_layer/servlet_jsp/servlet_redirection" >}})) :
- avec `sendRequest` mais ceci crée des appels supplémentaire (HTTP 302 pour signaler la redirection puis ensuite une HTTP GET de /result.jsp)
- ainsi le mieux est d'utiliser dans notre cas le `requestDispatcher`

## 2. Passer le résultat à la page
A la fin de la partie précédente, vous n'avez pas le résultat qui s'affiche sur la page. Pour arriver au résultat souhaiter :
- Utiliser la méthode `setAttribute()` pour stocker l'information dans la requête, puis utilisez un dispatcher pour rediriger vers la page JSP.

```java
@WebServlet("/convert")
public class TemperatureConverterServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Récupérer le paramètre ?temperature=30
        String temperatureParam = request.getParameter("temperature")

		// Appeler la logique métier (ici la conversion)
		Double conversion = TemperatureConverter.convertCelsiusToFahrenheit(Double.parseDouble(temperatureParam));

		request.setAttribute("result", Double.toString(conversion)); // ADD THIS LINE

		// Redirection vers la Vue : result.jsp
		// response.sendRedirect("result.jsp");
		RequestDispatcher requestDispatcher = request.getRequestDispatcher("result.jsp");
		requestDispatcher.forward(request, response);
	}
}
```

Note : le `setAttribute` est sur l'objet `request` car lors d'un forward, l'objet request est conservé et transmis à la JSP.
- Cela signifie également qu'on aura aussi accès aux paramètre `temperature`

=> Nous n'avons pas de redirection, si on regarde l'url nous restons sur `/convert?temperature=30`
```html
 <!-- result.jsp -->
<!DOCTYPE html>
<html>
<head>
    <title>Résultat de la conversion</title>
</head>
<body>
    <h1>Conversion de Température</h1>
    
    <!-- Récupération du paramètre "temperature" depuis l'URL -->
    <p>Température en Celsius : ${param.temperature}</p>

    <!-- Récupération du résultat depuis l'attribut "result" -->
    <p>Température en Fahrenheit : ${result}</p>

    <!-- Récupération du résultat depuis l'attribut "result" alternative 
         Rappel : nous pouvons executer du code Java dans les fichier JSP 
    -->
    <%
        String result = (String) request.getAttribute("result");
        if (result != null) {
    %>
        <p><%= result %></p>
    <%
        }
    %>
</body>
</html>
```

![pas de changement d'url](tp1_correction.png)


Les attributs définis avec `request.setAttribute()` sont stockés côté serveur et sont uniquement disponibles lors du traitement de la requête entre le Servlet et la JSP. Ils ne sont pas envoyés dans la réponse HTTP et donc invisibles pour le navigateur.
Par conséquent en utilisant une `response.sendRedirect("result.jsp")` on ne peut pas passer d'attribut (comme nous avons une nouvelle requête HTTP). Nous devons donc passer l'ensemble des données en paramètre (ou en cookie)

