#### Modulo: React
**Titolo:** Iterazioni in JSX

**📍 Indice Rapido**

1. [Iterazioni in JSX](#1-iterazioni-in-jsx)
  - 1.1 [La key (da non dimenticare mai)](#la-key-da-non-dimenticare-mai)
    * 1.1.1 [Perché la Key è importante?](#perché-la-key-è-importante)
    * 1.1.2 [Quale valore usare come Key](#quale-valore-usare-come-key)
    * 1.1.3 [Quando evitare index come Key](#quando-evitare-index-come-key)
  - 1.2 [Il return](#il-return)
    * 1.2.1 [return implicito](#return-implicito)
    * 1.2.2 [return esplicito](#return-esplicito)
  - 1.3 [Riassunto Finale](#riassunto-finale)

2. [Il Destructuring](#2-destructuring)
  - 2.1 [Cos'è il destructuring?](#cosè-il-destructuring)
  - 2.2 [Destructuring dentro la map](#destructuring-dentro-la-map)

3. [Filter e operatori ternari](#3-filter-e-operatori-ternari)
  - 3.1 [Il metodo .filter()](#il-metodo-filter)
  - 3.2 [Usare condizioni direttamente nel render](#usare-condizioni-direttamente-nel-render)
    * 3.2.1[Operatore ternario ( ? : )](#operatore-ternario--)

4. [Risorse e Documentazione](#4-risorse-e-documentazione)
5. [Key Takeaways del giorno](#5-key-takeaways-del-giorno)
6. [Glossario: Definizioni Istituzionali vs Spiega Brutta](#6-glossario-definizioni-istituzionali-vs-spiega-brutta)




# 1 Iterazioni in JSX

In JSX, le iterazioni vengono gestite inserendo codice JavaScript all'interno delle parentesi graffe `{}`. Il metodo più comune ed efficiente per generare liste di elementi è utilizzare la funzione `.map()` di JavaScript, che permette di ciclare un array e restituire un nuovo array di elementi JSX.

Ci sono alcune regole fondamentali da ricordare quando si utilizza `.map()` in React.

## La key (da non dimenticare mai)

Quando si genera una lista di elementi, la key va assegnata all'elemento JSX principale restituito da ogni iterazione della map, cioè al primo elemento JSX renderizzato all'interno della `.map()`.

```jsx
const users = [
  { id: 1, name: "Mario" },
  { id: 2, name: "Luigi" }
];

function List() {
  return (
    <ul>
      {users.map(user => {
        return (
          <li key={user.id}>
            {user.name}
          </li>
        );
      })}
    </ul>
  );
}
```

### Perché la key è importante?

La key serve ad aiutare React a identificare correttamente quali elementi:
- sono stati aggiunti
- sono stati modificati
- sono stati rimossi

durante il processo di aggiornamento del DOM virtuale (reconciliation).
Grazie alle key, React riesce a ottimizzare il rendering evitando aggiornamenti inutili o comportamenti inattesi.

### Quale valore usare come key?

È fondamentale utilizzare sempre un identificativo univoco e stabile. Se gli elementi dell'array possiedono un id, è sempre preferibile utilizzare quello.

### Quando evitare index come key?

Usare l'indice dell'array (`index`) come key è sconsigliato nelle liste dinamiche.

Va evitato quando:
- la lista può cambiare ordine
- gli elementi possono essere aggiunti o rimossi
- la lista non è statica
- gli elementi contengono stato locale o input

In queste situazioni, usare index può causare:
- bug grafici
- rendering errati
- perdita dello stato dei componenti
- problemi con animazioni o input controllati

## Il return

Quando si utilizza `.map()`, bisogna ricordarsi di restituire il JSX. Esistono due modalità:

### Return implicito

Se usiamo le parentesi tonde `()`, il return è automatico:

```jsx
users.map(user => (
  <Card key={user.id} />
))
```

### Return esplicito

Se usiamo le parentesi graffe `{}`, dobbiamo scrivere manualmente `return` (altrimenti restituisce `undefined`):

```jsx
users.map(user => {
  return <Card key={user.id} />
})
```
## Riassunto finale

Quando utilizziamo `.map()` in React:
- inseriamo il codice JavaScript dentro `{}` in JSX
- ogni elemento della lista deve avere una key
- la key va assegnata all'elemento root restituito dalla callback all'interno della `.map()`
- è preferibile usare un id univoco e stabile
- evitare index nelle liste dinamiche
- ricordarsi sempre di fare il return del JSX (se non implicito)

Queste regole aiutano React a gestire il rendering in modo corretto, efficiente e senza bug.

# 2 Destructuring

Quando utilizziamo `.map()` per ciclare questi dati, potremmo accedere alle proprietà dell'oggetto in questo modo:

```jsx
users.map(user => (
  <p key={user.id}>{user.name}</p>
))
```

Questo approccio è corretto, ma quando gli oggetti contengono molte proprietà, scrivere continuamente `user.nomeProprietà` può rendere il codice più lungo e meno leggibile.

Per questo motivo, in React e JavaScript moderno si utilizza spesso il destructuring direttamente nei parametri della `.map()`.

## Cos'è il destructuring?

Il destructuring permette di "estrarre" le proprietà di un oggetto e salvarle in variabili più comode da utilizzare.

Esempio con variabile:

```jsx
const user = {
  id: 1,
  name: 'Mario'
}
const { id, name } = user
```

Ora possiamo usare direttamente `id` e `name` senza scrivere `user.id` o `user.name`.

## Destructuring dentro la .map()

Possiamo fare questa operazione direttamente nei parametri della funzione:

```jsx
users.map(({ id, name }) => (
  <p key={id}>{name}</p>
))
```

# 3 Filter e operatori ternari

Prima di mostrare i dati in React, possiamo "pulirli" usando `.filter()` per selezionare solo gli elementi necessari. Inoltre, dentro la `.map()`, possiamo utilizzare condizioni rapide come l'operatore ternario per cambiare il contenuto renderizzato in base a una determinata situazione.

## Il metodo .filter()

Il metodo `.filter()` serve a creare un nuovo array contenendo solo gli elementi che rispettano una determinata condizione.

**Esempio:**

```jsx
const numbers = [1, 2, 3, 4, 5];

numbers
  .filter(number => number > 2)
  .map(number => (
    <p key={number}>{number}</p>
  ))
```

In questo caso, `.filter()` controlla ogni numero dell'array e mantiene solo quelli maggiori di 2, quindi `.map()` trasforma ogni numero in un elemento JSX da mostrare a schermo.

## Usare condizioni direttamente nel render

A volte non serve filtrare l'intero array. Possiamo invece inserire condizioni direttamente dentro la `.map()`. Uno dei modi più comuni è usare l'operatore ternario.

### Operatore ternario (? :)

Il ternario è una forma compatta di if/else.

**Sintassi:**

```
condizione ? valoreSeVero : valoreSeFalso
```

**Esempio pratico:**

```jsx
users.map(user => (
  <p key={user.id}>
    {user.online ? 'Online' : 'Offline'}
  </p>
))
```

In questo caso:
- se `user.online` è `true` → mostra "Online"
- altrimenti → mostra "Offline"

# 4 Risorse e Documentazione
- [Rendering Lists](https://react.dev/learn/rendering-lists)
- [Le Keys](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key)
- [Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring)
- [Filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [Operatore Ternario](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

# 5 Key Takeaways del giorno

1. **`.map()` è il tuo amico per le liste** — Quando devi renderizzare una lista di elementi, `.map()` trasforma un array di dati in un array di componenti JSX. È il metodo standard in React.

2. **La `key` è obbligatoria, non facoltativa** — Assegna sempre una `key` al primo elemento JSX restituito da ogni iterazione di `.map()`. Usa un identificativo univoco e stabile (come `id`), mai l'indice della posizione.

3. **Evita `index` come key nelle liste dinamiche** — Se gli elementi possono cambiare ordine, essere aggiunti o rimossi, usare `index` crea bug grafici e comportamenti inattesi. React non riuscirà a tenere traccia correttamente degli elementi.

4. **Return implicito vs esplicito** — Con parentesi tonde `()` il return è automatico; con parentesi graffe `{}` devi scrivere `return` manualmente. Entrambi sono validi, scegli lo stile che preferisci.

5. **Destructuring rende il codice più leggibile** — Invece di scrivere `user.id` e `user.name` continuamente, estrai le proprietà direttamente nei parametri: `.map(({ id, name }) => ...)`.

6. **Chain `.filter()` e `.map()` insieme** — Filtra prima i dati con `.filter()`, poi trasformali in JSX con `.map()`. Questo mantiene la logica separata e il codice più pulito.

7. **L'operatore ternario è perfetto per condizioni inline** — Usa `condizione ? valoreSeVero : valoreSeFalso` dentro il JSX per rendere contenuto diverso in base a una condizione, senza bisogno di if/else separati.

# 6 Glossario: Definizioni Istituzionali vs Spiega Brutta

| Termine Istituzionale | Definizione Formale                                          | “Spiega Brutta” (In pratica)                                                                                                       |
| --------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **JSX**               | Sintassi che permette di scrivere strutture HTML in file JS. | Un modo per scrivere “HTML dentro JavaScript” per costruire la pagina in modo più semplice e leggibile.                            |
| **Expression**        | Codice JS valutato dentro le parentesi graffe.               | Un pezzo di JavaScript che React calcola al volo e trasforma in un valore da mostrare nella pagina (es. testo, numero, risultato). |
| **Fragment**          | `<> ... </>`                                                 | Un contenitore invisibile che serve a raggruppare più elementi senza creare un tag extra nel DOM.                                  |
| **Componente**        | Funzione JavaScript che restituisce JSX.                     | Un pezzo della pagina che puoi riusare: prende dati e restituisce cosa deve essere mostrato a schermo.                             |
| **.map()**            | Metodo array che trasforma ogni elemento in un nuovo valore. | Prendi un array, fai qualcosa con ogni elemento, e ottieni un nuovo array. In React, lo usi per trasformare dati in componenti.   |
| **key**               | Prop speciale che aiuta React a identificare elementi unici.  | Un'etichetta univoca che dici a React "questo elemento è sempre lo stesso, anche se si muove". Evita bug quando la lista cambia.   |
| **Reconciliation**    | Processo di aggiornamento del DOM virtuale in React.          | React confronta il vecchio DOM virtuale con il nuovo e aggiorna solo le parti che sono cambiate. È come un diff intelligente.     |
| **Destructuring**     | Tecnica per estrarre valori da oggetti o array.               | Prendi un oggetto con molte proprietà e "scarichi" solo quelle che ti servono in variabili separate. Rende il codice più pulito.  |
| **.filter()**         | Metodo array che seleziona solo elementi che soddisfano una condizione. | Crea un nuovo array contenendo solo gli elementi per cui la funzione ritorna `true`. Utile per pulire i dati prima di renderizzare. |
| **Operatore ternario**| Operatore condizionale compatto: `condizione ? se vero : se falso`. | Un if/else in una sola riga. Se la condizione è vera, usa il primo valore; altrimenti usa il secondo.                               |
| **Return implicito**  | Return automatico quando usi parentesi tonde in arrow function. | `() => <div>contenuto</div>` ritorna automaticamente il JSX senza scrivere `return`. Pratico e leggibile.                          |
| **Return esplicito**  | Return scritto manualmente con la parola chiave `return`.     | `() => { return <div>contenuto</div> }`. Più verboso ma a volte necessario se hai più logica dentro la funzione.                  |
