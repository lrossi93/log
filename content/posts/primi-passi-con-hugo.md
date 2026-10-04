+++
title = "Hugo: primi passi"
date = 2026-09-28
draft = false
description = "Used for <meta description> / OpenGraph if summary is empty."
summary = "Un tentativo di replica della guida *quickstart* di Hugo."
tags = ["hugo"]
+++

## Introduzione

Questo è il primo di una serie di articoli sul mio viaggio nella tecnologia. In questo, parlerò di [Hugo](https://gohugo.io), di come ho conosciuto il framework e di come lo potrei utilizzare per progetti personali. 

L'obiettivo di questo esperimento è puramente didattico, pertanto cercherò il più possibile di non utilizzare tool AI di alcun genere, per quanto i loro prodotti siano sempre più impeccabili e, oggettivamente, facciano risparmiare ore per lavori ripetitivi.

Il tono con cui cercherò di parlare sarà a metà strada tra il pub e la formalità, ma tratterò questo progetto come una guida scritta da me stesso e rivolta a me stesso. Spero, comunque, che possa essere utile a qualcuno.

## Primi passi

Per prima cosa, ho seguito la guida [quickstart](https://gohugo.io/getting-started/quick-start/) di Hugo, che mi ha permesso di avviare l'infrastruttura e di creare questo primo post. Al momento sto gestendo il progetto da due finestre di terminale: una con cui ho avviato il server Hugo e l'altra con cui organizzo i contenuti.

Trovo molto comodo lanciare il server con il comando

```hugo server --buildDrafts```

grazie a cui la renderizzazione della pagina web corrispondente all'articolo che sto scrivendo è praticamente istantanea.

### Configurazione

Ho installato il tema [Omegion](https://github.com/omegion/hugo-omegion), scelto tra i vari temi disponibili per Hugo. Magari, quando avrò una maggiore comprensione di tutte le parti in gioco in un sito Hugo, riuscirò a creare il mio tema, potenzialmente in accordo con lo stesso tema grafico del mio sito.

Dopodiché, ho modificato la configurazione generale del progetto Hugo, impostando baseURL, locale, titolo e tema.

## Pubblicazione

La documentazione ci tiene a marcare la differenza tra *pubblicazione* e *deploy*. La pubblicazione, che avviene semplicemente col comando ```hugo``` nella working directory, permette il rendering di tutti i file realizzati per comporre il materiale web finale (HTML, CSS, asset vari, JS). I file renderizzati verranno collocati nella directory **public** e verranno sovrascritti ad ogni nuova pubblicazione.

Dato che mi interessa che la cartella **public** venga ripulita ad ogni pubblicazione, ho modificato la configurazione della build di progetto con il codice seguente, aggiunto in coda al file *hugo.toml* (come indicato nella sezione di [build](https://gohugo.io/configuration/build/#clean-destination-directory) di Hugo):

```toml
[build]
  [build.cleanDestinationDir]
    enable = false
    keepDirs = ['{**/,}.*']
    keepFiles = ['{**/,}.{git,gitignore,gitattributes}']
```

Dopo aver pubblicato il sito con il comando ```hugo```, la struttura della directory ```public/``` dovrebbe essere all'incirca la seguente:

```text
public/
├── categories/
│   ├── index.html
│   └── index.xml  <-- Feed RSS di questa sezione
├── posts/
│   ├── my-first-post/
│   │   └── index.html
│   ├── index.html
│   └── index.xml  <-- Feed RSS di questa sezione
├── tags/
│   ├── index.html
│   └── index.xml  <-- Feed RSS di questa sezione
├── index.html
├── index.xml      <-- Feed RSS di questa sezione
└── sitemap.xml
```

È molto interessante il fatto che venga renderizzata anche la ```sitemap.xml```, è un aiuto in più per chi ha bisogno, ad esempio, di gestire la SEO.

## Deploy

Uno dei passi successivi che vengono toccati è il deploy. Ci sono diversi modi di effettuare il deploy di un sito Hugo ma, a me, interessa il [deploy con GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/).

Considererò questa guida più avanti, quando avrò una maggiore comprensione dell'infrastruttura.

## Utilizzo base

Per questa sezione, ho proseguito nella documentazione con la pagina [Basic usage](https://gohugo.io/getting-started/usage/).

La prima parte è un piccolo compendio su comandi base come ```hugo version```, ```hugo help```, ```hugo server --help``` e ```hugo build```, tutti da lanciare nella working directory. 

Poco oltre, c'è una parte più interessante sulle *bozze*: in particolare, servendoci di appositi flag per ciascun articolo, è possibile considerarlo o meno per la pubblicazione del progetto.

Ogni articolo, quindi ogni "foglia" del ramo di filesystem che parte dalla working directory, è un file markdown (```.md```) che può avere quattro variabili: ```draft```, ```date```, ```publishDate``` e ```expiryDate```. 

## Struttura a directory

Ogni progetto Hugo è composto da directory in cui ci sono subdirectory che concorrono a contenuto, struttura, comportamento e presentazione.

Il comando 

```
hugo new project log.lrossi.xyz
```

crea una struttura come la seguente:

```text
my-project/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
└── hugo.toml         <-- configurazione
```

A seconda delle esigenze, questa struttura può essere personalizzata ma, a me, per il momento, va bene la struttura standard.

### Directories
Di seguito un breve riassunto dello scopo delle varie directory.

|Directory|Contenuto|
|---|---|
|**archetypes**|Template dei potenziali futuri contenuti.|
|**assets**|Immagini, CSS, Sass, JS e TS.|
|**config**|Configurazione di progetto ed eventuali subdirectory e file. Per progetti con configurazione minima, basta il file ```hugo.toml```.|
|**content**|File ```.md``` delle pagine del progetto.|
|**data**|Configurazioni per contenuto, localizzazione e navigazione.|
|**i18n**|Tabelle di traduzione.|
|**layouts**|Modelli per utilizzare i contenuti per renderizzare il sito finale.|
|**public**|Sito web pubblicato, generato con comando ```hugo build``` o ```hugo server```.|
|**resources**|Cache per i contenuti multimediali renderizzati con ```hugo build```, principalmente CSS e immagini.|
|**static**|File che vengono copiati in **public** alla build (icone, immagini, CSS e JS).|
|**themes**|Uno o più temi, ciascuno nella sua subdirectory.|

## Temi
Il tema che ho inizialmente scelto per questo progetto è [Omegion](https://github.com/omegion/hugo-omegion), perchè ha un'interfaccia minimal e molto rilassante, specialmente con il tema dark. Con l'obiettivo di raccogliere post in varie categorie (cosa che, devo ancora capire come farsi), mi sembrava la scelta migliore. 

Ad ogni modo, si può creare un nuovo tema da zero con il comando

```hugo new theme theme-name```

che risulta nella creazione del seguente ramo di file system:

```text
my-theme/
├── archetypes/
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
└── hugo.toml
```

Esiste la possibilità di gestire un file system unificato per due progetti Hugo, o per un progetto che sfrutti elementi di due temi diversi. Questo è possibile grazie a una configurazione da aggiungere in coda al file ```hugo.toml```:

```toml
[module]
  [[module.mounts]]
    source = 'content' # riferito alla working directory
    target = 'content'
  [[module.mounts]]
    source = '/path/to/other/content' # riferito ad altra directory
    target = 'content'
```

In questo modo, è possibile gestire progetti con file system più articolato, come ad esempio il seguente:

```text
home/
└── user/
    ├── my-project/
    │   ├── content/
    │   │   ├── books/
    │   │   │   ├── _index.md
    │   │   │   ├── book-1.md
    │   │   │   └── book-2.md
    │   │   └── _index.md
    │   ├── themes/
    │   │   └── my-theme/
    │   └── hugo.toml
    └── shared-content/
        └── films/
            ├── _index.md
            ├── film-1.md
            └── film-2.md
```

## Risorse esterne
- [Corso Hugo di Cloudcannon](https://cloudcannon.com/tutorials/hugo-beginner-tutorial/);
- [Corso Hugo di Giraffe Academy](https://www.giraffeacademy.com/static-site-generators/hugo/)

