# Use Case Diagram

<!-- Gli Use Case e i relativi diagrammi dettagliano le interazioni tra attori e sistema a partire dai requisiti funzionali e mantengono riferimenti espliciti agli RF correlati. Ogni Use Case utilizza un identificatore stabile UC-XXX, indipendente dagli identificatori RF-*, e indica i requisiti a cui contribuisce. Non esiste una corrispondenza 1:1 tra requisiti e Use Case: uno stesso RF può essere dettagliato da più Use Case e uno Use Case può contribuire a più RF. -->

<!--
### [Attore 1]
- UC-000: [titolo use case] — RF-000
- UC-010: [titolo use case] — RF-010

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
| Moderatore | Ruolo incaricato della verifica dei contenuti inviati dalla community. |
| Provider di Identità | Sistema esterno di autenticazione (Google SSO, SPID, CIE). |
| Servizio Email | Sistema esterno per l'invio delle comunicazioni transazionali. |
| Supporto Tecnico | Team che riceve e gestisce bug report e feedback. |
| Ente Territoriale | Sorgente dati ufficiale (Comune, Provincia) per cantieri, ordinanze e piani neve. |
| Mezzo Spazzaneve | Mezzo operativo dotato di trasmettitore GPS. |
| Servizi Parcheggi e TPL | Sorgenti dati in tempo reale su sosta e trasporto pubblico. |
| Servizio di Posizione | Sensore GPS e servizio di localizzazione del dispositivo. |
| Sistema Terzo | Applicativo o portale istituzionale che consuma le API Open Data. |

---

### Utente Anonimo
- UC-001: Registrazione utente — RF-001
- UC-002: Login utente — RF-002
- UC-006: Gestione dei permessi — RF-006
- UC-017: Consultazione mappa interattiva ed eventi — RF-017

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
1. Richiesta di elevazione del ruolo a Operatore PA o Moderatore, soggetta ad approvazione.

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

---

### Utente Autenticato — Account e privacy
- UC-003: Gestione del profilo — RF-003
- UC-004: Gestione preferenze — RF-004
- UC-007: Logout utente — RF-007
- UC-008: Gestione degli utenti e privacy — RF-008
- UC-010: Gestione del canale di notifiche — RF-010
- UC-012: Gestione di feedback ed errori — RF-012
- UC-029: Chiusura e cancellazione dell'account — RF-029

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

---

### Utente Autenticato — Mappa e navigazione
- UC-005: Gestione luoghi e percorsi salvati — RF-005
- UC-009: Gestione della geolocalizzazione — RF-009
- UC-016: Calcolo e ottimizzazione percorsi — RF-016
- UC-021: Notifiche e allerte preventive di mobilità — RF-021
- UC-023: Modalità di guida offline parziale — RF-023
- UC-024: Integrazione con trasporti pubblici e parcheggi — RF-024
- UC-028: Pianificazione temporale dei viaggi — RF-028

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

---

### Utente Autenticato — Contributi alla community
- UC-011: Cronologia delle segnalazioni — RF-011
- UC-014: Invio segnalazioni in tempo reale — RF-014
- UC-015: Valutazione affidabilità e reputation scoring — RF-015
- UC-025: Badge e gamification della reputazione — RF-025

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
1. Il sistema riceve una nuova segnalazione, inviata da un utente (UC-014) o esitata da un moderatore (UC-022).
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

---

### Operatore PA
- UC-013: Ingestion e sincronizzazione dati ufficiali PA — RF-013
- UC-018: Gestione del piano neve e tracciamento mezzi — RF-018
- UC-019: Pannello di gestione e Smart Governance — RF-019
- UC-020: Analisi aggregate e reporting del traffico — RF-020
- UC-027: Gestione eventi straordinari ed emergenze — RF-027

```mermaid
usecase-beta
direction LR

actor OperatorePA("Operatore PA")
actor EnteTerritoriale("Ente Territoriale")
actor MezzoSpazzaneve("Mezzo Spazzaneve")
actor UtenteAutenticato("Utente Autenticato")

systemBoundary NoJam["NoJam"]
  UC013("UC-013: Ingestion e sincronizzazione dati ufficiali PA")
  UC018("UC-018: Gestione del piano neve e tracciamento mezzi")
  UC019("UC-019: Pannello di gestione e Smart Governance")
  UC020("UC-020: Analisi aggregate e reporting del traffico")
  UC027("UC-027: Gestione eventi straordinari ed emergenze")
  UC021("UC-021: Notifiche e allerte preventive di mobilità")

  UC013a("Parsing di cantieri, chiusure e ordinanze")
  UC013b("Parsing dei piani di sgombero neve")
  UC018a("Tracciamento GPS dei mezzi operativi")
  UC019a("Pubblicazione di ordinanze e cantieri")
  UC020a("Esportazione dei report in PDF o CSV")
  UC027a("Archiviazione dell'evento di emergenza")
end

EnteTerritoriale --> UC013
MezzoSpazzaneve --> UC018
OperatorePA --> UC018
OperatorePA --> UC019
OperatorePA --> UC020
OperatorePA --> UC027
UtenteAutenticato --> UC021

UC013 ..> : include UC013a
UC013 ..> : include UC013b
UC018 ..> : include UC013b
UC018 ..> : include UC018a
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

#### Use case UC-018: Gestione del piano neve e tracciamento mezzi
**Riassunto:** Permette la pianificazione dello sgombero neve e la visualizzazione in tempo reale delle rotte e della posizione dei mezzi spazzaneve.

**Descrizione:**
1. L'Operatore PA carica o aggiorna la pianificazione delle tratte di sgombero.
2. Il sistema acquisisce la posizione GPS trasmessa dai mezzi operativi.
3. Il sistema mostra sulla mappa la pianificazione aggiornata e lo stato di pulizia delle strade.

**Eccezioni:**
1. Trasmettitore GPS di un mezzo disconnesso: il sistema mostra la posizione come "ultimo aggiornamento noto" con il relativo orario.

**Estensioni:**
1. Invio di notifiche dedicate agli utenti delle zone interessate dai passaggi imminenti (UC-021).

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

---

### Moderatore
- UC-022: Moderazione e gestione dei contenuti — RF-022

```mermaid
usecase-beta
direction LR

actor Moderatore("Moderatore")
actor OperatorePA("Operatore PA")

systemBoundary NoJam["NoJam"]
  UC022("UC-022: Moderazione e gestione dei contenuti")
  UC015("UC-015: Valutazione affidabilità e reputation scoring")

  UC022a("Presa in carico della coda di segnalazioni")
  UC022b("Blocco dell'utente segnalatore")
end

OperatorePA --|> Moderatore

Moderatore --> UC022

UC022 ..> : include UC022a
UC022 ..> : include UC015

UC022b ..> : extend UC022
```

#### Use case UC-022: Moderazione e gestione dei contenuti
**Riassunto:** Consente a moderatori e operatori PA di confermare, correggere o rimuovere le segnalazioni errate, obsolete o improprie inviate dalla community.

**Descrizione:**
1. Il moderatore accede alla coda delle segnalazioni attive e ne prende in carico una.
2. Il moderatore valuta l'accuratezza dell'informazione e sceglie se confermarla, modificarla o rimuoverla.
3. Il sistema aggiorna la mappa e trasmette l'esito all'algoritmo di reputazione (include UC-015) per aggiornare il punteggio dell'utente segnalatore.

**Eccezioni:**
1. Segnalazione già presa in carico da un altro operatore: il sistema notifica il conflitto e non consente la doppia moderazione.

**Estensioni:**
1. Blocco temporaneo o permanente degli utenti che inviano ripetutamente segnalazioni ingannevoli.

---

### Sistemi esterni
- UC-026: Esportazione ed API dati aperti (Open Data) — RF-026

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
