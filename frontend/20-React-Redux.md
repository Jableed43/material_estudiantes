# React: Redux y Estado Global 🔄

## Introducción a Redux

Redux es una biblioteca para manejar el estado global de aplicaciones JavaScript. Es especialmente útil en aplicaciones React grandes.

### ¿Qué es Redux?

Redux es un contenedor de estado predecible que ayuda a escribir aplicaciones que se comportan de manera consistente.

**Características**:
- ✅ Estado global centralizado
- ✅ Flujo de datos unidireccional
- ✅ Predecible y fácil de depurar
- ✅ Time-travel debugging

### ¿Cuándo usar Redux?

**Usar Redux cuando**:
- ✅ Estado compartido entre muchos componentes
- ✅ Estado complejo que cambia frecuentemente
- ✅ Necesitas time-travel debugging
- ✅ Aplicación grande con múltiples desarrolladores

**NO usar Redux cuando**:
- ❌ Estado local es suficiente
- ❌ Aplicación pequeña
- ❌ Estado simple

---

## Conceptos Clave de Redux

### Store

El **Store** es el objeto que contiene el estado global de la aplicación.

```javascript
import { createStore } from 'redux'

const store = createStore(reducer)
```

### Actions

Las **Actions** son objetos que describen qué pasó.

```javascript
// Action
{
  type: 'INCREMENT',
  payload: 1
}

// Action Creator
const incrementar = (cantidad) => ({
  type: 'INCREMENT',
  payload: cantidad
})
```

### Reducers

Los **Reducers** son funciones puras que especifican cómo el estado cambia en respuesta a las actions.

```javascript
const initialState = { count: 0 }

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + action.payload }
    case 'DECREMENT':
      return { ...state, count: state.count - action.payload }
    default:
      return state
  }
}
```

### Dispatch

**Dispatch** es el método para enviar actions al store.

```javascript
store.dispatch(incrementar(5))
```

---

## Redux con React

### Instalación

```bash
npm install redux react-redux
```

### Configuración Básica

```jsx
// store.js
import { createStore } from 'redux'
import counterReducer from './reducers/counterReducer'

const store = createStore(counterReducer)

export default store

// App.jsx
import { Provider } from 'react-redux'
import store from './store'

function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  )
}
```

### useSelector y useDispatch

```jsx
import { useSelector, useDispatch } from 'react-redux'

function Counter() {
  const count = useSelector(state => state.count)
  const dispatch = useDispatch()
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>
        Incrementar
      </button>
    </div>
  )
}
```

---

## Redux Toolkit (RTK)

Redux Toolkit es la forma recomendada de escribir Redux moderno.

### Instalación

```bash
npm install @reduxjs/toolkit react-redux
```

### Configuración con RTK

```javascript
// store.js
import { configureStore } from '@reduxjs/toolkit'
import counterSlice from './slices/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterSlice
  }
})

// slices/counterSlice.js
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
    }
  }
})

export const { increment, decrement } = counterSlice.actions
export default counterSlice.reducer
```

### Uso en Componentes

```jsx
import { useSelector, useDispatch } from 'react-redux'
import { increment, decrement } from './slices/counterSlice'

function Counter() {
  const count = useSelector(state => state.counter.value)
  const dispatch = useDispatch()
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  )
}
```

---

## Ejemplos Prácticos

### Ejemplo 1: Store Completo

```javascript
// store.js
import { configureStore } from '@reduxjs/toolkit'
import userSlice from './slices/userSlice'
import productSlice from './slices/productSlice'

export const store = configureStore({
  reducer: {
    user: userSlice,
    products: productSlice
  }
})
```

### Ejemplo 2: Slice con Payload

```javascript
// slices/userSlice.js
import { createSlice } from '@reduxjs/toolkit'

const userSlice = createSlice({
  name: 'user',
  initialState: { nombre: '', email: '' },
  reducers: {
    setUser: (state, action) => {
      state.nombre = action.payload.nombre
      state.email = action.payload.email
    },
    clearUser: (state) => {
      state.nombre = ''
      state.email = ''
    }
  }
})

export const { setUser, clearUser } = userSlice.actions
export default userSlice.reducer
```

---

## Conceptos Clave

1. **Store**: Contenedor del estado global
2. **Actions**: Objetos que describen cambios
3. **Reducers**: Funciones que actualizan el estado
4. **Dispatch**: Método para enviar actions
5. **Provider**: Componente que provee el store
6. **useSelector**: Hook para leer el estado
7. **useDispatch**: Hook para enviar actions

---

## Buenas Prácticas

- Usa Redux Toolkit para proyectos nuevos
- Mantén los reducers puros (sin side effects)
- Usa slices para organizar el código
- No mutes el estado directamente (RTK lo permite con Immer)
- Separa la lógica de negocio de los componentes
- Usa selectores para acceder al estado
- Considera si realmente necesitas Redux antes de usarlo

