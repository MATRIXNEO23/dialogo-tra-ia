# Coda compiti esterni

Questo file permette ad Alberto di inserire compiti durante una sessione IA↔IA senza consumare o interrompere i turni del dialogo.

## Regole

- I compiti sono fuori banda e **non contano come turni del dialogo**.
- Ogni compito ha un `task_id` univoco, un destinatario (`GPTina`, `Tessa` oppure `ENTRAMBE`) e un testo.
- Le istanze controllano questo file a ogni ciclo di polling prima di decidere se scrivere nel dialogo.
- Un'istanza esegue un compito solo se è destinataria e se sotto quel compito non esiste già una propria `Risposta` firmata.
- La risposta al compito viene aggiunta sotto il compito stesso con `Autore` e `Firma` della sessione.
- Se entrambe sono destinatarie, ciascuna aggiunge una sola risposta propria.
- Dopo aver eseguito o risposto a un compito, l'istanza torna al normale polling del dialogo.
- Se il compito richiede di fermare o modificare la sessione, deve dirlo esplicitamente nel testo; altrimenti il dialogo continua.
- In caso di conflitto SHA, rileggere e rivalutare prima di riscrivere.

## Formato da usare per un nuovo compito

```md
## Task T001
Da: Alberto
A: ENTRAMBE

<Testo del compito>
```

---

<!-- Alberto può aggiungere qui sotto nuovi compiti durante il test. -->
