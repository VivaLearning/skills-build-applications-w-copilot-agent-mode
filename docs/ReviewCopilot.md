# OctoFit Copilot Review

## Osservazioni

### Architettura

- Le separazioni tra livello di presentazione, logica e dati sono definite chiaramente, ma la struttura potrebbe essere resa ancora più esplicita con esempi dei flussi principali.
- La configurazione delle porte è coerente; è importante mantenere il backend API privato e usare sempre il proxy di Vite per le chiamhe relative `/api`.
- La separazione tra route, service e modelli è appropriata: conviene mantenere la validazione nei modelli Mongoose e la logica applicativa nei service.
- I contratti API dovrebbero restare sincronizzati con i test e con la documentazione, in modo da evitare divergenze tra frontend e backend.

### Stato attuale dei test

- Il test data-tier attuale ha 4 test falliti su 5: mancano validazioni Mongoose per `User`, `Team`, `Activity` e `Workout`.
- La suite API ha 4 test falliti su 4:
  - la creazione di attività e team non è implementata e restituisce `501`;
  - la leaderboard restituisce una lista vuota;
  - i suggerimenti di workout non rispettano l'attività o il goal dell'utente.
- La suite frontend ha 15 test falliti su 15: i componenti mostrano ancora lo stato “Implementation pending” e non gestiscono correttamente caricamento, dati vuoti, successo ed errori.

### Miglioramenti consigliati

- Implementare prima i modelli Mongoose e il seed: riduce i problemi a cascata nelle suite API e frontend.
- Definire validazioni e risposte 4xx intenzionali per input vuoti, ObjectId non validi e risorse inesistenti, evitando errori 500 da cast di Mongoose.
- Rendere il seed stabile e idempotente con upsert, senza cancellare dati utente esistenti e senza lasciare membri obsoleti nelle team.
- Implementare funzioni service separate per attività, team, leaderboard e workout, mantenendo le route più semplici.
- Ordinare la leaderboard per punti totali decrescenti e assegnare rank 1-based.
- Nel suggeritore, utilizzare prima il goal o il tipo di attività recente dell'utente e fornire una motivazione comprensibile.
- Nel frontend usare `src/api/client.ts` come unico punto di accesso alle API e gestire con testo accessibile gli stati di loading, errore, vuoto e successo.
- Eseguire i test a ogni piccolo incremento e correggere prima il punto responsabile del fallimento.
- Non introdurre autenticazione in questa fase: può essere aggiunta come sfida successiva.

## Checklist

- [ ] Completare le decisioni di implementazione in `docs/architecture.md`.
- [ ] Aggiungere le validazioni richieste ai modelli `User`.
- [ ] Aggiungere le validazioni richieste ai modelli `Team`.
- [ ] Aggiungere le validazioni richieste ai modelli `Activity`.
- [ ] Aggiungere le validazioni richieste ai modelli `Workout`.
- [ ] Implementare un seed rappresentativo, idempotente e non distruttivo.
- [ ] Verificare il test data-tier.
- [ ] Implementare la creazione e la lista delle attività.
- [ ] Validare ObjectId e risorse esistenti per attività e team.
- [ ] Restituire risposte 4xx deliberate per input non valido.
- [ ] Implementare la creazione e la lista dei team.
- [ ] Implementare il calcolo e l'ordinamento della leaderboard.
- [ ] Implementare i suggerimenti workout basati su goal e attività recenti.
- [ ] Gestire email utente sconosciute con una risposta 4xx.
- [ ] Verificare la suite API.
- [ ] Implementare il component Activities.
- [ ] Implementare il component Teams.
- [ ] Implementare il component Leaderboard.
- [ ] Implementare il component Workouts.
- [ ] Gestire loading, contenuto vuoto, successi ed errori accessibili.
- [ ] Verificare la suite frontend.
- [ ] Eseguire il build backend.
- [ ] Eseguire il build frontend.
- [ ] Rivedere documentazione e contratti dopo l'implementazione.
