# Requisiti – NoJam

Documento consolidato dei requisiti di NoJam. Testi e tracciabilità sono riportati dai file in `d1/`:
- visione e limiti: `d1/00-overview.md`
- attori: `d1/03-use-cases.md`
- requisiti funzionali (RF) e non funzionali (RNF): `d1/01-functional-requirements.md`, `d1/02-non-functional-requirements.md`
- collegamenti con use case (UC), user story (US), criteri di accettazione (AC) e mockup (MU): `d1/06-traceability.md`
- descrizione dei mockup: `d1/05-design-frontend.md`

Gli identificatori sono quelli originali e non vengono modificati.

---

## 1. Visione

A Trento il traffico è un problema sistemico: la nostra città si sviluppa su assi viari obbligati. Quando partono i cantieri estivi o nevica, la città si ferma. Le informazioni ufficiali esistono, il Comune e la Provincia pubblicano dettagliate ordinanze in PDF e tabelle di marcia, ma rimangono isolate su siti burocratici.

La soluzione è NoJam, una piattaforma basata su un'architettura duale. Da un lato colleziona dalla Pubblica Amministrazione tutti i dati ufficiali e statici: tabelle dello sgombero neve, cantieri e deviazioni programmate. Dall'altro raccoglie dagli utenti le informazioni dinamiche in tempo reale, come rallentamenti ed incidenti. Il cuore del sistema è un algoritmo che incrocia queste fonti per valutare l'affidabilità delle segnalazioni, calcolare i percorsi migliori e inviare alert preventivi.

L'architettura di NoJam nasce sul territorio di Trento, ma è nativamente scalabile a qualsiasi altra realtà comunale o provinciale.

**Limiti dell'applicazione**
- Dipendenza dai dati aperti della Pubblica Amministrazione: la completezza e l'aggiornamento delle informazioni statiche dipendono dalla qualità, dal formato e dalla frequenza di pubblicazione dei dati degli enti territoriali.
- Efficacia legata all'adozione degli utenti: la copertura e l'affidabilità delle segnalazioni in tempo reale crescono con il numero di cittadini attivi sulla piattaforma.

---

## 2. Attori

| Attore | Descrizione |
|---|---|
| Utente | Attore generalizzato: qualunque fruitore dell'applicazione, autenticato o meno. |
| Utente Anonimo | Utente non autenticato; può consultare la mappa, registrarsi e autenticarsi. |
| Utente Autenticato | Utente con account attivo; accede alle funzionalità personali e contributive. |
| Operatore PA | Operatore accreditato del Comune o della Provincia, con accesso alla dashboard di Smart Governance. |
| Amministratore PA | Operatore PA con privilegi amministrativi; definisce ruoli, permessi e policy di conservazione dei dati. |
| Provider di Identità | Sistema esterno di autenticazione (Google SSO, SPID, CIE). |
| Servizio Email | Sistema esterno per l'invio delle comunicazioni transazionali. |
| Supporto Tecnico | Team che riceve e gestisce bug report e feedback. |
| Ente Territoriale | Sorgente dati ufficiale (Comune, Provincia) per cantieri, ordinanze e piani neve. |
| Servizi Parcheggi e TPL | Sorgenti dati in tempo reale su sosta e trasporto pubblico. |
| Servizio di Posizione | Sensore GPS e servizio di localizzazione del dispositivo. |
| Servizio di Geocoding | Sorgente esterna di toponomastica e punti di interesse del territorio. |
| Sistema di Infotainment | Piattaforma veicolare esterna (Android Auto, Apple CarPlay) su cui viene proiettata l'interfaccia di navigazione. |
| Sistema Terzo | Applicativo o portale istituzionale che consuma le API Open Data. |

---

## 3. Requisiti funzionali

| ID | Titolo | Descrizione | UC | US | AC | MU | Note |
|---|---|---|---|---|---|---|---|
| RF-001 | Registrazione utente | Ogni utente deve potersi registrare all'interno dell'applicazione. | UC-001 | US-001 | AC-US001-01…03 | MU-2 | |
| RF-002 | Login utente | Ogni utente deve potersi autenticare all'interno dell'applicazione tramite credenziali standard, provider terzi e sistemi di identità digitale. | UC-002 | US-001 | AC-US001-01…03 | MU-3 | |
| RF-003 | Gestione del profilo | Il sistema deve consentire all'utente di consultare e aggiornare le informazioni del suo profilo. | UC-003 | US-003 | AC-US003-01…03 | MU-9 | |
| RF-004 | Gestione preferenze | L'utente può gestire la lingua e le impostazioni generali dell'app. | UC-004 | US-020 | AC-US020-01…03 | MU-10 | |
| RF-005 | Gestione luoghi e percorsi salvati | Il sistema deve consentire agli utenti di salvare, modificare ed eliminare luoghi preferiti e percorsi frequenti per accedere velocemente alla navigazione e ricevere notifiche. | UC-005 | US-005 | AC-US005-01…03 | - | |
| RF-006 | Gestione dei permessi | Il sistema deve gestire i permessi in base al ruolo assegnato all'utente autenticato, mentre gli utenti non autenticati possono solo consultare la mappa. | UC-006 | US-002 | AC-US002-01…04 | MU-1 | |
| RF-007 | Logout utente | Ogni utente deve potersi deautenticare all'interno dell'applicazione. | UC-007 | US-003 | AC-US003-01…03 | MU-11 | |
| RF-008 | Gestione degli utenti e privacy | Il sistema deve permettere all'utente di visualizzare, prestare, modificare o revocare i consensi al trattamento dei dati personali e alla geolocalizzazione nel rispetto delle normative riguardanti il trattamento della privacy. | UC-008 | US-004 | AC-US004-01…03 | MU-4, MU-11 | |
| RF-009 | Gestione della geolocalizzazione | Il sistema deve tracciare la posizione corrente in tempo reale sulla mappa, centrare la vista, inviare segnalazioni di geolocalizzazione e alert di prossimità. | UC-009 | US-009 | AC-US009-01…05 | MU-5 | |
| RF-010 | Gestione del canale di notifiche | L'utente deve poter abilitare, disabilitare e filtrare le notifiche. | UC-010 | US-011 | AC-US011-01…03 | MU-10 | |
| RF-011 | Cronologia delle segnalazioni | Ogni utente deve poter vedere lo storico delle segnalazioni, visualizzandone lo stato di approvazione/validazione, l'impatto della community e il proprio punteggio di reputazione. | UC-011 | US-010 | AC-US010-01…04 | MU-9 | |
| RF-012 | Gestione di feedback ed errori | Il sistema deve fornire un canale di comunicazione tra utenti e supporto tecnico per segnalare bug e inviare feedback. | UC-012 | US-021 | AC-US021-01…04 | - | |
| RF-013 | Ingestion e sincronizzazione dati ufficiali PA | Il sistema deve importare, strutturare e sincronizzare automaticamente i dati statici e programmati provenienti dagli enti territoriali (Comune e Provincia), inclusi cantieri stradali, chiusure temporanee, ordinanze di viabilità e piani di sgombero neve. | UC-013 | US-012 | AC-US012-01…03 | - | |
| RF-014 | Invio segnalazioni in tempo reale | L'utente autenticato deve poter inviare segnalazioni geolocalizzate in tempo reale relative a eventi dinamici sulla rete stradale, come incidenti, rallentamenti, presenza di ostacoli, condizioni meteo avverse o passaggi/presenza dei mezzi spazzaneve. | UC-014 | US-009 | AC-US009-01…05 | MU-5 | |
| RF-015 | Algoritmo di valutazione affidabilità e reputation scoring | Il sistema deve incrociare le segnalazioni inviate dagli utenti con i dati ufficiali della Pubblica Amministrazione e con altre segnalazioni concorrenti per calcolare un indice di affidabilità del dato (peso dell'evento) e aggiornare il punteggio di reputazione dell'utente segnalatore. | UC-015 | US-010 | AC-US010-01…03 | - | |
| RF-016 | Calcolo e ottimizzazione percorsi (Routing predittivo) | Il sistema deve calcolare e proporre itinerari di navigazione che tengano conto sia delle limitazioni ufficiali programmate (cantieri, ordinanze, zone d'intervento neve) sia delle criticità temporanee convalidate in tempo reale dalla community. | UC-016 | US-006 | AC-US006-01…03 | MU-6 | |
| RF-017 | Consultazione mappa interattiva ed eventi | Il sistema deve consentire sia agli utenti autenticati che a quelli non autenticati di consultare la mappa interattiva del territorio, visualizzando lo stato del traffico, i cantieri attivi, i dettagli delle ordinanze comunali, i percorsi dello sgombero neve e le allerte confermate. | UC-017 | US-002 | AC-US002-01…04 | MU-1, MU-8 | |
| RF-018 | Gestione del piano neve | Il sistema deve gestire la mappa e la pianificazione dello sgombero neve, permettendo la visualizzazione dei percorsi programmati e dello stato di avanzamento delle operazioni. Lo stato di avanzamento è aggiornato dagli operatori della PA e dalle segnalazioni della community, senza tracciamento in tempo reale dei mezzi operativi. | UC-018 | US-014 | AC-US014-01…03 | MU-8 | |
| RF-019 | Pannello di gestione e Smart Governance per la PA | Il sistema deve mettere a disposizione degli operatori accreditati del Comune e della Provincia una dashboard amministrativa per pubblicare ordinanze, inserire o modificare manualmente cantieri ed emergenze, validare segnalazioni e supervisionare la viabilità. | UC-019 | US-013 | AC-US013-01…04 | MU-12 | |
| RF-020 | Analisi aggregate e reporting del traffico | Il sistema deve elaborare ed evidenziare dati aggregati ed anonimizzati sui flussi di traffico, sui tempi di percorrenza e sulle criticità ricorrenti, offrendo all'amministrazione report e analisi utili alla pianificazione urbana. | UC-020 | US-016 | AC-US016-01…03 | - | |
| RF-021 | Notifiche e allerte preventive di mobilità | Il sistema deve inviare notifiche push o alert di prossimità tempestivi per avvisare gli utenti di cantieri programmati lungo i percorsi salvati o chiusure stradali improvvise prima dell'inizio del viaggio. | UC-021 | US-011 | AC-US011-02 | - | |
| RF-022 | Moderazione automatica dei contenuti | Il sistema deve determinare automaticamente la validità delle segnalazioni inviate dagli utenti, senza intervento di moderatori umani: più segnalazioni concordanti sono presenti nella stessa area spazio-temporale, maggiore è l'affidabilità attribuita all'avviso. Il sistema deve confermare le segnalazioni corroborate, far decadere quelle non confermate entro i tempi previsti e rimuovere quelle smentite dai dati ufficiali o dalle segnalazioni successive. Resta ferma la facoltà degli operatori PA di intervenire manualmente sulle singole segnalazioni dalla dashboard, in quanto fonte autorevole (RF-019). | UC-022 | US-010 | AC-US010-02 | MU-12 | |
| RF-023 | Modalità di guida offline parziale | Il sistema deve consentire il salvataggio in locale (caching) delle mappe e dei percorsi frequenti per garantirne la consultazione e il routing base anche in caso di disconnessione o copertura di rete limitata. | UC-023 | US-005 | AC-US005-04, AC-US002-03 | - | |
| RF-024 | Integrazione con trasporti pubblici e parcheggi | Il sistema deve mostrare sulla mappa punti d'interesse come la disponibilità in tempo reale dei parcheggi cittadini. | UC-024 | US-022 | AC-US022-01…04 | MU-8 | |
| RF-025 | Sistema di badge e gamification della reputazione | Il sistema deve attribuire punti, livelli o badge di affidabilità agli utenti in base all'accuratezza delle segnalazioni confermate, incentivando la partecipazione attiva della community. | UC-025 | US-010 | AC-US010-03 | MU-9 | |
| RF-026 | Esportazione ed API dati aperti (Open Data) | Il sistema deve esporre API pubbliche e formati standard per consentire la ricondivisione in formato Open Data dei dati di viabilità ed eventi elaborati verso terzi o altri portali istituzionali. | UC-026 | US-019 | AC-US019-01…03 | - | |
| RF-027 | Gestione eventi straordinari ed emergenze | Il sistema deve permettere alla PA di attivare una modalità di "Emergenza Viabilità" (es. nevicate eccezionali, frane, allagamenti) per evidenziare immediatamente allerte ad alta priorità a tutti gli utenti sul territorio. | UC-027 | US-015 | AC-US015-01…03 | MU-13 | |
| RF-028 | Pianificazione temporale dei viaggi | L'utente deve poter inserire la data e l'ora di una partenza futura e ricevere la stima del percorso e dei rallentamenti basata sui cantieri e le ordinanze già programmate per quel lasso di tempo. | UC-028 | US-006 | AC-US006-02, AC-US006-04 | MU-6 | |
| RF-029 | Chiusura e cancellazione dell'account | Il sistema deve consentire all'utente di richiedere la chiusura definitiva del proprio account con contestuale rimozione o anonimizzazione dei dati personali registrati. | UC-029 | US-003 | AC-US003-02…03 | MU-11 | |
| RF-030 | Ricerca indirizzi e punti di interesse | Il sistema deve consentire agli utenti di cercare indirizzi, toponimi e punti di interesse sul territorio, visualizzando i risultati sulla mappa e permettendo di avviare la navigazione o salvare la località tra i preferiti. | UC-030 | US-007 | AC-US007-01 | MU-6 | |
| RF-031 | Anteprima e simulazione del percorso | Prima di avviare la navigazione, l'utente deve poter visualizzare in anteprima il percorso calcolato e, opzionalmente, avviare una simulazione del viaggio per conoscerne i tempi e le indicazioni stradali. | UC-031 | US-007 | AC-US007-01, AC-US007-04 | MU-6 | |
| RF-032 | Navigazione turn-by-turn | Il sistema deve fornire indicazioni vocali e visive passo-passo durante la guida, aggiornando le istruzioni in tempo reale in base alla posizione dell'utente e alle variazioni del percorso. | UC-032 | US-007 | AC-US007-02…03 | MU-7 | |
| RF-033 | Integrazione con sistemi di infotainment veicolari | L'applicazione mobile deve supportare la proiezione dell'interfaccia di navigazione su Android Auto e Apple CarPlay, garantendo l'accesso alle funzioni essenziali in modalità hands-free. | UC-033 | US-008 | AC-US008-01…03 | - | |
| RF-034 | Gestione ruoli e permessi degli operatori | Il sistema deve permettere agli amministratori della PA di definire ruoli personalizzati e assegnare permessi granulari agli operatori (es. lettura, inserimento, modifica, validazione, pubblicazione). | UC-034 | US-017 | AC-US017-01 | - | |
| RF-035 | Log di audit operativi | Il sistema deve registrare in maniera non modificabile le azioni eseguite dagli operatori PA (inserimenti, modifiche, validazioni, pubblicazioni) per garantire tracciabilità e responsabilità. | UC-035 | US-017 | AC-US017-02…03 | - | |
| RF-036 | Importazione massiva dati | Il sistema deve consentire agli operatori PA di importare in blocco dati relativi a cantieri, ordinanze, chiusure e piani neve tramite file standard (es. CSV, GeoJSON, Shapefile). | UC-036 | US-018 | AC-US018-01…03 | MU-14 | |
| RF-037 | Accettazione dei termini e condizioni | Il sistema deve richiedere agli utenti l'accettazione dei termini di servizio e della privacy policy prima della registrazione e, in caso di aggiornamenti sostanziali, prima di continuare l'uso dell'app. | UC-037 | US-004 | AC-US004-01 | MU-2 | |
| RF-038 | Gestione consensi cookie e GDPR | Il sistema deve mostrare un banner informativo sui cookie e sui trattamenti dei dati, permettendo all'utente di gestire i consensi in modo granulare in conformità al GDPR. | UC-038 | US-004 | AC-US004-02 | MU-4 | |
| RF-039 | Applicazione delle politiche di retention | Il sistema deve applicare automaticamente le politiche di conservazione dei dati personali e delle segnalazioni, anonimizzando o eliminando le informazioni al termine dei periodi previsti dalle normative e dalle policy interne. | UC-039 | US-023 | AC-US023-01…03 | - | |
| RF-040 | Gestione avanzata delle notifiche | L'utente deve poter configurare in dettaglio le notifiche ricevute (tipologia di evento, canale, raggio di prossimità, fasce orarie di silenzio), nonché disattivarle temporaneamente. | UC-040 | US-011 | AC-US011-01…03 | MU-10 | |
| RF-041 | Notifiche contestuali multicanale | Il sistema deve inviare notifiche push o email in base alla natura e alla priorità dell'evento e alle preferenze dell'utente, mantenendo in ogni caso gli avvisi consultabili nel centro notifiche in-app. | UC-041 | US-011 | AC-US011-03…04 | MU-10 | |
| RF-042 | Cambio lingua | L'utente deve poter selezionare e cambiare istantaneamente la lingua dell'interfaccia tra Italiano, Inglese e Tedesco. Il sistema deve applicare la nuova lingua a tutti gli elementi dell'interfaccia grafica e ai messaggi di sistema senza necessità di riavvio. | UC-042 | US-020 | AC-US020-01…03 | MU-10 | |
| RF-043 | Autenticazione a più fattori per operatori PA | Il sistema deve richiedere l'autenticazione a più fattori (MFA) agli operatori della Pubblica Amministrazione per l'accesso alle funzionalità amministrative. | UC-043 | US-013 | AC-US013-01 | MU-12 | |
| RF-044 | Cifratura delle comunicazioni e dei dati sensibili | Il sistema deve cifrare tutte le comunicazioni tra client, server e servizi terzi utilizzando TLS 1.2 o superiore. I dati sensibili (password, token, posizioni GPS grezze, log di audit) devono essere crittografati a riposo con algoritmi conformi agli standard correnti. | UC-044 | US-024 | AC-US024-01…03 | - | |
| RF-045 | Controllo degli accessi alle funzionalità amministrative | L'accesso alle funzionalità amministrative da parte degli operatori PA deve essere soggetto a controllo degli accessi basato sui ruoli, garantendo che ciascun operatore possa utilizzare solo le funzioni autorizzate dal proprio profilo. | UC-045 | US-013 | AC-US013-01 | MU-12 | |
| RF-046 | Backup automatico dei dati critici | Il sistema deve effettuare backup automatici giornalieri dei dati critici (configurazioni, dati utente, eventi di viabilità, log di audit), con politica di retention conforme alle policy interne e normative. Almeno una copia di backup deve essere mantenuta in una zona geografica diversa da quella di produzione. | UC-046 | US-025 | AC-US025-01…04 | - | |
| RF-047 | Ingestion automatica di dati PA in formati standard | Il sistema deve essere in grado di ingerire e sincronizzare automaticamente i dati ufficiali provenienti dalla Pubblica Amministrazione nei formati standard CSV, GeoJSON e Shapefile. | UC-047 | US-012 | AC-US012-01…03 | - | |
| RF-048 | Documentazione e formati delle API pubbliche | Le API pubbliche del sistema devono essere documentate secondo lo standard OpenAPI 3.0 e devono restituire dati in formati JSON e/o GeoJSON. | UC-048 | US-019 | AC-US019-01 | - | |
| RF-049 | Integrazione con provider di identità standard | Il sistema deve supportare l'autenticazione tramite provider di identità che utilizzino protocolli standard come OAuth 2.0, OpenID Connect, SPID e CIE. | UC-049 | US-001 | AC-US001-01…03 | MU-2, MU-3 | UC-049 non ha AC dedicate |

---

## 4. Requisiti non funzionali

| ID | Titolo | Descrizione | Motivazione | UC | US | AC | MU | Note |
|---|---|---|---|---|---|---|---|---|
| RNF-001 | Compatibilità | Il sistema deve essere accessibile e pienamente operativo su dispositivi mobili e desktop. Per la versione web/web-app, deve garantire piena compatibilità con le ultime versioni dei principali browser (Mozilla Firefox v115+, Google Chrome/Chromium v115+, Apple Safari v16+, Microsoft Edge v115+). Per l'applicazione mobile native/cross-platform, deve supportare i sistemi operativi Android (dalla versione 10) e iOS (dalla versione 15). Il sistema deve garantire un'esperienza utente reattiva (responsive) e coerente, assicurando la corretta visualizzazione delle mappe dinamiche e il pieno funzionamento della geolocalizzazione indipendentemente dal dispositivo utilizzato. | Trattandosi di un'app per la mobilità, la maggior parte degli utenti consulta o segnala imprevisti in mobilità da smartphone, mentre gli operatori del Comune lavorano da postazioni desktop. È fondamentale garantire accessibilità universale senza costringere gli utenti ad aggiornare hardware o sistemi obsoleti. | UC-009, UC-017 | US-026 | AC-US026-01…03 | MU-1, MU-12 | |
| RNF-002 | Performance | Il sistema deve garantire tempi di risposta per il caricamento delle mappe e la visualizzazione degli eventi (deviazioni, cantieri, segnalazioni) inferiori a 1,5 secondi per il 95% delle richieste. L'invio di una nuova segnalazione (es. incidente, spazzaneve) da parte dell'utente o la pubblicazione di un avviso da parte del Comune deve essere elaborato e distribuito alla mappa in tempo reale (entro 3 secondi). Queste prestazioni devono essere mantenute con un carico continuo di almeno 500 utenti/operatori simultaneamente connessi. | In un contesto di viabilità e approccio predittivo, la tempestività dell'informazione è critica. Un ritardo nella visualizzazione di un cantiere o di un incidente rende l'informazione inutile per chi è alla guida. | UC-014, UC-017 | US-002, US-009, US-013, US-028 | AC-US002-04, AC-US009-04, AC-US013-03, AC-US028-03 | - | Coperti i tempi di caricamento (1,5 s) e di propagazione (3 s) |
| RNF-003 | Scalabilità | Il sistema deve scalare elasticamente per gestire picchi improvvisi fino a 50.000 utenti simultaneamente connessi e oltre 5.000 eventi/segnalazioni concorrenti sul territorio, mantenendo i livelli di prestazione indicati in RNF-002. | La domanda su un'app di viabilità non è uniforme: in caso di eventi atmosferici eccezionali (es. nevicate improvvise, emergenze) o durante le ore di punta cittadine, il traffico sull'app può centuplicarsi in pochi minuti. Il sistema cloud sottostante deve poter assorbire il carico senza rallentamenti. | - | US-028 | AC-US028-03 | - | |
| RNF-004 | Affidabilità e Disponibilità | Il sistema deve garantire una disponibilità minima del 99,9% su base annua (downtime massimo non pianificato inferiore a 8 ore e 46 minuti/anno). In caso di manutenzione programmata, questa deve avvenire esclusivamente in fasce orarie a basso traffico (es. 01:00 - 05:00) con preavviso agli operatori del Comune. | NoJam gestisce la viabilità e servizi essenziali (es. passaggi spazzaneve o chiusure strade per emergenze). L'indisponibilità della piattaforma durante un'emergenza viaria avrebbe un impatto negativo diretto sulla sicurezza e sul traffico cittadino. | - | US-028 | AC-US028-01…02 | - | |
| RNF-005 | Lingua e Localizzazione | Tutti gli elementi dell'interfaccia grafica e i messaggi di sistema devono essere completamente tradotti e localizzati nelle lingue supportate, garantendo coerenza terminologica e culturale. | Il Comune gestisce il traffico non solo per i residenti, ma anche per pendolari, turisti e autotrasportatori stranieri che attraversano il territorio del comune di Trento e che necessitano di essere immediatamente a conoscenza di avvisi e deviazioni. | UC-042 | US-020 | AC-US020-01, AC-US020-03 | MU-10 | |
| RNF-006 | Usabilità e Sicurezza d'Uso | L'interfaccia per i cittadini deve consentire l'invio di una segnalazione con un massimo di 3 tocchi/click (es. selezione tipo imprevisto + conferma posizione), utilizzabile intuitivamente entro 2 minuti dal primo avvio senza necessità di istruzioni. Per la componente di navigazione/consultazione alla guida, la visualizzazione deve essere ottimizzata a contrasto elevato con indicazioni sintetiche per ridurre al minimo le distrazioni. | Gli utenti inviano segnalazioni spesso dal veicolo (a veicolo fermo o tramite passeggero). Un processo di segnalazione lungo o complesso scoraggerebbe l'uso della funzione predittiva della community e aumenterebbe i rischi legati alla distrazione. | UC-014 | US-007, US-009 | AC-US007-05, AC-US009-01, AC-US009-05 | MU-5, MU-7 | |
| RNF-007 | Sicurezza e Privacy | Il sistema deve garantire la riservatezza, l'integrità e la disponibilità dei dati personali e delle credenziali degli utenti nel rispetto del GDPR, consentendo l'esercizio dei diritti dell'interessato (accesso, retifica, cancellazione, portabilità). | NoJam tratta dati personali (account, storico spostamenti, geolocalizzazione) e dati istituzionali sensibili. Una violazione o un accesso non autorizzato alla dashboard PA comprometterebbe la fiducia degli utenti e la sicurezza della viabilità gestita dal Comune. | UC-008, UC-029, UC-044 | US-003, US-004, US-024 | AC-US003-02…03, AC-US004-01, AC-US024-01…03 | MU-4, MU-11 | |
| RNF-008 | Accessibilità | L'interfaccia web e mobile deve rispettare i requisiti di accessibilità previsti dallo standard WCAG 2.1 livello AA. In particolare, devono essere garantiti: contrasto minimo 4.5:1 per il testo normale, navigazione completa tramite tastiera/screen reader, testi alternativi per le immagini e le icone, dimensioni dei caratteri ridimensionabili fino al 200% senza perdita di funzionalità, e supporto per le principiali tecnologie assistive. | NoJam è un servizio pubblico rivolto a tutti i cittadini, inclusi utenti con disabilità visive, motorie o cognitive. Garantire l'accessibilità non è solo un obbligo normativo per le PA, ma allarga la base di utenti attivi che contribuiscono con segnalazioni in tempo reale. | UC-014, UC-017 | US-027 | AC-US027-01…03 | - | |
| RNF-009 | Disaster Recovery | In caso di incidente deve essere garantito un Recovery Point Objective (RPO) massimo di 24 ore e un Recovery Time Objective (RTO) massimo di 4 ore per i servizi core. | In situazioni di emergenza viaria (nevicate, frane, allagamenti) l'indisponibilità prolungata o la perdita di dati sarebbero inaccettabili. Un piano di disaster recovery robusto integra il requisito di alta disponibilità già espresso in RNF-004. | UC-046 | US-025 | AC-US025-01…05 | - | |
| RNF-010 | Interoperabilità e Standard Aperti | Il sistema deve scambiare dati con enti territoriali, servizi di terze parti e sistemi esterni utilizzando formati e protocolli aperti e standard di settore riconosciuti, garantendo riusabilità, trasparenza e facilità di integrazione. | NoJam nasce per integrare fonti eterogenee (Comune, Provincia, servizi parcheggi/TPL) e per rilasciare Open Data verso terzi. L'adozione di standard aperti riduce i costi di integrazione, garantisce la riusabilità della piattaforma da parte di altri enti e sostiene gli obiettivi di Smart Governance e trasparenza. | UC-013, UC-026, UC-047, UC-048 | US-012, US-019 | AC-US012-01, AC-US019-01 | - | |

---

## 5. Mockup

Descrizione e collegamenti dei mockup in `d1/mockups/` e `d1/assets/mockups/`, come riportati in `d1/05-design-frontend.md`.

| MU | Schermata | Collegamenti principali |
|---|---|---|
| MU-1 | Mappa per utente anonimo | RF-006, RF-017; UC-006, UC-017; US-002 |
| MU-2 | Registrazione | RF-001, RF-037, RF-049; UC-001, UC-037, UC-049; US-001, US-004 |
| MU-3 | Accesso | RF-002, RF-049; UC-002, UC-049; US-001 |
| MU-4 | Consensi cookie e privacy | RF-008, RF-038; UC-008, UC-038; US-004 |
| MU-5 | Segnalazione di un evento in tempo reale | RF-009, RF-014; UC-009, UC-014; US-009; RNF-006 |
| MU-6 | Pianificazione e anteprima del percorso | RF-016, RF-028, RF-030, RF-031; UC-016, UC-028, UC-030, UC-031; US-006, US-007 |
| MU-7 | Navigazione turn-by-turn ad alto contrasto | RF-032; UC-032; US-007; RNF-006 |
| MU-8 | Livelli della mappa: piano neve e mobilità | RF-017, RF-018, RF-024; UC-017, UC-018, UC-024; US-014, US-022 |
| MU-9 | Profilo, reputazione e segnalazioni | RF-003, RF-011, RF-025; UC-003, UC-011, UC-025; US-010 |
| MU-10 | Notifiche e preferenze | RF-004, RF-010, RF-040, RF-041, RF-042; UC-004, UC-010, UC-040, UC-041, UC-042; US-011, US-020 |
| MU-11 | Privacy e gestione account | RF-007, RF-008, RF-029; UC-007, UC-008, UC-029; US-003 |
| MU-12 | Dashboard Smart Governance | RF-019, RF-022, RF-043, RF-045; UC-019, UC-022, UC-043, UC-045; US-013 |
| MU-13 | Attivazione Emergenza Viabilità | RF-027; UC-027; US-015 |
| MU-14 | Importazione massiva dei dati | RF-036; UC-036; US-018 |
