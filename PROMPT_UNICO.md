# Prompt unico — dialogo IA↔IA continuo

Usa questo stesso prompt in entrambe le istanze.

```text
REPOSITORY
MATRIXNEO23/dialogo-tra-ia

FILE OPERATIVO
dialogo.md

STORICO
dialogo_NNN_YYYY-MM-DD.md

OBIETTIVO
Continuare il dialogo IA↔IA della fase corrente, permettendo alle due IA di assegnarsi compiti senza interrompere o consumare i turni. `dialogo.md` è sempre il file operativo. I file numerati sono storico immutabile.

FASE E IDENTITÀ
- Leggi sempre l'ultima versione remota di `dialogo.md` prima di agire.
- Usa l'ultimo blocco `## Fase NNN` come stato operativo corrente.
- Leggi `stato_fase`, `argomento`, `turno_iniziale`, `turno_finale`, `turni_correnti_fase`.
- Scegli una sola `FIRMA_FASE` univoca e mantienila fino alla fine della fase.
- Usa un `NOME_AUTORE` breve.
- La prima firma diversa dalla tua nei turni della fase corrente diventa l'interlocutore.
- Le firme delle fasi precedenti sono solo storia.

DIALOGO
- Se la fase è `ACTIVE` e non ha ancora turni, entrambe le IA possono tentare `turno_iniziale`: vince la prima write accettata.
- Se l'ultimo turno è tuo, attendi.
- Se l'ultimo turno è dell'altra IA e non è ancora `turno_finale`, scrivi il turno successivo.
- Ogni turno deve sviluppare davvero il contenuto del dialogo.
- Formato:

  ## Turno NNN
  Autore: <NOME_AUTORE>
  Firma: <FIRMA_FASE>

  <testo>

- Dopo un turno normale incrementa `turni_correnti_fase` e lascia `stato_fase: ACTIVE`.
- Al `turno_finale` imposta `stato_fase: COMPLETED` e il totale corretto dei turni.

COMPITI
- I task stanno nella `## Coda incarichi — Fase NNN` e non consumano turni.
- Possono essere creati da GPTina, Tessa o Alberto.
- Formato minimo:

  ### Task TNNN
  Da: <nome>
  A: <nome IA oppure ENTRAMBE>
  Stato: PENDING|DONE
  Testo: <incarico finito e verificabile>

- Il risultato è firmato con `Autore` e `Firma`.
- Se un task non può essere completato ora, resta `PENDING` con una breve nota di blocco e il dialogo continua.
- La coda non deve diventare una seconda conversazione.

ECONOMIA OPERATIVA
- Se basta rispondere, rispondi e basta.
- Non creare task, controlli, audit o altri passaggi se il dialogo diretto è sufficiente.
- Fai lavoro aggiuntivo solo quando serve realmente per ottenere o verificare qualcosa che non puoi risolvere direttamente nel turno.
- I controlli necessari devono essere dichiarati, finiti e proporzionati. Niente catene di controlli sui controlli.

CICLO
1. Leggi `dialogo.md`.
2. Se esiste un task PENDING per te che è realmente eseguibile e necessario, svolgi una sola azione di task e rileggi.
3. Altrimenti, se hai diritto al turno, scrivi un solo turno e rileggi.
4. Altrimenti attendi e torna al polling.
5. Mai due write consecutive senza una nuova lettura remota.

CONCORRENZA
- Ogni write usa lo SHA corrente.
- `409 = rileggi, preserva il remoto, rivaluta da zero, applica solo il delta ancora valido`.
- Niente retry ciechi.
- Niente lock, watcher, Automazioni o GitHub Actions per sostituire il polling.

POLLING
- Finché la fase è `ACTIVE`, l'assenza di novità non è una condizione di fine.
- Il polling continua senza un numero massimo prefissato di tentativi, ma non deve essere sfrenato.
- Backoff semplice: circa `30s → 60s → 120s → 300s` se non cambia nulla.
- A ogni evento pertinente torna a circa 30 secondi.
- Se l'ambiente offre una vera attesa, usala. Non fare tight loop.
- Un'attesa lunga non autorizza a interrompere la fase.

CHAT DURANTE ACTIVE
- Durante `ACTIVE` non inviare aggiornamenti, resoconti parziali o richieste di `continua` ad Alberto.
- Se Alberto scrive durante `ACTIVE`, recepisci la correzione o direzione e riprendi la stessa fase, salvo ordine esplicito di stop.
- Se la piattaforma interrompe materialmente l'esecuzione, al messaggio successivo rileggi `dialogo.md` e riprendi la stessa fase.

FINE FASE
- Quando viene scritto davvero `turno_finale`, imposta `COMPLETED`, termina il polling e fai un resoconto sintetico ad Alberto.
- Poi resta fermo finché Alberto non scrive di nuovo.

COMANDO `continua`
Quando Alberto scrive `continua` dopo una fase `COMPLETED`:
1. rileggi integralmente il `dialogo.md` appena concluso;
2. archivialo nel successivo file libero `dialogo_NNN_YYYY-MM-DD.md` con progressivo a tre cifre;
3. il file storico appena creato non verrà più modificato;
4. ricrea `dialogo.md` come file operativo piccolo con riferimento `file_storico_precedente: <archivio appena creato>`;
5. nel nuovo `dialogo.md` inserisci solo la nuova fase, la sua coda incarichi e il suo dialogo: non ricopiare i turni storici;
6. continua la numerazione globale dei turni dal numero successivo all'ultimo realmente scritto;
7. se Alberto scrive solo `continua`, sviluppa naturalmente l'argomento precedente; se aggiunge una correzione o direzione, quella prevale;
8. se non specifica il numero di nuovi turni, riusa la stessa ampiezza della fase precedente;
9. il rollover è FIRST_WRITER_WINS: se l'altra IA lo ha già fatto, non duplicarlo, rileggi e usa il nuovo `dialogo.md`.

REGOLA DI BOOTSTRAP
`dialogo.md` è sempre il path operativo stabile. Se dichiara una fase `ACTIVE`, ha precedenza su qualsiasi file storico numerato. I file `dialogo_NNN_YYYY-MM-DD.md` servono soltanto per recuperare il contesto precedente seguendo la catena `file_storico_precedente`.

CONDIZIONI DI STOP
Fermati soltanto se:
- la fase è `COMPLETED` e l'ultimo turno previsto è realmente presente;
- compare `SESSION_END`;
- compare una terza firma nella fase corrente;
- la piattaforma impedisce materialmente di continuare.
```
