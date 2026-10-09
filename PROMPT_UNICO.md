# Prompt unico — test di polling IA↔IA, 10 turni

Usa questo stesso prompt in entrambe le istanze cambiando soltanto i due valori iniziali `IDENTITA` e `ALTRA_IA`.

```text
IDENTITA = <nome di questa istanza>
ALTRA_IA = <nome dell'altra istanza>

Repository condivisa: MATRIXNEO23/dialogo-tra-ia
File condiviso: dialogo.md

OBIETTIVO
Portare avanti autonomamente il dialogo registrato in dialogo.md fino al completamento di 10 turni totali, senza chiedere ad Alberto un nuovo prompt a ogni scambio.

REGOLE OPERATIVE
1. Leggi sempre l'ultima versione remota di dialogo.md prima di decidere se parlare.
2. Se `stato: WAITING_FOR_TOPIC`, non inventare l'argomento e non scrivere turni. Continua il controllo finché l'esecuzione rimane attiva.
3. Se `stato: ACTIVE`:
   - se non esistono turni e `prossimo_autore` coincide con IDENTITA, scrivi il Turno 001;
   - se l'ultimo turno è dell'altra IA e i turni correnti sono < 10, rispondi con il turno successivo;
   - se l'ultimo turno è tuo, non aggiungere nulla: torna al polling.
4. Ogni risposta deve contribuire davvero all'argomento: sviluppa, critica, approfondisci o correggi ciò che l'altra IA ha scritto. Non limitarti a confermare.
5. Mantieni i turni precedenti intatti. Per scrivere: rileggi il file, prepara il contenuto completo con il nuovo turno aggiunto in fondo e aggiorna usando lo SHA/blob corrente. Se GitHub rifiuta la scrittura per conflitto, rileggi il file e rivaluta da zero prima di riprovare.
6. Formato di ogni turno:

   ## Turno NNN
   Autore: <IDENTITA>

   <testo>

7. Dopo una scrittura riuscita NON considerare concluso l'incarico. Torna al polling di dialogo.md in attesa della replica dell'altra IA.
8. Intervallo obiettivo: non controllare più spesso di circa 30 secondi. Se l'ambiente non offre una vera primitive di attesa, non fingere che siano trascorsi 30 secondi: continua il test per quanto l'esecuzione lo consente e non dichiarare una persistenza che non esiste.
9. Non rispondere due volte allo stesso turno. Il numero del turno e l'autore dell'ultimo messaggio sono la protezione logica principale.
10. Quando scrivi il Turno 010:
    - imposta `stato: COMPLETED`;
    - imposta `turni_correnti: 10`;
    - imposta `prossimo_autore: NONE`;
    - aggiungi il Turno 010 in fondo;
    - poi termina il polling.
11. Dopo ogni turno da 001 a 009 aggiorna anche l'header:
    - `turni_correnti` = numero del turno appena scritto;
    - `prossimo_autore` = ALTRA_IA.
12. Non modificare altri file della repository durante questo test.
13. Se una limitazione della piattaforma o degli strumenti interrompe il polling prima del Turno 010, non fingere che il test sia terminato: lascia dialogo.md nello stato reale e, nella risposta finale della tua istanza, indica l'ultimo turno realmente osservato/scritto e il motivo dell'interruzione se noto.

CONDIZIONE DI FINE
Termina soltanto se una di queste condizioni è vera:
- dialogo.md è `COMPLETED` con 10 turni;
- compare esplicitamente `SESSION_END`;
- la piattaforma/strumento impedisce materialmente di continuare l'esecuzione.

IMPORTANTE
Questo è un esperimento sul comportamento di polling dentro un incarico lungo. Non sostituire autonomamente il meccanismo con Automazioni, watcher esterni, GitHub Actions o altri trigger: il test serve proprio a verificare quanto regge il solo incarico iniziale dato alle due istanze.
```
