#### Modulo: React
**Titolo:** JSX e Componenti

**📍 Indice Rapido**

1. [React: Componenti e JSX](#1-react-componenti-e-jsx)
  - 1.1 [L'Unione di Logica e Interfaccia](#11-lunione-di-logica-e-interfaccia)
    * 1.1.1 [I Componenti come "Unità Autonome"](#111-i-componenti-come-unità-autonome)
    * 1.1.2 JSX: [JavaScript + XML](#112-jsx-javascript--xml)
  - 1.2 [Separazione delle Responsabilità: Componenti vs Utility](#12-separazione-delle-responsabilità-componenti-vs-utility)
  - 1.3 [Regole d'Oro della Sintassi](#13-regole-doro-della-sintassi)
    * 1.3.1 [Il Potere delle Graffe](#131-il-potere-delle-graffe--)
    * 1.3.2 [Vincoli Strutturali (Fragment e CamelCase)](#132-vincoli-strutturali-fragment-e-camelcase)
  - 1.4 [Architettura del Layout: Macro, Micro e Nesting](#14-architettura-del-layout-macro-micro-e-nesting)

2. [Risorse e Documentazione](#2--risorse-e-documentazione)
3. [Key Takeaways del giorno](#3--key-takeaways-del-giorno)
4. [Glossario: Definizioni Istituzionali vs Spiega Brutta](#4--glossario-tecnico)

# 1. React: Componenti e JSX

### 1.1 L'Unione di Logica e Interfaccia

#### 1.1.1 I Componenti come "Unità Autonome"
In React, non scriviamo più pagine intere, ma costruiamo **Componenti**. Un componente è una funzione JavaScript che descrive una parte della UI (User Interface).

 - **Esempio di Componente Autonomo:**
 
```js
// Il componente "Bottone" è isolato e riutilizzabile
function BottoneInvio() {
  return (
    <button className="btn-primary">Invia Dati</button>
  );
}
```

- **Modularità:** Se il tasto "Invia" si rompe, devi intervenire solo sul componente `Bottone`, senza rischiare di danneggiare il resto del sito.
    
- **Riutilizzabilità:** Una volta creato il componente `CardProdotto`, puoi usarlo cento volte in punti diversi dell'app semplicemente richiamando il suo nome come se fosse un tag HTML.
    

#### 1.1.2 JSX: JavaScript + XML 
JSX è l'estensione sintattica che ci permette di scrivere codice simile all'HTML direttamente dentro le funzioni JavaScript.

- **Esempio di Sintassi:**

```js
	function Saluto() {
	  return <h1>Benvenuti nel nostro progetto!</h1>; 
}
```

- **Niente stringhe:** Non usiamo le virgolette per l'HTML; lo scriviamo "nudo" dopo il comando `return`.
    
- **Trasformazione:** Sotto il cofano, React trasforma quel codice in oggetti JavaScript reali che il browser può gestire velocemente.
    
### 1.2 Separazione delle Responsabilità: Componenti vs Utility

Un’architettura professionale divide nettamente la **logica di calcolo** (puro JavaScript) dalla **logica di visualizzazione** (struttura JSX).

* `src/components/` $\rightarrow$ File `.jsx` dedicati all'interfaccia.
* `src/utility/` $\rightarrow$ File `.js` dedicati a funzioni matematiche, formattazioni o chiamate API.

### 💻 Snippet: Integrazione di Funzioni pure e Rendering Condizionale

**La Funzione Utility (`src/utility/random.js`):**

```javascript
// Esportazione nominata (Named Export) di una funzione di calcolo
export const getRandomInt = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min;
};
```

### 1.3 Regole d'Oro della Sintassi

#### 1.3.1 Il Potere delle Graffe `{ }`
Le parentesi graffe sono il ponte tra HTML e logica. Tutto ciò che viene inserito tra `{ }` viene letto come JavaScript puro.

- **Esempio Pratico:**

```js
	function ProfiloUtente() {
  const nome = "Mario";
  const oreLavorate = 8;

  return (
    <div>
      <h2>Utente: {nome}</h2>
      <p>Oggi hai lavorato per {oreLavorate * 60} minuti.</p>
    </div>
  );
}
```


- **Il Portale `{ }`**: Le graffe interrompono l'HTML e dicono a React: _"Esegui questo pezzo come JavaScript"_.
    
- **Stampa di variabili**: `{nome}` recupera il valore "Mario" e lo inserisce nel tag `<h2>`. Se cambi il valore della variabile, il testo nel sito si aggiorna da solo.
    
- **Calcoli al volo**: `{oreLavorate * 60}` dimostra che puoi fare operazioni logiche direttamente nell'interfaccia. React calcola il risultato (480) e lo mostra all'utente.
    

**In breve:** Senza graffe scriveresti un testo fisso; con le graffe scrivi un'interfaccia che "pensa" e visualizza dati reali.
    

#### 1.3.2 Vincoli Strutturali (Fragment e CamelCase)
Poiché JSX vive dentro JavaScript, deve rispettare alcune regole per non creare conflitti:

- **Il Genitore Unico:** Ogni componente deve restituire un solo elemento radice. Se hai più elementi fratelli, avvolgili nel **Fragment** `<> ... </>` per non aggiungere inutili `div` nel codice finale.

   - **Esempio Corretto:**
   
```js
	function ListaContatti() {
  return (
    <>
      <h3>Contatti</h3>
      <ul>
        <li className="item">Email: test@esempio.com</li>
        <li onClick={() => alert('Ciao!')}>Cliccami</li>
      </ul>
    </>
  );
}
```
    
- **Attributi in CamelCase:** Gli attributi HTML standard cambiano nome per non confondersi con le parole riservate di JS. Ad esempio, `class` diventa `className` e `onclick` diventa `onClick`.
---
### 1.4 Architettura del Layout: Macro, Micro e Nesting

I componenti seguono una struttura gerarchica ad albero (Relazione Padre-Figlio) e si dividono in due categorie principali:

1. **Macro Layout (La Cornice):** Definisce lo scheletro e le zone di ingombro strutturali della pagina (es. `Navbar.jsx`, `Footer.jsx`, `MainLayout.jsx`).
2. **Micro Layout (I Mattoncini):** Componenti atomici, altamente riutilizzabili e indipendenti dal contesto (es. `Button.jsx`, `Badge.jsx`).

Per creare dei veri e propri modelli grafici (Template), React mette a disposizione la proprietà speciale **`props.children`**. Essa consente di creare un componente "involucro" (Wrapper) capace di ospitare al suo interno qualsiasi codice o tag inserito all'interno.

### 💻 Snippet: Nesting e utilizzo di `children`

**Creazione del Wrapper di Layout (`src/components/layout/PageWrapper.jsx`):**

```jsx
import Navbar from '../macro/Navbar.jsx';
import Footer from '../macro/Footer.jsx';

function PageWrapper({ children }) {
  return (
    <div className="page-container">
      <Navbar/>
      
      
      <main className="content">
        {children} 
      </main>
      
      <Footer/>
    </div>
  );
}

export default PageWrapper;
```

---

### 2. 🔗 Risorse e Documentazione

- ⚛️ **React Docs:** [Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx) - La guida definitiva sulle regole sintattiche.
    
- 📚 **MDN Web Docs:** [Pensare in React](https://developer.mozilla.org/it/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/React_getting_started) - Come dividere l'interfaccia in componenti.
    

---

### 3. 🚀 **Key Takeaways del Giorno**

- **Nomi in Maiuscola:** I componenti iniziano sempre con la lettera maiuscola (es. `Header`, non `header`).
    
- **Tag Chiusi:** In JSX tutti i tag devono essere chiusi, anche quelli che in HTML sono "solitari" (es. `<img />` o `<input />`).
    
- **Dinamismo:** Grazie alle `{ }`, l'interfaccia non è più statica ma reagisce ai dati del codice.
    

---

### 4. 📖 Glossario Tecnico

| Termine Istituzionale | Definizione Formale                                          | “Spiega Brutta” (In pratica)                                                                                                       |
| --------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **JSX**               | Sintassi che permette di scrivere strutture HTML in file JS. | Un modo per scrivere “HTML dentro JavaScript” per costruire la pagina in modo più semplice e leggibile.                            |
| **Expression**        | Codice JS valutato dentro le parentesi graffe.               | Un pezzo di JavaScript che React calcola al volo e trasforma in un valore da mostrare nella pagina (es. testo, numero, risultato). |
| **Fragment**          | `<> ... </>`                                                 | Un contenitore invisibile che serve a raggruppare più elementi senza creare un tag extra nel DOM.                                  |
| **Componente**        | Funzione JavaScript che restituisce JSX.                     | Un pezzo della pagina che puoi riusare: prende dati e restituisce cosa deve essere mostrato a schermo.                             |


