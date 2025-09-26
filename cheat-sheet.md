# React Todo List - Cheat Sheet

Deze cheat sheet bevat alle antwoorden voor de React Todo tutorial. Gebruik dit alleen als je vastloopt!

## 📋 Wat ga je bouwen?

Een todo applicatie met de volgende functionaliteiten:
- Nieuwe todos toevoegen
- Todos als voltooid markeren/unmarkeren
- Todos verwijderen
- Lokale opslag (localStorage) voor persistentie
- Responsive design met mooie styling

## 🛠️ Setup

Het project is al opgezet met Vite. Start de development server:

```bash
npm install
npm run dev
```

## 📁 Project Structuur

Je vindt de volgende bestanden:
- `src/App.jsx` - Je hoofdcomponent (startpunt)
- `src/styles.css` - Complete styling (al beschikbaar!)
- `src/main.jsx` - Entry point (hoef je niet aan te passen)

## 🎯 Tutorial Antwoorden

### Stap 1: Basis State Setup

Begin in `App.jsx` met het toevoegen van React hooks en basis state:

```jsx
import { useState } from "react"
import "./styles.css"

export default function App() {
  const [todos, setTodos] = useState([])

  return (
    <>
      <h1 className="header">Todo List</h1>
    </>
  )
}
```

**💡 Tip:** useState geeft je een state variabele en een functie om deze bij te werken.

### Stap 2: Formulier Component Maken

Maak een nieuw bestand `src/NewTodoForm.jsx`:

```jsx
import { useState } from "react"

export function NewTodoForm({ onSubmit }) {
  const [newItem, setNewItem] = useState("")

  function handleSubmit(e) {
    e.preventDefault()
    if (newItem === "") return

    onSubmit(newItem)
    setNewItem("")
  }

  return (
    <form onSubmit={handleSubmit} className="new-item-form">
      <div className="form-row">
        <label htmlFor="item">New Item</label>
        <input
          value={newItem}
          onChange={e => setNewItem(e.target.value)}
          type="text"
          id="item"
        />
      </div>
      <button className="btn">Add</button>
    </form>
  )
}
```

**📝 CSS Classes gebruikt:**
- `new-item-form` - Styling voor het formulier
- `form-row` - Styling voor een rij in het formulier
- `btn` - Basis button styling

**💡 Tips:**
- `onSubmit` prop wordt gebruikt om data naar de parent component te sturen
- `e.preventDefault()` voorkomt dat de pagina refresht
- Controlled input: `value` en `onChange` zorgen dat React de input controleert

### Stap 3: addTodo Functie Implementeren

Voeg de `addTodo` functie toe aan `App.jsx` en importeer het formulier:

```jsx
import { useState } from "react"
import { NewTodoForm } from "./NewTodoForm"
import "./styles.css"

export default function App() {
  const [todos, setTodos] = useState([])

  function addTodo(title) {
    setTodos(currentTodos => {
      return [
        ...currentTodos,
        { id: crypto.randomUUID(), title, completed: false },
      ]
    })
  }

  return (
    <>
      <NewTodoForm onSubmit={addTodo} />
      <h1 className="header">Todo List</h1>
    </>
  )
}
```

**💡 Tips:**
- `crypto.randomUUID()` genereert een unieke ID voor elke todo
- Spread operator (`...`) houdt de bestaande todos en voegt er één toe
- Functional state update: `setTodos(currentTodos => ...)` is veiliger

### Stap 4: TodoList Component Maken

Maak een nieuw bestand `src/TodoList.jsx`:

```jsx
import { TodoItem } from "./TodoItem"

export function TodoList({ todos, toggleTodo, deleteTodo }) {
  return (
    <ul className="list">
      {todos.length === 0 && "No Todos"}
      {todos.map(todo => (
        <TodoItem
          {...todo}
          key={todo.id}
          toggleTodo={toggleTodo}
          deleteTodo={deleteTodo}
        />
      ))}
    </ul>
  )
}
```

**📝 CSS Classes gebruikt:**
- `list` - Styling voor de todo lijst

**💡 Tips:**
- `{...todo}` spread de todo properties als individuele props
- `key={todo.id}` is verplicht voor React bij het renderen van lijsten
- Conditional rendering: `{todos.length === 0 && "No Todos"}`

### Stap 5: TodoItem Component Maken

Maak een nieuw bestand `src/TodoItem.jsx`:

```jsx
export function TodoItem({ completed, id, title, toggleTodo, deleteTodo }) {
  return (
    <li>
      <label>
        <input
          type="checkbox"
          checked={completed}
          onChange={e => toggleTodo(id, e.target.checked)}
        />
        {title}
      </label>
      <button onClick={() => deleteTodo(id)} className="btn btn-danger">
        Delete
      </button>
    </li>
  )
}
```

**📝 CSS Classes gebruikt:**
- `btn btn-danger` - Rode delete button styling

**💡 Tips:**
- `onChange={e => toggleTodo(id, e.target.checked)}` geeft de checkbox status door
- `onClick={() => deleteTodo(id)}` zorgt dat de functie pas wordt aangeroepen bij klik
- Destructuring props maakt de component cleaner

### Stap 6: Toggle en Delete Functies

Voeg de toggle en delete functies toe aan `App.jsx`:

```jsx
import { useState } from "react"
import { NewTodoForm } from "./NewTodoForm"
import { TodoList } from "./TodoList"
import "./styles.css"

export default function App() {
  const [todos, setTodos] = useState([])

  function addTodo(title) {
    setTodos(currentTodos => {
      return [
        ...currentTodos,
        { id: crypto.randomUUID(), title, completed: false },
      ]
    })
  }

  function toggleTodo(id, completed) {
    setTodos(currentTodos => {
      return currentTodos.map(todo => {
        if (todo.id === id) {
          return { ...todo, completed }
        }
        return todo
      })
    })
  }

  function deleteTodo(id) {
    setTodos(currentTodos => {
      return currentTodos.filter(todo => todo.id !== id)
    })
  }

  return (
    <>
      <NewTodoForm onSubmit={addTodo} />
      <h1 className="header">Todo List</h1>
      <TodoList todos={todos} toggleTodo={toggleTodo} deleteTodo={deleteTodo} />
    </>
  )
}
```

**💡 Tips:**
- `map()` wordt gebruikt om een specifieke todo te updaten
- `filter()` wordt gebruikt om een todo te verwijderen
- Altijd een nieuwe array/object returnen voor React state updates

### Stap 7: LocalStorage Persistentie (Bonus)

Voeg localStorage toe voor het bewaren van todos tussen sessies:

```jsx
import { useEffect, useState } from "react"
// ... andere imports

export default function App() {
  const [todos, setTodos] = useState(() => {
    const localValue = localStorage.getItem("ITEMS")
    if (localValue == null) return []
    return JSON.parse(localValue)
  })

  useEffect(() => {
    localStorage.setItem("ITEMS", JSON.stringify(todos))
  }, [todos])

  // ... rest van de component blijft hetzelfde
}
```

**💡 Tips:**
- Lazy initial state: `useState(() => ...)` wordt maar één keer uitgevoerd
- `useEffect` wordt getriggerd elke keer dat `todos` verandert
- `JSON.parse()` en `JSON.stringify()` voor data conversie

## 🎨 CSS Classes Overzicht

De `styles.css` bevat alle styling. Belangrijkste classes:

### Layout & Structure
- `header` - Hoofdtitel styling
- `new-item-form` - Formulier layout
- `form-row` - Formulier rij styling
- `list` - Todo lijst styling

### Buttons
- `btn` - Basis button styling (blauw)
- `btn btn-danger` - Delete button (rood)

### Form Elements
- Input fields krijgen automatisch styling via element selectors

### Interactive States
- Hover, focus, en checked states zijn al gedefinieerd
- Completed todos krijgen automatisch grijze tekst

## 🚀 Uitbreidingen

Als je klaar bent met de basis tutorial, probeer deze uitbreidingen:

1. **Categorieën**: Voeg categorieën toe aan todos
2. **Due Dates**: Voeg vervaldatums toe
3. **Priority Levels**: Voeg prioriteiten toe (hoog, normaal, laag)
4. **Search/Filter**: Zoek of filter todos
5. **Drag & Drop**: Herorden todos met drag and drop
6. **Dark/Light Mode**: Toggle tussen themes

## 📚 React Concepten Geleerd

Na deze tutorial heb je ervaring met:

- ✅ **Components** - Herbruikbare UI blokken
- ✅ **Props** - Data doorgeven tussen components
- ✅ **State** - Component data die kan veranderen
- ✅ **Event Handling** - Reageren op user interactions
- ✅ **Conditional Rendering** - UI tonen/verbergen
- ✅ **Lists & Keys** - Dynamische lijsten renderen
- ✅ **useEffect** - Side effects en lifecycle
- ✅ **Local Storage** - Data persistentie
- ✅ **Controlled Components** - Form input handling

## 🎯 Volgende Stappen

Klaar voor meer? Probeer te leren over:
- React Router (multiple pages)
- Context API (global state)
- Custom Hooks (reusable logic)
- Testing met Jest & React Testing Library
- TypeScript met React

Veel plezier met je React leertraject! 🚀
