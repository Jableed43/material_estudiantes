# React: Componentes, Estados y Formularios 🧩

## Estados en React

### useState Hook

`useState` es un hook que permite a los componentes funcionales tener estado.

```jsx
import { useState } from 'react'

function Contador() {
  const [count, setCount] = useState(0)
  
  const incrementar = () => {
    setCount(count + 1)
  }
  
  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={incrementar}>Incrementar</button>
    </div>
  )
}
```

### Tipos de Estado

```jsx
function Componente() {
  // Estado numérico
  const [count, setCount] = useState(0)
  
  // Estado string
  const [nombre, setNombre] = useState('')
  
  // Estado booleano
  const [activo, setActivo] = useState(false)
  
  // Estado array
  const [items, setItems] = useState([])
  
  // Estado objeto
  const [usuario, setUsuario] = useState({ nombre: '', edad: 0 })
  
  return <div>...</div>
}
```

### Actualización de Estado

```jsx
// Actualización directa
setCount(5)

// Actualización basada en valor anterior (recomendado)
setCount(prevCount => prevCount + 1)
```

---

## Renderizado de Listas y Keys

### Renderizar Listas con `.map()`

```jsx
function ListaTareas() {
  const tareas = [
    { id: 1, texto: "Aprender React" },
    { id: 2, texto: "Hacer ejercicio" },
    { id: 3, texto: "Leer un libro" }
  ]
  
  return (
    <ul>
      {tareas.map(tarea => (
        <li key={tarea.id}>{tarea.texto}</li>
      ))}
    </ul>
  )
}
```

### Keys en React

Las keys son importantes para el rendimiento de React:

```jsx
// ✅ CORRECTO: Usar ID único
{items.map(item => (
  <li key={item.id}>{item.nombre}</li>
))}

// ⚠️ ACEPTABLE: Usar índice solo si la lista NO cambia
{items.map((item, index) => (
  <li key={index}>{item}</li>
))}
```

---

## Renderizado Condicional

### if/else

```jsx
function Saludo({ usuario }) {
  if (usuario) {
    return <h1>Hola, {usuario.nombre}!</h1>
  } else {
    return <h1>Hola, Invitado!</h1>
  }
}
```

### Operador Ternario

```jsx
function Saludo({ usuario }) {
  return (
    <h1>
      {usuario ? `Hola, ${usuario.nombre}!` : 'Hola, Invitado!'}
    </h1>
  )
}
```

### Operador &&

```jsx
function Lista({ items }) {
  return (
    <div>
      {items.length > 0 && (
        <ul>
          {items.map(item => <li key={item.id}>{item.nombre}</li>)}
        </ul>
      )}
    </div>
  )
}
```

---

## Formularios Controlados

### Input Controlado

```jsx
function Formulario() {
  const [nombre, setNombre] = useState('')
  
  const handleChange = (e) => {
    setNombre(e.target.value)
  }
  
  const handleSubmit = (e) => {
    e.preventDefault()
    console.log('Nombre:', nombre)
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text" 
        value={nombre}
        onChange={handleChange}
        placeholder="Ingresa tu nombre"
      />
      <button type="submit">Enviar</button>
    </form>
  )
}
```

### Múltiples Inputs

```jsx
function Formulario() {
  const [formData, setFormData] = useState({
    nombre: '',
    email: '',
    edad: ''
  })
  
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    })
  }
  
  return (
    <form>
      <input 
        name="nombre"
        value={formData.nombre}
        onChange={handleChange}
      />
      <input 
        name="email"
        value={formData.email}
        onChange={handleChange}
      />
    </form>
  )
}
```

---

## Virtual DOM

### ¿Qué es el Virtual DOM?

El **Virtual DOM** es una representación en memoria del DOM real. React lo usa para optimizar las actualizaciones.

**Proceso**:
1. React crea una representación virtual del DOM
2. Cuando el estado cambia, React crea un nuevo Virtual DOM
3. React compara (diff) el Virtual DOM anterior con el nuevo
4. React actualiza solo las partes que cambiaron en el DOM real

**Ventajas**:
- ✅ Mejor rendimiento
- ✅ Actualizaciones eficientes
- ✅ Menos manipulación directa del DOM

---

## Componentización

### ¿Cuándo Componentizar?

**Componentizar cuando**:
- ✅ El código se repite
- ✅ Un componente es muy grande (> 100 líneas)
- ✅ Una parte tiene responsabilidad específica
- ✅ Quieres reutilizar código

**Ejemplo**:

```jsx
// Antes: Todo en un componente
function App() {
  return (
    <div>
      <header>...</header>
      <nav>...</nav>
      <main>...</main>
      <footer>...</footer>
    </div>
  )
}

// Después: Componentizado
function App() {
  return (
    <div>
      <Header />
      <Navbar />
      <Main />
      <Footer />
    </div>
  )
}
```

---

## Ejemplos Prácticos

### Ejemplo 1: Lista con Estado

```jsx
function ListaTareas() {
  const [tareas, setTareas] = useState([])
  const [nuevaTarea, setNuevaTarea] = useState('')
  
  const agregarTarea = () => {
    if (nuevaTarea.trim()) {
      setTareas([...tareas, { id: Date.now(), texto: nuevaTarea }])
      setNuevaTarea('')
    }
  }
  
  return (
    <div>
      <input 
        value={nuevaTarea}
        onChange={(e) => setNuevaTarea(e.target.value)}
      />
      <button onClick={agregarTarea}>Agregar</button>
      <ul>
        {tareas.map(tarea => (
          <li key={tarea.id}>{tarea.texto}</li>
        ))}
      </ul>
    </div>
  )
}
```

### Ejemplo 2: Formulario Completo

```jsx
function FormularioUsuario() {
  const [usuario, setUsuario] = useState({
    nombre: '',
    email: '',
    edad: ''
  })
  
  const handleChange = (e) => {
    setUsuario({
      ...usuario,
      [e.target.name]: e.target.value
    })
  }
  
  const handleSubmit = (e) => {
    e.preventDefault()
    console.log('Usuario:', usuario)
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        name="nombre"
        value={usuario.nombre}
        onChange={handleChange}
        placeholder="Nombre"
      />
      <input 
        name="email"
        type="email"
        value={usuario.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input 
        name="edad"
        type="number"
        value={usuario.edad}
        onChange={handleChange}
        placeholder="Edad"
      />
      <button type="submit">Enviar</button>
    </form>
  )
}
```

---

## Conceptos Clave

1. **useState**: Hook para manejar estado en componentes funcionales
2. **Keys**: Identificadores únicos para elementos en listas
3. **Renderizado Condicional**: Mostrar contenido según condiciones
4. **Formularios Controlados**: Inputs controlados por estado de React
5. **Virtual DOM**: Representación en memoria del DOM
6. **Componentización**: Dividir código en componentes reutilizables
7. **Inmutabilidad**: No mutar estado directamente, crear nuevos objetos

---

## Buenas Prácticas

- Usa keys únicas y estables en listas
- Actualiza estado con funciones callback cuando dependa del valor anterior
- Mantén componentes pequeños y enfocados
- Usa formularios controlados para inputs
- No mutes estado directamente (usa spread operator)
- Componentiza cuando el código se repite o es muy grande
- Usa renderizado condicional para mostrar/ocultar elementos

