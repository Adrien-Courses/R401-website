+++
title = "Couche présentation"
weight = 10
+++

Les spécifications JEE de ce groupe reçoivent la requête du serveur web
et renvoient la réponse, généralement au format HTML. Toutefois, il est également possible de ne renvoyer que les données de la couche de présentation au format JSON (e.g. API REST)

![presentation layer](../images/presentation.png)

J'aimerai également faire le lien avec l'architecture MVC qui sépare la logique métier de l'affichage.
- Contrôleur : est chargé de recevoir les requêtes clients et les transmets aux composants chargés de traiter les données. Ici c'est le rôle de la Servlet
- Vue : la logique de présentation est géré par la vue, chez nous c'est la JSP
- Modèle : englobe la logique métier, réalisé par la couche métier

## Java Servlet
Les servlets Java sont des modules côté serveur, généralement utilisés pour traiter une demande et renvoyer une réponse dans les applications web. Les servlets sont utiles pour traiter les demandes qui
qui ne génèrent pas de grandes réponses en HTML. Elles sont généralement utilisées comme contrôleurs
dans les frameworks MVC (Model View Controller).

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    
    out.println("<html>");
    out.println("<head>");
    out.println("<title>Hello Servlet</title>");
    out.println("</head>");
    out.println("<body>");
    out.println("<h1>Hello from my first servlet!</h1>");
    out.println("<p>The time is: " + new java.util.Date() + "</p>");
    out.println("</body>");
    out.println("</html>");
}
```

## Java Server Pages
Comme les servlets, les JSP sont également des modules côté serveur utilisés pour traiter les requêtes web.
Les JSP (Java Server Pages) sont parfaits pour traiter les requêtes qui génèrent de grandes réponses en HTML
de grande taille. Dans les pages JSP, le code Java ou les balises JSP peuvent être mélangés à d'autres codes HTML, tels que des balises HTML, du JavaScript et du CSS.

```html
<body>
    <h1>Welcome to My JSP Page</h1>
    
    <p>This is a simple JSP example.</p>
    
    <% 
    // Java code block
    Date currentDate = new Date();
    %>
    
    <p>Current date and time: <%= currentDate %></p>
    
    <h2>A simple loop:</h2>
    <ul>
    <% for(int i = 1; i <= 5; i++) { %>
        <li>Item <%= i %></li>
    <% } %>
    </ul>
</body>
</html>
```

## Java Server Faces
