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
L'interfaccia dell'applicazione (sia mobile che desktop) deve consentire il cambio di lingua istantaneo tra Italiano, Inglese e Tedesco. Tutti gli elementi dell'interfaccia grafica e i messaggi di sistema devono essere completamente tradotti e localizzati.

**Motivazione:** Il Comune gestisce il traffico non solo per i residenti, ma anche per pendolari, turisti e autotrasportatori stranieri che attraversano il territorio del comune di Trento e che necessitano di essere immediatamente a conoscenza di avvisi e deviazioni.

### RNF-006: Usabilità e Sicurezza d'Uso
L'interfaccia per i cittadini deve consentire l'invio di una segnalazione con un massimo di 3 tocchi/click (es. selezione tipo imprevisto + conferma posizione), utilizzabile intuitivamente entro 2 minuti dal primo avvio senza necessità di istruzioni. Per la componente di navigazione/consultazione alla guida, la visualizzazione deve essere ottimizzata a contrasto elevato con indicazioni sintetiche per ridurre al minimo le distrazioni.

**Motivazione:** Gli utenti inviano segnalazioni spesso dal veicolo (a veicolo fermo o tramite passeggero). Un processo di segnalazione lungo o complesso scoraggerebbe l'uso della funzione predittiva della community e aumenterebbe i rischi legati alla distrazione.