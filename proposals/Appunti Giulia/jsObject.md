# JavaScript Objects

## Cos'è un Oggetto

In JavaScript, un **oggetto** (`object`) è una struttura dati utilizzata per memorizzare più informazioni relative a una singola entità all'interno dello stesso contenitore.

A differenza dei tipi di dato semplici, che contengono un solo valore, un oggetto aggrega diverse informazioni correlate tramite coppie **chiave/valore** chiamate **proprietà**.

L'accesso ai dati avviene tramite il nome della chiave e non tramite la posizione.

Gli oggetti permettono di raggruppare diverse proprietà che descrivono le caratteristiche di un elemento complesso.

---

## Differenza tra Array e Oggetti

### Array

Un array è una lista ordinata di dati a cui si accede tramite un indice numerico (`0`, `1`, `2`, ...).

È la scelta migliore quando:

- l'ordine degli elementi è importante;
- si gestiscono liste di dati.

### Esempio

```js
const colori = ["rosso", "blu", "verde"];

console.log(colori[0]); // rosso
```

### Oggetti

Un oggetto contiene coppie chiave/valore.

Ogni valore è associato a una chiave specifica che lo identifica univocamente all'interno dell'oggetto.

Non è importante l'ordine delle informazioni, ma il nome della chiave utilizzata per recuperarle.

### Esempio

```js
const utente = {
  nome: "Luca",
  eta: 25
};

console.log(utente.nome); // Luca
```

### Riepilogo

| Array | Oggetti |
|---------|---------|
| Accesso tramite indice numerico | Accesso tramite chiave |
| Mantengono un ordine | Descrivono un'entità |
| Ideali per liste | Ideali per dati correlati |

---

## Sintassi

Per creare un oggetto letterale:

1. Dichiarare una variabile (`const` o `let`);
2. Aprire le parentesi graffe `{}`;
3. Inserire coppie `chiave: valore` separate da virgole.

### Esempio

```js
const utente = {
  nome: "Luca",
  eta: 25,
  isIscritto: true
};
```

---

## Proprietà e Metodi

Un oggetto può contenere:

- **Proprietà** → dati;
- **Metodi** → funzioni.

### Proprietà

```js
const utente = {
  nome: "Luca",
  eta: 25
};
```

### Metodi

```js
const utente = {
  nome: "Luca",

  saluta() {
    console.log(`Ciao, sono ${this.nome}`);
  }
};

utente.saluta(); // Ciao, sono Luca
```

### La parola chiave `this`

All'interno di un metodo, `this` fa riferimento all'oggetto che ha invocato il metodo.

```js
console.log(this.nome);
```

equivale a:

```js
console.log(utente.nome);
```

quando il metodo viene chiamato tramite `utente.saluta()`.

---

## Funzioni come Oggetti

Le funzioni in JavaScript sono valori a tutti gli effetti e possono essere:

- assegnate a variabili;
- salvate in proprietà di oggetti;
- passate come argomenti ad altre funzioni.

### Esempio di Callback

```js
button.addEventListener("click", function () {
  console.log("Hai cliccato!");
});
```

La funzione viene eseguita solo quando si verifica l'evento.

### Scope delle Funzioni

Le variabili dichiarate all'interno di una funzione appartengono al suo **scope locale**.

```js
function esempio() {
  const nome = "Luca";
}

console.log(nome); // Errore
```

---

## Dot Notation e Bracket Notation

Esistono due modi per accedere alle proprietà di un oggetto.

### Dot Notation

```js
utente.nome;
```

Risultato:

```js
"Luca"
```

### Bracket Notation

```js
utente["eta"];
```

Risultato:

```js
25
```

### Quando usare la Bracket Notation

Quando la chiave è contenuta in una variabile:

```js
const chiave = "nome";

console.log(utente[chiave]);
```

---

## Proprietà Inesistenti

Se si tenta di accedere a una proprietà che non esiste, JavaScript restituisce `undefined`.

```js
console.log(utente.cognome);
```

Risultato:

```js
undefined
```

---

## Iterazione con `for...in`

Per scorrere tutte le proprietà senza conoscerne a priori il numero o il nomedi un oggetto, si utilizza il ciclo `for...in`.

### Esempio

```js
const utente = {
  nome: "Luca",
  eta: 25,
  citta: "Torino"
};

for (const chiave in utente) {
  console.log(chiave, utente[chiave]);
}
```

Output:

```txt
nome Luca
eta 25
citta Torino
```

---

## Oggetti Annidati (Nested Objects)

Un oggetto può contenere altri oggetti.

### Esempio

```js
const utente = {
  nome: "Luca",

  indirizzo: {
    citta: "Torino",
    cap: "10100"
  }
};
```

### Accesso alle Proprietà Interne

```js
console.log(utente.indirizzo.citta);
```

Risultato:

```js
Torino
```

---

## Recap delle Strutture Dati

### Variabili Semplici

Per un singolo valore.

```js
let nome = "Luca";
```

### Oggetti

Per rappresentare una singola entità con più caratteristiche.

```js
const utente = {
  nome: "Luca",
  eta: 25
};
```

### Array

Per gestire liste ordinate.

```js
const colori = ["rosso", "blu", "verde"];
```

### Combinazioni

Array di oggetti:

```js
const utenti = [
  { nome: "Luca" },
  { nome: "Anna" }
];
```

Oggetti che contengono array:

```js
const utente = {
  nome: "Luca",
  hobby: ["sport", "musica"]
};
```

---

# Semplificazioni Sintattiche

## Property Shorthand

Quando il nome della chiave coincide con quello della variabile, è possibile scrivere una sintassi abbreviata.

### Senza Shorthand

```js
const nome = "Luca";
const eta = 25;

const utente = {
  nome: nome,
  eta: eta
};
```

### Con Shorthand

```js
const nome = "Luca";
const eta = 25;

const utente = {
  nome,
  eta
};
```

---

## Operatore Spread (`...`)

Permette di copiare o unire oggetti.

### Copiare un Oggetto

```js
const utente = {
  nome: "Luca",
  eta: 25
};

const copia = {
  ...utente
};
```

### Unire Oggetti

```js
const datiBase = {
  nome: "Luca"
};

const datiExtra = {
  eta: 25
};

const utente = {
  ...datiBase,
  ...datiExtra
};
```

Risultato:

```js
{
  nome: "Luca",
  eta: 25
}
```

---

## Parametro Rest

Permette di raccogliere le proprietà rimanenti in un nuovo oggetto.

### Esempio

```js
const utente = {
  nome: "Luca",
  eta: 25,
  citta: "Torino"
};

const { nome, ...altreInfo } = utente;
```

Risultato:

```js
nome; // Luca

altreInfo;
// {
//   eta: 25,
//   citta: "Torino"
// }
```

---

## Destructuring

La destrutturazione consente di estrarre proprietà da un oggetto e salvarle direttamente in variabili.

### Sintassi

```js
const { nomeDellaChiave } = nomeOggetto;
```

### Esempio

```js
const utente = {
  nome: "Luca",
  eta: 25
};

const { nome, eta } = utente;
```

Ora possiamo usare direttamente:

```js
console.log(nome);
console.log(eta);
```

senza scrivere ogni volta:

```js
utente.nome;
utente.eta;
```

### Vantaggi

- codice più leggibile;
- meno ripetizioni;
- accesso più rapido alle proprietà.
---

### Key Takeaway
- Un oggetto rappresenta una singola entità.
- Gli oggetti sono formati da coppie chiave/valore.
- Array e oggetti hanno scopi diversi.
- Gli **array** rappresentano liste ordinate.
- Gli **oggetti** rappresentano entità con caratteristiche.
- Le proprietà si possono leggere in due modi (dot notation e bracket notation).
- Un oggetto può contenere funzioni (metodi).
- Le proprietà possono essere aggiunte o modificate.
- Se una proprietà non esiste viene restituito `undefined`.
- Gli oggetti possono contenere altri oggetti.
- Possono esserci anche array di oggetti o oggetti che contengono array.
- Per scorrere tutte le proprietà si usa `for...in`
- Spread e Rest utilizzano entrambi `...`
- Il Destructuring estrae proprietà in variabili

# Glossario

| Termine | Definizione |
|----------|------------|
| Object | Struttura dati che raggruppa informazioni tramite coppie chiave/valore. |
| Proprietà | Una coppia chiave/valore presente in un oggetto. |
| Chiave (Key) | Nome utilizzato per identificare un valore all'interno di un oggetto. |
| Valore (Value) | Informazione associata a una chiave. |
| Metodo | Funzione definita all'interno di un oggetto. |
| Dot Notation | Accesso alle proprietà tramite il punto (`utente.nome`). |
| Bracket Notation | Accesso alle proprietà tramite parentesi quadre (`utente["nome"]`). |
| this | Riferimento all'oggetto corrente. |
| Nested Object | Oggetto contenuto all'interno di un altro oggetto. |
| Scope | Ambito di visibilità di variabili e funzioni. |
| Callback | Funzione passata come argomento ad un'altra funzione. |
| for...in | Ciclo utilizzato per iterare le proprietà di un oggetto. |
| Property Shorthand | Sintassi abbreviata per creare proprietà quando chiave e variabile hanno lo stesso nome. |
| Spread Operator (`...`) | Operatore che copia o espande proprietà di oggetti o elementi di array. |
| Rest Operator (`...`) | Operatore che raccoglie gli elementi rimanenti in un nuovo oggetto o array. |
| Destructuring | Tecnica che permette di estrarre proprietà da un oggetto in nuove variabili. |
| undefined | Valore restituito quando una proprietà non esiste o non è definita. |

---

## Formula Mentale da Ricordare

```txt
Variabile → un valore

Array → una lista di valori

Oggetto → un'entità con più caratteristiche

Array di oggetti → una lista di entità
```

Esempio:

```js
const utenti = [
  {
    nome: "Luca",
    eta: 25
  },
  {
    nome: "Anna",
    eta: 30
  }
];
```