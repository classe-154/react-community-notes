
## Cosa sono i Contesti?

In React, i dati passano normalmente dall'alto verso il basso (dal componente genitore al figlio) tramite le `props`. Quando un dato serve a molti componenti sparsi nell'applicazione, passare le props attraverso livelli intermedi che non ne hanno bisogno diventa noioso e difficile da gestire (**Prop Drilling**).

Il **Contesto (Context)** risolve questo problema: agisce come una "funivia" o un canale globale che permette di condividere dati (come il tema scuro/chiaro, i dati dell'utente loggato o la lingua) direttamente con qualsiasi componente che ne ha bisogno, saltando i passaggi intermedi.

---

## Come è strutturato un contesto e da cosa è composto

Un contesto React standard è composto principalmente da tre elementi:

1. **Il Contesto stesso (`Context`):** L'oggetto che contiene il dato.
    
2. **Il Provider (`Provider`):** Il componente che "avvolge" l'applicazione (o una parte di essa) e rende disponibile il dato a tutti i componenti figli.
    
3. **Il Consumatore (`Consumer` o l'hook `useContext`):** Il meccanismo con cui i componenti figli leggono il dato.
    

---

## 1. Creare e Strutturare il Contesto (File separato)

Immaginiamo di voler gestire il tema della nostra applicazione (chiaro o scuro). Creiamo un file isolato per il nostro contesto.


```JSX
// src/context/ThemeContext.jsx
import { createContext, useState } from 'react';

// 1. Creiamo il contesto vero e proprio con un valore iniziale di default (opzionale)
export const ThemeContext = createContext(null);

// 2. Creiamo il componente Provider che gestirà lo stato globale
export function ThemeProvider({ children }) {
  // Definiamo lo stato che vogliamo condividere
  const [theme, setTheme] = useState('light');

  // Funzione per cambiare il tema
  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  // Creiamo un oggetto che contiene sia lo stato che la funzione per modificarlo
  const value = {
    theme,
    toggleTheme
  };

  return (
    // Il Provider distribuisce il "value" a tutti i componenti figli ({children})
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}
```

---

## 2. "Avvolgiamo" il Contesto alla tua applicazione

Per fare in modo che i componenti abbiano accesso al contesto, dobbiamo avvolgerli nel `ThemeProvider` che abbiamo appena creato. Di solito si fa nel file principale dell'app (es. `App.jsx` o `main.jsx`).



``` JSX
// src/App.jsx
import { ThemeProvider } from './context/ThemeContext';
import Header from './components/Header';
import PageContent from './components/PageContent';

function App() {
  return (
// Avvolgiamo l'intera applicazione nel Provider.
// Ora Header, PageContent e tutti i loro figli possono accedere al tema.
    <ThemeProvider>
      <div className="app-container">
        <Header />
        <PageContent />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

---

## 3. Perché è utile creare un Hook Personalizzato (Custom Hook)?

Per usare il contesto in un componente, React mette a disposizione l'hook `useContext(ThemeContext)`. Tuttavia, creare un **hook personalizzato** (es. `useTheme`) offre enormi vantaggi:

- **Pulizia del codice:** Nei componenti non dovrai importare ogni volta sia `useContext` sia l'oggetto `ThemeContext`. Ti basterà importare solo il tuo hook personalizzato.
    
- **Controllo degli errori:** Puoi inserire un controllo per verificare se stai provando a usare il contesto _fuori_ dal Provider (cosa che causerebbe bug difficili da scovare).
    

Ecco come si integra l'hook personalizzato direttamente nel file del contesto:


```js
// AGGIUNTA al file src/context/ThemeContext.jsx
import { createContext, useState, useContext } from 'react'; // Aggiunto useContext

// ... (qui rimane il codice di creazione visto nel punto 1) ...

// 3. Creiamo l'hook personalizzato
export function useTheme() {
  const context = useContext(ThemeContext);

  // Se il contesto è undefined, significa che il componente che lo usa 
  // non è dentro il <ThemeProvider>
  if (!context) {
    throw new Error('useTheme deve essere usato all\'interno di un ThemeProvider');
  }

  // Restituiamo direttamente l'oggetto con stato e funzioni
  return context;
}
```

---

## 4. Consumare il contesto usando l'Hook Personalizzato

Ora vediamo com'è semplice e pulito usare il contesto all'interno di un componente figlio grazie all'hook personalizzato.


``` jsx
// src/components/Header.jsx
import React from 'react';
// Importiamo solo l'hook personalizzato, non serve altro!
import { useTheme } from '../context/ThemeContext';

function Header() {
  // Estraiamo i dati che ci servono dal contesto usando il nostro hook
  const { theme, toggleTheme } = useTheme();

  return (
    <header style={{ 
      backgroundColor: theme === 'light' ? '#fff' : '#333', 
      color: theme === 'light' ? '#000' : '#fff' 
    }}>
      <h1>La mia App ({theme})</h1>
      {/* Al click cambiamo il tema globalmente */}
      <button onClick={toggleTheme}>
        Cambia Tema
      </button>
    </header>
  );
}

export default Header;
```