
# 🚀 Guida Completa: Lo Stack Moderno con Node.js e React

Benvenuto nel mondo dello sviluppo web moderno. Questi appunti coprono l'intero ecosistema: dalla storia delle tecnologie agli strumenti di sviluppo e alla sintassi di base.

## 📜 1. L'Evoluzione del Web: Perché siamo qui?

Per capire **React** e **Node.js**, dobbiamo guardare al passato.

* **L'era del Web Statico (Anni '90 - Primi 2000):** Il web era composto da documenti HTML statici. Ogni interazione (es. cliccare un link) richiedeva al server di rigenerare e inviare una pagina intera. L'esperienza era lenta e frammentata. 🐢
* **La Rivoluzione di Node.js (2009):** Ryan Dahl portò il motore **V8** di Chrome fuori dal browser. Per la prima volta, JavaScript poteva essere usato per scrivere il back-end, permettendo agli sviluppatori di usare un unico linguaggio su tutto lo stack (Full-stack JS). 🟢
* **La Nascita di React (2013):** Creato da Jordan Walke (Facebook), React nasce per risolvere la complessità della gestione manuale del DOM. Invece di dire al browser *cosa fare* passo dopo passo (imperativo), React ti permette di descrivere *come deve apparire* l'interfaccia in base allo stato (dichiarativo). ⚛️

---

## ⚙️ 2. Node.js: Il Motore del Back-end

Node.js non è un linguaggio, ma un **runtime** (ambiente di esecuzione).

* **Asincrono e Event-Driven:** Grazie all'**Event Loop**, Node gestisce migliaia di connessioni simultanee senza bloccarsi. È ideale per applicazioni in tempo reale. ⚡
* **NPM / PNPM:** L'ecosistema di librerie più vasto al mondo. Qualsiasi funzionalità ti serva, esiste già un pacchetto pronto.
* **Express.js:** Il framework standard per creare API REST, i "ponti" che permettono al front-end di comunicare con il database. 🛠️

---

## 📦 3. Gestione dei Pacchetti: Perché usare `pnpm`?

`pnpm` (Performant NPM) è l'evoluzione moderna dei gestori di pacchetti.

| Caratteristica | npm / yarn (Tradizionali) | pnpm (Moderno) |
| --- | --- | --- |
| **Spazio su Disco** | Copie multiple per ogni progetto. 💾 | **Content-Addressable Storage:** Una sola copia globale. ✅ |
| **Velocità** | Più lento (copia fisica dei file). | **Link Fisici:** Installazioni istantanee. 🏎️ |
| **Sicurezza** | Permette "Ghost Dependencies". | **Struttura Rigida:** Più sicuro e prevedibile. 🛡️ |

> **Nota:** Quando usi `pnpm`, viene creato un file `pnpm-lock.yaml`. Questo file garantisce che tutti i membri del team installino le stesse identiche versioni delle librerie. **Non cancellarlo mai!**

---

## 🛠️ 4. Workflow del Developer: Terminale e Comandi

### Navigazione Base 🧭

* `pwd`: "Dove sono?" (Mostra il percorso attuale).
* `ls -la`: "Cosa c'è qui?" (Include file nascosti come `.env`).
* `cd ..`: Torna indietro di una cartella.
* `code .`: Apri la cartella corrente in VS Code.

### Iniziare un Progetto con Vite ⚡

**Vite** è lo strumento standard moderno per creare app React velocemente.

1. `pnpm create vite`: Avvia l'assistente (scegli React > JavaScript).
2. `cd nome-progetto`: Entra nella cartella.
3. `pnpm install`: Scarica le dipendenze (crea la cartella `node_modules`).
4. `pnpm dev`: Avvia il server di sviluppo locale.

---

## ⚛️ 5. React: L'Architetto del Front-end

React trasforma il modo di pensare la UI attraverso i **Componenti**.

### I Pilastri di React

1. **Componenti:** Pezzi di UI piccoli e riutilizzabili.
2. **JSX (JavaScript XML):** Permette di scrivere HTML dentro JavaScript.
* *Regola:* Ogni componente deve restituire un **singolo elemento radice**. Se non vuoi aggiungere un `div` inutile, usa un **Fragment** `<> ... </>`. 🧩


3. **Virtual DOM:** React aggiorna solo ciò che è cambiato, rendendo l'app fluida.
4. **Stato (State):** La "memoria" del componente. Quando lo stato cambia, React "re-renderizza" automaticamente la vista.

---

## 🧬 6. ES6 Modules: Import & Export

In React, ogni file è un modulo isolato. Per condividere codice, usiamo `import` ed `export`.

### Default Export vs Named Export

| Tipo | Sintassi Export | Sintassi Import | Quando usarlo? |
| --- | --- | --- | --- |
| **Default** | `export default App` | `import App from './App'` | Per il componente principale del file. |
| **Named** | `export const User = ...` | `import { User } from './file'` | Per utility, costanti o export multipli. |

---

## 💡 7. Best Practices e Tips

* **React StrictMode:** `<StrictMode>` in fase di sviluppo renderizza i componenti due volte. Serve a scovare bug e "effetti collaterali" indesiderati. Non preoccuparti se vedi doppi log in console! 🔍
* **Naming Convention:** I componenti React devono sempre iniziare con la **lettera maiuscola** (es. `Header`, non `header`).
* **JSX vs HTML:** In JSX usa `className` invece di `class` e `htmlFor` invece di `for`.
* **Scripts personalizzati:** Nel `package.json` puoi creare alias. Se aggiungi `"pippo": "ls -la"`, potrai eseguirlo con `pnpm pippo`. ⚡

---

### 🗺️ Roadmap Consigliata

1. **JS Moderno (ES6+):** Arrow functions, Destructuring, Map/Filter, Async/Await.
2. **Node Basics:** Creare piccole API con Express.
3. **React Fundamentals:** Hooks (`useState`, `useEffect`).
4. **Integrazione:** Collegare il front-end (React) al back-end (Node) tramite `fetch`.

---


////////////////////////////////////////////////////////////////////////////////////////


#### Modulo: React  
**Titolo:** React: introduzione e uso del terminale  

**📍 Indice Rapido**

1. [Breve storia di React](#1-breve-storia-di-react)
2. [Esempi pratici](#2-esempi-pratici)  
3. [Risorse Tecniche (MDN, W3Schools)](#3-risorse-e-documentazione)
4. [Key Takeaways del giorno](#4-key-takeaways-del-giorno)
5. [Glossario: termini essenziali](#5-glossario-termini-essenziali)


### 1. Breve storia di React

### Breve storia di React

useState è un Hook fornito da React (a partire dalla v16.8) che permette di dichiarare variabili di stato all'interno di un componente funzionale.  
Lo stato memorizza i dati che determinano cosa mostra la UI e rende il componente "reattivo": quando il valore di stato cambia tramite il setter, React pianifica un nuovo render per aggiornare la UI.

Perché è necessario:  
- UI reattiva: sincronizza l'interfaccia con i dati.  
- Conservazione dello stato tra render.  
- Gestione di input, visibilità, conteggi, dati caricati, ecc.

Differenza con variabili normali: aggiornare una variabile JS non aggiorna la UI; usare useState assicura che React riesegua il render e applichi le modifiche al DOM, aggiornando ciò che l'utente vede sullo schermo.

### Sintassi e inizializzazione

Per usare useState, è necessario importarlo da React. 
Importazione:
```js
import { useState } from 'react';
```

La funzione si invoca passandole come argomento il valore iniziale che vogliamo assegnare alla variabile. 
La funzione restituisce sempre un array di due elementi, quindi per comodità e leggibilità, si utilizza il destructuring:

```js
const [count, setCount] = useState(0);
```
Il primo elemento è il valore corrente e il secondo è la funzione setter per aggiornarlo. 
Per convenzione, se la variabile è nome, il setter sarà setNome.

Tipi gestiti: useState può memorizzare primitivi (number, string, boolean), oggetti e array; attenzione ai riferimenti quando usi strutture complesse.

### Lazy Initialization

Per evitare calcoli costosi a ogni render, passa una funzione inizializzatrice a useState; la funzione verrà eseguita solo al primo render.  

Esempio:
```js
const [data, setData] = useState(() => heavyComputation());
```

### Meccanismo del re-render

Il re-render è il processo con cui React aggiorna l'interfaccia. Viene innescato (triggerato) ogni volta che la funzione setter viene chiamata con un nuovo valore, assicurando che la UI sia sincronizzata con i dati memorizzati nello stato. 

React mantiene internamente lo stato associato a ogni componente e, quando chiami il setter, pianifica un nuovo render in cui la funzione del componente viene rieseguita e viene creato un nuovo Virtual DOM che React confronta con il precedente per aggiornare il DOM reale.  
React usa un confronto superficiale (shallow equality) per decidere se aggiornare; per oggetti/array conta il riferimento, non il contenuto profondo.

Nota importante: durante l'esecuzione di un singolo render la variabile di stato è immutabile (comportamento simile a const) e chiamare il setter prenota il nuovo valore per il render successivo, non lo aggiorna immediatamente.

### Setter e immutabilità

Il setter restituito da useState è l'unico metodo corretto per aggiornare lo stato e notificare React.  
Non mutare direttamente lo stato (specialmente oggetti/array): React confronta i riferimenti di memoria. Se modifichi una proprietà di un oggetto esistente, il riferimento rimane uguale e React potrebbe non rilevare il cambiamento, quindi non aggiorna la UI.

Esempio sbagliato:
```js
const [user, setUser] = useState({ name: "Mario" });
// ❌ ERRORE
user.name = "Luca";
```
Esempio corretto:
```js
setUser(prev => ({ ...prev, name: "Luca" }));
```
Bisogna sempre creare una copia (es. tramite l'operatore spread ...) e sovrascrivere solo ciò che serve. 

### Functional updates

Quando il nuovo valore dipende dallo stato precedente, usa la forma a callback del setter. Questo garantisce che l'aggiornamento usi il valore più aggiornato anche con batching di React.  
Senza la callback, più chiamate consecutive basate su una closure potrebbero usare un valore "vecchio" e produrre risultati inattesi.

// ✅ CORRETTO
```
setCount(prev => prev + 1)
```

// ❌ ERRORE
```
setCount(count + 1);
```
Chiamare il setter non aggiorna immediatamente la variabile: prenota solo il nuovo valore per il prossimo render. Conseguenza: tre setCounter(counter + 1) consecutivi incrementano di 1, non di 3.

Batching: React può raggruppare più aggiornamenti nello stesso evento per performance. Senza la callback, aggiornamenti multipli consecutivi potrebbero basarsi su un valore "vecchio", portando a bug.

Stato asincrono: subito dopo la chiamata a `setState`, la variabile di stato nel contesto corrente non è ancora aggiornata; il nuovo valore sarà visibile solo al prossimo render.

### Chiavi dinamiche e form handling

Per gestire più input con un singolo handler usa chiavi dinamiche e functional update:

```js
const [formData, setFormData] = useState({ username: "", email: "" });

function handleChange(event) {
  const { name, value } = event.target;
  setFormData(prev => ({ ...prev, [name]: value }));
}
```
Questo permette a un solo handler di aggiornare campi diversi basandosi su `event.target.name`.  

Nota Bene: Ricordati di usare `event.preventDefault()` su submit per evitare il reload che resettarebbe lo stato del componente.

### Controlled components e state lifting

Controlled components: un input è controllato quando il suo valore è legato allo stato di React tramite `value` e `onChange`; questo rende React la *single source of truth* per quel dato.  

State lifting: se più componenti figli devono condividere uno stato, solleva lo stato al loro genitore comune e trasferiscilo via props; lo stato dovrebbe vivere nel componente più vicino che ne ha bisogno.

### Errori comuni e best practice

Errori comuni:
- Chiamare useState condizionalmente (es. dentro if o loop), gli hook devono essere invocati nello stesso ordine ad ogni render.  
- Mutare direttamente oggetti/array in stato.  
- Dimenticare `event.preventDefault()` nei form.  
- Mettere nello stato valori derivabili da props (stato derivato non necessario).

Best practice:
- Naming: gli hook custom iniziano con "use" e i componenti con lettere maiuscole.  
- Tratta lo stato come immutabile; usa spread, filter e map per aggiornare dati complessi.  
- Usa functional updates quando il nuovo stato dipende dal precedente.  
- Usa lazy initialization per costose operazioni iniziali.

### 2. Esempi pratici

1) Contatore semplice:
```js
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  function increment() {
    setCount(prev => prev + 1);
  }

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+1</button>
    </div>
  );
}
```

2) Form controllato con chiavi dinamiche:
```js
function SignupForm() {
  const [formData, setFormData] = useState({ username: '', email: '' });

  function handleChange(e) {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  }

  function handleSubmit(e) {
    e.preventDefault();
    // invia formData
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" name="username" value={formData.username} onChange={handleChange} />
      <input type="email" name="email" value={formData.email} onChange={handleChange} />
      <button type="submit">Invia</button>
    </form>
  );
}
```

3) Lazy init con calcolo pesante:
```js
const [items, setItems] = useState(() => heavyComputation());
```

### 3. Risorse e Documentazione

• [w3Schools](https://www.w3schools.com/react/react_usestate.asp)  
• [MDM_](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/React_interactivity_events_state) 

### 4. Key Takeaways del giorno

- useState aggiunge stato ai componenti funzionali e rende la UI reattiva quando chiami il setter.  
- Non mutare lo stato direttamente; crea copie per aggiornare oggetti/array.  
- Usa functional updates se il nuovo stato dipende dal precedente per evitare bug dovuti a batching.  
- La lazy initialization evita calcoli costosi ad ogni render, eseguendo l'inizializzatore solo al primo render.  
- Gli hook devono essere invocati nello stesso ordine ad ogni render; evita chiamate condizionali.

### 5. Glossario: termini essenziali

| Termine | Definizione Formale | "Spiega Brutta" |
| --- | --- | --- |
| useState | Hook di React per aggiungere stato ai componenti funzionali. | Variabile che mantiene i dati tra i render. |
| Setter | Funzione restituita da useState per aggiornare lo stato. | Il pulsante che dice a React "aggiorna tutto". |
| Functional update | Modalità di chiamata del setter con callback (prev => ...). | Incremento sicuro che usa l'ultimo valore possibile. |
| Shallow equality | Confronto superficiale dei valori usato da React per ottimizzare i re-render. | React guarda solo il riferimento per oggetti/array, non il contenuto. |
| Controlled component | Input il cui valore è gestito dallo stato React. | L'input che obbedisce a React invece di fare come gli pare. |

---

Checklist dello Studente  
[ ] Hai importato `{ useState }`?  
[ ] Usi la destrutturazione `[state, setState]`?  
[ ] Usi lo spread operator per oggetti e array (evitando mutazioni dirette)?  
[ ] Usi il functional update se il nuovo valore dipende dal precedente?  
[ ] Hai gestito `preventDefault()` nei form per evitare il reload della pagina (che resetterebbe lo stato)?  
[ ] Hai evitato di mettere nello stato dati che possono essere calcolati da altre props (stato derivato)?


///////////////////////////////////////////////

# 24-04-26
# APPUNTI FLASH: NODE JS e TERMINALE

## PACKAGE MANAGER

NPM sta per NODE PACKAGE MANAGER, serve a gestire le dipendenze di un sito web.
PNPM è un altro gestore di dipendenze, che mantiene le stesse dipendenze se sono già scaricate sul locale. (Noi usiamo questo)

## PACKAGE.JSON

File che mantiene un oggetto che definisce tutte le dipendenze di un sito.

## VITE

NON E' UN FRAMEWORK!!
E' uno strumento che semplifica la creazione di un progetto di React (e non solo).
Tra le altre cose, permette di fare scaffholding del progetto e aprire un live server.

## DIRECTORY

root / users / nomeutente / documents
	Come i tag HTML, i rami di directory hanno "figli" (directory interne) e "fratelli" (directory parallele).

## COMANDI TERMINALE

- ls / dir	= dice quali file sono presenti nella directory presente;
- pwd 		= (print working directory) indica la directory di riferimento;
- cd ""		= (change directory) cambia la directory di riferimento; 		ACCETTA STRINGA CON NOME DIRECTORY
	ESEMPIO	= cd "Documents"
- ls --color	= ls che distingue file e cartelle con colori diversi;

## COMANDI TERMINALE PNPM e vari

- pnpm 			= accede al terminale pnpm;
- pnpm create vite	= crea un nuovo progetto vite;					FUNZIONE CON DUE PARAMETRI
- pnpm install		= installa tutte le dipendenze nel progetto;
- pnpm dev		= apre il server localhost;

- pnpm -v		= (pnpm, node o qualsiasi altro programma) indica la versione e conferma che il programma è installato;
- volta list		= indica la versione di volta e node nel progetto;

- CNTRL+C		COMANDO per uscire da terminali custom;

Puoi creare un nuovo comando 'pnpm' aggiungendolo all'oggetto 'scripts' all'interno di "package.json":
	"scripts": {
	  "comandoMio": "ls",
	}

## GITIGNORE

Il gitignore viene generato in automatico, ignorando anche il folder "node_modules" perché tutte le dipendenze sono già listate nel "package.json" e possono essere riscaricate in automatico con il comando "pnpm install".

## SRC

Contiene file con sintassi ES6 + JSX, quindi un misto tra ECMASCRIPT e JAVASCRIPT. Le funzioni in questi file possono resituire sia JS che HTML.

Le funzioni che contengono HTML in jsx devono avere un tag principale come regola. Per convenzione si usa un tag generico vuoto "dummy tag" <> </>.

Nel file App.js è contenuta la struttura principale.
Tramite 'export' posso creare variabili globali da poter usare in tutto il progetto.
All'interno del documento che mi serve, come in "main.jsx", con 'import' importo la variabile globale nel documento.
