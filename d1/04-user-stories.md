# User Story

<!--
Ogni User Story utilizza un identificatore stabile US-XXX e ogni relativo criterio di accettazione è identificato come AC-USXXX-YY (ad esempio, US-000 → AC-US000-01, AC-US000-02, …). Gli stessi identificatori vengono mantenuti nelle sezioni di traceability e negli artefatti dei deliverable successivi.
-->

<!--
### US-000 - [Titolo user story]
**Collegamenti:** RF-000; UC-000

Come [tipo di utente], voglio [obiettivo] in modo da [beneficio].

**Criteri di accettazione:**
- AC-US000-01: [criterio di accettazione verificabile.]
- AC-US000-02: [criterio di accettazione verificabile.]
- AC-US000-03: [criterio di accettazione verificabile.]
-->

# User Story

### US-001 - Registrazione utente standard e tramite identità digitale
**Collegamenti:** RF-001, RF-002, RF-049; UC-001, UC-002, UC-049

Come utente anonimo, voglio poter creare un account standard o autenticarmi tramite provider terzi e sistemi di identità digitale (Google SSO, SPID, CIE) in modo da accedere alle funzionalità personali e contributive della piattaforma.

**Criteri di accettazione:**
- AC-US001-01: L'utente può registrarsi inserendo nome, cognome, email e data di nascita oppure selezionando un provider esterno (Google SSO, SPID, CIE).
- AC-US001-02: Il sistema invia un'email di verifica all'indirizzo fornito prima di completare la creazione dell'account standard.
- AC-US001-03: In caso di credenziali errate o autenticazione rifiutata dal provider terzo, il sistema mostra un messaggio di errore e blocca l'accesso.

---

### US-002 - Consultazione della mappa interattiva senza autenticazione
**Collegamenti:** RF-006, RF-017; UC-006, UC-017

Come utente anonimo, voglio poter consultare la mappa interattiva del territorio visualizzando traffico, cantieri e ordinanze in modo da informarmi sulla viabilità senza l'obbligo di creare un account.

**Criteri di accettazione:**
- AC-US002-01: L'utente non autenticato visualizza i livelli di traffico, cantieri attivi, ordinanze comunali e zone del piano neve.
- AC-US002-02: Se l'utente tenta di accedere a funzionalità riservate (es. invio segnalazioni), il sistema blocca l'operazione e richiede l'autenticazione.
- AC-US002-03: In assenza di connessione, il sistema mostra l'ultima versione delle tessere della mappa memorizzata in cache.

---

### US-003 - Gestione profilo e chiusura account (GDPR)
**Collegamenti:** RF-003, RF-007, RF-029; UC-003, UC-007, UC-029

Come utente autenticato, voglio aggiornare le mie informazioni personali o richiedere l'eliminazione definitiva del mio account in modo da mantenere il controllo sui miei dati nel rispetto del GDPR.

**Criteri di accettazione:**
- AC-US003-01: L'utente può consultare e modificare le informazioni del proprio profilo dalla sezione dedicata.
- AC-US003-02: La richiesta di eliminazione dell'account richiede una conferma dell'identità (tramite password o codice via email).
- AC-US003-03: Confermata l'eliminazione, il sistema invalida la sessione, cancella i dati personali e anonimizza i contributi stradali precedentemente inviati.

---

### US-004 - Gestione dei consensi privacy e cookie
**Collegamenti:** RF-008, RF-037, RF-038; UC-008, UC-037, UC-038

Come utente, voglio poter accettare o modificare in modo granulare i consensi al trattamento dei dati, alla geolocalizzazione e ai cookie in modo da tutelare la mia privacy durante l'uso dell'app.

**Criteri di accettazione:**
- AC-US004-01: Il sistema richiede l'accettazione dei Termini di Servizio e Privacy Policy in fase di registrazione e a ogni aggiornamento sostanziale.
- AC-US004-02: Al primo accesso viene mostrato un banner per la gestione granulare dei cookie e dei trattamenti non essenziali.
- AC-US004-03: La revoca del consenso alla geolocalizzazione comporta la disattivazione delle sole funzionalità che ne dipendono, avvisando l'utente.

---

### US-005 - Salvataggio di luoghi e percorsi preferiti
**Collegamenti:** RF-005, RF-023; UC-005, UC-023

Come utente autenticato, voglio salvare, modificare ed eliminare luoghi frequenti e destinazioni preferite in modo da accedere rapidamente alla navigazione e ricevere notifiche dedicate.

**Criteri di accettazione:**
- AC-US005-01: L'utente può salvare un luogo o un itinerario assegnandogli un'etichetta personalizzata.
- AC-US005-02: Il sistema consente la modifica ed eliminazione degli elementi salvati in qualsiasi momento.
- AC-US005-03: Raggiunto il limite massimo di elementi salvabili, il sistema segnala l'impossibilità di aggiungerne altri senza prima rimuoverne uno esistente.

---

### US-006 - Calcolo e ottimizzazione dei percorsi (Routing Predittivo)
**Collegamenti:** RF-016, RF-028; UC-016, UC-028

Come utente autenticato, voglio inserire punto di partenza, destinazione e orario futuro per ottenere l'itinerario ottimale basato sia su dati ufficiali della PA sia su segnalazioni validate della community.

**Criteri di accettazione:**
- AC-US006-01: Il motore di routing integra cantieri programmati, ordinanze e criticità convalidate in tempo reale per proporre gli itinerari migliori.
- AC-US006-02: Impostando un orario di partenza futuro, il sistema stima i rallentamenti in base alle ordinanze programmate per quella specifica fascia oraria.
- AC-US006-03: In caso di chiusura totale del percorso senza alternative percorribili, il sistema notifica l'impossibilità di raggiungere la destinazione.

---

### US-007 - Navigazione assistita Turn-by-Turn e simulazione
**Collegamenti:** RF-030, RF-031, RF-032; UC-030, UC-031, UC-032

Come utente guidatore, voglio ricevere indicazioni vocali e visive passo-passo e poter visualizzare l'anteprima del viaggio in modo da guidare in sicurezza senza distrazioni.

**Criteri di accettazione:**
- AC-US007-01: L'utente può cercare indirizzi o punti di interesse (POI) tramite geocoding e visualizzare l'anteprima del tracciato prima di partire.
- AC-US007-02: Durante la guida, il sistema aggiorna le istruzioni visive e vocali turn-by-turn in base alla posizione GPS in tempo reale.
- AC-US007-03: In caso di deviazione o di nuova criticità sul percorso, il sistema ricalcola automaticamente l'itinerario.

---

### US-008 - Integrazione con infotainment veicolare (Android Auto / CarPlay)
**Collegamenti:** RF-033; UC-033

Come utente guidatore, voglio proiettare l'interfaccia di navigazione sullo schermo del veicolo in modo da interagire in modalità hands-free durante la guida.

**Criteri di accettazione:**
- AC-US008-01: L'applicazione supporta la proiezione dell'interfaccia su Android Auto e Apple CarPlay.
- AC-US008-02: Le funzionalità complesse non ammesse durante la guida vengono disabilitate e rese disponibili solo a veicolo fermo.
- AC-US008-03: In caso di disconnessione dello schermo del veicolo, la navigazione prosegue sul dispositivo mobile senza interrompere la sessione.

---

### US-009 - Invio di segnalazioni viarie in tempo reale
**Collegamenti:** RF-009, RF-014; UC-009, UC-014

Come utente autenticato, voglio segnalare in tempo reale eventi stradali (incidenti, spazzaneve, ostacoli) rilevando la mia posizione in modo da avvisare gli altri utenti della community.

**Criteri di accettazione:**
- AC-US009-01: L'utente può inviare una segnalazione geolocalizzata selezionando la categoria dell'evento (incidente, rallentamento, meteo, spazzaneve).
- AC-US009-02: Il sistema rileva automaticamente la posizione GPS corrente permettendo una rifinitura manuale prima dell'invio.
- AC-US009-03: In assenza di connettività, la segnalazione viene accodata in locale e trasmessa al ripristino della rete.

---

### US-010 - Moderazione automatica e punteggio di reputazione
**Collegamenti:** RF-011, RF-015, RF-022, RF-025; UC-011, UC-015, UC-022, UC-025

Come utente contributore, voglio che le mie segnalazioni vengano valutate dall'algoritmo tramite incrocio dati in modo da accrescere la mia reputazione ed erogare badge nel sistema di gamification.

**Criteri di accettazione:**
- AC-US010-01: Il sistema valuta l'affidabilità di una segnalazione incrociando dati ufficiali PA e conferme di altri utenti nell'area spazio-temporale.
- AC-US010-02: Una segnalazione priva di conferme entro il tempo limite decade automaticamente; una segnalazione smentita viene rimossa applicando una penalità al segnalatore.
- AC-US010-03: Al raggiungimento di specifiche soglie di reputazione, il sistema attribuisce automaticamente punti e badge visualizzabili nel profilo utente.

---

### US-011 - Notifiche preventive e configurazione avanzata
**Collegamenti:** RF-010, RF-021, RF-040, RF-041; UC-010, UC-021, UC-040, UC-041

Come utente autenticato, voglio configurare i canali, il raggio di prossimità e le fasce orarie di silenzio delle notifiche per essere avvisato preventivamente solo degli eventi rilevanti sui miei percorsi.

**Criteri di accettazione:**
- AC-US011-01: L'utente può definire raggio di prossimità, canali (push, email) e orari di silenzio per ciascuna tipologia di allerta.
- AC-US011-02: Il sistema invia notifiche preventive su cantieri o chiusure lungo i percorsi salvati prima dell'orario di partenza abituale.
- AC-US011-03: Le notifiche ad alta priorità (emergenze) ignorano le fasce di silenzio e vengono comunque recapitate.

---

### US-012 - Ingestion automatica dei dati ufficiali della PA
**Collegamenti:** RF-013, RF-047; UC-013, UC-047

Come operatore PA, voglio che il sistema importi e sincronizzi automaticamente i dati statici su cantieri e ordinanze dai feed comunali/provinciali per mantenere aggiornata la mappa senza interventi manuali ripetitivi.

**Criteri di accettazione:**
- AC-US012-01: Il sistema effettua periodicamente il parsing automatizzato dei dati statici in formati standard (CSV, GeoJSON, Shapefile).
- AC-US012-02: I dati importati vengono normalizzati e resi subito disponibili al motore di routing e alla mappa interattiva.
- AC-US012-03: Se i feed esterni non sono raggiungibili o contengono errori di formato, il sistema registra un'anomalia e mantiene in uso gli ultimi dati validi.

---

### US-013 - Dashboard di Smart Governance e gestione cantieri
**Collegamenti:** RF-019, RF-043, RF-045; UC-019, UC-043, UC-045

Come operatore PA accreditato, voglio accedere a una dashboard riservata con autenticazione a più fattori per pubblicare ordinanze e inserire cantieri o deviazioni stradali.

**Criteri di accettazione:**
- AC-US013-01: L'accesso alla dashboard richiede l'autenticazione a più fattori (MFA) e rispetta il controllo degli accessi basato sui ruoli (RBAC).
- AC-US013-02: L'operatore può creare, modificare o revocare manualmente cantieri, ordinanze e chiusure stradali.
- AC-US013-03: Le modifiche apportate dalla dashboard si propagano entro 3 secondi alla mappa pubblica e ai percorsi proposti agli utenti.

---

### US-014 - Gestione e monitoraggio del Piano Neve
**Collegamenti:** RF-018; UC-018

Come operatore PA, voglio inserire la pianificazione delle tratte di sgombero neve e aggiornarne lo stato di avanzamento per informare in trasparenza i cittadini durante le nevicate.

**Criteri di accettazione:**
- AC-US014-01: L'operatore può caricare le tratte programmate del piano neve con le relative fasce orarie di intervento.
- AC-US014-02: L'operatore e le segnalazioni convalidate degli utenti aggiornano lo stato della tratta (da effettuare, in corso, completata).
- AC-US014-03: La mappa pubblica mostra visivamente lo stato aggiornato dei percorsi senza tracciare i singoli mezzi fisici in tempo reale.

---

### US-015 - Attivazione della modalità Emergenza Viabilità
**Collegamenti:** RF-027; UC-027

Come operatore PA, voglio poter attivare lo stato di emergenza su una determinata area geografica per inviare allerte immediate ad alta priorità in caso di eventi straordinari (es. frane, alluvioni).

**Criteri di accettazione:**
- AC-US015-01: L'operatore delimita sulla dashboard la zona interessata dall'evento straordinario e attiva la modalità "Emergenza Viabilità".
- AC-US015-02: Il sistema invia immediatamente una notifica push prioritaria a tutti gli utenti presenti o in avvicinamento all'area.
- AC-US015-03: Il motore di routing aggiorna istantaneamente i calcoli per evitare la zona interdetta finché l'emergenza non viene archiviata.

---

### US-016 - Analisi aggregate e reportistica del traffico
**Collegamenti:** RF-020; UC-020

Come operatore PA, voglio consultare ed esportare report aggregati ed anonimizzati sui flussi e le criticità viarie per pianificare interventi infrastrutturali urbani basati sui dati.

**Criteri di accettazione:**
- AC-US016-01: La dashboard genera grafici e dati aggregati su tempi di percorrenza e colli di bottiglia selezionando area e periodo temporale.
- AC-US016-02: Tutti i dati elaborati sono rigorosamente anonimizzati in conformità alle norme sulla privacy.
- AC-US016-03: L'operatore può esportare i report elaborati in formato PDF o CSV.

---

### US-017 - Gestione ruoli e audit log degli operatori PA
**Collegamenti:** RF-034, RF-035; UC-034, UC-035

Come amministratore PA, voglio gestire i ruoli degli operatori e consultare un registro di audit non modificabile per garantire la tracciabilità e la responsabilità di ogni modifica alla viabilità.

**Criteri di accettazione:**
- AC-US017-01: L'amministratore può creare ruoli personalizzati con permessi granulari (lettura, inserimento, validazione, pubblicazione).
- AC-US017-02: Ogni azione eseguita dagli utenti PA viene registrata in un log di audit non modificabile comprensivo di timestamp e autore.
- AC-US017-03: Il sistema blocca la cancellazione o la modifica manuale delle voci presenti nel log di audit.

---

### US-018 - Importazione massiva dei dati di viabilità
**Collegamenti:** RF-036; UC-036

Come operatore PA, voglio caricare file in blocco (CSV, GeoJSON, Shapefile) per inserire contemporaneamente un elevato numero di cantieri o chiusure stradali programmate.

**Criteri di accettazione:**
- AC-US018-01: L'interfaccia permette l'upload di file nei formati standard CSV, GeoJSON e Shapefile.
- AC-US018-02: Il sistema valida il file e fornisce un'anteprima mostrando i record corretti e quelli scartati prima di confermare la scrittura.
- AC-US018-03: L'esito dell'importazione viene registrato automaticamente nel log di audit.

---

### US-019 - Esposizione di API Open Data verso sistemi terzi
**Collegamenti:** RF-026, RF-048; UC-026, UC-048

Come sviluppatore/sistema terzo, voglio accedere ad API pubbliche documentate secondo lo standard OpenAPI 3.0 per consumare ed elaborare i dati aperti sulla viabilità cittadina.

**Criteri di accettazione:**
- AC-US019-01: Le API pubbliche del sistema sono documentate in standard OpenAPI 3.0 e restituiscono dati nei formati JSON o GeoJSON.
- AC-US019-02: Il sistema richiede l'autenticazione delle chiamate API e restituisce un errore HTTP 429 in caso di superamento del rate limit.
- AC-US019-03: È consentito anche il download manuale diretto dei dataset Open Data dal portale web.