# Use Case Diagram

<!-- Gli Use Case e i relativi diagrammi dettagliano le interazioni tra attori e sistema a partire dai requisiti funzionali e mantengono riferimenti espliciti agli RF correlati. Ogni Use Case utilizza un identificatore stabile UC-XXX, indipendente dagli identificatori RF-*, e indica i requisiti a cui contribuisce. Non esiste una corrispondenza 1:1 tra requisiti e Use Case: uno stesso RF può essere dettagliato da più Use Case e uno Use Case può contribuire a più RF. -->

<!--
### [Attore 1]
- UC-000: [titolo use case] - RF-000
- UC-010: [titolo use case] - RF-010

![](use case diagram 1)

#### Use case UC-000: [titolo use case]
**Riassunto:** [breve descrizione dell'obiettivo dello use case.]

**Descrizione:**
1. [Primo passo dell'interazione.]
2. [Secondo passo dell'interazione.]
3. [Terzo passo dell'interazione.]

**Eccezioni:**
1. [Condizione di errore e comportamento del sistema.]
2. [Altra condizione di errore.]

**Estensioni:**
1. [Comportamento alternativo opzionale.]
-->

## Attori

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

### Utente Anonimo
- UC-001: Registrazione utente - RF-001
- UC-002: Login utente - RF-002
- UC-006: Gestione dei permessi - RF-006
- UC-017: Consultazione mappa interattiva ed eventi - RF-017

```mermaid
usecase-beta
direction LR

actor Utente("Utente")
actor UtenteAnonimo("Utente Anonimo")
actor UtenteAutenticato("Utente Autenticato")
actor ProviderIdentita("Provider di Identità")
actor ServizioEmail("Servizio Email")

systemBoundary NoJam["NoJam"]
  UC001("UC-001: Registrazione utente")
  UC002("UC-002: Login utente")
  UC006("UC-006: Gestione dei permessi")
  UC017("UC-017: Consultazione mappa interattiva ed eventi")

  UC001a("Verifica dell'indirizzo email")
  UC002a("Recupero della password")
  UC006a("Richiesta di elevazione del ruolo")
  UC017a("Filtro dei livelli della mappa")
end

UtenteAnonimo --|> Utente
UtenteAutenticato --|> Utente

UtenteAnonimo --> UC001
UtenteAnonimo --> UC002
Utente --> UC017
UtenteAutenticato --> UC006

UC001 ..> : include UC001a
UC002 ..> : include UC006
UC001a --> ServizioEmail
UC002 --> ProviderIdentita

UC002a ..> : extend UC002
UC006a ..> : extend UC006
UC017a ..> : extend UC017
```

#### Use case UC-001: Registrazione utente
**Riassunto:** Consente a un utente non autenticato di creare un account sulla piattaforma, tramite inserimento dei propri dati o tramite un provider di identità esterno.

**Descrizione:**
1. L'utente anonimo seleziona la funzionalità di registrazione.
2. L'utente inserisce i dati richiesti (nome, cognome, email, data di nascita) oppure seleziona un provider di identità esterno.
3. Il sistema convalida i dati e invia un'email di verifica tramite il Servizio Email.
4. L'utente conferma l'indirizzo e completa la creazione dell'account.

**Eccezioni:**
1. Dati non validi o indirizzo email già registrato: il sistema mostra un messaggio di errore e richiede la correzione.
2. Servizio Email non raggiungibile: l'account resta in stato "non verificato" e il sistema consente di richiedere un nuovo invio.

**Estensioni:**
1. Inserimento facoltativo del numero di telefono durante la procedura.

#### Use case UC-002: Login utente
**Riassunto:** Consente all'utente di autenticarsi tramite credenziali standard, provider terzi o sistemi di identità digitale.

**Descrizione:**
1. L'utente inserisce le proprie credenziali oppure seleziona l'autenticazione tramite provider terzo (Google SSO, SPID, CIE).
2. Il sistema o il provider di identità verifica l'identità dell'utente.
3. A verifica superata il sistema apre la sessione e assegna i permessi correlati al ruolo (include UC-006).

**Eccezioni:**
1. Credenziali errate o autenticazione rifiutata dal provider: il sistema mostra un messaggio di errore e non apre la sessione.
2. Account non ancora verificato: il sistema blocca l'accesso e propone il rinvio dell'email di verifica.

**Estensioni:**
1. Recupero della password dimenticata tramite email.

#### Use case UC-006: Gestione dei permessi
**Riassunto:** Determina le funzionalità accessibili in base allo stato di autenticazione e al ruolo dell'utente.

**Descrizione:**
1. L'utente accede all'applicazione.
2. Il sistema verifica se l'utente è autenticato e quale ruolo gli è assegnato.
3. Se non autenticato, il sistema abilita la sola consultazione della mappa; se autenticato, abilita le funzionalità previste dal ruolo.

**Eccezioni:**
1. Tentativo di accesso a funzionalità riservate da parte di un utente non autorizzato: il sistema blocca l'operazione e richiede l'autenticazione.

**Estensioni:**
1. Richiesta di elevazione del ruolo a Operatore PA, soggetta ad approvazione.

#### Use case UC-017: Consultazione mappa interattiva ed eventi
**Riassunto:** Permette la libera consultazione della mappa del territorio con evidenza di traffico, cantieri, ordinanze e allerte.

**Descrizione:**
1. L'utente, autenticato o meno, apre la sezione Mappa dell'applicazione.
2. Il sistema visualizza i livelli attivi: flussi di traffico, cantieri, ordinanze comunali, zone del piano neve e allerte convalidate.
3. L'utente naviga la mappa e seleziona i singoli elementi per visualizzarne i dettagli.

**Eccezioni:**
1. Errore nel caricamento delle tessere della mappa: il sistema mostra l'ultima versione in cache e segnala lo stato di disconnessione.

**Estensioni:**
1. Filtro dei livelli e delle categorie di contenuto visualizzati sulla mappa.

### Utente Autenticato - Account e privacy
- UC-003: Gestione del profilo - RF-003
- UC-004: Gestione preferenze - RF-004
- UC-007: Logout utente - RF-007
- UC-008: Gestione degli utenti e privacy - RF-008
- UC-010: Gestione del canale di notifiche - RF-010
- UC-012: Gestione di feedback ed errori - RF-012
- UC-029: Chiusura e cancellazione dell'account - RF-029

```mermaid
usecase-beta
direction LR

actor UtenteAutenticato("Utente Autenticato")
actor ServizioEmail("Servizio Email")
actor SupportoTecnico("Supporto Tecnico")

systemBoundary NoJam["NoJam"]
  UC003("UC-003: Gestione del profilo")
  UC004("UC-004: Gestione preferenze")
  UC007("UC-007: Logout utente")
  UC008("UC-008: Gestione degli utenti e privacy")
  UC010("UC-010: Gestione del canale di notifiche")
  UC012("UC-012: Gestione di feedback ed errori")
  UC029("UC-029: Chiusura e cancellazione dell'account")

  UC003a("Modifica della foto del profilo")
  UC004a("Ripristino delle impostazioni predefinite")
  UC007a("Disconnessione automatica per inattività")
  UC008a("Esportazione dei dati personali")
  UC010a("Selezione dei canali e delle categorie di allerta")
  UC012a("Tracciamento dello stato del ticket")
  UC029a("Verifica dell'identità")
  UC029b("Anonimizzazione dei dati contributivi")
end

UtenteAutenticato --> UC003
UtenteAutenticato --> UC004
UtenteAutenticato --> UC007
UtenteAutenticato --> UC008
UtenteAutenticato --> UC010
UtenteAutenticato --> UC012
UtenteAutenticato --> UC029

UC010 ..> : include UC010a
UC029 ..> : include UC029a
UC029 ..> : include UC029b
UC029 ..> : include UC007

UC003a ..> : extend UC003
UC004a ..> : extend UC004
UC007a ..> : extend UC007
UC008a ..> : extend UC008
UC012a ..> : extend UC012

UC012 --> SupportoTecnico
UC029a --> ServizioEmail
UC008a --> ServizioEmail
```

#### Use case UC-003: Gestione del profilo
**Riassunto:** Consente all'utente autenticato di consultare e aggiornare le informazioni del proprio profilo.

**Descrizione:**
1. L'utente accede alla sezione dedicata al proprio profilo.
2. Il sistema mostra i dati personali memorizzati.
3. L'utente modifica le informazioni desiderate e salva.
4. Il sistema convalida e aggiorna i dati a profilo.

**Eccezioni:**
1. Validazione dei nuovi dati fallita: il sistema impedisce il salvataggio e segnala i campi non corretti.

**Estensioni:**
1. Modifica della foto del profilo e delle informazioni secondarie.

#### Use case UC-004: Gestione preferenze
**Riassunto:** Permette all'utente di gestire la lingua dell'interfaccia e le impostazioni generali dell'applicazione.

**Descrizione:**
1. L'utente accede alla sezione Impostazioni.
2. L'utente modifica la lingua o le preferenze visive e funzionali.
3. Il sistema applica le modifiche e le rende persistenti per le sessioni successive.

**Eccezioni:**
1. Errore nel salvataggio lato server: le preferenze restano applicate solo in locale per la sessione corrente.

**Estensioni:**
1. Ripristino delle impostazioni predefinite.

#### Use case UC-007: Logout utente
**Riassunto:** Consente la deautenticazione sicura dell'utente dall'applicazione.

**Descrizione:**
1. L'utente seleziona l'opzione di logout dal menu utente.
2. Il sistema invalida il token di sessione.
3. L'utente viene reindirizzato alla vista mappa per utenti non autenticati.

**Eccezioni:**
1. Errore di rete durante la richiesta: il sistema elimina comunque la sessione locale e avvisa l'utente.

**Estensioni:**
1. Disconnessione automatica dopo un periodo prolungato di inattività.

#### Use case UC-008: Gestione degli utenti e privacy
**Riassunto:** Permette all'utente di consultare e gestire i consensi relativi al trattamento dei dati personali e alla geolocalizzazione.

**Descrizione:**
1. L'utente accede alla sezione "Privacy e dati personali".
2. Il sistema visualizza lo stato corrente dei consensi rilasciati.
3. L'utente presta, modifica o revoca i singoli consensi (es. tracciamento continuo, invio dati analitici).
4. Il sistema salva le preferenze aggiornando il registro dei consensi.

**Eccezioni:**
1. Revoca di un consenso essenziale (es. geolocalizzazione): il sistema avvisa che le funzionalità dipendenti verranno disabilitate.

**Estensioni:**
1. Richiesta di esportazione dei dati personali, recapitata tramite il Servizio Email.

#### Use case UC-010: Gestione del canale di notifiche
**Riassunto:** Consente all'utente di abilitare, disabilitare e filtrare i canali e le tipologie di notifica inviate dall'applicazione.

**Descrizione:**
1. L'utente accede alla sezione "Impostazioni notifiche".
2. L'utente seleziona i canali (push, email) e le categorie di allerta desiderate (cantieri, emergenze meteo, piano neve).
3. Il sistema memorizza le preferenze e le applica ai successivi invii.

**Eccezioni:**
1. Permessi di notifica negati a livello di sistema operativo: l'applicazione segnala la necessità di abilitarli nelle impostazioni del dispositivo.

**Estensioni:**
1. Silenziamento temporaneo di tutte le notifiche non critiche.

#### Use case UC-012: Gestione di feedback ed errori
**Riassunto:** Fornisce un canale di comunicazione diretto tra utenti e Supporto Tecnico per l'invio di bug report e suggerimenti.

**Descrizione:**
1. L'utente seleziona la funzione "Invia feedback / Segnala un problema".
2. L'utente descrive l'anomalia o il suggerimento, allegando facoltativamente screenshot o log.
3. Il sistema inoltra il report al Supporto Tecnico e restituisce all'utente una conferma con il riferimento della richiesta.

**Eccezioni:**
1. Errore di connessione durante l'invio: il report viene salvato in locale e reinviato alla prima connessione disponibile.

**Estensioni:**
1. Tracciamento dello stato del ticket di supporto aperto.

#### Use case UC-029: Chiusura e cancellazione dell'account
**Riassunto:** Consente all'utente di richiedere la chiusura definitiva del proprio account, con rimozione o anonimizzazione dei dati personali.

**Descrizione:**
1. L'utente accede alle impostazioni di sicurezza dell'account e seleziona "Elimina account".
2. Il sistema richiede la conferma dell'identità tramite password oppure codice inviato via email.
3. L'utente conferma definitivamente la richiesta.
4. Il sistema chiude la sessione attiva (include UC-007), elimina l'account e anonimizza i contributi pubblicati, in conformità al GDPR.

**Eccezioni:**
1. Verifica dell'identità non superata: l'operazione viene annullata e l'account resta attivo.

**Estensioni:**
1. Invio di un'email di conferma dell'avvenuta eliminazione.

### Utente - Consensi e conformità normativa
- UC-037: Accettazione dei termini e condizioni - RF-037
- UC-038: Gestione consensi cookie e GDPR - RF-038
- UC-039: Applicazione delle politiche di retention - RF-039

```mermaid
usecase-beta
direction LR

actor Utente("Utente")
actor UtenteAnonimo("Utente Anonimo")
actor UtenteAutenticato("Utente Autenticato")
actor AmministratorePA("Amministratore PA")

systemBoundary NoJam["NoJam"]
  UC037("UC-037: Accettazione dei termini e condizioni")
  UC038("UC-038: Gestione consensi cookie e GDPR")
  UC039("UC-039: Applicazione delle politiche di retention")
  UC001("UC-001: Registrazione utente")
  UC008("UC-008: Gestione degli utenti e privacy")

  UC037a("Consultazione del testo integrale")
  UC037b("Ri-accettazione dopo un aggiornamento sostanziale")
  UC038a("Scelta granulare per singola finalità")
  UC038b("Revisione dei consensi dal pannello privacy")
  UC039a("Anonimizzazione delle segnalazioni storiche")
  UC039b("Configurazione dei periodi di conservazione")
end

UtenteAnonimo --|> Utente
UtenteAutenticato --|> Utente

UtenteAnonimo --> UC001
Utente --> UC038
UtenteAutenticato --> UC008
AmministratorePA --> UC039

UC001 ..> : include UC037
UC037 ..> : include UC037a
UC038 ..> : include UC038a
UC039 ..> : include UC039a

UC037b ..> : extend UC037
UC038b ..> : extend UC008
UC039b ..> : extend UC039
```

#### Use case UC-037: Accettazione dei termini e condizioni
**Riassunto:** Richiede all'utente l'accettazione esplicita dei termini di servizio e della privacy policy prima della registrazione e a ogni aggiornamento sostanziale.

**Descrizione:**
1. Durante la registrazione (UC-001) il sistema presenta all'utente i termini di servizio e la privacy policy in vigore.
2. L'utente consulta i documenti e ne accetta esplicitamente il contenuto.
3. Il sistema registra la versione accettata, la data e l'ora del consenso, e completa l'operazione in corso.

**Eccezioni:**
1. Mancata accettazione: il sistema interrompe la registrazione e non crea l'account.
2. Accettazione negata a seguito di un aggiornamento: il sistema limita l'uso dell'applicazione alle sole funzionalità consultabili senza autenticazione.

**Estensioni:**
1. Richiesta di ri-accettazione al primo accesso successivo alla pubblicazione di una nuova versione dei documenti.

#### Use case UC-038: Gestione consensi cookie e GDPR
**Riassunto:** Informa l'utente sui cookie e sui trattamenti effettuati e gli consente di prestare o negare i consensi in modo granulare.

**Descrizione:**
1. Al primo accesso il sistema mostra il banner informativo su cookie e finalità di trattamento.
2. L'utente accetta tutto, rifiuta i trattamenti non necessari oppure apre il pannello di scelta granulare.
3. Il sistema applica immediatamente le scelte, attivando solo i servizi consentiti, e ne conserva traccia nel registro dei consensi.

**Eccezioni:**
1. Nessuna scelta effettuata: il sistema attiva i soli cookie tecnici necessari e ripropone il banner all'accesso successivo.

**Estensioni:**
1. Revisione e revoca dei consensi in qualsiasi momento dalla sezione "Privacy e dati personali" (UC-008).

#### Use case UC-039: Applicazione delle politiche di retention
**Riassunto:** Applica automaticamente i periodi di conservazione previsti, anonimizzando o eliminando dati personali e segnalazioni scadute.

**Descrizione:**
1. Il sistema esegue periodicamente il controllo delle scadenze definite dalle policy di conservazione.
2. Il sistema individua dati personali, tracciati di posizione e segnalazioni che hanno superato il periodo previsto.
3. Il sistema anonimizza o elimina i record interessati e registra l'operazione nel log di audit (UC-035).

**Eccezioni:**
1. Dato soggetto a un obbligo di conservazione prevalente (es. contenzioso in corso): il record viene escluso dalla cancellazione e marcato come trattenuto.
2. Esecuzione interrotta da un errore: il sistema riprende dal punto di interruzione al ciclo successivo e segnala l'anomalia all'Amministratore PA.

**Estensioni:**
1. Configurazione dei periodi di conservazione per tipologia di dato da parte dell'Amministratore PA.

### Utente Autenticato - Mappa e navigazione
- UC-005: Gestione luoghi e percorsi salvati - RF-005
- UC-009: Gestione della geolocalizzazione - RF-009
- UC-016: Calcolo e ottimizzazione percorsi - RF-016
- UC-021: Notifiche e allerte preventive di mobilità - RF-021
- UC-023: Modalità di guida offline parziale - RF-023
- UC-024: Integrazione con trasporti pubblici e parcheggi - RF-024
- UC-028: Pianificazione temporale dei viaggi - RF-028

```mermaid
usecase-beta
direction LR

actor UtenteAutenticato("Utente Autenticato")
actor ServizioPosizione("Servizio di Posizione")
actor ServiziMobilita("Servizi Parcheggi e TPL")

systemBoundary NoJam["NoJam"]
  UC005("UC-005: Gestione luoghi e percorsi salvati")
  UC009("UC-009: Gestione della geolocalizzazione")
  UC016("UC-016: Calcolo e ottimizzazione percorsi")
  UC021("UC-021: Notifiche e allerte preventive di mobilità")
  UC023("UC-023: Modalità di guida offline parziale")
  UC024("UC-024: Integrazione con trasporti pubblici e parcheggi")
  UC028("UC-028: Pianificazione temporale dei viaggi")

  UC005a("Salvataggio, modifica ed eliminazione di un elemento")
  UC009a("Centratura della vista sulla posizione corrente")
  UC016a("Interrogazione di cantieri e ordinanze vigenti")
  UC016b("Integrazione delle criticità convalidate")
  UC021a("Alert di prossimità a un ostacolo attivo")
  UC023a("Download in locale di mappe e percorsi")
  UC024a("Filtro per ricarica EV e stalli riservati")
  UC028a("Promemoria automatico prima della partenza")
end

UtenteAutenticato --> UC005
UtenteAutenticato --> UC009
UtenteAutenticato --> UC016
UtenteAutenticato --> UC021
UtenteAutenticato --> UC023
UtenteAutenticato --> UC024
UtenteAutenticato --> UC028

UC009 --> ServizioPosizione
UC024 --> ServiziMobilita

UC005 ..> : include UC005a
UC009 ..> : include UC009a
UC016 ..> : include UC009
UC016 ..> : include UC016a
UC016 ..> : include UC016b
UC021 ..> : include UC005
UC021 ..> : include UC009
UC023 ..> : include UC023a

UC021a ..> : extend UC021
UC023 ..> : extend UC016
UC024a ..> : extend UC024
UC028 ..> : extend UC016
UC028a ..> : extend UC028
```

#### Use case UC-005: Gestione luoghi e percorsi salvati
**Riassunto:** Consente all'utente di salvare, modificare ed eliminare luoghi preferiti e percorsi frequenti.

**Descrizione:**
1. L'utente seleziona un punto sulla mappa o un itinerario calcolato.
2. L'utente lo aggiunge ai propri salvataggi assegnandogli un'etichetta, oppure modifica o rimuove un elemento esistente.
3. Il sistema aggiorna l'elenco dei salvataggi, rendendoli disponibili per l'accesso rapido e per le notifiche preventive.

**Eccezioni:**
1. Raggiungimento del numero massimo di elementi salvabili: il sistema invita l'utente a rimuovere elementi esistenti prima di aggiungerne di nuovi.

**Estensioni:**
1. Download in locale del luogo o del percorso per l'utilizzo offline (UC-023).

#### Use case UC-009: Gestione della geolocalizzazione
**Riassunto:** Traccia la posizione corrente in tempo reale per centrare la mappa, geolocalizzare le segnalazioni e generare alert di prossimità.

**Descrizione:**
1. L'utente concede il permesso di accesso alla posizione del dispositivo.
2. Il sistema acquisisce la posizione dal Servizio di Posizione e centra la vista della mappa.
3. Il sistema confronta la posizione corrente con gli eventi e i pericoli attivi nelle vicinanze.

**Eccezioni:**
1. Segnale GPS assente o permessi negati dal sistema operativo: il sistema avvisa l'utente e mantiene la mappa sull'ultima posizione nota.

**Estensioni:**
1. Generazione di un alert quando l'utente si avvicina a un ostacolo o a un cantiere attivo (UC-021).

#### Use case UC-016: Calcolo e ottimizzazione percorsi
**Riassunto:** Calcola itinerari di navigazione tenendo conto di cantieri, ordinanze della PA e criticità convalidate in tempo reale dalla community.

**Descrizione:**
1. L'utente inserisce il punto di partenza e la destinazione.
2. Il sistema interroga il motore di routing integrando le limitazioni ufficiali programmate e le criticità stradali convalidate.
3. Il sistema propone uno o più itinerari ottimizzati, indicando tempi di percorrenza e rallentamenti previsti.
4. L'utente seleziona un itinerario e avvia la navigazione.

**Eccezioni:**
1. Nessun percorso percorribile a causa di chiusure totali: il sistema informa l'utente dell'impossibilità di raggiungere la destinazione.

**Estensioni:**
1. Impostazione di una data e di un'ora di partenza futura (UC-028).
2. Prosecuzione della navigazione in assenza di connettività (UC-023).

#### Use case UC-021: Notifiche e allerte preventive di mobilità
**Riassunto:** Avvisa l'utente di cantieri programmati sui percorsi salvati e di chiusure improvvise, prima o durante lo spostamento.

**Descrizione:**
1. Il sistema confronta periodicamente i percorsi e i luoghi salvati dall'utente con cantieri, ordinanze ed eventi convalidati.
2. Al rilevamento di un impatto sulla mobilità dell'utente, il sistema compone la notifica rispettando i filtri impostati in UC-010.
3. Il sistema recapita la notifica sul canale scelto, con un anticipo sufficiente rispetto all'orario abituale di partenza.

**Eccezioni:**
1. Notifiche disabilitate o canale non raggiungibile: l'avviso resta consultabile nel centro notifiche in-app.

**Estensioni:**
1. Alert di prossimità emesso quando l'utente in viaggio si avvicina a un ostacolo attivo.

#### Use case UC-023: Modalità di guida offline parziale
**Riassunto:** Garantisce la consultazione della mappa e il routing di base anche in assenza di connettività, tramite caching locale.

**Descrizione:**
1. Il sistema scarica in background la mappa dell'area di interesse e i percorsi salvati dall'utente.
2. In caso di perdita della connessione, l'applicazione passa automaticamente alla modalità offline.
3. L'utente prosegue la navigazione di base e la consultazione della mappa memorizzata in locale.

**Eccezioni:**
1. Spazio di archiviazione insufficiente: il sistema notifica l'impossibilità di completare il salvataggio e indica lo spazio necessario.

**Estensioni:**
1. Sincronizzazione automatica dei dati e delle segnalazioni accodate al ripristino della connettività.

#### Use case UC-024: Integrazione con trasporti pubblici e parcheggi
**Riassunto:** Mostra sulla mappa la disponibilità in tempo reale dei parcheggi cittadini e i punti di interesse del trasporto pubblico.

**Descrizione:**
1. L'utente attiva il livello "Parcheggi e mobilità pubblica" sulla mappa.
2. Il sistema interroga i Servizi Parcheggi e TPL e mostra l'occupazione delle aree di sosta e la posizione delle linee.
3. L'utente seleziona un parcheggio per ottenerne le indicazioni stradali (UC-016).

**Eccezioni:**
1. Dati in tempo reale non disponibili: il sistema mostra l'ultimo valore noto specificando l'orario del rilevamento.

**Estensioni:**
1. Filtro dei parcheggi per presenza di colonnine di ricarica EV o stalli riservati.

#### Use case UC-028: Pianificazione temporale dei viaggi
**Riassunto:** Consente di indicare una data e un'ora di partenza futura per stimare percorso e rallentamenti sulla base delle ordinanze già programmate.

**Descrizione:**
1. L'utente imposta origine, destinazione e un orario di partenza futuro.
2. Il sistema analizza il calendario delle ordinanze e dei cantieri programmati per quella data.
3. Il sistema calcola l'itinerario ottimale previsto ed evidenzia i rallentamenti attesi in quella fascia oraria.

**Eccezioni:**
1. Programmazione non ancora pubblicata per la data richiesta: il sistema fornisce una stima basata sui dati storici di viabilità, segnalandone la natura indicativa.

**Estensioni:**
1. Salvataggio del viaggio pianificato con promemoria automatico prima della partenza.

### Utente - Ricerca e navigazione assistita
- UC-030: Ricerca indirizzi e punti di interesse - RF-030
- UC-031: Anteprima e simulazione del percorso - RF-031
- UC-032: Navigazione turn-by-turn - RF-032
- UC-033: Integrazione con sistemi di infotainment veicolari - RF-033

```mermaid
usecase-beta
direction LR

actor Utente("Utente")
actor UtenteAutenticato("Utente Autenticato")
actor ServizioGeocoding("Servizio di Geocoding")
actor SistemaInfotainment("Sistema di Infotainment")

systemBoundary NoJam["NoJam"]
  UC030("UC-030: Ricerca indirizzi e punti di interesse")
  UC031("UC-031: Anteprima e simulazione del percorso")
  UC032("UC-032: Navigazione turn-by-turn")
  UC033("UC-033: Integrazione con sistemi di infotainment")
  UC005("UC-005: Gestione luoghi e percorsi salvati")
  UC009("UC-009: Gestione della geolocalizzazione")
  UC016("UC-016: Calcolo e ottimizzazione percorsi")

  UC030a("Geocodifica e ordinamento per prossimità")
  UC030b("Salvataggio del risultato tra i preferiti")
  UC031a("Simulazione animata del viaggio")
  UC032a("Guida vocale e indicazioni visive")
  UC032b("Ricalcolo automatico in caso di deviazione")
  UC033a("Comandi vocali in modalità hands-free")
end

UtenteAutenticato --|> Utente

Utente --> UC030
UtenteAutenticato --> UC031
UtenteAutenticato --> UC032
UtenteAutenticato --> UC033

UC030 ..> : include UC030a
UC030a --> ServizioGeocoding
UC032 ..> : include UC016
UC032 ..> : include UC009
UC032 ..> : include UC032a
UC033 --> SistemaInfotainment

UC030b ..> : extend UC005
UC031 ..> : extend UC016
UC031a ..> : extend UC031
UC032b ..> : extend UC032
UC033 ..> : extend UC032
UC033a ..> : extend UC033
```

#### Use case UC-030: Ricerca indirizzi e punti di interesse
**Riassunto:** Consente di cercare indirizzi, toponimi e punti di interesse sul territorio e di visualizzarne i risultati sulla mappa.

**Descrizione:**
1. L'utente inserisce nella barra di ricerca un indirizzo, un toponimo o il nome di un punto di interesse.
2. Il sistema interroga il Servizio di Geocoding e ordina i risultati per prossimità alla posizione corrente o all'area visualizzata.
3. Il sistema mostra i risultati in elenco e li evidenzia sulla mappa.
4. L'utente seleziona un risultato e ne consulta la scheda di dettaglio.

**Eccezioni:**
1. Nessun risultato corrispondente: il sistema segnala l'assenza di corrispondenze e propone ricerche alternative.
2. Servizio di Geocoding non raggiungibile: il sistema limita la ricerca ai dati memorizzati in locale, segnalandone la parzialità.

**Estensioni:**
1. Avvio della navigazione verso il risultato selezionato (UC-016).
2. Salvataggio della località tra i luoghi preferiti (UC-005).

#### Use case UC-031: Anteprima e simulazione del percorso
**Riassunto:** Permette di visualizzare in anteprima l'itinerario calcolato e, facoltativamente, di simularne la percorrenza prima della partenza.

**Descrizione:**
1. Dopo il calcolo dell'itinerario (UC-016) l'utente seleziona l'anteprima del percorso.
2. Il sistema mostra il tracciato completo sulla mappa, l'elenco delle manovre, i tempi stimati e le criticità presenti lungo il tragitto.
3. L'utente scorre le tappe dell'itinerario e confronta le alternative proposte.

**Eccezioni:**
1. Itinerario non più valido a causa di una chiusura sopravvenuta: il sistema ricalcola il percorso e aggiorna l'anteprima avvisando l'utente.

**Estensioni:**
1. Avvio della simulazione animata del viaggio, con riproduzione accelerata delle indicazioni stradali.

#### Use case UC-032: Navigazione turn-by-turn
**Riassunto:** Fornisce indicazioni vocali e visive passo-passo durante la guida, aggiornandole in tempo reale in base alla posizione dell'utente.

**Descrizione:**
1. L'utente avvia la navigazione sull'itinerario selezionato (include UC-016).
2. Il sistema traccia la posizione in tempo reale (include UC-009) e presenta la manovra successiva in forma vocale e visiva.
3. Il sistema aggiorna progressivamente le istruzioni, i tempi di arrivo e gli avvisi sulle criticità lungo il percorso.
4. Al raggiungimento della destinazione il sistema conclude la sessione di navigazione.

**Eccezioni:**
1. Perdita del segnale GPS: il sistema sospende le indicazioni, avvisa l'utente e riprende alla riacquisizione della posizione.
2. Assenza di connettività: la navigazione prosegue sui dati memorizzati in locale (UC-023), senza aggiornamenti sul traffico.

**Estensioni:**
1. Ricalcolo automatico dell'itinerario in caso di deviazione dal percorso o di nuova criticità convalidata.
2. Proiezione dell'interfaccia sul sistema di infotainment del veicolo (UC-033).

#### Use case UC-033: Integrazione con sistemi di infotainment veicolari
**Riassunto:** Proietta l'interfaccia di navigazione su Android Auto e Apple CarPlay, rendendo accessibili le funzioni essenziali in modalità hands-free.

**Descrizione:**
1. L'utente collega il dispositivo mobile al sistema di infotainment del veicolo.
2. Il sistema riconosce la piattaforma e adatta l'interfaccia di navigazione al display del veicolo.
3. L'utente consulta mappa e indicazioni e utilizza le funzioni essenziali (navigazione, invio di una segnalazione, silenziamento della guida vocale) tramite i comandi del veicolo.

**Eccezioni:**
1. Piattaforma di infotainment non supportata o connessione interrotta: la navigazione prosegue sul dispositivo mobile e il sistema avvisa l'utente.
2. Funzionalità non ammesse durante la guida: il sistema le disabilita e le rende disponibili solo a veicolo fermo.

**Estensioni:**
1. Interazione tramite comandi vocali per avviare la navigazione o inviare una segnalazione senza distogliere l'attenzione dalla guida.

### Utente Autenticato - Contributi alla community
- UC-011: Cronologia delle segnalazioni - RF-011
- UC-014: Invio segnalazioni in tempo reale - RF-014
- UC-015: Valutazione affidabilità e reputation scoring - RF-015
- UC-025: Badge e gamification della reputazione - RF-025

```mermaid
usecase-beta
direction LR

actor UtenteAutenticato("Utente Autenticato")

systemBoundary NoJam["NoJam"]
  UC011("UC-011: Cronologia delle segnalazioni")
  UC014("UC-014: Invio segnalazioni in tempo reale")
  UC015("UC-015: Valutazione affidabilità e reputation scoring")
  UC025("UC-025: Badge e gamification della reputazione")
  UC009("UC-009: Gestione della geolocalizzazione")

  UC014a("Scelta della tipologia di evento")
  UC014b("Allegare una foto o un commento")
  UC015a("Incrocio con i dati ufficiali della PA")
  UC015b("Promozione a segnalazione convalidata")
  UC025a("Condivisione dei badge sui canali social")
end

UtenteAutenticato --> UC011
UtenteAutenticato --> UC014
UtenteAutenticato --> UC025

UC014 ..> : include UC009
UC014 ..> : include UC014a
UC014 ..> : include UC015
UC015 ..> : include UC015a

UC014b ..> : extend UC014
UC015b ..> : extend UC015
UC025 ..> : extend UC015
UC025a ..> : extend UC025
```

#### Use case UC-011: Cronologia delle segnalazioni
**Riassunto:** Permette all'utente di consultare lo storico delle proprie segnalazioni, il relativo stato di validazione e il punteggio di reputazione maturato.

**Descrizione:**
1. L'utente seleziona la sezione "Le mie segnalazioni".
2. Il sistema mostra l'elenco delle segnalazioni inviate, con stato di approvazione e impatto sulla community.
3. Il sistema espone il punteggio di reputazione complessivo e i badge sbloccati.

**Eccezioni:**
1. Nessuna segnalazione presente: il sistema mostra una vista vuota che invita l'utente a contribuire.

**Estensioni:**
1. Filtro dello storico per stato di validazione o per intervallo temporale.

#### Use case UC-014: Invio segnalazioni in tempo reale
**Riassunto:** Consente all'utente di inviare segnalazioni geolocalizzate su eventi dinamici della rete stradale.

**Descrizione:**
1. L'utente autenticato seleziona la tipologia di evento da segnalare (incidente, rallentamento, ostacolo, meteo avverso, passaggio spazzaneve).
2. Il sistema rileva automaticamente la posizione corrente (include UC-009) e consente all'utente di affinarla manualmente.
3. L'utente conferma l'invio.
4. Il sistema trasmette la segnalazione all'algoritmo di valutazione (include UC-015) e la rende visibile in stato provvisorio.

**Eccezioni:**
1. Assenza di connettività: la segnalazione viene accodata in locale e trasmessa al ripristino della connessione.
2. Segnalazione duplicata nella stessa area spazio-temporale: il sistema propone di confermare quella esistente anziché crearne una nuova.

**Estensioni:**
1. Aggiunta facoltativa di un commento testuale o di una fotografia dell'evento.

#### Use case UC-015: Valutazione affidabilità e reputation scoring
**Riassunto:** Incrocia le segnalazioni degli utenti con i dati ufficiali della PA e con le segnalazioni concorrenti per calcolare il peso dell'evento e aggiornare la reputazione del segnalatore.

**Descrizione:**
1. Il sistema riceve una nuova segnalazione inviata da un utente (UC-014) o rivaluta una segnalazione già attiva a seguito della moderazione automatica (UC-022).
2. Il sistema la incrocia con i dati ufficiali della PA e con le altre segnalazioni presenti nella stessa area spazio-temporale.
3. Il sistema assegna un indice di affidabilità all'evento e aggiorna di conseguenza il punteggio di reputazione dell'utente segnalatore.

**Eccezioni:**
1. Segnalazione valutata come falsa o impropria: l'evento viene rimosso dalla mappa e al punteggio dell'utente viene applicata una penalità.
2. Dati ufficiali non disponibili per l'area: il sistema calcola l'indice sulle sole segnalazioni concorrenti, con un livello di confidenza ridotto.

**Estensioni:**
1. Promozione automatica a "segnalazione convalidata", visibile a tutti gli utenti e utilizzabile dal motore di routing.
2. Assegnazione di punti e badge al superamento delle soglie di reputazione (UC-025).

#### Use case UC-025: Badge e gamification della reputazione
**Riassunto:** Attribuisce punti, livelli e badge di affidabilità agli utenti in base all'accuratezza delle segnalazioni confermate.

**Descrizione:**
1. Il sistema rileva l'aggiornamento del punteggio di reputazione prodotto da UC-015.
2. Al superamento delle soglie previste, il sistema assegna automaticamente punti esperienza e nuovi badge di contributo.
3. L'utente riceve una notifica e visualizza il nuovo livello sul proprio profilo.

**Eccezioni:**
1. Perdita di reputazione dovuta a segnalazioni errate: il sistema retrocede il livello e revoca i badge non più corrispondenti al punteggio.

**Estensioni:**
1. Condivisione dei badge ottenuti sui canali social dell'utente.

### Utente Autenticato - Notifiche avanzate
- UC-040: Gestione avanzata delle notifiche - RF-040
- UC-041: Notifiche contestuali multicanale - RF-041

```mermaid
usecase-beta
direction LR

actor UtenteAutenticato("Utente Autenticato")
actor ServizioEmail("Servizio Email")

systemBoundary NoJam["NoJam"]
  UC040("UC-040: Gestione avanzata delle notifiche")
  UC041("UC-041: Notifiche contestuali multicanale")
  UC010("UC-010: Gestione del canale di notifiche")
  UC021("UC-021: Notifiche e allerte preventive di mobilità")
  UC027("UC-027: Gestione eventi straordinari ed emergenze")

  UC040a("Impostazione del raggio di prossimità")
  UC040b("Definizione delle fasce orarie di silenzio")
  UC040c("Sospensione temporanea delle notifiche")
  UC041a("Selezione del canale in base alla priorità")
  UC041b("Riepilogo periodico via email degli avvisi non letti")
end

UtenteAutenticato --> UC010
UtenteAutenticato --> UC040

UC040 ..> : include UC040a
UC040 ..> : include UC040b
UC041 ..> : include UC040
UC041 ..> : include UC041a
UC021 ..> : include UC041
UC027 ..> : include UC041

UC041 --> ServizioEmail

UC040 ..> : extend UC010
UC040c ..> : extend UC040
UC041b ..> : extend UC041
```

#### Use case UC-040: Gestione avanzata delle notifiche
**Riassunto:** Consente all'utente di configurare in dettaglio tipologia di evento, canale, raggio di prossimità e fasce orarie di silenzio delle notifiche ricevute.

**Descrizione:**
1. L'utente accede alle impostazioni avanzate del centro notifiche (UC-010).
2. L'utente associa a ciascuna tipologia di evento (cantieri, emergenze, piano neve, esiti delle proprie segnalazioni) i canali desiderati.
3. L'utente imposta il raggio di prossimità entro cui ricevere gli avvisi e le fasce orarie di silenzio.
4. Il sistema salva la configurazione e la applica agli invii successivi.

**Eccezioni:**
1. Configurazione che escluderebbe anche le allerte di emergenza: il sistema avvisa che le notifiche critiche vengono comunque recapitate.
2. Errore di salvataggio lato server: il sistema mantiene la configurazione precedente e invita a ritentare.

**Estensioni:**
1. Sospensione temporanea di tutte le notifiche non critiche per un intervallo scelto dall'utente.

#### Use case UC-041: Notifiche contestuali multicanale
**Riassunto:** Recapita le notifiche su push o email in base alla natura e alla priorità dell'evento e alle preferenze dell'utente.

**Descrizione:**
1. Il sistema genera una notifica a seguito di un evento rilevante (UC-021, UC-027) o dell'esito di una segnalazione.
2. Il sistema determina il canale di recapito incrociando la priorità dell'evento con le preferenze configurate in UC-040.
3. Il sistema invia il messaggio tramite notifica push o Servizio Email e ne registra l'esito di consegna.

**Eccezioni:**
1. Dispositivo privo di connessione dati: la notifica push viene recapitata alla riconnessione e l'avviso resta disponibile nel centro notifiche in-app.
2. Canale non disponibile o recapito fallito: il sistema ritenta l'invio e mantiene l'avviso nel centro notifiche in-app.

**Estensioni:**
1. Riepilogo periodico via email degli avvisi non letti.

### Operatore PA
- UC-013: Ingestion e sincronizzazione dati ufficiali PA - RF-013
- UC-018: Gestione del piano neve - RF-018
- UC-019: Pannello di gestione e Smart Governance - RF-019
- UC-020: Analisi aggregate e reporting del traffico - RF-020
- UC-027: Gestione eventi straordinari ed emergenze - RF-027

```mermaid
usecase-beta
direction LR

actor OperatorePA("Operatore PA")
actor EnteTerritoriale("Ente Territoriale")
actor UtenteAutenticato("Utente Autenticato")

systemBoundary NoJam["NoJam"]
  UC013("UC-013: Ingestion e sincronizzazione dati ufficiali PA")
  UC018("UC-018: Gestione del piano neve")
  UC014("UC-014: Invio segnalazioni in tempo reale")
  UC019("UC-019: Pannello di gestione e Smart Governance")
  UC020("UC-020: Analisi aggregate e reporting del traffico")
  UC027("UC-027: Gestione eventi straordinari ed emergenze")
  UC021("UC-021: Notifiche e allerte preventive di mobilità")

  UC013a("Parsing di cantieri, chiusure e ordinanze")
  UC013b("Parsing dei piani di sgombero neve")
  UC018a("Aggiornamento dello stato di avanzamento delle tratte")
  UC019a("Pubblicazione di ordinanze e cantieri")
  UC020a("Esportazione dei report in PDF o CSV")
  UC027a("Archiviazione dell'evento di emergenza")
end

EnteTerritoriale --> UC013
OperatorePA --> UC018
OperatorePA --> UC019
OperatorePA --> UC020
OperatorePA --> UC027
UtenteAutenticato --> UC014
UtenteAutenticato --> UC021

UC013 ..> : include UC013a
UC013 ..> : include UC013b
UC018 ..> : include UC013b
UC018 ..> : include UC018a
UC014 ..> : extend UC018a
UC019 ..> : include UC019a
UC027 ..> : include UC021

UC020a ..> : extend UC020
UC027 ..> : extend UC019
UC027a ..> : extend UC027
```

#### Use case UC-013: Ingestion e sincronizzazione dati ufficiali PA
**Riassunto:** Importa, struttura e sincronizza automaticamente i dati statici e programmati pubblicati dagli enti territoriali.

**Descrizione:**
1. Il sistema si collega periodicamente alle sorgenti dati ufficiali dell'Ente Territoriale.
2. Il sistema effettua il parsing di cantieri stradali, chiusure temporanee, ordinanze di viabilità e piani di sgombero neve.
3. Il sistema normalizza i dati e aggiorna il database interno, rendendoli disponibili alla mappa e al motore di routing.

**Eccezioni:**
1. API o feed dell'ente irraggiungibili: il sistema registra l'anomalia e conserva gli ultimi dati noti, contrassegnandoli come non aggiornati.
2. Formato del dato non conforme: il record viene scartato e messo in coda di revisione manuale.

**Estensioni:**
1. Notifica all'Operatore PA in caso di discrepanze o scarti rilevanti nei dati importati.

#### Use case UC-018: Gestione del piano neve
**Riassunto:** Permette la pianificazione dello sgombero neve e la visualizzazione dei percorsi programmati e dello stato di avanzamento delle operazioni.

**Descrizione:**
1. L'Operatore PA carica o aggiorna la pianificazione delle tratte di sgombero e la relativa fascia oraria di intervento.
2. Il sistema mostra sulla mappa i percorsi programmati e lo stato dichiarato di ciascuna tratta (da effettuare, in corso, completata).
3. L'Operatore PA aggiorna lo stato di avanzamento delle tratte durante le operazioni.
4. Il sistema aggiorna la mappa pubblica e il motore di routing con lo stato più recente.

**Eccezioni:**
1. Stato di una tratta non aggiornato oltre la fascia oraria prevista: il sistema lo presenta come "non confermato", indicando l'orario dell'ultimo aggiornamento.
2. Piano neve non pubblicato dall'ente per la stagione in corso: il sistema mostra la sola mappa delle zone di intervento storiche, segnalandone la natura indicativa.

**Estensioni:**
1. Aggiornamento dello stato di una tratta a partire dalle segnalazioni di passaggio inviate dagli utenti e convalidate dal sistema (UC-014, UC-015).
2. Invio di notifiche dedicate agli utenti delle zone interessate dagli interventi programmati (UC-021).

#### Use case UC-019: Pannello di gestione e Smart Governance
**Riassunto:** Mette a disposizione degli operatori accreditati una dashboard amministrativa per pubblicare ordinanze, gestire cantieri e supervisionare la viabilità.

**Descrizione:**
1. L'operatore accreditato accede alla dashboard di Smart Governance.
2. L'operatore pubblica nuove ordinanze, imposta chiusure stradali o aggiorna lo stato dei cantieri in corso.
3. Il sistema propaga le modifiche alla mappa pubblica e al motore di routing.

**Eccezioni:**
1. Sessione amministrativa scaduta: il sistema richiede una nuova autenticazione e conserva la bozza delle modifiche non salvate.

**Estensioni:**
1. Attivazione della modalità "Emergenza Viabilità" per eventi straordinari (UC-027).

#### Use case UC-020: Analisi aggregate e reporting del traffico
**Riassunto:** Elabora dati aggregati e anonimizzati su flussi stradali e criticità ricorrenti, a supporto della pianificazione urbana.

**Descrizione:**
1. L'Operatore PA seleziona un intervallo temporale e un'area geografica di interesse.
2. Il sistema genera report e grafici su flussi di traffico, tempi medi di percorrenza e colli di bottiglia ricorrenti.
3. L'operatore consulta i risultati per pianificare interventi infrastrutturali.

**Eccezioni:**
1. Campione dati insufficiente per il periodo selezionato: il sistema segnala la scarsa significatività statistica del risultato.

**Estensioni:**
1. Esportazione dei report in formato PDF o CSV.

#### Use case UC-027: Gestione eventi straordinari ed emergenze
**Riassunto:** Permette alla PA di attivare la modalità "Emergenza Viabilità" per diffondere allerte ad alta priorità sul territorio.

**Descrizione:**
1. L'Operatore PA rileva una situazione straordinaria (alluvione, frana, nevicata eccezionale).
2. L'operatore delimita la zona interessata e attiva la modalità "Emergenza Viabilità".
3. Il sistema invia notifiche prioritarie agli utenti presenti nell'area (include UC-021) e aggiorna il motore di routing per evitare la zona interdetta.

**Eccezioni:**
1. Mancata conferma da parte dell'amministratore responsabile: l'emergenza resta in stato di bozza e non viene propagata.

**Estensioni:**
1. Disattivazione e archiviazione dell'evento al ripristino delle condizioni di sicurezza.

### Amministratore PA - Ruoli, audit e importazione dati
- UC-034: Gestione ruoli e permessi degli operatori - RF-034
- UC-035: Log di audit operativi - RF-035
- UC-036: Importazione massiva dati - RF-036

```mermaid
usecase-beta
direction LR

actor AmministratorePA("Amministratore PA")
actor OperatorePA("Operatore PA")
actor EnteTerritoriale("Ente Territoriale")

systemBoundary NoJam["NoJam"]
  UC034("UC-034: Gestione ruoli e permessi degli operatori")
  UC035("UC-035: Log di audit operativi")
  UC036("UC-036: Importazione massiva dati")
  UC006("UC-006: Gestione dei permessi")
  UC019("UC-019: Pannello di gestione e Smart Governance")

  UC034a("Definizione di ruoli personalizzati")
  UC034b("Revoca immediata delle abilitazioni")
  UC035a("Filtro del registro per operatore e periodo")
  UC035b("Esportazione del registro di audit")
  UC036a("Validazione e anteprima dei record importati")
  UC036b("Annullamento dell'importazione")
end

AmministratorePA --|> OperatorePA

AmministratorePA --> UC034
AmministratorePA --> UC035
OperatorePA --> UC036
EnteTerritoriale --> UC036

UC034 ..> : include UC034a
UC034 ..> : include UC006
UC035 ..> : include UC035a
UC036 ..> : include UC036a
UC036 ..> : include UC035
UC034 ..> : include UC035
UC019 ..> : include UC035

UC034b ..> : extend UC034
UC035b ..> : extend UC035
UC036b ..> : extend UC036
UC036 ..> : extend UC019
```

#### Use case UC-034: Gestione ruoli e permessi degli operatori
**Riassunto:** Consente all'Amministratore PA di definire ruoli personalizzati e di assegnare agli operatori permessi granulari sulle funzioni della dashboard.

**Descrizione:**
1. L'Amministratore PA accede alla sezione di amministrazione degli accessi.
2. L'amministratore crea o modifica un ruolo, selezionando i permessi granulari da concedere (lettura, inserimento, modifica, validazione, pubblicazione).
3. L'amministratore assegna il ruolo agli operatori accreditati dell'ente.
4. Il sistema applica le abilitazioni aggiornate al controllo dei permessi (include UC-006) e registra l'operazione nel log di audit (UC-035).

**Eccezioni:**
1. Rimozione dell'ultimo ruolo con privilegi amministrativi: il sistema blocca l'operazione per evitare la perdita del controllo sull'ente.
2. Operatore con attività in corso al momento della modifica: le nuove abilitazioni si applicano dalla sessione successiva e il sistema ne dà evidenza.

**Estensioni:**
1. Revoca immediata di tutte le abilitazioni di un operatore con chiusura forzata delle sue sessioni attive.

#### Use case UC-035: Log di audit operativi
**Riassunto:** Registra in forma non modificabile le azioni eseguite dagli operatori PA, garantendo tracciabilità e responsabilità degli interventi sulla viabilità.

**Descrizione:**
1. Il sistema intercetta ogni azione rilevante eseguita da un operatore (inserimento, modifica, validazione, pubblicazione, importazione, cambio di ruolo).
2. Il sistema registra autore, ruolo, tipologia di azione, oggetto interessato, data e ora, in un archivio non modificabile.
3. L'Amministratore PA consulta il registro filtrandolo per operatore, tipologia di azione o intervallo temporale.

**Eccezioni:**
1. Tentativo di modifica o cancellazione di una voce del registro: il sistema rifiuta l'operazione e ne traccia il tentativo.
2. Archivio di audit non scrivibile: il sistema blocca l'azione operativa in corso per non produrre interventi non tracciati.

**Estensioni:**
1. Esportazione del registro di audit in formato CSV o PDF per verifiche e adempimenti ispettivi.

#### Use case UC-036: Importazione massiva dati
**Riassunto:** Permette agli operatori PA di caricare in blocco cantieri, ordinanze, chiusure e piani neve tramite file in formato standard.

**Descrizione:**
1. L'Operatore PA seleziona la funzione di importazione massiva dalla dashboard e carica un file CSV, GeoJSON o Shapefile.
2. Il sistema analizza il file, valida i record rispetto al modello dati e mostra un'anteprima con le righe conformi e quelle scartate.
3. L'operatore conferma l'importazione.
4. Il sistema registra i dati, li propaga alla mappa e al motore di routing e traccia l'operazione nel log di audit (include UC-035).

**Eccezioni:**
1. Formato del file non supportato o struttura non conforme: il sistema interrompe l'elaborazione e indica gli errori rilevati.
2. Record in conflitto con dati già presenti: il sistema li segnala e chiede all'operatore se sovrascriverli o ignorarli.
3. Interruzione durante l'elaborazione: nessun record viene reso effettivo e il sistema riporta l'archivio allo stato precedente.

**Estensioni:**
1. Annullamento di un'importazione completata entro un intervallo di tempo definito, con ripristino dello stato precedente.

### Moderazione automatica
- UC-022: Moderazione automatica dei contenuti - RF-022

```mermaid
usecase-beta
direction LR

actor UtenteAutenticato("Utente Autenticato")

systemBoundary NoJam["NoJam"]
  UC022("UC-022: Moderazione automatica dei contenuti")
  UC014("UC-014: Invio segnalazioni in tempo reale")
  UC015("UC-015: Valutazione affidabilità e reputation scoring")

  UC022a("Conferma di una segnalazione già attiva")
  UC022b("Decadimento della segnalazione non confermata")
  UC022c("Rimozione della segnalazione smentita")
  UC022d("Limitazione dei contributi di un utente poco affidabile")
end

UtenteAutenticato --> UC014

UC014 ..> : include UC022
UC022 ..> : include UC015
UC022 ..> : include UC022b

UC022a ..> : extend UC022
UC022c ..> : extend UC022
UC022d ..> : extend UC015
```

#### Use case UC-022: Moderazione automatica dei contenuti
**Riassunto:** Determina automaticamente la validità delle segnalazioni della community sulla base delle conferme concordanti, dei dati ufficiali e del tempo trascorso, senza intervento di moderatori umani.

**Descrizione:**
1. Il sistema riceve una nuova segnalazione (UC-014) oppure rivaluta periodicamente quelle già attive.
2. Il sistema conta le segnalazioni concordanti presenti nella stessa area spazio-temporale e le confronta con i dati ufficiali della PA (UC-015): maggiore è il numero di conferme, maggiore è l'affidabilità attribuita all'avviso.
3. Al superamento della soglia di affidabilità, il sistema promuove la segnalazione a convalidata e la rende utilizzabile dalla mappa e dal motore di routing.
4. In assenza di conferme entro il tempo di validità previsto per la tipologia di evento, il sistema fa decadere la segnalazione e la rimuove dalla mappa.

**Eccezioni:**
1. Segnalazione smentita dai dati ufficiali o da segnalazioni successive concordanti: il sistema la rimuove immediatamente e applica una penalità alla reputazione del segnalatore (UC-015).
2. Segnalazione isolata proveniente da un utente ad alta reputazione: il sistema la mantiene visibile con un livello di affidabilità ridotto ed esplicitato all'utente.

**Estensioni:**
1. Conferma esplicita di una segnalazione già attiva da parte di un altro utente, che ne incrementa l'affidabilità e ne prolunga la validità.
2. Intervento manuale di un Operatore PA sulla singola segnalazione dalla dashboard, in quanto fonte autorevole (UC-019).
3. Limitazione automatica dei contributi degli utenti la cui reputazione scende sotto la soglia minima prevista.

### Sistemi esterni
- UC-026: Esportazione ed API dati aperti (Open Data) - RF-026

```mermaid
usecase-beta
direction LR

actor SistemaTerzo("Sistema Terzo / Portale Istituzionale")
actor OperatorePA("Operatore PA")

systemBoundary NoJam["NoJam"]
  UC026("UC-026: Esportazione ed API dati aperti")

  UC026a("Autenticazione della richiesta")
  UC026b("Serializzazione in formato standard")
  UC026c("Download manuale dei dataset dal portale")
end

SistemaTerzo --> UC026
OperatorePA --> UC026c

UC026 ..> : include UC026a
UC026 ..> : include UC026b

UC026c ..> : extend UC026
```

#### Use case UC-026: Esportazione ed API dati aperti (Open Data)
**Riassunto:** Espone API pubbliche in formati standard per la ricondivisione in Open Data delle informazioni di viabilità elaborate dalla piattaforma.

**Descrizione:**
1. Un applicativo di terze parti invia una richiesta HTTPS alle API pubbliche del sistema.
2. Il sistema autentica la richiesta e ne verifica i permessi di accesso al dataset.
3. Il sistema serializza ed esporta il dataset richiesto (cantieri attivi, ordinanze, dati aggregati sul traffico) in formato standard JSON, XML o GTFS.

**Eccezioni:**
1. Raggiungimento del limite di chiamate consentite: il sistema risponde con un errore 429 Too Many Requests indicando il tempo di attesa.
2. Chiave di accesso non valida o revocata: il sistema risponde con un errore 401 Unauthorized.

**Estensioni:**
1. Download manuale dei dataset Open Data direttamente dal portale web istituzionale.

### Utente - Internazionalizzazione e identità federata
- UC-042: Cambio lingua dell'interfaccia - RF-042
- UC-049: Integrazione con provider di identità standard - RF-049

```mermaid
usecase-beta
direction LR

actor Utente("Utente")
actor UtenteAnonimo("Utente Anonimo")
actor UtenteAutenticato("Utente Autenticato")
actor ProviderIdentita("Provider di Identità")

systemBoundary NoJam["NoJam"]
  UC042("UC-042: Cambio lingua dell'interfaccia")
  UC049("UC-049: Integrazione con provider di identità standard")
  UC002("UC-002: Login utente")
  UC004("UC-004: Gestione preferenze")

  UC042a("Selezione della lingua (IT, EN, DE)")
  UC042b("Aggiornamento immediato dell'interfaccia senza riavvio")
  UC042c("Memorizzazione preferenza nel profilo")
  UC049a("Negoziazione protocollo standard (OAuth2 / OIDC)")
  UC049b("Autenticazione federata SPID o CIE")
  UC049c("Associazione identità digitale a profilo esistente")
end

UtenteAnonimo --|> Utente
UtenteAutenticato --|> Utente

Utente --> UC042
UtenteAnonimo --> UC049
UtenteAutenticato --> UC042c
ProviderIdentita --> UC049

UC042 ..> : include UC042a
UC042 ..> : include UC042b
UC049 ..> : include UC049a

UC042c ..> : extend UC042
UC042c ..> : extend UC004
UC049 ..> : extend UC002
UC049b ..> : extend UC049
UC049c ..> : extend UC049
```

#### Use case UC-042: Cambio lingua dell'interfaccia
**Riassunto:** Permette all'utente di selezionare e cambiare istantaneamente la lingua dell'interfaccia tra Italiano, Inglese e Tedesco senza richiedere il riavvio dell'applicazione.

**Descrizione:**
1. L'utente accede alla selezione della lingua dalle impostazioni dell'applicazione o dalla schermata principale.
2. L'utente sceglie una delle lingue supportate (Italiano, Inglese, Tedesco).
3. Il sistema applica istantaneamente il bundle di localizzazione selezionato a tutti gli elementi visivi dell'interfaccia grafica e ai messaggi di sistema.
4. Il sistema memorizza la preferenza linguistica a livello locale sul dispositivo e, se l'utente è autenticato, la sincronizza con il suo profilo (include UC-004).

**Eccezioni:**
1. Risorsa di localizzazione non disponibile o stringa mancante: il sistema applica automaticamente il fallback sulla lingua di default (Italiano) mantenendo inalterata la navigazione.

**Estensioni:**
1. Rilevamento e impostazione automatica della lingua in base alle impostazioni internazionali di sistema del dispositivo al primo avvio.

#### Use case UC-049: Integrazione con provider di identità standard
**Riassunto:** Consente l'autenticazione sicura e federata degli utenti tramite provider di identità basati su protocolli standard aperti (OAuth 2.0, OpenID Connect) e identità digitali nazionali (SPID, CIE).

**Descrizione:**
1. L'utente seleziona l'accesso tramite un provider di identità esterno supportato (Google OAuth, SPID, CIE).
2. Il sistema reindirizza l'utente alla schermata di autenticazione del provider selezionato avvalendosi di flussi standard OAuth 2.0 / OpenID Connect.
3. L'utente completa la procedura di autenticazione presso il provider fornendo il consenso al trasferimento delle informazioni di base del profilo.
4. Il provider restituisce al sistema un token crittografico firmato attestante l'identità dell'utente.
5. Il sistema valida la firma crittografica del token, associa o crea l'account utente locale ed emette la sessione applicativa (estende UC-002).

**Eccezioni:**
1. Rifiuto dell'autenticazione o mancato consenso da parte dell'utente presso il provider esterno: il sistema annulla la procedura e reindirizza alla schermata di accesso locale senza creare sessioni.
2. Token non valido o scaduto: il sistema respinge l'accesso, registra l'errore e richiede un nuovo tentativo di login.
3. Provider di identità irraggiungibile o in errore: il sistema segnala l'indisponibilità temporanea e suggerisce una modalità di accesso alternativa.

**Estensioni:**
1. Associazione di un'identità federata (SPID o CIE) a un profilo locale preesistente verificato via email.

### Operatore PA - Sicurezza e controllo accessi
- UC-043: Autenticazione a più fattori per operatori PA - RF-043
- UC-045: Controllo degli accessi alle funzionalità amministrative - RF-045

```mermaid
usecase-beta
direction LR

actor OperatorePA("Operatore PA")
actor AmministratorePA("Amministratore PA")
actor ProviderIdentita("Provider di Identità")

systemBoundary NoJam["NoJam"]
  UC043("UC-043: Autenticazione a più fattori per operatori PA")
  UC045("UC-045: Controllo degli accessi alle funzionalità amministrative")
  UC019("UC-019: Pannello di gestione e Smart Governance")
  UC034("UC-034: Gestione ruoli e permessi degli operatori")
  UC035("UC-035: Log di audit operativi")

  UC043a("Verifica del secondo fattore OTP")
  UC043b("Configurazione iniziale o reset del secondo fattore")
  UC045a("Verifica dei permessi basata sui ruoli (RBAC)")
  UC045b("Inibizione delle funzionalità non autorizzate")
end

AmministratorePA --|> OperatorePA

OperatorePA --> UC043
OperatorePA --> UC045
AmministratorePA --> UC043b
AmministratorePA --> UC034
ProviderIdentita --> UC043

UC043 ..> : include UC043a
UC019 ..> : include UC043
UC019 ..> : include UC045
UC045 ..> : include UC045a
UC045 ..> : include UC035

UC043b ..> : extend UC043
UC045b ..> : extend UC045
UC045 ..> : extend UC034
```

#### Use case UC-043: Autenticazione a più fattori per operatori PA
**Riassunto:** Richiede l'autenticazione a più fattori (MFA) agli operatori e amministratori della PA prima di consentire l'accesso alle funzionalità della dashboard di Smart Governance.

**Descrizione:**
1. L'Operatore PA inserisce le proprie credenziali primarie (email e password o credenziali federate della PA).
2. Il sistema convalida le credenziali e rileva che l'account appartiene a un profilo con privilegi amministrativi.
3. Il sistema richiede l'inserimento del codice di autenticazione a due fattori (OTP generato da app o inviato tramite canale sicuro).
4. L'operatore inserisce il codice monouso.
5. Il sistema valida il secondo fattore e concede l'accesso alla dashboard di Smart Governance (include UC-019).

**Eccezioni:**
1. Codice OTP non corretto o scaduto: il sistema nega l'accesso e richiede un nuovo inserimento, bloccando l'utenza dopo una serie di tentativi falliti consecutivi.
2. Mancata configurazione preventiva del secondo fattore: il sistema avvia obbligatoriamente il flusso di onboarding MFA prima di abilitare l'accesso operativo.

**Estensioni:**
1. Reset e riconfigurazione assistita del secondo fattore autorizzata dall'Amministratore PA in caso di smarrimento del dispositivo dell'operatore.

#### Use case UC-045: Controllo degli accessi alle funzionalità amministrative
**Riassunto:** Applica un controllo degli accessi basato sui ruoli (RBAC) alle funzionalità amministrative, limitando le operazioni di ciascun operatore PA a quelle espressamente concesse dal proprio profilo.

**Descrizione:**
1. L'Operatore PA autenticato tenta di accedere a una funzionalità o di inviare una richiesta amministrativa (es. pubblicazione cantieri, modifica piano neve, consultazione audit).
2. Il sistema verifica i ruoli e i permessi granulari attribuiti al profilo dell'operatore (definiti in UC-034).
3. Se l'operatore dispone dell'abilitazione richiesta, il sistema autorizza l'esecuzione e registra l'evento nel log di audit (include UC-035).
4. Il sistema esegue l'operazione richiesta e ne mostra l'esito sulla dashboard.

**Eccezioni:**
1. Profilo non autorizzato all'azione richiesta: il sistema inibisce l'operazione, mostra una notifica di autorizzazione insufficiente (HTTP 403 Forbidden) e registra l'accesso anomalo nel log di audit (UC-035).
2. Modifica o revoca dei permessi durante una sessione attiva: il sistema blocca istantaneamente le chiamate successive e richiede l'aggiornamento della sessione.

**Estensioni:**
1. Inibizione preventiva nell'interfaccia grafica: il sistema nasconde o disattiva i comandi per le azioni non previste dal profilo dell'operatore.

### Amministratore PA - Sicurezza dei dati e continuità operativa
- UC-044: Cifratura delle comunicazioni e dei dati sensibili - RF-044
- UC-046: Backup automatico dei dati critici - RF-046

```mermaid
usecase-beta
direction LR

actor AmministratorePA("Amministratore PA")
actor Utente("Utente")

systemBoundary NoJam["NoJam"]
  UC044("UC-044: Cifratura delle comunicazioni e dei dati sensibili")
  UC046("UC-046: Backup automatico dei dati critici")
  UC035("UC-035: Log di audit operativi")

  UC044a("Negoziazione canale cifrato TLS 1.2 o superiore")
  UC044b("Cifratura a riposo dei dati sensibili (AES-256)")
  UC044c("Rotazione programmata delle chiavi crittografiche")
  UC046a("Generazione snapshot coerente con checksum")
  UC046b("Replica cifrata in zona geografica remota")
  UC046c("Ripristino dati critici (restore) da backup")
end

Utente --> UC044
AmministratorePA --> UC044c
AmministratorePA --> UC046
AmministratorePA --> UC046c

UC044 ..> : include UC044a
UC044 ..> : include UC044b
UC046 ..> : include UC046a
UC046 ..> : include UC046b
UC046 ..> : include UC035

UC044c ..> : extend UC044
UC046c ..> : extend UC046
```

#### Use case UC-044: Cifratura delle comunicazioni e dei dati sensibili
**Riassunto:** Protegge i flussi di rete e i dati archiviati crittografando tutte le comunicazioni con protocollo TLS 1.2 o superiore e memorizzando i dati sensibili cifrati a riposo.

**Descrizione:**
1. Il client (app mobile o interfaccia web) o un servizio terzo stabilisce una sessione di rete con la piattaforma NoJam.
2. Il sistema forza la negoziazione della connessione attraverso standard crittografici TLS 1.2 o superiore, rifiutando canali in chiaro.
3. Durante il salvataggio dei dati nel database o nel file store, il sistema cifra a riposo tutti i dati sensibili (password tramite hashing sicuro e salato, token di sessione, coordinate GPS grezze, log di audit) con algoritmi robusti (es. AES-256).
4. I dati vengono decifrati in memoria solo al momento della legittima consultazione da parte di processi autorizzati.

**Eccezioni:**
1. Connessione non conforme o tentativo di downgrade di protocollo TLS: il sistema rifiuta la negoziazione e termina la connessione.
2. Errore di decifratura o chiave di crittografia non corrispondente: il sistema blocca l'accesso al record interessato, traccia l'anomalia nel log di audit e genera un alert per l'amministratore.

**Estensioni:**
1. Rotazione periodica o straordinaria delle chiavi di cifratura gestita in sicurezza dall'Amministratore PA.

#### Use case UC-046: Backup automatico dei dati critici
**Riassunto:** Esegue backup automatici giornalieri dei dati essenziali di NoJam e ne replica almeno una copia in una zona geografica remota, applicando le policy di retention e garantendo il ripristino.

**Descrizione:**
1. Con cadenza giornaliera (schedulazione periodica automatica), il sistema avvia la procedura di salvataggio dei dati critici (configurazioni, anagrafiche utente, eventi di viabilità, log di audit).
2. Il sistema genera un'immagine consistente dei database e degli allegati, calcolando un codice di controllo (checksum crittografico) per garantirne l'integrità.
3. Il sistema trasferisce e replica in modo protetto una copia del backup verso una seconda zona geografica indipendente da quella di produzione.
4. Il sistema applica la retention prefissata, archiviando o eliminando le versioni scadute, e notifica l'esito nel registro delle operazioni consultabile dall'Amministratore PA (include UC-035).

**Eccezioni:**
1. Errore o interruzione durante il trasferimento verso la zona geografica remota: il sistema ritenta l'operazione e, in caso di fallimento definitivo, invia una segnalazione urgente all'Amministratore PA.
2. Mancata corrispondenza del checksum del file di backup generato: l'archivio corrotto viene marcato come invalido e la procedura viene rieseguita.

**Estensioni:**
1. Esecuzione di una procedura di verifica o ripristino (restore) dei dati critici su richiesta dell'Amministratore PA in ambiente di test.

### Sistemi esterni ed Enti Territoriali - Interoperabilità e formati standard
- UC-047: Ingestion automatica di dati PA in formati standard - RF-047
- UC-048: Documentazione e formati delle API pubbliche - RF-048

```mermaid
usecase-beta
direction LR

actor EnteTerritoriale("Ente Territoriale")
actor OperatorePA("Operatore PA")
actor SistemaTerzo("Sistema Terzo / Portale Istituzionale")

systemBoundary NoJam["NoJam"]
  UC047("UC-047: Ingestion automatica di dati PA in formati standard")
  UC048("UC-048: Documentazione e formati delle API pubbliche")
  UC013("UC-013: Ingestion e sincronizzazione dati ufficiali PA")
  UC026("UC-026: Esportazione ed API dati aperti")

  UC047a("Parsing e validazione CSV, GeoJSON e Shapefile")
  UC047b("Notifica discrepanze e scarti all'operatore")
  UC048a("Consultazione documentazione interattiva OpenAPI 3.0")
  UC048b("Erogazione payload standard JSON e GeoJSON")
  UC048c("Download specifica OpenAPI (JSON/YAML)")
end

EnteTerritoriale --> UC047
OperatorePA --> UC047b
SistemaTerzo --> UC048

UC047 ..> : include UC047a
UC047 ..> : include UC013
UC048 ..> : include UC048a
UC048 ..> : include UC048b
UC048 ..> : include UC026

UC047b ..> : extend UC047
UC048c ..> : extend UC048
```

#### Use case UC-047: Ingestion automatica di dati PA in formati standard
**Riassunto:** Acquisisce e sincronizza automaticamente i dati ufficiali provenienti dagli Enti Territoriali nei formati standard CSV, GeoJSON e Shapefile per alimentare mappa e routing.

**Descrizione:**
1. Il sistema interroga periodicamente gli endpoint o i repository ufficiali dell'Ente Territoriale.
2. Il sistema riconosce e scarica i file contenenti aggiornamenti sulla viabilità nei formati standard CSV, GeoJSON o Shapefile.
3. Il sistema effettua il parsing e la validazione dei dati spaziali e temporali, normalizzando le informazioni secondo gli standard interni di NoJam (include UC-013).
4. I dati validati vengono pubblicati automaticamente sulla mappa (UC-017) e integrati nel motore di calcolo dei percorsi (UC-016).

**Eccezioni:**
1. File con formato corrotto o geometrie non conformi: il sistema scarta i record errati, mantiene inalterati i dati precedentemente validi e traccia le anomalie nel registro di sistema.
2. Servizio o repository dell'ente non raggiungibile: il sistema pianifica nuovi tentativi di sincronizzazione mantenendo le ultime informazioni caricate con successo.

**Estensioni:**
1. Notifica delle discrepanze o degli scarti rilevati durante la sincronizzazione all'Operatore PA per opportuna verifica.

#### Use case UC-048: Documentazione e formati delle API pubbliche
**Riassunto:** Rende accessibile a terze parti la documentazione ufficiale delle API pubbliche secondo lo standard OpenAPI 3.0 e fornisce le risposte nei formati interoperabili JSON e GeoJSON.

**Descrizione:**
1. Un Sistema Terzo o uno sviluppatore esterno si collega al portale developer per consultare l'interfaccia delle API di NoJam.
2. Il sistema espone la documentazione interattiva generata a partire dalla specifica conforme allo standard OpenAPI 3.0, comprensiva di descrizioni, rotte, schemi dati ed esempi.
3. Il sistema terzo invia una richiesta alle API pubbliche (estende UC-026).
4. Il sistema elabora la richiesta e restituisce i dati formattati in JSON o GeoJSON a seconda della natura del dato richiesto.

**Eccezioni:**
1. Richiesta formulata con parametri non conformi allo schema OpenAPI 3.0: il sistema risponde con errore HTTP 400 Bad Request allegando il dettaglio dei campi errati.
2. Formato di risposta richiesto non supportato: il sistema restituisce codice HTTP 406 Not Acceptable indicando i formati accettati (`application/json`, `application/geo+json`).

**Estensioni:**
1. Download diretto della specifica OpenAPI 3.0 in formato JSON o YAML per importazione rapida in ambienti di sviluppo e testing.
