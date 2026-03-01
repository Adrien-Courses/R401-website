+++
title = "TP2 E-boutique"
weight = 20
+++

> [!ressource] Ressource
> https://github.com/Adrien-Courses/R401-TP-e-boutique

## Objectifs
Nous souhaitons créer un site de e-boutique, permettant au utilisateur d'ajouter des articles dans leur panier
- afficher le panier
- calculer le montant total
- pouvoir supprimer un article du panier

### Résultat attendu
Sur la page catalogue, les articles sont affichés et un bouton permet de l'ajouter au panier

![catalogue](td_tp/tp2/images/catalogue.png)

Puis sur la page panier, on peut consulter les articles ajoutés

![panier](td_tp/tp2/images/panier.png)


## 1. Créer le catalogue
- Compléter les *TODO* dans `catalogue.jsp` afin de rajouter des boutons pour ajouter l'article au panier (servlet `/panier`)
	
	
### Aide
- Les boutons seront des formulaires HTML contenant l'ensemble des informations : id, nom et prix de l'article
- Puis compléter la méthode `doPost` de `PanierServlet`
	1. Récupérer les paramètre du formulaire
	2. Créer un objet `Article` avec
	3. Récupérer ou créer un panier en session
	4. Y ajouter l'article


<!--
protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// Récupérer les paramètres du formulaire
		String idStr = request.getParameter("id");
		String nom = request.getParameter("nom");
		String prixStr = request.getParameter("prix");

		// Valider et convertir les données
		try {
			int id = Integer.parseInt(idStr);
			double prix = Double.parseDouble(prixStr);

			// Créer un nouvel article
			Article article = new Article(id, nom, prix);

			// Récupérer la session
			HttpSession session = request.getSession();

			// Récupérer le panier existant ou en créer un nouveau
			Panier panier = (Panier) session.getAttribute("panier");
			if (panier == null) {
				panier = new Panier();
				session.setAttribute("panier", panier);
			}

			// Ajouter l'article au panier
			panier.ajouterArticle(article); // deux article sont égaux s'ils ont le même id

			// Ajouter un message de confirmation
			request.setAttribute("message", "L'article " + nom + " a été ajouté au panier.");

		} catch (NumberFormatException e) {
			request.setAttribute("erreur", "Données invalides.");
		}

		// Rediriger vers la page du catalogue ou panier
		request.getRequestDispatcher("catalogue.jsp").forward(request, response);

-->



## 2. Afficher le panier
L'affichage du panier est déjà codé dans `panier.jsf`. Le panier doit être stocké en session
- Expliquez pourquoi le panier doit être stocké en session
- Coder `doGet` pour stocker le panier en session

<!--
```java
	// Récupérer la session
	HttpSession session = request.getSession(false); // false pour ne pas créer de session si on tombe dessus en
	// premier

	if (session != null) {
		// Récupérer le panier s'il existe
		Panier panier = (Panier) session.getAttribute("panier");
		if (panier == null) {
			panier = new Panier();
			session.setAttribute("panier", panier);
		}
	}

	// Rediriger vers la page du panier
	request.getRequestDispatcher("panier.jsp").forward(request, response);
```
-->

## 3. Supprimer un article du panier

La dernier étape consiste à pourquoi supprimer un article du panier

Aide :
- Même principe que l'ajout d'un article au panier
- Attention, en HTML `method="delete"` n'existe pas, uniquement `get` et `post`

<!--
// formulaire caché avec une action=delete
<form action="${pageContext.request.contextPath}/panier" method="post">
	<input type="hidden" name="id" value="${article.id}">
	<input type="hidden" name="action" value="delete">
	<input type="submit" value="Supprimer">
</form>

et dans le doPost on redirige vers doDelete

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		String action = request.getParameter("action");

		if ("delete".equals(action)) {
			doDelete(request, response);
		}
		... ...
	}

-->