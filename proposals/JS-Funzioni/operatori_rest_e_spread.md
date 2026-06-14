#### Modulo: JavaScript  
**Titolo:** Operatori Rest e Spread

## 📍 Indice Rapido 

1. [Operatori Rest e Spread](#-1-operatori-rest-e-spread)
2. [L'operatore Rest (...) nelle Funzioni](#-2-loperatore-rest--nelle-funzioni)
3. [L'operatore Spread (...) nelle Invocazioni e Strutture Dati](#-3-loperatore-spread--nelle-invocazioni-e-strutture-dati)
4. [Errori comuni](#️-4-errori-comuni)
5. [Risorse e Documentazione](#-5-risorse-e-documentazione)
6. [Key Takeaways del Giorno](#-6-key-takeaways-del-giorno)
7. [Glossario](#-7-glossario)

## 🔄 1. Operatori Rest e Spread

In JavaScript moderno, i tre puntini consecutivi `...` rappresentano uno dei costrutti sintattici più flessibili e utilizzati. Questo operatore cambia comportamento e nome in base alla posizione esatta in cui viene inserito nel codice:

- Si comporta come **Rest Parameter** quando deve raccogliere più elementi sparsi e raggrupparli in un unico contenitore.
    
- Si comporta come **Spread Operator** quando deve prendere un contenitore e sparpagliare i suoi elementi all'esterno.
    

Capire questa dualità è fondamentale per gestire flussi di dati dinamici ed evitare la manipolazione diretta della memoria.

## 📥 2. L'operatore Rest (...) nelle Funzioni

Il parametro Rest viene utilizzato all'interno della firma (i parametri) di una funzione. Risolve un problema storico di JavaScript: come gestire una funzione quando non sappiamo in anticipo quanti argomenti passerà l'utente.

Inserendo `...` prima del nome dell'ultimo parametro, ordiniamo a JavaScript di catturare tutti gli argomenti "rimanenti" passati durante l'invocazione e di impacchettarli automaticamente all'interno di un vero Array.

```JavaScript
// La funzione accetta un primo argomento fisso, e tutti gli altri finiscono nell'array 'competenze'
const creaProfiloSviluppatore = (nome, ...competenze) => {
  console.log(`Sviluppatore: ${nome}`);
  console.log(`Array competenze:`, competenze); // 'competenze' è un array a tutti gli effetti
};

creaProfiloSviluppatore("Nathan", "JavaScript", "React", "Node.js", "TypeScript");
// Output: 
// Sviluppatore: Nathan
// Array competenze: ["JavaScript", "React", "Node.js", "TypeScript"]
```

Le regole ingegneristiche del parametro Rest sono rigidissime:

1. **Unicità:** Può esserci un solo parametro Rest all'interno di una funzione.
    
2. **Posizionamento:** Deve essere tassativamente l'ultimo parametro nella firma della funzione (es: `(a, b, ...rest)` è valido, `(a, ...rest, b)` genererà un errore di sintassi bloccante).
    

#### 🎯 La soluzione moderna per le Arrow Functions

Come abbiamo visto nel capitolo precedente, le Arrow Functions non hanno accesso all'oggetto nativo `arguments`. Il Rest Parameter risolve questo limite alla radice: essendo un vero parametro espresso con la sintassi moderna, può essere usato senza problemi dentro le funzioni a freccia per catturare infiniti argomenti.

```JavaScript
// I tre puntini dicono: "Prendi tutti i prezzi che ti arrivano e infilali nell'array 'listaPrezzi'"
const calcolaSpesa = (...listaPrezzi) => {
  // Se facciamo un console.log, vedrai che listaPrezzi è un VERO array: [10, 5, 20]
  console.log(listaPrezzi); 
  
  let totale = 0;
  
  // Essendo un vero array, possiamo usarlo subito con un ciclo per sommare i prezzi
  for (const prezzo of listaPrezzi) {
    totale += prezzo;
  }
  
  return totale;
};

// 🛒 CASO 1: Il cliente compra 3 prodotti
let spesaFilippo = calcolaSpesa(10, 5, 20); 
console.log(`Filippo deve pagare: ${spesaFilippo}€`); // Output: 35€

// 🛒 CASO 2: Il cliente compra 5 prodotti
let spesaAnna = calcolaSpesa(2, 3, 1, 4, 10); 
console.log(`Anna deve pagare: ${spesaAnna}€`); // Output: 20€
```

## 📤 3. L'operatore Spread (...) nelle Invocazioni e Strutture Dati

L'operatore Spread fa l'esatto opposto del Rest: prende un contenitore (come un Array o un Oggetto) e lo "scompatta", spalmando i suoi singoli elementi all'interno di un nuovo contesto, come se li avessi scritti a mano uno per uno separati da virgole.

Viene utilizzato principalmente in tre scenari professionali:

### 1. Passare un Array a funzioni che richiedono argomenti singoli

Funzioni native come `Math.max()` o `Math.min()` non accettano un array come input, ma vogliono una lista di numeri singoli separati da virgole. Lo Spread "sbriciola" l'array prima di passarlo.

```JavaScript
const temperature = [19, 24, 31, 22, 18];

// Math.max(temperature) restituirebbe NaN perché non legge l'array intero
const temperaturaMassima = Math.max(...temperature); // Equivale a: Math.max(19, 24, 31, 22, 18)
console.log(temperaturaMassima); // Stampa: 31
```

### 2. Clonare Array o Oggetti (Copia Superficiale / Shallow Copy)

In JavaScript, assegnare un array o un oggetto a una nuova variabile non crea una copia, ma un **collegamento allo stesso indirizzo di memoria**. Se modifichi la nuova variabile, cambierai accidentalmente anche l'originale. Lo Spread `[...]` rompe questo legame creando un nuovo contenitore indipendente.

**Esempio: La differenza tra "collegamento" e "copia"**

```JavaScript
// ❌ ASSEGNAZIONE DIRETTA (Si rompe: condividono lo stesso indirizzo)
const originale = ["pane", "latte"];
const copiaSbagliata = originale; 

copiaSbagliata.push("uova"); 
console.log(originale); // ["pane", "latte", "uova"] -> ⚠️ L'originale è sporco!

// ✅ CON LO SPREAD (La fotocopia sicura)
const originalePulito = ["pane", "latte"];
const copiaSicura = [...originalePulito]; // Crea un NUOVO contenitore in memoria

copiaSicura.push("uova");
console.log(originalePulito); // ["pane", "latte"] -> ✅ L'originale è protetto!
console.log(copiaSicura);     // ["pane", "latte", "uova"]
```

### 3. Unire (Fondere) strutture dati

Lo Spread permette di combinare elementi di più array o proprietà di più oggetti all'interno di un unico elemento finale in modo pulito e dichiarativo.

```JavaScript
const configurazioneBase = { tema: "scuro", notificheAttive: true };
const impostazioniUtente = { notificheAttive: false, lingua: "it" };

// Unione di oggetti: le proprietà combaciano, le ultime sovrascrivono le prime
const configurazioneFinale = { ...configurazioneBase, ...impostazioniUtente };
console.log(configurazioneFinale); // { tema: "scuro", notificheAttive: false, lingua: "it" }
```

## ⚠️ 4. Errori comuni

- **Invertire l'ordine del Rest Parameter:** Scrivere `const impostaDati = (...elementi, idFisso) => {}` bloccherà immediatamente l'applicazione lanciando un `SyntaxError: Rest parameter must be last formal parameter`. Il Rest deve sempre chiudere la fila.
    
- **Confondere la copia profonda con lo Spread:** Lo Spread esegue una copia superficiale (_Shallow Copy_). Se cloni un array o un oggetto che contiene al suo interno altri oggetti o array annidati, i sotto-elementi strutturati rimarranno comunque collegati per riferimento alla memoria originale.
Ad esempio:
```JavaScript
const originale = [{ nome: "Luca" }, 2];
const copia = [...originale];
// Analiziamo cosa sta succedendo qui:
/*
copia è un nuovo array diverso da originale
copia[0] punta allo stesso oggetto di originale[0]
copia[1] è un numero e quindi è una copia del valore

Quindi:

Se modifichi copia[1], l’originale non cambia.
Se modifichi una proprietà interna dell’oggetto annidato (copia[0].nome = "Mario"), anche originale[0].nome cambia, perché l’oggetto annidato è lo stesso riferimento.
*/
```
    

## 🔗 5. **Risorse e Documentazione**

- 📚 MDN Web Docs (Rest parameters): [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
    
- 📚 MDN Web Docs (Spread syntax): [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
    

## 🚀 6. **Key Takeaways del Giorno**

- **Rest raccoglie, Spread distribuisce:** Identifica il contesto visivo: se si trova nei parametri di una funzione sta raggruppando elementi sparsi (`Rest`), se si trova in un'assegnazione o chiamata a funzione sta scompattando un contenitore (`Spread`).
    
- **Protezione dei dati:** Usa lo Spread `[...array]` o `{...oggetto}` per creare copie al volo e manipolare i dati in totale sicurezza senza intaccare gli originali.
    
- **Flessibilità totale:** Il Rest parameter elimina per sempre il bisogno di prevedere strutture fisse di parametri, lasciando la firma della funzione aperta a qualsiasi volume di input.
    

## 📖 7. **Glossario**

| **Termine Istituzionale** | **Definizione Formale**                                                                                         | **"Spiega Brutta"**                                                                                                        |
| ------------------------- | --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Rest Parameter**        | Sintassi che permette a una funzione di accettare un numero indefinito di argomenti espressi come un array.     | L'imballatore: prende tutte le variabili rimaste volanti a fine invocazione e le impacchetta dentro un array ordinato.     |
| **Spread Operator**       | Sintassi che permette di espandere un'espressione in punti in cui sono previsti argomenti o elementi multipli.  | Lo scompattatore: rompe le pareti di un array o di un oggetto e distribuisce i suoi pezzi sul tavolo.                      |
| **Shallow Copy**          | Copia superficiale di una struttura dati in cui vengono duplicati solo i valori o riferimenti di primo livello. | Una clonazione sicura per elementi semplici, ma che mantiene i collegamenti se ci sono liste complesse dentro altre liste. |
