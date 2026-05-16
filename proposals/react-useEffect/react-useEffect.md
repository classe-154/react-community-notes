# React `useEffect` — Appunti di lezione

1. Argomenti Lezione: React useEffect

 - [1.1 Cos'è useEffect? (Gestione dei Side Effects)](#11-cosè-useeffect)

 - [1.2 Anatomia di useEffect (Setup, Cleanup e Dipendenze)](#12-anatomia-di-useeffect)

 - [1.3 Ciclo di vita di React (Trigger, Render, Commit, Paint)](#13-ciclo-di-vita-di-react)

 - [1.4 Le tre varianti dell'array delle dipendenze](#14-le-tre-varianti-dellarray-delle-dipendenze)

 - [1.5 La funzione di cleanup (Prevenzione Memory Leaks)](#15-la-funzione-di-cleanup)

 - [1.6 Ordine di esecuzione — Schema completo](#16-ordine-di-esecuzione--schema-completo)

 - [1.7 Esempi pratici di sincronizzazione](#17-esempi-pratici)
  * [1.7.1 Mount e Unmount di un componente](#171-esempio-1--mount-e-unmount)
  * [1.7.2 Sincronizzazione dinamica tramite Dipendenze](#172-esempio-2--dipendenze)
  * [1.7.3 Interazione con le API del Browser](#173-esempio-3--side-effects-del-browser)

 - [1.8 Errori comuni e Anti-pattern](#18-errori-comuni)
  * [1.8.1 Loop infiniti da oggetti instabili](#181-loop-infinito--dipendenza-che-cambia-ad-ogni-render)
  * [1.8.2 Aggiornamenti di stato su componenti smontati](#182-aggiornare-state-su-componente-smontato)
  * [1.8.3 Omissione di variabili nell'array delle dipendenze](#183-dimenticare-le-dipendenze)

 - [1.9 Tabella riepilogativa e Casi d'uso](#19-tabella-riepilogativa)

2. [Risorse Tecniche (MDN, W3S, React)](#-risorse-tecniche)
3. [Key Takeaways](#key-takeaways) 
4. [Glossario](#4-glossario)

## 1.1 Cos'è `useEffect`?

`useEffect` è un **hook** di React che permette di eseguire codice **in risposta ai cambiamenti del ciclo di vita** di un componente.

Serve a gestire i **side effects**: operazioni che interagiscono con qualcosa *fuori* dal componente React.

**Esempi di side effects:**
- Fetch di dati da un'API
- Timer (`setTimeout`, `setInterval`)
- Sottoscrizioni a eventi del browser o websocket
- Manipolazione diretta del DOM (es. `document.title`)
- Logging e analytics

> **Perché non scrivere queste operazioni direttamente nel corpo del componente?**
> Perché il corpo del componente viene eseguito ad ogni render. Senza `useEffect`, un fetch verrebbe ripetuto ogni volta che lo state cambia — portando a loop infiniti e comportamenti inattesi.

---

## 1.2 Anatomia di `useEffect`

```jsx
useEffect(() => {
  // SETUP FUNCTION
  // Codice da eseguire al mount o quando le dipendenze cambiano

  return () => {
    // CLEANUP FUNCTION (opzionale)
    // Codice di pulizia: prima del prossimo setup o all'unmount
  };
}, [/* ARRAY DELLE DIPENDENZE */]);
```

Le tre parti:

| Parte | Cos'è | Quando si esegue |
|---|---|---|
| **Setup function** | La callback principale | Mount + ogni volta che una dipendenza cambia |
| **Cleanup function** | Il `return` della callback | Prima del prossimo setup + all'unmount |
| **Array dipendenze** | Controlla *quando* rieseguire | — |

---

## 1.3 Ciclo di vita di React

Ogni render di un componente attraversa tre fasi:

### Trigger
Ci sono due motivi per cui React renderizza un componente:
- **Render iniziale**: quando l'app si avvia (`createRoot().render()`)
- **Re-render**: quando lo state del componente (o di un antenato) viene aggiornato

### Render
React chiama le funzioni dei componenti per costruire il **Virtual DOM** (una rappresentazione JavaScript della struttura HTML). In questa fase:
- Calcola cosa dovrebbe essere visualizzato
- Confronta con il Virtual DOM precedente (**riconciliazione**)
- **Non tocca ancora il DOM reale**

### Commit
React applica le modifiche al DOM reale. In questa fase:
- **MOUNT** — un elemento viene *inserito* nel DOM per la prima volta
- **UPDATE** — un elemento già nel DOM viene *aggiornato*
- **UNMOUNT** — un elemento viene *rimosso* dal DOM

### Dove si inserisce `useEffect`?

```
Render → Commit (DOM update) → Paint (UI visibile) → useEffect
```

`useEffect` viene eseguito **dopo** che il browser ha già aggiornato lo schermo. Non blocca la UI.

> **Nota — Da React 18** per eventi discreti dell'utente (click, keydown), `useEffect` viene eseguito *prima* del paint per garantire coerenza visiva. Per la maggior parte dei casi pratici (fetch, log, timer) non fa differenza.

---

## 1.4 Le tre varianti dell'array delle dipendenze

### Array vuoto `[]` — esegui solo al mount/unmount

```jsx
useEffect(() => {
  console.log("eseguito solo al mount");

  return () => {
    console.log("eseguito solo all'unmount");
  };
}, []);
```

Utile per: connessioni iniziali, fetch una-tantum, setup di event listener globali.

### Array con variabili `[var]` — riesegui quando cambiano

```jsx
useEffect(() => {
  console.log("roomId è cambiato:", roomId);

  return () => {
    console.log("cleanup per roomId:", roomId);
  };
}, [roomId]);
```

Utile per: sincronizzare un effetto con una prop o uno state.

### Nessun array — esegui ad ogni render

```jsx
useEffect(() => {
  console.log("eseguito ad ogni render!");
});
```

> **Evita questa forma** nella quasi totalità dei casi. Causa esecuzioni inutili e può generare loop infiniti.

---

## 1.5 La funzione di cleanup

La cleanup è la funzione restituita con `return` dentro il setup. È **opzionale ma importante**.

**Quando viene chiamata:**
1. **Prima di ogni nuovo setup** (quando cambiano le dipendenze)
2. **All'unmount** del componente (rimozione dal DOM)

```jsx
useEffect(() => {
  // SETUP: prenota una risorsa
  const timer = setInterval(() => console.log("tick"), 1000);

  return () => {
    // CLEANUP: libera la risorsa
    clearInterval(timer);
  };
}, []);
```

**Senza cleanup si rischia:**
- Memory leaks (timer che continuano a girare, listener che si accumulano)
- Aggiornamenti di state su componenti già smontati
- Connessioni doppie o comportamenti imprevedibili

---

## 1.6 Ordine di esecuzione — schema completo

```
🔵 MOUNT (primo render)
   └─ [Render] → [Commit] → [Paint] → ✅ setup()

🔄 UPDATE (dipendenza cambiata)
   └─ [Render] → [Commit] → [Paint] → 🧹 cleanup(valori vecchi) → ✅ setup(valori nuovi)

🔴 UNMOUNT (componente rimosso)
   └─ 🧹 cleanup() finale
```

**Regola da ricordare:** il cleanup riceve sempre i valori *vecchi*, il setup riceve sempre i valori *nuovi*.

---

## 1.7 Esempi pratici

### 1.7.1 Esempio 1 — Mount e Unmount

Un componente che appare e scompare dal DOM in base a uno state booleano.

```jsx
function Dialog() {
  useEffect(() => {
    console.log("Dialog useEffect: setup");

    return () => {
      console.log("Dialog useEffect: cleanup");
    };
  }, []);

  return <div>Dialog aperto</div>;
}

function App() {
  const [show, setShow] = useState(false);

  return (
    <>
      <button onClick={() => setShow(s => !s)}>Toggle</button>
      {show && <Dialog />}
    </>
  );
}
```

**Come testarlo:** apri la console (F12) e clicca il bottone Toggle.

**Console attesa:**
```
Dialog useEffect: setup      ← al mount
Dialog useEffect: cleanup    ← all'unmount
```

**Concetti chiave:**
- `useEffect` con `[]` si esegue solo al **mount**
- Il `return` (cleanup) si esegue solo all'**unmount**
- Il cleanup serve a "disfare" ciò che il setup ha fatto (es. timer, connessioni)

---

### 1.7.2 Esempio 2 — Dipendenze

Un componente che si connette a una stanza chat. Quando la stanza cambia, deve disconnettersi da quella vecchia e connettersi a quella nuova.

```jsx
function DialogWithDeps({ roomId }) {
  useEffect(() => {
    console.log("🔌 setup: connect room", roomId);

    return () => {
      console.log("❌ cleanup: disconnect room", roomId);
    };
  }, [roomId]);

  return <div>Dialog room: {roomId}</div>;
}

function App() {
  const [show, setShow] = useState(false);
  const [roomId, setRoomId] = useState("general");

  return (
    <div>
      <select value={roomId} onChange={(e) => setRoomId(e.target.value)}>
        <option value="general">general</option>
        <option value="travel">travel</option>
        <option value="music">music</option>
      </select>

      <button onClick={() => setShow(s => !s)}>
        {show ? "Close Dialog" : "Open Dialog"}
      </button>

      {show && <DialogWithDeps roomId={roomId} />}
    </div>
  );
}
```

**Console attesa:**
```
🔌 setup: connect room general       ← mount
❌ cleanup: disconnect room general  ← roomId cambia
🔌 setup: connect room travel        ← nuovo setup con il valore nuovo
❌ cleanup: disconnect room travel   ← unmount
```

**Concetto chiave:** quando `roomId` cambia, React esegue *prima* la cleanup con il valore vecchio e *poi* il setup con quello nuovo. È come disconnettersi dalla stanza precedente prima di entrare in quella nuova.

---

### 1.7.3 Esempio 3 — Side effects del browser

Un componente che aggiorna il titolo della scheda del browser in base alla pagina corrente.

```jsx
function PageTitle({ page }) {
  useEffect(() => {
    const originalTitle = document.title;

    const pageNames = {
      home: "Home",
      about: "Chi Siamo",
      contact: "Contatti",
      products: "Prodotti"
    };

    document.title = `MyApp - ${pageNames[page] || page}`;

    return () => {
      document.title = originalTitle;
    };
  }, [page]);

  return (
    <div>
      Stai visualizzando: <strong>{page}</strong>
    </div>
  );
}
```

**Concetti chiave:**
- `useEffect` può modificare cose *fuori* dal componente (DOM, localStorage, URL...)
- La cleanup ripristina lo stato precedente: fondamentale in SPA dove lo stesso DOM persiste tra "pagine"
- `document.title` è un esempio classico di interazione con le API del browser

---

## 1.8 Errori comuni

### 1.8.1 Loop infinito — dipendenza che cambia ad ogni render

```jsx
// SBAGLIATO: ogni render crea un nuovo oggetto, che fa scattare l'effetto...
const options = { url: "/api/data" }; // nuovo oggetto ad ogni render!

useEffect(() => {
  fetch(options.url)
    .then(r => r.json())
    .then(json => {
      setData(json);
    });
}, [options]); // options è sempre "diverso" per React → loop infinito
```

```jsx
// CORRETTO: usa il valore direttamente dentro l'effetto
useEffect(() => {
  fetch("/api/data")
    .then(r => r.json())
    .then(json => {
      setData(json);
    });
}, []);
```

### 1.8.2 Aggiornare state su componente smontato

**Scenario:** un fetch viene avviato al mount del componente. Prima che la risposta arrivi, l'utente naviga altrove e il componente viene smontato. La callback
```jsx
.then(json => {
  setData(json);
})
```
è però ancora "in volo": quando il fetch completa, prova a chiamare `setData` su un componente che non esiste più.

**Perché è un problema:**
- Il componente resta in memoria finché il fetch non termina (**memory leak**)
- React segnala il warning: *"Can't perform a React state update on an unmounted component"*
- In app complesse può causare aggiornamenti di state inattesi su componenti sbagliati

```jsx
// SBAGLIATO: il fetch può completare dopo l'unmount
useEffect(() => {
  fetch("/api/data")
    .then(r => r.json())
    .then(json => {
      setData(json);
    }); // setState su componente già smontato!
}, []);
```

```jsx
// CORRETTO: usa un flag per ignorare il risultato se il componente è smontato
useEffect(() => {
  let cancelled = false;

  fetch("/api/data")
    .then(r => r.json())
    .then(json => {
      if (!cancelled) setData(json);
    });

  return () => {
    cancelled = true;
  };
}, []);
```

### 1.8.3 Dimenticare le dipendenze

```jsx
// SBAGLIATO: userId viene usato ma non è dichiarato nelle dipendenze
useEffect(() => {
  fetchUser(userId);
}, []); // React (e il linter) avvertirà di questo problema
```

```jsx
// CORRETTO: dichiara tutto ciò che usi dentro l'effetto
useEffect(() => {
  fetchUser(userId);
}, [userId]);
```

---

## 1.9 Tabella riepilogativa

| Array dipendenze | Quando si esegue il setup | Quando si esegue il cleanup |
|---|---|---|
| `[]` | Solo al mount | Solo all'unmount |
| `[a, b]` | Mount + ogni volta che `a` o `b` cambiano | Prima di ogni nuovo setup + unmount |
| *(omesso)* | Ad ogni render | Prima di ogni render successivo |

| Caso d'uso | Variante consigliata |
|---|---|
| Fetch iniziale | `[]` |
| Sincronizzare con una prop | `[propName]` |
| Timer, WebSocket, event listener | `[]` con cleanup |
| Titolo pagina / localStorage | `[valore]` con cleanup |

---

## 🔗 Risorse Tecniche

* **React Docs (v19):** [Synchronizing with Effects (`useEffect`)](https://react.dev/reference/react/useEffect) 
* **React Docs:** [Lifecycle of Reactive Effects](https://react.dev/learn/lifecycle-of-reactive-effects) – Approfondimento concettuale su come React gestisce la sincronizzazione rispetto ai vecchi cicli di vita.
* **MDN Web Docs:** [React Accessibility](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Frameworks_libraries/React_accessibility) 
* **W3School Docs:** [React UseEffect Hooks]()

---


## Key Takeaways


* **Quando si usa?**
  * Quando il componente deve fare qualcosa in un momento preciso della sua vita: appena appare (**mount**), quando cambiano dei dati (**update**), o appena prima di sparire (**unmount**).

* **A che cosa serve?**
  * A gestire i **side effects** (tutto ciò che esce dal controllo diretto di React). 
  * *Esempi:* Fetch di dati da un'API, far partire un timer (`setInterval`), salvare nel `localStorage` o cambiare il titolo della scheda (`document.title`).

* **Perché è importante?**
  * **Blocca i loop infiniti:** Scrivere un fetch direttamente nel corpo del componente significa rieseguirlo a ogni minimo cambio di stato, mandando l'app (o il server) in crash.
  * **Non rallenta l'interfaccia:** Viene eseguito *dopo* che la pagina è stata disegnata, così l'utente vede subito un'app fluida.

* **Cose da non dimenticare!**
  * 🧹 **La Cleanup (`return () => {}`):** Se crei un timer o una connessione, devi sempre "spegnerli" nel return. Altrimenti continuano a girare in background consumando memoria (*memory leak*).
  * **L'array delle dipendenze:** * `[]` vuoto = esegui solo al mount del componente.
    * `[variabile]` = riesegui solo se quel dato cambia.
    * *Nessun array* = esegui a ogni singolo render (da evitare!).
  * **Dichiarare tutto:** Ogni variabile (stato o prop) che usi dentro l'effetto *deve* stare nell'array delle dipendenze, o l'effetto userà dati vecchi e congelati nel tempo.

---
# 4 Glossario

| **Termine Istituzionale** | **Definizione Formale** | **"Spiega Brutta"** |
| --- | --- | --- |
| **Side Effect** | Qualsiasi operazione che modifichi uno stato al di fuori del proprio ambiente locale o che interagisca con API esterne al flusso di rendering puro del componente. | Tutto ciò che esce dai confini di React e va a toccare il mondo esterno (es. tabelle del DB, timer del browser o il titolo della scheda). |
| **Mount** | La fase del ciclo di vita in cui un componente viene inizializzato e inserito per la prima volta nell'albero del DOM reale del browser. | Il momento in cui il componente viene al mondo e appare per la prima volta sullo schermo dell'utente. |
| **Unmount** | La fase in cui un componente React viene rimosso definitivamente dall'albero del DOM e distrutto. | Quando il componente muore, sparisce dallo schermo e viene cancellato del tutto dalla memoria. |
| **Cleanup Function** | La funzione opzionale restituita dal setup di un effetto, eseguita prima del setup successivo o durante l'unmount per liberare risorse. | Il servizio di pulizia che cancella i casini fatti dall'effetto (timer appesi, connessioni aperte) prima che il componente si aggiorni o venga eliminato. |
| **Array delle Dipendenze** | Il secondo argomento passato a `useEffect`. Array di valori che React monitora tramite shallow equality per decidere se rieseguire l'effetto. | La lista delle "spie". Se anche uno solo dei valori in questa lista cambia rispetto al render precedente, l'effetto riparte da capo. |
| **Memory Leak** | Consumo ingiustificato di memoria causato dalla mancata deallocazione di risorse non più necessarie o utilizzate dall'applicazione. | Quando lasci i rubinetti aperti (es. un timer che gira all'infinito) anche se la casa (il componente) è stata demolita, intasando la memoria del browser. |
