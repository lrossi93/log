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


## Conclusioni
