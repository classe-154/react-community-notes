#### Modulo: React  
**Titolo:** Guida Completa a useState  
**Data:** 16/05/2026

**📍 Indice Rapido**

1. uso di useState, concetti fondamentali  
1.1 [Introduzione e perché serve](#introduzione-e-perché-serve)  
1.2 [Sintassi e inizializzazione](#sintassi-e-inizializzazione)  
1.3 [Lazy initialization](#lazy-initialization)  
1.4 [Meccanismo del re-render](#meccanismo-del-re-render)  
1.5 [Setter e immutabilità](#setter-e-immutabilità)  
1.6 [Functional updates](#functional-updates)  
1.7 [Chiavi dinamiche e form handling](#chiavi-dinamiche-e-form-handling)  
1.8 [Controlled components e state lifting](#controlled-components-e-state-lifting)  
1.9 [Errori comuni e best practice](#errori-comuni-e-best-practice)  
2. Esempi pratici  
3. Risorse Tecniche (MDN, W3Schools)  
4. Key Takeaways del giorno  
5. Glossario: termini essenziali


### 1. useState

1.1 Introduzione e perché serve

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