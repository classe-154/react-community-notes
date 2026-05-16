# React Context API — Appunti di lezione

1. Argomenti Lezione: React Context API

 - [1.1 Il Problema: Props Drilling](#-11-il-problema-props-drilling)

 - [1.2 La Soluzione: La Context API](#-12-la-soluzione-la-context-api)

 - [1.3 Creazione del context e del provider](#13-creazione-del-context-e-del-provider)

 - [1.4 Lettura del context con useContext](#-14-lettura-del-context-con-usecontext)

 - [1.5 Custom hooks per usare il Context](#-15-custom-hook-per-usare-il-context)

 - [1.6 Wrapping dell'applicazione e struttura dei file](#-16-wrapping-dellapplicazione-e-struttura-dei-file)

 - [1.7 Context semplice e Context complesso](#17-context-semplice-e-context-complesso)

2. [Capitolo Riassuntivo e Schematico](#-2-capitolo-riassuntivo-e-schematico)
3. [Risorse Tecniche (MDN, W3S, React)](#-3-risorse-tecniche-mdn-w3schools-react)
4. [Key Takeaways](#-4-key-takeaways) 
5. [Glossario](#-5-syllabus)

## 🔌 1.1 Il problema: Props Drilling

React passa normalmente i dati dall'alto verso il basso tramite **props**.

Questo funziona bene quando padre e figlio sono vicini, ma diventa scomodo quando lo stesso dato deve arrivare a componenti molto lontani nell'albero.

Esempi di dati condivisi:

- tema chiaro/scuro;
- utente loggato;
- lingua selezionata;
- carrello;
- preferenze globali dell'app.

Quando una prop viene passata attraverso componenti intermedi che non la usano davvero, solo per arrivare a un componente più profondo, si parla di **props drilling**.

```jsx
<App user={user}>
  <Layout user={user}>
    <Sidebar user={user}>
      <UserInfo user={user} />
    </Sidebar>
  </Layout>
</App>
```

In questo esempio `Layout` e `Sidebar` non usano davvero `user`.

Stanno solo trasportando il dato verso `UserInfo`.

La **Context API** permette di evitare questi passaggi inutili, rendendo un dato disponibile a tutti i componenti che si trovano sotto un determinato Provider.

---

## 🌊 1.2 La Soluzione: La Context API

Possiamo immaginare la Context API come un **canale d'acqua** che trasporta dati.

| Elemento | Metafora | Significato |
| --- | --- | --- |
| `createContext()` | Scava il canale | Crea il context |
| Context | Il canale / fiume | Il collegamento che trasporta i dati |
| Provider | La sorgente | Inserisce i dati nel canale |
| `value` | Gli oggetti nell'acqua | I dati condivisi |
| `useContext()` | Il secchio | Permette di leggere i dati |
| Custom hook | Il pescatore specializzato | Legge il context in modo più pulito e sicuro |

### 🧠 Regola fondamentale

Il **Provider** deve stare sopra tutti i componenti che vogliono leggere quel context.

```txt
ThemeProvider
└── App
    ├── Header
    ├── Main
    └── Footer
```

Tutti i componenti dentro `ThemeProvider` possono leggere i dati forniti dal context.

---

## 1.3 Creazione del Context e del Provider

Il primo passo è creare il context con `createContext()`.

### 💻 Snippet: Creazione del Context

```jsx
// src/contexts/ThemeContext.jsx
import { createContext } from "react";

const ThemeContext = createContext(null);

export default ThemeContext;
```

Il valore `null` è un valore di fallback.

Serve come controllo nel caso in cui un componente provi a leggere il context senza avere un Provider sopra di lui.

### 💻 Snippet: Creazione del Provider

```jsx
// src/contexts/ThemeProvider.jsx
import { useState } from "react";
import ThemeContext from "./ThemeContext";

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  function toggleTheme() {
    setTheme((currentTheme) =>
      currentTheme === "light" ? "dark" : "light"
    );
  }

  const value = {
    theme,
    toggleTheme,
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

export default ThemeProvider;
```

In questo esempio:

- `theme` è il dato condiviso;
- `toggleTheme` è la funzione che modifica il dato;
- `value` contiene ciò che mettiamo a disposizione;
- `children` rappresenta i componenti avvolti dal Provider.

> Nota: in React 19 è possibile usare anche la sintassi abbreviata `<ThemeContext value={value}>`. Nei progetti React precedenti è più comune trovare `<ThemeContext.Provider value={value}>`.

---

## 🎣 1.4 Lettura del Context con `useContext`

Per leggere i dati del context dentro un componente si usa `useContext()`.

### 💻 Snippet: Consumo diretto del Context

```jsx
// src/components/ThemeButton.jsx
import { useContext } from "react";
import ThemeContext from "../contexts/ThemeContext";

function ThemeButton() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>
      Tema attuale: {theme}
    </button>
  );
}

export default ThemeButton;
```

`useContext(ThemeContext)` permette al componente di pescare i dati inseriti nel Provider.

In questo caso il componente legge:

- `theme`;
- `toggleTheme`.

---

## 🧰 1.5 Custom Hook per usare il Context

Una buona pratica è creare un **custom hook** dedicato.

Invece di importare ogni volta `useContext` e `ThemeContext`, creiamo una funzione riutilizzabile.

### 💻 Snippet: Custom Hook

```jsx
// src/hooks/useTheme.js
import { useContext } from "react";
import ThemeContext from "../contexts/ThemeContext";

function useTheme() {
  const context = useContext(ThemeContext);

  if (context === null) {
    throw new Error("ThemeProvider non trovato a monte del componente.");
  }

  return context;
}

export default useTheme;
```

A questo punto nei componenti possiamo scrivere:

```jsx
import useTheme from "../hooks/useTheme";

function ThemeButton() {
  const { theme, toggleTheme } = useTheme();

  return (
    <button onClick={toggleTheme}>
      Cambia tema
    </button>
  );
}
```

Il custom hook è utile perché:

- rende il codice più leggibile;
- evita import ripetitivi;
- mostra un errore chiaro se manca il Provider.

---

## 🧩 1.6 Wrapping dell'applicazione e struttura dei file

Per rendere disponibile il context, dobbiamo avvolgere l'applicazione con il Provider.

### 💻 Snippet: Wrapping in `App.jsx`

```jsx
// src/App.jsx
import ThemeProvider from "./contexts/ThemeProvider";
import Header from "./components/Header";
import ThemeButton from "./components/ThemeButton";

function App() {
  return(
    <ThemeProvider>
      <Header>
        <ThemeButton />
      </Header>
    </ThemeProvider>
  );
}

export default App
```

Una possibile struttura del progetto:

```txt
src/
├── contexts/
│   ├── ThemeContext.jsx
│   └── ProductContext.jsx
│
├── hooks/
│   ├── useTheme.js
│   └── useProducts.js
│
├── components/
│   ├── Header.jsx
│   └── ThemeButton.jsx
│
└── pages/
    ├── Home.jsx
    └── ProductSingle.jsx
```

Questa organizzazione separa:

- i **context**, dove creiamo e forniamo i dati;
- gli **hook**, dove leggiamo i dati in modo comodo;
- i **components**, che consumano i dati;
- le **pages**, che rappresentano le viste principali.

---

## 1.7 Context semplice e Context complesso

Un'applicazione può avere più context, ognuno con una responsabilità precisa.

| Tipo di Context | Esempio | Dati gestiti | Logica |
| --- | --- | --- | --- |
| Context semplice | `ThemeContext` | `theme` | `toggleTheme()` |
| Context utente | `UserContext` | `user`, `isLoggedIn` | login / logout |
| Context carrello | `CartContext` | `cartProducts` | add / remove / update quantity |
| Context lingua | `LanguageContext` | `language` | cambio lingua |

Un componente può consumare anche più context contemporaneamente.

```jsx
function Header() {
  const { theme } = useTheme();
  const { cartProducts } = useCart();

  return (
    <header>
      <p>Tema: {theme}</p>
      <p>Prodotti nel carrello: {cartProducts.length}</p>
    </header>
  );
}
```

Questo significa che un componente può pescare dati da più canali.

---

## 🏁 2 Capitolo Riassuntivo e Schematico

### 📊 Tabella Comparativa: Props vs Context

| Tecnica | Meccanismo | Quando usarla |
| --- | --- | --- |
| **Props** | Passano dati da padre a figlio | Quando i componenti sono vicini |
| **Context API** | Condivide dati con componenti discendenti | Quando il dato serve in punti lontani |
| **useState** | Crea e aggiorna uno stato locale | Quando il dato appartiene a un componente |
| **useContext** | Legge un dato dal context | Quando un componente deve consumare dati globali |
| **Custom Hook** | Incapsula la lettura del context | Quando vogliamo codice più pulito e controllato |

### 🧠 Regole Mnemoniche del Context

- 💡 **Il Provider deve stare a monte**: un componente può leggere un context solo se è avvolto dal Provider corretto.
- 💡 **`useState` crea il dato, Context lo distribuisce**: Context non sostituisce `useState`, spesso lo usa.
- 💡 **Non tutto deve diventare Context**: se un dato passa solo da padre a figlio, le props sono più semplici.
- 💡 **Dividi le responsabilità**: meglio avere `ThemeContext`, `UserContext`, `CartContext` separati invece di un unico context gigante.
- 💡 **Il custom hook è il pescatore specializzato**: semplifica il codice e controlla se manca il Provider.

---

## 📌 3. Risorse Tecniche (MDN, W3Schools, React)
- [Context](https://react.dev/learn/passing-data-deeply-with-context)
- [useContext](https://react.dev/reference/react/useContext)
- [createContext](https://react.dev/reference/react/createContext)
- [W3Schools Context](https://www.w3schools.com/react/react_usecontext.asp)

---

## 📌 4. Key Takeaways

- La Context API serve a condividere dati tra componenti senza passare props manualmente a ogni livello.
- Il problema principale che risolve è il **props drilling**.
- `createContext()` crea il context.
- Il **Provider** fornisce i dati ai componenti figli.
- La prop `value` contiene i dati condivisi.
- `useContext()` permette di leggere i dati del context.
- Un custom hook rende l'utilizzo del context più pulito e sicuro.
- Context e `useState` spesso lavorano insieme.
- Context è utile per dati condivisi come tema, utente, lingua o carrello.
- Context non deve sostituire sempre le props.

---

## 📚 5. Syllabus

| Termine | Definizione istituzionale | Spiegazione semplice |
| --- | --- | --- |
| Context API | API di React che permette di condividere valori tra componenti senza passare props manualmente a ogni livello. | Un modo per dare lo stesso dato a più componenti senza fare troppi passaggi. |
| Props Drilling | Passaggio di props attraverso componenti intermedi che non usano direttamente quei dati. | Quando una prop attraversa troppi componenti solo per arrivare a quello finale. |
| `createContext()` | Funzione che crea un oggetto context. | Scava il canale dove passeranno i dati. |
| Provider | Componente che fornisce un valore ai componenti discendenti. | La sorgente che mette i dati nel canale. |
| `value` | Prop del Provider che contiene il valore condiviso. | I dati che scorrono nel canale. |
| `useContext()` | Hook che permette a un componente di leggere il valore di un context. | Il secchio con cui un componente pesca i dati. |
| Custom Hook | Funzione personalizzata che incapsula logica riutilizzabile basata su hook. | Un pescatore specializzato che legge il context in modo più comodo e sicuro. |
| `children` | Prop speciale che rappresenta i componenti contenuti dentro un altro componente. | I componenti che il Provider avvolge. |
| State globale | Stato condiviso tra più parti dell'applicazione. | Un dato che serve in più punti dell'app. |
| Re-render | Nuova esecuzione di un componente per aggiornare l'interfaccia. | React ricalcola cosa mostrare perché qualcosa è cambiato. |
