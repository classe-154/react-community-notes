# Timing Functions

📅 **Modulo:** JavaScript Basic-Intermediate

**Titolo:** JS Timing Functions e Programmazione Sincrona/Asincrona 

### 📍 Indice Rapido

1. [Programmazione Sincrona](#11-programmazione-sincrona)
2.  [Programmazione Asincrona](#12-programmazione-asincrona)
3.  [Perché JavaScript usa il codice asincrono](#13-perché-javascript-usa-il-codice-asincrono)
4.  [Cosa sono le Timing Functions](#14-cosa-sono-le-timing-functions)
5. [setTimeout](#15-settimeout)
6. [setInterval](#16-setinterval)
7. [clearTimeout e clearInterval](#17-cleartimeout-e-clearinterval)
8. [Risorse e Documentazione](#risorse-e-documentazione)
9. [Key Takeaways del Giorno](#key-takeaways-del-giorno)
10. [Glossario Unificato](#glossario-unificato)
11. [Errori Comuni da Evitare](#errori-comuni-da-evitare)
12. [Mini Riepilogo Finale](#mini-riepilogo-finale)

---

### 📑 Corpo Centrale

**Nota introduttiva:** In JavaScript non tutto il codice viene eseguito nello stesso modo. Alcune istruzioni vengono eseguite subito, in ordine, mentre altre vengono programmate per essere completate più tardi, senza bloccare il resto del programma. Possiamo decidere di eseguire una funzione dopo un certo tempo oppure ripeterla regolarmente usando funzioni native che lavorano con il tempo.

---

#### [1.1 Programmazione Sincrona](#-indice-rapido)

La programmazione **sincrona** esegue le istruzioni una dopo l'altra, seguendo rigorosamente l'ordine del codice scritto nel file, dall'alto verso il basso. Ogni istruzione aspetta che quella precedente sia terminata prima di iniziare.

> **Spiega Brutta:** Il codice sincrono è come una fila alla cassa del supermercato: prima passa una persona, poi la seconda, poi la terza. Nessuno salta la fila.

```JavaScript
console.log("A");
console.log("B");
console.log("C");
// Risultato: A, poi B, poi C.
```

---

#### [1.2 Programmazione Asincrona](#-indice-rapido)

La programmazione **asincrona** permette di avviare un'operazione che verrà completata più tardi, permettendo al programma di continuare con le istruzioni successive senza fermarsi. Questo tipo di codice è fondamentale per gestire operazioni che richiedono tempo senza "congelare" l'interfaccia utente.

> **Spiega Brutta:** Il codice asincrono è come ordinare una pizza: fai l'ordine, ma mentre aspetti che sia pronta puoi apparecchiare o fare altro. Non resti immobile davanti al forno.

---

#### [1.3 Perché JavaScript usa il codice asincrono](#-indice-rapido)

JavaScript utilizza l'asincronia perché molte operazioni non sono istantanee. Se JavaScript fosse solo sincrono, la pagina diventerebbe lenta o inutilizzabile durante l'attesa di:

- Timer (es. aspettare 5 secondi).
- Scaricamento di dati da un server o chiamate API.
- Lettura di informazioni da un database.
- Gestione di eventi dell'utente (click, input).

L'asincronia permette al programma di rimanere reattivo mentre queste operazioni vengono elaborate "sullo sfondo".

---

####[ 1.4 Cosa sono le Timing Functions](#-indice-rapido)

Le **Timing Functions** rappresentano il ponte pratico verso l'asincronia: sono funzioni native che permettono di programmare l'esecuzione di codice nel tempo. Le due più importanti sono `setTimeout()` e `setInterval()`. Il tempo in queste funzioni viene sempre espresso in **millisecondi** (1000ms = 1 secondo).

---

#### [1.5 setTimeout](#-indice-rapido)

Il metodo `setTimeout()` serve per eseguire una funzione **una sola volta** dopo un ritardo specificato. È un esempio perfetto di comportamento non bloccante: JavaScript programma l'esecuzione e passa subito alla riga successiva.

**Sintassi:**

```JavaScript
setTimeout(callback, tempoInMillisecondi);
```

**Esempio Pratico:**

```JavaScript
console.log("Inizio");

setTimeout(() => {
    console.log("Eseguito dopo 2 secondi");
}, 2000);

console.log("Fine");

// Risultato:
// 1. Inizio
// 2. Fine
// 3. Eseguito dopo 2 secondi (dopo l'attesa)
```

**Nota importante:** Quando si passa una funzione separata, bisogna usare solo il nome senza parentesi per evitare che venga eseguita immediatamente.

- ❌ `setTimeout(mostraMessaggio(), 2000);` (Sbagliato: esegue subito)
- ✅ `setTimeout(mostraMessaggio, 2000);` (Corretto: aspetta 2 secondi)

---

#### [1.6 setInterval](#-indice-rapido)

Il metodo `setInterval()` serve per eseguire una funzione **ripetutamente** a intervalli regolari.

**Esempio Contatore:**

```JavaScript
let counter = 1;
setInterval(() => {
    console.log("Contatore: " + counter);
    counter++;
}, 1000);
// Stampa il valore ogni secondo incrementandolo.
```

---

#### [1.7 clearTimeout e clearInterval](#-indice-rapido)

Quando attiviamo un timer, JavaScript restituisce un **identificativo (Timer ID)**. Salvando questo ID in una variabile, possiamo annullare l'esecuzione.

- **clearTimeout(id):** Annulla un `setTimeout` prima che venga eseguito.
- **clearInterval(id):** Ferma definitivamente un `setInterval` che si sta ripetendo.

```JavaScript
let intervallo = setInterval(() => {
    console.log("Check...");
    if (condizioneSoddisfatta) {
        clearInterval(intervallo); // Ferma il loop
    }
}, 1000);
```

---

### [🔗 Risorse e Documentazione](#-indice-rapido)

- 📚 **MDN Web Docs:** [Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
- 📚 **MDN Web Docs:** [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout) e [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/setInterval)
- 🏫 **W3Schools:** [JavaScript Asynchronous](https://www.w3schools.com/js/js_asynchronous.asp)
- 🏫 **W3Schools:** [JavaScript Timing Events](https://www.w3schools.com/js/js_timing.asp)

---

### [🚀 Key Takeaways del Giorno](#-indice-rapido)

- Il codice **sincrono** segue l'ordine scritto; il codice **asincrono** può essere eseguito più tardi senza bloccare il programma.
- Le **Timing Functions** controllano il "quando" delle esecuzioni.
- `setTimeout` = esecuzione singola; `setInterval` = esecuzione ciclica.
- 1000 millisecondi equivalgono a 1 secondo.
- Per annullare un timer serve il suo **ID** passato a `clearTimeout` o `clearInterval`.
- Non usare le parentesi quando passi una funzione come callback a un timer se non vuoi che parta subito.

---

### [📖 Glossario dei Metodi](#-indice-rapido)

|Termine Istituzionale|Definizione Formale|Spiega Brutta|
|:--|:--|:--|
|**API**|Interfaccia per la comunicazione tra programmi diversi.|Un modo per chiedere dati a qualcun altro.|
|**Callback**|Funzione passata come argomento per essere eseguita in seguito.|Le istruzioni da eseguire quando scatta l'ora X.|
|**clearInterval**|Metodo che interrompe un intervallo ripetitivo.|Ferma una cosa che continua a ripetersi.|
|**clearTimeout**|Metodo che annulla un timeout programmato.|Cancella una sveglia prima che suoni.|
|**Millisecondo**|Unità di misura pari a un millesimo di secondo.|1000 di questi fanno 1 secondo.|
|**Operazione Bloccante**|Operazione che ferma il programma finché non termina.|Una cosa che crea il "tappo" in una fila.|
|**Operazione Non Bloccante**|Operazione che permette al programma di proseguire.|Una cosa che parte ma non ferma il traffico.|
|**Programmazione Asincrona**|Esecuzione rimandata senza bloccare il flusso principale.|Faccio partire una cosa e intanto faccio altro.|
|**Programmazione Sincrona**|Esecuzione sequenziale e ordinata delle istruzioni.|Una fila ordinata: una cosa alla volta.|
|**setInterval**|Esegue una callback ripetutamente ogni tot millisecondi.|Ogni X secondi rifà la stessa azione.|
|**setTimeout**|Esegue una callback una sola volta dopo un ritardo.|Aspetta un po' e poi agisci una volta sola.|
|**Timer**|Meccanismo di esecuzione differita nel tempo.|Una sveglia che attiva una funzione.|
|**Timer ID**|Identificativo numerico restituito dalle timing functions.|Lo "scontrino" per annullare quel timer specifico.|
|**Timing Function**|Funzione per programmare codice nel tempo.|Funzione che dice: "fai questo dopo".|

---

### [❌Errori Comuni da Evitare](#-indice-rapido)

- ❌ **Confondere l'ordine di scrittura con quello di esecuzione:** Pensare che `setTimeout` blocchi il codice successivo. In realtà, JavaScript continua a leggere e l'output asincrono arriverà solo alla fine del tempo impostato.
- ❌ **Invocare la funzione nel timer:** Scrivere `setTimeout(miaFunzione(), 1000)`. Questo esegue `miaFunzione` immediatamente. La sintassi corretta non vuole le parentesi: `setTimeout(miaFunzione, 1000)`.
- ❌ **Dimenticare di fermare setInterval:** Un intervallo non si ferma da solo e continuerà a consumare risorse finché la pagina è aperta se non viene chiamato `clearInterval`.

---

### [📝Mini Riepilogo Finale](#-indice-rapido)

- Usa **setTimeout** per un ritardo singolo e **setInterval** per la ripetizione.
- Usa i rispettivi **clear** per fermarli tramite l'ID restituito.
- Ricorda: l'asincronia serve a mantenere la pagina fluida e reattiva.
- Quando leggi codice asincrono, chiediti sempre: "Questa istruzione parte subito o è rimandata a dopo?".