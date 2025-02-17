+++
title = "Servlet Redirection"
weight = 16
+++

> [!ressource] Ressource
> - [Redirection ou inclusion d'une ressource](https://blog.paumard.org/cours/servlet/chap03-servlet-interface-dispatcher.html)
> - [ 02 - 04 - Rediriger une Requête d'une Servlet vers une Autre ](https://youtu.be/KK8zvQ6p_zg?list=PLzzeuFUy_CniPG4Nj_4_lbfaejM2_ScCe)

Dans les applications web développées avec Java EE et les Servlets, la redirection est une technique permettant d’envoyer un utilisateur d’une page à une autre. Il existe deux types principaux de redirection :
- Redirection côté client (Client-Side Redirection)
- Redirection côté serveur (Server-Side Redirection)

## Redirection côté client : sendRedirect()
La méthode `sendRedirect()` de l'objet `HttpServletResponse` indique au navigateur de demander une nouvelle URL. Cela se fait en envoyant un code de statut `HTTP 302 (Found)` et un en-tête Location.

### Redirection externe
```java
@WebServlet("/RedirectExample")
public class RedirectExample extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        response.sendRedirect("https://www.google.com"); // Redirection vers un site externe
    }
}
```

### Redirection interne
```java
response.sendRedirect("newPage.jsp"); 
```

### Explication HTTP 302
![alt text](post_302.png)

1. Lorsqu'un serveur HTTP répond avec un code 302, il inclut un en-tête Location qui contient la nouvelle URL vers laquelle le client doit être redirigé.
2. Le navigateur interprète cet en-tête et effectue immédiatement une nouvelle requête HTTP GET (ligne d'après)

=> L’URL change dans la barre d’adresse du navigateur impliquant une nouvelle requête HTTP

## Redirection côté serveur : RequestDispatcher.forward()
La redirection côté serveur est effectuée avec `RequestDispatcher.forward()`. Ici, la même requête HTTP est transmise à une autre ressource (JSP, Servlet…).
- => **L’URL ne change pas dans la barre d'adresse du navigateur.**

### Redirection vers une JSP
```java
@WebServlet("/ForwardExample")
public class ForwardExample extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        RequestDispatcher dispatcher = request.getRequestDispatcher("newPage.jsp");
        dispatcher.forward(request, response);
    }
}
```

### Redirection vers une autre Servlet
```java
RequestDispatcher dispatcher = request.getRequestDispatcher("/AnotherServlet");
dispatcher.forward(request, response);
```

## 💡Bonnes pratiques
- Toujours éviter les redirections inutiles pour améliorer les performances.
- Utiliser `sendRedirect()` uniquement si une nouvelle requête est nécessaire.
Préférer `forward()` pour transférer des données entre pages d’une même application.