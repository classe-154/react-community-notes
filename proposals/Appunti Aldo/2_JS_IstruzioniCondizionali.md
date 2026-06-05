# 📘 Appunti JavaScript — Istruzioni Condizionali
> Modulo: Web Development — Definizione di Condizione; Operatori Relazionali; Operatori Logici; Operatori Binari

---

## 📍 Indice

5. [Definizione di Condizione](#5-definizione-di-condizione)
6. [Operatori Relazionali](#6-operatori-relazionali)
7. [Operatori Logici](#7-operatori-logici)
8. [Operatori Binari (Bitwise)](#8-operatori-binari-bitwise) — *⚠️ Approfondimento fuori programma*

---

## 5. Definizione di Condizione

### Cos'è una condizione

Una **condizione** è un'espressione che JavaScript valuta e che restituisce sempre un valore **booleano**: `true` (vero) o `false` (falso). Serve a controllare il flusso del programma: in base al risultato, JS decide quale parte di codice eseguire.

La struttura base è `if / else if / else`:

```js
if (condizione) {
  // Eseguito se la condizione è TRUE
} else if (altraCondizione) {
  // Eseguito se la prima è FALSE ma questa è TRUE
} else {
  // Eseguito se tutte le condizioni precedenti sono FALSE
}
```

### Esempio base

```js
let eta = 20;

if (eta >= 18) {
  console.log("Sei maggiorenne. Accesso consentito.");
} else {
  console.log("Sei minorenne. Accesso negato.");
}
// → "Sei maggiorenne. Accesso consentito."
```

### Esempio pratico — Sistema di Valutazione

```js
let voto = 75;

if (voto >= 90) {
  console.log("Ottimo! Voto: A");
} else if (voto >= 75) {
  console.log("Buono! Voto: B");
} else if (voto >= 60) {
  console.log("Sufficiente. Voto: C");
} else {
  console.log("Insufficiente. Ripeti l'esame.");
}
// → "Buono! Voto: B"
```

### L'operatore ternario (forma compatta)

Per condizioni semplici esiste una sintassi abbreviata su una riga:

```
condizione ? valoreSeVero : valoreSefalso
```

```js
let eta = 20;
let accesso = (eta >= 18) ? "Consentito" : "Negato";
console.log(accesso); // → "Consentito"
```

### Valori "truthy" e "falsy"

In JavaScript, non solo `true` e `false` sono usati nelle condizioni. Ogni valore viene valutato come "veritiero" o "falso":

| Falsy (→ `false`) | Truthy (→ `true`) |
|---|---|
| `false` | `true` |
| `0`, `-0` | Qualsiasi numero diverso da 0 |
| `""` (stringa vuota) | Qualsiasi stringa non vuota |
| `null` | Oggetti e array |
| `undefined` | |
| `NaN` | |

```js
let username = "";

if (username) {
  console.log("Benvenuto, " + username);
} else {
  console.log("Inserisci un nome utente."); // → questo viene eseguito (stringa vuota = falsy)
}
```

### ⚠️ Errori comuni

- **Usare `=` invece di `==` o `===`:** `if (x = 5)` assegna 5 a x invece di confrontarli. Usare sempre `==` o `===` nei confronti.
- **Dimenticare le parentesi graffe `{}`:** Per blocchi con più istruzioni, le `{}` sono obbligatorie.

---

## 6. Operatori Relazionali

### Cosa sono

Gli **operatori relazionali** (o di confronto) confrontano due valori e restituiscono sempre un **booleano** (`true` o `false`). Sono le fondamenta delle condizioni.

### Tabella completa

| Operatore | Significato | Esempio | Risultato |
|---|---|---|---|
| `==` | Uguale (solo valore) | `5 == "5"` | `true` |
| `===` | Strettamente uguale (valore E tipo) | `5 === "5"` | `false` |
| `!=` | Diverso (solo valore) | `5 != "5"` | `false` |
| `!==` | Strettamente diverso (valore E tipo) | `5 !== "5"` | `true` |
| `>` | Maggiore | `10 > 5` | `true` |
| `<` | Minore | `3 < 2` | `false` |
| `>=` | Maggiore o uguale | `5 >= 5` | `true` |
| `<=` | Minore o uguale | `4 <= 3` | `false` |

### `==` vs `===` — La differenza fondamentale

```js
let numero = 5;
let testo = "5";

console.log(numero == testo);  // true  → confronta solo il VALORE (5 == 5)
console.log(numero === testo); // false → confronta VALORE e TIPO (Number ≠ String)
```

> **Regola d'oro:** Usa **sempre `===`** e `!==` nei tuoi confronti. Evitano conversioni implicite di tipo che possono generare bug difficili da trovare.

### Esempio pratico

```js
let etaUtente = 17;
const etaMinima = 18;
const etaMassima = 65;

console.log(etaUtente >= etaMinima);  // false → non è maggiorenne
console.log(etaUtente < etaMassima);  // true  → è nella fascia di età
console.log(etaUtente === 17);        // true  → corrispondenza esatta
console.log(etaUtente !== etaMinima); // true  → la sua età è diversa da 18
```

### ⚠️ Errori comuni

- **Confondere `=` (assegnazione) con `==` (confronto):** `if (x = 10)` non confronta, assegna e provoca bug.
- **Usare `==` invece di `===`:** `0 == false` restituisce `true` a causa della conversione implicita. Con `===` restituisce `false` come ci si aspetterebbe.

---

## 7. Operatori Logici

### Cosa sono

Gli **operatori logici** permettono di combinare più condizioni insieme per creare espressioni booleane più complesse. Sono essenziali per controllare il flusso del programma.

### I tre operatori

| Operatore | Nome | Descrizione | Esempio |
|---|---|---|---|
| `&&` | AND (E) | `true` solo se **entrambe** le condizioni sono vere | `a && b` |
| `\|\|` | OR (O) | `true` se **almeno una** delle condizioni è vera | `a \|\| b` |
| `!` | NOT (NON) | **Inverte** il valore booleano | `!a` |

### `&&` — AND: entrambe vere

```js
let eta = 25;
let haPatente = true;

if (eta >= 18 && haPatente) {
  console.log("Puoi guidare!"); // → viene eseguito: entrambe le condizioni sono true
} else {
  console.log("Non puoi guidare.");
}
```

### `||` — OR: almeno una vera

```js
let staPiovendo = false;
let staNevicando = true;

if (staPiovendo || staNevicando) {
  console.log("Prendi qualcosa per copriti!"); // → viene eseguito: staNevicando è true
} else {
  console.log("Il tempo è bello oggi.");
}
```

### `!` — NOT: inverte il valore

```js
let loggedIn = false;

if (!loggedIn) {
  console.log("Devi effettuare il login."); // → viene eseguito: !false = true
}
```

### Combinare più operatori

```js
let eta = 22;
let haPatente = true;
let haAlcool = false;

// Può guidare se: (è maggiorenne E ha la patente) E NON ha bevuto alcool
if (eta >= 18 && haPatente && !haAlcool) {
  console.log("Puoi guidare in sicurezza.");
} else {
  console.log("Non puoi guidare.");
}
```

> **Consiglio:** Usa le parentesi `()` per rendere più leggibile l'ordine di valutazione quando combini più operatori.

### Short-circuit evaluation (valutazione cortocircuito)

JavaScript è "pigro": smette di valutare le condizioni non appena conosce il risultato finale.

- Con `&&`: se la **prima** condizione è `false`, la seconda non viene valutata (il risultato sarà comunque `false`)
- Con `||`: se la **prima** condizione è `true`, la seconda non viene valutata (il risultato sarà comunque `true`)

### ⚠️ Errori comuni

- **Confondere `&&` con `||`:** `&&` richiede che TUTTE le condizioni siano vere; `||` basta che UNA lo sia.
- **Dimenticare `!` inverte il tipo:** `!0` è `true`, `!"ciao"` è `false`. Usare con attenzione.

### Glossario

| Termine | Definizione | "Spiegazione Brutta" |
|---|---|---|
| **AND (`&&`)** | Restituisce `true` solo se entrambi gli operandi sono veri | "E": tutte le condizioni devono essere soddisfatte |
| **OR (`\|\|`)** | Restituisce `true` se almeno un operando è vero | "O": basta che almeno una condizione sia soddisfatta |
| **NOT (`!`)** | Inverte il valore booleano dell'operando | Il "contrario": trasforma vero in falso e viceversa |

---

## 8. Operatori Binari (Bitwise)

> ⚠️ **APPROFONDIMENTO FUORI PROGRAMMA** — Questo argomento non fa parte del programma obbligatorio del corso. È incluso come approfondimento per chi vuole capire come JavaScript lavora a basso livello.

### Cosa sono

Gli **operatori binari** (o *bitwise*) operano direttamente sulla rappresentazione **binaria** (in bit: 0 e 1) dei numeri interi. JavaScript converte ogni numero in un intero a 32 bit, esegue l'operazione bit per bit, e restituisce il risultato come numero decimale.

> **Quando si usano?** Sono meno comuni nello sviluppo web quotidiano, ma tornano utili in: manipolazione di permessi, flag di configurazione, compressione dati, grafica, crittografia e operazioni a basso livello.

### Concetto base: come funziona il binario

Ogni numero decimale ha una rappresentazione in binario (solo 0 e 1):

| Decimale | Binario |
|---|---|
| 0 | `0000` |
| 1 | `0001` |
| 5 | `0101` |
| 9 | `1001` |
| 12 | `1100` |

### Tabella degli operatori

| Operatore | Nome | Descrizione |
|---|---|---|
| `&` | AND Bitwise | Restituisce `1` solo se **entrambi** i bit sono `1` |
| `\|` | OR Bitwise | Restituisce `1` se **almeno uno** dei bit è `1` |
| `^` | XOR Bitwise | Restituisce `1` se i bit sono **diversi** |
| `~` | NOT Bitwise | **Inverte** tutti i bit (complemento a uno) |
| `<<` | Shift Sinistro | Sposta i bit a sinistra (moltiplica per 2) |
| `>>` | Shift Destro | Sposta i bit a destra (divide per 2) |
| `>>>` | Shift Destro Senza Segno | Shift destro, riempie con `0` a sinistra |

### AND Bitwise `&`

Restituisce `1` solo dove **entrambi** i bit corrispondenti sono `1`:

```js
// 5  = 0101
// 1  = 0001
// &  = 0001 → 1

console.log(5 & 1); // → 1
console.log(5 & 4); // → 4  (0101 & 0100 = 0100)
```

### OR Bitwise `|`

Restituisce `1` dove **almeno uno** dei bit è `1`:

```js
// 5  = 0101
// 1  = 0001
// |  = 0101 → 5

console.log(5 | 1); // → 5
console.log(5 | 2); // → 7  (0101 | 0010 = 0111)
```

### XOR Bitwise `^`

Restituisce `1` dove i bit sono **diversi**:

```js
// 5  = 0101
// 1  = 0001
// ^  = 0100 → 4

console.log(5 ^ 1); // → 4
console.log(5 ^ 7); // → 2  (0101 ^ 0111 = 0010)
```

### NOT Bitwise `~`

Inverte tutti i bit. Attenzione: a causa del **complemento a due**, `~n` equivale a `-(n + 1)`:

```js
console.log(~5);  // → -6  (inverte 0101 → ...11111010 = -6)
console.log(~0);  // → -1
```

### Shift Sinistro `<<` e Destro `>>`

Spostare i bit a sinistra equivale a **moltiplicare per 2**, spostarli a destra a **dividere per 2**:

```js
console.log(5 << 1); // → 10  (0101 → 1010: moltiplica 5 × 2)
console.log(5 << 2); // → 20  (0101 → 10100: moltiplica 5 × 4)

console.log(20 >> 1); // → 10  (10100 → 01010: divide 20 ÷ 2)
console.log(20 >> 2); // →  5  (10100 → 00101: divide 20 ÷ 4)
```

### Esempio pratico — Gestione Permessi

Un uso classico degli operatori bitwise è la gestione dei permessi tramite flag:

```js
// Definiamo i permessi come potenze di 2 (ogni bit rappresenta un permesso)
const LEGGI   = 1; // 001
const SCRIVI  = 2; // 010
const ELIMINA = 4; // 100

// Assegniamo permessi: l'utente può leggere e scrivere
let permessiUtente = LEGGI | SCRIVI; // 001 | 010 = 011 → 3

// Verifichiamo se l'utente ha il permesso di leggere
if (permessiUtente & LEGGI) {
  console.log("Può leggere ✅");
}

// Verifichiamo se ha il permesso di eliminare
if (permessiUtente & ELIMINA) {
  console.log("Può eliminare ✅");
} else {
  console.log("Non può eliminare ❌");
}
```

### ⚠️ Errori comuni

- **Confondere `&` (bitwise) con `&&` (logico):** `&` lavora sui bit dei numeri; `&&` confronta valori booleani nelle condizioni.
- **`~n` non è semplicemente l'inverso di n:** A causa del complemento a due, `~5` è `-6`, non `0`.
- **Usarli con numeri non interi:** Gli operatori bitwise convertono i valori in interi a 32 bit, quindi eventuali decimali vengono troncati.

### Glossario

| Termine | Definizione | "Spiegazione Brutta" |
|---|---|---|
| **Bit** | Unità minima di informazione, vale 0 o 1 | Il mattoncino base: o è spento (0) o è acceso (1) |
| **Binario** | Sistema numerico in base 2 (solo 0 e 1) | Il "linguaggio" dei computer, fatto solo di zeri e uni |
| **AND Bitwise `&`** | 1 solo se entrambi i bit sono 1 | Come il `&&` logico, ma applicato bit per bit |
| **OR Bitwise `\|`** | 1 se almeno un bit è 1 | Come il `\|\|` logico, ma applicato bit per bit |
| **XOR `^`** | 1 solo se i bit sono diversi | "O l'uno O l'altro, ma non entrambi" |
| **Shift `<< >>`** | Spostamento dei bit a sinistra/destra | Moltiplicare o dividere per 2 in modo fulmineo |

---

## 📚 Risorse e Documentazione
- [Appunti di Francesco C.](https://github.com/classe-154/react-community-notes/tree/big-upload-weekend/proposals/Appunti%20FrancescoC)
- [MDN Web Docs — JavaScript](https://developer.mozilla.org/it/docs/Web/JavaScript)
- [W3Schools — JavaScript Tutorial](https://www.w3schools.com/js/)
- [MDN — Operatori di confronto](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)
- [MDN — Operatori logici](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Operators/Logical_Operators)
- [W3Schools — Bitwise Operators](https://www.w3schools.com/js/js_bitwise.asp)
