#### Modulo: React
**Titolo:** Data Binding
**Data:** 16/05/2026

**📍 Indice Rapido**

*   [1. Data Binding](#1-data-binding)
    *   [Introduzione al Data Binding in React](#introduzione-al-data-binding-in-react)
    *   [1.2 Il Meccanismo del Two-Way Data Binding (Controlled Components)](#12-il-meccanismo-del-two-way-data-binding-controlled-components)
        *   [1.2.1 Come funziona il “Doppio Legame” in 2 Passaggi](#121-come-funziona-il-doppio-legame-in-2-passaggi)
*   [1.3 Gestione dello Stato Avanzata: Oggetti e Handler Generici](#13-gestione-dello-stato-avanzata-oggetti-e-handler-generici)
    *   [1.3.1 Focus Tecnico: Proprietà Calcolate [name]](#131-focus-tecnico-proprietà-calcolate-name)
    *   [L'importance dello Spread Operator (...user)](#limportanza-dello-spread-operator-user)
*   [1.4 Gestione del Submit e Reset del Form](#14-gestione-del-submit-e-reset-del-form)
    *   [1.4.1 Best Practices per l'Invio dei Dati](#141-best-practices-per-linvio-dei-dati)
*   [💡 Schema Logico: Il Flusso dei Dati](#schema-logico-il-flusso-dei-dati)
*   [2 Risorse e Documentazione](#2-risorse-e-documentazione)
*   [3 Key Takeaways](#3-key-takeaways)
*   [4 Glossario](#4-glossario)

# 1 Data Binding

## Introduzione al Data Binding in React

In React, il concetto di Data Binding rappresenta il meccanismo che collega i dati dell’applicazione alla sua interfaccia grafica (UI). È uno dei principi fondamentali che permette a React di creare applicazioni dinamiche, reattive e facilmente prevedibili.

A differenza di molti framework tradizionali che utilizzano un sistema di sincronizzazione automatica bidirezionale, React adotta principalmente un modello di One-Way Data Binding (flusso dati unidirezionale). Questo significa che i dati scorrono in una sola direzione: dallo stato del componente verso l’interfaccia utente.

Quando lo stato cambia, React aggiorna automaticamente la UI attraverso un nuovo rendering. Al contrario, le modifiche effettuate direttamente dall’utente nell’interfaccia non aggiornano automaticamente i dati dell’applicazione: è necessario intercettare gli eventi tramite JavaScript (ad esempio con onChange) e aggiornare esplicitamente lo stato.

Questo approccio rende il comportamento dell’applicazione più stabile, prevedibile e semplice da debuggare, perché ogni modifica segue un flusso logico controllato.

Su questa base nasce il pattern dei Controlled Components, in cui gli elementi dei form HTML non gestiscono autonomamente il proprio valore, ma vengono completamente controllati dallo stato React, trasformando il componente in una vera e propria “single source of truth”.

## 1.2 Il Meccanismo del Two-Way Data Binding (Controlled Components)

Nel modello dichiarativo di React, l'approccio standard prevede che gli input del form siano **Controlled Components** (Componenti Controllati). Questo significa che l'input non gestisce più il proprio testo internamente, ma è sincronizzato con lo stato di React attraverso un doppio legame.

---

## Esempio Base

```javascript
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

---

## 1.2.1 Come funziona il “Doppio Legame” in 2 Passaggi

### Dallo Stato alla UI (Model ➡️ View)

L'attributo `value={task}` blocca l'input. Il campo mostrerà sempre e solo quello che è scritto nella variabile di stato. Senza una funzione che modifica lo stato, l'input risulterebbe congelato.

### Dalla UI allo Stato (View ➡️ Model)

L'evento `onChange` intercetta la digitazione dell'utente, estrae il valore con `event.target.value` e aggiorna lo stato tramite la funzione setter (`setTask`). Questo provoca un re-render immediato dell'input con il nuovo carattere visualizzato.

### Il Vantaggio del Reset Immediato

Questa sincronizzazione rende operazioni come lo svuotamento dei form estremamente semplici. Per ripulire un campo di testo dopo un'operazione, basta richiamare il setter impostando una stringa vuota:

```javascript
setTask("");
```

L'input si svuoterà automaticamente senza dover manipolare manualmente i nodi del DOM.

---

# 1.3 Gestione dello Stato Avanzata: Oggetti e Handler Generici

Quando un form inizia ad avere molti campi (es. Nome, Cognome, Indirizzo), creare un `useState` per ogni singola stringa rende il codice ripetitivo, frammentato e difficile da scalare.

La soluzione ideale è raggruppare i dati in un unico oggetto di stato.

Per aggiornare questo oggetto in modo efficiente, si utilizza un singolo Handler Generico basato sull'attributo `name` dei tag HTML.

---

## Esempio con Oggetto Stato

```javascript
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
      ...user,
      [name]: value
    });
  };

  return (
    <form>
      <input
        name="name"
        value={user.name}
        onChange={changeGenericHandler}
        placeholder="Nome"
      />

      <input
        name="surname"
        value={user.surname}
        onChange={changeGenericHandler}
        placeholder="Cognome"
      />

      <input
        name="email"
        value={user.email}
        onChange={changeGenericHandler}
        placeholder="Email"
      />
    </form>
  );
}
```

---

## 1.3.1 Focus Tecnico: Proprietà Calcolate [name]
Le parentesi quadre servono a trasformare una chiave dell'oggetto da statica (testo fisso) a dinamica (contenuto di una variabile)
.
1. Senza Parentesi (Chiave Statica)
Se scrivi il codice così:
{ name: value }
JavaScript crea un oggetto dove la chiave è letteralmente la parola "name". È come scrivere un'etichetta fissa su un cassetto: non cambierà mai.
2. Con le Parentesi (Chiave Dinamica)
Usando le parentesi quadre:
{ [name]: value }
Stai dicendo a JavaScript: "Non chiamare la chiave 'name'. Apri la variabile name, guarda cosa c'è scritto dentro e usa quel valore come nome della chiave".

L'utilità pratica in React
Questa sintassi è fondamentale nei componenti controllati per gestire più input contemporaneamente con una sola funzione.

Immagina di avere due input:

```

<input name="email" />
<input name="password" />

```

Quando l'utente scrive, React cattura il name dell'input che sta cambiando. Grazie alle parentesi quadre, il tuo codice diventa universale:

Se l'utente scrive nell'input email, il codice genera: 
{ email: "..." }.
Se scrive in quello password, genera: 
{ password: "..." }.

In breve: Le parentesi quadre sono un "interruttore" che permette a una singola riga di codice di aggiornare qualsiasi campo dello stato in modo automatico.

--------------------------------------------------------------------------------
## L'importanza dello Spread Operator (`...user`)

Gli oggetti in React sono immutabili.

Non possiamo fare:

```javascript
user.name = "Luca";
```

perché React non rileverebbe il cambio di riferimento in memoria (*shallow comparison*) e non avvierebbe il re-render.

Lo spread operator crea un nuovo oggetto in memoria.

Senza di esso, ogni volta che scriviamo nel campo cognome cancelleremmo le proprietà del nome e dell'email.

---


# 1.4 Gestione del Submit e Reset del Form

Nelle applicazioni SPA (Single Page Application), la sottomissione di un form non deve seguire il comportamento standard dei vecchi siti multi-pagina.

---

## Esempio di Submit Handler

```javascript
const submitHandler = (event) => {
  event.preventDefault(); // 1. Blocca il refresh del browser

  // 2. I dati sono già pronti nello stato
  console.log("Dati pronti per l'invio API:", user);

  // 3. Reset totale
  setUser({
    name: "",
    surname: "",
    email: ""
  });
};

// Agganciamo l'handler al form
return <form onSubmit={submitHandler}>...</form>;
```

---

## 1.4.1 Best Practices per l'Invio dei Dati

### `event.preventDefault()` è Mandatario

Di default, il tag `<form>` ricarica l'intera pagina web all'invio.

Nelle SPA questo comportamento distruggerebbe lo stato dell'applicazione memorizzato in JavaScript.

Questa istruzione dice al browser di fermarsi, lasciando la gestione dell'invio (`fetch`, `Axios`, API REST, ecc.) interamente a JavaScript.

---

### Agganciare l'evento a `onSubmit` nel `<form>`

Non inserire l'evento `onClick` sul bottone di invio.

Registrando l'handler su `onSubmit` nel form:

* l'invio funzionerà cliccando il bottone;
* funzionerà anche premendo il tasto Invio;
* migliorerà accessibilità e UX.

---

# 💡 Schema Logico: Il Flusso dei Dati

In React, il flusso dei dati è unidirezionale (*One-Way Data Flow*).

## Le Props (Dall'alto verso il basso)

Le `props` viaggiano dal Genitore ➡️ Figlio.

Sono dati in sola lettura:

* il componente figlio può usarli;
* non può modificarli direttamente.

```plaintext
[Parent Component]
        │
        │ props
        ▼
[Child Component]
```

---

## Gli Eventi (Dal basso verso l'alto)

Quando il figlio deve comunicare qualcosa al genitore, non modifica direttamente i dati.

Invoca invece una funzione ricevuta tramite props.

```plaintext
[Child Component]
        │
        │ callback()
        ▼
[Parent Component]
```

Il genitore decide poi:

* se aggiornare lo stato;
* come aggiornarlo;
* quali dati ridistribuire.

---

## Lo Stato (Il Motore Centrale)

Quando cambia lo stato:

1. React esegue un nuovo render;
2. i dati aggiornati vengono propagati verso i figli;
3. la UI si sincronizza automaticamente.

```plaintext
[setState()]
      │
      ▼
[Nuovo Render]
      │
      ▼
[Props Aggiornate]
      │
      ▼
[UI Sincronizzata]
```

---

# 2 Risorse e Documentazione

* React Doc: [gestione dello stato e controlled components] (https://react.dev/learn/state-a-components-memory)


---

# 3 Key Takeaways 

* React aggiorna di default la UI attraverso un flusso dati unidirezionale.
* Nei Controlled Components il DOM perde la gestione autonoma dei dati: tutto passa attraverso lo stato React.
* Gli oggetti di stato devono essere trattati in modo immutabile.
* `onSubmit` + `event.preventDefault()` sono fondamentali nei form per garantire il corretto funzionamento della SPA.
* Lo stato in React è asincrono, snapshot-based e ottimizzato tramite batching.
* Le callback permettono ai componenti figli di comunicare con i genitori senza rompere il flusso unidirezionale.

---

# 4 Glossario 

| Termine Istituzionale             | Definizione Formale                                                                                                                                                  | "Spiega Brutta"                                                                                                                                                                           |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Data Binding**                  | Processo sistematico che stabilisce una connessione logica e automatica tra i dati dell'applicazione (Model) e gli elementi di presentazione visiva (View).          | Il ponte magico che fa sì che se tocchi qualcosa nel codice cambia lo schermo, e se l'utente scrive sullo schermo cambia la variabile nel codice.                                         |
| **Controlled Component**          | Elemento di un form HTML il cui valore corrente è governato ed impostato esclusivamente dallo stato di React, che funge da “singola fonte di verità”.                | Un input totalmente sottomesso a React: non ricorda nemmeno cosa ha scritto l'utente a meno che tu non glielo salvi esplicitamente in una variabile di stato.                             |
| **Two-Way Binding**               | Flusso bidirezionale di sincronizzazione in cui le mutazioni del modello modificano la UI e, simmetricamente, le interazioni sulla UI modificano il modello.         | Una strada a doppio senso: la variabile comanda l'input (`value`), ma l'input aggiorna la variabile (`onChange`) ad ogni singola lettera pigiata sulla tastiera.                          |
| **Immutabilità (State)**          | Principio per cui lo stato corrente non può subire modifiche *in-place*. React confronta solo la referenza di memoria per decidere se innescare il rendering.        | La regola d'oro per cui non puoi modificare un oggetto esistente. Devi fare una “fotocopia” totale, cambiare il dettaglio sulla copia e dare a React la copia nuova di zecca.             |
| **Computed Property Names**       | Funzionalità ES6 che permette di calcolare dinamicamente le chiavi di un oggetto a runtime usando la sintassi `[name]: value`.                                       | Usare le parentesi quadre per gestire 100 input diversi con una sola funzione, usando il nome del campo come “chiave” per trovare il pezzetto giusto dello stato.                         |
| **Single Page Application (SPA)** | Architettura web che carica una sola pagina HTML e aggiorna i contenuti tramite JavaScript usando le History API, simulando la navigazione.                          | Un sito che non si ricarica mai davvero; simula il cambio pagina via codice, rendendo ogni refresh manuale un potenziale reset della memoria dell'app.                                    |
| **Virtual DOM & Diffing**         | Rappresentazione leggera della UI in memoria che React confronta (*Diffing*) con la versione precedente per calcolare le modifiche minime da applicare al DOM reale. | React si fa una “bozza” veloce del sito, vede cosa hai cambiato e tocca il browser vero solo per quel millimetro di codice diverso.                                                       |
| **Side Effect**                   | Operazione che interagisce con l'esterno del componente, come chiamate API, timer o manipolazione manuale del DOM.                                                   | Tutto quello che il componente fa “di nascosto” oltre a mostrare il codice, come telefonare a un database o far partire un cronometro.                                                    |
| **Lifting State Up**              | Tecnica di spostamento dello stato al genitore comune più vicino per permettere a due o più componenti figli di condividere gli stessi dati.                         | Quando due fratelli (componenti) devono usare lo stesso gioco (dato), il gioco va dato in mano al papà (genitore) che decide a chi passarlo.                                              |
| **Snapshot**                      | Natura dello stato che rimane “congelato” al valore che aveva nel momento esatto in cui il rendering è iniziato.                                                     | Lo stato non è un secchio che si riempie in diretta, ma una foto ricordo: se fai tre clic velocissimi, React guarda sempre la vecchia foto finché non ha finito di “sviluppare” la nuova. |
| **Functional Update**             | Passaggio di una funzione di callback al setter (es. `setCount(prev => prev + 1)`) per aggiornare lo stato basandosi sul valore più recente disponibile.             | Invece di dire “il valore è 5”, dici a React “prendi l'ultimo valore che ti ricordi e aggiungici 1”, così non perdi il conto se sei troppo veloce.                                        |
| **Batching**                      | Processo per cui React raggruppa più aggiornamenti di stato in un unico re-render per ottimizzare le prestazioni.                                                    | Invece di correre a pulire ogni volta che cade una briciola, React aspetta che tu abbia finito di mangiare per fare una sola passata di scopa.                                            |
