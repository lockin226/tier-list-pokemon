# Tier list Pokémon

**En ligne : <https://lockin226.github.io/tier-list-pokemon/>**

Un site pour noter les 1212 Pokémon — formes régionales, Méga-Évolutions et Gigamax
comprises — et en tirer automatiquement des tier lists des générations, des types et
de tous les Pokémon. Avec un mode tournoi en duel et un export de chaque classement en image.

Tout tient dans un seul fichier `index.html`. Il n'y a rien à installer, rien à compiler,
aucune dépendance à gérer. Les noms et les types des 1212 Pokémon sont écrits en dur dans
le fichier ; seules les images sont chargées depuis internet.

---

## Modifier le site

Le site est publié par GitHub Pages depuis la branche `main`. Il suffit de pousser :

```
git add index.html
git commit -m "ce que j'ai changé"
git push
```

La mise en ligne prend une à deux minutes. Pour travailler en local avant de publier,
ouvre simplement `index.html` dans un navigateur — tout fonctionne sauf la sauvegarde
dans un fichier, que certains navigateurs réservent aux vraies adresses web.

Attention si tu changes l'adresse du site : elle est écrite en dur à cinq endroits,
dans `index.html` (balises `canonical`, `og:image` et données structurées),
dans `robots.txt` et dans `sitemap.xml`.

---

## Ce que contient le dossier

| Fichier | Rôle |
| --- | --- |
| `index.html` | Le site entier : interface, données des 1212 Pokémon, logique, styles |
| `favicon.svg` | La petite icône affichée dans l'onglet du navigateur |
| `og.png` | L'aperçu 1200×630 affiché quand on partage le lien |
| `robots.txt` | Autorise les moteurs de recherche et leur indique le plan du site |
| `sitemap.xml` | Le plan du site, à re-soumettre après un changement d'adresse |

---

## Où sont enregistrées les notes

Dans le navigateur du visiteur, et nulle part ailleurs. Aucun serveur ne les reçoit,
il n'y a pas de compte, pas de cookie de suivi, pas de statistiques.

Trois couches se relaient, de la plus discrète à la plus sûre :

1. **localStorage** — écrit à chaque clic, instantané, survit à une fermeture brutale.
2. **IndexedDB** — filet de sécurité si la première couche est vidée ou refusée.
3. **Un fichier `.json` sur le disque** — facultatif, proposé en bas de la page Résultats,
   sur Chrome et Edge seulement. Le site le réécrit à chaque note.

Au chargement, les deux premières couches sont relues et la plus récente l'emporte.
Les boutons *Exporter* et *Importer* permettent de transporter ses notes d'un appareil
à l'autre.

Attention : ces stockages sont liés à l'adresse du site. Des notes prises en ouvrant le
fichier en local ne se retrouveront pas sur la version en ligne — il faut passer par
l'export puis l'import.

---

## Les images

Elles viennent de [PokéAPI](https://pokeapi.co) via le réseau de diffusion **jsDelivr**,
et non de `raw.githubusercontent.com` en direct : GitHub déconseille cet usage et le limite
en débit, ce qui ferait tomber les images dès qu'il y a du monde sur le site.

jsDelivr renvoie aussi l'en-tête d'autorisation entre domaines, ce dont dépend
l'export des tier lists en image : sans lui, le navigateur refuserait d'enregistrer
une image contenant des sprites venus d'ailleurs.

---

## Référencement

Le nécessaire est en place : titre et description, un vrai `<h1>`, données structurées
`WebApplication`, `robots.txt`, `sitemap.xml`, aperçu de partage et adresse canonique.

Reste une étape qui demande un compte : inscrire le site sur
[Google Search Console](https://search.google.com/search-console), y soumettre
`sitemap.xml` et demander l'indexation. Sans ça, Google peut mettre des semaines
à découvrir le site tout seul.

---

## Et ensuite : les comptes

Pour que chacun retrouve ses notes sur n'importe quel appareil, il faudra un service
d'authentification et une base de données. [Supabase](https://supabase.com) convient bien :
offre gratuite, base PostgreSQL et comptes email + mot de passe intégrés.

Le code s'y prête : la sauvegarde est déjà isolée dans `snapshot()`, `save()`, `flush()`
et `restore()`, et gère plusieurs couches en parallèle avec un horodatage pour départager
les versions. Le nuage serait une couche de plus au même endroit.

Deux points à ne pas oublier le jour où ça arrivera :

- **Ne jamais coder soi-même la vérification des mots de passe.** Un site statique en est
  de toute façon incapable de façon sûre, puisque tout son code est lisible par les visiteurs.
- **Le RGPD s'applique** dès qu'on collecte des adresses mail : page de confidentialité,
  bouton de suppression de compte, et une région européenne à la création du projet.

---

## Mentions

Site de fans sans but lucratif, sans aucun lien avec Nintendo, Game Freak ou
The Pokémon Company. Pokémon et les noms des créatures sont leurs marques déposées.
Noms français, types et images proviennent de [PokéAPI](https://pokeapi.co).
