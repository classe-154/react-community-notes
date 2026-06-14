#### Modulo: JavaScript  
**Titolo:** Definizione di funzione

## 📍 Indice Rapido

1. [Definizione di Funzione](#1-️-definizione-di-funzione)

2. [Anatomia di una Funzione: Allocazione vs Invocazione](#-2-anatomia-di-una-funzione-allocazione-vs-invocazione)

3. [Scelte Strutturali: Function Declaration vs Function Expression](#️-3-scelte-strutturali-function-declaration-vs-function-expression)

4. [La vera differenza tecnica: L'Hoisting](#4-la-vera-differenza-tecnica-lhoisting-sollevamento)

5. [Concetto Avanzato: L'Isolamento dello Scope](#-5-concetto-avanzato-lisolamento-dello-scope-e-i-blocchi-di-memoria)

6. [Errori comuni](#️-6-errori-comuni)

7. [Risorse e Documentazione](#-7-risorse-e-documentazione)

8. [Key Takeaways del Giorno](#-8-key-takeaways-del-giorno)

9. [Glossario](#-9-glossario)

## 1. ⚙️ Definizione di Funzione

Per evitare di duplicare blocchi di codice identici, il linguaggio introduce il concetto di **Funzione**.

A livello teorico, una funzione è un **sotto-programma**: un blocco indipendente di istruzioni a cui viene dato un nome o un punto di ancoraggio e che viene memorizzato all'interno dell'applicazione. Questo blocco rimane in uno stato "dormiente" finché non viene esplicitamente richiesto dal resto del software.

## 🧠 2. Anatomia di una Funzione: Allocazione vs Invocazione

Capire il ciclo di vita di una funzione è fondamentale per non generare bug strutturali. Questo processo si divide rigorosamente in due fasi temporali distinte:

1. **La Definizione (Allocazione in memoria):** È la fase in cui spieghi a JavaScript le istruzioni da eseguire. Scrivi la ricetta, ma non stai ancora cucinando il piatto. In questa fase, il codice all'interno della funzione **non viene eseguito**, viene solo parcheggiato nella memoria RAM del computer.
    
2. **L'Invocazione (Esecuzione o Chiamata):** È il momento in cui ordini a JavaScript di andare a prendere quel pacchetto memorizzato e di eseguire le righe di codice al suo interno. Si attiva utilizzando l'operatore di invocazione, rappresentato dalle parentesi tonde `()`.
    

```JavaScript
// Fase 1: Definizione (Il computer memorizza il blocco, ma la console è vuota)
function mostrareMessaggioIniziale() {
  console.log("Sotto-programma attivato con successo.");
}

// Fase 2: Invocazione (Il computer esegue il blocco adesso)
mostrareMessaggioIniziale(); 
```

## 🏛️ 3. Scelte Strutturali: Function Declaration vs Function Expression

In JavaScript puoi definire un punto di ancoraggio per un sotto-programma in due modi differenti. Sebbene il risultato finale sia l'esecuzione di un blocco di codice, l'interprete gestisce la memoria in modo completamente diverso:

- **Function Declaration (Dichiarazione di Funzione):** Si dichiara usando la parola chiave nativa `function` seguita da un nome identificativo. È una struttura autonoma e indipendente.
    
- **Function Expression (Espressione di Funzione):** Una funzione viene creata senza un nome (funzione anonima) e viene assegnata direttamente come valore ad una variabile o costante (`const`). La funzione diventa a tutti gli effetti il contenuto della variabile.
    

```JavaScript
// Struttura 1: Function Declaration
function definisciScrittura() {
  console.log("Esecuzione da dichiarazione standard.");
}

// Struttura 2: Function Expression
const eseguiScrittura = function() {
  console.log("Esecuzione da espressione memorizzata.");
};
```

## ⚡4. La vera differenza tecnica: L'Hoisting (Sollevamento)

JavaScript gestisce la memoria delle due strutture in modo differente:

- Le **Function Declaration** vengono caricate in memoria prima di eseguire il file. Puoi quindi invocare la funzione anche prima di averla scritta nel codice.
    
- Le **Function Expression** seguono l'ordine di lettura dall'alto verso il basso. Non puoi usarle prima della riga in cui le hai create. (Rispettano le regole di const e let, quindi vengono dichiarate ma sono ancora nella temporal dead zone fino a che l'interprete non arriva alla riga dove vengono assegnate alla variabile)
    

```JavaScript
salutaStandard(); // ✅ Funziona! (Grazie all'Hoisting)

function salutaStandard() { 
  console.log("Ciao!"); 
}


salutaVariabile(); // ❌ Errore Bloccante (ReferenceError)

const salutaVariabile = function() { 
  console.log("Ciao!"); 
};
```


## 🔒 5. Concetto Avanzato: L'Isolamento dello Scope e i Blocchi di Memoria

Un aspetto ingegneristico cruciale quando si definisce una funzione è il concetto di **Isolamento della Memoria**.

Le parentesi graffe `{ }` che delimitano il corpo di una funzione creano una barriera impenetrabile verso l'esterno. Qualsiasi variabile creata all'interno di una funzione appartiene esclusivamente a quel sotto-programma. Questa caratteristica garantisce che le funzioni non interferiscano tra loro, permettendo di riutilizzare gli stessi nomi di variabili in funzioni diverse senza il rischio di sovrascritture accidentali.

```JavaScript
let tracciamentoStato = "Globale";

function simulazioneProcesso() {
  // Questa variabile vive e muore dentro questa funzione
  let tracciamentoStato = "Locale Interno";
  console.log(tracciamentoStato); // Stampa: "Locale Interno"
}

simulazioneProcesso();
console.log(tracciamentoStato); // Stampa: "Globale" (Il file esterno è protetto)
```

## ⚠️ 6. Errori comuni

- **Confondere il riferimento con l'attivazione:** Scrivere `let simulazione = simulazioneProcesso;` (senza parentesi tonde) non esegue la funzione. Stai semplicemente creando un secondo collegamento alla ricetta. Per ordinare a JavaScript di avviare il blocco della funzione devi tassativamente usare le parentesi tonde `()`.
    
- **Tentare di accedere alle variabili interne:** Pensare che le variabili nate dentro il blocco `{ }` della funzione siano accessibili nel resto del file è un errore comune. Qualsiasi tentativo di leggere una variabile locale dall'esterno causerà un errore di tipo `ReferenceError`, bloccando l'applicazione.
    

## 🔗 7. **Risorse e Documentazione**

- 📚 MDN Web Docs (Functions): [https://developer.mozilla.org/it/docs/Web/JavaScript/Guide/Functions](https://developer.mozilla.org/it/docs/Web/JavaScript/Guide/Functions)
    

## 🚀 8. **Key Takeaways del Giorno**

- **La definizione è solo una ricetta:** Creare una funzione non significa eseguirla; il codice rimane congelato in memoria finché non viene chiamato.
    
- **Le tonde sono l'interruttore:** Il nome della funzione è solo un indirizzo di puntamento; le parentesi tonde `()` sono l'operatore che avvia l'esecuzione.
    
- **Le graffe sono uno scudo:** Tutto ciò che nasce dentro il blocco di una funzione è isolato e protetto dal mondo esterno.
    

## 📖 9. **Glossario**

| **Termine Istituzionale** | **Definizione Formale**                                                                               | **"Spiega Brutta"**                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Definizione**           | Allocazione di un blocco di istruzioni logiche all'interno dello spazio di memoria dell'applicazione. | Scrivere un pezzo di codice e metterlo da parte in una scatola nominandolo per il futuro.     |
| **Invocazione**           | Chiamata esplicita al motore di esecuzione per processare le istruzioni di una funzione.              | Aprire la scatola e dire a JavaScript: "Esegui questi comandi adesso".                        |
| **Funzione Anonima**      | Funzione priva di un identificatore testuale diretto nel suo costrutto di dichiarazione.              | Una funzione senza nome che per essere usata deve essere salvata subito dentro una variabile. |
|**Hoisting**|Comportamento di default di sollevamento delle dichiarazioni.|**"JavaScript"** che legge in anticipo le funzioni prima di avviare tutto.