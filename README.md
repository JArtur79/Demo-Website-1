# Blowfish & Hugo Tutorial

Basierend auf: https://n9o.xyz/posts/202310-blowfish-tutorial/

---

## Status

- [x] Git Repository initialisiert
- [x] Hugo Site erstellt
- [x] Blowfish Theme als Git-Submodul eingebunden (`themes/blowfish`)
- [x] `.gitignore` erstellt

---

## Offene Schritte

### 1. GitHub Repository verknüpfen

```bash
gh auth login
gh repo create
git push --set-upstream origin main
```

---

### 2. Konfigurationsverzeichnis anlegen

Die einzelne `hugo.toml` durch ein `config/_default/` Verzeichnis mit 5 Dateien ersetzen:

```
config/
└── _default/
    ├── config.toml
    ├── languages.en.toml
    ├── markup.toml
    ├── menus.en.toml
    └── params.toml
```

**config.toml**
```toml
theme = "blowfish"
baseURL = "https://deine-domain.de/"
```

**languages.en.toml**
```toml
[author]
  name = "Dein Name"
  image = "profile.jpg"
  headline = "I'm only human"
  bio = "Ein bisschen über dich"
```

**menus.en.toml**
```toml
[[main]]
  name = "Posts-EN"
  pageRef = "posts"
  weight = 10

[[main]]
  name = "Tags"
  pageRef = "tags"
  weight = 30
```

**markup.toml**
```toml
[goldmark.renderer]
  unsafe = true

[highlight]
  noClasses = false
```

**params.toml**
```toml
colorScheme = "neon"
disableImageOptimization = false
defaultBackgroundImage = "image.jpg"

[homepage]
  layout = "background"
  homepageImage = "image.jpg"
  showRecent = true
  showRecentItems = 6
  cardView = true
  layoutBackgroundBlur = true

[header]
  layout = "fixed"

[article]
  showHero = true
  heroStyle = "background"
  showTableOfContents = true

[list]
  showCards = true
  cardView = true
```

---

### 3. Bilder hinzufügen

- `assets/profile.jpg` – Profilbild für den Autor
- `assets/image.jpg` – Hintergrundbild für die Homepage

---

### 4. Ersten Beitrag erstellen

Datei anlegen: `content/posts/myfirstpost/index.md`

```markdown
---
title: "Mein erster Beitrag"
date: 2024-01-01
draft: false
summary: "Kurze Zusammenfassung des Beitrags"
tags: ["hugo", "blowfish"]
---

## Überschrift

Inhalt hier...
```

Thumbnail-Bild als `content/posts/myfirstpost/featured.jpg` ablegen.

---

### 5. Lokal testen

```bash
hugo server
```

Seite unter http://localhost:1313 prüfen.

---

### 6. Firebase Deployment einrichten

```bash
brew install firebase
firebase login
firebase init
```

- Option **Hosting** wählen
- GitHub Actions Workflows werden automatisch generiert

Die generierten Workflow-Dateien anpassen:

**.github/workflows/firebase-hosting-merge.yml** – Deployment bei Push auf `main`:
```yaml
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: '0.115.4'
    extended: true

- name: Checkout
  uses: actions/checkout@v3
  with:
    submodules: recursive

- name: Build
  run: hugo --minify
```

**.github/workflows/firebase-hosting-pull-request.yml** – Preview bei Pull Requests (läuft 30 Tage).

---

### 7. Fertig deployen

```bash
git add .
git commit -m "Initial site setup"
git push
```

GitHub Actions baut die Seite automatisch und deployt zu Firebase Hosting.
Den Fortschritt im Repository unter **Actions** tab beobachten.
