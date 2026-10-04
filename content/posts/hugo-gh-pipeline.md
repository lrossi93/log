+++
date = '2026-10-04'
draft = true
title = "Da Hugo locale alla pubblicazione su GitHub Pages"
description = "Sistema per la pubblicazione automatica di articoli su framework Hugo."
summary = "Sistema per la pubblicazione automatica di articoli su framework Hugo."
tags = ["hugo", "github"]
+++

## Introduzione
Una volta seguita la documentazione del tema [Omegion](https://github.com/omegion/hugo-omegion) e avere deciso di suddividere i contenuti tra articoli (*Idee e riflessioni*) e lavori (*Progetti*), ora è il momento di realizzare la pipeline che mi permette, una volta che un articolo è terminato, di pubblicarlo automaticamente all'indirizzo [log.lrossi.xyz](https://log.lrossi.xyz).

Questo avviene utilizzando non solo l'utile funzionalità di [GitHub Pages](https://docs.github.com/en/pages) per l'hosting statico, ma anche [GitHub Actions](https://docs.github.com/en/actions) per automatizzare il rendering di un sito statico realizzato con framework [Hugo](https://gohugo.io).

## La pipeline

La sequenza di azioni che voglio compiere, per minimizzare i miei oneri, è la seguente: scrittura di contenuti, push su repository GitHub del sorgente aggiornato, compilazione dei sorgenti e creazione del sito statico tramite script GitHub Actions, pubblicazione del sito statico tramite GitHub Pages.

### Creazione della repository su GitHub
Nella working directory, nel mio caso specifico ```log/```, si può inizializzare la repository locale con il comando ```git init```. Tutti i sorgenti realizzati vanno aggiunti al pacchetto da sincronizzare con i comandi ```git add .``` e ```git commit -m "Initial commit"```.

Dopodiché va creata la repository remota sul profilo personale GitHub. Per permettere la pubblicazione con GitHub Pages, la repository deve necessariamente essere pubblica e, in fase di creazione, può essere vuota. Una volta creata, è possibile sincronizzare la repository remota con quella locale inizializzata al passaggio precedente con i seguenti comandi da terminale:

```text
git remote add origin git@github.com:<username>/<repository-name>.git
git branch -M main
git push -u origin main
```

GitHub, in questo, ci guida passo passo. Se tutto è andato liscio, a questo punto la repository locale e quella remota su GitHub dovrebbero avere gli stessi identici contenuti.

### Pubblicazione su GitHub Pages
Una volta che il sito è integralmente su repository remota, si può esporre su un dominio come ```<username>.github.io/<repository>```. Seguendo le istruzioni presenti nella [documentazione di GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages), il sito non dovrebbe risultare disponibile all'URL ```<username>.github.io/<repository>```, perché GitHub Pages cerca un ```index.html``` da esporre, il quale però non è presente nella root di progetto. Occorre indicare che il deploy del sito deve essere fatto da una specifica directory, in particolare dalla directory ```public```, che viene creata solo dopo aver compilato i sorgenti locali con il comando ```hugo``` da terminale.

A questo punto, è essenziale creare uno script di compilazione lato GitHub per fare il deploy dalla directory ```public```.

### Deploy automatico con GitHub Actions
Una volta su GitHub, nella repository del progetto corrente, occorre navigare su **Actions** e cliccare sul pulsante **New workflow**, dato che un workflow è la sequenza di azioni che, partendo dal sorgente committato nei passaggi precedenti, pubblicano il sito finale compilandolo come si farebbe da terminale, ma in maniera automatica.

Si può partire da un file di workflow per Hugo predefinito, che ho opportunamente modificato per allineare la versione di Hugo remota con quella locale.

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.165.0 # <-- La mia unica modifica...
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v5
```

Ho scoperto, a questo punto, che il mio file di configurazione ```hugo.toml``` era scritto male per via di errori di compilazione dovuti alla scelta della lingua (che, per quanto riguarda la scrittura dei contenuti, ho scelto essere l'italiano), quindi ho dovuto riscriverlo un po' meglio separando le variabili scalari da quelle vettoriali, raggruppando opportunamente queste ultime. Questo è quello che ha funzionato per me:

```toml
baseURL = 'https://log.lrossi.xyz/'
locale = 'it'
title = 'log.lrossi.xyz'
theme = 'hugo-omegion'

defaultContentLanguage = 'it'
defaultContentLanguageInSubdir = false
disableDefaultLanguageRedirect = false
disableLanguages = []

[languages]
  [languages.it]
    label = "italiano"
    weight = 1
    title = "log.lrossi.xyz"

[params]
  enableSearch = true
  description = "Breve descrizione del sito"

  [params.author]
    name = "Lorenzo Rossi"
    bio = [
      "Musician, programmer, creative time waster.",
      "Raccolgo qui idee, riflessioni e progetti che non interessano a nessuno."
    ]

    [[params.author.links]]
      name = "GitHub"
      url = "https://github.com/lrossi93"
      icon = "github"

    [[params.author.links]]
      name = "LinkedIn"
      url = "https://www.linkedin.com/in/lrossi1993"
      icon = "linkedin"

    [[params.author.links]]
      name = "e-mail"
      url = "mailto:lrossi93@proton.me"
      icon = "mail"

[outputs]
  home = ["html", "rss", "searchindex"]

[outputFormats.searchindex]
  mediaType = "application/json"
  baseName = "index"
  isPlainText = true
  notAlternative = true
```

## Conclusioni
