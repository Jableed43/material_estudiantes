# React Fase 2: Hooks y Estado ⚓

## 📋 Índice

1. [Introducción a los Hooks](#1-introducción-a-los-hooks)
2. [useState Avanzado](#2-usestate-avanzado)
3. [useEffect: Efectos Secundarios](#3-useeffect-efectos-secundarios)
4. [Ciclo de Vida con Hooks](#4-ciclo-de-vida-con-hooks)
5. [useReducer](#5-usereducer)
6. [Hooks Personalizados](#6-hooks-personalizados)
7. [Renderizado de Listas con Estado](#7-renderizado-de-listas-con-estado)
8. [Persistencia con localStorage](#8-persistencia-con-localstorage)
9. [Comparaciones: useState vs useReducer](#9-comparaciones-usestate-vs-usereducer)
10. [Ejemplos Prácticos del Código Modelo](#10-ejemplos-prácticos-del-código-modelo)

---

## 1. Introducción a los Hooks

### ¿Qué son los Hooks? (Analogía del Mundo Real)

### ⚓ Analogía: El Gancho de Pesca

Imagina que pescas:
- **Hook (Gancho)**: Te "enganchas" a algo (el pez, en este caso las funcionalidades de React)
- **Funcionalidad**: Como el pez que quieres atrapar (estado, ciclo de vida)
- **Componente**: Como la caña de pescar (tu componente funcional)

**Los Hooks te permiten "engancharte"** a funcionalidades de React desde componentes funcionales.

### 🔌 Analogía: El Enchufe Eléctrico

Piensa en un enchufe:
- **Hook**: Como el enchufe que te conecta a la electricidad
- **Funcionalidad**: Como la electricidad (estado, efectos)
- **Componente**: Como el aparato que necesita electricidad

**Los Hooks "conectan" tu componente** a las funcionalidades de React.

### 🎣 Analogía: La Caña de Pescar

Una caña de pescar:
- **Hook**: El gancho que atrapa el pez
- **Funcionalidad**: El pez (estado, ciclo de vida)
- **Componente**: La caña completa

**Antes de los Hooks**: Solo podías "pescar" con cañas especiales (componentes de clase).
**Con los Hooks**: Puedes "pescar" con cualquier caña (componentes funcionales).

### ¿Qué son los Hooks?

Los **Hooks** son funciones especiales que permiten "engancharse" a las funcionalidades de React (como el estado y el ciclo de vida) desde componentes funcionales. Antes de los Hooks, estas funcionalidades solo estaban disponibles en componentes de clase.

**En términos simples**: Los Hooks son como herramientas que te permiten usar las funcionalidades avanzadas de React en componentes funcionales simples.

### Reglas de los Hooks:

1. ✅ **Solo llamar Hooks en el nivel superior**: No dentro de loops, condiciones o funciones anidadas
2. ✅ **Solo llamar Hooks desde componentes funcionales**: No desde funciones JavaScript regulares
3. ✅ **Solo llamar Hooks desde componentes de React**: No desde funciones utilitarias

### Hooks Principales:

- **`useState`**: Manejar estado local
- **`useEffect`**: Manejar efectos secundarios y ciclo de vida
- **`useReducer`**: Manejar estado complejo
- **`useCallback`**: Memorizar funciones
- **`useMemo`**: Memorizar valores calculados
- **Hooks personalizados**: Crear tus propios Hooks

---

## 2. useState Avanzado

### useState con Diferentes Tipos de Datos

#### Estado Numérico

```jsx
const [count, setCount] = useState(0)

// Actualización directa
setCount(5)

// Actualización con callback (recomendado cuando depende del valor anterior)
setCount(prevCount => prevCount + 1)
```

#### Estado Booleano

```jsx
const [activo, setActivo] = useState(false)

// Toggle
setActivo(!activo)

// Toggle con callback (recomendado)
setActivo(prev => !prev)
```

#### Estado como Objeto

```jsx
const [usuario, setUsuario] = useState({ 
  nombre: "Juan", 
  edad: 25 
})

// ❌ INCORRECTO: Mutación directa
usuario.edad = 26  // NO hacer esto

// ✅ CORRECTO: Spread operator
setUsuario(prev => ({
  ...prev,
  edad: prev.edad + 1
}))

// ✅ CORRECTO: Actualización completa
setUsuario({
  nombre: "Juan",
  edad: 26
})
```

#### Estado como Array

```jsx
const [items, setItems] = useState([])

// Agregar elemento
setItems(prev => [...prev, nuevoItem])

// Eliminar elemento
setItems(prev => prev.filter(item => item.id !== id))

// Actualizar elemento
setItems(prev => prev.map(item => 
  item.id === id ? { ...item, ...cambios } : item
))

// Actualizar elemento específico por índice
setItems(prev => prev.map((item, index) => 
  index === indice ? { ...item, ...cambios } : item
))
```

### Actualización de Estado con Función Callback

**¿Cuándo usar función callback?**

Usa función callback cuando el nuevo valor **depende del valor anterior**:

```jsx
// ✅ CORRECTO: Cuando depende del anterior
setCount(prev => prev + 1)

// ✅ CORRECTO: Cuando no depende del anterior
setCount(5)

// ⚠️ PROBLEMA: Múltiples actualizaciones seguidas
setCount(count + 1)  // Usa el valor "antiguo" de count
setCount(count + 1)  // Usa el mismo valor "antiguo" de count
// Resultado: Solo se incrementa una vez

// ✅ SOLUCIÓN: Usar función callback
setCount(prev => prev + 1)  // Usa el valor más reciente
setCount(prev => prev + 1)  // Usa el valor más reciente
// Resultado: Se incrementa dos veces
```

### Actualización Dinámica de Propiedades de Objeto

```jsx
const [usuario, setUsuario] = useState({
  nombre: '',
  email: '',
  edad: 0
})

// Actualizar propiedad específica usando [key]
const handleChange = (key, value) => {
  setUsuario(prev => ({
    ...prev,
    [key]: value  // Actualización dinámica
  }))
}

// Uso
handleChange('nombre', 'Juan')
handleChange('email', 'juan@ejemplo.com')
```

### Múltiples Estados vs Estado Único

**Múltiples Estados** (Recomendado para valores independientes):
```jsx
const [nombre, setNombre] = useState('')
const [email, setEmail] = useState('')
const [edad, setEdad] = useState(0)
```

**Estado Único** (Recomendado para valores relacionados):
```jsx
const [formData, setFormData] = useState({
  nombre: '',
  email: '',
  edad: 0
})
```

---

## 3. useEffect: Efectos Secundarios

### ¿Qué es useEffect?

El hook `useEffect` se utiliza para realizar **efectos secundarios** en los componentes funcionales. Un efecto secundario puede ser cualquier cosa que afecte al mundo exterior, como:
- Hacer solicitudes a una API
- Suscribirse o cancelar suscripciones
- Actualizar el DOM
- Manipular `localStorage`
- Configurar timers o intervalos

### Sintaxis de useEffect:

```jsx
useEffect(() => {
  // Código del efecto
  return () => {
    // Función de limpieza (opcional)
  }
}, [dependencias])  // Array de dependencias
```

### Tres Casos de Uso de useEffect:

#### 1. Sin Dependencias (Solo Montaje)

Se ejecuta **solo una vez** cuando el componente se monta:

```jsx
useEffect(() => {
  console.log('Componente montado')
  // Cargar datos iniciales
  const datos = localStorage.getItem('datos')
  if (datos) {
    setDatos(JSON.parse(datos))
  }
}, [])  // Array vacío = solo al montar
```

**Casos de uso**:
- ✅ Cargar datos desde una API al iniciar
- ✅ Cargar datos desde `localStorage` al iniciar
- ✅ Configurar suscripciones iniciales
- ✅ Inicializar librerías externas

#### 2. Con Dependencias (Montaje + Actualización)

Se ejecuta cuando el componente se monta **Y** cada vez que las dependencias cambian:

```jsx
useEffect(() => {
  console.log('Contador cambió:', count)
  localStorage.setItem('count', count.toString())
}, [count])  // Se ejecuta cuando count cambia
```

**Casos de uso**:
- ✅ Guardar datos en `localStorage` cuando cambian
- ✅ Hacer peticiones a API cuando cambian ciertos valores
- ✅ Actualizar el título del documento
- ✅ Sincronizar estado con props

#### 3. Con Limpieza (Cleanup)

Se ejecuta el efecto y, antes de que se vuelva a ejecutar o el componente se desmonte, ejecuta la función de limpieza:

```jsx
useEffect(() => {
  console.log('Iniciando timer...')
  
  const timer = setInterval(() => {
    console.log('Timer ejecutándose...')
    setCount(prev => prev + 1)
  }, 1000)
  
  // Función de limpieza
  return () => {
    console.log('Limpiando timer...')
    clearInterval(timer)  // Limpieza al desmontar o cambiar dependencias
  }
}, [activo])  // Se ejecuta cuando activo cambia
```

**Casos de uso**:
- ✅ Limpiar timers (`setInterval`, `setTimeout`)
- ✅ Cancelar suscripciones
- ✅ Limpiar event listeners
- ✅ Cancelar peticiones HTTP pendientes

### Comparación: useEffect vs Lifecycle Methods

| Lifecycle Method (Clases) | useEffect (Hooks) |
|:---|:---|
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [deps])` |
| `componentWillUnmount` | `return () => {}` dentro de `useEffect` |

**Ejemplo Comparativo**:

```jsx
// Con Hooks (Funcional)
function Componente() {
  useEffect(() => {
    // Lógica de montaje
    console.log('Componente montado')
    
    return () => {
      // Lógica de desmontaje
      console.log('Componente desmontado')
    }
  }, [])
}

// Con Clases (Antiguo)
class Componente extends React.Component {
  componentDidMount() {
    // Lógica de montaje
    console.log('Componente montado')
  }
  
  componentWillUnmount() {
    // Lógica de desmontaje
    console.log('Componente desmontado')
  }
}
```

### Errores Comunes con useEffect:

#### 1. Olvidar Dependencias

```jsx
// ⚠️ PROBLEMA: Falta count en dependencias
useEffect(() => {
  console.log(count)
}, [])  // count no está en dependencias

// ✅ SOLUCIÓN: Incluir todas las dependencias
useEffect(() => {
  console.log(count)
}, [count])
```

#### 2. Loop Infinito

```jsx
// ⚠️ PROBLEMA: count cambia, efecto se ejecuta, count cambia de nuevo...
useEffect(() => {
  setCount(count + 1)  // Esto causa un loop infinito
}, [count])

// ✅ SOLUCIÓN: Solo actualizar cuando sea necesario
useEffect(() => {
  // Lógica que no actualiza count directamente
}, [count])
```

#### 3. Olvidar Limpieza

```jsx
// ⚠️ PROBLEMA: Timer no se limpia, causa memory leak
useEffect(() => {
  const timer = setInterval(() => {
    setCount(prev => prev + 1)
  }, 1000)
  // Falta return con clearInterval
}, [])

// ✅ SOLUCIÓN: Siempre limpiar recursos
useEffect(() => {
  const timer = setInterval(() => {
    setCount(prev => prev + 1)
  }, 1000)
  
  return () => {
    clearInterval(timer)  // Limpieza
  }
}, [])
```

---

## 4. Ciclo de Vida con Hooks

### Fases del Ciclo de Vida:

#### 1. Montaje (Mounting)

El componente se crea y se inserta en el DOM:

```jsx
useEffect(() => {
  console.log('Componente montado')
  // Cargar datos iniciales
}, [])  // Array vacío = solo al montar
```

#### 2. Actualización (Updating)

El componente se actualiza cuando cambian props o estado:

```jsx
useEffect(() => {
  console.log('Componente actualizado')
  // Reaccionar a cambios
}, [dependencia])  // Se ejecuta cuando dependencia cambia
```

#### 3. Desmontaje (Unmounting)

El componente se elimina del DOM:

```jsx
useEffect(() => {
  // Lógica del efecto
  
  return () => {
    console.log('Componente desmontado')
    // Limpieza de recursos
  }
}, [])
```

### Ejemplo Completo del Ciclo de Vida:

```jsx
function LifecycleExample() {
  const [count, setCount] = useState(0)
  const [activo, setActivo] = useState(true)
  
  // 1. Montaje: Solo una vez
  useEffect(() => {
    console.log('1. Componente montado')
    return () => {
      console.log('4. Componente desmontado')
    }
  }, [])
  
  // 2. Actualización: Cuando count cambia
  useEffect(() => {
    console.log('2. Count cambió:', count)
  }, [count])
  
  // 3. Actualización con limpieza: Cuando activo cambia
  useEffect(() => {
    if (!activo) return
    
    console.log('3. Timer iniciado')
    const timer = setInterval(() => {
      setCount(prev => prev + 1)
    }, 1000)
    
    return () => {
      console.log('3. Timer limpiado')
      clearInterval(timer)
    }
  }, [activo])
  
  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={() => setActivo(!activo)}>
        {activo ? 'Pausar' : 'Iniciar'}
      </button>
    </div>
  )
}
```

---

## 5. useReducer

### ¿Qué es useReducer?

`useReducer` es una alternativa a `useState` para manejar estado complejo. Se basa en una función **reducer** y el envío de **acciones** (`dispatch`).

### ¿Cuándo usar useReducer?

**Usar useReducer cuando**:
- ✅ Estado complejo con múltiples subvalores
- ✅ Lógica de actualización compleja
- ✅ Múltiples formas de actualizar el mismo estado
- ✅ Preferencia por un patrón similar a Redux

**Usar useState cuando**:
- ✅ Estado simple (número, string, booleano)
- ✅ Actualizaciones directas
- ✅ Menos código

### Sintaxis de useReducer:

```jsx
const [state, dispatch] = useReducer(reducer, initialState)
```

- **`state`**: El estado actual
- **`dispatch`**: Función para enviar acciones
- **`reducer`**: Función que especifica cómo cambia el estado
- **`initialState`**: Estado inicial

### Estructura del Reducer:

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'ACCION_1':
      return { ...state, /* cambios */ }
    case 'ACCION_2':
      return { ...state, /* cambios */ }
    default:
      return state
  }
}
```

### Ejemplo Básico:

```jsx
import { useReducer } from 'react'

// Reducer: Especifica cómo cambia el estado
const counterReducer = (state, action) => {
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

function Counter() {
  // useReducer: Estado y función dispatch
  const [state, dispatch] = useReducer(counterReducer, { count: 0 })
  
  return (
    <div>
      <p>Contador: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>
        Incrementar
      </button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>
        Decrementar
      </button>
      <button onClick={() => dispatch({ type: 'RESET' })}>
        Reset
      </button>
    </div>
  )
}
```

### Acciones con Payload:

```jsx
const todoReducer = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, {
          id: Date.now(),
          text: action.payload,  // Payload contiene los datos
          completed: false
        }]
      }
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      }
    default:
      return state
  }
}

function TodoApp() {
  const [state, dispatch] = useReducer(todoReducer, { todos: [] })
  
  const addTodo = (text) => {
    dispatch({ type: 'ADD_TODO', payload: text })
  }
  
  const toggleTodo = (id) => {
    dispatch({ type: 'TOGGLE_TODO', payload: id })
  }
  
  return (
    // ... JSX
  )
}
```

### useReducer vs useState:

| Característica | useState | useReducer |
|:---|:---|:---|
| **Simplicidad** | ✅ Simple y directo | ❌ Más código |
| **Estado simple** | ✅ Ideal | ❌ Excesivo |
| **Estado complejo** | ❌ Puede volverse complicado | ✅ Ideal |
| **Lógica centralizada** | ❌ Dispersa | ✅ Centralizada en reducer |
| **Testabilidad** | ⚠️ Media | ✅ Fácil de testear |
| **Curva de aprendizaje** | ✅ Baja | ❌ Más alta |

---

## 6. Hooks Personalizados

### ¿Qué es un Hook Personalizado?

Un **hook personalizado** es una función JavaScript que:
- ✅ Comienza con `use` (convención)
- ✅ Puede llamar a otros hooks
- ✅ Permite reutilizar lógica de estado entre componentes

### ¿Por qué crear Hooks Personalizados?

- ✅ **Reutilización de lógica**: Una vez creado, se usa en múltiples componentes
- ✅ **Separación de responsabilidades**: Lógica separada de la presentación
- ✅ **Fácil de testear**: Lógica aislada
- ✅ **Código más limpio**: Componentes más simples y legibles

### Estructura de un Hook Personalizado:

```jsx
function useNombreDelHook(parametros) {
  // Puede usar otros hooks
  const [estado, setEstado] = useState(valorInicial)
  
  useEffect(() => {
    // Lógica del hook
  }, [dependencias])
  
  // Retorna valores y funciones
  return { estado, funcion }
}
```

### Ejemplo: Hook Personalizado `useCounter`

```jsx
import { useState, useEffect } from 'react'

function useCounter(inicial, paso) {
  const [count, setCount] = useState(inicial)
  
  const aumentar = () => {
    setCount(prev => prev + paso)
  }
  
  const disminuir = () => {
    setCount(prev => prev - paso)
  }
  
  const reset = () => {
    setCount(inicial)
  }
  
  useEffect(() => {
    console.log('Contador actualizado:', count)
  }, [count])
  
  return { count, aumentar, disminuir, reset }
}

// Uso del hook personalizado
function Componente() {
  const { count, aumentar, disminuir, reset } = useCounter(0, 1)
  
  return (
    <div>
      <p>Contador: {count}</p>
      <button onClick={aumentar}>Incrementar</button>
      <button onClick={disminuir}>Decrementar</button>
      <button onClick={reset}>Reset</button>
    </div>
  )
}
```

### Ejemplo: Hook Personalizado `useLocalStorage`

```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      return initialValue
    }
  })
  
  const setValue = (value) => {
    try {
      setStoredValue(value)
      window.localStorage.setItem(key, JSON.stringify(value))
    } catch (error) {
      console.error(error)
    }
  }
  
  return [storedValue, setValue]
}

// Uso
function Componente() {
  const [nombre, setNombre] = useLocalStorage('nombre', '')
  
  return (
    <input 
      value={nombre}
      onChange={(e) => setNombre(e.target.value)}
    />
  )
}
```

### Reglas para Hooks Personalizados:

1. ✅ **Siempre comenzar con `use`**: `useCounter`, `useLocalStorage`, etc.
2. ✅ **Seguir las reglas de los Hooks**: Solo llamar hooks en el nivel superior
3. ✅ **Retornar valores útiles**: Estado, funciones, o ambos
4. ✅ **Documentar el propósito**: Comentarios claros sobre qué hace el hook

---

## 7. Renderizado de Listas con Estado

### Estado con Arrays

```jsx
function ListaTareas() {
  const [tareas, setTareas] = useState([])
  
  const agregarTarea = (texto) => {
    const nuevaTarea = {
      id: Date.now(),
      texto: texto,
      completada: false
    }
    setTareas(prev => [...prev, nuevaTarea])
  }
  
  const toggleTarea = (id) => {
    setTareas(prev => prev.map(tarea =>
      tarea.id === id
        ? { ...tarea, completada: !tarea.completada }
        : tarea
    ))
  }
  
  const eliminarTarea = (id) => {
    setTareas(prev => prev.filter(tarea => tarea.id !== id))
  }
  
  return (
    <div>
      {tareas.map(tarea => (
        <div key={tarea.id}>
          <input
            type="checkbox"
            checked={tarea.completada}
            onChange={() => toggleTarea(tarea.id)}
          />
          <span>{tarea.texto}</span>
          <button onClick={() => eliminarTarea(tarea.id)}>Eliminar</button>
        </div>
      ))}
    </div>
  )
}
```

### Actualización de Arrays con Spread Operator

```jsx
// Agregar elemento
setItems(prev => [...prev, nuevoItem])

// Eliminar elemento
setItems(prev => prev.filter(item => item.id !== id))

// Actualizar elemento
setItems(prev => prev.map(item =>
  item.id === id ? { ...item, ...cambios } : item
))

// Reemplazar elemento
setItems(prev => prev.map((item, index) =>
  index === indice ? nuevoItem : item
))
```

### Keys en Listas

**Reglas importantes**:
- ✅ **Usar IDs únicos**: Cuando sea posible, usar IDs de la base de datos
- ✅ **Keys estables**: No deben cambiar entre renders
- ✅ **Keys únicas**: Entre hermanos (elementos del mismo nivel)
- ❌ **NO usar índices**: Si la lista puede cambiar de orden
- ❌ **NO usar valores aleatorios**: Como `Math.random()`

```jsx
// ✅ CORRECTO: Usar ID único
{tareas.map(tarea => (
  <TareaItem key={tarea.id} tarea={tarea} />
))}

// ⚠️ ACEPTABLE: Usar índice solo si la lista NO cambia
{items.map((item, index) => (
  <Item key={index} item={item} />
))}
```

---

## 8. Persistencia con localStorage

### Integración de localStorage con useState y useEffect

#### Cargar Datos al Iniciar:

```jsx
useEffect(() => {
  const guardado = localStorage.getItem('tareas')
  if (guardado) {
    setTareas(JSON.parse(guardado))
  }
}, [])  // Solo al montar
```

#### Guardar Datos al Cambiar:

```jsx
useEffect(() => {
  localStorage.setItem('tareas', JSON.stringify(tareas))
}, [tareas])  // Cuando tareas cambia
```

### Ejemplo Completo: Todo List con Persistencia

```jsx
function TodoList() {
  const [tareas, setTareas] = useState([])
  
  // 1. Cargar al inicio
  useEffect(() => {
    const guardado = localStorage.getItem('tareas')
    if (guardado) {
      try {
        setTareas(JSON.parse(guardado))
      } catch (error) {
        console.error('Error al cargar tareas:', error)
      }
    }
  }, [])
  
  // 2. Guardar al cambiar
  useEffect(() => {
    localStorage.setItem('tareas', JSON.stringify(tareas))
  }, [tareas])
  
  const agregarTarea = (texto) => {
    const nuevaTarea = {
      id: Date.now(),
      texto: texto.trim(),
      completada: false
    }
    setTareas(prev => [...prev, nuevaTarea])
  }
  
  return (
    // ... JSX
  )
}
```

### Manejo de Errores con localStorage:

```jsx
// Función segura para cargar
const loadFromLocalStorage = (key, defaultValue) => {
  try {
    const item = localStorage.getItem(key)
    return item ? JSON.parse(item) : defaultValue
  } catch (error) {
    console.error(`Error al cargar ${key}:`, error)
    return defaultValue
  }
}

// Función segura para guardar
const saveToLocalStorage = (key, value) => {
  try {
    localStorage.setItem(key, JSON.stringify(value))
  } catch (error) {
    console.error(`Error al guardar ${key}:`, error)
  }
}
```

---

## 9. Comparaciones: useState vs useReducer

### Tabla Comparativa:

| Característica | useState | useReducer |
|:---|:---|:---|
| **Simplicidad** | ✅ Simple y directo | ❌ Más código |
| **Estado simple** | ✅ Ideal | ❌ Excesivo |
| **Estado complejo** | ❌ Puede volverse complicado | ✅ Ideal |
| **Lógica centralizada** | ❌ Dispersa en el componente | ✅ Centralizada en reducer |
| **Testabilidad** | ⚠️ Media | ✅ Fácil de testear |
| **Curva de aprendizaje** | ✅ Baja | ❌ Más alta |
| **Múltiples actualizaciones** | ⚠️ Puede ser confuso | ✅ Claro con acciones |
| **Boilerplate** | ✅ Mínimo | ❌ Más código |

### Ejemplo Comparativo: Contador

**Con useState**:
```jsx
function Counter() {
  const [count, setCount] = useState(0)
  
  const increment = () => setCount(count + 1)
  const decrement = () => setCount(count - 1)
  const reset = () => setCount(0)
  
  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  )
}
```

**Con useReducer**:
```jsx
const counterReducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT': return { count: state.count + 1 }
    case 'DECREMENT': return { count: state.count - 1 }
    case 'RESET': return { count: 0 }
    default: return state
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 })
  
  return (
    <div>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>+</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>-</button>
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
    </div>
  )
}
```

**Conclusión**: Para este caso simple, `useState` es mejor. `useReducer` es útil cuando la lógica es más compleja.

---

## 10. Ejemplos Prácticos del Código Modelo

### Ejemplo 01: Hooks, Lifecycle y useReducer

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle/ejemplo-01-hooks-lifecycle-reducer`

**Conceptos cubiertos**:
- ✅ Hook personalizado (`useCounter`)
- ✅ `useEffect` con diferentes dependencias
- ✅ Lifecycle: montaje, actualización, desmontaje
- ✅ Limpieza de efectos (cleanup function)
- ✅ `useReducer` para estado complejo
- ✅ Comparación entre hooks funcionales y componentes de clase

**Hook Personalizado - useCounter**:
```jsx
function useCounter(iniciarContador, paso) {
  const [count, setCount] = useState(iniciarContador)
  
  const aumentar = () => {
    setCount(acumulador => acumulador + paso)
  }
  
  const disminuir = () => {
    setCount(acumulador => acumulador - paso)
  }
  
  useEffect(() => {
    console.log(`El contador se ha actualizado: ${count}`)
  }, [count])
  
  return { count, aumentar, disminuir }
}
```

**useReducer - Contador**:
```jsx
const counterReducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 }
    case 'DECREMENT':
      return { count: state.count - 1 }
    default:
      return state
  }
}

function ReducerComponent() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 })
  
  return (
    <div>
      <p>Contador: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>
        Incrementar
      </button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>
        Decrementar
      </button>
    </div>
  )
}
```

### Ejemplo 02: Estado Avanzado y Mapeo

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle/ejemplo-02-estado-mapeo`

**Conceptos cubiertos**:
- ✅ `useState` con diferentes tipos (número, booleano, objeto)
- ✅ Actualización de estado con función callback
- ✅ Spread operator para actualizar objetos
- ✅ Actualización dinámica de propiedades de objeto
- ✅ Renderizado de listas con `map()`
- ✅ Keys en listas de React

### Ejemplo 03: useEffect Avanzado

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle/ejemplo-03-useeffect`

**Conceptos cubiertos**:
- ✅ `useEffect` sin dependencias (solo montaje)
- ✅ `useEffect` con dependencias (montaje + actualización)
- ✅ `useEffect` con limpieza (cleanup)
- ✅ Integración con `localStorage`
- ✅ Timers e intervalos con limpieza
- ✅ Renderizado condicional con efectos

**Código del Ejemplo**:
```jsx
function EjemploEffect() {
  const [contador, setContador] = useState(0)
  const [activo, setActivo] = useState(false)
  
  // 1. Sin dependencias: Solo al montar
  useEffect(() => {
    console.log("Componente montado, se ejecuta una sola vez")
    const datosGuardados = localStorage.getItem("MiDato")
    if(datosGuardados) {
      console.log("Datos guardados", datosGuardados)
    }
  }, [])
  
  // 2. Con dependencias: Al montar y cuando contador cambia
  useEffect(() => {
    console.log("El contador cambió", contador)
    localStorage.setItem("contador", contador.toString())
  }, [contador])
  
  // 3. Con limpieza: Timer con cleanup
  useEffect(() => {
    if(!activo) return
    
    console.log("Iniciando timer...")
    const timer = setInterval(() => {
      console.log("Timer ejecutándose...")
      setContador(prevContador => prevContador + 1)
    }, 1000)
    
    return () => {
      console.log("Limpiando timer...")
      clearInterval(timer)
    }
  }, [activo])
  
  return (
    <div>
      <h2>Efectos de useEffect</h2>
      <p>Contador: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>
        Incrementar
      </button>
      <button onClick={() => setActivo(!activo)}>
        {activo ? "Pausar" : "Iniciar"}
      </button>
    </div>
  )
}
```

---

## 📚 Índice por Temas del Código Modelo

### Tema 19: State, Hooks y Lifecycle
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-19-react-state-hooks-lifecycle`

**Conceptos cubiertos**:
- ✅ `useState` con diferentes tipos de datos (número, booleano, objeto, array)
- ✅ Ciclo de vida de componentes con `useEffect`
- ✅ Hooks personalizados para reutilizar lógica
- ✅ `useReducer` para manejar estado complejo
- ✅ Renderizado de listas con `map()` y keys
- ✅ Actualización de estado con funciones callback
- ✅ Limpieza de efectos (cleanup) en `useEffect`

**Ejemplos incluidos**:
1. **Ejemplo 01**: Hooks, Lifecycle y useReducer
2. **Ejemplo 02**: Estado Avanzado y Mapeo
3. **Ejemplo 03**: useEffect Avanzado

---

## 🎯 Resumen de Conceptos Clave

### useState
- Hook para manejar estado en componentes funcionales
- Retorna `[valor, setValor]`
- Cada actualización causa un re-render
- Usar función callback cuando el nuevo valor depende del anterior

### useEffect
- Hook para efectos secundarios
- Se ejecuta después del render
- Tres casos: sin dependencias, con dependencias, con limpieza
- Siempre limpiar recursos (timers, suscripciones)

### useReducer
- Alternativa a `useState` para estado complejo
- Basado en reducer y acciones
- Ideal para lógica de actualización compleja

### Hooks Personalizados
- Funciones que comienzan con `use`
- Permiten reutilizar lógica de estado
- Mejoran la organización del código

---

## 📝 Buenas Prácticas

1. **Siempre limpiar recursos** en `useEffect` (timers, suscripciones)
2. **Incluir todas las dependencias** en el array de `useEffect`
3. **Usar función callback** en `setState` cuando depende del valor anterior
4. **Inmutabilidad**: Nunca mutar estado directamente, siempre crear nuevo objeto/array
5. **Keys únicas** en listas (no índices si la lista cambia)
6. **Hooks personalizados** para lógica reutilizable
7. **useReducer** para estado complejo, `useState` para estado simple

---

## 🚀 Próximos Pasos

Después de dominar estos conceptos, continúa con:
- **React Fase 3**: Routing y consumo de APIs
- **React Fase 4**: Estado global (Context API, Redux)
