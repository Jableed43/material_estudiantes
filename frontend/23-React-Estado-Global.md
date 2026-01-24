# React Fase 4: Estado Global y Escalabilidad 🏛️

## 📋 Índice

1. [Introducción: Prop Drilling](#1-introducción-prop-drilling)
2. [Context API](#2-context-api)
3. [useReducer](#3-usereducer)
4. [Redux / Redux Toolkit](#4-redux--redux-toolkit)
5. [Comparación de Soluciones](#5-comparación-de-soluciones)
6. [Ejemplos Prácticos del Código Modelo](#6-ejemplos-prácticos-del-código-modelo)
7. [Cuándo Usar Cada Solución](#7-cuándo-usar-cada-solución)

---

## 1. Introducción: Prop Drilling

### ¿Qué es Prop Drilling? (Analogía del Mundo Real)

### 📦 Analogía: Pasar una Caja por Múltiples Personas

Imagina que tienes una caja que necesita llegar a alguien:
- **Prop Drilling**: Pasas la caja por varias personas que no la necesitan, solo para que llegue a quien la necesita
  - Tú → Persona 1 (no la necesita) → Persona 2 (no la necesita) → Persona 3 (no la necesita) → Persona Final (sí la necesita)

**Es ineficiente** - muchas personas manejan la caja sin necesidad.

### 🏠 Analogía: Pasar un Mensaje por Múltiples Habitaciones

Piensa en pasar un mensaje:
- **Prop Drilling**: El mensaje pasa por múltiples habitaciones donde nadie lo necesita, solo para llegar a la habitación correcta
  - Sala → Cocina (no lo necesita) → Dormitorio (no lo necesita) → Baño (no lo necesita) → Estudio (sí lo necesita)

**Es complicado** - el mensaje pasa por muchos lugares innecesariamente.

### 🎯 Analogía: El Teléfono Descompuesto

El juego del teléfono descompuesto:
- **Prop Drilling**: El mensaje pasa por muchas personas que no lo necesitan, solo para llegar a quien lo necesita
- **Problema**: El mensaje puede cambiar o perderse en el camino

**Es ineficiente y propenso a errores**.

### ¿Qué es Prop Drilling?

**Prop Drilling** es el problema de pasar props a través de múltiples niveles de componentes intermedios que no necesitan esos datos, solo para llegar a un componente que sí los necesita.

**En términos simples**: Es como pasar una caja por muchas personas que no la necesitan, solo para que llegue a quien la necesita - es ineficiente y complicado.

### Ejemplo del Problema:

```jsx
// App.jsx - Tiene el estado
function App() {
  const [usuario, setUsuario] = useState({ nombre: 'Juan' })
  
  return <Layout usuario={usuario} />  // Pasa prop
}

// Layout.jsx - No usa el estado, solo lo pasa
function Layout({ usuario }) {
  return <Header usuario={usuario} />  // Pasa prop
}

// Header.jsx - No usa el estado, solo lo pasa
function Header({ usuario }) {
  return <UserMenu usuario={usuario} />  // Pasa prop
}

// UserMenu.jsx - FINALMENTE usa el estado
function UserMenu({ usuario }) {
  return <p>Hola, {usuario.nombre}</p>
}
```

**Problemas**:
- ❌ Código repetitivo
- ❌ Difícil de mantener
- ❌ Componentes intermedios no necesitan los datos
- ❌ Cambios requieren modificar múltiples componentes

### Soluciones:

1. **Context API**: Para estado compartido moderado
2. **useReducer**: Para estado complejo local
3. **Redux**: Para estado global complejo y aplicaciones grandes

---

## 2. Context API

### ¿Qué es Context API?

**Context API** es una característica nativa de React que permite compartir datos entre componentes sin pasar props manualmente a través de cada nivel (evita prop drilling).

### ¿Cuándo usar Context API?

**Usar Context cuando**:
- ✅ Estado compartido entre muchos componentes
- ✅ Estado que cambia frecuentemente
- ✅ Evitar prop drilling (3+ niveles)
- ✅ Estado de autenticación, tema, carrito, idioma, etc.

**NO usar Context cuando**:
- ❌ Estado local es suficiente
- ❌ Solo necesitas pasar props 1-2 niveles
- ❌ Estado muy complejo (considerar Redux)

### Componentes de Context:

#### 1. createContext

Crea un objeto de contexto:

```jsx
import { createContext } from 'react'

const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {}
})
```

**Características**:
- Crea un objeto de contexto
- El valor por defecto es opcional
- Se usa principalmente para TypeScript/autocompletado

#### 2. Provider

Envuelve componentes y provee el valor del contexto:

```jsx
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light')
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}
```

**Características**:
- Envuelve componentes que necesitan el contexto
- El `value` prop contiene los datos compartidos
- Cualquier componente hijo puede acceder al contexto

#### 3. useContext

Hook para consumir el contexto:

```jsx
import { useContext } from 'react'
import { ThemeContext } from './Contexts'

function Component() {
  const { theme, toggleTheme } = useContext(ThemeContext)
  
  return (
    <div>
      <p>Tema: {theme}</p>
      <button onClick={toggleTheme}>Cambiar</button>
    </div>
  )
}
```

**Características**:
- Hook para consumir contexto
- Retorna el valor del Provider más cercano
- Si no hay Provider, retorna el valor por defecto

### Hook Personalizado para Context

Crear un hook personalizado simplifica el uso y valida el uso correcto:

```jsx
function useCart() {
  const context = useContext(CartContext)
  
  if (!context) {
    throw new Error('useCart debe usarse dentro de CartProvider')
  }
  
  return context
}
```

**Ventajas**:
- ✅ Simplifica el uso del contexto
- ✅ Validación de uso correcto
- ✅ Mejor experiencia de desarrollo
- ✅ Fácil de testear

### Integración con localStorage

```jsx
function CartProvider({ children }) {
  // Cargar estado inicial
  const [cart, setCart] = useState(() => {
    const saved = localStorage.getItem('cart')
    return saved ? JSON.parse(saved) : []
  })
  
  // Guardar cuando cambia
  useEffect(() => {
    localStorage.setItem('cart', JSON.stringify(cart))
  }, [cart])
  
  const addToCart = (product) => {
    setCart(prev => [...prev, product])
  }
  
  return (
    <CartContext.Provider value={{ cart, addToCart }}>
      {children}
    </CartContext.Provider>
  )
}
```

### Ejemplo Completo: CartContext

```jsx
import { createContext, useContext, useState, useEffect } from 'react'

const CartContext = createContext()

export function CartProvider({ children }) {
  const [cart, setCart] = useState(() => {
    const saved = localStorage.getItem('cart')
    return saved ? JSON.parse(saved) : []
  })
  
  useEffect(() => {
    localStorage.setItem('cart', JSON.stringify(cart))
  }, [cart])
  
  const addToCart = (product) => {
    setCart(prev => [...prev, product])
  }
  
  const removeFromCart = (productId) => {
    setCart(prev => prev.filter(item => item.id !== productId))
  }
  
  const getCartTotal = () => {
    return cart.reduce((total, item) => total + item.price, 0)
  }
  
  const getCartItemsCount = () => {
    return cart.length
  }
  
  return (
    <CartContext.Provider value={{
      cart,
      addToCart,
      removeFromCart,
      getCartTotal,
      getCartItemsCount
    }}>
      {children}
    </CartContext.Provider>
  )
}

export function useCart() {
  const context = useContext(CartContext)
  
  if (!context) {
    throw new Error('useCart debe usarse dentro de CartProvider')
  }
  
  return context
}
```

### Usos Comunes de Context:

1. **Manejo de Autenticación**: Estado de login/logout, usuario actual
2. **Temas (Estilos)**: Tema claro/oscuro, estilos globales
3. **Manejo de Idiomas**: Idioma seleccionado, traducciones
4. **Carrito de Compras**: Productos en el carrito, totales
5. **Notificaciones Globales**: Mensajes de éxito/error
6. **Configuración Global**: URLs de API, configuraciones

---

## 3. useReducer

### ¿Qué es useReducer?

`useReducer` es un hook de React que permite manejar estado complejo usando un patrón similar a Redux. Es una alternativa a `useState` cuando el estado tiene lógica compleja.

### ¿Cuándo usar useReducer?

**Usar useReducer cuando**:
- ✅ Estado complejo con múltiples subvalores
- ✅ Lógica de actualización compleja
- ✅ Múltiples formas de actualizar el mismo estado
- ✅ Preferencia por un patrón similar a Redux
- ✅ Estado depende del estado anterior

**NO usar useReducer cuando**:
- ❌ Estado simple (número, string, booleano)
- ❌ Actualizaciones independientes
- ❌ Lógica simple y directa

### Sintaxis:

```jsx
const [state, dispatch] = useReducer(reducer, initialState)
```

### Estructura del Reducer:

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 }
    case 'DECREMENT':
      return { count: state.count - 1 }
    case 'RESET':
      return { count: 0 }
    case 'SET_VALUE':
      return { count: action.payload }
    default:
      return state
  }
}
```

### Ejemplo Completo:

```jsx
import { useReducer } from 'react'

const initialState = { count: 0 }

const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 }
    case 'DECREMENT':
      return { count: state.count - 1 }
    case 'RESET':
      return initialState
    case 'SET_VALUE':
      return { count: action.payload }
    default:
      return state
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState)
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>
        Incrementar
      </button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>
        Decrementar
      </button>
      <button onClick={() => dispatch({ type: 'RESET' })}>
        Reset
      </button>
      <button onClick={() => dispatch({ type: 'SET_VALUE', payload: 10 })}>
        Establecer 10
      </button>
    </div>
  )
}
```

### useReducer con Estado Complejo:

```jsx
const initialState = {
  items: [],
  loading: false,
  error: null
}

const reducer = (state, action) => {
  switch (action.type) {
    case 'FETCH_START':
      return { ...state, loading: true, error: null }
    case 'FETCH_SUCCESS':
      return { ...state, loading: false, items: action.payload }
    case 'FETCH_ERROR':
      return { ...state, loading: false, error: action.payload }
    case 'ADD_ITEM':
      return { ...state, items: [...state.items, action.payload] }
    case 'REMOVE_ITEM':
      return {
        ...state,
        items: state.items.filter(item => item.id !== action.payload)
      }
    default:
      return state
  }
}

function TaskManager() {
  const [state, dispatch] = useReducer(reducer, initialState)
  
  const addTask = (task) => {
    dispatch({ type: 'ADD_ITEM', payload: task })
  }
  
  return (
    <div>
      {state.loading && <p>Cargando...</p>}
      {state.error && <p>Error: {state.error}</p>}
      {state.items.map(item => (
        <div key={item.id}>{item.text}</div>
      ))}
    </div>
  )
}
```

### Comparación: useState vs useReducer

| Aspecto | useState | useReducer |
|---------|----------|------------|
| **Complejidad del Estado** | Simple (número, string, booleano) | Complejo (objeto con múltiples propiedades) |
| **Actualizaciones** | Independientes | Dependen del estado anterior |
| **Sintaxis** | Simple y directa | Más declarativa (reducer + dispatch) |
| **Lógica de Estado** | En el componente | Centralizada en el reducer |
| **Legibilidad** | Fácil para casos simples | Mejor para casos complejos |
| **Escalabilidad** | Puede volverse complejo | Mejor para lógica compleja |
| **Performance** | Similar | Similar |

**Ejemplo de useState complejo**:
```jsx
// useState puede volverse complejo
const [count, setCount] = useState(0)
const [loading, setLoading] = useState(false)
const [error, setError] = useState(null)

// Múltiples funciones de actualización
const increment = () => setCount(prev => prev + 1)
const decrement = () => setCount(prev => prev - 1)
const reset = () => setCount(0)
const setLoadingTrue = () => setLoading(true)
// ... más funciones
```

**Mismo ejemplo con useReducer**:
```jsx
// useReducer centraliza la lógica
const [state, dispatch] = useReducer(reducer, initialState)

// Una sola función dispatch para todas las acciones
dispatch({ type: 'INCREMENT' })
dispatch({ type: 'DECREMENT' })
dispatch({ type: 'RESET' })
dispatch({ type: 'SET_LOADING', payload: true })
```

---

## 4. Redux / Redux Toolkit

### ¿Qué es Redux?

**Redux** es una biblioteca de gestión de estado global para aplicaciones JavaScript. Permite:
- **Estado centralizado**: Todo el estado en un solo lugar (store)
- **Flujo predecible**: Cambios de estado a través de acciones
- **Debugging**: Herramientas como Redux DevTools
- **Escalabilidad**: Fácil de mantener en aplicaciones grandes

### ¿Cuándo usar Redux?

**Usar Redux cuando**:
- ✅ Estado compartido entre muchos componentes
- ✅ Estado complejo que necesita organización
- ✅ Necesitas time-travel debugging
- ✅ Aplicación grande con múltiples desarrolladores
- ✅ Estado global que cambia frecuentemente

**NO usar Redux cuando**:
- ❌ Aplicación pequeña
- ❌ Estado local es suficiente
- ❌ Solo necesitas estado de formularios
- ❌ Context API es suficiente

### Redux Toolkit

**Redux Toolkit (RTK)** es la forma moderna y recomendada de usar Redux. Simplifica:
- Configuración del store
- Creación de reducers
- Generación de acciones
- Código boilerplate

### Conceptos Clave de Redux:

#### 1. Store

El **Store** es el lugar único donde se almacena el estado global:

```jsx
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from './features/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

#### 2. Slice

Un **Slice** agrupa estado inicial, reducers y acciones:

```jsx
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    reset: (state) => {
      state.value = 0
    }
  }
})

export const { increment, decrement, reset } = counterSlice.actions
export default counterSlice.reducer
```

**Características**:
- `name`: Nombre del slice
- `initialState`: Estado inicial
- `reducers`: Funciones que modifican el estado
- Redux Toolkit usa Immer, permite mutaciones "aparentes"

#### 3. Provider

El **Provider** envuelve la aplicación y hace el store disponible:

```jsx
import { Provider } from 'react-redux'
import { store } from './app/store'

createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
)
```

#### 4. useSelector

`useSelector` lee el estado del store:

```jsx
import { useSelector } from 'react-redux'

function Component() {
  const count = useSelector((state) => state.counter.value)
  
  return <div>{count}</div>
}
```

**Con selector**:
```jsx
// En el slice
export const selectCount = (state) => state.counter.value

// En el componente
const count = useSelector(selectCount)
```

#### 5. useDispatch

`useDispatch` despacha acciones al store:

```jsx
import { useDispatch } from 'react-redux'
import { increment } from './features/counterSlice'

function Component() {
  const dispatch = useDispatch()
  
  return (
    <button onClick={() => dispatch(increment())}>
      Incrementar
    </button>
  )
}
```

#### 6. Acciones con Payload

```jsx
// En el slice
reducers: {
  addTask: (state, action) => {
    state.push({
      id: Date.now(),
      text: action.payload,
      completed: false
    })
  }
}

// En el componente
dispatch(addTask('Nueva tarea'))
```

### Flujo de Redux (Unidireccional):

```
1. Componente → dispatch(action)
2. Store recibe la acción
3. Store ejecuta el reducer correspondiente
4. Reducer calcula el nuevo estado
5. Store actualiza el estado
6. Componentes suscritos se re-renderizan
```

### Integración con localStorage:

```jsx
// Cargar estado inicial
const loadState = () => {
  try {
    const serializedState = localStorage.getItem('tasks')
    if (serializedState === null) return undefined
    return JSON.parse(serializedState)
  } catch (error) {
    return undefined
  }
}

// Guardar estado
const saveState = (state) => {
  try {
    const serializedState = JSON.stringify(state)
    localStorage.setItem('tasks', serializedState)
  } catch (error) {
    // Ignorar errores
  }
}

// PreloadedState
export const store = configureStore({
  reducer: {
    tasks: taskReducer
  },
  preloadedState: {
    tasks: loadState()
  }
})

// Suscribirse a cambios
store.subscribe(() => {
  saveState(store.getState().tasks)
})
```

### Redux DevTools

**Redux DevTools** es una extensión del navegador que permite:
- Ver el estado actual
- Ver historial de acciones
- Time-travel debugging
- Inspeccionar cambios de estado

**Instalación**:
1. Instalar extensión en Chrome/Firefox: "Redux DevTools"
2. El código ya está configurado para usarla automáticamente

---

## 5. Comparación de Soluciones

### Tabla Comparativa:

| Aspecto | useState | Context API | useReducer | Redux |
|---------|----------|-------------|------------|-------|
| **Complejidad** | Simple | Moderada | Moderada | Alta |
| **Estado** | Local | Global compartido | Local complejo | Global complejo |
| **Prop Drilling** | Sí (múltiples niveles) | No | Sí (múltiples niveles) | No |
| **Boilerplate** | Mínimo | Moderado | Moderado | Alto |
| **Debugging** | Básico | Básico | Básico | Avanzado (DevTools) |
| **Escalabilidad** | Baja | Media | Media | Alta |
| **Curva de Aprendizaje** | Baja | Media | Media | Alta |
| **Tamaño Bundle** | 0 KB | 0 KB | 0 KB | ~15 KB |
| **Time-travel** | No | No | No | Sí |
| **Middleware** | No | No | No | Sí |

### Comparación Detallada:

#### useState vs Context API

**useState**:
- ✅ Simple y directo
- ✅ Ideal para estado local
- ❌ Prop drilling con muchos niveles

**Context API**:
- ✅ Solución nativa de React
- ✅ Ideal para estado compartido moderado
- ✅ Menos código que Redux
- ❌ Puede causar re-renders innecesarios
- ❌ No tan bueno para estado muy complejo

#### Context API vs Redux

**Context API**:
- ✅ Solución nativa (sin dependencias)
- ✅ Menos boilerplate
- ✅ Ideal para estado compartido moderado
- ❌ Puede volverse complicado en aplicaciones grandes
- ❌ No tiene DevTools avanzadas

**Redux**:
- ✅ Ideal para estado muy complejo
- ✅ Herramientas de debugging (DevTools)
- ✅ Escalable para aplicaciones grandes
- ✅ Time-travel debugging
- ✅ Middleware para lógica asíncrona
- ❌ Más código y configuración
- ❌ Curva de aprendizaje más alta
- ❌ Dependencia externa

#### useState vs useReducer

**useState**:
- ✅ Simple y directo
- ✅ Ideal para estado simple
- ✅ Menos código
- ❌ Puede volverse complejo con múltiples estados relacionados

**useReducer**:
- ✅ Ideal para estado complejo
- ✅ Lógica de actualización centralizada
- ✅ Fácil de testear
- ✅ Patrón similar a Redux
- ❌ Más código para casos simples
- ❌ Curva de aprendizaje más alta

---

## 6. Ejemplos Prácticos del Código Modelo

### Ejemplo 01: Context API - Tema Claro/Oscuro

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-23-react-context-api/ejemplo-01-tema-claro-oscuro`

**Conceptos cubiertos**:
- ✅ `createContext` para crear contexto
- ✅ Provider para compartir estado
- ✅ `useContext` para consumir contexto
- ✅ Estado compartido entre componentes
- ✅ Función para modificar estado desde contexto

**Código del Context**:
```jsx
import { createContext, useState } from 'react'

const ThemeContext = createContext()

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light')
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export default ThemeContext
```

### Ejemplo 02: Context API - CartContext Completo

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-23-react-context-api/ejemplo-02-cart-context`

**Conceptos cubiertos**:
- ✅ Context complejo con múltiples funciones
- ✅ Hook personalizado (`useCart`) para simplificar uso
- ✅ Integración con localStorage
- ✅ Validación de errores
- ✅ Funciones complejas (addToCart, removeFromCart, updateQuantity)
- ✅ Cálculos derivados (getCartTotal, getCartItemsCount)

### Ejemplo 03: useReducer - Contador

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle/ejemplo-01-hooks-lifecycle-reducer`

**Conceptos cubiertos**:
- ✅ `useReducer` para estado complejo
- ✅ Reducer con múltiples acciones
- ✅ Dispatch para enviar acciones
- ✅ Comparación con useState

**Código del Reducer**:
```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 }
    case 'DECREMENT':
      return { count: state.count - 1 }
    case 'RESET':
      return { count: 0 }
    default:
      return state
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 })
```

### Ejemplo 04: Redux - Contador Simple

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-20-react-redux/ejemplo-01-contador-simple`

**Conceptos cubiertos**:
- ✅ Configuración básica de Redux Toolkit
- ✅ Crear un slice con `createSlice`
- ✅ Definir acciones (increment, decrement, reset)
- ✅ Configurar el store con `configureStore`
- ✅ Usar `Provider` para envolver la aplicación
- ✅ `useSelector` para leer estado
- ✅ `useDispatch` para despachar acciones

**Código del Slice**:
```jsx
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    reset: (state) => {
      state.value = 0
    }
  }
})

export const { increment, decrement, reset } = counterSlice.actions
export default counterSlice.reducer
```

### Ejemplo 05: Redux - Task Manager Completo

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-20-react-redux/ejemplo-02-task-manager-completo`

**Conceptos cubiertos**:
- ✅ Slice complejo con múltiples acciones
- ✅ Estado como array (no solo objeto simple)
- ✅ Integración con localStorage
- ✅ PreloadedState desde localStorage
- ✅ Múltiples componentes usando Redux
- ✅ Acciones con payload
- ✅ Reducers que modifican arrays

**Código del Slice Completo**:
```jsx
import { createSlice } from '@reduxjs/toolkit'

const taskSlice = createSlice({
  name: 'tasks',
  initialState: [],
  reducers: {
    addTask: (state, action) => {
      state.push({
        id: Date.now(),
        text: action.payload,
        completed: false
      })
    },
    toggleComplete: (state, action) => {
      const task = state.find(task => task.id === action.payload)
      if (task) {
        task.completed = !task.completed
      }
    },
    deleteTask: (state, action) => {
      return state.filter(task => task.id !== action.payload)
    }
  }
})

export const { addTask, toggleComplete, deleteTask } = taskSlice.actions
export default taskSlice.reducer
```

---

## 7. Cuándo Usar Cada Solución

### useState

**Usar cuando**:
- ✅ Estado local del componente
- ✅ Estado simple (número, string, booleano)
- ✅ No necesitas compartir entre componentes
- ✅ Actualizaciones independientes

**Ejemplo**:
```jsx
function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

### Context API

**Usar cuando**:
- ✅ Estado compartido entre muchos componentes
- ✅ Evitar prop drilling (3+ niveles)
- ✅ Estado de autenticación, tema, carrito
- ✅ Aplicación pequeña/mediana

**Ejemplo**:
```jsx
// Tema, autenticación, carrito de compras
<ThemeProvider>
  <App />
</ThemeProvider>
```

### useReducer

**Usar cuando**:
- ✅ Estado complejo con múltiples subvalores
- ✅ Lógica de actualización compleja
- ✅ Múltiples formas de actualizar el mismo estado
- ✅ Preferencia por patrón similar a Redux

**Ejemplo**:
```jsx
// Carrito de compras, formulario complejo, estado con múltiples propiedades
const [state, dispatch] = useReducer(cartReducer, initialState)
```

### Redux

**Usar cuando**:
- ✅ Estado global muy complejo
- ✅ Aplicación grande con múltiples desarrolladores
- ✅ Necesitas time-travel debugging
- ✅ Necesitas middleware para lógica asíncrona
- ✅ Context API no es suficiente

**Ejemplo**:
```jsx
// E-commerce grande, aplicación con múltiples módulos, estado muy complejo
<Provider store={store}>
  <App />
</Provider>
```

### Resumen de Decisión:

```
¿Es estado local?
  Sí → useState
  No → ¿Necesitas compartir entre componentes?
    Sí → ¿Aplicación pequeña/mediana?
      Sí → Context API
      No → ¿Estado muy complejo?
        Sí → Redux
        No → Context API
    No → ¿Estado complejo?
      Sí → useReducer
      No → useState
```

---

## 📚 Índice por Temas del Código Modelo

### Tema 19: React - State, Hooks y Lifecycle
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle`

**Conceptos cubiertos**:
- ✅ `useState` con diferentes tipos
- ✅ `useEffect` con diferentes dependencias
- ✅ Hooks personalizados
- ✅ `useReducer` para estado complejo
- ✅ Lifecycle de componentes

### Tema 20: React - Redux
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-20-react-redux`

**Conceptos cubiertos**:
- ✅ Configuración de Redux Toolkit
- ✅ Crear slices con `createSlice`
- ✅ Usar `useSelector` y `useDispatch`
- ✅ Integración con localStorage
- ✅ Flujo unidireccional de Redux

### Tema 23: React - Context API
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-23-react-context-api`

**Conceptos cubiertos**:
- ✅ `createContext` para crear contexto
- ✅ Provider para compartir estado
- ✅ `useContext` para consumir contexto
- ✅ Hooks personalizados para contexto
- ✅ Integración con localStorage

---

## 🎯 Resumen de Conceptos Clave

### Context API
- `createContext`: Crea el contexto
- `Provider`: Envuelve componentes y provee valor
- `useContext`: Consume el contexto
- Hook personalizado: Simplifica uso y valida

### useReducer
- `useReducer`: Hook para estado complejo
- `reducer`: Función que calcula nuevo estado
- `dispatch`: Función para enviar acciones
- `action`: Objeto con `type` y `payload`

### Redux
- `Store`: Estado global centralizado
- `Slice`: Agrupa estado, reducers y acciones
- `Provider`: Envuelve aplicación
- `useSelector`: Lee estado
- `useDispatch`: Despacha acciones

---

## 📝 Buenas Prácticas

1. **No usar Redux para todo**: Solo cuando realmente lo necesites
2. **Context para estado compartido moderado**: Ideal para tema, autenticación
3. **useReducer para estado complejo local**: Cuando useState se vuelve complicado
4. **Hooks personalizados**: Crear hooks para simplificar uso de Context
5. **Validar uso correcto**: Lanzar error si se usa fuera del Provider
6. **Inmutabilidad**: Nunca mutar estado directamente
7. **Selectores**: Usar selectores en Redux para mejor rendimiento
8. **Limpiar recursos**: Limpiar suscripciones y timers en useEffect

---

## 🚀 Próximos Pasos

Después de dominar estos conceptos, continúa con:
- **React Fase 5**: Proyecto E-commerce (aplicación completa)

---

**Referencias del Código Modelo**:
- `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle/`
- `cursadas/frontend/frontEnd_modelo/tema-20-react-redux/`
- `cursadas/frontend/frontEnd_modelo/tema-23-react-context-api/`
