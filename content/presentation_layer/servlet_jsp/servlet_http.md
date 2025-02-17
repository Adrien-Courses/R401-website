+++
title = "Servlet HTTP"
weight = 15
+++

> [!ressource]
> [ 02 - 03 - Ecrire une Première Servlet ](https://youtu.be/0v3X3hGWNds?list=PLzzeuFUy_CniPG4Nj_4_lbfaejM2_ScCe)

> [!definition] Note
> Les servlets sont conçues pour agir selon un modèle de requête/réponse. Tous les protocoles utilisant ce modèle peuvent être utilisés : http, ftp, etc ...

Mais l'un des usage principale des servlet est la création de pages HTML dynamiques. On parle de pages dynamique car contrairement aux pages HTML statiques qui sont écrites une fois et servies telles quelles aux utilisateurs, une servlet Java (comme une classe dérivée de HttpServlet) génère le contenu HTML en fonction des requêtes de l'utilisateur et des données disponibles.

## HttpServlet
La méthode `service()` héritée de `HttpServlet` appelle l'une ou l'autre de ces méthodes en fonction du type de la requête http :
- une requête `GET` : c'est une requête qui permet au client de demander une ressource
- une requête `POST` : c'est une requête qui permet au client d'envoyer des informations issues par exemple d'un formulaire


```java
public class TestServlet extends HttpServlet {

   public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
      res.setContentType("text/html");
      ServletOutputStream out = res.getOutputStream();
      out.println("<HTML>\n");
      out.println("<HEAD>\n");
      out.println("<TITLE>Bonjour</TITLE>\n");
      out.println("</HEAD>\n");
      out.println("<BODY>\n");
      out.println("<H1>Bonjour</H1>\n");
      out.println("</BODY>\n");
      out.println("</HTML>");
   } 
}
```

Note, cette servlet est très simple mais nous pourrions faire en sorte qu'elle appelle de la logique métier qui va chercher des informations stockées en base de données par exemple.

## Paramètres
Les méthodes `doXXX` ont toutes deux paramètres : 
- `javax.servlet.http.HttpServletRequest`
- `javax.servlet.http.HttpServletResponse` qui représentent respectivement la requête HTTP entrante et la réponse renvoyée par le serveur.

### Opérations sur HttpServletRequest
- `HttpServletRequest.getParameter(String)` qui permet de récupérer le valeur d’un paramètre d’une requête GET ou POST

### Opérations sur HttpServletResponse
- `HttpServletResponse.getWriter()` : Retourne un objet de type PrintWriter qui permet d’écrire la réponse dans le flux de sortie.
- `HttpServletResponse.getOutputStream()` : Retourne un objet représentant le flux de sortie en mode binaire. Cette méthode est utile lorsque la réponse générée est au format binaire (comme une image par exemple).

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
   // Opérations sur HttpServletRequest
   req.setCharacterEncoding("utf-8");
   String name = req.getParameter("name");

   // Opération sur HttpServletResponse
   resp.setContentType("text/plain");
   resp.setCharacterEncoding("utf-8");
   resp.getWriter().write("Hello " + name + "!");
}
```

## Exercice : afficher une message personnalisé
Créer une servlet Java qui affiche un message personnalisé en fonction du paramètre passé dans l'URL.

1. Créer un projet Java EE avec un serveur Tomcat.
2. Créer une servlet Java nommée MessageServlet.
3. Configurer la servlet dans le fichier web.xml ou avec l’annotation @WebServlet.
4. Récupérer un paramètre depuis l'URL (ex: nom).
5. Afficher un message de bienvenue en réponse.