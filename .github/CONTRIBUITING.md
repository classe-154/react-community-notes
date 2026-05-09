## Esempio di guida su come contribuire

### 1. Sincronizzazione Iniziale (Pull)

Prima di iniziare a scrivere qualsiasi cosa, devi assicurarti di avere l'ultima versione del lavoro dei tuoi colleghi.

* **Comando:** `git pull origin main`
* **Perché:** Evita di lavorare su file obsoleti e riduce al minimo i conflitti di merge.

### 2. Creazione di un Branch (Ramo)

**Mai lavorare direttamente su `main`.** Ogni nuova lezione o esempio va creato in un ramo separato.

* **Comando:** `git checkout -b feature/nome-tuo-argomento`
* **Esempio:** `git checkout -b feature/hooks-tutorial`

### 3. Lavoro e Commit

Lavori nella tua cartella `playground/` o inizi a strutturare la proposta in `proposals/`. Quando hai finito una parte:

* **Aggiungi i file:** `git add .`
* **Salva con messaggio:** `git commit -m "Descrizione chiara di cosa hai aggiunto"`

### 4. Caricamento sul Server (Push)

Invia il tuo ramo locale su GitHub.

* **Comando:** `git push origin feature/hooks-tutorial`
* **Nota:** Questo non modifica ancora il progetto ufficiale, crea solo una copia del tuo ramo online.

### 5. Pull Request (La fase di Review)

Qui avviene la magia della collaborazione:

1. Vai su GitHub e clicca su **"Compare & pull request"**.
2. Descrivi cosa hai fatto.
3. **Review:** I tuoi compagni di team guardano il tuo codice/appunti e lasciano commenti o suggeriscono correzioni direttamente nell'interfaccia di GitHub.
4. Se ci sono modifiche da fare, le fai localmente, fai di nuovo `commit` e `push`, e la Pull Request si aggiornerà automaticamente.

### 6. Merge e Pulizia

Una volta che il team ha dato l'approvazione (il famoso **LGTM** - *Looks Good To Me*):

1. Si clicca su **"Merge pull request"** su GitHub.
2. Il tuo lavoro viene unito al ramo `main`.
3. Ora tutti gli altri membri faranno `git pull origin main` per ricevere il tuo contributo.

---

### Riassunto visivo per il team

Potete inserire questo schema nel vostro `WORKFLOW.md`:

| Fase | Azione | Comando Git |
| --- | --- | --- |
| **Start** | Sincronizza il locale | `git pull origin main` |
| **Branch** | Crea spazio di lavoro | `git checkout -b nome-branch` |
| **Work** | Scrivi e salva | `git add` + `git commit -m "..."` |
| **Share** | Invia a GitHub | `git push origin nome-branch` |
| **Review** | Apri Pull Request | (Interfaccia Web GitHub) |
| **Done** | Merge su Main | (Interfaccia Web GitHub) |

---
