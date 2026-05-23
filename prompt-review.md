# Analisi di `prompt.md` — Revisione e proposte

> Documento di lavoro per affinare il prompt prima di generare il syllabus definitivo del repo formativo Radicle.

---

## 1. Decisioni già prese

Risposte fornite alle domande chiarificatrici (da considerare vincoli per il prompt finale):

| Aspetto | Decisione |
|---|---|
| Lingua | **Inglese**, sia documentazione sia parte pratica |
| Versione Oracle DB target | **19c** (LTS più diffusa in produzione) |
| Segmentazione percorsi | **Per dominio tecnico**, con livelli (Junior / Intermediate / Senior) all'interno di ciascun dominio |
| Modello contribuzione | **Branch personali nel repo + PR review** (collaboratori in whitelist) |

Conseguenze immediate:
- Niente argomenti Oracle 23ai-only nel core (JSON Duality View, AI Vector Search, Property Graph nativo). Possono restare come *appendix / preview* opzionale, segnalata come "23ai-only".
- Tutto il materiale (commit, PR, file, esempi) deve essere in inglese. Servirà uno *style guide* linguistico minimo.
- I "trainer" e i "collaboratori interni" devono essere whitelisted via GitHub team o `CODEOWNERS`; contributi del pubblico generico restano comunque su fork + PR.

---

## 2. Criticità del prompt attuale

### 2.1 Refusi e ambiguità lessicali
Da correggere prima di sottoporre il prompt al modello, perché riducono qualità di parsing:

`utilizzando` → utilizzato · `piubblicamente` · `ORacle` · `traversali` → trasversali · `classicazione` · `begineer` · `tempo necessaria` · `efforte` · `ogni collaborare` → collaboratore · `intermidate` · `repositpry`.

### 2.2 Modello Git non scalabile
"Ogni branch figlio fa merge/rebase a ogni pubblicazione" non è sostenibile con N collaboratori: significa N rebase per ogni push su `main`. Va sostituito con una regola pull-based: il collaboratore allinea il proprio branch quando gli serve, non a ogni pubblicazione.

### 2.3 Doppio target con esigenze non riconciliate
"Neo-assunti Radicle" e "pubblico globale" hanno bisogni divergenti (onboarding aziendale vs autocontenuto). Va dichiarata una priorità o creati due ingressi (`/onboarding-radicle`, `/learning-paths`).

### 2.4 Governance dei trainer mancante
Non è specificato chi può scrivere su `main`, chi fa review, quale qualità minima è richiesta. Servono `CODEOWNERS`, branch protection rules, checklist di review.

### 2.5 Lista argomenti eterogenea
La lista a riga 15 mescola assi diversi (linguaggi, architettura, feature DB, infrastruttura, discipline). Senza una matrice di classificazione il syllabus risultante sarà disordinato.

### 2.6 "Solutions" pubbliche = spoiler permanenti
Esercizi e soluzioni nello stesso repo pubblico annullano il valore didattico degli esercizi. Va separato (repo soluzioni dedicato, oppure branch `solutions/*` non linkati nei materiali principali, oppure rilascio dopo un periodo di latenza).

---

## 3. Omissioni rilevanti

- **Licenza** — bloccante per repo pubblico. Proposta: **CC BY-SA 4.0** per testi e **Apache-2.0** (o MIT) per codice.
- **`CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`**, issue & PR templates.
- **Ambienti di lab**: Oracle XE 19c via container? LiveSQL? OCI Always Free? APEX su `apex.oracle.com`? Va fissato.
- **Definizione formati di contenuto**: differenza operativa tra *lesson*, *lab*, *tutorial*, *exercise* (durata, struttura, output atteso).
- **Prerequisiti tra moduli**: serve un grafo, non solo un ordine lineare.
- **Verifica apprendimento**: quiz, esercizi auto-correggibili (utPLSQL su CI), criteri di "completato".
- **Unità di stima tempo**: minuti di lettura vs ore totali (lettura + lab + esercizi). Proponi entrambe.
- **Strumenti**: Git/GitHub basics, SQLcl, SQL Developer, Liquibase/Flyway, utPLSQL, AWR / SQL tuning, partitioning, sicurezza Oracle (VPD, TDE, RBAC), backup & recovery, data modeling (Barker), CI/CD per database.
- **Tracce di ruolo**: il prompt parla solo di seniority, ma "Junior APEX dev" ≠ "Junior DBA". Senza ruolo, i livelli sono ambigui.
- **Sito statico**: con molti `.md` annidati GitHub diventa scomodo; valutare **MkDocs Material** o **Docusaurus** su GitHub Pages.
- **Maintenance policy**: chi aggiorna, ciclo di revisione, deprecation strategy.

---

## 4. Proposte di miglioramento del prompt

### 4.1 Struttura il deliverable atteso
Chiedi esplicitamente al modello di produrre il syllabus come **tabella Markdown** con colonne:

```
ID | Domain | Title | Type (lesson/lab/exercise) | Level (J/I/S) | Prerequisites (IDs) | Est. read (min) | Est. total (h) | Notes
```

Più, separatamente:
- Un **grafo di prerequisiti** come lista di archi `from -> to`.
- Una **proposta di struttura cartelle** del repo.
- Una sezione **gaps** in cui il modello segnala cosa manca rispetto ai 3 ruoli tipici di Radicle.

### 4.2 Vincoli quantitativi
- "Massimo ~60 moduli nel syllabus iniziale".
- "Ogni livello (J/I/S) di ogni dominio completabile in ≤ X ore di studio".
- "Ogni lezione ≤ 30 min di lettura; lab ≤ 2 h; esercizio ≤ 1 h".

### 4.3 Definisci personas / ruoli Radicle
Prima dei livelli, descrivi i ruoli target (es. *Oracle Application Developer*, *APEX Developer*, *Data / BI Engineer su stack Oracle+OCI*). Il modello mappa i moduli ai ruoli.

### 4.4 Esplicita ambiente e strumentazione
"Tutti i lab assumono Oracle DB 19c in container (immagine X), client SQLcl, IDE SQL Developer o VS Code con estensione Oracle. Lab APEX su `apex.oracle.com` workspace gratuito."

### 4.5 Riordina il prompt
Sequenza suggerita:
1. Contesto azienda e mission del repo
2. Vincoli (repo pubblico GitHub, lingua inglese, licenza, target Oracle 19c)
3. Target audience e personas
4. Modello di governance e contribuzione
5. Tassonomia dei contenuti (domini × livelli × formati)
6. Deliverable richiesto al modello (con schema esatto)
7. Vincoli quantitativi
8. Cosa NON fare (no sviluppo contenuti, no 23ai-only nel core)

---

## 5. Bozza di repo skeleton suggerita

```
academy/
├── README.md                    # entry point, learning paths overview
├── LICENSE                      # CC BY-SA 4.0
├── LICENSE-CODE                 # Apache-2.0 for code samples
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── CODEOWNERS
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE.md
├── syllabus.md                  # master index, generated/maintained
├── docs/
│   ├── 00-foundations/          # relational model, ER, normalization
│   ├── 10-sql/                  # SQL (19c)
│   ├── 20-plsql/                # PL/SQL
│   ├── 30-db-architecture/      # SGA, PGA, tablespaces, instance
│   ├── 40-data-modeling/        # star schema, facts/dims
│   ├── 50-formats/              # XML, JSON
│   ├── 60-rest-ords/            # REST services, ORDS
│   ├── 70-apex/
│   ├── 80-bi-etl/
│   ├── 90-cloud-oci/            # OCI basics
│   ├── 95-infrastructure/       # networking basics, ISO/OSI, Docker, Podman
│   └── appendix-23ai/           # JSON Duality, Graph, Vector (preview)
├── labs/
│   └── <domain>/<level>/<lab-id>/
├── exercises/
│   └── <domain>/<level>/<exercise-id>/
└── solutions/                   # branch-scoped or separate repo
    └── <github-handle>/
```

Ogni `<lesson>.md` segue un template fisso: *objectives, prerequisites, est. time, theory, examples, exercises, references*.

---

## 6. Domande ancora aperte

1. **Personas di Radicle**: confermi i tre ruoli principali (Application Dev / APEX Dev / Data-BI Engineer)? Va aggiunto/tolto qualcosa (es. DBA puro, OCI architect)?
2. **Profondità DBA**: SGA/PGA/tablespace devono restare a livello "literacy" per developer, oppure serve un track DBA completo (backup&recovery, RMAN, Data Guard, partitioning, RAC)?
3. **Cloud OCI**: solo *fundamentals* (regioni, compartment, IAM, networking base, OCI DB service), oppure anche Always Free hands-on?
4. **Soluzioni esercizi**: ti va bene il pattern "soluzioni in branch `solutions/<github-handle>/`, non linkati dai materiali principali"? O preferisci un repo separato?
5. **Sito statico**: rendiamo navigabile via **MkDocs Material** su GitHub Pages, oppure ci accontentiamo della navigazione GitHub nativa per la prima iterazione?
6. **Cadenza di pubblicazione trainer**: weekly? monthly? Lo dichiariamo nel README per gestire le aspettative.
7. **Lingua e style guide**: vuoi una guida di stile minima (es. American English, code-blocks con `sql`/`plsql`, naming `kebab-case` per file)?
8. **Esercizi auto-verificabili**: vogliamo introdurre **utPLSQL** + CI GitHub Actions sin dall'inizio, oppure lasciamo gli esercizi come "auto-correzione da parte dello studente" nella v1?

---

## 7. Prossimo passo proposto

Una volta chiarite le domande aperte:
1. Riscrivo `prompt.md` in versione **v2** (in inglese, ordinato, con deliverable schema esatto e vincoli).
2. Sottometto il prompt v2 al modello per produrre `syllabus.md` (tabella + grafo prerequisiti + gaps).
3. Inizializziamo il repo con `README.md`, `LICENSE`, `CONTRIBUTING.md`, `CODEOWNERS`, e la struttura cartelle vuota dello scheletro proposto al §5.
