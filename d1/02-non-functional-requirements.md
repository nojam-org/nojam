# Requisiti Non Funzionali

<!-- I requisiti non funzionali sono formulati in modo concreto e verificabile, identificando, quando applicabile, metriche, condizioni e valori soglia. I valori quantitativi riportati sono motivati sulla base di esigenze di stakeholder, vincoli normativi, caratteristiche del contesto d’uso, dati disponibili o assunzioni concordate. -->

<!--
### RNF-000: [Questo è un requisito non funzionale di esempio]
[Descrizione del requisito non funzionale, con metriche e soglie quando applicabili.]

**Motivazione:** [motivazione o origine del valore soglia indicato.]
-->

### RNF-001: Compatibilità
Il sistema deve essere accessibile e pienamente operativo su dispositivi mobili e desktop. Per la versione web/web-app, deve garantire piena compatibilità con le ultime versioni dei principali browser (Mozilla Firefox v115+, Google Chrome/Chromium v115+, Apple Safari v16+, Microsoft Edge v115+). Per l'applicazione mobile native/cross-platform, deve supportare i sistemi operativi Android (dalla versione 10) e iOS (dalla versione 15). Il sistema deve garantire un'esperienza utente reattiva (responsive) e coerente, assicurando la corretta visualizzazione delle mappe dinamiche e il pieno funzionamento della geolocalizzazione indipendentemente dal dispositivo utilizzato.

**Motivazione:** Trattandosi di un'app per la mobilità, la maggior parte degli utenti consulta o segnala imprevisti in mobilità da smartphone, mentre gli operatori del Comune lavorano da postazioni desktop. È fondamentale garantire accessibilità universale senza costringere gli utenti ad aggiornare hardware o sistemi obsoleti.

### RNF-002: Performance
Il sistema deve garantire tempi di risposta per il caricamento delle mappe e la visualizzazione degli eventi (deviazioni, cantieri, segnalazioni) inferiori a 1,5 secondi per il 95% delle richieste. L'invio di una nuova segnalazione (es. incidente, spazzaneve) da parte dell'utente o la pubblicazione di un avviso da parte del Comune deve essere elaborato e distribuito alla mappa in tempo reale (entro 3 secondi). Queste prestazioni devono essere mantenute con un carico continuo di almeno 500 utenti/operatori simultaneamente connessi.

**Motivazione:** In un contesto di viabilità e approccio predittivo, la tempestività dell'informazione è critica. Un ritardo nella visualizzazione di un cantiere o di un incidente rende l'informazione inutile per chi è alla guida.

### RNF-003: Scalabilità
Il sistema deve scalare elasticamente per gestire picchi improvvisi fino a 50.000 utenti simultaneamente connessi e oltre 5.000 eventi/segnalazioni concorrenti sul territorio, mantenendo i livelli di prestazione indicati in RNF-002.

**Motivazione:** La domanda su un'app di viabilità non è uniforme: in caso di eventi atmosferici eccezionali (es. nevicate improvvise, emergenze) o durante le ore di punta cittadine, il traffico sull'app può centuplicarsi in pochi minuti. Il sistema cloud sottostante deve poter assorbire il carico senza rallentamenti.

### RNF-004: Affidabilità e Disponibilità
Il sistema deve garantire una disponibilità minima del 99,9% su base annua (downtime massimo non pianificato inferiore a 8 ore e 46 minuti/anno). In caso di manutenzione programmata, questa deve avvenire esclusivamente in fasce orarie a basso traffico (es. 01:00 - 05:00) con preavviso agli operatori del Comune.

**Motivazione:** *nojam* gestisce la viabilità e servizi essenziali (es. passaggi spazzaneve o chiusure strade per emergenze). L'indisponibilità della piattaforma durante un'emergenza viaria avrebbe un impatto negativo diretto sulla sicurezza e sul traffico cittadino.

### RNF-005: Lingua e Localizzazione
Tutti gli elementi dell'interfaccia grafica e i messaggi di sistema devono essere completamente tradotti e localizzati nelle lingue supportate, garantendo coerenza terminologica e culturale.

**Motivazione:** Il Comune gestisce il traffico non solo per i residenti, ma anche per pendolari, turisti e autotrasportatori stranieri che attraversano il territorio del comune di Trento e che necessitano di essere immediatamente a conoscenza di avvisi e deviazioni.

### RNF-006: Usabilità e Sicurezza d'Uso
L'interfaccia per i cittadini deve consentire l'invio di una segnalazione con un massimo di 3 tocchi/click (es. selezione tipo imprevisto + conferma posizione), utilizzabile intuitivamente entro 2 minuti dal primo avvio senza necessità di istruzioni. Per la componente di navigazione/consultazione alla guida, la visualizzazione deve essere ottimizzata a contrasto elevato con indicazioni sintetiche per ridurre al minimo le distrazioni.

**Motivazione:** Gli utenti inviano segnalazioni spesso dal veicolo (a veicolo fermo o tramite passeggero). Un processo di segnalazione lungo o complesso scoraggerebbe l'uso della funzione predittiva della community e aumenterebbe i rischi legati alla distrazione.

### RNF-007: Sicurezza e Privacy
Il sistema deve garantire la riservatezza, l'integrità e la disponibilità dei dati personali e delle credenziali degli utenti nel rispetto del GDPR, consentendo l'esercizio dei diritti dell'interessato (accesso, retifica, cancellazione, portabilità).

**Motivazione:** NoJam tratta dati personali (account, storico spostamenti, geolocalizzazione) e dati istituzionali sensibili. Una violazione o un accesso non autorizzato alla dashboard PA comprometterebbe la fiducia degli utenti e la sicurezza della viabilità gestita dal Comune.

### RNF-008: Accessibilità
L'interfaccia web e mobile deve rispettare i requisiti di accessibilità previsti dallo standard WCAG 2.1 livello AA. In particolare, devono essere garantiti: contrasto minimo 4.5:1 per il testo normale, navigazione completa tramite tastiera/screen reader, testi alternativi per le immagini e le icone, dimensioni dei caratteri ridimensionabili fino al 200% senza perdita di funzionalità, e supporto per le principiali tecnologie assistive.

**Motivazione:** NoJam è un servizio pubblico rivolto a tutti i cittadini, inclusi utenti con disabilità visive, motorie o cognitive. Garantire l'accessibilità non è solo un obbligo normativo per le PA, ma allarga la base di utenti attivi che contribuiscono con segnalazioni in tempo reale.

### RNF-009: Disaster Recovery
In caso di incidente deve essere garantito un Recovery Point Objective (RPO) massimo di 24 ore e un Recovery Time Objective (RTO) massimo di 4 ore per i servizi core.

**Motivazione:** In situazioni di emergenza viaria (nevicate, frane, allagamenti) l'indisponibilità prolungata o la perdita di dati sarebbero inaccettabili. Un piano di disaster recovery robusto integra il requisito di alta disponibilità già espresso in RNF-004.

### RNF-010: Interoperabilità e Standard Aperti
Il sistema deve scambiare dati con enti territoriali, servizi di terze parti e sistemi esterni utilizzando formati e protocolli aperti e standard di settore riconosciuti, garantendo riusabilità, trasparenza e facilità di integrazione.

**Motivazione:** NoJam nasce per integrare fonti eterogenee (Comune, Provincia, servizi parcheggi/TPL) e per rilasciare Open Data verso terzi. L'adozione di standard aperti riduce i costi di integrazione, garantisce la riusabilità della piattaforma da parte di altri enti e sostiene gli obiettivi di Smart Governance e trasparenza.
