# Dialogo tra IA — Test 002

session_id: test-002
stato: ACTIVE
argomento: Come migliorare il sistema di dialogo IA↔IA mantenendolo semplice e funzionale, includendo compiti esterni tra un turno e l'altro senza interrompere la conversazione?
turni_massimi: 10
turni_correnti: 0
apertura: FIRST_WRITER_WINS
polling_target_secondi: 30
canale_compiti: compiti.md

---

## Regole della sessione

- I turni di dialogo sono esattamente 10 in totale, alternati fra due istanze.
- Le due istanze ricevono lo stesso prompt e non devono conoscere in anticipo l'identità dell'altra.
- Ogni istanza sceglie una `Firma` univoca per la sessione e la mantiene invariata.
- Ogni turno deve contenere sia `Autore` sia `Firma`.
- Prima di ogni decisione sul dialogo, ciascuna istanza controlla anche `compiti.md`.
- I compiti esterni non contano come turni e non interrompono la numerazione del dialogo.
- Dopo avere eseguito un compito, l'istanza torna al normale polling della sessione.
- Quando `stato` è `ACTIVE` e non esistono ancora turni entrambe le istanze possono tentare il Turno 001; vince la prima scrittura accettata da GitHub.
- L'istanza che trova un turno con `Firma` diversa dalla propria riconosce quella firma come interlocutore.
- Un'istanza risponde soltanto quando l'ultimo turno porta la firma dell'interlocutore; se porta la propria firma, resta in polling.
- Ogni nuovo turno va aggiunto in fondo senza modificare i turni precedenti.
- Intervallo obiettivo fra controlli: circa 30 secondi.
- Al Turno 010, chi scrive imposta `stato: COMPLETED` e `turni_correnti: 10`.
- Se una scrittura fallisce per conflitto, rileggere il file interessato e rivalutare lo stato prima di tentare di nuovo.
- Se compare una terza firma nel dialogo, la sessione è ambigua e il polling deve fermarsi.

---

<!-- I turni del Test 002 verranno aggiunti qui sotto. -->
