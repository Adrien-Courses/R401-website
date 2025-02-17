+++
title = "Architecture web application"
weight = 10
+++

En JavaEE (Jakarta EE), une application web est généralement empaquetée sous la forme d’un fichier .war (Web Application Archive), qui est ensuite déployé sur un serveur d’application JavaEE comme Tomcat, WildFly, GlassFish, etc..

## Définition d’un Web Component

Un Web Component en JavaEE regroupe tous les fichiers nécessaires pour exécuter une application web, notamment :

- Pages HTML, JSP, images
- Servlets et classes Java
- Bibliothèques tierces (.jar)
- Fichiers de configuration (web.xml, propriétés, Tag Libraries, etc.)

Le tout est packagé dans un fichier `.war` et déployé dans le répertoire `webapps/` du serveur.

## Structure d’une Web Application Archive (.war)

```
MyWebApp.war
│── index.html
│── logo.png
│── login.jsp
│── applets.jar
│── midlets.jar
│
├── WEB-INF/
│   ├── web.xml  (Fichier de configuration principal)
│   │
│   ├── classes/  (Classes Java compilées)
│   │   ├── com/
│   │   │   ├── myapp/
│   │   │   │   ├── controllers/
│   │   │   │   │   ├── LoginServlet.class
│   │   │   │   ├── models/
│   │   │   │   │   ├── User.class
│   │   │   │   ├── utils/
│   │   │   │   │   ├── DatabaseUtil.class
│   │
│   ├── lib/  (Librairies tierces)
│   │   ├── mysql-connector-java.jar
│   │   ├── jakarta.servlet-api.jar
│   │   ├── jsf-api.jar
│   │
│   ├── tlds/  (Tag Libraries)
│   │   ├── custom-tags.tld
│
└── META-INF/
    ├── manifest.mf

```

### WEB-INF/web.xml (Fichier de configuration du déploiement)
Définit la configuration de l’application (mapping des servlets, filtres, sécurité, etc.).

```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         version="3.0">

    <display-name>MyWebApp</display-name>

    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>com.myapp.controllers.LoginServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>

    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>

</web-app>
```

### WEB-INF/classes/
Contient les classes compilées (.class) des servlets, JavaBeans et autres classes utilisées.

```java
package com.myapp.controllers;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        if ("admin".equals(username) && "1234".equals(password)) {
            response.sendRedirect("welcome.jsp");
        } else {
            response.sendRedirect("login.jsp?error=1");
        }
    }
}
```

### WEB-INF/lib/
Contient les fichiers .jar nécessaires au bon fonctionnement de l’application. Par exemple
- `mysql-connector-java.jar` → Pilote JDBC pour MySQL
- `jakarta.servlet-api.jar` → API Servlet
- `jsf-api.jar` → JavaServer Faces