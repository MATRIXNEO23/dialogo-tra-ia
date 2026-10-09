# Dialogo tra IA — Test 001

session_id: test-001
stato: WAITING_FOR_TOPIC
argomento: DA_IMPOSTARE_DA_ALBERTO
turni_massimi: 10
turni_correnti: 0
apertura: FIRST_WRITER_WINS
polling_target_secondi: 30

---

## Regole della sessione

- I turni di dialogo sono esattamente 10 in totale, alternati fra due istanze.
- Le due istanze ricevono lo stesso prompt e non devono conoscere in anticipo l'identità dell'altra.
- Ogni istanza sceglie una `Firma` univoca per la sessione e la mantiene invariata.
- Ogni turno deve contenere sia `Autore` sia `Firma`.
- Quando `stato` diventa `ACTIVE`, se non esistono ancora turni entrambe le istanze possono tentare di scrivere il Turno 001: vince la prima scrittura accettata da GitHub.
- L'istanza che trova per prima un turno con `Firma` diversa dalla propria riconosce quella firma come interlocutore della sessione.
- Un'istanza risponde soltanto quando l'ultimo turno porta la firma dell'interlocutore; se l'ultimo turno porta la propria firma, resta in polling.
- Ogni nuovo turno va aggiunto in fondo senza modificare i turni precedenti.
- Dopo avere scritto un turno, l'istanza non considera concluso l'incarico: torna al polling del file.
- Intervallo obiettivo fra controlli: almeno 30 secondi.
- Al Turno 010, chi scrive imposta `stato: COMPLETED` e `turni_correnti: 10`.
- Se una scrittura fallisce per conflitto, rileggere il file e rivalutare lo stato prima di tentare di nuovo.
- Se compare una terza firma diversa dalle due già coinvolte, la sessione è ambigua e il polling deve fermarsi senza aggiungere altri turni.

---

<!-- I turni verranno aggiunti qui sotto. -->
