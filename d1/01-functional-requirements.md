# Requisiti Funzionali

<!-- I requisiti funzionali descrivono con sufficiente dettaglio le capacità e i comportamenti richiesti al sistema, comprese le informazioni coinvolte e le principali regole di dominio note. Non descrivono la sequenza delle interazioni utente-sistema, che viene dettagliata negli Use Case, né introducono proprietà qualitative del sistema, che appartengono ai requisiti non funzionali. -->

<!--
### RF-000: [Questo è un requisito di esempio]
[Descrizione del requisito funzionale: cosa deve fare il sistema, quali informazioni gestisce e quali regole di dominio applica.]
-->

### RF-001: Registrazione utente
Ogni utente deve potersi registrare all'interno dell'applicazione.

### RF-002: Login utente
Ogni utente deve potersi autenticare all'interno dell'applicazione tramite credenziali standard, provider terzi e sistemi di identità digitale.

### RF-003: Gestione del profilo
Il sistema deve consentire all'utente di consultare e aggiornare le informazioni del suo profilo.

### RF-004: Gestione preferenze
L'utente può gestire la lingua e le impostazioni generali dell'app.

### RF-005: Gestione luoghi e percorsi salvati
Il sistema deve consentire agli utenti di salvare, modificare ed eliminare luoghi preferiti e percorsi frequenti per accedere velocemente alla navigazione e ricevere notifiche.

### RF-006: Gestione dei permessi
Il sistema deve gestire i permessi in base al ruolo assegnato all'utente autenticato, mentre gli utenti non autenticati possono solo consultare la mappa.

### RF-007: Logout utente
Ogni utente deve potersi deautenticare all'interno dell'applicazione.

### RF-008: Gestione degli utenti e privacy
Il sistema deve permettere all'utente di visualizzare, prestare, modificare o revocare i consensi al trattamento dei dati personali e alla geolocalizzazione nel rispetto delle normative riguardanti il trattamento della privacy.

### RF-009: Gestione della geolocalizzazione
Il sistema deve tracciare la posizione corrente in tempo reale sulla mappa, centrare la vista, inviare segnalazioni di geolocalizzazione e alert di prossimità.

### RF-010: Gestione del canale di notifiche
L'utente deve poter abilitare, disabilitare e filtrare le notifiche.

### RF-011: Cronologia delle segnalazioni
Ogni utente deve poter vedere lo storico delle segnalazioni, visualizzandone lo stato di approvazione/validazione, l'impatto della community e il proprio punteggio di reputazione.

### RF-012: Gestione di feedback ed errori
Il sistema deve fornire un canale di comunicazione tra utenti e supporto tecnico per segnalare bug e inviare feedback.

### RF-013: Ingestion e sincronizzazione dati ufficiali PA
Il sistema deve importare, strutturare e sincronizzare automaticamente i dati statici e programmati provenienti dagli enti territoriali (Comune e Provincia), inclusi cantieri stradali, chiusure temporanee, ordinanze di viabilità e piani di sgombero neve.

### RF-014: Invio segnalazioni in tempo reale
L'utente autenticato deve poter inviare segnalazioni geolocalizzate in tempo reale relative a eventi dinamici sulla rete stradale, come incidenti, rallentamenti, presenza di ostacoli, condizioni meteo avverse o passaggi/presenza dei mezzi spazzaneve.

### RF-015: Algoritmo di valutazione affidabilità e reputation scoring
Il sistema deve incrociare le segnalazioni inviate dagli utenti con i dati ufficiali della Pubblica Amministrazione e con altre segnalazioni concorrenti per calcolare un indice di affidabilità del dato (peso dell'evento) e aggiornare il punteggio di reputazione dell'utente segnalatore.

### RF-016: Calcolo e ottimizzazione percorsi (Routing predittivo)
Il sistema deve calcolare e proporre itinerari di navigazione che tengano conto sia delle limitazioni ufficiali programmate (cantieri, ordinanze, zone d'intervento neve) sia delle criticità temporanee convalidate in tempo reale dalla community.

### RF-017: Consultazione mappa interattiva ed eventi
Il sistema deve consentire sia agli utenti autenticati che a quelli non autenticati di consultare la mappa interattiva del territorio, visualizzando lo stato del traffico, i cantieri attivi, i dettagli delle ordinanze comunali, i percorsi dello sgombero neve e le allerte confermate.

### RF-018: Gestione del piano neve e tracciamento mezzi
Il sistema deve gestire la mappa e la pianificazione dello sgombero neve, permettendo la visualizzazione dei percorsi degli spazzaneve.

### RF-019: Pannello di gestione e Smart Governance per la PA
Il sistema deve mettere a disposizione degli operatori accreditati del Comune e della Provincia una dashboard amministrativa per pubblicare ordinanze, inserire o modificare manualmente cantieri ed emergenze, validare segnalazioni e supervisionare la viabilità.

### RF-020: Analisi aggregate e reporting del traffico
Il sistema deve elaborare ed evidenziare dati aggregati ed anonimizzati sui flussi di traffico, sui tempi di percorrenza e sulle criticità ricorrenti, offrendo all'amministrazione report e analisi utili alla pianificazione urbana.

### RF-021: Notifiche e allerte preventive di mobilità
Il sistema deve inviare notifiche push o alert di prossimità tempestivi per avvisare gli utenti di cantieri programmati lungo i percorsi salvati o chiusure stradali improvvise prima dell'inizio del viaggio.

### RF-022: Moderazione e gestione dei contenuti
Il sistema deve permettere agli operatori PA o ai moderatori di confermare, modificare, annullare o rimuovere segnalazioni errate, obsolete o improprie inviate dagli utenti.

### RF-023: Modalità di guida offline parziale
Il sistema deve consentire il salvataggio in locale (caching) delle mappe e dei percorsi frequenti per garantirne la consultazione e il routing base anche in caso di disconnessione o copertura di rete limitata.

### RF-024: Integrazione con trasporti pubblici e parcheggi
Il sistema deve mostrare sulla mappa punti d'interesse come la disponibilità in tempo reale dei parcheggi cittadini.

### RF-025: Sistema di badge e gamification della reputazione
Il sistema deve attribuire punti, livelli o badge di affidabilità agli utenti in base all'accuratezza delle segnalazioni confermate, incentivando la partecipazione attiva della community.

### RF-026: Esportazione ed API dati aperti (Open Data)
Il sistema deve esporre API pubbliche e formati standard per consentire la ricondivisione in formato Open Data dei dati di viabilità ed eventi elaborati verso terzi o altri portali istituzionali.

### RF-027: Gestione eventi straordinari ed emergenze
Il sistema deve permettere alla PA di attivare una modalità di "Emergenza Viabilità" (es. nevicate eccezionali, frane, allagamenti) per evidenziare immediatamente allerte ad alta priorità a tutti gli utenti sul territorio.

### RF-028: Pianificazione temporale dei viaggi
L'utente deve poter inserire la data e l'ora di una partenza futura e ricevere la stima del percorso e dei rallentamenti basata sui cantieri e le ordinanze già programmate per quel lasso di tempo.

### RF-029: Chiusura e cancellazione dell'account
Il sistema deve consentire all'utente di richiedere la chiusura definitiva del proprio account con contestuale rimozione o anonimizzazione dei dati personali registrati.

### RF-030: Ricerca indirizzi e punti di interesse
Il sistema deve consentire agli utenti di cercare indirizzi, toponimi e punti di interesse sul territorio, visualizzando i risultati sulla mappa e permettendo di avviare la navigazione o salvare la località tra i preferiti.

### RF-031: Anteprima e simulazione del percorso
Prima di avviare la navigazione, l'utente deve poter visualizzare in anteprima il percorso calcolato e, opzionalmente, avviare una simulazione del viaggio per conoscerne i tempi e le indicazioni stradali.

### RF-032: Navigazione turn-by-turn
Il sistema deve fornire indicazioni vocali e visive passo-passo durante la guida, aggiornando le istruzioni in tempo reale in base alla posizione dell'utente e alle variazioni del percorso.

### RF-033: Integrazione con sistemi di infotainment veicolari
L'applicazione mobile deve supportare la proiezione dell'interfaccia di navigazione su Android Auto e Apple CarPlay, garantendo l'accesso alle funzioni essenziali in modalità hands-free.

### RF-034: Gestione ruoli e permessi degli operatori
Il sistema deve permettere agli amministratori della PA di definire ruoli personalizzati e assegnare permessi granulari agli operatori (es. lettura, inserimento, modifica, validazione, pubblicazione).

### RF-035: Log di audit operativi
Il sistema deve registrare in maniera non modificabile le azioni eseguite dagli operatori PA (inserimenti, modifiche, validazioni, pubblicazioni) per garantire tracciabilità e responsabilità.

### RF-036: Importazione massiva dati
Il sistema deve consentire agli operatori PA di importare in blocco dati relativi a cantieri, ordinanze, chiusure e piani neve tramite file standard (es. CSV, GeoJSON, Shapefile).

### RF-037: Accettazione dei termini e condizioni
Il sistema deve richiedere agli utenti l'accettazione dei termini di servizio e della privacy policy prima della registrazione e, in caso di aggiornamenti sostanziali, prima di continuare l'uso dell'app.

### RF-038: Gestione consensi cookie e GDPR
Il sistema deve mostrare un banner informativo sui cookie e sui trattamenti dei dati, permettendo all'utente di gestire i consensi in modo granulare in conformità al GDPR.

### RF-039: Applicazione delle politiche di retention
Il sistema deve applicare automaticamente le politiche di conservazione dei dati personali e delle segnalazioni, anonimizzando o eliminando le informazioni al termine dei periodi previsti dalle normative e dalle policy interne.

### RF-040: Gestione avanzata delle notifiche
L'utente deve poter configurare in dettaglio le notifiche ricevute (tipologia di evento, canale, raggio di prossimità, fasce orarie di silenzio), nonché disattivarle temporaneamente.

### RF-041: Notifiche contestuali multicanale
Il sistema deve inviare notifiche push, email o SMS in base alla natura dell'evento e alle preferenze dell'utente, garantendo la comunicazione anche in assenza di connessione dati attiva sull'app.