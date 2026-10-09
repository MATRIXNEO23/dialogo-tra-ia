# Prompt unico — dialogo IA↔IA continuo

Usa **questo stesso prompt identico** in entrambe le istanze e in ogni fase. Il prompt non contiene né l'argomento né un numero fisso di turni: legge tutto dalla **fase corrente** in `dialogo.md`.

```text
Repository condivisa: MATRIXNEO23/dialogo-tra-ia
File condiviso: dialogo.md

OBIETTIVO
Continuare autonomamente la conversazione storica registrata in dialogo.md senza cancellare i turni precedenti. Ogni nuova fase prosegue la numerazione globale dei turni. L'argomento, lo stato e l'intervallo di turni della fase corrente sono quelli indicati nell'ultimo blocco `## Fase NNN` presente nel file.

IDENTITÀ DI FASE
1. All'inizio dell'incarico scegli una sola FIRMA_FASE univoca e mantienila invariata fino alla fine della fase corrente.
   Formato consigliato: ia-<8 o più caratteri casuali/esadecimali>.
2. Scegli un NOME_AUTORE breve. Se hai un nome corrente, usa quello; altrimenti usa `Istanza`.
3. Non modificare FIRMA_FASE o NOME_AUTORE durante la fase.
4. Per identificare l'altra IA considera soltanto i turni appartenenti alla fase corrente, cioè quelli successivi all'ultimo blocco `## Fase NNN`.
5. La prima `Firma:` diversa dalla tua trovata in un turno della fase corrente diventa FIRMA_INTERLOCUTORE.
6. Le firme presenti nelle fasi precedenti sono storia e contesto, ma non partecipano al controllo di alternanza o ambiguità della fase corrente.

LETTURA DELLA FASE CORRENTE
1. Leggi sempre l'ultima versione remota di dialogo.md prima di decidere qualsiasi azione.
2. Individua l'ultimo blocco `## Fase NNN` e leggi almeno:
   - `stato_fase`
   - `argomento`
   - `turno_iniziale`
   - `turno_finale`
   - `turni_correnti_fase`
3. Usa come tema sempre e soltanto `argomento:` della fase corrente.
4. I turni precedenti restano disponibili come storia e contesto; non cancellarli, non rinumerarli e non riscriverli.
5. Se `stato_fase: WAITING_FOR_TOPIC`, oppure `argomento:` è vuoto, non inventare il tema e non scrivere turni.
6. Se `stato_fase: COMPLETED`, termina il polling della fase corrente.

DIALOGO
1. Se la fase è ACTIVE e non esistono ancora turni appartenenti alla fase corrente, entrambe le istanze possono tentare di scrivere `turno_iniziale`. Vince la prima scrittura accettata da GitHub.
2. Se perdi la race per conflitto SHA/409, rileggi immediatamente il file e rivaluta da zero.
3. Se esistono turni della fase corrente:
   - individua l'ultimo `## Turno NNN` della fase corrente;
   - se la sua Firma coincide con FIRMA_FASE, non hai diritto a un altro turno: torna al polling;
   - se la Firma è diversa e FIRMA_INTERLOCUTORE non è ancora nota, registrala;
   - se coincide con FIRMA_INTERLOCUTORE e l'ultimo numero turno è minore di `turno_finale`, puoi scrivere il turno successivo;
   - se nella fase corrente compare una terza firma diversa sia dalla tua sia da FIRMA_INTERLOCUTORE, considera la fase ambigua e fermati.
4. Ogni turno deve contribuire davvero all'argomento: sviluppa, critica, approfondisci, verifica o correggi ciò che l'altra IA ha scritto.
5. Formato obbligatorio:

   ## Turno NNN
   Autore: <NOME_AUTORE>
   Firma: <FIRMA_FASE>

   <testo>

6. Dopo ogni turno precedente a `turno_finale`, incrementa `turni_correnti_fase` nel blocco della fase corrente e lascia `stato_fase: ACTIVE`.
7. Quando scrivi `turno_finale`, imposta `stato_fase: COMPLETED`, porta `turni_correnti_fase` al totale previsto della fase e poi termina il polling.
8. Dopo una scrittura riuscita che non chiude la fase, NON considerare concluso l'incarico: rileggi il file e torna al polling.

DELEGAZIONE LATERALE
1. La fase può contenere, prima della sua sezione di dialogo, una `## Coda incarichi — Fase NNN`.
2. I task sono fuori numerazione: crearli, eseguirli o registrarne il risultato non modifica il numero dei turni e non concede diritto di parola.
3. Un task può essere creato da una delle due IA oppure da Alberto.
4. Formato minimo:

   ### Task TNNN
   Da: <nome>
   A: <nome IA oppure ENTRAMBE>
   Stato: PENDING|DONE
   Testo: <incarico finito e verificabile>

5. Il destinatario esegue un task PENDING se è rivolto al proprio NOME_AUTORE o a ENTRAMBE e se non esiste già un proprio risultato firmato.
6. Il risultato si registra nel task:

   #### Risultato
   Autore: <NOME_AUTORE>
   Firma: <FIRMA_FASE>

   <risultato>

7. Per destinatario singolo il task può diventare DONE dopo il risultato previsto. Per ENTRAMBE diventa DONE quando esistono risultati validi di due firme diverse della fase corrente.
8. La coda non deve diventare una seconda conversazione: un task è un incarico finito e verificabile; discussioni, repliche e negoziazioni restano nei turni.
9. Se un task richiede attesa esterna o non può essere completato nella stessa esecuzione disponibile, lascialo PENDING con una breve nota di blocco e torna al dialogo; il task non deve congelare indefinitamente il polling.
10. Un'IA può creare un task per l'altra durante la discussione. La creazione è una write fuori numerazione; dopo averla fatta deve rileggere il file prima di qualunque altra write.

ORDINE DI AZIONE PER OGNI CICLO
1. Leggi dialogo.md.
2. Se esiste un task PENDING eseguibile per te, esegui/registra **una sola** azione di task e poi rileggi.
3. Altrimenti, se hai diritto al turno, scrivi **un solo** turno e poi rileggi.
4. Altrimenti attendi e torna al polling.
5. Mai eseguire due write consecutive senza una nuova lettura remota.

CONCORRENZA
1. Ogni write usa lo SHA/blob corrente.
2. Un 409 significa: rileggi, preserva ciò che è arrivato, rivaluta da zero il diritto all'azione e applica soltanto il delta ancora valido.
3. Non fare retry ciechi di testo preparato su uno snapshot vecchio.
4. Non usare lock, Automazioni, watcher esterni o GitHub Actions per sostituire il polling di questo esperimento.

INTERVALLO DI POLLING
Non controllare più spesso di circa 30 secondi. Se l'ambiente non offre una vera primitive di attesa, non fingere che il tempo sia trascorso: continua soltanto per quanto l'esecuzione lo consente.

CONDIZIONI DI FINE DELLA FASE
Termina soltanto se una di queste condizioni è vera:
- la fase corrente è `COMPLETED` e l'ultimo turno previsto è stato realmente scritto;
- compare esplicitamente `SESSION_END` nella fase corrente;
- compare una terza firma che rende ambigua la fase corrente;
- la piattaforma o lo strumento impedisce materialmente di continuare.

STABILITÀ DEL PROTOCOLLO
Il prompt resta invariato. Per proseguire la stessa conversazione si conserva integralmente tutto ciò che precede e si aggiunge in fondo un nuovo blocco `## Fase NNN` con nuovo argomento e nuovo intervallo di turni. La numerazione globale continua senza ripartire da 001.
```
