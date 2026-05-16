#### Modulo: React  
**Titolo:** Guida Completa a Props 

**📍 Indice Rapido**

1. [Passaggio dei dati in react](#1-passaggio-dei-dati)
   - 1.1 [Cosa sono le Props](#11-cosa-sono-le-props)
   - 1.2 [Sintassi e Destrutturazione](#12-sintassi-e-destrutturazione)  
   - 1.3 [Props Speciali](#13-props-speciali)
2. [Organizzazione dei dati](#2-organizzazione-dei-dati)
   - 2.1 [Props Multiple](#21-props-multiple)  
   - 2.2 [Rendering di liste da oggetti tramite props e operatore ternario](#22-rendering-di-liste-da-oggetti-tramite-props-e-operatore-ternario)  
   - 2.3 [Il Props Drilling](#23-il-prop-drilling-il-passamano-delle-props)
   - 
3. [Risorse Techniche](#3--risorse-e-documentazione)
4. [Key Takeaways del giorno](#4--key-takeaways-del-giorno)
5. [Glossario: termini essenziali](#5--glossario)

### **1 Passaggio dei dati**

#### 1.1 Cosa sono le Props
Le **Props** (abbreviazione di _properties_) sono lo strumento fondamentale per far parlare i componenti tra loro. Immaginale come dei messaggi o delle istruzioni che un componente "Genitore" invia a un componente "Figlio" per dirgli cosa deve mostrare.

- **Dinamismo:** In React i componenti sono riutilizzabili; le props sono il modo principale per renderli dinamici. Senza di esse, ogni componente sarebbe una copia identica dell'altro.
    
- **Flusso Unidirezionale:** I dati viaggiano sempre e solo dal Padre al Figlio. Il Figlio riceve le props come "sola lettura" e non può modificarle direttamente.

### 💻 Esempio di Invocazione (Componente Padre)

```jsx
// src/App.jsx
import PannelloStanza from './components/PannelloStanza.jsx';

function App() {
  return (
    <main className="dashboard">
      <h1>Smart Home Control Panel</h1>
      
      {/* Caso 1: Passiamo tutte le informazioni richieste */}
      <PannelloStanza 
        stanza="Soggiorno" 
        dispositivoAttivo="Condizionatore" 
      />

      {/* Caso 2: Passiamo solo la prop obbligatoria */}
      <PannelloStanza 
        stanza="Corridoio" 
      />
    </main>
  );
}

export default App;

```
    

#### 1.2 Sintassi e Destrutturazione 
Un componente può ricevere valori come parametri. Possiamo usare l'oggetto intero o, per rendere il codice più leggibile, estrarre direttamente i valori che ci servono.

- **Azione:** Usa la destrutturazione tra le graffe `{ }` nei parametri della funzione per evitare di scrivere `props.nome` ogni volta.
    
- **Esempio:**
    

```Js
// Destrutturazione: prendiamo solo 'nome' dall'oggetto props
function Saluto({ nome }) {
  return <h1>Ciao {nome}</h1>;
}

// Nel componente Genitore (App)
function App() {
  return <Saluto nome="Luca" />;
}
```

#### 1.3 Props Speciali 
Esistono modi avanzati per gestire i dati e il contenuto annidato:

- **La prop `children`:** Contiene tutto ciò che viene inserito tra i tag di apertura e chiusura di un componente. Utile per creare "contenitori" grafici.

    Il componente non sa cosa riceverà, si limita a definire _dove_ mostrare il contenuto.

```js
	// Destructuring di children nei parametri
function Box({ children }) {
  return (
    <div className="container-esterno">
      {children}
    </div>
  );
}
```

  - **Utilizzo**
    Puoi inserire qualsiasi cosa tra i tag di apertura e chiusura di `<Box>`.

```js
function App() {
  return (
    <Box>
      <h1>Titolo dentro il box</h1>
      <p>Questo testo è passato come prop children.</p>
      <button>Clicca qui</button>
    </Box>
  );
}

```


- **Valori di Default:** Si impostano i valori predefiniti direttamente nei parametri JavaScript:

### 💻 Snippet: Destructuring Avanzato e impostazione di un valore di default

```jsx

// Estraiamo 'stanza', lo rinominiamo in 'nomeStanza' e diamo un valore di default.
// Estraiamo 'dispositivoAttivo' direttamente senza rinominarlo.
function PannelloStanza({ stanza: nomeStanza = 'Stanza Generica', dispositivoAttivo }) {
  
  // NOTA: La variabile originaria 'stanza' non è più accessibile, usiamo 'nomeStanza'
  return (
    <div className="room-card">
      <h2>{nomeStanza}</h2>
      <p>Dispositivo in funzione: {dispositivoAttivo}</p>
    </div>
  );
}

export default PannelloStanza;

```


    
- **Spread Operator (`...`):** Se hai un oggetto complesso (es. da un'API), puoi "spalmarlo" nel componente: `<UserCard {...datiUtente} />`.

  Supponiamo di ricevere questo oggetto che rappresenta un profilo utente:

```js
const datiUtente = {
  id: 1,
  nome: "Alex",
  cognome: "Rossi",
  ruolo: "Developer",
  bio: "Appassionato di React e CSS.",
  avatar: "https://link-immagine.it",
  isOnline: true
};
```

- **L'applicazione dello Spread Operator**

	Invece di scrivere `nome={datiUtente.nome}`, `ruolo={datiUtente.ruolo}`, ecc., usi le "briciole" (`...`).
	
```js
function App() {
  return (
    <div className="lista-profili">
      {/* Lo Spread "apre" l'oggetto datiUtente e passa 
        ogni chiave come se fosse una prop singola.
      */}
      <UserCard {...datiUtente} />
    </div>
  );
}
```

### **2 Organizzazione dei Dati**

#### 2.1 Props Multiple 
Un componente può gestire infiniti dati contemporaneamente, basta elencarli nella destrutturazione.

```js
function UserCard({ nome, eta }) {
  return (
    <div>
      <p>Nome: {nome}</p>
      <p>Età: {eta}</p>
    </div>
  );
}
```

#### 2.2 Rendering di Liste da Oggetti tramite Props e Operatore Ternario
Spesso i dati non arrivano dal componente Padre come singoli testi, ma come un oggetto unico e strutturato (ad esempio, un report inviato da una centralina con lo stato di vari sensori). Per gestirli in modo dinamico, il componente Figlio riceve l'oggetto tramite le props, lo trasforma in un array leggibile e lo elabora.

- **La trasformazione:** Usiamo **Object.entries()** per convertire l'oggetto ricevuto in un array di coppie [chiave,   valore]. Questo trucco rende il componente Figlio universale e pronto a ricevere qualsiasi oggetto dal Padre.

- **Ciclo e destrutturazione:** Cicliamo l'array ottenuto ed eseguiamo la destrutturazione della coppia [sistema, attivo] all'inizio del ciclo per estrarre i dati al volo.

- **Identificativi nelle Key:** Come chiave univoca richiesta da React per le liste, usiamo il nome stesso della chiave dell'oggetto (es. key={sistema}), evitando gli indici numerici che cambiano facilmente.

- **Operatore Ternario (? :):** Lo usiamo per gestire una logica binaria veloce (una sorta di if/else rapido) e scegliere cosa restituire a seconda che il valore sia vero o falso.

```js
// 1. IL COMPONENTE PADRE (App.jsx)
// Possiede i dati di telemetria grezzi e li passa al figlio dentro la prop "reportSistemi"
function App() {
  const telemetriaCasa = {
    allarmeAntifurto: true,
    rilevatoreFumo: false,
    sensoreAllagamento: false
  };

  return <StatusSicurezza reportSistemi={telemetriaCasa} />;
}

// 2. IL COMPONENTE FIGLIO (StatusSicurezza.jsx)
// Riceve l'oggetto esternamente tramite le props e lo trasforma in nodi JSX ciclabili
function StatusSicurezza({ reportSistemi }) {

  // Convertiamo l'oggetto ricevuto nelle props in un array per poter usare il .map()
  const listaSistemiJsx = Object.entries(reportSistemi).map(([sistema, attivo]) => {
    return (
      // Utilizziamo il nome del sistema (chiave dell'oggetto) come key univoca stabile
      <li key={sistema} className="system-item">
        <span className="system-name">{sistema}:</span>
        {/* Il ternario decide la dicitura visiva in base al booleano */}
        {attivo ? ' Attivo 🛡️' : ' Spento 💤'}
      </li>
    );
  });

  return (
    <div className="security-panel">
      <h3>Diagnostica Sicurezza:</h3>
      <ul>{listaSistemiJsx}</ul>
    </div>
  );
}

export default StatusSicurezza;
```


#### 2.3 Il Prop Drilling (Il "Passamano" delle Props)
Il Prop Drilling (letteralmente "perforazione tramite props") si verifica quando decidiamo di spostare un dato in alto nell'albero dei componenti per renderlo accessibile a più rami (questa operazione si chiama lifting state, o sollevamento dello stato).

Il problema sorge quando un componente posizionato molto in fondo ha bisogno di quel dato: per farglielo arrivare, siamo costretti a passare la prop lungo tutta la catena, livello dopo livello, attraversando componenti che non ne avrebbero alcun bisogno.

#### I Componenti Intermediari (I "Trasportatori")
In questa situazione, i componenti che stanno nel mezzo si comportano come dei semplici corrieri o trasportatori:

 - Ricevono la prop dal loro "Padre".

 - La girano immediatamente al loro "Figlio".

 - Non la usano per sé, non la modificano e non sono minimamente interessati a quel dato: serve solo a fare da ponte.

#### L'esempio pratico
Immaginiamo una struttura a tre livelli: 
l'applicazione principale (App), la struttura della pagina (Layout) e l'intestazione (Header).

```
App (Possiede il dato 'userName')
 │
 └── Layout (Riceve 'userName' ma NON lo usa, serve solo a passarlo sotto)
      │
      └── Header (È il componente finale che usa davvero 'userName')
```

Nel codice, questa catena di passaggi si traduce così:

```js
// 1. IL PADRE: Possiede il dato originale e lo passa al primo intermediario
function App() {
  const nomeUtente = "Alex";
  return <Layout userName={nomeUtente} />;
}

// 2. L'INTERMEDIARIO: Riceve 'userName' solo per poterlo girare a 'Header'
function Layout({ userName }) {
  return (
    <div>
      <Header userName={userName} /> 
    </div>
  );
}

// 3. IL CONSUMATORE FINALE: È l'unico a cui serviva davvero quel dato
function Header({ userName = 'Anonimo' }) {
  return userName; 
}
```

#### Quando diventa un problema?
Se i livelli diventano 5, 10 o 20, questo sistema mostra i suoi limiti: il codice si riempie di passaggi inutili, diventa difficile da seguire e, se decidi di cambiare il nome alla prop originaria, devi aggiornare manualmente ogni singolo componente della catena.

Quando l'albero dei componenti diventa troppo profondo, React mette a disposizione soluzioni più avanzate per "saltare la fila" e mandare il dato direttamente a destinazione, come la Context API o librerie esterne di gestione dello stato (es. Redux o Zustand).

---

### 3. 🔗 Risorse e Documentazione

- 📚 **React Docs:** [Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component)
    
- ⚛️ **React:** [Sito Ufficiale](https://react.dev/)
    
- 🧠 **MDN Web Docs:** [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

- 🛠️ **MDN Web Docs:** [Object.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) 

- ⚡ **MDN Web Docs:** [Conditional (ternary) operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator)  

---

### 4. 🚀 Key Takeaways del Giorno

- **Comunicazione:** Le props servono a passare dati dal componente padre al figlio.
    
- **Immutabilità:** Le props non si possono modificare all'interno del componente figlio.
    
- **Children:** Permette di passare intere porzioni di codice o testo dentro un componente.
    
- **Pulizia:** La destrutturazione e lo spread operator rendono il codice professionale e leggibile.
    
- **React 19:** Usa i parametri di default di JS per gestire le props mancanti.

- **Cicli su Oggetti via Props:** Quando il Padre passa un oggetto come prop, il Figlio può usare Object.entries() per trasformarlo in un array e generare liste dinamiche.

---
### 5. 📖 Glossario

| **Termine Istituzionale** | **Definizione Formale**                           | **"Spiega Brutta"**                                                         |
| ------------------------- | ------------------------------------------------- | --------------------------------------------------------------------------- |
| **Props**                 | Dati passati da un componente padre a uno figlio. | I valori che "infili" nel componente per farlo cambiare.                    |
| **Children**              | Contenuto interno di un componente.               | Comprende tutto quello che scrivi "nel mezzo" dei due tag.                  |
| **Destrutturazione**      | Estrazione di valori da un oggetto.               | Prendere solo il pezzetto di dati che ti serve senza fare giri lunghi.      |
| **Spread operator**       | Espansione di un oggetto in props singole.        | Spargere un intero pacchetto di dati dentro il componente in un colpo solo. |
| **Object.entries()**      | Metodo JavaScript che converte un oggetto in un array di coppie [chiave, valore]. |Smontare un oggetto per trasformarlo in una lista di elementi che puoi finalmente ciclare con .map(). |
