#### Modulo: React  
**Titolo:** React: introduzione e uso del terminale  

**📍 Indice Rapido**

1. [React & Node.js: introduzione](#1-react--nodejs-introduzione)
    1.1 [Breve storia di React e Node.js](#11-breve-storia-di-react-e-nodejs)
    1.2 [Node.js: Il Motore del Back-end](#12-nodejs-il-motore-del-back-end)
    1.3 [Gestione dei Pacchetti: Perché usare `pnpm`?](#13-gestione-dei-pacchetti-perché-usare-pnpm)
2. [Workflow del Developer: Terminale e Comandi](#2-workflow-del-developer-terminale) 
    2.1 [Terminal: navigazione base](#21-terminal-navigazione-base-)
    2.2 [Terminal: pnpm e Vite](#22-terminal-pnpm-e-vite)
        2.2.1 [Comandi essenziali](#221-essenziali)
        2.2.2 [Ulteriori comandi](#222-ulteriori-comandi)
3. [Risorse Tecniche (MDN, W3Schools)](#3-risorse-e-documentazione)
4. [Key Takeaways del giorno](#4-key-takeaways-del-giorno)
5. [Glossario: termini essenziali](#5-glossario-termini-essenziali)


# 1. React & Node.js: introduzione

## 1.1 Breve storia di React e Node.js

Per capire **React** e **Node.js**, dobbiamo guardare al passato.

* **L'era del Web Statico (Anni '90 - Primi 2000):** Il web era composto da documenti HTML statici. Ogni interazione (es. cliccare un link) richiedeva al server di rigenerare e inviare una pagina intera. L'esperienza era lenta e frammentata. 🐢
* **La Rivoluzione di Node.js (2009):** Ryan Dahl portò il motore **V8** di Chrome fuori dal browser. Per la prima volta, JavaScript poteva essere usato per scrivere il back-end, permettendo agli sviluppatori di usare un unico linguaggio su tutto lo stack (Full-stack JS). 🟢
* **La Nascita di React (2013):** Creato da Jordan Walke (Facebook), React nasce per risolvere la complessità della gestione manuale del DOM. Invece di dire al browser *cosa fare* passo dopo passo (imperativo), React ti permette di descrivere *come deve apparire* l'interfaccia in base allo stato (dichiarativo). ⚛️

## 1.2 Node.js: Il Motore del Back-end

Node.js non è un linguaggio, ma un **runtime** (ambiente di esecuzione).

* **Asincrono e Event-Driven:** Grazie all'**Event Loop**, Node gestisce migliaia di connessioni simultanee senza bloccarsi. È ideale per applicazioni in tempo reale. ⚡
* **NPM / PNPM:** L'ecosistema di librerie più vasto al mondo. Qualsiasi funzionalità ti serva, esiste già un pacchetto pronto.
* **Express.js:** Il framework standard per creare API REST, i "ponti" che permettono al front-end di comunicare con il database. 🛠️
* **Vite** ***NON E' UN FRAMEWORK!!*** ma uno strumento che semplifica la creazione di un progetto di React (e non solo) e permette di fare molte cose utili, come lo scaffholding del progetto o aprire un live server.

## 1.3 Gestione dei Pacchetti: Perché usare `pnpm`?

`npm` sta per **NODE PACKAGE MANAGER**, un'applicazione che serve a gestire le dipendenze di un sito web.

`pnpm` (Performant NPM) è l'evoluzione moderna dei gestori di pacchetti, che mantiene le stesse dipendenze se sono già scaricate sul locale.

| Caratteristica | npm / yarn (Tradizionali) | pnpm (Moderno) |
| --- | --- | --- |
| **Spazio su Disco** | Copie multiple per ogni progetto. 💾 | **Content-Addressable Storage:** Una sola copia globale. ✅ |
| **Velocità** | Più lento (copia fisica dei file). | **Link Fisici:** Installazioni istantanee. 🏎️ |
| **Sicurezza** | Permette "Ghost Dependencies". | **Struttura Rigida:** Più sicuro e prevedibile. 🛡️ |

> **Nota:** Quando usi `pnpm`, viene creato un file `pnpm-lock.yaml`. Questo file garantisce che tutti i membri del team installino le stesse identiche versioni delle librerie. **Non cancellarlo mai!**

---

# 2. Workflow del Developer: Terminale

## 2.1 Terminal Navigazione Base 🧭

Alla base dell'uso del terminale, ci sono i comandi di spostamento tra le directories locali.

`root / users / nomeutente / documents`
	Come i tag HTML, i rami di directory hanno "figli" (directory interne) e "fratelli" (directory parallele).

- `ls`, `dir`	= indica quali file sono presenti nella directory presente;
    - `ls --color`	= ls che distingue file e cartelle con colori diversi;
    - `ls -la`: "Cosa c'è qui?" (Include file nascosti come `.env`).
- `pwd` 		= (print working directory) indica la directory di riferimento;
- `cd ""`		= (change directory) cambia la directory di riferimento (accetta stringhe); **ESEMPIO**	= cd "Documents";
    - `cd ..`: Torna indietro di una cartella.
- `code`        = Apri la cartella corrente in VS Code;
	
## 2.2 Terminal pnpm e Vite

### 2.2.1 Essenziali
- `pnpm `		        = accede al terminale pnpm;
- `pnpm create vite`	= crea un nuovo progetto vite;
- `pnpm install`		= installa tutte le dipendenze nel progetto;
- `pnpm dev`		    = apre il server localhost;

### 2.2.2 Ulteriori comandi

- `pnpm -v`		        = (pnpm, node o qualsiasi altro programma) indica la versione e conferma che il programma è installato;
- `volta list`		    = indica la versione di volta e node nel progetto;
- ``CNTRL+C``		    = **COMANDO** per uscire dal terminale custom;

> **Nota:** Puoi creare un nuovo comando 'pnpm' aggiungendolo all'oggetto 'scripts' all'interno di "package.json":
```
    "scripts": {
	  "comandoMio": "ls",
	}
```

## 3. React: L'Architetto del Front-end

React trasforma il modo di pensare la UI attraverso i **Componenti**.

### 3.1 Struttura base di un progetto React

- Il file `package.json` mantiene un oggetto che definisce tutte le dipendenze di un progetto.

- La cartella `src` in un progetto React contiene file con sintassi **ES6 + JSX**, quindi un misto tra ***ECMASCRIPT*** e ***JAVASCRIPT***. Le funzioni in questi file possono resituire sia JS che HTML.

- Nel file `App.js` è contenuta la struttura principale del progetto.

- Il `.gitignore` viene generato in automatico, ignorando anche il folder `node_modules` perché tutte le dipendenze sono già listate nel `package.json` e possono essere riscaricate in automatico con il comando `>pnpm install`.

* *Regola:* Le funzioni che contengono HTML in jsx devono avere un tag principale padre. Per convenzione si usa un tag generico vuoto ***"dummy tag"*** <> </>.

### 3.2 I Pilastri di React

1. **Componenti:** Pezzi di UI piccoli e riutilizzabili.
2. **JSX (JavaScript XML):** Permette di scrivere HTML dentro JavaScript.
* *Regola:* Ogni componente deve restituire un **singolo elemento radice**. Se non vuoi aggiungere un `div` inutile, usa un **Fragment** `<> ... </>`. 🧩


3. **Virtual DOM:** React aggiorna solo ciò che è cambiato, rendendo l'app fluida.
4. **Stato (State):** La "memoria" del componente. Quando lo stato cambia, React "re-renderizza" automaticamente la vista.

---

## 4. ES6 Modules: Import & Export

In React, ogni file è un modulo isolato. Per condividere codice, usiamo `import` ed `export`.

- Tramite `export` posso creare variabili globali da poter usare in tutto il progetto.
- All'interno del documento che mi serve, con `import` importo la variabile globale nel documento.

### Default Export vs Named Export

| Tipo | Sintassi Export | Sintassi Import | Quando usarlo? |
| --- | --- | --- | --- |
| **Default** | `export default App` | `import App from './App'` | Per il componente principale del file. |
| **Named** | `export const User = ...` | `import { User } from './file'` | Per utility, costanti o export multipli. |

---

## 5. Best Practices e Tips

* **React StrictMode:** `<StrictMode>` in fase di sviluppo renderizza i componenti due volte. Serve a scovare bug e "effetti collaterali" indesiderati. Non preoccuparti se vedi doppi log in console! 🔍
* **Naming Convention:** I componenti React devono sempre iniziare con la **lettera maiuscola** (es. `Header`, non `header`).
* **JSX vs HTML:** In JSX usa `className` invece di `class` e `htmlFor` invece di `for`.
* **Scripts personalizzati:** Nel `package.json` puoi creare alias. Se aggiungi `"pippo": "ls -la"`, potrai eseguirlo con `pnpm pippo`. ⚡

---