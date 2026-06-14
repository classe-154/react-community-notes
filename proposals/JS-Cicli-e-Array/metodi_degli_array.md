#### Modulo: JavaScript  
**Titolo:** I Metodi degli Array

**📍 Indice Rapido**

1. [I Metodi degli Array (Base e Avanzati)](#️-1-i-metodi-degli-array-base-e-avanzati)

2. [I metodi base di inserimento, rimozione e taglio](#-2-i-metodi-base-di-inserimento-rimozione-e-taglio)

3. [Metodi di Ricerca e Conversione rapida](#-3-metodi-di-ricerca-e-conversione-rapida)

4. [Errori comuni](#️-4-errori-comuni)

5. [Approfondimento leggero: Modificare l'array originale o crearne uno nuovo? (Mutabilità)](#-5-approfondimento-leggero-modificare-larray-originale-o-crearne-uno-nuovo-mutabilità)

6. [Risorse e Documentazione](#6-risorse-e-documentazione)

7. [Key Takeaways del Giorno](#7-key-takeaways-del-giorno)

8. [Glossario](#8-glossario)

## 🛠️ 1. **I Metodi degli Array (Base e Avanzati)**

Un metodo è una funzione nativa (un comando predefinito) che esegue un'azione specifica sull'array su cui viene agganciato. I metodi si dividono in due grandi categorie: quelli base (per inserire, rimuovere e cercare) e quelli avanzati (per trasformare o filtrare i dati tramite funzioni).

## 🔀 2. **I metodi base di inserimento, rimozione e taglio**

I comandi più utilizzati per modificare la struttura dell'array operano sulle estremità o su porzioni specifiche della lista:

- `.`**`push(elemento1, elemento2, ...)`**: Aggiunge uno o più elementi in fondo e restituisce la nuova lunghezza dell'array. **Questo metodo cambia l'array originale.**   
- `.`**`pop()`**: Rimuove l'ultimo elemento in fondo e lo restituisce. **Questo metodo cambia l'array originale.**

*Esempio* `push()` e `pop()` (Lavorare in fondo):
```javascript
const lista = ["A", "B"];
lista.push("C", "D"); // lista ora è: ["A", "B", "C", "D"]
const ultimo = lista.pop(); // ultimo è "D", lista ora è: ["A", "B", "C"]
```

- `.`**`unshift(elemento1, elemento2, ...)`**: Inserisce uno o più elementi all'inizio (fa scalare gli altri in avanti) e restituisce la nuova lunghezza dell'array. **Questo metodo cambia l'array originale.**
    
- `.`**`shift()`**: Rimuove il primo elemento all'inizio (fa scalare gli altri indietro) e lo restituisce. **Questo metodo cambia l'array originale.**
    
*Esempio* `unshift()` e `shift()` (Lavorare all'inizio):
```javascript
const lista = ["B", "C"];
lista.unshift("A"); // lista ora è: ["A", "B", "C"]
const primo = lista.shift(); // primo è "A", lista ora è: ["B", "C"]
```

### ✂️ Modifiche arbitrarie: `splice()` e `slice()`

- `splice()`: Chirurgia mirata (Modifica l'originale)

```JavaScript
let colori = ["Rosso", "Verde", "Blu", "Giallo"];

// Rimuovere 2 elementi partendo dall'indice 1
colori.splice(1, 2); // colori: ["Rosso", "Giallo"]

// Inserire elementi all'indice 1 senza rimuovere nulla
colori.splice(1, 0, "Viola", "Arancio"); // colori: ["Rosso", "Viola", "Arancio", "Giallo"]

// Sostituire 1 elemento all'indice 0 con "Nero"
colori.splice(0, 1, "Nero"); // colori: ["Nero", "Viola", "Arancio", "Giallo"]
```

-  `slice()`: Estrazione sicura (Crea una copia)

```JavaScript
const numeri = [10, 20, 30, 40, 50];

// Prende gli elementi dall'indice 1 fino al 4 (escluso)
const parte = numeri.slice(1, 4); // parte è: [20, 30, 40]

// L'array originale non viene toccato
console.log(numeri); // [10, 20, 30, 40, 50]
```

## 🔍 3. **Metodi di Ricerca e Conversione rapida**

Oltre a modificare l'array, spesso abbiamo bisogno di sapere se un elemento esiste o in quale posizione si trova:

- `.`**`includes(valore)`**: Restituisce `true` se il valore è presente nell'array, altrimenti `false`.
    
- `.`**`indexOf(valore)`**: Restituisce l'indice numerico della prima posizione in cui trova il valore. Se il valore non esiste nell'array, restituisce `-1`.
    
- `.`**`join(separatore)`**: Incolla tutti gli elementi dell'array trasformandoli in una singola stringa, separati dal carattere inserito tra le tonde.


```js
const spesa = ["Pane", "Latte", "Caffè"];

console.log(spesa.includes("Latte")); // Output: true
console.log(spesa.indexOf("Caffè"));  // Output: 2
console.log(spesa.indexOf("Pasta"));  // Output: -1 (non esiste!)

console.log(spesa.join(" - "));      // Output: "Pane - Latte - Caffè"
```
## ⚠️ 4. **Errori comuni**

- **Confondere .splice() con .slice():** I loro nomi si somigliano, ma fanno cose diverse. `.splice()` (con la 'p') modifica l'array originale, distruggendo o cambiando i suoi dati sul posto. `.slice()` (senza 'p') è sicuro, non tocca l'array originale e si limita a fare una copia di una porzione.
    
- **Dimenticare il return implicito nelle arrow function di .map():** Se scrivi `const nuovi = array.map((el) => { el * 2 })` usando le parentesi graffe senza scrivere la parola chiave `return`, il nuovo array si riempirà di valori `undefined`. Se usi le parentesi graffe devi mettere `return`. Se non le metti, il valore viene restituito da solo in automatico: `array.map((el) => el * 2)`.

 - **Dimenticare il return implicito nelle arrow function di .filter:** Se scrivi `const nuovi = array.filter((el) => { el * 2 > 5 })` usando le parentesi graffe senza scrivere la parola chiave `return`, il nuovo array sarà vuoto perché la callback restituisce `undefined` che è visto dalla filter come `false` . Se usi le parentesi graffe devi mettere `return`. Se non le metti, il valore viene restituito da solo in automatico: `array.filter((el) => el * 2 > 5)`. 

- **Pensare che .forEach() restituisca qualcosa:** Scrivere `let risultato = array.forEach(...)` salverà sempre `undefined` dentro la variabile. Il `forEach` serve solo ad eseguire azioni (come i `console.log`), non a generare nuovi dati o modificare l'array per l'assegnazione.
    

## 📌 5. **Approfondimento leggero: Modificare l'array originale o crearne uno nuovo? (Mutabilità)**

In JavaScript avanzato è fondamentale distinguere tra metodi mutativi (che modificano i dati di partenza) e immutativi (che lasciano intatto l'array iniziale e ne creano uno nuovo).

- **Metodi Mutativi (Attenzione!):** `push()`, `pop()`, `shift()`, `unshift()`, `splice()`.
    
- **Metodi Immutativi (Più sicuri):** `slice()`, `map()`, `filter()`.
    

Nello sviluppo moderno (specialmente quando si utilizzano librerie come React) si preferisce quasi sempre l'approccio immutativo per evitare che una funzione modifichi per errore dati usati in altre parti del programma.

## 6. **Risorse e Documentazione**

- 📚 [MDN Web Docs (Array Map)](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
    
- 📚 [MDN Web Docs (Array Filter)](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
    
- 🏫 [W3Schools (JS Array Iteration)](https://www.w3schools.com/js/js_array_iteration.asp)
    

## 7. **Key Takeaways del Giorno**

- **Splice taglia, Slice copia:** Usa `.splice()` se devi asportare o iniettare elementi modificando la lista originale. Usa `.slice()` per fare una foto parziale senza fare danni.
    
- **Map trasforma:** `.map()` è il re delle trasformazioni. Genera sempre un array identico per numero di elementi, ma con i valori modificati uno per uno.
    
- **Filter seleziona:** `.filter()` riduce le dimensioni della lista trattenendo solo gli elementi che superano il tuo test logico.
    

## 8. **Glossario**

| **Termine Istituzionale**   | **Definizione Formale**                                                                                                       | **"Spiega Brutta"**                                                                                                                        |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Metodo Iterativo**        | Metodo che accetta una funzione di callback per elaborare sequenzialmente gli elementi di una collezione.                     | Un comando avanzato che si fa tutto il giro dell'array da solo ed esegue i tuoi ordini su ogni elemento.                                   |
| **Immutabilità**            | Paradigma di programmazione in cui le strutture dati non possono essere modificate dopo la loro creazione.                    | La buona abitudine di non distruggere l'array originale, ma di generarne uno nuovo modificato per non perdere lo storico.                  |
| **Array Multidimensionale** | Struttura dati in cui gli elementi di un array sono a loro volta degli array, definendo una griglia a più dimensioni.         | Un'app di foglio di calcolo (tipo Excel) o una scacchiera: un array di righe dove ogni riga contiene un array di celle.                    |
| **Ciclo For...Of**          | Costrutto iterativo che esegue un ciclo sui valori di una collezione di dati indicizzata o iterabile.                         | Il ciclo pigro: non ti dà il numero del cassetto (`i`), ma ti lancia direttamente in mano il contenuto di ogni cassetto, uno dopo l'altro. |
| **Callback (in metodi)**    | Funzione passata come argomento a un'altra funzione, destinata a essere eseguita in risposta a un evento o per ogni elemento. | La micro-istruzione che dai in pasto a `.map()` o `.filter()` per spiegargli cosa fare su ogni singolo dato.      |