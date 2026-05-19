#### Modulo: React
**Titolo:** Iterazioni in JSX

**📍 Indice Rapido**

1. [Iterazioni in JSX](#1-iterazioni-in-jsx)
   - 1.0 [Programmazione Funzionale in React](#10-programmazione-funzionale-in-react)
   - 1.1 [L'attributo key: il pilastro delle liste](#11-lattributo-key-il-pilastro-delle-liste)
     * 1.1.1 [Perché la Key è importante?](#perché-la-key-è-importante)
     * 1.1.2 [Quale valore usare come Key](#quale-valore-usare-come-key)
     * 1.1.3 [Quando evitare index come Key](#quando-evitare-index-come-key)
   - 1.2 [Arrow Functions: Breve Reminder](#12-arrow-functions-breve-reminder)
     * 1.2.1 [Return Implicito vs Esplicito](#121-return-implicito-vs-esplicito)
     * 1.2.2 [Destructuring nei Parametri](#122-destructuring-nei-parametri)
   - 1.3 [Il metodo .map(): Trasformare i Dati in JSX](#13-il-metodo-map-trasformare-i-dati-in-jsx)
     * 1.3.1 [Sintassi Base](#131-sintassi-base)
     * 1.3.2 [Come Funziona .map() in React](#132-come-funziona-map-in-react)
   - 1.4 [Il metodo .filter(): Selezionare e Pulire i Dati](#14-il-metodo-filter-selezionare-e-pulire-i-dati)

2. [Il Destructuring Avanzato](#2-il-destructuring-avanzato)
   - 2.1 [Cos'è il destructuring?](#cosè-il-destructuring)
   - 2.2 [Destructuring dentro la map](#destructuring-dentro-la-map)

3. [Operatori Ternari e Condizioni](#3-operatori-ternari-e-condizioni)
   - 3.1 [Usare condizioni direttamente nel render](#usare-condizioni-direttamente-nel-render)
     * 3.1.1 [Operatore ternario ( ? : )](#operatore-ternario--)

4. [Risorse e Documentazione](#4-risorse-e-documentazione)
5. [Key Takeaways del giorno](#5-key-takeaways-del-giorno)
6. [Glossario: Definizioni Istituzionali vs Spiega Brutta](#6-glossario-definizioni-istituzionali-vs-spiega-brutta)

---

# 1 Iterazioni in JSX

## 1.0 Programmazione Funzionale in React

Quando lavori con React, scoprirai rapidamente che la **programmazione funzionale** è al cuore del framework. React incoraggia fortemente l'uso di metodi array come **`.map()`** e **`.filter()`** per trasformare i dati in interfacce utente.

- **Perché?** Perché React è costruito attorno al concetto di **trasformazione dei dati**: prendi i dati, applicagli una trasformazione funzionale, ottieni JSX da renderizzare.

- **Il Paradigma:** Invece di modificare manualmente il DOM elemento per elemento (imperativo), descrivi cosa vuoi che accada ai tuoi dati (dichiarativo). React fa il resto.

**Esempio concettuale:**
```
Dati → Trasformazione Funzionale (.map() / .filter()) → JSX → UI sullo schermo
```

---

## 1.1 L'attributo key: Il Pilastro delle Liste

Quando generi liste di elementi dinamicamente in React, l'attributo **`key`** è fondamentale. Senza di esso, React non sa come tracciare gli elementi mentre cambiano.

### Perché la Key è importante?

La key serve ad aiutare React a identificare correttamente quali elementi:
- sono stati **aggiunti**
- sono stati **modificati**
- sono stati **rimossi**

durante il processo di aggiornamento del DOM virtuale (reconciliation).

Grazie alle key, React riesce a **ottimizzare il rendering** evitando aggiornamenti inutili o comportamenti inattesi.

**Analogia:** Immagina una lista di compiti. Se aggiungi un nuovo compito all'inizio, React ha bisogno di sapere quale è quale. Le key sono come i numeri di riga che evitano confusione.

### Quale valore usare come Key?

È fondamentale utilizzare sempre un **identificativo univoco e stabile**. Se gli elementi dell'array possiedono un `id`, è sempre preferibile utilizzarlo.

```jsx
const utenti = [
  { id: 1, nome: "Mario" },
  { id: 2, nome: "Luigi" }
];

function ListaUtenti() {
  return (
    <ul>
      {utenti.map(utente => (
        <li key={utente.id}>  {/* ✅ Usa l'id come key */}
          {utente.nome}
        </li>
      ))}
    </ul>
  );
}
```

### Quando evitare index come Key?

Usare l'indice dell'array (`index`) come key è **sconsigliato nelle liste dinamiche**.

Va evitato quando:
- la lista può **cambiare ordine**
- gli elementi possono essere **aggiunti o rimossi**
- la lista **non è statica**
- gli elementi contengono **stato locale o input**

In queste situazioni, usare `index` può causare:
- 🐛 bug grafici
- ❌ rendering errati
- 🔥 perdita dello stato dei componenti
- ⚙️ problemi con animazioni o input controllati

**Perché?** Se la lista viene ordinata, l'index cambia ma la key rimane la stessa, confondendo React.

---

## 1.2 Arrow Functions: Breve Reminder

Prima di approfondire `.map()`, è importante ricordare come funzionano le arrow function e due concetti cruciali: il **return implicito** e il **destructuring**.

### 1.2.1 Return Implicito vs Esplicito

Le arrow function in JavaScript offrono due sintassi diverse, con comportamenti differenti per il `return`:

**Return Esplicito (con graffe `{ }`):**
```javascript
// Devi scrivere RETURN manualmente
const numeri = [1, 2, 3];
const raddoppiati = numeri.map(numero => {
  return numero * 2;
});
```

**Return Implicito (con parentesi tonde `( )`):**
```javascript
// Return automatico, perfetto per una sola espressione
const numeri = [1, 2, 3];
const raddoppiati = numeri.map(numero => numero * 2);

// Se vuoi JSX in una sola riga, usa le parentesi:
numeri.map(numero => (
  <div>{numero}</div>
))
```

**Quando usare quale?**
- **Parentesi tonde `( )`**: Quando la funzione restituisce una sola espressione (numero, stringa, JSX).
- **Graffe `{ }`**: Quando hai logica multipla o hai bisogno di più linee di codice.

### 1.2.2 Destructuring nei Parametri

Il **destructuring** permette di estrarre valori direttamente dal parametro, rendendo il codice più leggibile.

**Senza destructuring:**
```javascript
const utenti = [
  { id: 1, nome: "Mario", eta: 30 },
  { id: 2, nome: "Luigi", eta: 25 }
];

utenti.map(utente => {
  return `${utente.nome} ha ${utente.eta} anni`;
});
// ❌ Devi scrivere 'utente.' ogni volta
```

**Con destructuring:**
```javascript
utenti.map(({ id, nome, eta }) => {
  return `${nome} ha ${eta} anni`;
});
// ✅ Estrai direttamente le proprietà che ti servono
```

**In JSX, con return implicito:**
```jsx
utenti.map(({ id, nome, eta }) => (
  <p key={id}>{nome} ha {eta} anni</p>
))
```

---

## 1.3 Il metodo .map(): Trasformare i Dati in JSX

### 1.3.1 Sintassi Base

`.map()` è un **metodo JavaScript** che trasforma ogni elemento di un array in un nuovo valore, creando un nuovo array con i risultati.

**Definizione:** Prendi un array, applica un'operazione a ogni elemento, ottieni un nuovo array con i risultati trasformati.

**Sintassi Generale:**
```javascript
array.map((elemento) => {
  // Logica: trasforma elemento
  return risultatoTrasformato;
});
```

**Componenti:**
- `array`: l'array originale
- `.map()`: il metodo che itera
- `elemento`: il singolo item durante l'iterazione
- `=>`: arrow function
- `return`: il valore trasformato da mettere nel nuovo array

**Esempi in JavaScript Puro:**

```javascript
// Esempio 1: Moltiplicare numeri
const numeri = [1, 2, 3, 4, 5];
const raddoppiati = numeri.map(num => num * 2);
console.log(raddoppiati); // [2, 4, 6, 8, 10]

// Esempio 2: Estrarre proprietà da oggetti
const persone = [
  { id: 1, nome: "Mario" },
  { id: 2, nome: "Luigi" }
];
const nomi = persone.map(persona => persona.nome);
console.log(nomi); // ["Mario", "Luigi"]

// Esempio 3: Creare nuovi oggetti
const prodotti = [
  { nome: "Laptop", prezzo: 1000 },
  { nome: "Mouse", prezzo: 25 }
];
const conSconto = prodotti.map(prodotto => ({
  ...prodotto,
  prezzoScontato: prodotto.prezzo * 0.9
}));
```

### 1.3.2 Come Funziona .map() in React

In React, `.map()` viene usato **dentro il JSX** per trasformare dati in componenti JSX renderizzabili.

**La Differenza Cruciale:** Invece di restituire numeri o stringhe, restituisci **elementi JSX**.

**Struttura Base:**

```jsx
function ListaUtenti() {
  const utenti = [
    { id: 1, nome: "Mario" },
    { id: 2, nome: "Luigi" }
  ];

  return (
    <ul>
      {utenti.map(utente => {
        return (
          <li key={utente.id}>
            {utente.nome}
          </li>
        );
      })}
    </ul>
  );
}
```

**Con Return Implicito (Più Idiomatico):**

```jsx
function ListaUtenti() {
  const utenti = [
    { id: 1, nome: "Mario" },
    { id: 2, nome: "Luigi" }
  ];

  return (
    <ul>
      {utenti.map(utente => (
        <li key={utente.id}>{utente.nome}</li>
      ))}
    </ul>
  );
}
```

**Con Destructuring (Ancora Più Leggibile):**

```jsx
function ListaUtenti() {
  const utenti = [
    { id: 1, nome: "Mario" },
    { id: 2, nome: "Luigi" }
  ];

  return (
    <ul>
      {utenti.map(({ id, nome }) => (
        <li key={id}>{nome}</li>
      ))}
    </ul>
  );
}
```

**Regole Importanti:**
- ✅ La **key deve stare sull'elemento JSX principale** restituito (nel nostro caso `<li>`)
- ✅ Usa **return implicito** (parentesi tonde) quando possibile per leggibilità
- ✅ **Destructura i parametri** per evitare di scrivere `utente.id`, `utente.nome` ripetutamente
- ✅ Metti il `.map()` **dentro le graffe `{}`** del JSX

---

## 1.4 Il metodo .filter(): Selezionare e Pulire i Dati

### Cos'è `.filter()`?

`.filter()` è un **metodo JavaScript** che crea un nuovo array contenente **solo gli elementi che soddisfano una condizione specifica**.

**Definizione:** Prendi un array, seleziona solo gli elementi per cui la funzione ritorna `true`, ottieni un nuovo array con solo quelli selezionati.

**Sintassi Generale:**
```javascript
array.filter((elemento) => {
  return condizione; // true = include, false = scarta
});
```

**Esempi in JavaScript Puro:**

```javascript
// Esempio 1: Filtrare numeri
const numeri = [1, 2, 3, 4, 5, 6];
const pari = numeri.filter(num => num % 2 === 0);
console.log(pari); // [2, 4, 6]

// Esempio 2: Filtrare oggetti per proprietà
const utenti = [
  { id: 1, nome: "Mario", attivo: true },
  { id: 2, nome: "Luigi", attivo: false },
  { id: 3, nome: "Peach", attivo: true }
];
const attivi = utenti.filter(utente => utente.attivo === true);
console.log(attivi); 
// [
//   { id: 1, nome: "Mario", attivo: true },
//   { id: 3, nome: "Peach", attivo: true }
// ]

// Esempio 3: Filtrare stringhe
const parole = ["ciao", "react", "a", "javascript"];
const lunghe = parole.filter(parola => parola.length > 2);
console.log(lunghe); // ["ciao", "react", "javascript"]
```

### Usare `.filter()` in React

In React, spesso combini `.filter()` con `.map()` per **pulire i dati prima di renderizzarli**:

```jsx
function ListaUtentiAttivi() {
  const utenti = [
    { id: 1, nome: "Mario", attivo: true },
    { id: 2, nome: "Luigi", attivo: false },
    { id: 3, nome: "Peach", attivo: true }
  ];

  return (
    <ul>
      {utenti
        .filter(utente => utente.attivo === true)  // Step 1: Filtra
        .map(({ id, nome }) => (                     // Step 2: Trasforma in JSX
          <li key={id}>{nome} ✅</li>
        ))}
    </ul>
  );
}
```

**Come Leggerlo:**
1. Prendi l'array `utenti`
2. **Filtra** solo quelli con `attivo === true`
3. **Mappa** gli utenti filtrati e trasformali in `<li>` JSX
4. Renderizza solo gli utenti attivi

**Vantaggi di questa approccio:**
- ✅ La logica è **separata e leggibile**
- ✅ Non modifichi l'array originale
- ✅ Puoi **chainare** metodi funzionali
- ✅ Il codice è **facilmente testabile**

---

# 2 Operatori Ternari e Condizioni

## Usare condizioni direttamente nel render

A volte non serve filtrare l'intero array. Possiamo invece inserire condizioni direttamente dentro la `.map()`. Uno dei modi più comuni è usare l'**operatore ternario**.

### Operatore Ternario (? :)

Il ternario è una forma compatta di `if/else` in una sola riga.

**Sintassi:**

```javascript
condizione ? valoreSeVero : valoreSeFalso
```

**Esempio pratico:**

```jsx
utenti.map(({ id, nome, online }) => (
  <p key={id}>
    {nome}: {online ? '🟢 Online' : '⚫ Offline'}
  </p>
))
```

In questo caso:
- se `online` è `true` → mostra "🟢 Online"
- altrimenti → mostra "⚫ Offline"

**Caso più complesso:**

```jsx
{utenti.map(({ id, ruolo, nome }) => (
  <div key={id} className={ruolo === 'admin' ? 'admin-badge' : 'user-badge'}>
    {nome} - {ruolo === 'admin' ? '👑 Amministratore' : '👤 Utente'}
  </div>
))}
```

---

# 4 Risorse e Documentazione
- [Rendering Lists](https://react.dev/learn/rendering-lists)
- [Le Keys](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key)
- [Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring)
- [Filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [Operatore Ternario](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator)
- [Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

# 5 Key Takeaways del giorno

1. **Programmazione Funzionale è il Core di React** — `.map()` e `.filter()` non sono opzionali, sono il modo idioomatico di trasformare dati in interfacce in React.

2. **La `key` è obbligatoria, non facoltativa** — Assegna sempre una `key` al primo elemento JSX restituito da ogni iterazione di `.map()`. Usa un identificativo univoco e stabile (come `id`), mai index in liste dinamiche.

3. **Arrow Functions: Return Implicito** — Usa parentesi tonde `()` per il return automatico quando hai una sola espressione. È più conciso e leggibile.

4. **Destructuring dei Parametri** — Estrai direttamente le proprietà che ti servono: `.map(({ id, nome }) => ...)` è meglio di `.map(utente => ...)`.

5. **Chain `.filter()` e `.map()` insieme** — Filtra prima i dati con `.filter()`, poi trasformali in JSX con `.map()`. Questo mantiene la logica separata e il codice più pulito.

6. **Operatore Ternario per Logica Inline** — Usa `condizione ? valoreSeVero : valoreSeFalso` dentro il JSX per rendere contenuto diverso senza bisogno di if/else complessi.

7. **Non modificare Array Originali** — `.map()` e `.filter()` creano nuovi array, non modificano l'originale. Questo è il paradigma funzionale che React predilige.

---

# 6 Glossario: Definizioni Istituzionali vs Spiega Brutta

| Termine Istituzionale | Definizione Formale | "Spiega Brutta" (In pratica) |
|---|---|---|
| **Programmazione Funzionale** | Paradigma di programmazione che enfatizza funzioni pure, immutabilità e trasformazioni di dati. | Scrivere codice usando funzioni che trasformano dati senza cambiarli (`.map()`, `.filter()`, ecc). |
| **.map()** | Metodo array che trasforma ogni elemento in un nuovo valore, creando un nuovo array. | Prendi un array, fai qualcosa con ogni elemento, e ottieni un nuovo array. In React, lo usi per trasformare dati in JSX. |
| **.filter()** | Metodo array che seleziona solo elementi che soddisfano una condizione. | Crea un nuovo array contenendo solo gli elementi per cui la funzione ritorna `true`. Utile per pulire i dati. |
| **key** | Prop speciale che aiuta React a identificare elementi unici in una lista. | Un'etichetta univoca che dici a React "questo elemento è sempre lo stesso, anche se si muove". Evita bug. |
| **Return Implicito** | Return automatico quando usi parentesi tonde in arrow function. | `() => (<div>contenuto</div>)` ritorna automaticamente il JSX senza scrivere `return`. |
| **Return Esplicito** | Return scritto manualmente con la parola chiave `return`. | `() => { return <div>contenuto</div> }`. Più verboso ma necessario se hai più logica dentro la funzione. |
| **Destructuring** | Tecnica per estrarre valori da oggetti o array. | Prendi un oggetto con molte proprietà e "scarichi" solo quelle che ti servono in variabili separate. Rende il codice più leggibile. |
| **Reconciliation** | Processo di aggiornamento del DOM virtuale in React. | React confronta il vecchio DOM virtuale con il nuovo e aggiorna solo le parti che sono cambiate. |
| **Arrow Function** | Sintassi moderna di JavaScript per definire funzioni. | Un modo compatto di scrivere funzioni: `(parametro) => risultato`. Più breve e leggibile delle funzioni tradizionali. |
| **Operatore Ternario** | Operatore condizionale compatto: `condizione ? se vero : se falso`. | Un if/else in una sola riga. Se la condizione è vera, usa il primo valore; altrimenti usa il secondo. |
| **JSX** | Sintassi che permette di scrivere strutture HTML in file JS. | Un modo per scrivere "HTML dentro JavaScript" per costruire la pagina in modo più semplice e leggibile. |
| **Immutabilità** | Principio per cui i dati non vengono modificati direttamente. | Una volta creato un valore, non lo cambi; invece, crei un nuovo valore. React adora questo approccio. |
