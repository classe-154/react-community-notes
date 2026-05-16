# React Router 

## 1. Web tradizionale: MPA

Nel web tradizionale si parla spesso di **MPA**, cioè **Multi-Page Application**.

In una MPA, ogni pagina è un file HTML diverso, oppure una risposta diversa generata dal server.

Il flusso è questo:

```txt
utente clicca un link
↓
il browser chiede una nuova pagina al server
↓
il server restituisce un nuovo HTML
↓
il browser distrugge la pagina precedente
↓
carica da zero la nuova pagina
```

Questo significa che ogni navigazione provoca un **refresh completo**.

Esempio:

```html
<a href="/contact">Contatti</a>
```

Con un link HTML normale, il browser cambia pagina. Fa una nuova richiesta HTTP e ricarica tutto.

---

## 2. SPA: Single Page Application

React permette di costruire applicazioni SPA, cioè **Single Page Application**.

In una SPA, al primo caricamento il server manda:

```txt
1 file HTML
1 bundle JavaScript (cioè il pacchetto finale con il codice della tua app React pronto per il browser)
1 app React

Il browser riceve un file HTML quasi vuoto, scarica il bundle JavaScript e quel bundle avvia l’app React dentro la pagina.
```

Poi, quando l’utente cambia pagina, il browser **non ricarica** un nuovo documento HTML.

Succede invece questo:

```txt
utente clicca un link
↓
React intercetta la navigazione
↓
l’URL cambia
↓
React smonta i vecchi componenti
↓
React monta i nuovi componenti
↓
la pagina non viene ricaricata
```

Quindi il sito sembra avere più pagine, ma in realtà React sta cambiando componenti dentro la stessa applicazione.

Esempio:

```txt
/           → Home
/contact    → Contact
/products   → Products
```

Non sono tre file HTML diversi. Sono tre componenti React diversi mostrati in base all’URL.

---

## 3. Perché serve React Router

Il problema principale è questo:

> Il browser ragiona tramite URL, ma una SPA ha un solo file HTML.

Quindi serve uno strumento che tenga sincronizzati:

```txt
URL del browser
↓
componente React da mostrare
```

Questo strumento è **React Router**.

React Router usa la **History API** del browser per modificare l’URL senza ricaricare la pagina. In questo modo puoi avere URL reali, navigazione avanti/indietro, deep linking e componenti diversi in base al percorso corrente.

La modalità usata in questi appunti è il **Declarative Mode**, cioè la modalità in cui configuri le rotte scrivendo componenti come `BrowserRouter`, `Routes`, `Route`, `Link`, `NavLink`, `Outlet`.

---

## 4. Convenzione: cartella `pages`

Nei progetti React è buona pratica creare una cartella:

```txt
src/pages/
```

Dentro questa cartella mettiamo i componenti che rappresentano intere schermate dell’app.

Esempio:

```txt
src/
├─ pages/
│  ├─ Home.jsx
│  ├─ Contact.jsx
│  ├─ Products.jsx
│  └─ ProductDetail.jsx
```

Questi componenti sono diversi dai componenti più piccoli.

Esempio:

```txt
pages/Home.jsx              → intera pagina
components/Navbar.jsx       → pezzo riutilizzabile
components/ProductCard.jsx  → card singola
layouts/MainLayout.jsx      → struttura comune della pagina
```

---

## 5. Routing manuale con `useState`

Prima di usare React Router, possiamo simulare una navigazione con `useState`.

Esempio:

```jsx
import { useState } from "react"
import Home from "./pages/Home"
import Contact from "./pages/Contact"

function App() {
  const [page, setPage] = useState("home")

  return (
    <>
      <nav>
        <button onClick={() => setPage("home")}>Home</button>
        <button onClick={() => setPage("contact")}>Contatti</button>
      </nav>

      <main>
        {page === "home" && <Home />}
        {page === "contact" && <Contact />}
      </main>
    </>
  )
}

export default App
```

Qui lo stato `page` decide quale componente mostrare.

Se `page` vale `"home"`, React mostra:

```jsx
<Home />
```

Se `page` vale `"contact"`, React mostra:

```jsx
<Contact />
```

Questa è una specie di router artigianale.

---

## 6. Rendering condizionale con `&&`

Nel codice:

```jsx
{page === "home" && <Home />}
```

l’operatore `&&` viene usato come interruttore.

Funziona così:

```txt
se page === "home" è true
↓
React renderizza <Home />

se page === "home" è false
↓
React non renderizza nulla
```

Quindi:

```jsx
condizione && componente
```

significa:

```txt
mostra il componente solo se la condizione è vera
```

---

## 7. Come funziona davvero `&&` in JavaScript

L’operatore `&&` valuta da sinistra verso destra.

Esempio:

```js
a === "prova" && console.log("a è uguale a prova")
```

Se la prima parte è falsa:

```js
a === "prova"
```

JavaScript si ferma subito. Non esegue neanche `console.log`.

Se invece la prima parte è vera, allora passa alla seconda parte e la esegue.

Questa cosa si chiama **short-circuit evaluation**.

In React è utile perché il JSX dentro `{}` è JavaScript. Quindi possiamo scrivere:

```jsx
{isLoggedIn && <Dashboard />}
```

Traduzione:

```txt
se isLoggedIn è true, mostra Dashboard
se isLoggedIn è false, non mostrare nulla
```

---

## 8. Limiti del routing manuale

Il routing fatto con `useState` funziona solo visivamente, ma ha grossi limiti.

### L’URL non cambia

Se passo da Home a Contact, l’URL resta lo stesso.

Quindi non posso condividere il link alla pagina Contatti.

### Il refresh resetta tutto

Se sono su Contact e aggiorno la pagina, lo stato torna al valore iniziale.

Esempio:

```jsx
const [page, setPage] = useState("home")
```

Dopo il refresh torno sempre a Home.

### Avanti e indietro del browser non funzionano bene

Il browser non sa che sei passato da Home a Contact, perché l’URL non è cambiato.

### Non esiste deep linking

Non posso aprire direttamente:

```txt
/contact
```

perché quella rotta non è realmente collegata al browser.

Per risolvere questi problemi serve React Router.

---

## 9. Installazione di React Router

L’installazione è:

```bash
pnpm add react-router
```

Esempio di setup:

```jsx
import { BrowserRouter } from "react-router"
import App from "./App"

createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
)
```

Nota pratica: in molti progetti basati su React Router v6 si trova ancora `react-router-dom`.

Quindi:

```txt
React Router v7 / materiale nuovo → react-router
React Router v6 / progetti precedenti → react-router-dom
```

In React Router v7 il pacchetto consigliato dalla documentazione è react-router.
react-router-dom esiste ancora per compatibilità con i progetti v6 e continua a funzionare in molti casi, ma nei nuovi appunti ufficiali si usa react-router.

Non mischiare i due stili nello stesso progetto.

---

## 10. Configurazione base

Esempio:

```jsx
import { BrowserRouter, Routes, Route } from "react-router"
import Home from "./pages/Home"
import Contact from "./pages/Contact"

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

Qui abbiamo tre elementi fondamentali:

```txt
BrowserRouter
Routes
Route
```

`Routes` e `Route` formano una mappa: a ogni `path` corrisponde un componente da visualizzare.

---

## 11. `BrowserRouter`

`BrowserRouter` è il contenitore principale del routing.

Deve avvolgere la parte dell’app che usa il router.

```jsx
<BrowserRouter>
  <Routes>
    ...
  </Routes>
</BrowserRouter>
```

Serve a collegare React Router all’URL del browser.

Senza `BrowserRouter`, non funzionano correttamente:

```txt
Routes
Route
Link
NavLink
Outlet
useParams
useNavigate
useLocation
useSearchParams
```

---

## 12. `Routes`

`Routes` è il contenitore delle rotte.

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/contact" element={<Contact />} />
</Routes>
```

Il suo lavoro è:

```txt
guardo l’URL attuale
↓
cerco la Route corrispondente
↓
renderizzo il componente giusto
```

È come un blocco decisionale.

---

## 13. `Route`

`Route` associa un URL a un componente.

```jsx
<Route path="/contact" element={<Contact />} />
```

Significa:

```txt
quando l’URL è /contact
mostra il componente Contact
```

La proprietà `path` è l’URL.

La proprietà `element` o `Component` indica il componente da mostrare.

---

## 14. `Link`: navigare senza refresh

In una SPA non devi usare il normale tag `<a>` per le navigazioni interne.

Questo:

```jsx
<a href="/contact">Contatti</a>
```

fa una navigazione classica e può causare refresh.

Con React Router si usa:

```jsx
import { Link } from "react-router"

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/contact">Contatti</Link>
    </nav>
  )
}
```

La differenza:

```txt
<a href="">     → navigazione del browser, refresh pagina
<Link to="">    → navigazione React Router, niente refresh
```

`Link` sotto sotto renderizza comunque un `<a>`, ma intercetta il click, blocca il comportamento standard e aggiorna l’URL tramite React Router.

### Quando usare `Link`

Usa `Link` per navigazioni semplici e generiche, cioè quando vuoi portare l’utente da una pagina all’altra senza bisogno di evidenziare lo stato attivo del link.

Esempi tipici:

```txt
logo che porta alla Home
link dentro un testo
bottone “Scopri i prodotti”
card cliccabile che porta al dettaglio
```

Esempio:

```jsx
<Link to="/products">Vai ai prodotti</Link>
```

In sintesi:

```txt
Link = navigazione interna semplice, senza refresh
```

---

## 15. `NavLink`

`NavLink` è come `Link`, ma gestisce automaticamente lo stato active.

Esempio:

```jsx
import { NavLink } from "react-router"

function Navbar() {
  return (
    <nav>
      <NavLink to="/" end>Home</NavLink>
      <NavLink to="/contact">Contatti</NavLink>
    </nav>
  )
}
```

Quando l’URL corrisponde al link, React Router aggiunge automaticamente la classe:

```txt
active
```

Esempio CSS:

```css
.active {
  color: blue;
  font-weight: bold;
}
```

### Quando usare `NavLink`

Usa `NavLink` soprattutto nella Navbar o nei menu di navigazione, perché permette di capire visivamente in quale pagina si trova l’utente.

Esempio:

```jsx
<NavLink to="/products">Prodotti</NavLink>
```

Se l’utente si trova su `/products`, React Router aggiunge automaticamente la classe `active`.

Quindi:

```txt
Link    → navigazione semplice
NavLink → navigazione + stato active
```

---

## 16. `end` su `NavLink`

Il link verso `/`, oppure verso un path padre, può risultare active anche su rotte più specifiche.

Esempio senza `end`:

```txt
NavLink to="/products"

/products        → active
/products/5      → active
/products/5/edit → active
```

Se vuoi che il link sia active solo sulla pagina esatta `/products`, usi `end`:

```jsx
<NavLink to="/products" end>
  Lista prodotti
</NavLink>
```

Risultato:

```txt
/products        → active
/products/5      → non active
/products/5/edit → non active
```

Per la Home:

```jsx
<NavLink to="/" end>
  Home
</NavLink>
```

`end` evita che il link Home rimanga active anche su altre pagine.

---

## 17. Styling condizionale con `NavLink`

Oltre alla classe `active` automatica, `NavLink` può ricevere una funzione in `className`, `style` o `children`.

Esempio:

```jsx
<NavLink
  to="/messages"
  className={({ isActive }) =>
    isActive ? "text-primary fw-bold" : "text-dark"
  }
>
  Messaggi
</NavLink>
```

Qui React Router passa un oggetto con `isActive`.

Se `isActive` è `true`, dai una classe.

Se è `false`, ne dai un’altra.

Qui di seguito viene mostrato anche un esempio di combinazione con Bootstrap:

```jsx
<NavLink
  to="/products"
  className={({ isActive }) =>
    `nav-link ${isActive ? "active" : ""}`
  }
>
  Prodotti
</NavLink>
```

---

## 18. `path="*"`: gestione 404

Per gestire URL non validi si usa la rotta jolly:

```jsx
<Route path="*" element={<NotFound />} />
```

Esempio completo:

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/contact" element={<Contact />} />
  <Route path="*" element={<NotFound />} />
</Routes>
```

`path="*"` cattura tutto ciò che non è stato intercettato dalle rotte precedenti.

Esempio:

```txt
/ciao
/test
/pagina-che-non-esiste
```

andranno tutte su `NotFound`.

---

## 19. Componente `NotFound`

Esempio:

```jsx
import { Link } from "react-router"

function NotFound() {
  return (
    <div style={{ textAlign: "center", marginTop: "100px" }}>
      <h1>404 - Pagina non trovata</h1>
      <p>La pagina che stai cercando non esiste.</p>
      <Link to="/">Torna alla Home</Link>
    </div>
  )
}

export default NotFound
```

Il link per tornare alla home deve essere un `Link`, non un `<a>`, così resti dentro la SPA senza refresh.

---

## 20. Layout condivisi

In un’app reale molte pagine hanno elementi comuni che si ripetono sempre, per esempio:

```txt
Navbar
main content
Footer
```

La parte che cambia davvero da una pagina all’altra è quasi sempre solo il contenuto centrale.

```txt
Home      → cambia il contenuto principale
Contact   → cambia il contenuto principale
Products  → cambia il contenuto principale
```

Ma la Navbar sopra e il Footer sotto restano uguali.

Non conviene quindi scrivere Navbar e Footer dentro ogni singola pagina, perché significherebbe duplicare codice.

Il problema è che stai ripetendo sempre la stessa struttura. Se un giorno devi modificare la Navbar o il Footer, rischi di dover intervenire in più punti.

La soluzione è creare un layout condiviso, cioè un componente che contiene le parti fisse della pagina e lascia uno spazio centrale per la pagina attiva.

Per questo si crea una cartella dedicata:

Esempio:

```txt
src/
├─ layouts/
│  └─ MainLayout.jsx
├─ pages/
│  ├─ Home.jsx
│  ├─ Contact.jsx
│  └─ NotFound.jsx
```

Il layout diventa il “guscio” comune dell’app:

```txt
MainLayout
├─ Navbar
├─ contenuto della pagina corrente
└─ Footer
```

Quindi il layout contiene ciò che resta sempre uguale. Le pages contengono solo ciò che cambia da una rotta all’altra.

---

## 21. `Outlet`

Nel layout si usa `Outlet`.

```jsx
import { Outlet } from "react-router"
import Navbar from "../components/Navbar"
import Footer from "../components/Footer"

function MainLayout() {
  return (
    <>
      <Navbar />

      <main>
        <Outlet />
      </main>

      <Footer />
    </>
  )
}

export default MainLayout
```

`Outlet` è il punto in cui React Router inserisce la pagina figlia attiva, ovvero il segnaposto dove viene renderizzato il componente corrispondente alla rotta corrente.

Schema:

```txt
MainLayout
├─ Navbar
├─ Outlet  ← qui entra Home / Contact / Products
└─ Footer
```

---

## 22. Rotta layout senza path

Per usare un layout condiviso:

```jsx
<Route element={<MainLayout />}>
  <Route index element={<Home />} />
  <Route path="contact" element={<Contact />} />
</Route>
```

oppure:

```jsx
<Route Component={MainLayout}>
  <Route index Component={Home} />
  <Route path="contact" Component={Contact} />
</Route>
```

La Route padre non ha `path`.

Questo significa:

```txt
non aggiunge prefissi all’URL
serve solo come guscio strutturale
```

Le figlie restano:

```txt
/
/contact
```

ma vengono mostrate dentro il layout.

---

## 23. 404 dentro o fuori dal layout

Hai due possibilità.

### 404 dentro il layout

```jsx
<Route element={<MainLayout />}>
  <Route index element={<Home />} />
  <Route path="contact" element={<Contact />} />
  <Route path="*" element={<NotFound />} />
</Route>
```

La pagina 404 avrà Navbar e Footer.

### 404 fuori dal layout

```jsx
<Route element={<MainLayout />}>
  <Route index element={<Home />} />
  <Route path="contact" element={<Contact />} />
</Route>

<Route path="*" element={<NotFound />} />
```

La pagina 404 sarà pulita, senza Navbar e Footer.

Dipende dal risultato che vuoi ottenere.

---

## 24. Rotte annidate con path

Le rotte annidate possono anche servire per creare URL gerarchici.

Esempio:

```jsx
<Route path="/percorso">
  <Route index element={<SuDiNoi />} />
  <Route path="storia" element={<Storia />} />
  <Route path="team" element={<Team />} />
</Route>
```

Risultato:

```txt
/percorso         → SuDiNoi
/percorso/storia  → Storia
/percorso/team    → Team
```

Qui la route padre ha un `path`, quindi crea un prefisso URL.

---

## 25. Differenza tra layout route e nested route con path

Sono entrambe rotte annidate, ma hanno scopi diversi.

### Route padre con `path`

```jsx
<Route path="/percorso">
  <Route index element={<SuDiNoi />} />
  <Route path="storia" element={<Storia />} />
</Route>
```

Serve per creare una struttura URL comune.

```txt
/percorso
/percorso/storia
```

### Route padre con `element`

```jsx
<Route element={<MainLayout />}>
  <Route index element={<Home />} />
  <Route path="contact" element={<Contact />} />
</Route>
```

Serve per creare un layout condiviso.

```txt
Navbar e Footer comuni
URL non vengono prefissati
```

---

## 26. Route prefixes

C’è anche un caso intermedio: una Route padre con `path`, ma senza `element`.

Esempio:

```jsx
<Route path="projects">
  <Route index element={<ProjectsHome />} />
  <Route path=":pid" element={<Project />} />
  <Route path=":pid/edit" element={<EditProject />} />
</Route>
```

Qui la Route padre non renderizza un layout, ma aggiunge solo il prefisso `projects` alle rotte figlie.

Risultato:

```txt
/projects
/projects/12
/projects/12/edit
```

---

## 27. `index` route

Quando una rotta padre ha delle figlie, si può usare `index` per indicare la figlia predefinita.

Esempio:

```jsx
<Route path="/percorso">
  <Route index element={<SuDiNoi />} />
  <Route path="storia" element={<Storia />} />
</Route>
```

Qui:

```txt
/percorso → SuDiNoi
/percorso/storia → Storia
```

`index` significa:

```txt
mostra questo componente quando l’URL corrisponde esattamente al padre
```

È simile a `path=""`, ma più esplicito.

Nota importante: una index route non può avere figli; se hai bisogno di figli, probabilmente ti serve una layout route.

---

## 28. Parametri dinamici

Se devi creare pagine dinamiche, non crei una rotta per ogni elemento.

Non fai:

```jsx
<Route path="/users/1" element={<User1 />} />
<Route path="/users/2" element={<User2 />} />
<Route path="/users/3" element={<User3 />} />
```

Fai:

```jsx
<Route path="/users/:userId" element={<User />} />
```

`:userId` è una variabile nell’URL.

Esempi validi:

```txt
/users/1
/users/3
/users/mario
```

Se un segmento del path inizia con `:`, diventa dinamico e il valore viene reso disponibile come parametro.

---

## 29. `useParams()`

Per leggere il parametro dinamico si usa `useParams`.

```jsx
import { useParams } from "react-router"

function User() {
  const { userId } = useParams()

  return <h1>Utente {userId}</h1>
}
```

Se visiti:

```txt
/users/3
```

allora:

```js
userId = "3"
```

Attenzione importante:

> I parametri URL sono sempre stringhe.

Quindi se nel database hai ID numerici, devi convertire:

```js
Number(userId)
```

oppure:

```js
parseInt(userId)
```

---

## 30. Esempio con ricerca dati

```jsx
import { useParams } from "react-router"

const usersData = [
  { id: 1, name: "Giangiacomo" },
  { id: 3, name: "Mario" },
]

function User() {
  const { userId } = useParams()

  const userFound = usersData.find((user) => user.id === Number(userId))

  if (!userFound) {
    return <h1>Nessun utente trovato</h1>
  }

  return (
    <div>
      <h1>Ciao {userFound.name}</h1>
      <p>ID profilo: {userId}</p>
    </div>
  )
}

export default User
```

Qui succede:

```txt
/users/3
↓
userId = "3"
↓
Number(userId) = 3
↓
cerco utente con id 3
↓
mostro Mario
```

Se l’id non esiste, mostro un messaggio di errore.

---

## 31. Parametri dinamici in un e-commerce

Nel tuo caso:

```jsx
<Route path="/products/:productId" element={<ProductSingle />} />
```

Dentro `ProductSingle`:

```jsx
const { productId } = useParams()
```

Poi puoi fare:

```jsx
fetch(`https://fakestoreapi.com/products/${productId}`)
```

Oppure, se i prodotti sono già nel context:

```jsx
const product = products.find(
  (product) => product.id === Number(productId)
)
```

---

## 32. Parametri multipli

Puoi avere anche più parametri dinamici nella stessa route.

Esempio:

```jsx
<Route path="/c/:categoryId/p/:productId" element={<Product />} />
```

Dentro il componente:

```jsx
const { categoryId, productId } = useParams()
```

Questo può servire per URL più strutturati:

```txt
/c/electronics/p/15
```

Attenzione: i nomi dei parametri nello stesso path devono essere unici, altrimenti uno sovrascrive l’altro nell’oggetto `params`.

---

## 33. Optional segments

React Router permette anche segmenti opzionali con `?`.

Esempio:

```jsx
<Route path=":lang?/categories" element={<Categories />} />
```

Così puoi avere:

```txt
/categories
/en/categories
/it/categories
```

Oppure puoi rendere opzionale un segmento statico:

```jsx
<Route path="users/:userId/edit?" element={<User />} />
```

Questa è una funzionalità più avanzata.

---

## 34. Search params: valori dopo `?`

Oltre ai parametri nel path, esistono i **search params**, cioè i valori dopo il punto interrogativo.

Esempio:

```txt
/products?category=tech&q=phone
```

Qui:

```txt
category = tech
q = phone
```

React Router mette a disposizione `useSearchParams`.

Esempio:

```jsx
import { useSearchParams } from "react-router"

function SearchResults() {
  const [searchParams] = useSearchParams()

  return (
    <p>
      Hai cercato: {searchParams.get("q")}
    </p>
  )
}
```

I search params sono i valori dopo `?` nell’URL e sono accessibili con `useSearchParams`, che restituisce un’istanza di `URLSearchParams`.

### `useSearchParams` come stato nell’URL

`useSearchParams` si può pensare come una specie di `useState`, ma invece di salvare il valore solo nello stato React, lo salva nell’URL.

Con `useState` il filtro vive solo nella memoria del componente:

```jsx
const [category, setCategory] = useState("tech")
```

Con `useSearchParams`, invece, il filtro può diventare parte dell’indirizzo:

```txt
/products?category=tech
```

Questo è utile perché l’URL diventa condivisibile. Se l’utente copia il link e lo manda a un’altra persona, anche quella persona può vedere la stessa ricerca o lo stesso filtro.

Esempio:

```jsx
import { useSearchParams } from "react-router"

function Products() {
  const [searchParams, setSearchParams] = useSearchParams()

  const category = searchParams.get("category") || "all"

  function filterByTech() {
    setSearchParams({ category: "tech" })
  }

  return (
    <>
      <p>Categoria attiva: {category}</p>
      <button onClick={filterByTech}>Filtra Tech</button>
    </>
  )
}
```

Schema mentale:

```txt
useState
↓
salva il dato nel componente

useSearchParams
↓
salva il dato nell’URL
```

---

## 35. `useNavigate()`

`useNavigate` serve per cambiare pagina tramite codice.

Esempio:

```jsx
import { useNavigate } from "react-router"

function Login() {
  const navigate = useNavigate()

  function handleLogin() {
    // logica login...
    navigate("/dashboard")
  }

  return <button onClick={handleLogin}>Login</button>
}
```

È utile quando non hai un click su un link, ma vuoi navigare dopo un evento.

Esempi tipici:

```txt
dopo il login
dopo un submit
dopo una cancellazione
quando clicco “prodotto successivo”
```

Differenza con `Link`:

```txt
Link        → dichiarativo, vive nel JSX
useNavigate → imperativo, si usa in funzioni/event handler/effect
```

Esempi:

```jsx
navigate("/product-list")
navigate(-1)
```

`navigate(-1)` torna alla pagina precedente, come il tasto Indietro del browser.

Per la navigazione standard è meglio usare `Link` o `NavLink`, perché gestiscono meglio accessibilità, apertura in nuova scheda, click destro e comportamento naturale dei link. `useNavigate` va riservato ai casi in cui la navigazione avviene via codice.

### Quando usare `useNavigate`

Usa `useNavigate` quando prima di cambiare pagina devi eseguire una logica JavaScript.

Esempi pratici:

```txt
dopo un submit
dopo un login
dopo un salvataggio
dopo un controllo condizionale
dopo un timer
per andare al prodotto precedente o successivo
```

Esempio:

```jsx
const navigate = useNavigate()

function handleSubmit(event) {
  event.preventDefault()

  // qui potresti salvare dati, validare una form, fare una POST, ecc.
  navigate("/dashboard")
}
```

Quindi:

```txt
Link / NavLink → cambio pagina direttamente dal JSX
useNavigate    → cambio pagina dopo aver eseguito codice
```

---

## 36. `useLocation()`

`useLocation` serve per leggere informazioni sull’URL corrente.

```jsx
import { useLocation } from "react-router"

function Page() {
  const location = useLocation()

  console.log(location.pathname)
  console.log(location.search)
  console.log(location.hash)
  console.log(location.state)

  return <h1>Pagina corrente: {location.pathname}</h1>
}
```

Esempio URL:

```txt
/products?category=tech
```

Risultato:

```js
location.pathname = "/products"
location.search = "?category=tech"
```

L’oggetto `location` può contenere:

```txt
pathname → path corrente, es. /products/5
search   → query string, es. ?category=tech
hash     → frammento, es. #reviews
state    → eventuali dati passati durante la navigazione
```

### `location.state`

`location.state` permette di passare piccoli dati da una pagina all’altra senza mostrarli nell’URL.

Esempio:

```jsx
<Link to="/checkout" state={{ fromCart: true }}>
  Vai al checkout
</Link>
```

Nella pagina di destinazione puoi recuperarli con `useLocation`:

```jsx
import { useLocation } from "react-router"

function Checkout() {
  const location = useLocation()

  console.log(location.state)

  return <h1>Checkout</h1>
}
```

In questo esempio, l’URL resta pulito:

```txt
/checkout
```

ma la pagina può comunque sapere che l’utente arriva dal carrello.

È utile per informazioni temporanee e leggere, ma non va usato per dati fondamentali: se l’utente ricarica la pagina o apre direttamente l’URL, quello `state` potrebbe non esserci.

---

## 37. `Navigate`: redirect dichiarativo

Oltre all’hook `useNavigate`, React Router ha anche il componente `Navigate`.

Esempio:

```jsx
import { Navigate } from "react-router"

function PrivateRoute({ isLogged, children }) {
  if (!isLogged) {
    return <Navigate to="/" />
  }

  return children
}
```

`Navigate` esegue un redirect direttamente nel JSX.

Differenza:

```txt
useNavigate → hook, si usa dentro funzioni/eventi/effect
Navigate    → componente, si usa nel return/rendering condizionale
```

È utile quando il redirect dipende da una condizione di render.

### `Navigate` con `replace`

`Navigate` può ricevere la prop `replace`.

Esempio:

```jsx
<Navigate to="/login" replace />
```

Con `replace`, la nuova pagina sostituisce quella corrente nella cronologia invece di aggiungerne una nuova.

Questo è utile nei redirect.

Esempio pratico:

```txt
utente prova ad aprire una pagina privata
↓
non è loggato
↓
viene mandato a /login con replace
↓
se preme Indietro, non torna alla pagina privata bloccata
```

Senza `replace`, la pagina protetta potrebbe restare nella cronologia del browser. Con `replace`, invece, il redirect è più pulito.

---

## 38. Rotte private

Una rotta privata è una rotta accessibile solo se una certa condizione è vera, di solito se l’utente è autenticato.

Esempio:

```jsx
function PrivateRoute({ isLogged, children }) {
  if (!isLogged) {
    return <Navigate to="/" />
  }

  return children
}
```

Uso in `App.jsx`:

```jsx
<Route
  path="product-list"
  element={
    <PrivateRoute isLogged={isLogged}>
      <ProductList />
    </PrivateRoute>
  }
/>
```

Flusso:

```txt
se isLogged è false
↓
redirect alla Home

se isLogged è true
↓
mostra ProductList
```

Questo pattern è utile per aree riservate, dashboard, profili utente, carrelli protetti, ecc.

---

## 39. `element` vs `Component`

React Router permette due sintassi.

### Sintassi con `element`

```jsx
<Route path="/contact" element={<Contact />} />
```

Qui passi un elemento React già creato.

È utile se vuoi passare props:

```jsx
<Route path="/contact" element={<Contact title="Contatti" />} />
```

### Sintassi con `Component`

```jsx
<Route path="/contact" Component={Contact} />
```

Qui passi il componente, non JSX.

React Router crea internamente l’elemento.

Questa sintassi è comoda quando non devi passare props.

### Regola semplice

```txt
element   → vuole <Componente />
Component → vuole Componente
```

Corretto:

```jsx
<Route element={<MainLayout />} />
```

Corretto:

```jsx
<Route Component={MainLayout} />
```

Sbagliato:

```jsx
<Route element={MainLayout} />
```

---

## 40. Esempio completo con `element`

```jsx
import { BrowserRouter, Routes, Route } from "react-router"
import MainLayout from "./layouts/MainLayout"
import Home from "./pages/Home"
import Contact from "./pages/Contact"
import User from "./pages/User"
import NotFound from "./pages/NotFound"

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<MainLayout />}>
          <Route index element={<Home />} />
          <Route path="contact" element={<Contact />} />
          <Route path="users/:userId" element={<User />} />
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

---

## 41. Esempio completo con `Component`

```jsx
import { BrowserRouter, Routes, Route } from "react-router"
import MainLayout from "./layouts/MainLayout"
import Home from "./pages/Home"
import Contact from "./pages/Contact"
import User from "./pages/User"
import NotFound from "./pages/NotFound"

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route Component={MainLayout}>
          <Route index Component={Home} />
          <Route path="contact" Component={Contact} />
          <Route path="users/:userId" Component={User} />
          <Route path="*" Component={NotFound} />
        </Route>
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

Nota:

```jsx
<Route index Component={Home} />
```

è la pagina predefinita del layout.

---

## 42. Fetch al mount con `useEffect`

Di seguito viene ripassato anche un concetto importante collegato alle pagine di routing: caricare dati quando una pagina viene montata.

Esempio pagina lista prodotti:

```jsx
import { useEffect, useState } from "react"

function ProductList() {
  const [products, setProducts] = useState([])

  useEffect(() => {
    fetch("https://fakestoreapi.com/products")
      .then((res) => res.json())
      .then((data) => setProducts(data))
  }, [])

  return (
    <div>
      {products.map((product) => (
        <p key={product.id}>{product.title}</p>
      ))}
    </div>
  )
}

export default ProductList
```

L’array vuoto:

```jsx
[]
```

significa:

```txt
esegui questa fetch una sola volta, dopo il primo render
```

Questo è il momento giusto per caricare dati da un’API quando entri in una pagina.

---

## 43. History API

Le **History API** sono API native del browser. React Router le usa internamente per simulare la navigazione in una SPA.

Normalmente:

```js
window.location.href = "/about"
```

causa un reload della pagina.

Con History API:

```js
history.pushState({}, "", "/about")
```

l’URL cambia, ma la pagina non viene ricaricata.

Le principali API sono:

```txt
history.pushState(state, "", "/about")
↓
cambia URL e aggiunge una voce alla cronologia

history.replaceState(state, "", "/about")
↓
cambia URL ma sostituisce la voce corrente

popstate
↓
evento che scatta quando l’utente usa Avanti/Indietro
```

`BrowserRouter` è un wrapper attorno a queste API:

```txt
Link / useNavigate
↓
BrowserRouter usa pushState
↓
URL cambia senza reload

tasto avanti/indietro
↓
popstate
↓
React Router aggiorna i componenti
```

---

## 44. CSS Modules

Ripasso anche dell’uso dei **CSS Modules**.

Un file CSS Module si chiama così:

```txt
MainLayout.module.css
```

Lo importi come oggetto:

```jsx
import styles from "./MainLayout.module.css"
```

Poi usi le classi così:

```jsx
<NavLink
  className={({ isActive }) =>
    `nav-link ${isActive ? `active ${styles.activeLink}` : ""}`
  }
>
  Home
</NavLink>
```

Il vantaggio è che le classi vengono trasformate in nomi univoci a build time.

Esempio:

```txt
.activeLink
↓
MainLayout_activeLink__xK2p
```

Questo evita conflitti con classi globali, per esempio quelle di Bootstrap.

---

## 45. Rules of Hooks

Un altro ripasso importante riguarda le **Rules of Hooks**.

React traccia gli hook per posizione. Questo significa che a ogni render gli hook devono essere chiamati sempre nello stesso ordine.

Sbagliato:

```jsx
if (condizione) {
  const [data, setData] = useState(null)
}
```

Perché in alcuni render l’hook viene chiamato, in altri no.

Corretto:

```jsx
const [data, setData] = useState(null)

useEffect(() => {
  if (!condizione) return

  fetch(...)
}, [condizione])
```

Regola:

```txt
gli hook vanno chiamati sempre al primo livello del componente
mai dentro if
mai dentro for
mai dentro funzioni annidate
```

La condizione si mette **dentro** l’hook, non attorno all’hook.

---


## Tabella di scelta rapida

| Se devi... | Usa... | Perché |
|---|---|---|
| Configurare il router dell’app | `BrowserRouter` | Collega React Router alla cronologia del browser |
| Definire la struttura delle pagine | `Routes` + `Route` | Mappano URL e componenti |
| Creare un link standard | `Link` | Naviga senza refresh |
| Creare un link nella navbar | `NavLink` | Aggiunge la classe `active` |
| Creare una struttura comune | `Outlet` | Inserisce le pagine figlie dentro il layout |
| Reindirizzare in JSX | `Navigate` | Fa redirect in base a una condizione |
| Cambiare pagina da una funzione | `useNavigate` | Navigazione programmatica |
| Leggere un ID nell’URL | `useParams` | Recupera segmenti dinamici tipo `:id` |
| Gestire filtri nell’URL | `useSearchParams` | Legge e modifica query string |
| Sapere dove si trova l’utente | `useLocation` | Legge `pathname`, `search` e `state` |

---

## 46. Riferimento rapido: componenti

| Componente | Quando usarlo | Esempio |
|---|---|---|
| `BrowserRouter` | Abilita la navigazione client-side | `<BrowserRouter>...</BrowserRouter>` |
| `Routes` | Contiene la mappa delle rotte | `<Routes>...</Routes>` |
| `Route` | Collega URL e componente | `<Route path="about" Component={About} />` |
| `Link` | Navigazione interna generica | `<Link to="/about">Vai</Link>` |
| `NavLink` | Link con stato active | `<NavLink className={({ isActive }) => ...}>` |
| `Navigate` | Redirect dichiarativo nel JSX | `<Navigate to="/" />` |
| `Outlet` | Segnaposto per rotte figlie | `<Outlet />` |

---

## 47. Riferimento rapido: hook

| Hook | Quando usarlo | Esempio |
|---|---|---|
| `useParams` | Leggere parametri dinamici dell’URL | `const { id } = useParams()` |
| `useNavigate` | Navigare via codice | `const navigate = useNavigate()` |
| `useLocation` | Leggere informazioni sull’URL corrente | `location.pathname` |
| `useSearchParams` | Leggere/modificare query params | `params.get("key")` |

---

## 48. Schema finale dei concetti

```txt
SPA
↓
una sola pagina HTML, contenuto gestito da React

Routing manuale
↓
useState + rendering condizionale, ma URL non cambia

React Router
↓
sincronizza URL e componenti React

BrowserRouter
↓
abilita il sistema di routing

Routes
↓
decide quale Route mostrare

Route
↓
collega path e componente

Link
↓
navigazione interna senza refresh

NavLink
↓
Link con classe active automatica

end
↓
evita che un path padre risulti active anche su pagine figlie

Outlet
↓
punto dove vengono renderizzate le route figlie

layout route senza path
↓
aggiunge struttura comune ma non modifica l’URL

route padre con path senza element
↓
aggiunge un prefisso URL ma non renderizza layout

index
↓
pagina predefinita della route padre

path="*"
↓
catch-all per 404

useParams
↓
legge parametri dinamici, sempre stringhe

useSearchParams
↓
legge valori dopo ? nell’URL

useNavigate
↓
navigazione via codice

Navigate
↓
redirect dichiarativo nel JSX

useLocation
↓
legge info dell’URL corrente

element
↓
usa JSX: element={<Home />}

Component
↓
usa riferimento al componente: Component={Home}

History API
↓
base tecnica che permette di cambiare URL senza reload

Rules of Hooks
↓
gli hook vanno chiamati sempre al primo livello
```
