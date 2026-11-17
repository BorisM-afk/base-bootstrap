# Base Bootstrap 5.3.8 + SASS

Projet de départ prêt à compiler avec Live Sass Compiler (sans npm).

## Démarrer

1. Ouvrir le dossier dans VS Code (Fichier → Ouvrir le dossier).
2. Extensions : Live Sass Compiler (Glenn Marks) et Live Server (Ritwick Dey).
3. Ouvrir `assets/scss/main.scss` puis cliquer sur **Watch Sass** (barre du bas).
4. Clic droit sur `index.html` → **Open with Live Server**.

## Fichiers

- `assets/scss/main.scss` : ordre des imports
- `assets/scss/_variables.scss` : surcharges des variables Bootstrap (avant Bootstrap)
- `assets/vendor/bootstrap/scss/` : sources Bootstrap 5.3.8 (ne pas modifier)
- `assets/css/main.css` : CSS compilé, chargé par `index.html`
- `.vscode/settings.json` : sortie de Live Sass Compiler dans `assets/css/`
