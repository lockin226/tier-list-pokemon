# Tier list Pokémon

Un site pour noter les 1212 Pokémon — formes régionales, Méga-Évolutions et Gigamax
comprises — et en tirer automatiquement des tier lists des générations, des types et
de tous les Pokémon. Avec un mode tournoi en duel et un export de chaque classement en image.

Tout tient dans un seul fichier `index.html`. Il n'y a rien à installer, rien à compiler,
aucune dépendance à gérer. Les noms et les types des 1212 Pokémon sont écrits en dur dans
le fichier ; seules les images sont chargées depuis internet.

---

## Mettre le site en ligne

### Avant toute chose : remplacer `VOTRE-ADRESSE`

Ouvre `index.html` et cherche `VOTRE-ADRESSE` (deux occurrences, tout en haut du fichier).
Remplace-les par l'adresse réelle du site une fois que tu la connaîtras, par exemple
`tierlist-pokemon.pages.dev`.

Ces deux lignes servent à l'aperçu affiché quand quelqu'un colle le lien sur Discord,
WhatsApp ou un réseau social. Elles doivent contenir une adresse complète : ces services
lisent le HTML brut sans exécuter le script, donc ils ne peuvent pas la deviner.
Le site fonctionne même si tu oublies, seul l'aperçu de partage restera vide.

### Option A — Cloudflare Pages (recommandé)

Bande passante illimitée, gratuit, et le site se met à jour tout seul à chaque `git push`.

1. Crée un dépôt sur GitHub et pousse ce dossier dedans.
2. Va sur [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** →
   **Create** → **Pages** → **Connect to Git**.
3. Choisis ton dépôt. Laisse la commande de build **vide** et le dossier de sortie
   sur la racine : il n'y a rien à construire.
4. **Save and Deploy.** Le site est en ligne en une minute sur `<nom>.pages.dev`.

### Option B — GitHub Pages

1. Pousse ce dossier sur GitHub.
2. Dans le dépôt : **Settings** → **Pages** → Source : **Deploy from a branch**,
   branche `main`, dossier `/ (root)`.
3. Le site paraît sous `<pseudo>.github.io/<dépôt>` au bout de quelques minutes.

### Un nom de domaine (facultatif)

Une dizaine d'euros par an chez n'importe quel registrar. Les deux hébergeurs ci-dessus
acceptent un domaine personnalisé gratuitement, certificat HTTPS compris.

---

## Ce que contient le dossier

| Fichier | Rôle |
| --- | --- |
| `index.html` | Le site entier : interface, données des 1212 Pokémon, logique, styles |
| `favicon.svg` | La petite icône affichée dans l'onglet du navigateur |
| `og.png` | L'aperçu 1200×630 affiché quand on partage le lien |
| `README.md` | Ce fichier |

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
