# 6. Data Fetching


### 6.1 Data Fetching Avanzato e Ciclo di Vita (`useEffect`)

Quando un’app React deve comunicare con un’API esterna, entra in gioco una logica asincrona (cioè che non avviene subito, ma “in futuro”). Per gestire correttamente questa situazione si separano due responsabilità:

* **Utility** (logica di rete) → si occupa di prendere i dati
* **Componente** (UI) → si occupa di mostrarli

Questa separazione è importante perché evita codice confuso e facilita il riutilizzo delle funzioni.

#### 6.1.1 La Logica di Rete: `src/utils/fakeStore.js`
La funzione `fetch()` serve per fare richieste HTTP verso un server. Il suo comportamento però è spesso frainteso:
⚠️ `fetch()` NON considera automaticamente gli errori HTTP (come 404 o 500) come errori “veri”. Per fetch, un errore è solo quando la richiesta NON arriva proprio al server (es. internet offline).

Quindi:
* ❌ 404 -> NON va nel `.catch()`
* ❌ 500 -> NON va nel `.catch()`
* ✔️ offline -> va nel `.catch()`

Per questo dobbiamo controllare manualmente la risposta.

```js
const API_URL = '[https://fakestoreapi.com/products](https://fakestoreapi.com/products)';

export function fetchFakeStoreData() {
  return fetch(API_URL)
    .then((response) => {
      // Validazione manuale degli stati di errore HTTP
      if (response.status === 404) {
        throw new Error('Pagina non trovata'); // Interrompe il flusso e spara l'errore al .catch()
      }
      return response.json(); // Trasforma lo stream grezzo in un oggetto JS leggibile
    })
    .then((json) => json);
}
```

- **`throw new Error()`:** Funziona come un pulsante di emergenza. Interrompe immediatamente il codice e salta direttamente al blocco .catch() che gestirà l'errore.
    
- **`response.json()`:** Anche questa operazione richiede del tempo (restituisce una Promise), perché traduce la stringa di testo ricevuta dal server in array o oggetti che JavaScript può manipolare.
#### 6.1.2 La Gestione dello Stato nel Componente: `ProductList.jsx`

In React i dati dinamici si gestiscono con `useState`, mentre le operazioni esterne (come fetch) si fanno dentro `useEffect`.

```js
const [products, setProducts] = useState([]);
const [errorMsg, setErrorMsg] = useState('');
```

Un ottimo trucco per evitare che l'applicazione si rompa mentre aspetta i dati è inizializzare lo stato con lo stesso tipo di dato che ci aspettiamo. Se l'API ci manderà una lista (un array) di prodotti, noi partiamo con un array vuoto `[]`.

#### 6.1.3 Avviare la Richiesta con `useEffect`

Per attivare la richiesta nel momento in cui la pagina si carica, usiamo l'hook `useEffect`.

```js
import { useState, useEffect } from 'react';
import { fetchFakeStoreData } from '../utils/fakeStore';

function ProductList() {
  // Inizializziamo l'array vuoto per i prodotti e una stringa vuota per l'errore
  const [products, setProducts] = useState([]);
  const [errorMsg, setErrorMsg] = useState(''); 

  useEffect(() => {
    // Chiamiamo la funzione che abbiamo creato nel file fakeStore.js
    fetchFakeStoreData()
      .then((data) => {
        // Se tutto va a buon fine, salviamo i dati nello stato
        setProducts(data); 
      })
      .catch((error) => {
        // Se qualcosa è andato storto (inter internet non c'è o abbiamo lanciato il 404 manuale)
        // entriamo in questo blocco .catch()
        if (error.message === 'Pagina non trovata') {
          setErrorMsg(error.message); // Mostriamo il messaggio specifico
        } else {
          setErrorMsg('Errore generico di caricamento'); // Errore di riserva se il problema è un altro
        }
      });
  }, []); // ⚠️ L'array di dipendenze vuoto [] dice a React di eseguire questo codice UNA SOLA VOLTA, 
          // precisamente quando il componente viene mostrato sullo schermo per la prima volta.

  return (
    <div>
      {/* Se la stringa dell'errore non è vuota, mostra il messaggio sullo schermo */}
      {errorMsg && <h1>❌ Errore: {errorMsg}</h1>}
      
      {/* Mostriamo i prodotti formattati in modo grezzo sullo schermo */}
      <pre>{JSON.stringify(products, null, 2)}</pre>
    </div>
  );
}

export default ProductList;
```

#### 6.1.4 Il Rischio del Loop Infinito

Ogni volta che aggiorni lo stato di un componente (usando `setProducts`), React per funzionare deve eseguire di nuovo da capo tutto il codice di quel componente (operazione chiamata **re-render**).

Se inserisci la `fetch` libera nel corpo del componente, crei una reazione a catena distruttiva:

1. Il componente viene eseguito ed esegue la `fetch`.
    
2. La `fetch` ottiene i dati e aggiorna lo stato con `setProducts`.
    
3. L'aggiornamento dello stato costringe React a rieseguire il componente.
    
4. Il componente viene rieseguito e... ritrova la `fetch`, facendola ripartire.
    

Si torna al punto 2 e il ciclo si ripetete all'infinito, bloccando l'applicazione e bombardando il server di richieste.

#### Come Evitare il Loop Infinito

L'hook `useEffect` con l'array di dipendenze vuoto `[]` serve proprio a evitare questo:

> Dice a React di eseguire la `fetch` una volta sola quando il componente appare sullo schermo (fase di montaggio).

In questo modo il ciclo infinito viene bloccato nei successivi re-render.

#### **🔗 Risorse e Documentazione**

- 📚 **MDN Web Docs:** [Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
    
- ⚛️ **React Documentation:** [Synchronizing with Effects (`useEffect`)](https://react.dev/reference/react/useEffect)
    

#### **🚀 Key Takeaways del Giorno**

_I punti fondamentali da portarsi a casa_

- **Gestione Errori manuale:** `fetch()` non rigetta le Promise in caso di errori HTTP come 404 o 500; è obbligatorio controllare `response.status` e lanciare un errore manuale tramite `throw new Error()`.
    
- **Prevenzione dei Loop:** Eseguire modifiche di stato direttamente nel corpo di un componente genera re-render infiniti. L'uso di `useEffect` con array di dipendenze vuoto `[]` fissa l'esecuzione al solo montaggio iniziale.
    
- **Inizializzazione dello Stato:** Partire sempre con un tipo di dato coerente con quello atteso (es. `[]` per le liste) evita crash dell'applicazione prima dell'arrivo della risposta asincrona.
    

#### **📖 Glossario**

| **Termine Istituzionale** | **Definizione Formale**                                                                                                                                  | **"Spiega Brutta"**                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Data Fetching**         | Processo asincrono di richiesta e acquisizione di dati da una sorgente o API esterna verso l'applicazione.                                               | Andare a bussare al server per farsi dare le informazioni che mancano alla pagina.                       |
| **Re-render**             | Ciclo di riesecuzione del codice di un componente da parte di React a seguito di una variazione di stato o di prop.                                      | React che ricalcola da capo tutta la pagina perché è cambiato qualcosa e deve aggiornare la vista.       |
| **Asincronia**            | Modello di esecuzione in cui un'operazione viene avviata senza bloccare il flusso principale del programma, gestendo il risultato in un secondo momento. | Chiedere una cosa che richiede tempo e continuare a fare altro mentre si aspetta che arrivi la risposta. |
