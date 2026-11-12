# AGENTS.md – NoJam

Linee guida per agenti e collaboratori. Le regole derivano dai documenti in `d1/` e da `CONTRIBUTING.md`.

## Progetto

NoJam è una piattaforma per la gestione della viabilità urbana: integra i dati ufficiali della Pubblica Amministrazione (cantieri, ordinanze, chiusure, piani di sgombero neve) con le segnalazioni in tempo reale degli utenti, validate da un algoritmo di affidabilità. Il progetto nasce per il Comune di Trento ed è progettato per essere esteso ad altri comuni e province (`d1/00-overview.md`).

Lingua: i contenuti sono in italiano; mantenere la stessa lingua per i nuovi contenuti (`CONTRIBUTING.md`, `d1/`).

## File

| File | Contenuto | Fonte |
|---|---|---|
| `requirements.md` | Requisiti funzionali (RF) e non funzionali (RNF), attori, mockup e tracciabilità | `d1/01-functional-requirements.md`, `d1/02-non-functional-requirements.md`, `d1/03-use-cases.md`, `d1/04-user-stories.md`, `d1/05-design-frontend.md`, `d1/06-traceability.md` |
| `oas3.yaml` | Specifica OpenAPI 3.0.3 delle API | Requisiti RF e UC indicati in ciascuna operazione |

Non aggiungere a `requirements.md` o `oas3.yaml` contenuti non presenti nei documenti `d1/`. Se un valore manca, va chiesto al team e non inventato.

## Identificatori

- Gli identificatori sono stabili: `RF-XXX`, `RNF-XXX`, `UC-XXX`, `US-XXX`, `AC-USXXX-YY`, `MU-N` / `MU-Fig.N`. Non rinumerare e non riutilizzare un ID.
- Le nuove voci prendono il numero successivo e vanno in fondo al rispettivo file.
- Non modificare ID, tag o riferimenti di altre voci (`CONTRIBUTING.md`).
- Un UC può servire più RF e un RF può essere servito da più UC; non forzare una corrispondenza 1:1 (`d1/03-use-cases.md`).
- Gli AC seguono il formato `AC-USXXX-YY`, dove `XXX` è il numero della storia e `YY` una sequenza a due cifre.

## Tracciabilità

- Ogni RF deve essere collegato ad almeno un artefatto (UC, US o MU), quando applicabile. Ogni RNF deve essere collegato agli artefatti pertinenti (`d1/06-traceability.md`).
- Collegamenti mancanti sono ammessi solo quando non applicabili o adeguatamente motivati. Il motivo va nella colonna "Note" (come per UC-049).
- Quando si modifica un RF, RNF, UC, US o MU, aggiornare nella stessa modifica `requirements.md` e, se la modifica riguarda un'API, `oas3.yaml`.

## OpenAPI (`oas3.yaml`)

- Versione OpenAPI 3.0 (RF-048).
- Le API pubbliche restituiscono JSON e/o GeoJSON (RF-048, RNF-010).
- Le API Open Data richiedono autenticazione; il superamento del limite di chiamate restituisce `429` (US-019, UC-026).
- Un formato di risposta non supportato restituisce `406` con i formati accettati (UC-048).
- Richieste con parametri non conformi restituiscono `400` (UC-048).
- Le funzioni amministrative della PA richiedono autenticazione, e un accesso non autorizzato restituisce `403`; ogni tentativo viene registrato nel log di audit (RF-045, UC-045, RF-035).
- La consultazione della mappa, incluse le tratte del piano neve, è accessibile senza autenticazione (RF-017, UC-017). La ricerca di indirizzi e POI è accessibile anche agli utenti anonimi (UC-030, attore Utente). I parcheggi richiedono autenticazione (UC-024, attore Utente Autenticato).
- Gli `operationId` sono unici.

## Regole di contenuto

- Il testo non deve cambiare il significato dei requisiti originali. Riportare il testo dei documenti `d1/` senza modifiche sostanziali.
- Non cambiare la lingua delle voci esistenti.
- Non eliminare requisiti, use case, user story o mockup. Se il team decide di rimuoverne uno, segnalarlo nel testo mantenendone l'ID.
- Non sovrascrivere né generare file in `d1/mockups/` e `d1/assets/`.

## Commit

Seguire `CONTRIBUTING.md`:

- Un commit per sessione di lavoro, con un messaggio chiaro e breve, ad esempio `added RFs from 020 to 023`.
- Non inviare molti commit piccoli nella stessa pull request.
- Non modificare voci di altre sezioni nello stesso commit che aggiunge nuove voci, salvo correzioni concordate con il team.
