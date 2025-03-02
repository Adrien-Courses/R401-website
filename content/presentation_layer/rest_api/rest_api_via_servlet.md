+++
title = "Rest API via Servlet"
weight = 10
+++

> [!ressource] Ressources
> - [https://github.com/Adrien-Courses/R401-EXO-restapi-with-servlet](https://github.com/Adrien-Courses/R401-EXO-restapi-with-servlet)


## GET
- préciser le type du contenu lu
- préciser le code de retour (200, 404)
- si un `id` est précisé chercher l'utilisateur
- sinon retourner l'ensemble des utilisateurs

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String pathInfo = req.getPathInfo();

    resp.setContentType("application/json");
    resp.setCharacterEncoding("UTF-8");

    if (pathInfo == null || pathInfo.equals("/")) {
        // Return all users
        Collection<User> users = userDB.values();
        resp.getWriter().write(objectMapper.writeValueAsString(users));
    } else {
        // Get a specific user by ID
        try {
            int userId = Integer.parseInt(pathInfo.substring(1));
            User user = userDB.get(userId);
            if (user != null) {
                resp.getWriter().write(objectMapper.writeValueAsString(user));
            } else {
                resp.setStatus(HttpServletResponse.SC_NOT_FOUND);
                resp.getWriter().write("{\"error\":\"User not found\"}");
            }
        } catch (NumberFormatException e) {
            resp.setStatus(HttpServletResponse.SC_BAD_REQUEST);
            resp.getWriter().write("{\"error\":\"Invalid user ID\"}");
        }
    }
}
```

## POST
- préciser le type du contenu retourné
- préciser le code de retour (201)
- retourner l'utilisateur créé

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    BufferedReader reader = req.getReader();
    User newUser = objectMapper.readValue(reader, User.class);

    newUser.setId(userIdCounter++);
    userDB.put(newUser.getId(), newUser);

    resp.setStatus(HttpServletResponse.SC_CREATED);
    resp.setContentType("application/json");
    resp.getWriter().write(objectMapper.writeValueAsString(newUser));
}
```

```
curl -X POST http://localhost:8080/R401-EXO-restapi-with-servlet/users \
     -H "Content-Type: application/json" \
     -d '{"name": "John Doe", "email": "john@example.com"}'
```

### req.getReader()
```java
BufferedReader reader = req.getReader(); // Retrieves the body of the request as character data using a BufferedReader.
User newUser = objectMapper.readValue(reader, User.class); 
```