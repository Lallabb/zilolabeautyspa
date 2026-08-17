# Zilola Beauty Spa — Guide : ajouter des photos dans la galerie

Ce guide explique comment ajouter, remplacer ou supprimer des photos dans la galerie Lookbook du site, sans connaissances techniques avancées.

---

## Comment fonctionne la galerie

La galerie s'affiche dans une fenêtre coulissante appelée **Lookbook**. Elle contient **7 catégories** :

| Catégorie | Dossier | Fichiers actuels | Nom des fichiers |
|---|---|---|---|
| Women's haircuts | `assets/lookbook/women/` | 16 | `women-01.webp` à `women-16.webp` |
| Men's haircuts | `assets/lookbook/men/` | 15 | `men-01.webp` à `men-15.webp` |
| Beard styles | `assets/lookbook/beard/` | 9 | `beard-01.webp` à `beard-09.webp` |
| Makeup styles | `assets/lookbook/makeup/` | 14 | `makeup-01.webp` à `makeup-14.webp` |
| Boys | `assets/lookbook/boys/` | 10 | `boys-01.webp` à `boys-10.webp` |
| Girls | `assets/lookbook/girls/` | 8 | `girls-01.webp` à `girls-08.webp` |
| Nails | `assets/lookbook/nails/` | 33 | `nails-01.webp` à `nails-33.webp` |

Chaque catégorie est définie par :

1. **Les fichiers image** dans le dossier `assets/lookbook/<catégorie>/`.
2. **Le nombre de photos** déclaré dans `index.html` (section `LOOKBOOK`).

Les deux doivent correspondre.

---

## Format des images

| Propriété | Valeur requise |
|---|---|
| Format | `.webp` (recommandé) ou `.jpg` |
| Dimensions | 720 × 900 px (ratio 4:5 portrait) |
| Poids max | 200 Ko par image |
| Orientation | Portrait |

### Convertir une photo en .webp

Si vos photos sont en `.jpg` ou `.png`, convertissez-les en `.webp` avec l'un de ces outils gratuits :

**En ligne (le plus simple) :**
- https://squoosh.app — glisser la photo, choisir WebP, export.

**Sur Mac :**
- Ouvrir la photo dans Aperçu → Exporter → format WebP.

**Sur Windows :**
- Ouvrir la photo dans Paint → Enregistrer sous → WebP.
- Ou utiliser https://squoosh.app.

**Avec Photoshop :**
- File → Export → Save for Web → WebP.

---

## Étape par étape : ajouter des photos

### Exemple : ajouter 3 photos dans la catégorie Women

#### Étape 1 — Préparer les images

Renommez vos 3 photos converties en `.webp` en suivant la numérotation existante :

```text
women-17.webp
women-18.webp
women-19.webp
```

Le numéro doit continuer après le dernier fichier existant (ici `women-16.webp`).

#### Étape 2 — Déposer les images

Placez les fichiers dans le dossier :

```text
assets/lookbook/women/
```

#### Étape 3 — Mettre à jour le nombre dans index.html

Ouvrez `index.html` dans un éditeur de texte (Bloc-notes, TextEdit, VS Code, ou directement sur GitHub).

Cherchez la ligne :

```javascript
women:{title:"Women's haircuts",copy:"Cuts, color and texture references for every age and personal style.",count:16,generatedFrom:8,alt:"Women's hairstyle inspiration"},
```

Changez `count:16` par `count:19` :

```javascript
women:{title:"Women's haircuts",copy:"Cuts, color and texture references for every age and personal style.",count:19,generatedFrom:8,alt:"Women's hairstyle inspiration"},
```

C'est tout. La galerie affichera maintenant 19 photos dans la catégorie Women.

#### Étape 4 — Sauvegarder et publier

Si vous éditez sur GitHub :
1. Cliquez sur l'icône crayon (✏️) en haut à droite du fichier.
2. Modifiez la valeur `count`.
3. Cliquez **Commit changes**.
4. Le site se met à jour automatiquement si vous êtes connecté à Netlify ou un autre hébergeur automatique.

---

## Remplacer une photo existante

Pour remplacer une photo sans en ajouter de nouvelle :

1. Donnez à votre nouvelle photo exactement le même nom que celle à remplacer (ex : `women-03.webp`).
2. Déposez-la dans le même dossier `assets/lookbook/women/` en écrasant l'ancienne.
3. Ne modifiez pas `index.html` — le nombre ne change pas.
4. Committez/poussez le changement.

---

## Supprimer une photo

1. Supprimez le fichier du dossier (ex : `women-16.webp`).
2. Diminuez le `count` dans `index.html` (ex : `count:16` → `count:15`).
3. **Important** : si vous supprimez une photo au milieu (ex : `women-05`), renommez les suivantes pour qu'il n'y ait pas de trou :

```text
women-05.webp  (était women-06)
women-06.webp  (était women-07)
...
```

Les numéros doivent toujours se suivre sans interruption, de `01` à `count`.

---

## Ajouter des photos dans n'importe quelle catégorie

La méthode est identique pour toutes les catégories. Voici les paramètres actuels :

```javascript
const LOOKBOOK={
  women:{count:16, generatedFrom:8, ...},
  men:{count:15, generatedFrom:6, ...},
  beard:{count:9, generatedFrom:4, ...},
  makeup:{count:14, ...},
  boys:{count:10, ...},
  girls:{count:8, ...},
  nails:{count:33, generatedFrom:23, ...}
};
```

Pour ajouter 5 photos dans `nails` :
1. Créez `nails-34.webp` à `nails-38.webp`.
2. Déposez-les dans `assets/lookbook/nails/`.
3. Changez `count:33` en `count:38`.
4. Committez.

---

## Le champ `generatedFrom`

Certaines catégories ont un champ `generatedFrom`. Il sépare les photos réelles du salon des photos d'inspiration :

```text
women-01 à women-08  → vraies photos du salon
women-09 à women-16  → inspirations générées (badge "Style inspiration")
```

Si vous ajoutez de vraies photos du salon, augmentez aussi `generatedFrom` pour qu'elles apparaissent avant les inspirations.

Exemple : ajouter 3 vraies photos Women → `generatedFrom:8` devient `generatedFrom:11`, et `count:16` devient `count:19`.

---

## Méthode rapide via GitHub (sans installer rien)

1. Allez sur https://github.com/Lallabb/zilolabeautyspa
2. Naviguez vers `assets/lookbook/women/`
3. Cliquez **Add file → Upload files**
4. Glissez vos fichiers `.webp` renommés
5. Cliquez **Commit changes**
6. Remontez à la racine, ouvrez `index.html`
7. Cliquez l'icône crayon (✏️)
8. Cherchez `count:16` (Ctrl+F) et changez en `count:19`
9. Cliquez **Commit changes**
10. Le site se met à jour.

---

## Récapitulatif

| Action | Dossier | index.html |
|---|---|---|
| Ajouter 3 photos Women | Déposer `women-17` à `women-19` | `count:16` → `count:19` |
| Remplacer `women-03` | Écraser `women-03.webp` | Aucun changement |
| Supprimer `women-16` | Supprimer le fichier | `count:16` → `count:15` |
| Ajouter 5 photos Nails | Déposer `nails-34` à `nails-38` | `count:33` → `count:38` |

## Règles d'or

1. Les numéros de fichiers doivent se suivre sans interruption (`01`, `02`, `03`...).
2. Le `count` dans `index.html` doit correspondre au nombre de fichiers.
3. Le format `.webp` est fortement recommandé pour la performance.
4. Les dimensions recommandées sont 720 × 900 px (portrait 4:5).
5. Ne pas dépasser 200 Ko par image.
6. Toujours **committer** après chaque changement.

## Besoin d'aide ?

Contactez **Clandestudio Agency** en fournissant :
- les photos à ajouter ;
- la catégorie concernée ;
- l'URL du site ou le nom du dépôt GitHub.