#### Modulo: JavaScript Basic-Intermediate

**Titolo:** JS Timing Functions: `setTimeout` e `setInterval`

**Data:** 01/06/2026

**📍 Indice Rapido**

1. Argomento Lezione: Timing Functions

    1.1 [[#1.1 Cosa sono le Timing Functions]]

    1.2 [[#1.2 setTimeout]]

    1.3 [[#1.3 setInterval]]

    1.4 [[#1.4 clearTimeout e clearInterval]]

2. [[#risorse-e-documentazione|Risorse e Documentazione]]

3. [[#key-takeaways-del-giorno|Key Takeaways del giorno]]

4. [[#glossario-definizioni-istituzionali-vs-spiega-brutta|Glossario: Definizioni Istituzionali vs Spiega Brutta]]

---

### 1. Argomento Lezione

> **Nota introduttiva:**  
> In JavaScript possiamo decidere di eseguire una funzione dopo un certo tempo oppure ripeterla ogni tot secondi.  
> Per farlo usiamo le **Timing Functions**, cioè funzioni native che lavorano con il tempo.

---

#### 1.1 Cosa sono le Timing Functions

Le **Timing Functions** permettono di programmare l'esecuzione di una funzione nel tempo.

Le due più importanti sono:

- `setTimeout()`
- `setInterval()`

La differenza principale è questa:

- `setTimeout()` esegue una funzione **una sola volta** dopo un ritardo;
- `setInterval()` esegue una funzione **più volte**, a intervalli regolari.

Il tempo viene scritto in **millisecondi**.

```js
1000 // 1 secondo
2000 // 2 secondi
3000 // 3 secondi
```

---

#### 1.2 setTimeout

Il metodo `setTimeout()` serve per eseguire una funzione **una sola volta**, dopo un certo tempo.

##### Sintassi base

```js
setTimeout(callback, tempoInMillisecondi);
```

Dove:

- `callback` è la funzione da eseguire;
- `tempoInMillisecondi` è il tempo di attesa prima dell'esecuzione.

##### Esempio

```js
setTimeout(function () {
    console.log("Sono passati 2 secondi");
}, 2000);
```

Risultato:

```js
"Sono passati 2 secondi"
```

JavaScript aspetta 2 secondi e poi esegue la funzione.

---

##### Esempio con funzione separata

```js
function mostraMessaggio() {
    console.log("Benvenuto nella pagina!");
}

setTimeout(mostraMessaggio, 3000);
```

In questo caso la funzione `mostraMessaggio` viene eseguita dopo 3 secondi.

Attenzione: quando passiamo una funzione a `setTimeout`, non dobbiamo mettere le parentesi.

Corretto:

```js
setTimeout(mostraMessaggio, 3000);
```

Sbagliato:

```js
setTimeout(mostraMessaggio(), 3000);
```

Nel secondo caso la funzione viene eseguita subito, senza aspettare.

---

#### 1.3 setInterval

Il metodo `setInterval()` serve per eseguire una funzione **ripetutamente**, ogni tot millisecondi.

##### Sintassi base

```js
setInterval(callback, tempoInMillisecondi);
```

##### Esempio

```js
setInterval(function () {
    console.log("Messaggio ripetuto ogni secondo");
}, 1000);
```

Risultato:

```js
"Messaggio ripetuto ogni secondo"
"Messaggio ripetuto ogni secondo"
"Messaggio ripetuto ogni secondo"
...
```

Il codice continua a ripetersi ogni secondo.

---

##### Esempio: contatore

```js
let counter = 1;

setInterval(function () {
    console.log(counter);
    counter++;
}, 1000);
```

Risultato:

```js
1
2
3
4
...
```

Ogni secondo viene stampato il valore di `counter`, poi il valore viene aumentato di 1.

---

#### 1.4 clearTimeout e clearInterval

Quando usiamo `setTimeout()` o `setInterval()`, JavaScript restituisce un identificativo.

Questo identificativo può essere salvato in una variabile e usato per fermare il timer.

---

##### clearTimeout

`clearTimeout()` serve per annullare un `setTimeout` prima che venga eseguito.

```js
const timeoutId = setTimeout(function () {
    console.log("Questo messaggio non verrà mostrato");
}, 3000);

clearTimeout(timeoutId);
```

In questo esempio il timeout viene cancellato prima dei 3 secondi.

---

##### clearInterval

`clearInterval()` serve per fermare un `setInterval`.

```js
let counter = 1;

const intervalId = setInterval(function () {
    console.log(counter);
    counter++;

    if (counter > 5) {
        clearInterval(intervalId);
    }
}, 1000);
```

Risultato:

```js
1
2
3
4
5
```

Quando `counter` diventa maggiore di 5, l'intervallo viene fermato.

---

### Risorse e Documentazione

- 📚 **MDN Web Docs:** [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)
- 📚 **MDN Web Docs:** [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval)
- 🏫 **W3Schools:** [JavaScript Timing Events](https://www.w3schools.com/js/js_timing.asp)

---

### Key Takeaways del Giorno

_I punti fondamentali da portarsi a casa_

- Le **Timing Functions** servono per controllare quando una funzione deve essere eseguita.
- `setTimeout()` esegue una funzione **una sola volta** dopo un ritardo.
- `setInterval()` esegue una funzione **più volte** a intervalli regolari.
- Il tempo si scrive in **millisecondi**: 1000 millisecondi equivalgono a 1 secondo.
- `clearTimeout()` annulla un timeout non ancora eseguito.
- `clearInterval()` ferma un intervallo che si ripete.
- Quando passiamo una funzione a `setTimeout` o `setInterval`, dobbiamo passare il nome della funzione senza parentesi.

---

### Glossario: Definizioni Istituzionali vs Spiega Brutta

| **Termine Istituzionale** | **Definizione Formale** | **Spiega Brutta** |
|---|---|---|
| **Timing Function** | Funzione che permette di programmare l'esecuzione di codice dopo un certo intervallo di tempo. | Una funzione che dice a JavaScript: “questa cosa falla dopo”. |
| **setTimeout** | Metodo che esegue una callback una sola volta dopo un ritardo espresso in millisecondi. | Aspetta un po', poi esegue quella funzione una volta. |
| **setInterval** | Metodo che esegue una callback ripetutamente a intervalli di tempo regolari. | Ogni tot secondi rifà la stessa cosa. |
| **clearTimeout** | Metodo che annulla un timeout precedentemente impostato. | Cancella un'azione programmata prima che parta. |
| **clearInterval** | Metodo che interrompe un intervallo precedentemente impostato. | Ferma una cosa che si sta ripetendo. |
| **Callback** | Funzione passata come argomento a un'altra funzione, per essere eseguita in un secondo momento. | Le istruzioni che consegniamo a un'altra funzione per dirle cosa fare dopo. |
| **Millisecondo** | Unità di misura del tempo equivalente a un millesimo di secondo. | 1000 millisecondi fanno 1 secondo. |
| **Timer ID** | Identificativo restituito da `setTimeout` o `setInterval`, utile per annullare o fermare il timer. | Il numero/scontrino che mi serve per cancellare quel timer. |

---

### Errori comuni da evitare

#### 1. Chiamare subito la funzione

Sbagliato:

```js
setTimeout(saluta(), 2000);
```

Corretto:

```js
setTimeout(saluta, 2000);
```

Nel primo caso la funzione viene eseguita subito.  
Nel secondo caso viene eseguita dopo 2 secondi.

---

#### 2. Dimenticare di fermare setInterval

```js
setInterval(function () {
    console.log("Continuo per sempre");
}, 1000);
```

Questo codice continua a ripetersi finché la pagina resta aperta.

Per fermarlo serve `clearInterval`.

---

### Mini riepilogo finale

- Se voglio eseguire qualcosa **dopo un ritardo**, uso `setTimeout`.
- Se voglio eseguire qualcosa **più volte**, uso `setInterval`.
- Se voglio fermare un timeout, uso `clearTimeout`.
- Se voglio fermare un intervallo, uso `clearInterval`.
