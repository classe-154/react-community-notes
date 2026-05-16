## Cosa sono gli stati?

Gli stati sono delle variabili che mantengono il valore anche dopo i render, ma possono anche essere la causa scatenante del rendere, dato che per React gli stati sono un dei modi che ha per capire se qualcosa é cambiato all'interno del `DOM`, quindi le variabili di stato sono dei `Trigger`, che possiamo usare per aggiornare la pagina.


## Sintassi

```js
// dobbiamo prima importare l'hook useState da react
import {useState} from "react"

// l'hook della useState restituisce sempre un array 
const [active, setActive] = useState(false);
```

## Spiegazione della sintassi

per fare `import` della `useState` dobbiamo usare le parentesi `{ nome dell' hook }`,

Quando dobbiamo dichiarare la nostra variabile di stato dobbiamo seguire delle regole ben precise per far si che il nostro stato funzioni correttamente:

- Dobbiamo sempre usare il **destructuring** dato che la `useState` restituisce sempre e solo un array con 2 valori  `const [nomeVariavile, funzionePerModificala] = useState(valore inziale della variabile)`
- il primo elemento restituito dall'array sará la nostra variabile di stato e il secondo elemento sará la funzione che utilizzeremo per modificare la variabile di stato, per convenzione il nome della funzione sará sempre "set" seguito dal nome della variabile di stato.
- finito il **destructuring** assegnamo il tutto all'hook `usestate()` e nelle parentesi andremo ad inserire il valore iniziale dello stato.

## Immutabilitá degli stati

Gli stati a differenza delle variabili normali non posso no essere modificate normalmente con una semplice assegnazione

```js
const [active, setActive] = useState(false);

active = true; //SBAGLIATO ❌

setActive(true); // CORRETTO ✅


//////////////////////////////////////////////

const [nome, setNome] = useState("Mario");

nome = "Luca"; //SBAGLIATO ❌

setNome("Luca") // CORRETTO ✅
```

Questo perché se modifichiamo direttamente lo stato React non si accorge del cambiamento della variabile e non aggiornarebbe la pagina.
per questo motivo in ogni dichiarazione di una variabile di stato abbbiamo anche una funzione (quella che inzia sempre con il "set" per convenzione), dato che non possiamo modificare gli stati direttamente dobbiamo sfruttare la funzione associata e passare come argomento della funzione il valore nuovo o una variabile che contenga il valore nuovo.

### casi piú complessi

```js
const [listaFrutta, setListaFrutta] = useState(["mela","pera","banana"]);

listaFrutta.push("kiwi"); //SBAGLIATO ❌
////////////////////////////////////////////

const [listaFrutta, setListaFrutta] = useState(["mela","pera","banana"]);

const nuovaListaFrutta = [...listaFrutta, "kiwi"];
setListaFrutta(nuovaListaFrutta);// CORRETTO ✅

// oppure se si vuole evitare variabili d'appoggio
setListaFrutta([...listaFrutta, "kiwi"]);// CORRETTO ✅

```