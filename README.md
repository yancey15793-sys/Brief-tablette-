# Briefeed — Édition Tablette

Lecteur de flux RSS pour iPad / tablette en position horizontale, dérivé de **Briefeed iPhone**. Reprend la logique éditoriale de la version mobile : typographie Fraunces / Merriweather / JetBrains Mono, accent jaune `#FFDB03` et rouge éditorial `#EB001B`, animations grayscale-to-color sur courbe spring iOS.

Un seul fichier HTML autonome, lançable directement ou hébergeable sur GitHub Pages.

## Aperçu

- **Format cible** : iPad horizontal / écran large (min-width 1100px)
- **Fond** : noir profond `#07090b`
- **Sidebar** : accordéon Briefeed avec dossiers thématiques
- **Briefing du jour** : carrousel Apple TV+ deux rangées défilantes
- **Carte vedette** : photo grayscale qui se révèle en couleur au survol
- **Trois vues** : Grille (3 colonnes), Maçonnerie (Pinterest-style), Liste éditoriale
- **Timeline flottante** : navigation entre les jours
- **Modal de lecture** : Readability.js, typographie Merriweather, vue étendue à 92 vh

## Modifications appliquées depuis la version originale

### Typographie éditoriale
- **Fraunces** (variable, optical size 30–144) pour tous les titres : logo, "Bonsoir", titres de cartes, headlines modal
- **Merriweather** pour le texte d'article en mode lecture
- **JetBrains Mono** pour toutes les métadonnées : dates relatives, libellés de section, "VOTRE BRIEFING QUOTIDIEN", compteurs
- **Inter** conservé pour le corps de l'interface

### Système de couleurs unifié
- Tokens CSS centralisés (`--bg`, `--accent`, `--accent-2`, `--surface`, etc.)
- Accent primaire jaune `#FFDB03` (CTA, état actif)
- Accent secondaire rouge éditorial `#EB001B` (source en majuscules dans la modal)
- Hiérarchie de texte à quatre niveaux (`--text-primary` à `--text-quaternary`)

### Effet signature grayscale-to-color
Appliqué à toutes les images cliquables (featured card, marquee, grille, maçonnerie, liste, modal preview). Filtre `grayscale(100%) contrast(0.85) brightness(0.88)` → `grayscale(0%)` avec transition `0.8s cubic-bezier(0.16, 1, 0.3, 1)`.

### Espace blanc augmenté
La structure HTML des cartes est intacte. Ce qui a été augmenté :
- Gap de la grille : 42px / 20px → **56px / 28px**
- Gap de la maçonnerie : 20px → **28px**
- Margin entre maçonnerie cards : 20px → **36px**
- Padding des list-items : 22px → **32px**
- Padding intérieur des cartes (`.card-body`, `.masonry-body`) : 10/12px → **18/20px**
- Gap entre sections : 60px → **80px**
- Hauteur de l'image en liste : 110px → **124px**

### Animations spring iOS
Courbe `cubic-bezier(0.16, 1, 0.3, 1)` partout (au lieu de `0.25, 0.1, 0.25, 1`), pour un rebond plus naturel. Hover des cards : `translateY(-4px)`. Hover des list-items : `padding-left: 16px`.

### Modal repensée
- Titre en Fraunces 30 → 44 px en vue étendue (optical size 144)
- Description en Merriweather italique en lecture
- Source en JetBrains Mono majuscules `#EB001B`
- Bouton primaire jaune (`var(--accent)`) au lieu de gris uniforme

## Structure du dépôt

```
.
├── index.html      # Application complète (autonome)
└── README.md       # Ce fichier
```

## Utilisation

### En local
Ouvrir `index.html` dans un navigateur. Aucune dépendance à installer.

### Sur GitHub Pages
1. Créer un repo et y déposer `index.html` à la racine
2. Settings → Pages → Source : `main` branch / `/ (root)`
3. Attendre l'URL `https://<user>.github.io/<repo>/`

### En raccourci iOS
Comme pour Briefeed iPhone : créer un raccourci Safari qui ouvre l'URL GitHub Pages en plein écran.

## Sources RSS incluses

24 flux répartis en six catégories :
- **Internationale** : Le Monde, Libération, France 24, BBC, Le Monde Diplo, Courrier International, RFI
- **Sport** : L'Équipe, Foot Mercato, RMC Sport, ESPN, The Athletic
- **Technologie** : The Verge, Wired, TechCrunch, Numerama, Ars Technica, Frandroid
- **Économie** : Les Échos, BFM Business
- **Culture** : Télérama, Konbini
- **Science** : Futura Sciences, Science Daily

Pour ajouter un flux : éditer le tableau `RSS_FEEDS` au début du `<script>`.

## Dépendances externes (via CDN)

- Fonts Google (Inter, Fraunces, Merriweather, JetBrains Mono)
- Meyer CSS Reset
- Readability.js (Mozilla, 0.4.4) — extraction du contenu d'article
- Proxys RSS : rss2json.com, allorigins.win, corsproxy.io (cascade)

## Notes techniques

- Pas de framework, pas de build : vanilla JS, un seul fichier
- Chargement RSS via cascade de 3 proxies CORS
- Cache image en mémoire (`Map`) pour éviter les re-fetchs
- Enrichissement d'images en arrière-plan (10 stratégies : OG, Twitter, JSON-LD, sélecteurs CSS)
- Auto-refresh des flux toutes les 5 minutes

---

*Repris de la logique éditoriale de Briefeed iPhone : noir profond, photos en niveaux de gris au repos, mise en couleur à l'interaction, typographie variable serif sur métadonnées mono.*
