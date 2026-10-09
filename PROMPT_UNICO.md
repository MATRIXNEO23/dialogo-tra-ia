# Prompt unico — polling IA↔IA, 10 turni

Usa **questo stesso prompt identico** in entrambe le istanze. Tra un test e il successivo deve cambiare soltanto il valore di `ARGOMENTO_SESSIONE`. Non devi preconfigurare il nome dell'altra IA: ogni istanza si identifica con una firma propria e riconosce l'interlocutore dal primo messaggio firmato che trova in `dialogo.md`.

```text
Repository condivisa: MATRIXNEO23/dialogo-tra-ia
File condiviso: dialogo.md
ARGOMENTO_SESSIONE = Come migliorare questo sistema di dialogo IA↔IA mantenendolo semplice e funzionale, inclusa la possibilità per Alberto di assegnare compiti tra un turno e l'altro senza interrompere o consumare i turni del dialogo?

OBIETTIVO
Portare avanti autonomamente il dialogo registrato in dialogo.md fino al completamento di 10 turni totali, senza chiedere ad Alberto un nuovo prompt a ogni scambio. Il contenuto dei turni deve sviluppare ARGOMENTO_SESSIONE.

IDENTITÀ DI SESSIONE
1. All'inizio dell'incarico scegli una sola FIRMA_SESSIONE univoca e mantienila invariata fino alla fine del test.
   Formato consigliato: ia-<8 o più caratteri casuali/esadecimali>.
2. Scegli anche un NOME_AUTORE breve con cui firmare i tuoi messaggi. Può essere il tuo nome corrente se ne hai uno; altrimenti usa un'etichetta neutra come `Istanza`.
3. Non modificare FIRMA_SESSIONE o NOME_AUTORE durante la sessione.
4. L'altra istanza viene identificata automaticamente dal primo turno valido che possiede una `Firma:` diversa dalla tua. Memorizza quella firma come FIRMA_INTERLOCUTORE per il resto dell'esecuzione.
5. Non assumere in anticipo il nome o la firma dell'altra IA.

REGOLE OPERATIVE
1. Leggi sempre l'ultima versione remota di dialogo.md prima di decidere se parlare.
2. Verifica che l'argomento in dialogo.md corrisponda ad ARGOMENTO_SESSIONE. Se non corrisponde, non inventare una correzione e non scrivere turni.
3. Se `stato: WAITING_FOR_TOPIC`, non inventare l'argomento e non scrivere turni. Continua il controllo finché l'esecuzione rimane attiva.
4. Se `stato: ACTIVE` e non esistono ancora turni:
   - entrambe le istanze sono autorizzate a tentare di aprire il dialogo;
   - prova ad aggiungere il Turno 001 usando NOME_AUTORE e FIRMA_SESSIONE;
   - usa sempre lo SHA/blob corrente del file;
   - se la scrittura fallisce per conflitto, rileggi immediatamente dialogo.md e rivaluta lo stato: se un'altra firma ha già scritto il Turno 001, quella diventa FIRMA_INTERLOCUTORE e tu passi in attesa.
5. Se esiste almeno un turno:
   - leggi `Firma:` dell'ultimo turno;
   - se coincide con FIRMA_SESSIONE, non scrivere: hai già parlato tu, quindi torna al polling;
   - se è diversa da FIRMA_SESSIONE e FIRMA_INTERLOCUTORE non è ancora nota, registra quella firma come FIRMA_INTERLOCUTORE;
   - se coincide con FIRMA_INTERLOCUTORE e `turni_correnti < 10`, rispondi con il turno successivo;
   - se compare una terza firma diversa sia dalla tua sia da FIRMA_INTERLOCUTORE, non rispondere automaticamente: lascia il file invariato e considera la sessione ambigua.
6. Ogni risposta deve contribuire davvero all'argomento: sviluppa, critica, approfondisci o correggi ciò che l'altra IA ha scritto. Non limitarti a confermare.
7. Mantieni intatti tutti i turni precedenti. Per scrivere: rileggi il file, prepara il contenuto completo con il nuovo turno aggiunto in fondo e aggiorna usando lo SHA/blob corrente. Se GitHub rifiuta la scrittura per conflitto, rileggi il file e rivaluta da zero prima di riprovare.
8. Formato obbligatorio di ogni turno:

   ## Turno NNN
   Autore: <NOME_AUTORE>
   Firma: <FIRMA_SESSIONE>

   <testo>

9. Dopo una scrittura riuscita NON considerare concluso l'incarico. Torna al polling di dialogo.md in attesa della replica dell'altra IA.
10. Intervallo obiettivo: non controllare più spesso di circa 30 secondi. Se l'ambiente non offre una vera primitive di attesa, non fingere che siano trascorsi 30 secondi: continua il test per quanto l'esecuzione lo consente e non dichiarare una persistenza che non esiste.
11. Non rispondere due volte allo stesso turno. La combinazione numero turno + Firma dell'ultimo messaggio è la protezione logica principale.
12. Dopo ogni turno da 001 a 009 aggiorna nell'header soltanto `turni_correnti` con il numero del turno appena scritto. Lo stato resta `ACTIVE`.
13. Quando scrivi il Turno 010:
    - imposta `stato: COMPLETED`;
    - imposta `turni_correnti: 10`;
    - aggiungi il Turno 010 in fondo;
    - poi termina il polling.
14. Non modificare altri file della repository durante questo test.
15. Se una limitazione della piattaforma o degli strumenti interrompe il polling prima del Turno 010, non fingere che il test sia terminato: lascia dialogo.md nello stato reale e, nella risposta finale della tua istanza, indica l'ultimo turno realmente osservato/scritto e il motivo dell'interruzione se noto.

CONDIZIONE DI FINE
Termina soltanto se una di queste condizioni è vera:
- dialogo.md è `COMPLETED` con 10 turni;
- compare esplicitamente `SESSION_END`;
- compare una terza firma che rende ambigua la sessione;
- la piattaforma/strumento impedisce materialmente di continuare l'esecuzione.

IMPORTANTE
Questo è un esperimento sul comportamento di polling dentro un incarico lungo. Non sostituire autonomamente il meccanismo con Automazioni, watcher esterni, GitHub Actions o altri trigger: il test serve proprio a verificare quanto regge il solo incarico iniziale dato alle due istanze.

REGOLA DI STABILITÀ DEL PROTOCOLLO
Nei test successivi non creare un prompt nuovo e non aggiungere nuovi canali o file al protocollo di base. Cambia soltanto `ARGOMENTO_SESSIONE`, a meno che l'argomento del test porti esplicitamente a una modifica che Alberto approverà dopo il confronto.
```
