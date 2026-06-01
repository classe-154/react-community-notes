# Scope e Hoisting

📅 **Modulo:** JavaScript Basic-Intermediate & Advanced

**Titolo:** Scope e Hoisting: Navigare le Dimensioni Invisibili del Codice 

---

### 📍 Indice Rapido

1. [1.1 Lo Scope: Il Confine dei tuoi Poteri (Globale vs Locale)](#11-lo-scope-il-confine-dei-tuoi-poteri-globale-vs-locale)
2. [1.2 Function Scope vs Block Scope: Gabbie Vecchie e Nuove](#12-function-scope-vs-block-scope-gabbie-vecchie-e-nuove)
3. [1.3 Hoisting: Il Sollevamento Misterioso e la Fantomatica TDZ](#13-hoisting-il-sollevamento-misterioso-e-la-fantomatica-tdz)
4. [1.4 Pratiche Avanzate e Scenari Apocalittici](#14-pratiche-avanzate-e-scenari-apocalittici)
5. [🔗 Risorse Tecniche (MDN, W3S, JS)](https://www.google.com/search?q=%23-risorse-e-documentazione)
6. [🚀 Key Takeaways del giorno](https://www.google.com/search?q=%23-key-takeaways-del-giorno)
7. [📖 Glossario: Definizioni Istituzionali vs Spiega Brutta](https://www.google.com/search?q=%23-glossario-del-male)


### 📑 Corpo Centrale

#### [1.1 Lo Scope: Il Confine dei tuoi Poteri (Globale vs Locale)](#-indice-rapido)

**a) Spiegazione concettuale**

Immagina lo scope come i vetri oscurati di una limousine di lusso. Chi sta dentro la macchina può guardare fuori e vedere tutto il panorama, ma chi sta fuori sul marciapiede non può assolutamente spiare cosa succede dentro. In JavaScript, lo scope è esattamente questo: stabilisce i confini di visibilità delle tue variabili, decidendo chi può usarle e dove. Serve a evitare il caos totale, impedendo a parti diverse del programma di sovrascrivere i dati a vicenda per errore.

**b) Esempi di codice**

```JavaScript
// Questo è uno scope globale (la piazza del paese: tutti vedono tutto)
let messaggioGlobale = "Sono visibile ovunque!";

function mostraMessaggio() {
  // La funzione accede felicemente alla variabile fuori dalle sue mura
  console.log(messaggioGlobale);
}

mostraMessaggio();
// Output: "Sono visibile ovunque!"
```


```JavaScript
function segretoDiStato() {
  // Scope Locale: questa variabile nasce e muore dentro questa funzione
  let codiceLancio = "12345_SuperSegreto";
  console.log(codiceLancio); // Funziona perfettamente all'interno
}

segretoDiStato();
// Output: "12345_SuperSegreto"

console.log(codiceLancio);
// ❌ Errore Comune! ReferenceError: codiceLancio is not defined
// Perché stai cercando di guardare dentro i vetri oscurati della limousine!
```


**c) Varianti e casi limite (Lo Shadowing)**

Cosa succede se crei una variabile locale con lo stesso identico nome di una globale? La locale "oscura" temporaneamente la globale all'interno del suo territorio.



```JavaScript
// Scenario Avanzato: Shadowing (Oscuramento)
let eroe = "Batman"; // Eroe globale

function trasformaCitta() {
  let eroe = "Robin"; // Eroe locale con lo stesso nome
  console.log(eroe);  // JS guarda prima nello scope più vicino
}

trasformaCitta();
// Output: "Robin"

console.log(eroe);
// Output: "Batman" (Il Batman globale non è stato toccato o modificato)
```

#### [1.2 Function Scope vs Block Scope: Gabbie Vecchie e Nuove](#-indice-rapido)

**a) Spiegazione concettuale**

Fino al 2015, JavaScript usava solo la parola chiave `var`, che creava una gabbia protettiva unicamente attorno alle funzioni (Function Scope). Se infilavi una `var` dentro un blocco `if` o un ciclo `for`, questa scappava fuori come se nulla fosse. Con l'arrivo di `let` e `const`, sono state introdotte le pareti blindate per ogni singolo blocco di parentesi graffe `{}` (Block Scope). È la differenza tra una dogana che controlla solo i confini di stato (le funzioni) e le porte blindate per ogni stanza della casa (i blocchi `{}`).

**b) Esempi di codice**



```JavaScript
function testFunctionScope() {
  if (true) {
    var evaso = "Sono scappato dall'if!"; // var se ne frega delle graffe dell'if
  }
  // var è visibile in TUTTA la funzione, anche fuori dal blocco if!
  console.log(evaso); 
}

testFunctionScope();
// Output: "Sono scappato dall'if!"
```


```JavaScript
function testBlockScope() {
  if (true) {
    let prigioniero = "Non posso uscire dalle graffe"; // let rispetta rigorosamente il blocco
  }
  // Cerchiamo di chiamarlo fuori dalle sue amate graffe
  console.log(prigioniero); 
}

testBlockScope();
// ❌ Errore Comune! ReferenceError: prigioniero is not defined
```


**c) Varianti e casi limite**

Uno dei bug storici più famosi di JavaScript riguardava l'uso di `var` nei cicli di ripetizione, dove la variabile finiva per inquinare tutto l'ambiente circostante.


```JavaScript
// Scenario Avanzato: Il loop malefico con var
for (var i = 0; i < 3; i++) {
  // Esegue il ciclo 3 volte
}
// Poiché var non ha block scope, 'i' sopravvive all'esterno del ciclo!
console.log(i);
// Output: 3 (Con 'let' avresti ricevuto un sanissimo ed educato ReferenceError)
```

#### [1.3 Hoisting: Il Sollevamento Misterioso e la Fantomatica TDZ](#-indice-rapido)

**a) Spiegazione concettuale**

L'Hoisting è un comportamento automatico in cui JavaScript, prima di eseguire il codice riga per riga, sposta mentalmente tutte le dichiarazioni in cima al loro scope di appartenenza. È come se un cameriere al ristorante ti portasse il conto prima ancora che tu abbia aperto il menu. Con la vecchia parola chiave `var`, il cameriere ti porta un foglio bianco (`undefined`); con le moderne `let` e `const`, invece, finisci direttamente nella _Temporal Dead Zone_ (Zona Morta Temporale), una terra di mezzo in cui la variabile esiste ma è severamente vietato toccarla prima della sua reale inizializzazione.

**b) Definizioni con Esempi di codice**

**Definizione**:
  - _Hoisting_ é il meccanismo per cui JavaScript, prima di leggere il codice, individua tutte le variabili e le funzioni e le "prenota" in memoria, facendole esistere ancora prima che arrivi il turno della riga in cui le hai scritte.

```JavaScript
// ❌ CODICE CHE SCRIVI (Pensando che esploda tutto)
console.log(nomeGatto);
var nomeGatto = "Malvagio";

// 🔍 COSA JAVASCRIPT FA INTERNAMENTE (Hoisting con var)
var nomeGatto;          // La dichiarazione viene sollevata in cima senza il valore
console.log(nomeGatto); // undefined (Esiste la scatola, ma dentro è vuota!)
nomeGatto = "Malvagio"; // L'assegnamento del valore rimane al suo posto originale
```

**Definizione**:
  - _Temporal Dead Zone (TDZ)_ é la "zona d'ombra" (il blocco di tempo) in cui una variabile creata con let o const esiste già nella memoria ma non può ancora essere usata. Inizia all'apertura del blocco di codice e finisce nell'esatto momento in cui la variabile viene definita e le viene assegnato un valore.



```JavaScript
// ❌ CODICE CHE SCRIVI CON LET
console.log(nomeCane);
let nomeCane = "Birba";

// ❌ Risultato: ReferenceError: Cannot access 'nomeCane' before initialization
// Spiegazione: 'let' viene sollevata, ma non viene inizializzata a undefined.
// Da inizio blocco fino alla riga di 'let nomeCane', sei nella Temporal Dead Zone!
```


**c) Varianti e casi limite (Hoisting delle Funzioni)**

Le funzioni create con la forma tradizionale vengono sollevate completamente (dichiarazione + corpo della funzione), mentre le funzioni salvate dentro variabili seguono le regole delle variabili.


```JavaScript
// Scenario Avanzato: Funzioni vs Arrow Functions
faiMiao(); 
// Output: "Miao!" (Funziona! La funzione classica viene sollevata interamente)

function faiMiao() {
  console.log("Miao!");
}

faiRinghio();
// ❌ Errore! ReferenceError: Cannot access 'faiRinghio' before initialization
let faiRinghio = () => {
  console.log("Ringhio!");
};
```

#### [1.4 Pratiche Avanzate e Scenari Apocalittici](#-indice-rapido)

**a) Spiegazione concettuale**

Scrivere codice senza gestire correttamente gli scope è come lanciare vernice fresca in una stanza ventosa: sporcherai ovunque. Dimenticare una parolina chiave può agganciare la tua variabile direttamente all'oggetto globale del browser, rendendola accessibile (e modificabile) da qualsiasi altro script esterno. La regola d'oro è il _Principio del Minimo Privilegio_: tieni tutto il codice chiuso nel recinto più piccolo e protetto possibile.

**b) Esempi di codice**


```JavaScript
// Cattiva pratica estrema: Inquinamento dello Scope Globale
function creaCaos() {
  variabileFantasma = "Ops, ho dimenticato let!"; 
  // Senza dichiarazione, JS la regala allo scope globale (window)
}
creaCaos();

console.log(window.variabileFantasma);
// Output: "Ops, ho dimenticato let!" (Chiunque ora può distruggerla)
```

**c) Varianti e scenari avanzati (Introduzione alle Closures)**

Quando una funzione viene racchiusa dentro un'altra funzione, quella interna si ricorda per sempre dello scope in cui è nata, portandosi dietro le variabili come in uno zainetto invisibile.


```JavaScript
// Scenario Avanzato: La Closure (Chiusura)
function fabbricaDiContatori() {
  let conteggio = 0; // Variabile blindata e inaccessibile dall'esterno
  
  return function() {
    conteggio++;
    console.log(conteggio);
  };
}

const mioContatore = fabbricaDiContatori();
mioContatore(); // Output: 1
mioContatore(); // Output: 2 (Si ricorda il valore grazie allo scope di nascita!)
```

### [🔗 Risorse e Documentazione](#-indice-rapido)

• 📚 **MDN Web Docs:** 
	-[Capire lo Scope in JS](https://developer.mozilla.org/en-US/docs/Glossary/Scope) • Per scoprire perché quella variabile dichiara guerra al tuo codice.
	-[Il Sollevamento Magico (Hoisting)](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) • Una guida passo passo per non farsi spaventare dal codice che fluttua verso l'alto.

• 🏫 **W3Schools:** 
	-[Esercizi semplici per comprendere lo Scope](https://www.w3schools.com/js/js_scope.asp)
	-[Esercizi semplici per comprendere l'Hoisting](https://www.w3schools.com/js/js_hoisting.asp)


• ⚛️ **Guida Galattica per Sviluppatori:** [Sopravvivere nella Temporal Dead Zone](https://www.geeksforgeeks.org/javascript/temporal-dead-zone-in-javascript/) • Come evitare di essere folgorati dalla TDZ.

### [🚀 Key Takeaways del Giorno](#-indice-rapido)

_I punti fondamentali da portarsi a casa per non piangere in produzione:_

• **Scope Globale come Discarica:** È la piazza pubblica dell'applicazione. Usalo solo se strettamente necessario, altrimenti rischi collisioni disastrose.

• **Abbraccia `const` e `let`:** Dimentica l'esistenza di `var`. Le parentesi graffe `{}` devono essere i guardiani impenetrabili delle tue variabili.

• **L'effetto Zombie di `var`:** Ricorda che l'hoisting di `var` crea variabili spettrali con valore `undefined`, pronte a generare bug silenziosi.

• **La Trappola della TDZ:** Se usi `let` o `const`, scrivi sempre le dichiarazioni all'inizio del blocco prima di usarle. Vietato saltare la fila!

• **Il Principio dello Zainetto:** Le funzioni si ricordano sempre del luogo in cui sono nate e mantengono l'accesso a quelle variabili (Closure).

### [📖 Glossario del Male](#-indice-rapido)

_Lista delle definizioni istituzionali tradotte in linguaggio umano per superare i colloqui tecnici senza ansia._

|**Termine Tecnico**|**Definizione Formale**|**"Spiega Brutta"**|
|---|---|---|
|**var**|Parola chiave originale per dichiarare variabili con scope limitato alla funzione.|Il vecchio Far West di JavaScript, dove le variabili saltano fuori dalle finestre degli `if`.|
|**let**|Parola chiave moderna per dichiarare variabili riassegnabili con scope di blocco.|La variabile con la cintura di sicurezza: rispetta fedelmente i confini delle graffe `{}`.|
|**const**|Parola chiave per dichiarare costanti a blocco non riassegnabili.|Un tatuaggio indelebile sul codice. Se provi a modificarlo, JavaScript ti lancia un errore in faccia.|
|**Scope**|Il contesto di esecuzione in cui i valori e le espressioni sono visibili e accessibili.|Il recinto di proprietà privata. Se sei fuori dal cancello, non puoi toccare quello che c'è dentro.|
|**Hoisting**|Meccanismo per cui le dichiarazioni vengono spostate in cima al loro contesto prima dell'esecuzione.|Il cameriere telepatico che sa già cosa ordinerai, ma intanto ti porta un piatto vuoto (`undefined`).|
|**Temporal Dead Zone**|Area di un blocco compresa tra l'inizio del blocco stesso e l'effettiva dichiarazione di una variabile `let`/`const`.|Il Purgatorio del codice. Se provi anche solo a nominare la variabile lì dentro, vieni fulminato.|
|**Closure**|La combinazione di una funzione e del rispettivo ambiente lessicale in cui è stata creata.|Una funzione con lo zainetto che si ricorda le variabili di casa sua anche se va a vivere dall'altra parte del mondo.|
|**Global Object**|Oggetto globale (`window` nel browser) che contiene le variabili e funzioni accessibili ovunque.|Il grande calderone pubblico dove tutti buttano cose a caso finché la memoria del computer non implode.|