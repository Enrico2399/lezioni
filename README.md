# Avanzamento lezioni

Una pagina statica (`index.html`) per segnare le lezioni viste, con il calendario delle ore di studio.

La pagina ha due tab:

- **Attuale** (si apre per prima): Fisica, Probabilità e Statistica, Reti Neurali.
- **Totale**: tutte le materie che mancano.

Ogni tab ha la sua barra di avanzamento, il suo obiettivo e il suo calendario. Le tab si possono aprire direttamente con `…/#attuale` e `…/#totale`.

L'obiettivo si calcola da solo: ore di lezione mancanti alla data di inizio ÷ lezioni guardate in contemporanea. Le settimane sono quelle che servono con le fasce del calendario (17 h a settimana).
I progressi si salvano da soli nel file `progress.json` sul branch `progress-data` dello stesso repository: non serve un database.

## Pubblicazione su GitHub Pages

1. Crea un repository **pubblico**, per esempio `lezioni`, e carica `index.html` sul branch `main`.
2. Vai in **Settings → Pages → Build and deployment**, scegli *Deploy from a branch*, poi `main` e `/ (root)`.
3. Dopo un minuto la pagina è online su `https://<tuo-utente>.github.io/lezioni/`.

## Salvataggio automatico online (una volta per ogni dispositivo)

1. Su GitHub vai in **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
   - *Repository access*: seleziona solo il repository `lezioni`.
   - *Permissions → Repository permissions → Contents*: **Read and write**.
   - Scegli una scadenza lunga, per esempio 1 anno.
2. Apri la pagina, clicca **Impostazioni** e incolla il token. Utente e repository vengono compilati da soli.
3. Da questo momento ogni modifica viene salvata online dopo circa 1,5 secondi. Gli altri dispositivi si aggiornano quando riapri la pagina o al massimo ogni 2 minuti.

Il token resta salvato solo nel browser di quel dispositivo e non finisce mai nel repository. Senza token la pagina mostra i progressi ma non li salva online.
I dati vanno su un branch separato (`progress-data`), quindi i salvataggi non fanno ripubblicare il sito.

## App su Android

Carica nel repository tutti i file: `index.html`, `manifest.webmanifest`, `sw.js` e le 4 icone `.png`.

1. Apri il sito con **Chrome** sul telefono.
2. Tocca il menu **⋮** e scegli **Installa app** (oppure **Aggiungi a schermata Home**).
3. L'icona compare tra le app: si apre a schermo intero, senza barra degli indirizzi, sulla tab **Attuale**.

L'app si apre anche senza connessione. Le modifiche fatte offline restano sul telefono e vengono salvate online appena torna la rete.
Se nel Chrome del telefono avevi già sbloccato il salvataggio con la password, l'app è già configurata: usa gli stessi dati.

## Altri dispositivi con una password

1. Sul dispositivo dove hai già inserito il token apri **Impostazioni → Password per gli altri dispositivi**.
2. Scegli una password lunga (almeno 12 caratteri, meglio 3-4 parole) e clicca **Imposta password**.
3. Su un browser nuovo compare un riquadro blu: scrivi la password, clicca **Sblocca** e quel browser resta configurato.

Il token viene cifrato con la password (AES-GCM, chiave derivata con PBKDF2 a 600.000 iterazioni) e salvato in `progress.json`. In chiaro non finisce mai online.
Se rigeneri il token, imposta di nuovo la password dal dispositivo che ha il token nuovo.

## Personalizzazione

Tutto si trova in cima allo `<script>` dentro `index.html`:

- `GROUPS`: le materie e il numero di lezioni di ciascuna.
- `TABS`: quali materie compaiono nella tab **Attuale** (le metti in `ids`).
- `INITIAL`: la lezione da cui riparti in ogni materia.
- `SLOTS`: le fasce del calendario (`dow`: 0 = domenica … 6 = sabato, nome, minuti).

Da **Impostazioni** puoi cambiare la data di inizio e il numero di lezioni guardate in contemporanea. Il pulsante **Riparti da oggi** ricalcola l'obiettivo partendo dai progressi di oggi.

## Prova in locale

Apri `index.html` con un doppio clic: funziona anche senza GitHub, ma salva solo nel browser che stai usando.
