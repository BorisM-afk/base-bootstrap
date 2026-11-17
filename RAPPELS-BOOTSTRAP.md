> **Lire ce fichier proprement dans VS Code :** `Ctrl + Shift + V` (aperçu dans l'onglet)
> ou `Ctrl + K` puis `V` (aperçu à côté du code).

# Rappels Bootstrap 5.3 + SASS

Les rappels essentiels, rien de plus. Pour le reste : la doc Bootstrap 5.3.

---

## 1. Ordre dans `main.scss`

```scss
@import "../vendor/bootstrap/scss/functions";        // 1. fonctions
@import "variables";                                  // 2. MES variables (avant !)
@import "../vendor/bootstrap/scss/variables";         // 3. variables Bootstrap
@import "../vendor/bootstrap/scss/variables-dark";
$theme-colors: map-merge($theme-colors, ("accent": #e2622f));   // 4. maps
@import "../vendor/bootstrap/scss/bootstrap";         // 5. Bootstrap
@import "hero";                                       // 6. mes partiels BEM
```

Une variable écrite **après** l'import de Bootstrap ne sert à rien (`!default`).

---

## 2. Variables SASS les plus utiles (`_variables.scss`)

| Variable | Ce qu'elle change |
|---|---|
| `$primary` `$secondary` `$dark` `$light` | couleurs du thème → `btn-*`, `bg-*`, `text-*`, `text-bg-*` |
| `$body-bg` `$body-color` | fond et texte de la page |
| `$font-family-sans-serif` | police du texte |
| `$headings-font-family` `$headings-font-weight` | police et graisse de `h1`…`h6` / `.h1`…`.h6` |
| `$display-font-family` `$display-font-weight` | police et graisse des `.display-*` (300 par défaut) |
| `$lead-font-size` | taille de `.lead` |
| `$border-radius` `$border-radius-lg` | arrondis par défaut |
| `$btn-border-radius` `$btn-padding-x` `$btn-font-weight` | boutons (`50rem` = pilule, `0` = carré) |
| `$card-border-radius` `$card-border-color` | cartes |
| `$input-border-radius` | champs de formulaire |
| `$nav-pills-link-active-bg` | onglet actif des `nav-pills` |
| `$link-color` `$link-decoration` | liens (`none` = pas souligné) |
| `$spacer` | base de tous les `m-*`, `p-*`, `gap-*` |
| `$enable-shadows` `$enable-cssgrid` | options globales |

**Maps** (après les variables Bootstrap) :

```scss
$theme-colors:  map-merge($theme-colors,  ("accent": #e2622f));        // → btn-accent, bg-accent…
$aspect-ratios: map-merge($aspect-ratios, ("3x2": calc(2 / 3 * 100%)));  // → ratio-3x2
```

**Outils SASS de Bootstrap** dans tes partiels :

```scss
@include media-breakpoint-up(lg) { … }   // à partir de 992px
tint-color($primary, 80%)                 // plus clair
shade-color($primary, 30%)                // plus foncé
$spacer * 3   $border-radius-lg   $headings-font-family
```

---

## 3. Grille et breakpoints

`container` → `row` → `col-*` (12 colonnes, mobile first)

| Préfixe | — | `sm` | `md` | `lg` | `xl` | `xxl` |
|---|---|---|---|---|---|---|
| À partir de | 0 | 576 | 768 | 992 | 1200 | 1400 px |

```html
<div class="row g-4">
  <div class="col-12 col-md-6 col-lg-4">…</div>          <!-- 1 / 2 / 3 par ligne -->
</div>
<div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4">
  <div class="col">…</div>                                <!-- même chose, plus court -->
</div>
```

- `g-*` `gx-*` `gy-*` : gouttières (sur la `row`)
- `align-items-center` (sur la `row`) : aligner verticalement
- `order-lg-1` / `order-lg-2` : inverser l'ordre sur desktop
- `offset-lg-2`, `col-lg-8 mx-auto` : décaler / centrer
- `d-none d-lg-block` : caché sur mobile, visible dès `lg`

---

## 4. Utilitaires

| Besoin | Classes |
|---|---|
| Marges / padding | `m p` + `t b s e x y` + `0`…`5` / `auto` → `mt-3` `py-5` `mx-auto` `ms-auto` |
| Flex | `d-flex` `flex-column` `flex-md-row` `justify-content-between/center` `align-items-center` `gap-3` |
| Texte | `text-center` `text-lg-start` `fw-bold` `fw-semibold` `text-uppercase` `lead` `small` |
| Titres visuels | `display-1`…`6` (5 → 2,5 rem) · `h1`…`h6` (2,5 → 1 rem) |
| Couleurs | `text-primary` `text-secondary` `text-white-50` `bg-light` `bg-body-tertiary` `text-bg-dark` |
| Bordures | `border` `border-0` `border-bottom` `rounded-3` `rounded-pill` `rounded-circle` |
| Ombres | `shadow-sm` `shadow` `shadow-lg` |
| Tailles | `w-100` `h-100` `mw-100` |
| Divers | `sticky-top` `list-unstyled` `list-inline` `link-light` `visually-hidden` |

Bootstrap 5 : `ms-*` / `me-*` (plus de `ml-*` / `mr-*`).

---

## 5. Images

```html
<img src="…" class="img-fluid rounded-3" alt="…">                    <!-- ne déborde jamais -->

<div class="ratio ratio-4x3">                                         <!-- format fixe -->
  <img src="…" class="object-fit-cover" alt="…">
</div>

<img src="…" width="96" height="96" class="rounded-circle object-fit-cover" alt="…">  <!-- avatar -->
```

Ratios fournis : `1x1` `4x3` `16x9` `21x9`. Image de fond → en SCSS : `url("../img/hero.jpg") center / cover` (chemin depuis `main.css`).

---

## 6. Composants

### Navbar + burger

```html
<nav class="navbar navbar-expand-lg bg-white border-bottom">
  <div class="container">
    <a class="navbar-brand" href="#">Logo</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#menu"
            aria-controls="menu" aria-expanded="false" aria-label="Ouvrir le menu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="menu">
      <ul class="navbar-nav ms-auto">           <!-- mx-auto = liens centrés -->
        <li class="nav-item"><a class="nav-link active" aria-current="page" href="#">Accueil</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Page</a></li>
      </ul>
    </div>
  </div>
</nav>
```

Navbar sombre : `bg-dark` + `data-bs-theme="dark"`.

### Boutons et badges

```html
<a class="btn btn-primary btn-lg" href="#">Lien</a>
<button type="button" class="btn btn-outline-secondary btn-sm">Action</button>
<div class="btn-group">…</div>
<span class="badge rounded-pill text-bg-primary">Nouveau</span>
```

### Carte

```html
<article class="card h-100 shadow-sm">
  <div class="ratio ratio-4x3">
    <img src="…" class="card-img-top object-fit-cover" alt="…">
  </div>
  <div class="card-body">
    <h3 class="card-title h5">Titre</h3>
    <p class="card-text text-secondary">Texte</p>
    <a href="#" class="btn btn-primary">Voir</a>
  </div>
</article>
```

Carte horizontale : `card` > `row g-0` > `col-4` (image) + `col-8` (`card-body`).

### Formulaire

```html
<div class="row g-3">
  <div class="col-12 col-md-6">
    <label for="email" class="form-label">E-mail</label>
    <input type="email" class="form-control" id="email">
  </div>
  <div class="col-12">
    <select class="form-select">…</select>
  </div>
</div>

<div class="input-group">                              <!-- champ + bouton collés -->
  <input type="email" class="form-control">
  <button class="btn btn-dark" type="submit">OK</button>
</div>
```

### Accordéon

```html
<div class="accordion" id="faq">
  <div class="accordion-item">
    <h3 class="accordion-header">
      <button class="accordion-button" type="button" data-bs-toggle="collapse"
              data-bs-target="#q1" aria-expanded="true" aria-controls="q1">Question</button>
    </h3>
    <div id="q1" class="accordion-collapse collapse show" data-bs-parent="#faq">
      <div class="accordion-body">Réponse</div>
    </div>
  </div>
  <!-- suivants : bouton "collapsed" + aria-expanded="false", div sans "show" -->
</div>
```

### Onglets

```html
<ul class="nav nav-pills justify-content-center gap-2" role="tablist">
  <li class="nav-item" role="presentation">
    <button class="nav-link active" data-bs-toggle="pill" data-bs-target="#t1" type="button" role="tab">Onglet 1</button>
  </li>
  <li class="nav-item" role="presentation">
    <button class="nav-link" data-bs-toggle="pill" data-bs-target="#t2" type="button" role="tab">Onglet 2</button>
  </li>
</ul>
<div class="tab-content">
  <div class="tab-pane fade show active" id="t1" role="tabpanel">…</div>
  <div class="tab-pane fade" id="t2" role="tabpanel">…</div>
</div>
```

### Liste, tableau, citation

```html
<ul class="list-group list-group-flush">
  <li class="list-group-item d-flex justify-content-between">Café <span>2,80 €</span></li>
</ul>

<table class="table table-striped">…</table>

<figure>
  <blockquote class="blockquote"><p>Citation</p></blockquote>
  <figcaption class="blockquote-footer">Auteur</figcaption>
</figure>
```

Burger, accordéon, onglets, modale, carrousel → besoin du **bundle JS** avant `</body>`, et `data-bs-target="#x"` = `id="x"`.

---

## 7. Pièges

- Variable écrite après l'import de Bootstrap → aucun effet.
- CSS Bootstrap du CDN chargé en plus de `main.css` → tes variables sont écrasées.
- Oublier la `row` entre `container` et `col-*`.
- `!important` ou `style=""` → à éviter.
- Graisse de police non importée → faux gras.
- Tester seulement en plein écran → tester à 375, 768 et 1200 px.

---

## 8. Extensions VS Code

**Tout installer d'un coup** : copier cette ligne dans le terminal de VS Code (`Ctrl + ù`) puis Entrée.

```
code --install-extension ritwickdey.liveserver --install-extension glenn2223.live-sass --install-extension hossaini.bootstrap-intellisense --install-extension anbuselvanrocky.bootstrap5-vscode --install-extension yzhang.markdown-all-in-one --install-extension enkia.tokyo-night --install-extension formulahendry.auto-close-tag --install-extension formulahendry.auto-rename-tag --install-extension pranaygp.vscode-css-peek --install-extension ecmel.vscode-html-css --install-extension fuzionix.devtool-plus
```

**Si « code n'est pas reconnu »** (PC de l'IUT par exemple), passer par une des solutions ci-dessous.

**Sans terminal ni GitHub** :
1. Ouvrir ce dossier dans VS Code → une notification propose d'installer les extensions recommandées (fichier `.vscode/extensions.json`) → **Tout installer**.
2. Pas de notification ? `Ctrl + Shift + X`, taper `@recommended` → installer la liste.
3. En dernier recours : `Ctrl + Shift + X`, coller l'ID d'une extension (ex. `glenn2223.live-sass`) → **Installer**, une par une.

**Avec un profil VS Code** (extensions + thème + réglages) :
1. Sur mon PC : roue crantée en bas à gauche → **Profils** → **Exporter le profil** → **Exporter vers un fichier** (`.code-profile`).
2. Mettre le fichier sur une clé USB ou un drive.
3. Sur l'autre PC : roue crantée → **Profils** → **Importer le profil** → choisir le fichier.

| Extension | À quoi ça sert |
|---|---|
| Live Server | Ouvre le site et le recharge à chaque sauvegarde |
| Live Sass Compiler | Compile le `.scss` en `.css` (bouton **Watch Sass**) |
| Bootstrap IntelliSense | Propose les classes Bootstrap pendant la frappe |
| Bootstrap 5 Snippets | Raccourcis pour générer des composants (navbar, card…) |
| Markdown All in One | Lire et écrire les `.md` |
| Tokyo Night | Thème → choisir **Tokyo Night Storm** |
| Auto Close Tag | Ferme la balise toute seule |
| Auto Rename Tag | Renomme la balise fermante avec l'ouvrante |
| CSS Peek | `Ctrl + clic` sur une classe → va à son CSS |
| HTML CSS Support | Propose tes propres classes dans le HTML |
| DevTool Plus | Boîte à outils (pipette, convertisseurs…) |
