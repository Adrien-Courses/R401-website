+++
title = "Servlet"
weight = 10
+++

## javax.servlet et javax.servlet.http

![alt text](servlet.png)

## Définition
> [!definition] Définition
> Une servlet est un programme qui s'exécute côté serveur en tant qu'extension du serveur. Elle reçoit une requête du client, elle effectue des traitements et renvoie le résultat.

Le cycle de vie d'une servlet en Java repose sur trois méthodes clés

`init()`
- Appelée une seule fois lors de l'instanciation de la servlet par le serveur.

`service()`
- Gère les requêtes entrantes et envoie des réponses au client.
- Redirige les requêtes vers doGet(), doPost(), etc., en fonction du type de requête HTTP.

`destroy()`
- Appelée juste avant la destruction de la servlet par le serveur.
- Permet de libérer les ressources allouées dans init() (ex: fermeture d'une connexion à une base de données).

## Le fonctionnement d'une servlet
> [!ressource] Ressource
> https://www.jmdoudoux.fr/java/dej/chap-servlets.htm


1. Le serveur reçoit du navigateur la requête http qui a recours à une servlet
2. Si c'est la première sollicitation de la servlet, le serveur l'instancie. Les servlets sont stockées (sous forme de fichiers .class) dans un répertoire particulier du serveur. Ce répertoire dépend du serveur d'applications utilisé. La servlet reste en mémoire jusqu'à l'arrêt du serveur. Certains serveurs d'applications permettent aussi d'instancier des servlets dès le lancement du serveur.
    Au fil des requêtes, la servlet peut être appelée par plusieurs threads lancés par le serveur. Ce principe de fonctionnement évite d'instancier un objet de type servlet à chaque requête et permet de maintenir un ensemble de ressources actives telles qu'une connexion à une base de données.
3. Le serveur crée un objet qui représente la requête http ainsi que l'objet qui contiendra la réponse et les envoie à la servlet
4. La servlet crée dynamiquement la réponse sous forme de page html transmise par un flux dans l'objet contenant la réponse. La création de cette réponse utilise bien sûr la requête du client mais aussi un ensemble de ressources incluses sur le serveur telles que des fichiers ou des bases de données.
5. Le serveur récupère l'objet réponse et envoie la page html au client.

```java
public class TestServlet implements Servlet {
  private ServletConfig cfg;

  public void init(ServletConfig config) throws ServletException {
     cfg = config;
  }

  public ServletConfig getServletConfig() {
    return cfg;
  }

  public String getServletInfo() {
    return "Une servlet de test";
  }	

  public void destroy() {
  }

  public void service (ServletRequest req,  ServletResponse res ) 
  throws ServletException, IOException  {
    res.setContentType( "text/html" );
    PrintWriter out = res.getWriter();
    out.println( "<HTML>" );
    out.println( "<HEAD>");
    out.println( "<TITLE>Page generee par une servlet</TITLE>" );
    out.println( "</HEAD>" );
    out.println( "<BODY>" );
    out.println( "<H1>Bonjour</H1>" );
    out.println( "</BODY>" );
    out.println( "</HTML>" );
    out.close();
  }
}
```