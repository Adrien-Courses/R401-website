+++
title = "Architecture"
weight = 20
+++

![](architecture_simple.png)

![](architecture_detaillee.png)

## Exemple
### Backend
Une api backend qui est exposé sur `localhost:8080/api/hello`
```java
@ApplicationPath("/api")
public class RestApplication extends Application {
    @Override
    public Set<Class<?>> getClasses() {
        return Collections.singleton(HelloResource.class);
    }
}

@Path("hello")
class HelloResource {
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public String sayHello() {
        return "{\"message\": \"Hello from JAX-RS!\"}";
    }
}
```

### Frontend
Une application ReactJS, qui est lancé sur le serveur `http://localhost:5173/` et qui contacte `localhost:8080/api/hello`

```js
import { useEffect, useState } from "react";

export default function App() {
  const [message, setMessage] = useState("");

  useEffect(() => {
    fetch("http://localhost:8080/api/hello")
      .then((response) => response.json())
      .then((data) => setMessage(data.message))
      .catch((error) => console.error("Error fetching data:", error));
  }, []);

  return (
    <div className="flex flex-col items-center justify-center min-h-screen bg-gray-100">
      <h1 className="text-2xl font-bold text-blue-500">
        {message || "Loading..."}
      </h1>
    </div>
  );
}
```

### Conséquence
Il faut une gestion des routes côté frontend et côté backend, mais elles peuvent se nommer différent.
L'avantage est que le backend va fournir un ensemble de route pour récupérer, insérer des données et le frontend va les exploiter, en agréger plusieurs pour n'en exposer qu'une seule.

Par exemple, une page pour accéder à une commande sera accessible depuis `GET monsite.fr/commande` (front-end) mais plusieurs route backend seront appeler :
- premièrement `GET /commande/12345` pour récupérer les informations de la commande
- puis un nouvel appel pour récupérer les informations de l'utilisateur `GET /utilisateurs/567`
- et un autre pour la facture si la commande a été validé `GET /facture/9876`
- ...

Exemple avec HATEOAS
```json
{
  "id": 12345,
  "date": "2025-03-02T14:30:00Z",
  "total": 150.75,
  "statut": "PAYÉ",
  "_links": {
    "self": {
      "href": "https://api.exemple.com/commandes/12345"
    },
    "utilisateur": {
      "href": "https://api.exemple.com/utilisateurs/567"
    },
    "facture": {
      "href": "https://api.exemple.com/factures/9876"
    }
  }
}
```

> Note, on aurait pu passer l'ensemble du contenu des objets Utilisateur et Facture, mais ici on a privilégier l'approche HATEOAS.