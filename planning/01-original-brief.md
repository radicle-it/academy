# Brief originale: repository formativo di Radicle

## Contesto

Radicle è una realtà italiana specializzata in soluzioni Oracle. Questo repository pubblico nasce come **punto di aggregazione** del materiale formativo utilizzato e condiviso internamente dall'azienda.

Il repository ha un doppio obiettivo:

1. **Onboarding interno**: guidare i neo-assunti nella costruzione delle competenze necessarie a operare sullo stack Oracle.
2. **Risorsa pubblica**: mettere a disposizione, su internet, materiale utile a chiunque voglia sviluppare competenze trasversali sulla tecnologia Oracle, in particolare Oracle Database, SQL, PL/SQL e APEX.

## Struttura immaginata dei contenuti

Una struttura gerarchica in grado di ospitare formati diversi:

- lezioni
- lab
- tutorial
- esercizi
- link a risorse esterne
- codice di esempio

Per ogni lezione si vorrebbe **stimare il tempo** necessario per leggerla e assimilarla.

## Modello di contribuzione

- I **trainer** pubblicano il materiale sul branch `main` con cadenza periodica. `main` è un branch protetto.
- Ogni **collaboratore** può creare un branch personale, nominato secondo il proprio handle GitHub, su cui versionare e condividere le proprie soluzioni agli esercizi.
- Ogni volta che viene pubblicato del nuovo materiale, i branch derivati dovrebbero allinearsi al ramo principale tramite merge o rebase.

## Classificazione per livello

Da valutare se introdurre una segmentazione dei contenuti per seniority:

- **Beginner**
- **Intermediate**
- **Expert**

In caso affermativo, ciascun contenuto andrà classificato anche in base al livello.

## Argomenti da coprire

Spunto iniziale dei temi rilevanti, da consolidare e completare:

- basi di dati relazionali
- linguaggio SQL (versione recente)
- linguaggio PL/SQL
- star-schema, fatti e dimensioni
- XML, JSON
- servizi REST, ORDS
- Oracle APEX
- Oracle Database: istanza, SGA, PGA, tablespace, ecc.
- funzioni analitiche
- JSON duality view
- grafi
- vettori
- nozioni di base su OCI Cloud
- stack ISO/OSI, principi di networking
- business intelligence
- attività di ETL
- container: Docker, Podman

L'elenco è una traccia iniziale: è probabile che alcuni argomenti rilevanti manchino. Servirà completarli e classificarli secondo un **ordine naturale di apprendimento**.

## Profilo dell'azienda

Radicle è un'azienda di **sviluppo software**. Nozioni come il networking restano utili come fondamentali, ma non rappresentano il cuore dell'attività e vanno dimensionate di conseguenza.

## Approccio formativo

Ogni unità di formazione dovrebbe articolarsi in due parti:

- una **parte teorica**;
- una **parte pratica** con esercizi da svolgere.

## Vincoli operativi

- Il repository è **pubblico** su GitHub, accessibile a tutti.
- Il piano formativo dovrà essere **sostenibile** rispetto ai diversi gradi di seniority (junior, intermediate, senior).

## Richiesta

In questa fase non è richiesto lo sviluppo dei contenuti, ma la definizione della **struttura del syllabus** (l'indice). In particolare:

1. l'elenco dei contenuti principali da coprire;
2. una stima dell'effort necessario per studiarli;
3. la classificazione secondo un ordine naturale di apprendimento;
4. una proposta di declinazione del piano formativo per i tre livelli di seniority.
