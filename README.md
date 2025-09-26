# React Todo List - Tutorial

Deze tutorial laat je een volledig werkende React Todo applicatie bouwen. Je gaat leren over state management, event handling, components, en meer door het zelf te doen!

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

## � Hulp Nodig?

Kijk in `cheat-sheet.md` als je vastloopt, daar staan alle antwoorden. Doe dit wel alleen als je echt niet meer verder komt. Het gaat erom dat je zelf leert!

## 🎯 Opdrachten

### Opdracht 1: State Toevoegen

**Doel:** Voeg React state toe aan je App component voor het opslaan van todos.

**Wat moet je doen:**
- Importeer `useState` from React
- Maak een state variabele `todos` die start als een lege array
- Zorg dat je h1 element de class `header` gebruikt

**Test:** Je app moet nog steeds werken zonder errors in de console.

### Opdracht 2: Formulier Component Maken

**Doel:** Maak een herbruikbaar formulier component voor het toevoegen van nieuwe todos.

**Wat moet je doen:**
- Maak een nieuw bestand `src/NewTodoForm.jsx`
- Maak een component die een `onSubmit` prop accepteert
- Voeg state toe voor de input waarde (`newItem`)
- Maak een form met een text input en submit button
- Voorkom dat de pagina refresht bij form submit
- Clear de input na het submitten
- Zorg dat lege todos niet gesubmit worden

**CSS Classes die je moet gebruiken:**
- `new-item-form` - Voor het form element
- `form-row` - Voor de div die label en input bevat
- `btn` - Voor de submit button

**Tips:**
- Gebruik `useState` voor de input waarde
- Gebruik `e.preventDefault()` in de submit handler
- Maak de input "controlled" met `value` en `onChange`

### Opdracht 3: Todo Toevoegen Functionaliteit

**Doel:** Maak het mogelijk om nieuwe todos toe te voegen aan je lijst.

**Wat moet je doen:**
- Importeer je `NewTodoForm` component in `App.jsx`
- Maak een `addTodo` functie die een title parameter accepteert
- De functie moet een nieuw todo object maken met: `id`, `title`, en `completed: false`
- Voeg het nieuwe todo toe aan de bestaande todos array
- Geef de `addTodo` functie door aan `NewTodoForm` als `onSubmit` prop
- Plaats het formulier boven de header in je JSX

**Tips:**
- Gebruik `crypto.randomUUID()` voor unieke IDs of gebruik de useId hook van React
- Gebruik de spread operator (`...`) om arrays te combineren
- Gebruik functional state updates: `setTodos(currentTodos => ...)`

### Opdracht 4: TodoList Component Maken

**Doel:** Maak een component die de lijst met todos weergeeft.

**Wat moet je doen:**
- Maak een nieuw bestand `src/TodoList.jsx`
- Maak een component die `todos`, `toggleTodo`, en `deleteTodo` props accepteert
- Render een `<ul>` element met de class `list`
- Toon "No Todos" als de todos array leeg is
- Map over de todos array en render voor elke todo een `TodoItem` component
- Geef alle todo properties door aan `TodoItem` plus de callback functies
- Vergeet niet de `key` prop!

**CSS Classes die je moet gebruiken:**
- `list` - Voor het ul element

**Tips:**
- Gebruik conditional rendering: `{condition && <element>}`
- Gebruik `.map()` om over de todos te itereren
- Gebruik spread operator (`{...todo}`) om alle todo properties door te geven
- De `key` prop is verplicht bij het renderen van lijsten

### Opdracht 5: TodoItem Component Maken

**Doel:** Maak een component voor individuele todo items met checkbox en delete button.

**Wat moet je doen:**
- Maak een nieuw bestand `src/TodoItem.jsx`
- Maak een component die props accepteert: `completed`, `id`, `title`, `toggleTodo`, `deleteTodo`
- Render een `<li>` element
- Maak een `<label>` met daarin:
  - Een checkbox input die `checked` is als de todo completed is
  - De title van de todo
- Voeg een delete button toe naast de label
- Zorg dat de checkbox de `toggleTodo` functie aanroept met de juiste parameters
- Zorg dat de delete button de `deleteTodo` functie aanroept met het todo ID

**CSS Classes die je moet gebruiken:**
- `btn btn-danger` - Voor de delete button (beide classes!)

**Tips:**
- Gebruik destructuring voor de props
- Bij onChange: geef zowel het ID als de nieuwe checked status door
- Bij onClick: gebruik een arrow function om de functie pas bij klik aan te roepen

### Opdracht 6: Toggle en Delete Functies

**Doel:** Implementeer de functionaliteit om todos aan/uit te vinken en te verwijderen.

**Wat moet je doen:**
- Importeer je `TodoList` component in `App.jsx`
- Maak een `toggleTodo` functie die `id` en `completed` parameters accepteert
  - Deze moet de juiste todo vinden en zijn completed status updaten
  - Andere todos blijven ongewijzigd
- Maak een `deleteTodo` functie die een `id` parameter accepteert
  - Deze moet de todo met dat ID uit de array verwijderen
- Voeg de `TodoList` component toe onder je header
- Geef alle benodigde props door: `todos`, `toggleTodo`, `deleteTodo`

**Tips:**
- Gebruik `.map()` voor `toggleTodo` - update alleen de todo met het juiste ID
- Gebruik `.filter()` voor `deleteTodo` - behoud alleen todos die NIET het gegeven ID hebben
- Vergeet niet nieuwe arrays/objecten te returnen (React immutability)

### Opdracht 7: LocalStorage Persistentie (Bonus)

**Doel:** Zorg dat todos bewaard blijven als je de pagina refresht.

**Wat moet je doen:**
- Importeer `useEffect` naast `useState` in `App.jsx`
- Verander de `useState([])` naar een functie die probeert data uit localStorage te laden
  - Gebruik `localStorage.getItem("ITEMS")`
  - Als er geen data is, return een lege array
  - Als er wel data is, parse het met `JSON.parse()`
- Voeg een `useEffect` toe die de todos naar localStorage schrijft elke keer als ze veranderen
  - Gebruik `localStorage.setItem("ITEMS", JSON.stringify(todos))`
  - Zorg dat het effect afhankelijk is van `[todos]`

**Tips:**
- Lazy initial state: `useState(() => { ... })` - de functie wordt maar één keer uitgevoerd
- `JSON.stringify()` om JavaScript objecten naar strings te converteren
- `JSON.parse()` om strings terug naar JavaScript objecten te converteren
- Test het door todos toe te voegen en de pagina te refreshen!

## ✅ Checklist

Gebruik deze checklist om te controleren of je alles hebt geïmplementeerd:

- [ ] **Opdracht 1**: State toegevoegd aan App component
- [ ] **Opdracht 2**: NewTodoForm component gemaakt
- [ ] **Opdracht 3**: addTodo functie werkt
- [ ] **Opdracht 4**: TodoList component toont todos
- [ ] **Opdracht 5**: TodoItem component met checkbox en delete
- [ ] **Opdracht 6**: Toggle en delete functionaliteit werkt
- [ ] **Opdracht 7**: LocalStorage persistentie (bonus)

## 🎨 CSS Classes Beschikbaar

De `styles.css` bevat alle styling. Belangrijkste classes die je moet gebruiken:

### Layout & Structure
- `header` - Voor de hoofdtitel
- `new-item-form` - Voor het form element
- `form-row` - Voor form rij containers
- `list` - Voor de ul todo lijst

### Buttons
- `btn` - Basis button styling (blauw)
- `btn btn-danger` - Voor delete buttons (rood, gebruik beide classes!)

### Automatische Styling
- Input fields krijgen automatisch styling
- Hover en focus states zijn al gedefinieerd
- Afgevinkte todos krijgen automatisch grijze tekst

## 🚀 Extra Uitdagingen

Als je de basis tutorial hebt voltooid, probeer deze uitbreidingen:

1. **Edit Functionaliteit**: Dubbelklik om todo tekst te bewerken
2. **Categorieën**: Voeg kleur-gecodeerde categorieën toe
3. **Due Dates**: Voeg vervaldatums en alerts toe
4. **Priority Levels**: Hoog, normaal, laag prioriteiten
5. **Search/Filter**: Zoek of filter todos op tekst/status
6. **Keyboard Shortcuts**: Enter voor toevoegen, Delete voor verwijderen
7. **useContext**: Gebruik Context API voor global state management in plaats van prop drilling
8. **Custom Hooks**: Maak herbruikbare hooks voor:
   - `useLocalStorage` - localStorage functionaliteit
   - `useSearch` - search/filter logica
   - `useTodos` - alle todo operaties
9. **Performance Optimizations**: Implementeer:
   - `React.memo` voor components
   - `useMemo` en `useCallback` voor expensive operations
   - Lazy loading van components
   - Virtualisatie voor grote todo lijsten
