# Il progetto NoJam

![Problema](assets/problem.png){width=33%}
![Soluzione](assets/solution.png){width=33%}
![Vantaggi](assets/advantages.png){width=33%}

A Trento il traffico è un problema sistemico: la nostra città si sviluppa su assi viari obbligati. Quando partono i cantieri estivi o nevica, la città si ferma. Le informazioni ufficiali esistono, il Comune e la Provincia pubblicano dettagliate ordinanze in PDF e tabelle di marcia, ma rimangono isolate su siti burocratici. I grandi navigatori ignorano la programmazione locale. Il risultato è ore perse nel traffico, aumento delle emissioni e un’amministrazione che fatica a gestire la viabilità.

La nostra soluzione è NoJam, una piattaforma basata su un'architettura duale. Da un lato colleziona dalla Pubblica Amministrazione tutti i dati ufficiali e statici: tabelle dello sgombero neve, cantieri e deviazioni programmate. Dall'altro raccoglie dagli utenti le informazioni dinamiche in tempo reale, come rallentamenti ed incidenti. Il cuore del sistema è un algoritmo che incrocia queste fonti per valutare l'affidabilità delle segnalazioni, calcolare i percorsi migliori e inviare alert preventivi. 
A differenza delle grandi app globali, NoJam agisce con granularità iper-locale sui dati certi del territorio, con benefici immediati e tangibili. Per i cittadini significa viaggiare senza stress, ricevendo alert preventivi e percorsi basati su dati verificati. Per la Pubblica Amministrazione rappresenta uno strumento di Smart Governance, fornendo un canale diretto per orchestrare la mobilità e valorizzare il proprio lavoro sul territorio. 
L'architettura di NoJam nasce sul territorio di Trento, ma è nativamente scalabile a qualsiasi altra realtà comunale o provinciale.

Ci si potrebbe chiedere: perché un utente dovrebbe scegliere NoJam rispetto a Google Maps o Waze?
Le grandi piattaforme globali lavorano su macro-dati e sulla reattività aggregata. NoJam vince sulla granularità: dialoga direttamente con gli enti territoriali e conosce la mappa dei lavori o della neve prima che abbiano un impatto sulla strada.
Trento rappresenta il nostro campo di prova e il laboratorio ideale per densità e conformazione viaria. Tuttavia, l'architettura software di NoJam è strutturata in modo modulare: la logica di ingestion delle ordinanze e il motore predittivo sono nativamente pronti per essere scalati e adottati da qualsiasi altro Comune o Provincia.

### Vantaggi per il comune
- **Smart governance della mobilità:** il Comune e la Provincia dispongono di un canale diretto per pubblicare ordinanze, cantieri, deviazioni e allerte neve, orchestrando la viabilità in modo proattivo anziché reattivo.
- **Decisioni basate sui dati:** l’incrocio tra dati ufficiali e segnalazioni degli utenti fornisce al territorio analitiche <!-- TODO: is that how you say it??? -->aggregate sul traffico e sulle criticità, a supporto della pianificazione urbana e della gestione delle emergenze.
- **Valorizzazione del lavoro amministrativo:** le ordinanze e le informazioni già prodotte dagli enti diventano immediatamente fruibili per i cittadini, riducendo confusione, reclami e pressione sui canali istituzionali.

### Vantaggi per i cittadini
- **Percorsi personalizzati su dati verificati:** il motore di routing combina dati ufficiali del territorio con segnalazioni in tempo reale, proponendo itinerari più affidabili rispetto ai navigatori globali.
- **Alert preventivi prima di partire:** notifiche su cantieri programmati, sgombero neve, incidenti e rallentamenti permettono di pianificare gli spostamenti con anticipo e ridurre il tempo perso nel traffico.
- **Trasparenza e fiducia:** ogni segnalazione viene pesata dall’algoritmo tramite incrocio con fonti ufficiali, rendendo esplicito il livello di affidabilità delle informazioni.

### Limiti dell'applicazione
- **Dipendenza dai dati aperti della Pubblica Amministrazione:** la completezza e l’aggiornamento delle informazioni statiche dipendono dalla qualità, dal formato e dalla frequenza di pubblicazione dei dati degli enti territoriali.
- **Efficacia legata all’adozione degli utenti:** la copertura e l’affidabilità delle segnalazioni in tempo reale crescono con il numero di cittadini attivi sulla piattaforma.
