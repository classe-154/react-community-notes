# 📘 Appunti JavaScript — Intro JS, Variabili e Tipi di Dati

> Modulo: Web Development — Introduzione a JS; Variabili; Tipi di Dati; Metodi dei Dati

-----

## 📍 Indice

1. [Introduzione a JavaScript](#1-introduzione-a-javascript)
    - 1.1 [Cos'è e a cosa serve](#11-cosè-e-a-cosa-serve)
    - 1.2 [Esempio Base](#12-esempio-base)
    - 1.3 [Esempio Pratico](#13-esempio-pratico)
    - 1.4 [Errori Comuni](#14-️-errori-comuni)
    - 1.5 [Risorse e Documentazione](#15-risorse-e-documentazione)
    - 1.6 [Key Takeaways del Giorno](#16-key-takeaways-del-giorno)
    - 1.7 [Glossario](#17-glossario)
2. [Variabili — `const`, `let`, `var`](#2-variabili--const-let-var)
3. [Tipi di Dati](#3-tipi-di-dati)
4. [Metodi dei Dati](#4-metodi-dei-dati)

----

## 1. Introduzione a JavaScript

### 1.1 Cos’è e a cosa serve

Immagina di voler creare un sito web. Con l’**HTML** crei la struttura (testi, titoli, bottoni), con il **CSS** decidi lo stile (colori, font, layout). Ma il sito, finché usi solo questi due strumenti, è statico e fermo.

**JavaScript** è il linguaggio di programmazione che trasforma quel sito in qualcosa di vivo e interattivo. È il “motore” che permette alla pagina di reagire a quello che fa l’utente.

Esempi concreti:

- Far apparire un messaggio quando clicchi su un bottone
- Controllare se l’email inserita in un form è corretta
- Creare un timer con il conto alla rovescia
- Caricare nuovi contenuti senza ricaricare la pagina

> **Regola d’oro:** JavaScript esegue le istruzioni **in ordine**, dall’alto verso il basso, una riga alla volta.

### 1.2 Esempio base

```js
// Creiamo una variabile con un messaggio e la mostriamo all'utente
let messaggio = "Ciao! Benvenuto nel mondo di JavaScript!";
alert(messaggio);
```

- `let messaggio = ...` → crea una “scatola” chiamata `messaggio` e ci mette dentro il testo
- `alert(messaggio)` → mostra una finestra pop-up con il contenuto della variabile

### 1.3 Esempio pratico

```js
// Saluto personalizzato per un utente registrato
let nomeUtente = "Luca";
let saluto = "Benvenuto nel tuo profilo, " + nomeUtente + "!";

console.log(saluto); // → "Benvenuto nel tuo profilo, Luca!"
```

Il simbolo `+` tra stringhe non fa una somma matematica, ma **concatena** (unisce) i testi. `console.log()` stampa il risultato nella console del browser, strumento utile per i programmatori durante lo sviluppo.

### 1.4 ⚠️ Errori comuni

- **Dimenticare le virgolette per i testi:** `let nome = Luca;` provoca un errore perché JS cerca una variabile chiamata `Luca`, non il testo.
- **Sbagliare maiuscole/minuscole:** `nomeUtente` e `nomeutente` sono due variabili diverse per JavaScript.
- **Confondere il nome con il contenuto:** `console.log("nomeUtente")` stampa la parola *nomeUtente*; `console.log(nomeUtente)` stampa il valore contenuto nella variabile.

### 1.5 Risorse e Documentazione:
- MDN: [Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

### 1.6 Key Takeaways del Giorno

- JavaScript è il linguaggio che rende il sito web dinamico e interattivo, non solo statico.
- Il codice viene eseguito in ordine, dall’alto verso il basso: una riga alla volta.
- Usa JavaScript per reagire a eventi dell’utente come click, invio di un form o caricamento di contenuti.
- `alert()` mostra un messaggio all’utente, mentre `console.log()` è lo strumento per il programmatore.
- Una stringa si concatena con `+`, ma con JavaScript moderno conviene usare i Template Literals per leggibilità.
- In JavaScript i nomi sono case sensitive: `nomeUtente` e `nomeutente` sono diversi.


### 1.7 Glossario

|Termine            |Definizione                                                     |“Spiegazione Brutta”                                               |
|-------------------|----------------------------------------------------------------|-------------------------------------------------------------------|
|**Variabile**      |Area di memoria che memorizza un valore, identificata da un nome|Una “scatola” con un’etichetta in cui metti un dato da riutilizzare|
|**`alert()`**      |Metodo che apre una finestra di dialogo modale con un messaggio |Il pop-up fastidioso che blocca tutto finché non clicchi OK        |
|**`console.log()`**|Funzione che scrive un messaggio nella console del browser      |Il “radiografo” del programmatore per vedere se il codice funziona |


-----

## 2. Variabili — `const`, `let`, `var`

### 2.1 `let` e `const` (moderno)

In JavaScript moderno abbiamo due parole chiave principali per dichiarare variabili:

- **`let`** → usata quando il valore **può cambiare** nel tempo (es. punteggio di un gioco, elementi in un carrello, secondi di un timer)
- **`const`** → usata quando il valore **non deve mai cambiare** (es. anno di nascita, costanti matematiche, nomi di configurazione)

### 2.2 Esempio base

```js
// 'let': il punteggio cambierà durante la partita
let punteggio = 0;
punteggio = 10; // ✅ Aggiornamento corretto (senza riscrivere 'let')

// 'const': la data di nascita è fissa
const annoNascita = 2000;
// annoNascita = 2005; // ❌ ERRORE: non si può modificare una costante
```

> La parola chiave `let` o `const` si scrive **solo la prima volta** (dichiarazione). Per aggiornare il valore di una `let`, si usa solo il nome della variabile.

### 2.3 Esempio pratico

```js
let prezzoIniziale = 50;   // Modificabile: può cambiare con opzioni aggiuntive
const scontoEuro = 5;      // Fisso: lo sconto non deve essere modificato per errore

let prezzoFinale = prezzoIniziale - scontoEuro;
console.log("Prezzo da pagare: " + prezzoFinale + " €"); // → "Prezzo da pagare: 45 €"
```

### 2.4 `var` (vecchio stile)

Prima di `let` e `const`, esisteva solo `var`. Si trova ancora in codice legacy, ma **non va usato nei progetti moderni** perché ha comportamenti anomali legati allo *scope* (visibilità della variabile nel codice) che possono generare bug difficili da trovare.

```js
var vecchioModo = "Usato fino a ES5"; // ⚠️ Evitare nei progetti moderni
```

|Keyword|Modificabile?|Scope      |Quando usarla                  |
|-------|-------------|-----------|-------------------------------|
|`const`|❌ No         |Blocco `{}`|Sempre, come scelta di default |
|`let`  |✅ Sì         |Blocco `{}`|Solo se il valore deve cambiare|
|`var`  |✅ Sì         |Funzione   |Mai (codice legacy)            |

### 2.5 💡 Best Practice

> **Usa `const` come scelta predefinita.** Passa a `let` solo quando sai con certezza che il valore dovrà essere aggiornato. Questo previene modifiche accidentali e rende il codice più leggibile e sicuro.

### 2.6 ⚠️ Errori comuni

- **Modificare una `const`:** `const nome = "Luca"; nome = "Marco";` → errore bloccante.
- **Ridichiarare una variabile con `let`:** `let x = 1; let x = 2;` → errore. Per aggiornare, usa solo `x = 2;`.
- **Creare una `const` vuota:** `const codice;` → errore di sintassi. Le costanti devono essere inizializzate subito.

### 2.7 Glossario

|Termine             |Definizione                                                    |“Spiegazione Brutta”                                                |
|--------------------|---------------------------------------------------------------|--------------------------------------------------------------------|
|**Costante**        |Variabile il cui valore non può essere riassegnato             |Scatola sigillata: ci metti qualcosa una volta e non la apri più    |
|**Riassegnazione**  |Sostituzione del valore memorizzato in una variabile           |Cambiare il contenuto della scatola, possibile solo con `let`       |
|**Inizializzazione**|Assegnazione del valore iniziale al momento della dichiarazione|Riempire la scatola subito quando la crei (obbligatorio per `const`)|

-----

## 3. Tipi di Dati

### Cosa sono

I tipi di dati sono le diverse **categorie di informazioni** che JavaScript può comprendere e memorizzare. JavaScript è un linguaggio **tipizzato dinamicamente**: non siamo noi a dichiarare esplicitamente il tipo, ma JS lo deduce dal valore stesso.

Questo è importante perché il tipo determina quali operazioni si possono fare su un dato:

- Due **numeri** sommati con `+` danno un risultato matematico: `5 + 5 = 10`
- Due **stringhe** sommate con `+` si uniscono (concatenazione): `"5" + "5" = "55"`

### I tre tipi fondamentali

```js
// 1. STRINGA — Testo racchiuso tra virgolette, apici o backtick
let filmPreferito = "Ritorno al Futuro";
let saluto = 'Ciao mondo!';

// 2. NUMERO — Interi o decimali (con il punto, non la virgola)
let annoUscita = 1985;
let prezzo = 9.99;

// 3. BOOLEANO — Solo due valori possibili: true o false
let eUnBelFilm = true;
let haVinto = false;
```

### Esempio pratico — Scheda Fitness

```js
let nomeUtente = "Sara";        // Stringa
let pesoKg = 62.5;              // Numero decimale (punto, non virgola!)
let allenamentoFatto = false;   // Booleano: inizialmente non ha fatto l'allenamento

// Il giorno dopo, Sara finisce l'allenamento
allenamentoFatto = true;

console.log("Nome: " + nomeUtente);
console.log("Peso: " + pesoKg + " kg");
console.log("Allenamento completato? " + allenamentoFatto);
```

### Template Literals (modo moderno per unire testo e variabili)

Invece dell’operatore `+`, nei progetti moderni si usano i **Template Literals**, racchiudendo il testo tra backtick (```) e inserendo le variabili con la sintassi `${variabile}`.

```js
let utente = "Sara";
let peso = 62.5;

// Modo classico (scomodo con molte variabili)
console.log("Nome: " + utente + " - Peso: " + peso + " kg");

// Modo moderno con Template Literals ✅
console.log(`Nome: ${utente} - Peso: ${peso} kg`);
```

I Template Literals permettono anche di andare a capo liberamente e di inserire espressioni come `${2 + 2}` o `${peso * 2}` direttamente nel testo.

### Tipi avanzati: `null`, `undefined`, `BigInt`, `Symbol`

```js
let cartaDiCredito = null;           // Null: assenza INTENZIONALE di valore
let indirizzoSpedizione;             // Undefined: variabile creata ma non inizializzata
const idGigante = 9007199254740991n; // BigInt: numeri astronomici (aggiungere 'n')
const chiaveSegreta = Symbol("id");  // Symbol: identificatore unico e non modificabile
```

|Tipo       |Valore              |Quando si usa                          |
|-----------|--------------------|---------------------------------------|
|`null`     |Assenza intenzionale|L’utente non ha ancora inserito un dato|
|`undefined`|Non inizializzato   |Variabile creata ma ancora vuota       |
|`BigInt`   |Numero enorme + `n` |Crittografia, ID di database enormi    |
|`Symbol`   |Identificatore unico|Chiavi “segrete” nelle strutture dati  |

### Tipi di dato complessi: Object, Array, Function e Date

Oltre ai tipi primitivi, JavaScript ha i **tipi complessi** (o di riferimento). La differenza fondamentale è nel modo in cui vengono memorizzati:

- I **primitivi** copiano il loro valore reale
- I **complessi** memorizzano solo un **riferimento** alla memoria (un “indirizzo”), non il valore diretto

```js
// Object — raccoglie più dati correlati in coppie chiave: valore
const utente = {
  nome: "Luca",
  eta: 25,
  attivo: true
};
console.log(utente.nome); // → "Luca"

// Array — lista ordinata di valori
const colori = ["rosso", "verde", "blu"];
console.log(colori[0]); // → "rosso"

// Function — blocco di codice riutilizzabile
function saluta(nome) {
  return "Ciao, " + nome + "!";
}
console.log(saluta("Sara")); // → "Ciao, Sara!"

// Date — gestione di date e orari
const oggi = new Date();
console.log(oggi); // → data e ora corrente
```

#### ⚠️ Passaggio per Riferimento

Quando copi un tipo complesso in una nuova variabile, non stai duplicando il dato: stai copiando solo l’**indirizzo di memoria**. Questo significa che entrambe le variabili puntano allo stesso dato.

```js
let arr1 = [1, 2, 3];
let arr2 = arr1; // Copia il riferimento, non l'array!

arr2.push(4);

console.log(arr1); // → [1, 2, 3, 4] — È cambiato anche arr1!
console.log(arr2); // → [1, 2, 3, 4]
```

> Con i primitivi questo non succede: ogni variabile ha la sua copia indipendente del valore.

### ⚠️ Errori comuni

- **Numeri tra virgolette:** `let eta = "20";` → JS la tratta come testo. `eta + 1` darà `"201"`, non `21`.
- **Virgola nei numeri decimali:** `let prezzo = 12,50;` → errore di sintassi. Usare sempre il punto: `12.50`.

### Glossario

|Termine                      |Definizione                                                   |“Spiegazione Brutta”                                                                    |
|-----------------------------|--------------------------------------------------------------|----------------------------------------------------------------------------------------|
|**Stringa**                  |Sequenza immutabile di caratteri                              |Un pezzo di testo tra virgolette                                                        |
|**Number**                   |Valore numerico a virgola mobile 64-bit                       |Qualsiasi numero su cui fare calcoli                                                    |
|**Boolean**                  |Tipo logico con valori `true` o `false`                       |L’interruttore del codice: ACCESO o SPENTO                                              |
|**Null**                     |Assenza intenzionale di valore                                |Scatola svuotata di proposito dal programmatore                                         |
|**Undefined**                |Valore assegnato automaticamente a variabili non inizializzate|La scatola esiste ma è ancora totalmente vuota                                          |
|**Object**                   |Tipo complesso che raccoglie coppie chiave-valore             |Una scheda con più informazioni su una cosa                                             |
|**Array**                    |Lista ordinata di valori accessibili tramite indice           |Una lista numerata di elementi                                                          |
|**Function**                 |Blocco di codice riutilizzabile con un nome                   |Una ricetta salvata che puoi richiamare quando vuoi                                     |
|**Passaggio per riferimento**|I tipi complessi condividono lo stesso indirizzo di memoria   |Due etichette appiccicate sulla stessa scatola: cambiarla da una parte la cambia ovunque|

-----

## 4. Metodi dei Dati

### Cosa sono i metodi

Un **metodo** è un’azione predefinita che possiamo far compiere a una variabile. Ogni tipo di dato ha i suoi metodi specifici. Si attivano con la **notazione a punto** seguita da parentesi tonde `()`.

```
variabile.nomeMetodo()
variabile.nomeMetodo(argomento)
```

Alcuni metodi accettano un **argomento** tra le parentesi: un’istruzione precisa che personalizza l’azione (es. quale parola cercare, quante cifre decimali mantenere).

### Metodi principali delle Stringhe

```js
let testo = "  javascript è bello  ";

// toUpperCase() — converte in MAIUSCOLO
let maiuscolo = testo.toUpperCase();     // "  JAVASCRIPT È BELLO  "

// toLowerCase() — converte in minuscolo
let minuscolo = testo.toLowerCase();     // "  javascript è bello  "

// trim() — rimuove spazi iniziali e finali
let pulito = testo.trim();               // "javascript è bello"

// includes(arg) — verifica se contiene una parola (restituisce true/false)
let trovato = pulito.includes("bello");  // true

// replace(vecchio, nuovo) — sostituisce la prima occorrenza trovata
let modificato = pulito.replace("bello", "fantastico"); // "javascript è fantastico"
```

> **Importante:** Le stringhe sono **immutabili**. I metodi non modificano la variabile originale, ma restituiscono una nuova stringa. Per salvare il risultato, devi assegnarlo a una variabile.

```js
let nome = "luca";
nome.toUpperCase();       // ❌ Il risultato viene perso!
nome = nome.toUpperCase(); // ✅ Ora 'nome' contiene "LUCA"
```

### Metodi principali dei Numeri

```js
let prezzo = 19.98765;

// toFixed(n) — arrotonda a n cifre decimali (⚠️ restituisce una STRINGA)
let prezzoFormattato = prezzo.toFixed(2); // "19.99"
```

### Proprietà `.length`

Le **proprietà** sono caratteristiche fisse di un dato, si leggono **senza parentesi**:

```js
let parola = "Ciao";
let lunghezza = parola.length; // 4
```

### Conversione con `.toString()`

```js
let numero = 500;
let testo = numero.toString(); // "500"
console.log(testo.length);     // 3
```

### Esempio pratico — Registrazione Utente

```js
// Pulizia email inserita dall'utente
let emailUtente = " Mario.Rossi@Email.com ";
let emailCorretta = emailUtente.trim().toLowerCase();
// → "mario.rossi@email.com"

// Censura di una parola non consentita in un commento
let commento = "Questo codice è stupido!";
let commentoCensurato = commento.replace("stupido", "******");
// → "Questo codice è ******!"

// Formattazione del prezzo
let totale = 54.3219;
let totaleMostrato = totale.toFixed(2);
// → "54.32"

console.log(`Email: ${emailCorretta}`);
console.log(`Commento: ${commentoCensurato}`);
console.log(`Totale: ${totaleMostrato} €`);
```

### ⚠️ Errori comuni

- **Dimenticare le parentesi:** `nome.toUpperCase` senza `()` non esegue nulla, assegna il metodo stesso.
- **Case sensitivity in `.includes()`:** Se il testo ha `"JavaScript"` (J maiuscola) e cerchi `.includes("javascript")` (j minuscola), restituisce `false`.
- **Metodi sul tipo sbagliato:** `.toUpperCase()` funziona solo sulle stringhe, non sui numeri.
- **`.toFixed()` restituisce una stringa:** Se sommi il risultato con un numero, otterrai una concatenazione, non una somma.

### Glossario

|Termine          |Definizione                                                     |“Spiegazione Brutta”                                                |
|-----------------|----------------------------------------------------------------|--------------------------------------------------------------------|
|**Metodo**       |Funzione associata a un tipo di dato che opera su di esso       |Un “superpotere” integrato nel dato, si attiva con `.nome()`        |
|**Argomento**    |Valore passato a un metodo per personalizzarne il comportamento |L’istruzione che metti tra le parentesi per dire al metodo cosa fare|
|**Proprietà**    |Caratteristica descrittiva di un dato, senza parentesi          |Un’informazione già pronta da leggere, come `.length`               |
|**`.trim()`**    |Rimuove spazi iniziali e finali da una stringa                  |“Taglia” gli spazi inutili ai bordi del testo                       |
|**`.toFixed(n)`**|Formatta un numero con n cifre decimali, restituisce una stringa|Il sarto dei decimali: arrotonda ma ti trasforma il numero in testo |

-----

## 📚 Risorse e Documentazione

- [Appunti di Francesco C.](https://github.com/classe-154/react-community-notes/tree/big-upload-weekend/proposals/Appunti%20FrancescoC)
- [MDN Web Docs — JavaScript](https://developer.mozilla.org/it/docs/Web/JavaScript)
- [W3Schools — JavaScript Tutorial](https://www.w3schools.com/js/)
- [MDN — Tipi di dati](https://developer.mozilla.org/it/docs/Web/JavaScript/Data_structures)
- [MDN — Metodi delle Stringhe](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/String)