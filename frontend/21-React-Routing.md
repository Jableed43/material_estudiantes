# React Fase 3: Routing y Consumo de APIs 🌐

## 📋 Índice

### Parte 1: Routing (Navegación)
1. [Introducción a React Router](#1-introducción-a-react-router)
2. [Configuración Básica](#2-configuración-básica)
3. [Componentes de Routing](#3-componentes-de-routing)
4. [Layout y Outlet](#4-layout-y-outlet)
5. [Rutas Anidadas](#5-rutas-anidadas)
6. [Navegación Programática](#6-navegación-programática)
7. [Rutas Dinámicas y Parámetros](#7-rutas-dinámicas-y-parámetros)
8. [Rutas Protegidas](#8-rutas-protegidas)

### Parte 2: Consumo de APIs (Data Fetching)
9. [Fetch API](#9-fetch-api)
10. [Estados Asíncronos](#10-estados-asíncronos)
11. [Hooks Personalizados para APIs](#11-hooks-personalizados-para-apis)
12. [Variables de Entorno](#12-variables-de-entorno)
13. [Paginación](#13-paginación)
14. [Manejo de Errores](#14-manejo-de-errores)
15. [Axios vs Fetch](#15-axios-vs-fetch)

### Parte 3: Ejemplos Prácticos
16. [Ejemplos Prácticos del Código Modelo](#16-ejemplos-prácticos-del-código-modelo)

---

## Parte 1: Routing (Navegación)

## 1. Introducción a React Router

### ¿Qué es React Router? (Analogía del Mundo Real)

### 🗺️ Analogía: El Sistema de Navegación GPS

Imagina un GPS:
- **Rutas**: Diferentes destinos (páginas) en tu aplicación
- **Navegación**: Cambiar de destino sin recargar el mapa
- **URL**: Como la dirección que estás visitando
- **Router**: El sistema que te lleva de un destino a otro

**React Router es como un GPS** que te permite navegar entre diferentes "destinos" (páginas) en tu aplicación sin recargar.

### 🏠 Analogía: La Casa con Múltiples Habitaciones

Piensa en una casa:
- **Habitaciones**: Diferentes páginas de tu aplicación
- **Puertas**: Los enlaces que te llevan de una habitación a otra
- **Router**: El sistema que abre las puertas correctas
- **URL**: Como la dirección de cada habitación

**Sin Router**: Tienes que salir de la casa y entrar de nuevo (recargar la página).
**Con Router**: Puedes moverte entre habitaciones sin salir de la casa (sin recargar).

### 📚 Analogía: El Libro con Páginas

Un libro:
- **Páginas**: Diferentes vistas de tu aplicación
- **Navegación**: Pasar páginas sin recargar el libro
- **Router**: El sistema que te lleva a la página correcta
- **URL**: Como el número de página

**React Router te permite "pasar páginas"** en tu aplicación sin recargar.

### ¿Qué es React Router?

**React Router** es una biblioteca que permite:
- ✅ **Navegación sin recargar**: Cambios de URL sin recargar la página
- ✅ **SPA (Single Page Application)**: Una sola página HTML
- ✅ **Rutas declarativas**: Definir rutas como componentes
- ✅ **Navegación programática**: Cambiar rutas desde código

**En términos simples**: React Router es como el sistema de navegación de tu aplicación - te permite ir de una "página" a otra sin recargar toda la aplicación.

### ¿Cuándo usar React Router?

**Usar React Router cuando**:
- ✅ Aplicación con múltiples páginas/vistas
- ✅ Necesitas URLs específicas para cada sección
- ✅ Quieres compartir URLs (bookmarking)
- ✅ Necesitas navegación del navegador (back/forward)

**NO usar React Router cuando**:
- ❌ Aplicación de una sola vista
- ❌ Solo necesitas mostrar/ocultar componentes
- ❌ No necesitas URLs específicas

### Instalación:

```bash
npm install react-router-dom
```

---

## 2. Configuración Básica

### createBrowserRouter

`createBrowserRouter` es la forma moderna de configurar rutas en React Router v6+:

```jsx
import { createBrowserRouter } from 'react-router-dom'
import Home from './pages/Home'
import Products from './pages/Products'

export const router = createBrowserRouter([
  {
    path: "/",           // URL de la ruta
    element: <Home />,   // Componente a mostrar
    index: true          // Ruta principal
  },
  {
    path: "/products",
    element: <Products />
  },
  {
    path: "*",            // Cualquier ruta no definida (404)
    element: <Error404 />
  }
])
```

### RouterProvider

El `RouterProvider` envuelve la aplicación y provee las rutas:

```jsx
import { RouterProvider } from 'react-router-dom'
import { router } from './router'

function App() {
  return <RouterProvider router={router} />
}
```

### Configuración en main.jsx:

```jsx
import { createRoot } from 'react-dom/client'
import { RouterProvider } from 'react-router-dom'
import { router } from './router'

createRoot(document.getElementById('root')).render(
  <RouterProvider router={router} />
)
```

---

## 3. Componentes de Routing

### Link

`Link` crea enlaces que no recargan la página (navegación SPA):

```jsx
import { Link } from 'react-router-dom'

function Navigation() {
  return (
    <nav>
      <Link to="/">Inicio</Link>
      <Link to="/products">Productos</Link>
      <Link to="/about">Acerca de</Link>
    </nav>
  )
}
```

**Características**:
- ✅ Navegación simple
- ✅ No tiene estilos especiales
- ✅ Ideal para enlaces ocasionales

### NavLink

`NavLink` detecta si la ruta está activa y permite estilos condicionales:

```jsx
import { NavLink } from 'react-router-dom'

function Navigation() {
  return (
    <nav>
      <NavLink 
        to="/"
        className={({ isActive }) => isActive ? "active" : ""}
      >
        Inicio
      </NavLink>
      <NavLink 
        to="/products"
        className={({ isActive }) => isActive ? "active" : ""}
      >
        Productos
      </NavLink>
    </nav>
  )
}
```

**Características**:
- ✅ Detecta ruta activa
- ✅ Permite estilos condicionales
- ✅ Ideal para navegación principal (navbar)

**Diferencia con Link**:
- `Link`: Enlace simple, no detecta ruta activa
- `NavLink`: Enlace que detecta ruta activa, ideal para navegación

### Routes y Route (Versión Antigua)

**Nota**: En React Router v6+, se usa `createBrowserRouter`. La versión antigua usaba `Routes` y `Route`:

```jsx
// Versión antigua (v5)
import { BrowserRouter, Routes, Route } from 'react-router-dom'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/products" element={<Products />} />
      </Routes>
    </BrowserRouter>
  )
}
```

**Versión moderna (v6+)**:
```jsx
// Usar createBrowserRouter (recomendado)
import { createBrowserRouter } from 'react-router-dom'

export const router = createBrowserRouter([
  { path: "/", element: <Home /> },
  { path: "/products", element: <Products /> }
])
```

---

## 4. Layout y Outlet

### ¿Qué es un Layout?

Un **Layout** es un componente que envuelve múltiples páginas compartiendo estructura común (header, footer, navegación).

### Outlet

`Outlet` es un componente que renderiza las rutas hijas (children) dentro del Layout:

```jsx
import { Outlet, Link, NavLink } from 'react-router-dom'

function Layout() {
  return (
    <>
      <header>
        <nav>
          <NavLink to="/">Inicio</NavLink>
          <NavLink to="/products">Productos</NavLink>
        </nav>
      </header>
      
      <main>
        <Outlet />  {/* Aquí se renderizan las páginas */}
      </main>
      
      <footer>
        <p>Soy footer!</p>
      </footer>
    </>
  )
}
```

### Configurar Layout en el Router:

```jsx
import { createBrowserRouter } from 'react-router-dom'
import Layout from './components/Layout'
import Home from './pages/Home'
import Products from './pages/Products'

export const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,  // Layout compartido
    children: [           // Rutas anidadas
      {
        index: true,      // Ruta principal (/)
        element: <Home />
      },
      {
        path: "products",  // Ruta relativa: /products
        element: <Products />
      }
    ]
  }
])
```

**Ventajas del Layout**:
- ✅ Evita repetir header/footer
- ✅ Estilos consistentes
- ✅ Menos código
- ✅ Fácil mantenimiento

---

## 5. Rutas Anidadas

### Rutas Relativas vs Absolutas

En rutas anidadas (children), las rutas son **relativas** al padre:

```jsx
export const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    children: [
      {
        path: "products",      // Relativa: /products
        element: <Products />
      },
      {
        path: "about",         // Relativa: /about
        element: <About />
      }
    ]
  }
])
```

**Rutas absolutas** (con `/`):
```jsx
{
  path: "/products",  // Absoluta: siempre /products
  element: <Products />
}
```

### Rutas Anidadas Múltiples Niveles:

```jsx
export const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    children: [
      {
        path: "products",
        element: <ProductsLayout />,
        children: [
          {
            path: ":id",           // /products/:id
            element: <ProductDetail />
          }
        ]
      }
    ]
  }
])
```

---

## 6. Navegación Programática

### useNavigate

`useNavigate` permite navegar programáticamente desde código:

```jsx
import { useNavigate } from 'react-router-dom'

function Componente() {
  const navigate = useNavigate()
  
  const handleClick = () => {
    navigate('/products')  // Navegar a otra ruta
  }
  
  const handleSubmit = async (data) => {
    await guardarDatos(data)
    navigate('/success')  // Navegar después de una acción
  }
  
  return (
    <button onClick={handleClick}>Ir a productos</button>
  )
}
```

### Navegación con Historial:

```jsx
const navigate = useNavigate()

// Navegar hacia atrás
navigate(-1)

// Navegar hacia adelante
navigate(1)

// Navegar a ruta específica
navigate('/products')

// Navegar con reemplazo (no agrega al historial)
navigate('/products', { replace: true })
```

**Casos de uso**:
- ✅ Navegar después de un submit
- ✅ Redirección después de login
- ✅ Navegación condicional
- ✅ Botones de "Atrás" o "Siguiente"

---

## 7. Rutas Dinámicas y Parámetros

### Rutas con Parámetros

Las rutas dinámicas permiten capturar valores de la URL:

```jsx
export const router = createBrowserRouter([
  {
    path: "/product/:id",  // :id es un parámetro
    element: <ProductDetail />
  },
  {
    path: "/user/:userId/post/:postId",  // Múltiples parámetros
    element: <UserPost />
  }
])
```

### useParams

`useParams` permite obtener los parámetros de la URL:

```jsx
import { useParams } from 'react-router-dom'

function ProductDetail() {
  const { id } = useParams()  // Obtener parámetro :id
  
  return <div>Producto ID: {id}</div>
}
```

### Ejemplo Completo:

```jsx
// router.jsx
export const router = createBrowserRouter([
  {
    path: "/products",
    element: <Products />
  },
  {
    path: "/product/:id",
    element: <ProductDetail />
  }
])

// ProductDetail.jsx
import { useParams } from 'react-router-dom'

function ProductDetail() {
  const { id } = useParams()
  
  // Usar id para cargar datos del producto
  const [producto, setProducto] = useState(null)
  
  useEffect(() => {
    fetch(`/api/products/${id}`)
      .then(res => res.json())
      .then(data => setProducto(data))
  }, [id])
  
  return (
    <div>
      <h1>Producto {id}</h1>
      {producto && <p>{producto.nombre}</p>}
    </div>
  )
}
```

---

## 8. Rutas Protegidas

### Concepto de Ruta Protegida

Una **ruta protegida** es una ruta que requiere autenticación o permisos específicos para acceder.

### Implementación Básica:

```jsx
import { Navigate } from 'react-router-dom'

function ProtectedRoute({ children }) {
  const isAuthenticated = // lógica de autenticación
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />
  }
  
  return children
}
```

### Uso en el Router:

```jsx
export const router = createBrowserRouter([
  {
    path: "/login",
    element: <Login />
  },
  {
    path: "/dashboard",
    element: (
      <ProtectedRoute>
        <Dashboard />
      </ProtectedRoute>
    )
  }
])
```

### Rutas Protegidas con Roles:

```jsx
function ProtectedRoute({ children, allowedRoles }) {
  const { user, isAuthenticated } = useAuth()
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />
  }
  
  if (allowedRoles && !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />
  }
  
  return children
}

// Uso
{
  path: "/admin",
  element: (
    <ProtectedRoute allowedRoles={["ADMIN"]}>
      <AdminPanel />
    </ProtectedRoute>
  )
}
```

---

## Parte 2: Consumo de APIs (Data Fetching)

## 9. Fetch API

### ¿Qué es Fetch?

**Fetch API** es una función nativa del navegador para hacer peticiones HTTP. Retorna una Promise.

### Sintaxis Básica:

```jsx
async function fetchData() {
  try {
    const response = await fetch('https://api.ejemplo.com/datos')
    
    if (!response.ok) {
      throw new Error('Error en la solicitud')
    }
    
    const data = await response.json()
    return data
  } catch (error) {
    console.error('Error:', error)
  }
}
```

### Características de Fetch:

- ✅ **Método nativo**: No requiere instalación
- ✅ **Retorna Promise**: Necesita `await` o `.then()`
- ✅ **`response.json()`**: Convierte JSON a objeto JavaScript
- ✅ **Validación necesaria**: Verificar `response.ok`

### Ejemplo Completo:

```jsx
async function obtenerUsuarios() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    
    const usuarios = await response.json()
    return usuarios
  } catch (error) {
    console.error('Error fetching usuarios:', error)
    throw error
  }
}
```

### Fetch con Opciones:

```jsx
// GET (por defecto)
const response = await fetch('https://api.ejemplo.com/datos')

// POST
const response = await fetch('https://api.ejemplo.com/datos', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    nombre: 'Juan',
    email: 'juan@ejemplo.com'
  })
})

// PUT
const response = await fetch('https://api.ejemplo.com/datos/1', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ nombre: 'Juan Actualizado' })
})

// DELETE
const response = await fetch('https://api.ejemplo.com/datos/1', {
  method: 'DELETE'
})
```

---

## 10. Estados Asíncronos

### Estados Necesarios para APIs

Cuando consumes una API, necesitas manejar tres estados:

```jsx
const [data, setData] = useState(null)      // Los datos obtenidos
const [loading, setLoading] = useState(true) // Si está cargando
const [error, setError] = useState(null)    // Si hay error
```

### Ejemplo con Estados:

```jsx
function Componente() {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    const fetchData = async () => {
      setLoading(true)
      setError(null)
      
      try {
        const response = await fetch('https://api.ejemplo.com/datos')
        
        if (!response.ok) {
          throw new Error('Error en la solicitud')
        }
        
        const datos = await response.json()
        setData(datos)
      } catch (err) {
        setError(err)
      } finally {
        setLoading(false)
      }
    }
    
    fetchData()
  }, [])
  
  if (loading) return <p>Cargando...</p>
  if (error) return <p>Error: {error.message}</p>
  if (!data) return <p>No hay datos</p>
  
  return <div>{/* Renderizar datos */}</div>
}
```

### Renderizado Condicional para Estados:

```jsx
function Componente() {
  const { data, loading, error } = useApi()
  
  // Mostrar loading
  if (loading) {
    return (
      <div>
        <p>Cargando...</p>
        <Spinner />
      </div>
    )
  }
  
  // Mostrar error
  if (error) {
    return (
      <div>
        <p>Error: {error.message}</p>
        <button onClick={reintentar}>Reintentar</button>
      </div>
    )
  }
  
  // Mostrar datos
  return (
    <div>
      {data.map(item => (
        <Item key={item.id} item={item} />
      ))}
    </div>
  )
}
```

---

## 11. Hooks Personalizados para APIs

### ¿Por qué crear Hooks Personalizados para APIs?

- ✅ **Reutilización**: Una vez creado, se usa en múltiples componentes
- ✅ **Separación de responsabilidades**: Lógica de API separada de la presentación
- ✅ **Manejo automático de estados**: Loading, error, data
- ✅ **Código más limpio**: Componentes más simples

### Estructura de un Hook Personalizado para API:

```jsx
function useApi(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    const fetchData = async () => {
      setLoading(true)
      setError(null)
      
      try {
        const response = await fetch(url)
        if (!response.ok) {
          throw new Error('Error en la solicitud')
        }
        const datos = await response.json()
        setData(datos)
      } catch (err) {
        setError(err)
      } finally {
        setLoading(false)
      }
    }
    
    fetchData()
  }, [url])
  
  return { data, loading, error }
}
```

### Ejemplo Completo: Hook para Rick and Morty API

```jsx
import { useEffect, useState } from "react"

const useRickAndMortyApi = () => {
  // Estados
  const [characters, setCharacters] = useState([])
  const [info, setInfo] = useState({})
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  
  // Variable de entorno
  const initialUrl = import.meta.env.VITE_RICK_AND_MORTY_API_URL
  
  // Función para hacer fetch
  const fetchCharacters = async (url) => {
    setLoading(true)
    setError(null)
    
    try {
      const response = await fetch(url)
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }
      
      const data = await response.json()
      setCharacters(data.results)
      setInfo(data.info)
    } catch (error) {
      console.error("Error fetching characters: ", error)
      setError(error)
      setCharacters([])
      setInfo({})
    } finally {
      setLoading(false)
    }
  }
  
  // Llamada inicial
  useEffect(() => {
    if (initialUrl) {
      fetchCharacters(initialUrl)
    } else {
      setError(new Error("URL no definida en variables de entorno"))
      setLoading(false)
    }
  }, [initialUrl])
  
  // Funciones de paginación
  const onPrevious = () => {
    if (info.prev) {
      fetchCharacters(info.prev)
    }
  }
  
  const onNext = () => {
    if (info.next) {
      fetchCharacters(info.next)
    }
  }
  
  return {
    characters,
    info,
    loading,
    error,
    onPrevious,
    onNext
  }
}

export default useRickAndMortyApi
```

### Uso del Hook Personalizado:

```jsx
function CharactersPage() {
  const { 
    characters, 
    loading, 
    error, 
    onPrevious, 
    onNext 
  } = useRickAndMortyApi()
  
  if (loading) return <p>Cargando...</p>
  if (error) return <p>Error: {error.message}</p>
  
  return (
    <div>
      {characters.map(character => (
        <CharacterCard key={character.id} character={character} />
      ))}
      <button onClick={onPrevious}>Anterior</button>
      <button onClick={onNext}>Siguiente</button>
    </div>
  )
}
```

---

## 12. Variables de Entorno

### ¿Qué son las Variables de Entorno?

Las **variables de entorno** son valores que se configuran fuera del código y pueden cambiar según el entorno (desarrollo, producción).

### Configuración en Vite:

**Crear archivo `.env`**:
```env
VITE_API_URL=https://api.ejemplo.com
VITE_RICK_AND_MORTY_API_URL=https://rickandmortyapi.com/api/character
```

**Importante**: En Vite, las variables deben empezar con `VITE_` para ser accesibles.

### Uso en el Código:

```jsx
// Obtener variable de entorno
const apiUrl = import.meta.env.VITE_API_URL

// Usar en fetch
const response = await fetch(`${apiUrl}/usuarios`)
```

### Ejemplo Completo:

```jsx
// .env
VITE_API_URL=https://api.ejemplo.com

// En el código
function useApi() {
  const apiUrl = import.meta.env.VITE_API_URL
  
  useEffect(() => {
    const fetchData = async () => {
      const response = await fetch(`${apiUrl}/datos`)
      // ...
    }
    fetchData()
  }, [apiUrl])
}
```

**Ventajas**:
- ✅ **Seguridad**: No se exponen en el código
- ✅ **Flexibilidad**: Fáciles de cambiar según el entorno
- ✅ **Configuración centralizada**: Un solo lugar para URLs

---

## 13. Paginación

### Implementación de Paginación

La paginación permite navegar entre páginas de resultados de una API:

```jsx
const fetchCharacters = async (url) => {
  const response = await fetch(url)
  const data = await response.json()
  
  setCharacters(data.results)  // Array de items
  setInfo(data.info)            // { prev, next, pages, count }
}

const onNext = () => {
  if (info.next) {
    fetchCharacters(info.next)
  }
}

const onPrevious = () => {
  if (info.prev) {
    fetchCharacters(info.prev)
  }
}
```

### Componente de Paginación:

```jsx
function Pagination({ info, onPrevious, onNext }) {
  return (
    <div>
      <button 
        onClick={onPrevious} 
        disabled={!info.prev}
      >
        Anterior
      </button>
      <span>Página {info.currentPage} de {info.pages}</span>
      <button 
        onClick={onNext} 
        disabled={!info.next}
      >
        Siguiente
      </button>
    </div>
  )
}
```

---

## 14. Manejo de Errores

### Validación de Respuesta

Siempre validar `response.ok` antes de procesar datos:

```jsx
const response = await fetch(url)

if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`)
}

const data = await response.json()
```

### Try/Catch para Errores:

```jsx
try {
  const response = await fetch(url)
  
  if (!response.ok) {
    throw new Error(`Error: ${response.status}`)
  }
  
  const data = await response.json()
  setData(data)
} catch (error) {
  console.error('Error:', error)
  setError(error)
}
```

### Manejo de Errores en Hooks Personalizados:

```jsx
function useApi(url) {
  const [error, setError] = useState(null)
  
  useEffect(() => {
    const fetchData = async () => {
      setError(null)  // Limpiar error anterior
      
      try {
        const response = await fetch(url)
        
        if (!response.ok) {
          throw new Error(`Error ${response.status}: ${response.statusText}`)
        }
        
        const data = await response.json()
        setData(data)
      } catch (err) {
        setError(err)
        setData(null)  // Limpiar datos en caso de error
      }
    }
    
    fetchData()
  }, [url])
  
  return { data, loading, error }
}
```

---

## 15. Axios vs Fetch

### Fetch API (Nativo)

**Ventajas**:
- ✅ **Nativo del navegador**: No requiere instalación
- ✅ **Ligero**: No agrega peso al bundle
- ✅ **Suficiente para casos básicos**: GET, POST simples

**Desventajas**:
- ❌ **Requiere `response.json()`**: Paso adicional
- ❌ **Manejo de errores manual**: Validar `response.ok`
- ❌ **Sin interceptores**: No hay interceptores nativos
- ❌ **Sin transformación automática**: JSON no se transforma automáticamente

**Ejemplo**:
```jsx
const response = await fetch(url)
if (!response.ok) {
  throw new Error('Error')
}
const data = await response.json()
```

### Axios (Librería Externa)

**Ventajas**:
- ✅ **Transforma JSON automáticamente**: No necesitas `response.json()`
- ✅ **Manejo de errores más claro**: Errores HTTP automáticos
- ✅ **Interceptores**: Para requests y responses
- ✅ **Cancelación de requests**: Cancelar peticiones pendientes
- ✅ **Timeouts**: Configurar timeouts fácilmente

**Desventajas**:
- ❌ **Requiere instalación**: `npm install axios`
- ❌ **Agrega peso**: Aumenta el tamaño del bundle
- ❌ **Dependencia externa**: Una dependencia más que mantener

**Instalación**:
```bash
npm install axios
```

**Ejemplo**:
```jsx
import axios from 'axios'

// GET
const response = await axios.get(url)
const data = response.data  // JSON ya transformado

// POST
const response = await axios.post(url, {
  nombre: 'Juan',
  email: 'juan@ejemplo.com'
})

// Con manejo de errores automático
try {
  const response = await axios.get(url)
  const data = response.data
} catch (error) {
  // Axios maneja errores HTTP automáticamente
  console.error('Error:', error.response?.status)
}
```

### Comparación Práctica:

**Fetch**:
```jsx
// Fetch requiere más código
const fetchData = async () => {
  try {
    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`Error: ${response.status}`)
    }
    const data = await response.json()
    return data
  } catch (error) {
    console.error(error)
  }
}
```

**Axios**:
```jsx
// Axios es más conciso
const fetchData = async () => {
  try {
    const response = await axios.get(url)
    return response.data  // JSON ya transformado
  } catch (error) {
    console.error(error.response?.status)
  }
}
```

### ¿Cuándo usar cada uno?

**Usar Fetch cuando**:
- ✅ Proyecto pequeño o simple
- ✅ No quieres agregar dependencias
- ✅ Solo necesitas peticiones básicas

**Usar Axios cuando**:
- ✅ Proyecto grande o complejo
- ✅ Necesitas interceptores
- ✅ Prefieres manejo de errores automático
- ✅ Necesitas cancelación de requests

---

## Parte 3: Ejemplos Prácticos

## 16. Ejemplos Prácticos del Código Modelo

### Ejemplo 01: Routing Básico

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-21-react-routing/ejemplo-01-routing-basico`

**Conceptos cubiertos**:
- ✅ Configuración básica de React Router
- ✅ `createBrowserRouter` para definir rutas
- ✅ `RouterProvider` para proveer rutas
- ✅ Componente `Link` para navegación
- ✅ Rutas simples (path + element)
- ✅ Ruta index (página principal)
- ✅ Manejo de rutas no encontradas (404)

**Código del Router**:
```jsx
import { createBrowserRouter } from 'react-router-dom'
import Home from './pages/Home'
import Products from './pages/Products'

export const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />,
    index: true  // Ruta principal
  },
  {
    path: "/products",
    element: <Products />
  },
  {
    path: "*",  // Cualquier ruta no definida
    element: (
      <div>
        <h1>Error 404</h1>
        <p>Página no encontrada</p>
      </div>
    )
  }
])
```

### Ejemplo 02: Routing con Layout

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-21-react-routing/ejemplo-02-routing-con-layout`

**Conceptos cubiertos**:
- ✅ Layout compartido con `Outlet`
- ✅ Rutas anidadas (children)
- ✅ `NavLink` con estilos activos
- ✅ Estructura de carpetas organizada
- ✅ Componentes reutilizables
- ✅ Rutas relativas en children

**Código del Layout**:
```jsx
import { Link, NavLink, Outlet } from 'react-router-dom'

function Layout() {
  return (
    <>
      <header>
        <nav>
          <Link to="/">
            <i className="fa-solid fa-house"></i>
          </Link>
          <NavLink to="/">Inicio</NavLink>
          <NavLink to="/products">Productos</NavLink>
        </nav>
      </header>
      
      <main>
        <Outlet />  {/* Aquí se renderizan las páginas */}
      </main>
      
      <footer>Soy footer!</footer>
    </>
  )
}
```

**Código del Router con Layout**:
```jsx
import { createBrowserRouter } from 'react-router-dom'
import Layout from './components/Layout'
import Home from './pages/Home'
import Products from './pages/Products'

export const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,  // Layout compartido
    children: [           // Rutas anidadas
      {
        index: true,
        element: <Home />
      },
      {
        path: "products",  // Ruta relativa: /products
        element: <Products />
      }
    ]
  }
])
```

### Ejemplo 03: Fetch Básico - NASA API

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-22-react-apis-hooks-personalizados/ejemplo-01-fetch-basico-nasa`

**Conceptos cubiertos**:
- ✅ Función `fetch` básica
- ✅ `async/await` para manejar asincronismo
- ✅ Manejo de errores con `try/catch`
- ✅ Validación de respuesta (`response.ok`)
- ✅ Conversión de JSON a objeto JavaScript

### Ejemplo 04: Hook Personalizado Completo - Rick and Morty API

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-22-react-apis-hooks-personalizados/ejemplo-02-hook-personalizado-completo`

**Conceptos cubiertos**:
- ✅ Hook personalizado completo (`useRickAndMortyApi`)
- ✅ Manejo de estados: loading, error, data
- ✅ `useEffect` para llamadas automáticas
- ✅ Paginación con funciones `onNext` y `onPrevious`
- ✅ Variables de entorno (`import.meta.env`)
- ✅ Hook adicional para detalles (`useCharacterDetail`)
- ✅ Componentes separados (Characters, Pagination, CharacterDetail)
- ✅ Integración con React Router (`useParams`, `useNavigate`)

**Código del Hook**:
```jsx
import { useEffect, useState } from "react"

const useRickAndMortyApi = () => {
  const [characters, setCharacters] = useState([])
  const [info, setInfo] = useState({})
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  
  const initialUrl = import.meta.env.VITE_RICK_AND_MORTY_API_URL
  
  const fetchCharacters = async (url) => {
    setLoading(true)
    setError(null)
    
    try {
      const response = await fetch(url)
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }
      const data = await response.json()
      setCharacters(data.results)
      setInfo(data.info)
    } catch (error) {
      console.error("Error fetching characters: ", error)
      setError(error)
      setCharacters([])
      setInfo({})
    } finally {
      setLoading(false)
    }
  }
  
  useEffect(() => {
    if (initialUrl) {
      fetchCharacters(initialUrl)
    } else {
      setError(new Error("URL no definida"))
      setLoading(false)
    }
  }, [initialUrl])
  
  const onNext = () => {
    if (info.next) {
      fetchCharacters(info.next)
    }
  }
  
  const onPrevious = () => {
    if (info.prev) {
      fetchCharacters(info.prev)
    }
  }
  
  return {
    characters,
    info,
    loading,
    error,
    onPrevious,
    onNext
  }
}

export default useRickAndMortyApi
```

---

## 📚 Índice por Temas del Código Modelo

### Tema 21: React - Routing
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-21-react-routing`

**Conceptos cubiertos**:
- ✅ Configurar rutas con `createBrowserRouter`
- ✅ Usar `Link` y `NavLink` para navegación
- ✅ Crear Layouts compartidos con `Outlet`
- ✅ Manejar rutas anidadas (children)
- ✅ Implementar página 404 (error)
- ✅ Usar `useNavigate` para navegación programática

**Ejemplos incluidos**:
1. **Ejemplo 01**: Routing Básico
2. **Ejemplo 02**: Routing con Layout

### Tema 22: React - Consumo de API y Hooks Personalizados
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-22-react-apis-hooks-personalizados`

**Conceptos cubiertos**:
- ✅ Usar `fetch` para consumir APIs REST
- ✅ Comprender el manejo de estados asíncronos (loading, error, data)
- ✅ Crear hooks personalizados para reutilizar lógica de APIs
- ✅ Manejar paginación en APIs
- ✅ Integrar variables de entorno para URLs de API
- ✅ Implementar renderizado condicional para loading y error
- ✅ Usar `useParams` para rutas dinámicas con APIs

**Ejemplos incluidos**:
1. **Ejemplo 01**: Fetch Básico - NASA API
2. **Ejemplo 02**: Hook Personalizado Completo - Rick and Morty API

---

## 🎯 Resumen de Conceptos Clave

### Routing
- `createBrowserRouter`: Configuración moderna de rutas
- `RouterProvider`: Provee rutas a la aplicación
- `Link` y `NavLink`: Navegación sin recargar
- `Outlet`: Renderiza rutas hijas en Layout
- `useNavigate`: Navegación programática
- `useParams`: Obtener parámetros de la URL

### Data Fetching
- `fetch`: API nativa para peticiones HTTP
- Estados asíncronos: `loading`, `error`, `data`
- Hooks personalizados: Reutilizar lógica de APIs
- Variables de entorno: Configuración externa
- Paginación: Navegar entre páginas de resultados

---

## 📝 Buenas Prácticas

1. **Siempre manejar loading y error**: No asumir que la API siempre funciona
2. **Validar respuesta**: Usar `response.ok` antes de procesar datos
3. **Hooks personalizados**: Crear hooks para lógica de API reutilizable
4. **Variables de entorno**: No hardcodear URLs en el código
5. **Renderizado condicional**: Mostrar diferentes estados según loading/error/data
6. **Layout compartido**: Usar `Outlet` para evitar repetir código
7. **Rutas protegidas**: Implementar autenticación cuando sea necesario
8. **Limpiar recursos**: Cancelar peticiones pendientes si el componente se desmonta

---

## 🚀 Próximos Pasos

Después de dominar estos conceptos, continúa con:
- **React Fase 4**: Estado global (Context API, Redux)

---

**Referencias del Código Modelo**:
- `cursadas/frontend/frontEnd_modelo/tema-21-react-routing/`
- `cursadas/frontend/frontEnd_modelo/tema-22-react-apis-hooks-personalizados/`
