+++
title = "Sessions et cookies"
weight = 30
+++

> [!ressource] Ressource
> Pourquoi les sessions et cookies ? [02 - 06 - Utiliser les cookies pour créer des sessions en HTTP](https://youtu.be/u6YmmdoA5x0?list=PLzzeuFUy_CniPG4Nj_4_lbfaejM2_ScCe)

Le protocole HTTP est dit *déconnecté*. C'est-à-dire que si depuis le navigateur on refait la même requête le serveur a "oublié" que nous réalisons notre deuxième requête (stateless).

> [!definition] Session
> Une session est un ensemble de requête dont le serveur se rappelle qu'elles appartiennent à un même client

Par exemple, lorsqu'on ajoute sur un site de e-commerce des articles à notre panier alors comme nous n'avons pas de notion de session avec le protocole HTTP nous n'aurons qu'un seul article dans notre panier. Le serveur a oublié nos requêtes précédentes. Ceci étant assez contraignant, nous utilisons des cookies pour "simuler" les sessions.

Lors de la réponse, en plus de nous renvoyer le html et css, le site de e-commerce renvoie un fichier contenant les informations de notre panier. Puis lorsqu'on va réaliser une nouvelle requête, en plus d'envoyer l'article à ajouter nous envoyons également le fichier contenant les articles déjà ajoutés.

## Session avec l'API Servlet
Pour enregistrer les informations en "session" on utilise

```java
HttpSession session = request.getSession();
session.setAttribut("clé", valeur);
```

Puis lorsqu'on souhaite récupérer un objet en session
```java
HttpSession session = request.getSession();
Object object = sessions.getAttribut("clé");
```

## Exemple
En nous appuyant sur le formulaire créé [précédemment]({{< relref "presentation_layer/formulaire/" >}}), nous souhaitons concaténer les noms d'une même session. La différence est dans le `doPost` qui doit gérer la session

```java
@WebServlet("/inscription")
public class InscriptionServlet extends HttpServlet {

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// Récupération des paramètres du formulaire
		String nomParam = request.getParameter("nom");

		Utilisateur user = (Utilisateur) request.getSession().getAttribute("user");
		if (user == null) {
			user = new Utilisateur();
			request.getSession().setAttribute("user", user);
		}

		user.nom = user.nom + " " + nomParam;

		request.setAttribute("message", "Inscription réussie pour " + user.nom);

		// Redirection vers la page de confirmation
		RequestDispatcher dispatcher = request.getRequestDispatcher("confirmation.jsp");
		dispatcher.forward(request, response);
	}
}
```

1. L'utilisateur interroge pour la première fois le serveur
2. Si aucune session existe il en crée une puis retourne un `JSESSIONID` cookie au client
3. Lors d'une nouvelle requête on fournit en plus le `JSESSIONID`


Ci-dessous, depuis le navigateur on a renseigné le nom "Adrien" puis ensuite via le JSESSIONID depuis `curl` nous avons rajouté "CAUBEL"
![alt text](navigateur.png)

![alt text](curl.png)