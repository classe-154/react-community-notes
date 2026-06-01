# JS Object Methods

📅 **Modulo:** JavaScript Basic-Intermediate & Advanced

**Titolo:** Manuale Definitivo sui Metodi degli Array: Manipolare i dati senza stress 

---

### 📍 Indice Rapido

1. [1.1 forEach (L'Iteratore Massivo)](#11-foreach-literatore-massivo)
2. [1.2 map (La Fabbrica di Dati)](#12-map-la-fabbrica-di-dati)
3. [1.3 filter (Il Selettore)](#13-filter-il-selettore)
4. [1.4 find (Il Cercatore)](#14-find-il-cercatore)
5. [1.5 reduce (Il Calcolatore)](#15-reduce-il-calcolatore)
6. [1.6 sort (L'Ordinatore)](#16-sort-lordinatore)
7. [🔗 Risorse e Documentazione](#-risorse-e-documentazione)
8. [🚀 Key Takeaways del Giorno](#-key-takeaways-del-giorno)
9. [📖 Glossario dei Metodi](#-glossario-dei-metodi)

---

### 📑 Corpo Centrale

#### [1.1 forEach (L'Iteratore Massivo)](#-indice-rapido)

Il metodo `forEach` è un'operazione nativa progettata per scorrere un array ed eseguire una funzione di callback su ogni singolo elemento, in modo automatico e pulito. Immaginalo come un postino che consegna una lettera a ogni casa di una via.

- **Visualizzazione Mentale:** Un nastro trasportatore porta i dati davanti a un operatore che esegue un'azione (es. stampare un nome) su ogni elemento che passa, senza però creare un nuovo contenitore.
- **Sintassi Base:**
    
    ```JavaScript
    array.forEach((elemento, indice, arrayOriginale) => {
      // Azione da compiere
    });
    ```
    
- **Casi d'uso con Array di Oggetti:** È ideale per generare "effetti collaterali" come stampare dati a schermo o modificare tag nel DOM.
    ```JavaScript
    const prodotti = [
        { id: 1, nome: "Smartphone", prezzo: 700, categoria: "Elettronica", inStock: true },
        { id: 2, nome: "Laptop", prezzo: 1200, categoria: "Elettronica", inStock: false },
        { id: 3, nome: "Giacca a vento", prezzo: 120, categoria: "Abbigliamento", inStock: true },
        { id: 4, nome: "Cuffie Wireless", prezzo: 80, categoria: "Elettronica", inStock: true },
        { id: 5, nome: "Zaino", prezzo: 50, categoria: "Abbigliamento", inStock: true }
    ];
    // Stampiamo una stringa di log per ogni prodotto disponibile 
    prodotti.forEach(prodotto => {
    if (prodotto.inStock) {
    console.log(`Disponibile: ${prodotto.nome} a soli €${prodotto.prezzo}`);
    }
    });
    ```

- **⚠️ Errori comuni:**
    - Cercare di salvare il risultato: `forEach` ritorna sempre `undefined`.
    - Usare `break` o `continue`: non è possibile interrompere il ciclo forEach; deve scorrere tutto l'array.

#### [1.2 map (La Fabbrica di Dati)](#-indice-rapido)

Se `forEach` è l'operaio, `.map()` è la fabbrica. Il suo scopo è **trasformare** ogni elemento di un array per generarne uno **nuovo** della stessa identica lunghezza, lasciando l'originale intatto (immutabilità).

- **Visualizzazione Mentale:** Una catena di montaggio dove ogni oggetto grezzo entra in una macchina (callback), viene trasformato e depositato in una nuova scatola (nuovo array).
- **Sintassi Base:**
    
    ```JavaScript
    const nuovoArray = array.map(elemento => {
      return elemento * 2; // Trasformazione
    });
    ```
    
- **Casi d'uso con Array di Oggetti:** Isolare specifiche proprietà (es. estrarre solo gli ID degli utenti) o clonare oggetti aggiungendo nuove informazioni (es. calcolo prezzi con IVA).
    ```JavaScript
    // 1. Il nostro array di dati iniziale (i record degli utenti)
    const utenti = [
    { id: 101, nome: "Alice", email: "alice@example.com", attivo: true },
    { id: 102, nome: "Bob", email: "bob@example.com", attivo: false },
    { id: 103, nome: "Charlie", email: "charlie@example.com", attivo: true },
    { id: 104, nome: "Diana", email: "diana@example.com", attivo: true }
    ];

    // 2. Applichiamo .map() per estrarre solo la proprietà 'id'
    const listaId = utenti.map(utente => utente.id);

    // 3. Verifichiamo il risultato
    console.log(listaId); 
    // Output: [101, 102, 103, 104]
    ```

- **⚠️ Errori comuni:**
    - Dimenticare il `return`: se non restituisci un valore, il nuovo array sarà pieno di `undefined`.
    - Modificare l'originale: se si manipolano oggetti annidati senza cautela, si rischia di alterare la sorgente.

#### [1.3 filter (Il Selettore)](#-indice-rapido)

Mentre `map` trasforma, `.filter()` agisce come un setaccio. Crea un nuovo array contenente solo gli elementi che superano una determinata condizione logica (restituiscono `true`).

- **Visualizzazione Mentale:** Sopra un nastro trasportatore c'è un filtro che fa cadere nel nuovo contenitore solo gli oggetti che rispettano una regola (es. "solo elettronica").
- **Sintassi Base:**
    
    ```JavaScript
    const filtrati = array.filter(elemento => elemento.proprieta > valore);
    ```
    
- **Casi d'uso con Array di Oggetti:** Estrarre record specifici, come utenti attivi o prodotti in una determinata fascia di prezzo.
    ```JavaScript
    // 1. Il nostro array di dati iniziale (i record degli utenti)
    const utenti = [
    { id: 1, nome: "Alice", email: "alice@example.com", attivo: true },
    { id: 2, nome: "Bob", email: "bob@example.com", attivo: false },
    { id: 3, nome: "Carlo", email: "carlo@example.com", attivo: true },
    { id: 4, nome: "Diana", email: "diana@example.com", attivo: false },
    { id: 5, nome: "Elena", email: "elena@example.com", attivo: true }
    ];

    // 2. Applichiamo il metodo .filter()
    const utentiAttivi = utenti.filter(utente => utente.attivo);

    // 3. Vediamo il risultato in console
    console.log("Utenti Attivi:", utentiAttivi);
    /* Output: 
    [
    { "id": 1, "nome": "Alice", "email": "alice@example.com", "attivo": true },
    { "id": 3, "nome": "Carlo", "email": "carlo@example.com", "attivo": true },
    { "id": 5, "nome": "Elena", "email": "elena@example.com", "attivo": true }
    ]
    */
    ```
- **⚠️ Errori comuni:**
    - Gestione assenza risultati: se nessun elemento soddisfa la condizione, restituisce un array vuoto `[]`, non `null`.
    - Confondere test con trasformazione: non usare `filter` per modificare dati, usalo solo per selezionarli.

#### [1.4 find (Il Cercatore)](#-indice-rapido)

A differenza di `filter`, che raccoglie tutti i risultati, `.find()` è un cercatore che si ferma al **primo** elemento che soddisfa la condizione.

- **Visualizzazione Mentale:** Una caccia al tesoro dove, appena trovi la prima scheda corretta nell'archivio, la prendi e smetti di cercare.
- **Sintassi Base:**
    
    ```JavaScript
    const trovato = array.find(elemento => elemento.id === 102);
    ```
    
- **Casi d'uso con Array di Oggetti:** Recuperare un singolo record univoco tramite ID o codice a barre.
    ```JavaScript
    // 1. Il nostro array di dati iniziale (i record degli utenti)
    const utenti = [
    { id: 1, nome: "Alice", email: "alice@example.com", attivo: true },
    { id: 2, nome: "Bob", email: "bob@example.com", attivo: false },
    { id: 3, nome: "Carlo", email: "carlo@example.com", attivo: true },
    { id: 4, nome: "Diana", email: "diana@example.com", attivo: false },
    { id: 5, nome: "Elena", email: "elena@example.com", attivo: true }
    ];

    // 2. Vogliamo recuperare l'utente univoco con ID uguale a 3
    const idDaCercare = 3;
    const utenteTarget = utenti.find(utente => utente.id === idDaCercare);

    // 3. Vediamo il risultato in console
    console.log("Utente trovato:", utenteTarget);
    ```


- **⚠️ Errori comuni:**
    - Accedere a proprietà di `undefined`: se non trova nulla, restituisce `undefined`. Accedere a proprietà di un oggetto inesistente causerà un crash.
    - Usarlo per liste: se ti aspetti più risultati, usa `filter`, poiché `find` restituisce solo il primo.

#### [1.5 reduce (Il Calcolatore)](#-indice-rapido)

È il metodo più potente e trasforma l'intero array in un **singolo valore finale** (numero, stringa o oggetto).

- **Visualizzazione Mentale:** Una palla di neve che rotola: parte da un valore iniziale e accumula dati a ogni giro.
- **Sintassi Base:**
    
    ```JavaScript
    const risultato = array.reduce((acc, curr) => {
      return acc + curr.valore;
    }, 0); // 0 è il valore iniziale
    ```
    
- **Casi d'uso con Array di Oggetti:** Calcolare il totale di un carrello o raggruppare dati in un oggetto riassuntivo.
    ```JavaScript
        // 1. Definiamo il carrello della spesa
        const carrello = [
            { prodotto: "Smartphone", prezzo: 599.99, quantita: 1 },
            { prodotto: "Cover protettiva", prezzo: 19.99, quantita: 2 },
            { prodotto: "Cavo USB-C", prezzo: 9.50, quantita: 3 },
            { prodotto: "Auricolari Bluetooth", prezzo: 49.90, quantita: 1 }
        ];

        // 2. Utilizziamo reduce per calcolare il totale
        const totaleCarrello = carrello.reduce((accumulatore, articoloCorrente) => {
            // Calcoliamo il costo per il singolo articolo (prezzo * quantità)
            const costoArticolo = articoloCorrente.prezzo * articoloCorrente.quantita;
  
            // Sommiamo il costo all'accumulatore e lo restituiamo per il prossimo ciclo
            return accumulatore + costoArticolo;
        }, 0); // <--- IMPORTANTE: 0 è il valore iniziale dell'accumulatore

        // 3. Mostriamo il risultato
        console.log(`Il totale del carrello è: €${totaleCarrello.toFixed(2)}`);
        // Output: Il totale del carrello è: €717.87
    ```
- **⚠️ Errori comuni:**
    - Dimenticare il valore iniziale: senza di esso, `reduce` usa il primo elemento dell'array come accumulatore, causando errori con gli oggetti.
    - Non restituire l'accumulatore: è obbligatorio scrivere `return acc` alla fine della callback.

#### [1.6 sort (L'Ordinatore)](#-indice-rapido)

Organizza gli elementi dell'array in base a un criterio specifico. Di base ordina come stringhe, quindi per i numeri serve una regola esplicita.

- **Visualizzazione Mentale:** Un arbitro che chiama due corridori alla volta (a, b), li confronta e decide chi deve stare davanti.
- **Sintassi Base:**
    
    ```JavaScript
    // Numeri (Crescente: a-b, Decrescente: b-a)
    array.sort((a, b) => a - b);
    
    // Stringhe (Alfabetico)
    array.sort((a, b) => a.nome.localeCompare(b.nome));
    ```
- **Casi d'uso con Array di Oggetti:** Ordinamento per Stringhe (Ordine Alfabetico) o Ordinamento per Numeri (Crescente / Decrescente).
    ```JavaScript
    const utenti = [
    { id: 1, nome: "Marco" },
    { id: 2, nome: "Anna" },
    { id: 3, nome: "Luigi" },
    { id: 4, nome: "Sofia" }
    ];

    // Ordina in base alla stringa 'nome' (dalla A alla Z)
    utenti.sort((a, b) => a.nome.localeCompare(b.nome));

    console.log("Utenti ordinati per nome:", utenti);
    /* Output:
    [
    { id: 2, nome: 'Anna' },
    { id: 3, nome: 'Luigi' },
    { id: 1, nome: 'Marco' },
    { id: 4, nome: 'Sofia' }
    ]
    */
    ```

    ```JavaScript
    const prodotti = [
    { id: 101, articolo: "Tastiera", prezzo: 45 },
    { id: 102, articolo: "Mouse", prezzo: 25 },
    { id: 103, articolo: "Monitor", prezzo: 220 },
    { id: 104, articolo: "Cuffie", prezzo: 80 }
    ];

    // Ordina in base al numero 'prezzo' (dal più basso al più alto)
    prodotti.sort((a, b) => a.prezzo - b.prezzo);

    console.log("Prodotti ordinati per prezzo:", prodotti);
    /* Output:
    [
    { id: 102, articolo: 'Mouse', prezzo: 25 },
    { id: 101, articolo: 'Tastiera', prezzo: 45 },
    { id: 104, articolo: 'Cuffie', prezzo: 80 },
    { id: 103, articolo: 'Monitor', prezzo: 220 }
    ]
    */
    ```
- **⚠️ Errori comuni:**
    - Mutabilità: `.sort()` modifica l'array originale. Per evitarlo, lavora su una copia: `[...array].sort()`.
    - Dimenticare la funzione di confronto con gli oggetti: JavaScript non sa autonomamente quale proprietà dell'oggetto usare per l'ordine.

---

### [🔗 Risorse e Documentazione](#-indice-rapido)

- 📚 **MDN Web Docs:** Guide complete per [forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach), [map](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/map), [filter](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), [find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find), [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) e [sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort).
- 🏫 **W3Schools:** Esercizi interattivi sui metodi degli Array.
- ⚛️ **React Documentation:** Rendering di liste tramite `.map()`.

---

### [🚀 Key Takeaways del Giorno](#-indice-rapido)

- **Immutabilità:** Metodi come `map` e `filter` creano copie e non rovinano i dati di partenza; `sort` invece è mutabile e modifica l'originale.
- **Il Ritorno (`return`):** Nelle callback di `map`, `filter`, `find` e `reduce`, il `return` è essenziale. Senza di esso, si ottengono risultati errati o `undefined`.
- **Scelta dello Strumento:**
    - Usa `forEach` per azioni (effetti collaterali).
    - Usa `map` per trasformazioni 1:1.
    - Usa `filter` per scartare dati e `find` per cercarne uno solo.
    - Usa `reduce` per aggregare dati in un unico risultato.

---

### [📖 Glossario dei Metodi](#-indice-rapido)

|Termine Istituzionale|Definizione Formale|"Spiega Brutta"|
|:--|:--|:--|
|**forEach**|Esegue una funzione di callback per ogni elemento dell'array.|Un passino automatico che applica un ordine a ogni riga, una dopo l'altra.|
|**map**|Crea un nuovo array popolato dai risultati della trasformazione di ogni elemento.|Una catena di montaggio: prendi gli oggetti e crei una nuova lista "sfornata".|
|**filter**|Crea un nuovo array con gli elementi che superano un test logico.|Un setaccio: tieni solo i dati che rispettano la tua regola.|
|**find**|Restituisce il valore del primo elemento che soddisfa la funzione di test.|Caccia al tesoro: cerca il primo oggetto che va bene, te lo dà e smette di cercare.|
|**reduce**|Esegue una funzione riduttrice risultando in un singolo valore di output.|Il raccoglitore: riassume l'intero array in un unico risultato finale.|
|**sort**|Ordina gli elementi di un array in base a un criterio specifico.|L'organizzatore: mette in fila i dati secondo la tua regola.|
|**Callback Function**|Funzione passata come argomento per essere eseguita in un secondo momento.|La busta con le istruzioni dettagliate su cosa fare quando arriva il proprio turno.|
|**Immutabilità**|Paradigma in cui le strutture dati non vengono modificate ma sostituite da nuove.|Guardare ma non toccare: se vuoi cambiare qualcosa, fanne una fotocopia.|
|**Accumulatore**|Variabile che mantiene il valore parziale durante un ciclo `reduce`.|Il "salvadanaio" dove metti i risultati temporanei prima del totale.|
|**localeCompare**|Metodo per confrontare stringhe in base all'ordine alfabetico locale.|Lo specialista delle parole: sa gestire accenti e maiuscole nell'ordine alfabetico.|