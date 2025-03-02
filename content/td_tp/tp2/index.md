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

http://localhost:8080/R401-TP-e-boutique/

### Endpoint /catalogue
![catalogue](catalogue.png)

- Lorsqu'on clique sur ajouté, on reste sur la page mais le nombre d'article augmente (ici 3 articles)

### Endpoint /afficherPanier
![panier](panier.png)

## 1. Créer le Model
Définir le Model de notre application, c'est-à-dire les classes métiers et les opérations métiers *ajouter un article* et *supprimer un article*

- Deux articles sont égaux, si leur id sont identiques

## 2. Définir l'endpoint /ajouterAuPanier
Cet endpoint sera appeler depuis le bouton "Ajouter au panier" présent sur chaque article du catalogue.

- Quels attributs va-t-on récupérer de la requête ?
- Comment va-t-on ajouter au panier ? et faire en sorte que le panier ne soit pas perdu
- Lorsqu'on ajoute au panier, on reste sur la page catalogue

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

## 3. Afficher le catalogue
Le catalogue est représenté par le fichier `catalogue.jsp`. Nous avons 3 articles de disponibles.

- Comment créer nos articles ?
  - Comment appeler l'endpoint `/ajouterAuPanier` ? 
  - Quelles informations passer ?

<!--
Pour chaque article, on définit un formulaire avec des champs caché

<div class="produit">
    <h3>Smartphone XYZ</h3>
    <p>Prix: 599.99 euro</p>
    <form action="${pageContext.request.contextPath}/ajouterAuPanier" method="post">
        <input type="hidden" name="id" value="1">
        <input type="hidden" name="nom" value="Smartphone XYZ">
        <input type="hidden" name="prix" value="599.99">
        <input type="submit" value="Ajouter au panier">
    </form>
</div>

-->

Puis,
- regarder l'utilisation dans `catalogue.jsp` de l'objet `sessionScope` pour récupérer le nombre d'article dans le panier

<!--
    <div>
        <a href="${pageContext.request.contextPath}/afficherPanier">
            Voir mon panier 
            <c:if test="${not empty sessionScope.panier}">
                (${sessionScope.panier.nombreArticles} article(s))
            </c:if>
        </a>
    </div>
-->

## 4. Définir l'endpoint /afficherPanier
La page pour afficher le panier `panier.jsp` est déjà existante.
- Créer la servlet pour l'appeler

## 5. Définir l'endpoint /supprimerArticle
Depuis la page `panier.jsp` on peut supprimer la ligne article. Créer l'endpoint

<!--
// formulaire caché
<form action="${pageContext.request.contextPath}/supprimerArticle" method="post">
    <input type="hidden" name="id" value="${article.id}">
    <input type="submit" value="Supprimer">
</form>

Puis via l'id on supprime l'article (pas besoin de ce soucier des qte car la qte est recalculé dynamiquement)


-->