# Review v2 — Tassonomia dei contenuti

> Revisione critica del raggruppamento proposto in `prompt-review.md` §5.
> Obiettivo: verificare se i domini sono coerenti, completi e ben dimensionati;
> proporre aggregazioni, scorpori e nuovi domini mancanti.

---

## 1. Sintesi del giudizio sulla v1

La tassonomia v1 è una **prima approssimazione corretta nei macro-blocchi** ma soffre di sei problemi strutturali:

1. **Ordine logico errato**: data modeling (logico) viene *dopo* db-architecture (fisico). L'ordine naturale è conceptual → logical → physical.
2. **Confusione tra modellazione relazionale e dimensionale**: `40-data-modeling` (star schema, fatti, dimensioni) è **modellazione dimensionale per BI**, non modellazione dati in generale. Va riallocata vicino a BI/ETL.
3. **`30-db-architecture` è un kitchen sink**: mescola architettura d'istanza (SGA/PGA/processi), architettura di storage (tablespace/datafile/segmenti), concorrenza (undo/redo/lock), backup&recovery, sicurezza — sono cinque sotto-domini distinti.
4. **`50-formats` (XML+JSON)** è un raggruppamento debole: XML è in gran parte legacy, JSON è first-class in 19c (con backport del data type) e profondamente legato a REST/ORDS/APEX. Andrebbero separati e JSON andrebbe avvicinato a REST.
5. **Argomenti mancanti** rispetto alla lista originale del prompt (riga 15) e al curriculum atteso per un'azienda Oracle: **funzioni analitiche** (presenti nel prompt, dimenticate da me!), security DB, tuning/performance, backup&recovery, tooling/IDE, testing PL/SQL, CI/CD database, partitioning.
6. **Numerazione incoerente**: salto da 90 a 95. Riordino consigliato a passo 10.

Sotto, il dettaglio per ciascun problema e una **tassonomia v2** proposta.

---

## 2. Problemi specifici della v1 (per dominio)

### 2.1 `00-foundations` — troppo magro
Contiene solo "relational model, ER, normalization". Mancano: paradigmi di DB (relational/document/graph/columnar/KV per *literacy*), ACID, transazioni e isolation levels, terminologia (schema/user/object), strumenti minimi (Git, SQLcl, container Oracle). Senza questi pezzi, le foundations non sono autosufficienti come prerequisito.

### 2.2 `10-sql` — manca asse di profondità
Un solo dominio per coprire J→S è ammissibile ma serve esplicitare *cosa* è J/I/S:
- **J**: SELECT, WHERE, ORDER BY, JOIN base, GROUP BY, set operators, DML, DDL base, vincoli.
- **I**: subquery (correlate, scalari), CTE/WITH, hierarchical (`CONNECT BY`), MERGE, regex, date/time avanzato, error handling lato client.
- **S**: **funzioni analitiche / window functions** (← assenti in v1!), `MATCH_RECOGNIZE`, MODEL clause, PIVOT/UNPIVOT, advanced grouping (ROLLUP/CUBE/GROUPING SETS), JSON in SQL (19c).

### 2.3 `20-plsql` — sotto-aree non distinte
J: blocchi anonimi, IF/LOOP, cursori espliciti.
I: package, exception handling, cursori parametrici, trigger.
S: bulk processing (BULK COLLECT/FORALL), dynamic SQL (EXECUTE IMMEDIATE, DBMS_SQL), autonomous transactions, pipelined functions, performance tuning PL/SQL, **testing con utPLSQL**.

### 2.4 `30-db-architecture` — da scorporare
Va spezzato in più domini (vedi v2):
- Instance & process architecture (SGA, PGA, background processes, listener, client/server).
- Storage architecture (tablespace, datafile, segment, extent, block, partitioning).
- Concurrency & transactions (undo, redo, MVCC, lock, isolation levels).
- Backup & recovery (RMAN basics, flashback, point-in-time) — *senior only*.
- Performance & tuning (Explain Plan, statistiche, AWR/ASH, hint, SQL Plan Management).
- Security (utenti, ruoli, privilegi, profili, VPD, TDE, Data Redaction, auditing).

### 2.5 `40-data-modeling` — mismatch semantico
Conteneva *star schema, fatti, dimensioni* = modellazione **dimensionale**. Da spostare nel dominio BI/DWH. Il dominio di "data modeling generale" (concettuale + logico + Barker) merita comunque un posto, ma vicino a `00-foundations`.

### 2.6 `50-formats` — coesione bassa
XML e JSON oggi sono usati in contesti diversi:
- **XML**: legacy, integrazioni B2B, qualche caso APEX/SOAP. Tenere come dominio piccolo o appendice.
- **JSON**: first-class in 19c (data type `JSON` portato da 21c indietro), `JSON_TABLE`, `JSON_OBJECT`, `JSON_QUERY`, `IS JSON`, integrato con ORDS e APEX. Da promuovere a dominio proprio e collocarlo *prima* di REST/ORDS perché ne è prerequisito.

### 2.7 `60-rest-ords` — sotto-aree implicite
Va esplicitata la copertura: ORDS architecture & install, auto-REST, REST handlers PL/SQL, auth (OAuth2/JWT), REST data sources (input per APEX), versioning, error model. Possibile suddivisione J/I/S chiara.

### 2.8 `70-apex` — rischio dominio gigante
APEX è ampio quanto SQL. Va sotto-strutturato (vedi v2):
- Architecture & workspace
- Pages, regions, items, processes
- Dynamic actions & client-side
- Data sources (local + REST)
- Auth & access control
- UX: themes, templates, plugins
- Lifecycle: deployment, packaged apps, supporting objects
- Performance & APEX best practices

### 2.9 `80-bi-etl` — troppo eterogeneo
Va spezzato:
- **Dimensional modeling** (star, snowflake, SCD types, fact types) ← qui finisce il contenuto malposto del v1 `40`.
- **DWH concepts** (Kimball vs Inmon, OLTP vs OLAP, grain, additività).
- **Analytic SQL applicato** (riusa il sub-dominio S di SQL ma con angolazione BI).
- **ETL/ELT patterns** (staging, CDC, idempotency, late-arriving facts).
- **Oracle tools for data movement**: External Tables, Data Pump, SQL*Loader, DBMS_SCHEDULER, materialized views, query rewrite.

### 2.10 `90-cloud-oci` — definire il livello di profondità
Per un'azienda di sviluppo software (non OCI-centric) il taglio è **literacy + hands-on Always Free**:
- OCI core: regions, AD, compartments, IAM, tagging.
- Network basics OCI: VCN, subnet, security list, NSG.
- OCI Database: Base DB Service, Autonomous Database (ATP/ADW), come differiscono.
- OCI Object Storage (utile per export/import, backup, ETL).
- ORDS & APEX su OCI (collegamento al dominio APEX).

### 2.11 `95-infrastructure` — incoerente
Mescola tre cose:
- **Networking fundamentals**: TCP/IP (4 layer è più moderno di ISO/OSI 7 layer — segnalare entrambi ma centrare su TCP/IP), DNS, HTTP basics, listener Oracle.
- **Containers**: Docker, Podman, compose, immagini Oracle 19c (`container-registry.oracle.com`).
- **(Implicito) Linux basics**: shell, filesystem, permessi, systemd — il prompt non lo cita ma è prerequisito per fare lab.

### 2.12 `appendix-23ai` — collocazione corretta ma sotto-popolata
Manca: AI Vector Search, Property Graph nativo, JSON Duality View, Select AI, In-Database ML (OML4Py). Tenerla come "future-facing" è giusto vista la scelta 19c.

### 2.13 Domini interamente mancanti nella v1

- **Tooling & developer experience**: Git/GitHub basics, SQLcl, SQL Developer, VS Code Oracle extension, formatter, linter.
- **Database lifecycle & CI/CD**: schema migrations (Liquibase, Flyway, SQLcl Liquibase), versioning di oggetti DB, GitHub Actions per test e migration.
- **Testing**: utPLSQL, test data management, mock strategies, code coverage PL/SQL.
- **Security trasversale**: parzialmente coperto dentro DB ma con un *learning path* dedicato avrebbe più valore.
- **Capstone projects**: progetti end-to-end che attraversano più domini (un mini-CRM con APEX + REST + DWH).

---

## 3. Tassonomia v2 proposta

Numerazione a passo 10, ordine = progressione naturale di apprendimento.

```
docs/
├── 00-foundations/                  # paradigmi DB, ACID, transazioni, terminologia, setup tooling
├── 10-tooling/                      # Git/GitHub, SQLcl, SQL Developer, VS Code, container Oracle 19c
├── 20-relational-modeling/          # ER, normalizzazione, notazione Barker, vincoli, design patterns
├── 30-sql/                          # SQL 19c — J/I/S (incl. analytic functions a livello S)
├── 40-plsql/                        # PL/SQL — J/I/S (incl. bulk, dynamic, pipelined a livello S)
├── 50-json-and-xml/                 # JSON-first (data type 19c, JSON_TABLE…), XML come legacy/optional
├── 60-database-architecture/        # istanza, processi, storage, MVCC, concorrenza
├── 70-database-security/            # utenti, ruoli, privilegi, profili, VPD, TDE, audit
├── 80-performance-and-tuning/       # explain plan, statistiche, AWR/ASH, hint, indici, partitioning
├── 90-backup-and-recovery/          # RMAN basics, flashback, PITR (senior, opzionale per dev)
├── 100-rest-and-ords/               # ORDS, auto-REST, handlers, auth, REST data sources
├── 110-apex/                        # sotto-strutturato: arch, pages, DA, data sources, auth, theming, deploy
├── 120-dimensional-modeling/        # star, snowflake, SCD, fact types, grain (era il vecchio 40 mal collocato)
├── 130-bi-and-dwh/                  # Kimball/Inmon, materialized views, query rewrite, analytic SQL applicato
├── 140-etl-elt/                     # external tables, Data Pump, SQL*Loader, CDC, scheduler, patterns
├── 150-database-cicd/               # Liquibase/Flyway, utPLSQL su CI, GitHub Actions, schema versioning
├── 160-testing/                     # utPLSQL approfondito, test data, mocking, coverage
├── 170-cloud-oci/                   # core, IAM, networking OCI, OCI DB (Base + Autonomous), Object Storage
├── 180-infrastructure-basics/       # TCP/IP, DNS, HTTP, Linux essentials, Docker/Podman
├── 190-capstone-projects/           # progetti end-to-end multi-dominio
└── 999-appendix-23ai/               # JSON Duality, AI Vector Search, Property Graph, Select AI, OML
```

Convenzioni:
- Ogni cartella ha `README.md` con: overview, ruoli che la usano, prerequisiti, durata stimata J/I/S, lista lezioni.
- Ogni lezione include front-matter YAML: `id`, `domain`, `level`, `type`, `prerequisites`, `est_read_min`, `est_total_h`, `tags`, `oracle_version`.

---

## 4. Mappatura argomenti originali → tassonomia v2

Verifica che **nessun argomento citato in `prompt.md` riga 15 sia perso**:

| Argomento originale | Dominio v2 |
|---|---|
| Basi di dati relazionali | `00-foundations` + `20-relational-modeling` |
| Linguaggio SQL (versione recente) | `30-sql` (target 19c) |
| Linguaggio PL/SQL | `40-plsql` |
| Star-schema, fatti e dimensioni | `120-dimensional-modeling` |
| XML | `50-json-and-xml` (sezione XML legacy) |
| JSON | `50-json-and-xml` (sezione JSON first-class) |
| Servizi REST | `100-rest-and-ords` |
| ORDS | `100-rest-and-ords` |
| Oracle APEX | `110-apex` |
| Oracle DB (istanza, SGA, PGA, tablespace) | `60-database-architecture` |
| Funzioni analitiche | `30-sql` (livello S) + uso applicato in `130-bi-and-dwh` |
| JSON Duality View | `999-appendix-23ai` (23ai-only) |
| Grafi | `999-appendix-23ai` (Property Graph 23ai; in 19c solo Spatial & Graph option, fuori scope) |
| Vettori | `999-appendix-23ai` (AI Vector Search, 23ai-only) |
| Nozioni base di OCI Cloud | `170-cloud-oci` |
| Stack ISO | `180-infrastructure-basics` (ridimensionato: focus TCP/IP, ISO/OSI come reference) |
| Principi di networking | `180-infrastructure-basics` |
| Business Intelligence | `130-bi-and-dwh` |
| Attività di ETL | `140-etl-elt` |
| Docker | `180-infrastructure-basics` |
| Podman | `180-infrastructure-basics` |

**Nuovi domini introdotti** rispetto al prompt originale (proposti, da confermare):
- `10-tooling` — onboarding strumentale.
- `70-database-security` — security esplicita.
- `80-performance-and-tuning` — tuning come dominio dedicato.
- `90-backup-and-recovery` — solo Senior, opzionale per Dev.
- `150-database-cicd` — Liquibase/Flyway + GitHub Actions.
- `160-testing` — utPLSQL.
- `190-capstone-projects` — progetti integrativi.

---

## 5. Suddivisione J / I / S per dominio (sintesi)

| Dominio | Junior | Intermediate | Senior |
|---|---|---|---|
| 00-foundations | Paradigmi, ACID, schema vs user | Transazioni, isolation, terminologia avanzata | — |
| 10-tooling | Git basics, SQLcl, container | Liquibase, formatter | VS Code workflow avanzato |
| 20-relational-modeling | ER, normalizzazione 1NF–3NF | BCNF, denormalizzazione tattica, Barker | Anti-pattern, refactoring schema |
| 30-sql | SELECT, JOIN, GROUP BY, DML, DDL | Subquery, CTE, MERGE, regex, hierarchical | Window functions, MATCH_RECOGNIZE, PIVOT, MODEL |
| 40-plsql | Blocchi, IF/LOOP, cursori espliciti | Package, exception, trigger | Bulk, dynamic SQL, pipelined, autonomous tx |
| 50-json-and-xml | JSON basics, `JSON_VALUE` | `JSON_TABLE`, `JSON_OBJECT`, validation | XMLType, XQuery (legacy), JSON+REST integration |
| 60-db-architecture | Istanza vs database, listener | SGA/PGA, processi, tablespace | MVCC deep dive, partitioning strategies |
| 70-db-security | Utenti, ruoli, GRANT/REVOKE | Profili, password policy, audit | VPD, TDE, Data Redaction, Database Vault |
| 80-tuning | Explain Plan reading | Statistiche, hint base, indici | AWR/ASH, SQL Plan Management, partition pruning |
| 90-backup-recovery | — | (literacy) Concetti, flashback query | RMAN, PITR, flashback DB |
| 100-rest-ords | Auto-REST su tabella | Handler PL/SQL, parametri, error model | OAuth2/JWT, versioning, performance |
| 110-apex | App da tabella, page wizard | Forms, IR/IG, validations, DA | REST data sources, plugins, theming, deployment |
| 120-dimensional-modeling | Star schema, grain | Snowflake, SCD I/II | SCD III/VI, junk/degenerate dim, factless |
| 130-bi-dwh | OLTP vs OLAP | Materialized views | Query rewrite, summary management, applied analytics |
| 140-etl-elt | External Tables, SQL\*Loader | Data Pump, scheduler | CDC, idempotency, late-arriving facts |
| 150-db-cicd | Schema in Git | Liquibase changelog | Pipeline GitHub Actions completa con utPLSQL |
| 160-testing | utPLSQL hello world | Suite, asserts, test data | Mocking, coverage, regressioni |
| 170-cloud-oci | Account, compartment, IAM | VCN, ATP/ADW provisioning | ORDS/APEX su Autonomous, Object Storage integration |
| 180-infrastructure | TCP/IP, DNS, HTTP base, Linux essentials | Docker, Oracle XE container | Compose, networking container, troubleshooting |
| 190-capstone | "Mini CRM" con APEX+DB | Mini DWH con ETL+BI | Full-stack: APEX + REST + DWH + CI/CD |
| 999-appendix-23ai | — | — | JSON Duality, Vector, Graph (preview) |

---

## 6. Tematiche trasversali (non sono domini, sono *tag*)

Alcuni temi attraversano più domini e vanno gestiti come **tag** sul front-matter, non come cartelle:

- `#security` — appare in DB, APEX, REST, OCI.
- `#performance` — appare in SQL, PL/SQL, APEX, BI.
- `#observability` — logging, tracing, AWR/ASH.
- `#integration` — REST, JSON, ETL.
- `#oracle-19c-only` / `#oracle-23ai-only` — gating per versione.

Questo evita la duplicazione "security" in ogni cartella e permette di costruire learning path tematici trasversali.

---

## 7. Cosa cambia rispetto al §5 di `prompt-review.md`

Cambiamenti **da approvare** prima di scrivere il prompt v2:

1. ➕ Aggiunti **7 nuovi domini** (tooling, security, tuning, backup, CI/CD, testing, capstone).
2. 🔀 Spostato **star schema** da `40-data-modeling` a `120-dimensional-modeling` (vicino a BI).
3. 🔀 Rinominato `40-data-modeling` → `20-relational-modeling` (per evitare ambiguità con dimensional).
4. ✂️ Scorporato `30-db-architecture` in 4 domini (architecture, security, tuning, backup).
5. 🔀 `50-formats` (XML+JSON) → `50-json-and-xml` con focus JSON-first, XML legacy.
6. ➕ Aggiunte **funzioni analitiche** (assenti dalla v1) come livello Senior di `30-sql`.
7. 🔢 Rinumerati domini a passo 10 (no più `95`).
8. 🏷️ Introdotti **tag trasversali** (`#security`, `#performance`, …) come dimensione alternativa alla gerarchia di cartelle.

---

## 8. Nuove domande emerse dalla v2

Oltre alle 8 di `prompt-review.md §6`:

9. **Granularità dominio APEX**: il dominio `110-apex` rischia di diventare 20+ moduli. Lo lasciamo flat dentro la cartella, oppure introduciamo sotto-cartelle (`110-apex/01-architecture`, `110-apex/02-pages`, …)?
10. **Backup & Recovery per developer**: davvero solo Senior, o serve almeno un modulo di *literacy* (cosa fa un DBA, perché flashback è utile come dev) a livello Intermediate?
11. **Capstone (`190`)**: 1 progetto solo o 3 progetti diversi (Dev-app / APEX / BI)? Sono molto onerosi da scrivere.
12. **Linux essentials in `180`**: lo trattiamo come dominio dovuto o come *pointer* a risorse esterne (siamo un'azienda Oracle, non corsi Linux)?
13. **`#oracle-23ai-only` come tag** vs cartella appendix dedicata: mantenere entrambi, oppure scegliere uno solo?
14. **Suddivisione J/I/S **per dominio** o anche **per ruolo**: Junior APEX dev e Junior DBA hanno cammini molto diversi. La matrice 7 domini × 3 ruoli × 3 livelli può esplodere. Confermi che ci limitiamo a J/I/S **per dominio**, e che ogni ruolo poi compone il suo path scegliendo tra i domini?

---

## 9. Prossimo passo

Se approvi questa v2:
1. **Confermi i nuovi domini** (`tooling`, `security`, `tuning`, `backup`, `cicd`, `testing`, `capstone`) — o ne tagliamo qualcuno?
2. **Confermi la separazione dimensional vs relational modeling**?
3. **Rispondi alle 6 nuove domande del §8**.

Poi procedo a scrivere `prompt.md` v2 (in inglese) usando questa tassonomia come *contratto* con il modello che genererà il syllabus.
