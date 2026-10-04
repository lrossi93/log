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


## Conclusioni
