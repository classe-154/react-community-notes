#### Modulo: JavaScript Basic-Intermediate

**Titolo:** JS Programmazione Sincrona e Asincrona

**Data:** 01/06/2026

**📍 Indice Rapido**

1. Argomento Lezione: Sincrono e Asincrono

    1.1 [[#1.1 Programmazione Sincrona]]

    1.2 [[#1.2 Programmazione Asincrona]]

    1.3 [[#1.3 Perché JavaScript usa codice asincrono]]

    1.4 [[#1.4 Esempio con setTimeout]]

2. [[#risorse-e-documentazione|Risorse e Documentazione]]

3. [[#key-takeaways-del-giorno|Key Takeaways del giorno]]

4. [[#glossario-definizioni-istituzionali-vs-spiega-brutta|Glossario: Definizioni Istituzionali vs Spiega Brutta]]

---

### 1. Argomento Lezione

> **Nota introduttiva:**  
> In JavaScript non tutto il codice viene eseguito nello stesso modo.  
> Alcune istruzioni vengono eseguite subito, in ordine.  
> Altre invece vengono programmate per essere completate più tardi, senza bloccare il resto del programma.

---

#### 1.1 Programmazione Sincrona

La programmazione **sincrona** esegue le istruzioni una dopo l'altra, seguendo l'ordine del codice.

JavaScript legge il file dall'alto verso il basso.

```js
console.log("A");
console.log("B");
console.log("C");
```

Risultato:

```js
"A"
"B"
"C"
```

In questo caso il codice è molto prevedibile:

1. prima stampa `"A"`;
2. poi stampa `"B"`;
3. infine stampa `"C"`.

Ogni istruzione aspetta che quella precedente sia terminata.

---

##### Spiegazione semplice

Il codice sincrono è come una fila alla cassa.

Prima passa una persona, poi la seconda, poi la terza.

Nessuno salta la fila.

---

#### 1.2 Programmazione Asincrona

La programmazione **asincrona** permette di avviare un'operazione che verrà completata più tardi, senza bloccare il resto del codice.

Esempio:

```js
console.log("A");

setTimeout(function () {
    console.log("B");
}, 2000);

console.log("C");
```

Risultato:

```js
"A"
"C"
"B"
```

Questo succede perché `setTimeout` non blocca il programma.

JavaScript dice:

> "Questa funzione la eseguo tra 2 secondi. Nel frattempo continuo con il resto del codice."

---

##### Spiegazione semplice

Il codice asincrono è come ordinare una pizza.

Tu fai l'ordine, ma non resti fermo immobile ad aspettare davanti al forno.

Nel frattempo puoi apparecchiare, bere qualcosa o fare altro.

Quando la pizza è pronta, vieni avvisato.

---

#### 1.3 Perché JavaScript usa codice asincrono

JavaScript usa spesso codice asincrono perché alcune operazioni richiedono tempo.

Esempi:

- aspettare un timer;
- scaricare dati da un server;
- leggere informazioni da un database;
- aspettare la risposta di una API;
- gestire eventi dell'utente, come click o input.

Se JavaScript si bloccasse ogni volta, la pagina diventerebbe lenta o inutilizzabile.

La programmazione asincrona permette invece al programma di continuare a funzionare mentre aspetta il completamento di alcune operazioni.

---

#### 1.4 Esempio con setTimeout

`setTimeout` è uno degli esempi più semplici per capire l'asincronia.

```js
console.log("Inizio");

setTimeout(function () {
    console.log("Operazione asincrona completata");
}, 3000);

console.log("Fine");
```

Risultato:

```js
"Inizio"
"Fine"
"Operazione asincrona completata"
```

Anche se `setTimeout` si trova prima di `console.log("Fine")`, il suo contenuto viene eseguito dopo.

Questo ci fa capire una cosa importante:

> Il codice asincrono non ferma tutto il programma.

---

### Risorse e Documentazione

- 📚 **MDN Web Docs:** [Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS)
- 📚 **MDN Web Docs:** [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)
- 🏫 **W3Schools:** [JavaScript Asynchronous](https://www.w3schools.com/js/js_asynchronous.asp)

---

### Key Takeaways del Giorno

_I punti fondamentali da portarsi a casa_

- Il codice **sincrono** viene eseguito in ordine, una riga dopo l'altra.
- Il codice **asincrono** permette di eseguire alcune operazioni più tardi.
- Il codice asincrono non blocca il resto del programma.
- `setTimeout` è un esempio semplice di comportamento asincrono.
- Anche se una funzione asincrona è scritta prima, può essere eseguita dopo.
- L'asincronia è importante per timer, eventi, chiamate API e operazioni che richiedono tempo.
- Quando leggiamo codice asincrono, dobbiamo ragionare sull'ordine reale di esecuzione, non solo sull'ordine scritto nel file.

---

### Glossario: Definizioni Istituzionali vs Spiega Brutta

| **Termine Istituzionale** | **Definizione Formale** | **Spiega Brutta** |
|---|---|---|
| **Programmazione Sincrona** | Modalità di esecuzione in cui le istruzioni vengono eseguite una dopo l'altra, in ordine. | Una fila ordinata: prima una cosa, poi l'altra. |
| **Programmazione Asincrona** | Modalità di esecuzione in cui alcune operazioni possono essere completate in un secondo momento senza bloccare il programma. | Faccio partire una cosa, ma nel frattempo continuo a fare altro. |
| **Operazione Bloccante** | Operazione che impedisce al programma di proseguire finché non è terminata. | Una cosa che blocca tutti finché non finisce. |
| **Operazione Non Bloccante** | Operazione che permette al programma di continuare mentre viene completata. | Una cosa che parte, ma non ferma il resto. |
| **Callback** | Funzione passata come argomento a un'altra funzione, per essere eseguita in un secondo momento. | Le istruzioni da eseguire quando arriva il momento giusto. |
| **Timer** | Meccanismo che permette di eseguire codice dopo un certo periodo di tempo. | Una sveglia che fa partire una funzione. |
| **API** | Interfaccia che permette a programmi diversi di comunicare tra loro. | Un modo per chiedere dati o servizi a qualcun altro. |

---

### Errori comuni da evitare

#### 1. Pensare che il codice venga sempre eseguito dall'alto verso il basso

```js
console.log("A");

setTimeout(function () {
    console.log("B");
}, 1000);

console.log("C");
```

Il risultato non è:

```js
"A"
"B"
"C"
```

Ma è:

```js
"A"
"C"
"B"
```

Perché la funzione dentro `setTimeout` viene eseguita dopo.

---

#### 2. Pensare che setTimeout metta in pausa tutto

`setTimeout` non ferma JavaScript.

```js
console.log("Prima");

setTimeout(function () {
    console.log("Durante");
}, 2000);

console.log("Dopo");
```

Risultato:

```js
"Prima"
"Dopo"
"Durante"
```

JavaScript continua a leggere il codice successivo.

---

### Mini riepilogo finale

- Il codice sincrono segue l'ordine scritto nel file.
- Il codice asincrono può essere eseguito più tardi.
- L'asincronia serve per non bloccare il programma.
- Per capire l'output di un codice asincrono, bisogna chiedersi: “questa istruzione parte subito o viene rimandata a dopo?”
