+++
title = "Web.xml"
weight = 17
+++

Le fichier web.xml est un fichier de configuration utilisé dans les applications web basées sur Java EE pour définir le comportement des servlets, des filtres et d'autres composants web. Il est situé dans le dossier `WEB-INF/` de l'application.


```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
         http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

    <!-- Déclaration de la servlet -->
    <servlet>
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>com.example.MyServlet</servlet-class>
    </servlet>

    <!-- Mapping de l'URL vers la servlet -->
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/MyServlet</url-pattern>
    </servlet-mapping>

</web-app>
```

## Configuration des Servlets dans web.xml
Nous pouvons configurer les servlets soit
- dans le fichier `web.xml`
- en annotant la classe avec `@WebServlet`

**Déclaration d'une servlet**
```xml
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
</servlet>
```

**Mapping d'URL à la servlet**
```xml
<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>/MyServlet</url-pattern>
</servlet-mapping>
```

Lorsqu'un utilisateur accède à `http://localhost:8080/MonApp/MyServlet`, il est dirigé vers `com.example.MyServlet`