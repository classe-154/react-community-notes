# Forms & POST

**Titolo:** Guida Completa a Forms & POST 

**📍 Indice Rapido**

1. [Le Forms in React](#1-forms)
   - 1.1 [Guida al Data Binding e Gestione Form in React](#11-guida-al-data-binding-e-gestione-form-in-react)
        * 1.1.i [Eventi di Input: Vanilla JS vs Il "Grande Inganno" di React](#11i-eventi-di-input-vanilla-js-vs-il-grande-inganno-di-react)
        * 1.1.ii [Il Meccanismo del Two-Way Data Binding (Controlled Components)](#11ii-il-meccanismo-del-two-way-data-binding-controlled-components)
        * 1.1.iii [Vari tipi di input e come gestire il loro binding](#11iii-vari-tipi-di-input-e-come-gestire-il-loro-data-binding)
   - 1.2 [Gestione dello Stato Avanzata: Oggetti e Handler Generici](#12-gestione-dello-stato-avanzata-oggetti-e-handler-generici)
   - 1.3 [Gestione del Submit e Reset del Form](#13-gestione-del-submit-e-reset-del-form)

2. [Mandare i dati con POST](#2-mandare-i-dati-con-post)
   - 2.1 [Comporre una richiesta di POST](#21-comporre-una-richiesta-di-post)  
   - 2.2 [Custom Hook per fare la POST](#22-custom-hook-per-fare-la-post)  

3. [Risorse Tecniche](#-3-risorse-tecniche)
4. [Key Takeaways del giorno](#-4-key-takeaways)
5. [Glossario: termini essenziali](#-5-syllabus)

# 1. Forms

In React, essendo una libreria basata sugli `stati`, potremmo voler legare i valori degli input nelle nostre `form` a degli stati.


# 1.1 Guida al Data Binding e Gestione Form in React

Nel modello dichiarativo di React, i dati guidano l'interfaccia utente. Il **Data Binding** è il meccanismo cardine che collega la logica JavaScript (lo Stato) agli elementi visivi del browser (la View).

---

## 1.1.i Eventi di Input: Vanilla JS vs Il "Grande Inganno" di React

Per comprendere come React gestisce l'inserimento dei dati, dobbiamo prima chiarire come si comportano gli input nel browser nativo e come React ha modificato queste regole per semplificarci la vita.

### L'Evento `input` vs `change` in Vanilla JS

In JavaScript puro, questi due eventi intercettano i dati in momenti completamente diversi:

* **`input` (Tempo Reale ⏱️):** Si attiva a ogni singolo carattere digitato. Se scrivi *"Ciao"*, si attiva 4 volte. È ideale per controllare la lunghezza di una password mentre l'utente scrive.
* **`change` (A Operazione Conclusa 🏁):** Si attiva solo quando l'utente conferma la modifica, ovvero quando toglie il focus dal campo di testo (evento `blur`) o preme Invio. Se scrivi *"Ciao"* e clicchi altrove, scatta una sola volta.

### ⚠️ Il comportamento di `onChange` in React

Ecco il dettaglio fondamentale che spesso trae in inganno chi proviene da JavaScript puro: **in React, l'evento `onChange` non si comporta come il `change` nativo, ma funziona esattamente come l'evento `input`!**

| Caratteristica | Vanilla JS `change` | React `onChange` |
| --- | --- | --- |
| **Frequenza di attivazione** | Bassa (una volta al termine, al `blur`). | Altissima (a ogni singolo carattere digitato). |
| **Comportamento effettivo** | Aspetta la fine della digitazione. | Si comporta come l'evento nativo `input`. |

> 🚩 **Perché questo è pericoloso?** Se inserisci una chiamata API direttamente dentro un handler `onChange` in React pensando che si attivi solo alla fine, scatenerai una richiesta di rete verso il server per ogni singola lettera digitata!
> 💡 **La Regola d'Oro:** Se in React hai bisogno del comportamento del vero `change` nativo (attivarsi solo quando l'utente sposta il focus altrove), devi usare l'evento **`onBlur`**.

### 🎯 Il ruolo di `event.target.value`

Quando viene catturato un evento di input, la funzione riceve automaticamente l'oggetto `event`.

* `event.target`: Rappresenta il riferimento all'elemento HTML effettivo del DOM (il tag `<input>`).
* `event.target.value`: È la proprietà stringa contenente il testo esatto presente in quel preciso istante dentro il campo.

```javascript
// Struttura standard in JavaScript puro
myInputEl.addEventListener('input', (event) => {
    console.log(event.target.value); // Stampa il testo aggiornato in tempo reale
});

```

---

## 1.1.ii Il Meccanismo del Two-Way Data Binding (Controlled Components)

In React, l'approccio standard prevede che gli input del form siano **Controlled Components** (Componenti Controllati). Questo significa che l'input non gestisce più il proprio testo internamente, ma è sincronizzato con lo stato di React attraverso un **doppio legame**.

```jsx
import { useState } from "react";

function ControlledInput() {
  const [task, setTask] = useState("");

  const changeTaskHandler = (event) => {
    setTask(event.target.value); // 2. Aggiorna lo stato (UI ➡️ Model)
  };

  // 1. Il valore dell'input è forzato dallo stato (Model ➡️ UI)
  return <input type="text" value={task} onChange={changeTaskHandler} />;
}

```

### 🛠️ Come funziona il "Doppio Legame" in 2 Passaggi:

1. **Dallo Stato alla UI (`Model ➡️ View`):** L'attributo `value={task}` blocca l'input. Il campo mostrerà sempre e solo quello che è scritto nella variabile di stato. Senza una funzione che modifica lo stato, l'input risulterebbe congelato.
2. **Dalla UI allo Stato (`View ➡️ Model`):** L'evento `onChange` intercetta la digitazione dell'utente, estrae il valore con `event.target.value` e aggiorna lo stato tramite la funzione setter (`setTask`). Questo provoca un re-render immediato dell'input con il nuovo carattere visualizzato.

> ✨ **Il Vantaggio del Reset Immediato:** Questa sincronizzazione rende operazioni come lo svuotamento dei form estremamente semplici. Per ripulire un campo di testo dopo un'operazione, basta richiamare il setter impostando una stringa vuota: `setTask('')`. L'input si svuoterà automaticamente.

---
## 1.1.iii Vari tipi di input e come gestire il loro data binding
### 1 Input text, Textarea, Select
Tutti e tre usano lo stesso meccanismo: `value` + `onChange`.
```jsx
{/* Input text */}
<input
  type="text"
  name="nome"           // <- corrisponde alla chiave in formData
  value={formData.nome} // <- two-way binding
  onChange={handleChange}
/>

{/* Textarea - in React usa value, non il contenuto tra i tag */}
<textarea
  name="descrizione"
  value={formData.descrizione}
  onChange={handleChange}
/>

{/* Select - value sull'elemento <select>, non sulle <option> */}
<select
  name="razza"
  value={formData.razza}
  onChange={handleChange}
>
  <option value="" disabled>Seleziona...</option>
  <option value="elf">Elfo</option>
</select>
```

### 2 Checkbox
La checkbox è diversa dagli altri input: non ci interessa il suo `value` (che è una stringa statica), ma il suo `checked` - un booleano che dice se è spuntata o no.

Dobbiamo quindi estendere `handleChange` per gestire i due casi:
```jsx
const handleChange = (event) => {
  const target  = event.target;
  const tagType = target.type;    // "text", "checkbox", ecc.
  const name    = target.name;
  const value   = target.value;   // per text/textarea/select/radio
  const checked = target.checked; // per checkbox (true/false)

  // Scegliamo il valore corretto in base al tipo di input
  const valueToUpdate = tagType === "checkbox" ? checked : value;

  setFormData({ ...formData, [name]: valueToUpdate });
};
```
Nel JSX la checkbox usa `checked` invece di `value` per il binding:
```jsx
<input
  type="checkbox"
  name="marketing"
  checked={formData.marketing}  // <- boolean, non value
  onChange={handleChange}
/>
```
### 3 Radio button
I radio button condividono tutti lo stesso `name` e si escludono a vicenda (ne può essere selezionato solo uno per gruppo).

In React, il radio "attivo" è determinato da un confronto tra lo stato e il `value` del singolo radio:
```jsx
<input
  type="radio"
  name="sesso"
  value="uomo"
  checked={formData.sesso === "uomo"}  // true solo se lo stato è "uomo"
  onChange={handleChange}
/>
<input
  type="radio"
  name="sesso"
  value="donna"
  checked={formData.sesso === "donna"} // true solo se lo stato è "donna"
  onChange={handleChange}
/>
```
Quando l'utente clicca un radio, `handleChange` salva il suo `value` nello stato. React aggiorna entrambi i `checked` di conseguenza, garantendo la mutua esclusività.

La `handleChange` dell'Esempio 2 funziona invariata per i radio, perché i radio restituiscono un `value` (stringa), non un `checked` booleano.



---

## 1.2 Gestione dello Stato Avanzata: Oggetti e Handler Generici

Quando un form inizia ad avere molti campi (es. Nome, Cognome, Indirizzo), creare un `useState` per ogni singola stringa rende il codice ripetitivo. La soluzione ideale è raggruppare i dati in un **unico oggetto di stato**.

Per aggiornare questo oggetto in modo efficiente, si utilizza un singolo **Handler Generico** basato sulle proprietà calcolate di ES6 e sull'attributo `name` dei tag HTML.

```jsx
import { useState } from "react";

function FormRegistrazione() {
  // Unico stato strutturato come oggetto
  const [user, setUser] = useState({
    name: "",
    surname: "",
    email: ""
  });

  // --- HANDLER GENERICO ---
  const changeGenericHandler = (event) => {
    const { name, value } = event.target;

    // Sfruttiamo l'immutabilità e le parentesi quadre per le chiavi dinamiche
    setUser({
      ...user,        // 1. Copia profonda di tutte le proprietà attuali
      [name]: value   // 2. Sovrascrive o aggiunge dinamicamente solo la proprietà modificata
    });
  };

  return (
    <form>
      {/* L'attributo name deve corrispondere ESATTAMENTE alla chiave dell'oggetto nello stato */}
      <input name="name" value={user.name} onChange={changeGenericHandler} placeholder="Nome" />
      <input name="surname" value={user.surname} onChange={changeGenericHandler} placeholder="Cognome" />
      <input name="email" value={user.email} onChange={changeGenericHandler} placeholder="Email" />
    </form>
  );
}

```

### 💡 Focus Tecnico: Le Parentesi Quadre `[name]` e l'Immutabilità

* **Sintassi delle Proprietà Calcolate:** Se scrivessimo `{ name: value }`, JavaScript creerebbe una chiave letterale chiamata `"name"`. Usando le parentesi quadre `[name]`, diciamo all'interprete di valutare il contenuto della variabile. Se l'utente sta digitando nell'input con `name="surname"`, la riga si tradurrà automaticamente in `surname: value`.
* **L'importanza dello Spread Operator (`...user`):** Gli oggetti in React sono immutabili. Non possiamo fare `user.name = "Luca"`, perché React non rileverebbe il cambio di riferimento in memoria e non avvierebbe il re-render. Lo spread operator crea una copia dell'oggetto. Senza di esso, ogni volta che scriviamo nel campo cognome cancelleremmo le proprietà del nome e dell'email.

> 🕵️‍♂️ **Strumento di Debug Rapido:** Per controllare la salute dello stato di un oggetto in tempo reale direttamente sullo schermo durante lo sviluppo, puoi stamparlo nel JSX usando la stringa serializzata: `<pre>{JSON.stringify(user, null, 2)}</pre>`.

---

## 1.3 Gestione del Submit e Reset del Form

Nelle applicazioni Single Page (SPA) per cui React è nato, la sottomissione di un form non deve seguire le regole standard dei vecchi siti web.

```jsx
const submitHandler = (event) => {
  event.preventDefault(); // 1. Blocca il refresh del browser

  // 2. I dati sono già pronti nello stato grazie al Two-Way binding
  console.log("Dati pronti per l'invio API:", user); 

  // 3. Reset totale riportando lo stato al modello iniziale vuoto
  setUser({ name: "", surname: "", email: "" });
};

// Agganciamo l'handler al form, non al bottone
return <form onSubmit={submitHandler}>...</form>;

```

### 📐 Best Practices per l'Invio dei Dati

1. **`event.preventDefault()` è Mandatario:** Di default, il tag `<form>` ricarica l'intera pagina web all'invio. Nelle SPA questo comportamento distruggerebbe l'intero stato dell'applicazione in memoria. Questa istruzione dice al browser di fermarsi, lasciando la gestione dell'invio (chiamate fetch o Axios) interamente a JavaScript.
2. **Agganciare l'evento a `onSubmit` nel `<form>`:** Non inserire l'evento `onClick` sul bottone di invio. Registrando l'handler su `onSubmit` nel form, l'invio funzionerà sia cliccando il bottone, sia premendo il tasto **Invio** della tastiera mentre si compila un qualsiasi campo di testo, migliorando drasticamente l'accessibilità (UX).

---

# 2. Mandare i dati con POST

Una volta recuperati i dati dalla nostra form, potremmo volerli mandare ad un nostro server di backend utilizzando un API di quel server e agganciandoci quindi a un suo endpoint

---

## 2.1 Comporre una richiesta di POST

Facciamo caso che questo sia il nostro componente FormRegistrazione. Come vediamo, alla fine del suo `changeGenericHandler` effettuiamo l'aggiornamento della variabile di stato `user` tramite `setUser`
```jsx
import { useState } from "react";

const templateUser = {
    name: "",
    surname: "",
    email: ""
  }

function FormRegistrazione() {
  // Unico stato strutturato come oggetto
  const [user, setUser] = useState(templateUser);

  // --- HANDLER GENERICO ---
  const changeGenericHandler = (event) => {
    const { name, value } = event.target;

    // Sfruttiamo l'immutabilità e le parentesi quadre per le chiavi dinamiche
    setUser({
      ...user,        // 1. Copia profonda di tutte le proprietà attuali
      [name]: value   // 2. Sovrascrive o aggiunge dinamicamente solo la proprietà modificata
    });
  };

  return (
    <form>
      {/* L'attributo name deve corrispondere ESATTAMENTE alla chiave dell'oggetto nello stato */}
      <input name="name" value={user.name} onChange={changeGenericHandler} placeholder="Nome" />
      <input name="surname" value={user.surname} onChange={changeGenericHandler} placeholder="Cognome" />
      <input name="email" value={user.email} onChange={changeGenericHandler} placeholder="Email" />
      <button type="submit">Submit</button>
    </form>
  );
}

```
Sarà nostro compito, implementare una funzione che si occupi di mandare i dati di questa `form` ad una API quando viene scatenato l'evento di `submit`
Andiamo a vedere che aspetto avrebbe innanzitutto una funzione che si occupa di fare il "POST" ad una API:
```js
// src/utility/sendData.js

function sendData(formData) {
    // Gli headers possono variare a seconda di cosa è richiesto dalla API che stiamo contattando. Ad Esempio, CLAUDE richiede headers aggiuntivi come
    /** 
        'anthropic-version': '2023-06-01',
        "X-Api-Key": CLAUDE_API_KEY,
        "anthropic-dangerous-direct-browser-access": "true" 
    */
    const options = {
        headers: {
            'Content-type': 'application/json'
        },
        method:'POST',
        body:JSON.stringify(formData)
    };

    fetch(API_URL, options)
    .then(response => response.json())
    .catch(error => console.error(error));
    // Al momento non siamo interessati a fare nulla con il response che riceviamo dal server dopo il nostro POST quindi lasciamo la cosa blank.
}

export default sendData

```
Una volta che abbiamo a disposizione questa funzione, implementare la funzione di `submit` per la form diventa molto semplice:
```jsx
import { useState } from "react";
import sendData from "../utility/sendData.js";

const templateUser = {
    name: "",
    surname: "",
    email: ""
  }

function FormRegistrazione() {
  // Unico stato strutturato come oggetto
  const [user, setUser] = useState(templateUser);

  // --- HANDLER GENERICO ---
  const changeGenericHandler = (event) => {
    const { name, value } = event.target;

    // Sfruttiamo l'immutabilità e le parentesi quadre per le chiavi dinamiche
    setUser({
      ...user,        // 1. Copia profonda di tutte le proprietà attuali
      [name]: value   // 2. Sovrascrive o aggiunge dinamicamente solo la proprietà modificata
    });
  };

  // --- FUNZIONE PER IL SUBMIT --- //
  // Siamo certi di avere sempre i dati corretti perché questa funzione 
  // viene chiamata al Submit, ovvero quando i change hanno già fatto il
  // loro compito e impostato i dati di user a quelli inseriti dall'utente.
  //  Tuttavia è sempre giusto fare validazioni su questi dati prima di mandarli
  //  in un server di back-end

  const submitHandler = (event) => {
    event.preventDefault();
    // Qui magari chiamo delle funzioni che validano i miei dati //
    // ....

    // Una volta che i dati sono validati
    sendData(user);
    setUser(templateUser)
  } 
  return (
    <form onSubmit={submitHandler}>
      {/* L'attributo name deve corrispondere ESATTAMENTE alla chiave dell'oggetto nello stato */}
      <input name="name" value={user.name} onChange={changeGenericHandler} placeholder="Nome" />
      <input name="surname" value={user.surname} onChange={changeGenericHandler} placeholder="Cognome" />
      <input name="email" value={user.email} onChange={changeGenericHandler} placeholder="Email" />
    </form>
  );
}
```

---

## 2.2 Custom Hook per fare la POST

Possiamo anche relegare tutta la logica di POST a un custom hook. Questo ci consente di gestire meglio gli errori.

```js
// src/hooks/usePost.js
import { useState, useEffect } from "react";

function usePost(API_URL, postData){
    const [isUploaded, setIsUploaded] = useState(false);
    const [uploadError, setUploadError] = useState("");
    const [response, setResponse] = useState({});
    const options = {
        headers: {
            'Content-type': 'application/json'
        },
        method:'POST',
        body:JSON.stringify(postData)
    };

    useEffect( () => {

            setIsUploaded(false);
            const isPostDataValid = validatePostData(postData); // Non stiamo a scrivere la funzione per questo esempio
                                                                // Basti sapere che è una funzione che tra le altre cose che fa
                                                                // Controlla che nessuna property di postData sia vuota
            if(isPostDataValid)
            {
                fetch(API_URL, options)
                .then(response => {
                    setIsUploaded(true);
                    return response.json();
                })
                .then(json => setResponse(json))
                .catch(error => {
                    setIsUploaded(false);
                    setUploadError(error);
                })
            }
        }, [postData, API_URL, options]);

    return {
        response,
        isUploaded,
        uploadError
    };

}

export default usePost


```
In questo modo, abbiamo un custom hook che si occupa lui di fare tutto (potremmo addirittura delegare le funzionalità di validazione a lui) e il nostro componente form diventa semplicemente:

```jsx

import { useState } from "react";
import usePost from "../hooks/usePost.js";

const templateUser = {
    name: "",
    surname: "",
    email: ""
  }

function FormRegistrazione() {
  // Unico stato strutturato come oggetto
  const [user, setUser] = useState(templateUser);
  const [postData, setPostData] = useState(templateUser);
  const {response, isUploaded, uploadError} = usePost(API_URL, postData);

  // --- HANDLER GENERICO ---
  const changeGenericHandler = (event) => {
    const { name, value } = event.target;

    // Sfruttiamo l'immutabilità e le parentesi quadre per le chiavi dinamiche
    setUser({
      ...user,        // 1. Copia profonda di tutte le proprietà attuali
      [name]: value   // 2. Sovrascrive o aggiunge dinamicamente solo la proprietà modificata
    });
  };

  const submitHandler = (event) => {
    event.preventDefault();
    // Dopo aver fatto tutte le validazioni che dobbiamo fare
    setPostData(user);
    setUser(templateUser); // Resettiamo le field di input della nostra form
  }


  return (
    <form onSubmit={submitHandler}>
      {/* L'attributo name deve corrispondere ESATTAMENTE alla chiave dell'oggetto nello stato */}
      <input name="name" value={user.name} onChange={changeGenericHandler} placeholder="Nome" />
      <input name="surname" value={user.surname} onChange={changeGenericHandler} placeholder="Cognome" />
      <input name="email" value={user.email} onChange={changeGenericHandler} placeholder="Email" />
      <button type="submit">Submit</button>
    </form>
  );
}

```
### Il cambio di `postData` al momento del submit farà si che la `useEffect` che ha tra le sue dependencies `postData` venga eseguita e quindi venga fatto il POST.
---

## 📌 3. Risorse Tecniche
- [Controlled and Uncontrolled Components](https://react.dev/learn/sharing-state-between-components#controlled-and-uncontrolled-components)
- [Custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)
- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [Riassunto semplice delle form in React by Samuel](https://github.com/classe154/react-form-full)
- [Spunto di approfondimento per quando abbiamo form enormi e non ha senso mettersi a scrivere tutto a mano](https://react-hook-form.com/docs/useform)

---

## 📌 4. Key Takeaways

- **Two-Way Data Binding:** in React gli input controllati sono sempre sincronizzati con lo stato, quindi il valore dell'input e lo stato sono aggiornati insieme.
- **`onChange` in React non è il `change` nativo:** si attiva a ogni digitazione, quindi non usare `onChange` per chiamate API finali senza debouncing o validazione.
- **`onBlur` se serve il vero comportamento `change`:** usa `onBlur` quando vuoi rilevare il valore solo al termine dell'editing e non ad ogni tasto premuto.
- **Handler generico con `[name]`:** raggruppare i campi in un oggetto stato e usare il nome dell'input come chiave rende i form più scalabili e meno ripetitivi.
- **`event.preventDefault()` è obbligatorio sul submit:** impedisce il refresh della pagina nelle SPA e permette gestire l'invio con JavaScript.
- **Validazione client-side prima del POST:** controlla i dati dell'utente prima di inviarli al server per evitare richieste errate o incomplete.
- **Custom Hook per la POST:** estrarre la logica di fetch in un hook (`usePost`) semplifica il componente e centralizza gestione errori, stato di caricamento e risposta.
- **Immutabilità dello stato:** usa lo spread operator (`...user`) per creare una nuova copia dell'oggetto stato, così React riconosce la modifica e rilancia il render.
- **Attributo `name` importante:** i `name` degli input devono corrispondere esattamente alle proprietà dell'oggetto stato per il binding dinamico.
- **Checkbox e radio richiedono binding specifico:** checkbox usa `checked`, radio usa `checked={state === value}` e tutti condividono lo stesso `name` per il gruppo.

---

## 📚 5. Syllabus

| Termine | Definizione istituzionale | Spiegazione semplice |
| --- | --- | --- |
| Two-Way Data Binding | Meccanismo che sincronizza i dati tra lo stato e la UI in entrambe le direzioni. | Quando lo stato aggiorna la view e l'input aggiorna lo stato. |
| Controlled Component | Componente JSX il cui valore è controllato dallo stato di React. | Un input che non gestisce il proprio valore da solo, ma lo prende dallo state. |
| Uncontrolled Component | Componente JSX che gestisce il proprio valore internamente, senza usare lo stato di React. | Un input che si comporta come un elemento DOM normale. |
| `onChange` | Evento React che si attiva quando il valore di un input cambia. | Scatta a ogni carattere digitato in un campo controllato. |
| `onBlur` | Evento React che si attiva quando un elemento perde il focus. | Si usa quando si vuole verificare il valore solo al termine dell'editing. |
| `event.target.value` | Proprietà dell'evento che contiene il valore corrente dell'input. | Il testo che l'utente ha scritto in quel momento. |
| Spread Operator (`...`) | Sintassi ES6 per copiare e unire oggetti o array. | Serve a creare una nuova copia dello stato senza modificarne l'originale. |
| Proprietà Calcolate (`[name]`) | Sintassi che usa il valore di una variabile come chiave di un oggetto. | Invece di scrivere la chiave a mano, la generi dinamicamente dal `name` del campo. |
| Custom Hook | Funzione React che incapsula logica riutilizzabile basata su hook. | Un modo per separare la logica di stato / effetti dal componente. |
| `useEffect` | Hook che esegue un effetto collaterale dopo il render o quando le dipendenze cambiano. | Si usa per fare fetch, sincronizzare dati o reagire a cambi di stato. |
| `event.preventDefault()` | Metodo che blocca il comportamento di default di un evento. | Previene il refresh della pagina quando si invia un form. |
| POST Request | Richiesta HTTP che invia dati al server per creare o aggiornare una risorsa. | Il metodo che manda i dati del form al backend. |
| Fetch API | API nativa per fare richieste HTTP asincrone dal browser. | La funzione `fetch()` che parla con il server. |
| Headers | Metadati HTTP inviati insieme alla richiesta. | Dicono al server come leggere i dati che stai mandando. |
| Validazione Client | Controllo dei dati sul lato client prima dell'invio. | Verificare che i campi siano validi prima di fare la POST. |
| Reset Form | Operazione che riporta i campi del form ai valori iniziali. | Svuotare il form dopo l'invio. |
| State Management | Gestione e aggiornamento organizzato dello stato dell'app. | Tenere ordinati i dati che cambiano mentre l'utente interagisce. |
| `name` attribute | Attributo HTML che identifica il campo del form. | Viene usato come chiave per aggiornare l'oggetto stato dinamicamente. |
