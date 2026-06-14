

#### Modulo: JavaScript Basic-Intermediate

**Titolo:** JS Object & Array Methods: Manipolare i dati senza stress

**Data:** 24/05/2026

**📍 Indice Rapido**

1. Argomenti Lezione (I metodi fondamentali per gli Array)
    
    1.1 [[#1.1 Metodi di Trasformazione e Ciclo]]
    
    1.2 [[#1.2 Metodi di Ricerca, Accumulo e Ordinamento]]
    
1. [[#^ccb879|Risorse e Documentazione**]]
2. [[#^617b97|Key Takeaways del giorno]]
    
3. [[#^617b97|Glossario: Definizioni Istituzionali vs Spiega Brutta]]

### 1. Argomento Lezione

> **Nota del docente:** In JavaScript, quasi tutto è un oggetto. Gli Array sono oggetti speciali che contengono liste di dati. Per manipolarli non useremo più i vecchi cicli `for` manuali, ma dei metodi nativi molto più comodi e potenti. Vediamoli insieme!

#### 1.1 Metodi di Trasformazione e Ciclo

**1.1.1 Il metodo `forEach`**

Serve per passare in rassegna (esaminare) ogni singolo elemento dell'array, uno dopo l'altro. Immaginatelo come un postino che consegna una lettera a ogni casa di una via. Non crea un nuovo array, si limita a fare qualcosa per ogni elemento.

- **Sintassi ed Esempio:**

```JavaScript
const studenti = ["Anna", "Luca", "Marco"];

studenti.forEach(function(studente) {
  console.log("Buongiorno " + studente);
});
// Risultato in console:
// "Buongiorno Anna"
// "Buongiorno Luca"
// "Buongiorno Marco"
```

**1.1.2 Il metodo `map`**

Questo metodo prende un array, modifica **ogni singolo elemento** secondo le nostre istruzioni e genera un **nuovo array** della stessa identica lunghezza. L'array originale non viene toccato.

- **Sintassi ed Esempio:**

```JavaScript
const prezziInEuro = [10, 20, 30];

const prezziRaddoppiati = prezziInEuro.map(function(prezzo) {
  return prezzo * 2;
});

console.log(prezziRaddoppiati); 
// Risultato: [20, 40, 60]
```

**1.1.3 Il metodo `filter`**

Funziona come un vero e proprio setaccio. Passa al setaccio l'array e tiene solo gli elementi che superano una determinata condizione (cioè quando la condizione è `true`). Anche questo genera un **nuovo array**.

- **Sintassi ed Esempio:**

```JavaScript
const voti = [18, 25, 30, 15, 22];

const votiSufficienti = voti.filter(function(voto) {
  return voto >= 18;
});

console.log(votiSufficienti);
// Risultato: [18, 25, 30, 22] (il 15 è stato scartato)
```

___
#### 1.2 Metodi di Ricerca, Accumulo e Ordinamento

**1.2.1 Il metodo `find`**

Vi serve a cercare un elemento specifico dentro l'array. Scorrerà la lista e vi restituirà **solo il primo elemento** che soddisfa la vostra condizione. Appena lo trova, si ferma. Se non trova nulla, restituisce `undefined`.

- **Sintassi ed Esempio:**

```JavaScript
const utenti = ["Luca", "Sofia", "Marco", "Sofia"];

const utenteCercato = utenti.find(function(nome) {
  return nome === "Sofia";
});

console.log(utenteCercato);
// Risultato: "Sofia" (trova la prima e si ferma)
```

**1.2.2 Il metodo `reduce`**

Il più potente ma anche il più "nascosto". Serve a prendere tutti i valori di un array e **accumularli** in un unico risultato finale (che può essere un numero, una stringa o persino un altro oggetto). Richiede un "accumulatore" (dove si salvano i passaggi) e il valore corrente.

- **Sintassi ed Esempio:**

```JavaScript
const carrelloSpesa = [5, 12, 3];

// Lo 0 alla fine è il valore di partenza dell'accumulatore
const totaleSpesa = carrelloSpesa.reduce(function(accumulatore, prezzoCorrente) {
  return accumulatore + prezzoCorrente;
}, 0);

console.log(totaleSpesa);
// Risultato: 20
```

**1.2.3 Il metodo `sort`**

Serve a ordinare gli elementi di un array. Attenzione: di base ordina le cose come se fossero stringhe (in ordine alfabetico), quindi con i numeri fa confusione se non gli diamo una "funzione di confronto" per spiegargli come calcolare chi viene prima. Modifica l'array originale!

- **Sintassi ed Esempio:**

Con le parole, `sort` è bravissimo anche da solo. Di base, ordina gli elementi in **ordine alfabetico** (dalla A alla Z).

``` js
const invitati = ["Stefano", "Anna", "Beatrice", "Zeno"];

// Usiamo sort senza passare nessuna funzione
invitati.sort();

console.log(invitati);
// Risultato: ["Anna", "Beatrice", "Stefano", "Zeno"]

```

Qui non serve spiegargli nulla. JavaScript guarda la prima lettera di ogni parola e le mette in fila come sul registro di classe.

---

``` js
//Ordine Crescente
const votiCorretti = [5, 10, 2, 25];

votiCorretti.sort(function(a, b) {
  return a - b; // Sottrazione magica per l'ordine crescente
});

console.log(votiCorretti);
// Risultato CORRETTO: [2, 5, 10, 25]



//Ordine Decrescente
const classificaVoti = [5, 10, 2, 25];

classificaVoti.sort(function(a, b) {
  return b - a; // Sottrazione invertita per l'ordine decrescente
});

console.log(classificaVoti);
// Risultato CORRETTO: [25, 10, 5, 2] 🚀
```
**La regola per i numeri:** >  Se fai `a - b` e il risultato è **negativo**, significa che `a` è più piccolo e deve stare prima.

- Se fai `return a - b` ordini dal **più piccolo al più grande**.
    
- Se fai `return b - a` ordini dal **puù grande al più piccolo**.



---


**🔗 Risorse e Documentazione** ^ccb879

- 📚 **MDN Web Docs:** [Guida completa agli Array JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
    
- 🏫 **W3Schools:** [Esercizi interattivi sui metodi degli Array](https://www.w3schools.com/jS/js_array_methods.asp)
    

___

**🚀 Key Takeaways del Giorno** ^617b97

_I punti fondamentali da portarsi a casa_

- **Immutabilità:** Metodi come `map` e `filter` non rovinano i dati di partenza, creano una copia modificata. Usateli per evitare bug!
    
- **Il Ritorno (`return`):** Dentro le funzioni di questi metodi, ricordatevi sempre il `return`. Senza di esso, JavaScript si perde e restituisce `undefined`.
    
- **Scegli lo strumento giusto:** Non usate sempre `forEach`. Se dovete trasformare dati usate `map`, se dovete scartarli usate `filter`.
    


___
**📖 Glossario dei Metodi**

_Lista definizioni istituzionali con testo a fronte di "spiega brutta"._

|**Termine Istituzionale**|**Definizione Formale**|**"Spiega Brutta"**|
|---|---|---|
|**Callback Function**|Una funzione passata come argomento all'interno di un altro metodo che viene eseguita in un secondo momento.|Le istruzioni dettagliate che diamo al metodo (es: "moltiplica per 2") da applicare sugli elementi.|
|**Iterazione**|L'atto di ripetere un processo o scorrere una lista di elementi sequenzialmente.|Fare il giro turistico di tutto l'array dall'inizio alla fine.|
|**Accumulatore**|Variabile di supporto nel metodo `reduce` che mantiene il valore parziale calcolato nei passaggi precedenti.|Il "salvadanaio" dove metto i risultati temporanei prima di avere il totale.|