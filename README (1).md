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

## Structure du dépôt

```
briefeed-tablette/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions → déploiement automatique
├── .gitignore                  # Exclusions standards (macOS, IDE, logs)
├── 404.html                    # Page d'erreur stylée
├── LICENSE                     # MIT
├── README.md                   # Ce fichier
├── apple-touch-icon.svg        # Icône iOS / iPadOS
├── favicon.svg                 # Favicon onglet navigateur
├── index.html                  # Application complète (autonome)
├── manifest.json               # PWA → ajout à l'écran d'accueil
└── robots.txt                  # Autorisation indexation
```

## Déploiement sur GitHub Pages — pas à pas

### Option 1 — Via l'interface GitHub (la plus simple)

1. **Créer un nouveau repo** sur github.com (nom au choix, ex. `briefeed-tablette`)
2. **Upload tous les fichiers** (drag & drop le contenu du dossier dans l'interface GitHub)
3. **Activer Pages** : Settings → Pages → Source : *Deploy from a branch* → Branch : `main` / `/ (root)` → Save
4. **Attendre 1–2 minutes**, puis ouvrir l'URL affichée : `https://<ton-username>.github.io/briefeed-tablette/`

### Option 2 — Avec le workflow GitHub Actions (déploiement auto)

Le fichier `.github/workflows/deploy.yml` est déjà inclus et redéploie automatiquement à chaque push.

1. **Créer le repo** sur GitHub
2. **Cloner et push** :
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Briefeed Édition Tablette"
   git branch -M main
   git remote add origin https://github.com/<username>/briefeed-tablette.git
   git push -u origin main
   ```
3. **Activer Pages** : Settings → Pages → Source : *GitHub Actions*
4. Le workflow s'exécute à chaque `git push` sur `main`

### Option 3 — Avec un domaine personnalisé

1. Ajouter un fichier `CNAME` à la racine contenant uniquement `tondomaine.com`
2. Settings → Pages → Custom domain : `tondomaine.com` → Save
3. Configurer le DNS chez ton registrar (ALIAS ou CNAME vers `<username>.github.io`)

## Utilisation

### En local
Ouvrir `index.html` dans un navigateur. Aucune dépendance à installer. Note : certains proxies RSS peuvent bloquer les requêtes depuis `file://`, mieux vaut servir via un serveur local :
```bash
python3 -m http.server 8000
# puis http://localhost:8000
```

### En PWA sur iPad
1. Ouvrir l'URL GitHub Pages dans Safari sur l'iPad
2. Bouton Partager → *Sur l'écran d'accueil*
3. L'app s'ouvre en plein écran, orientation paysage forcée (via `manifest.json`)

### Raccourci iOS Shortcuts
Identique à Briefeed iPhone : créer un raccourci `Ouvrir URL` pointant vers ton URL GitHub Pages.

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
Courbe `cubic-bezier(0.16, 1, 0.3, 1)` partout, pour un rebond plus naturel. Hover des cards : `translateY(-4px)`. Hover des list-items : `padding-left: 16px`.

### Modal repensée
- Titre en Fraunces 30 → 44 px en vue étendue (optical size 144)
- Description en Merriweather italique en lecture
- Source en JetBrains Mono majuscules `#EB001B`
- Bouton primaire jaune (`var(--accent)`) au lieu de gris uniforme

## Sources RSS incluses

24 flux répartis en six catégories :
- **Internationale** : Le Monde, Libération, France 24, BBC, Le Monde Diplo, Courrier International, RFI
- **Sport** : L'Équipe, Foot Mercato, RMC Sport, ESPN, The Athletic
- **Technologie** : The Verge, Wired, TechCrunch, Numerama, Ars Technica, Frandroid
- **Économie** : Les Échos, BFM Business
- **Culture** : Télérama, Konbini
- **Science** : Futura Sciences, Science Daily

Pour ajouter un flux : éditer le tableau `RSS_FEEDS` au début du `<script>` dans `index.html`.

## Dépendances externes (via CDN)

- Google Fonts : Inter, Fraunces, Merriweather, JetBrains Mono
- Meyer CSS Reset
- Readability.js (Mozilla, 0.4.4)
- Proxys RSS : rss2json.com, allorigins.win, corsproxy.io (cascade)

## Notes techniques

- Pas de framework, pas de build : vanilla JS, un seul fichier
- Chargement RSS via cascade de 3 proxies CORS
- Cache image en mémoire (`Map`) pour éviter les re-fetchs
- Enrichissement d'images en arrière-plan (10 stratégies : OG, Twitter, JSON-LD, sélecteurs CSS)
- Auto-refresh des flux toutes les 5 minutes

## Licence

[MIT](LICENSE) — libre usage, modification et distribution.

---

*Repris de la logique éditoriale de Briefeed iPhone : noir profond, photos en niveaux de gris au repos, mise en couleur à l'interaction, typographie variable serif sur métadonnées mono.*
