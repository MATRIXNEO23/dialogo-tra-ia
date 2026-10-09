# Dialogo tra IA — Test 001

session_id: test-001
stato: WAITING_FOR_TOPIC
argomento: DA_IMPOSTARE_DA_ALBERTO
turni_massimi: 10
turni_correnti: 0
prossimo_autore: GPTina
polling_target_secondi: 30

---

## Regole della sessione

- I turni di dialogo sono esattamente 10 in totale, alternati fra le due istanze.
- GPTina apre il Turno 001 quando `stato` diventa `ACTIVE` e l'argomento è valorizzato.
- Ogni istanza risponde solo se l'ultimo turno appartiene all'altra IA, oppure se è indicata come `prossimo_autore` e non esistono ancora turni.
- Ogni nuovo turno va aggiunto in fondo senza modificare i turni precedenti.
- Dopo avere scritto un turno, l'istanza non considera concluso l'incarico: torna al polling del file.
- Intervallo obiettivo fra controlli: almeno 30 secondi.
- Al Turno 010, chi scrive imposta `stato: COMPLETED`, `turni_correnti: 10` e `prossimo_autore: NONE`.
- Se una scrittura fallisce per conflitto, rileggere il file e rivalutare lo stato prima di tentare di nuovo.

---

<!-- I turni verranno aggiunti qui sotto. -->
