**React Router** è la libreria standard per la gestione della navigazione (routing) nelle applicazioni React. Permette di cambiare la visualizzazione della pagina senza ricaricare l'intero browser, creando un'esperienza da Single Page Application (SPA).

Ecco una panoramica concisa dei suoi componenti principali (basata sulla versione moderna, React Router v6), come usarli e quando sceglierli.

---

## 1. I Componenti di Configurazione (Il Motore)

Questi componenti servono a inizializzare il router e a definire l'albero delle pagine.

### `BrowserRouter`

- **Cosa fa:** Avvolge l'intera applicazione e sincronizza l'interfaccia utente con l'URL del browser usando l'API History di HTML5.
    
- **Quando usarlo:** È la scelta standard per quasi tutte le applicazioni web tradizionali che girano sul browser.
    

### `Routes` e `Route`

- **`Routes` (Cosa fa):** Contenitore che esamina l'URL corrente e seleziona la rotta più specifica che corrisponde al percorso.
    
- **`Route` (Cosa fa):** Definisce una singola associazione tra un percorso URL (`path`) e il componente React da mostrare (`element`).
    

JavaScript

```js
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './Home';
import About from './About';

function App() {
  return (
    // 1. Inizializza il router attorno all'app
    <BrowserRouter>
      {/* 2. Contenitore per tutte le rotte */}
      <Routes>
        {/* 3. Definizione dei singoli percorsi */}
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## 2. I Componenti di Navigazione (I Link)

Servono per permettere all'utente di spostarsi tra le pagine senza ricaricare il browser.

### `Link` vs `NavLink`

- **`Link` (Cosa fa):** Genera un tag `<a>` accessibile, ma blocca il comportamento standard del browser (il refresh) e cambia l'URL internamente.
    
    - _Caso d'uso ideale:_ Pulsanti generici, link nel testo, loghi che rimandano alla Home.
        
- **`NavLink` (Cosa fa):** È una versione speciale di `Link` che sa se la pagina a cui punta è quella attualmente attiva. Applica automaticamente una classe `active` al tag.
    
    - _Caso d'uso ideale:_ **Menu di navigazione (Navbar)**, dove vuoi colorare o evidenziare la voce della pagina in cui si trova l'utente.
        


```js
import { Link, NavLink } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      {/* NavLink aggiunge la classe "active" se l'URL è esattamente "/" */}
      <NavLink to="/" end>Home</NavLink>
      <NavLink to="/about">Chi Siamo</NavLink>

      {/* Link semplice per un'azione isolata */}
      <Link to="/contatti">Scrivici</Link>
    </nav>
  );
}
```

---

## 3. Componenti Avanzati (Layout e Reindirizzamenti)

### `Outlet`

- **Cosa fa:** Funge da "segnaposto" per le rotte figlie. Permette di creare layout condivisibili (es. una navbar e un footer fissi con solo il contenuto centrale che cambia).
    
- **Quando usarlo:** Quando hai pagine annidate che condividono la stessa struttura visiva.
    

```js
import { Route, Routes, Outlet } from 'react-router-dom';

// Layout comune con Navbar fissa
function Layout() {
  return (
    <div>
      <Navbar /> 
      {/* Qui dentro verranno renderizzati i componenti figli (Dashboard o Profilo) */}
      <main><Outlet /></main> 
    </div>
  );
}

// Configurazione delle rotte annidate
<Routes>
  <Route path="/account" element={<Layout />}>
    <Route path="dashboard" element={<Dashboard />} />
    <Route path="profilo" element={<Profilo />} />
  </Route>
</Routes>
```

### `Navigate` come componente

- **Cosa fa:** Cambia programmaticamente la rotta non appena viene renderizzato.
    
- **Quando usarlo:** Ottimo per i reindirizzamenti (redirect) dichiarativi, ad esempio per proteggere una rotta o rimandare a una pagina 404 se l'URL è errato.
    

```js
import { Navigate } from 'react-router-dom';

function PaginaProtetta({ utenteAutenticato }) {
  // Se l'utente non è loggato, viene rispedito istantaneamente alla login
  if (!utenteAutenticato) {
    return <Navigate to="/login" replace />;
  }

  return <h1>Benvenuto nella tua area riservata</h1>;
}
```

---

## Tabella di Scelta Rapida (Quale usare?)

| **Se devi...**                               | **Usa...**         | **Perché?**                                                      |
| -------------------------------------------- | ------------------ | ---------------------------------------------------------------- |
| **Configurare l'app all'inizio**             | `BrowserRouter`    | È la base necessaria per tracciare la cronologia dei link.       |
| **Creare la struttura delle pagine**         | `Routes` + `Route` | Mappano i componenti in base all'URL corrente.                   |
| **Creare un link standard**                  | `Link`             | Cambia l'URL senza ricaricare la pagina.                         |
| **Creare un menu di navigazione**            | `NavLink`          | Permette di formattare diversamente il link della pagina attiva. |
| **Creare una struttura fissa (es. Sidebar)** | `Outlet`           | Evita di duplicare elementi grafici comuni tra più pagine.       |
| **Reindirizzare l'utente automaticamente**   | `Navigate`         | Sposta l'utente in base a una condizione (es. non loggato).      |
___
## 4. Hooks


Oltre ai componenti visivi, React Router fornisce una serie di **Hooks** fondamentali. Gli hooks permettono ai tuoi componenti funzionali di interagire direttamente con lo stato del router, estrarre dati dall'URL o navigare programmaticamente.


### 4.1 `useNavigate`

- **Cosa fa:** Restituisce una funzione che ti permette di cambiare pagina programmaticamente all'interno del codice JavaScript (ad esempio dopo un'azione).
    
- **Quando usarlo:** Quando la navigazione deve avvenire a seguito di un evento, come l'invio di un form, il clic su un pulsante che esegue una logica, o dopo un timer.
    
- **Caso d'uso ideale vs `Link`:** Usa `Link` se l'utente deve solo cliccare per andare altrove. Usa `useNavigate` se prima di cambiare pagina devi fare qualcosa (es. salvare dati nel database).




```JavaScript
import { useNavigate } from 'react-router-dom';

function FormLogin() {
  const navigate = useNavigate();

  const gestisciLogin = (e) => {
    e.preventDefault();
    // ... logica di autenticazione (es. controllo password) ...
    
    // Reindirizza l'utente alla dashboard dopo il login riuscito
    navigate('/dashboard');
  };

  return (
    <form onSubmit={gestisciLogin}>
      <button type="submit">Accedi</button>
    </form>
  );
}
```

---

### 4.2 `useParams`

- **Cosa fa:** Permette di leggere i **parametri dinamici** inseriti nel percorso dell'URL (quelli definiti con i due punti, es. `:id`).
    
- **Quando usarlo:** Nelle pagine di dettaglio, dove il contenuto cambia in base all'ID presente nell'URL (es. la pagina di un singolo prodotto, di un post di un blog o di un profilo utente).
    

```JavaScript
// 1. Configurazione della rotta (nel file principale)
// <Route path="/prodotto/:id" element={<DettaglioProdotto />} />

import { useParams } from 'react-router-dom';

function DettaglioProdotto() {
  // 2. Estrai il parametro "id" dall'URL
  const { id } = useParams();

  return (
    <div>
      {/* Se l'URL è /prodotto/42, stamperà "ID del prodotto: 42" */}
      <h1>Dettaglio Prodotto</h1>
      <p>Stai visualizzando il prodotto con ID: {id}</p>
    </div>
  );
}
```

---

### 4.3 `useSearchParams`

- **Cosa fa:** Permette di leggere e modificare i **parametri di query** nell'URL (quelli dopo il punto di domanda, es. `?search=react&sort=asc`). Funziona in modo molto simile al classico `useState` di React.
    
- **Quando usarlo:** Quando devi gestire filtri di ricerca, paginazione o ordinamenti che vuoi rimangano salvati nell'URL (così se l'utente copia e incolla il link, un'altra persona vedrà gli stessi filtri).
    

```JavaScript
import { useSearchParams } from 'react-router-dom';

function PaginaProdotti() {
  // searchParams contiene i valori, setSearchParams permette di modificarli
  const [searchParams, setSearchParams] = useSearchParams();

  // Leggi il parametro "categoria" dall'URL (es. ?categoria=scarpe)
  const categoria = searchParams.get('categoria') || 'Nessuna';

  const impostaFiltro = () => {
    // Cambia l'URL in ?categoria=vestiti senza ricaricare la pagina
    setSearchParams({ categoria: 'vestiti' });
  };

  return (
    <div>
      <p>Categoria attiva: {categoria}</p>
      <button onClick={impostaFiltro}>Filtra per Vestiti</button>
    </div>
  );
}
```

---

## 4. `useLocation`

- **Cosa fa:** Restituisce l'oggetto `location` che rappresenta l'URL corrente. Contiene informazioni dettagliate come il percorso testuale (`pathname`), la stringa dei parametri (`search`), e persino uno stato invisibile passato tra le pagine (`state`).
    
- **Quando usarlo:** Quando hai bisogno di sapere esattamente dove si trova l'utente (es. per tracciare le visualizzazioni della pagina con Analytics) o per recuperare dati nascosti inviati tramite un `Link`.
    


```JavaScript
import { useLocation } from 'react-router-dom';

function TracciatorePagine() {
  const location = useLocation();

  // location.pathname restituirà stringhe come "/about" o "/contatti"
  return (
    <p>Ti trovi attualmente nel percorso: {location.pathname}</p>
  );
}
```

---

## Riepilogo degli Hooks (Quale usare?)

| **Se devi...**                                | **Usa...**        | **Esempio di URL associato**              |
| --------------------------------------------- | ----------------- | ----------------------------------------- |
| **Cambiare pagina dopo un'azione nel codice** | `useNavigate`     | _(Azione interna)_                        |
| **Leggere un ID fisso nel percorso**          | `useParams`       | `/utente/**42**`                          |
| **Gestire filtri, ricerche o ordinamenti**    | `useSearchParams` | `/prodotti**?filtro=tech&ordine=prezzo**` |
| **Sapere l'URL esatto in cui ti trovi**       | `useLocation`     | Legge l'intero oggetto dell'URL corrente  |