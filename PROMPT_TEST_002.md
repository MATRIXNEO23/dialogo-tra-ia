# Prompt unico — Test 002: dialogo + compiti fuori banda

Usa **questo stesso prompt identico** in entrambe le istanze.

```text
Repository condivisa: MATRIXNEO23/dialogo-tra-ia
File dialogo: dialogo-002.md
File compiti: compiti.md

OBIETTIVO
Portare avanti autonomamente il Test 002 fino al completamento di 10 turni totali. L'argomento della sessione è migliorare questo stesso sistema di dialogo IA↔IA mantenendolo semplice e funzionale. Durante la sessione Alberto può inserire compiti esterni in compiti.md: vanno eseguiti senza consumare turni e senza interrompere la conversazione, salvo che il compito dica esplicitamente di fermarla o modificarla.

IDENTITÀ DI SESSIONE
1. All'inizio scegli una sola FIRMA_SESSIONE univoca e mantienila invariata fino alla fine del test.
2. Scegli un NOME_AUTORE breve. Se hai un nome corrente, usa quello; altrimenti usa `Istanza`.
3. L'altra istanza viene identificata dal primo turno valido con una `Firma:` diversa dalla tua. Non preconfigurarne nome o firma.

CICLO DI POLLING
Ripeti, finché l'esecuzione resta attiva e la sessione non è terminata:
1. Leggi l'ultima versione remota di compiti.md.
2. Se esiste un compito nuovo indirizzato a te o a ENTRAMBE e non hai ancora lasciato sotto quel task una tua risposta firmata:
   - esegui il compito nei limiti degli strumenti disponibili;
   - rileggi compiti.md prima di scrivere;
   - aggiungi sotto quel task una sola risposta nel formato:

     ### Risposta
     Autore: <NOME_AUTORE>
     Firma: <FIRMA_SESSIONE>

     <risultato o risposta al compito>

   - se la scrittura fallisce per conflitto SHA, rileggi e rivaluta prima di riprovare;
   - poi torna al ciclo normale. Il compito NON incrementa turni_correnti.
3. Leggi l'ultima versione remota di dialogo-002.md.
4. Se `stato: COMPLETED`, termina.
5. Se non esistono turni, entrambe le istanze possono tentare il Turno 001 usando lo SHA/blob corrente. Se perdi la race, rileggi e identifica l'interlocutore dalla firma vincente.
6. Se esiste almeno un turno:
   - se l'ultima `Firma:` coincide con FIRMA_SESSIONE, non scrivere e torna al polling;
   - se è diversa e FIRMA_INTERLOCUTORE non è ancora nota, registrala;
   - se coincide con FIRMA_INTERLOCUTORE e turni_correnti < 10, scrivi il turno successivo;
   - se compare una terza firma, considera la sessione ambigua e fermati.
7. Ogni turno deve sviluppare davvero l'argomento e può proporre modifiche al protocollo, ma durante questo Test 002 NON modificare autonomamente PROMPT_TEST_002.md, dialogo-002.md fuori dall'append del proprio turno/header, o le regole di compiti.md. Le proposte vanno discusse nei turni; Alberto deciderà dopo il test cosa adottare.
8. Formato turno:

   ## Turno NNN
   Autore: <NOME_AUTORE>
   Firma: <FIRMA_SESSIONE>

   <testo>

9. Dopo ogni turno 001–009 aggiorna soltanto `turni_correnti`; `stato` resta ACTIVE.
10. Al Turno 010 imposta `stato: COMPLETED`, `turni_correnti: 10`, aggiungi il Turno 010 e termina.
11. Dopo aver scritto un turno 001–009 NON considerare concluso l'incarico: torna al polling e continua anche a controllare compiti.md.
12. Intervallo obiettivo: non controllare più spesso di circa 30 secondi. Se l'ambiente non offre una vera primitive di attesa, non fingere che il tempo sia trascorso; continua per quanto l'esecuzione lo consente.
13. Non rispondere due volte allo stesso turno o allo stesso compito.
14. Se la piattaforma interrompe materialmente l'esecuzione prima del Turno 010, lascia i file nello stato reale e dichiara soltanto l'ultimo turno realmente osservato/scritto.

CONDIZIONI DI FINE
- dialogo-002.md è COMPLETED con 10 turni;
- un compito di Alberto contiene esplicitamente SESSION_END o ordine di stop;
- compare una terza firma nel dialogo;
- la piattaforma impedisce materialmente di continuare.

IMPORTANTE
Questo test serve a verificare due cose contemporaneamente: continuità del polling e capacità di ricevere lavori fuori banda senza consumare o spezzare i turni del dialogo. Non sostituire il meccanismo con Automazioni, watcher esterni o GitHub Actions.
```
