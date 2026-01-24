# React: Consumo de APIs y Hooks Personalizados 🌐

## Fetch API

### Fetch Básico

```jsx
function Componente() {
  const [datos, setDatos] = useState(null)
  const [cargando, setCargando] = useState(true)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    fetch('https://api.ejemplo.com/datos')
      .then(response => response.json())
      .then(data => {
        setDatos(data)
        setCargando(false)
      })
      .catch(err => {
        setError(err)
        setCargando(false)
      })
  }, [])
  
  if (cargando) return <p>Cargando...</p>
  if (error) return <p>Error: {error.message}</p>
  
  return <div>{/* Mostrar datos */}</div>
}
```

### Fetch con async/await

```jsx
useEffect(() => {
  const obtenerDatos = async () => {
    try {
      const response = await fetch('https://api.ejemplo.com/datos')
      const data = await response.json()
      setDatos(data)
      setCargando(false)
    } catch (err) {
      setError(err)
      setCargando(false)
    }
  }
  
  obtenerDatos()
}, [])
```

---

## Estados Asíncronos

### Manejo de Estados

```jsx
const [estado, setEstado] = useState({
  datos: null,
  cargando: true,
  error: null
})

useEffect(() => {
  setEstado({ cargando: true, error: null })
  
  fetch('https://api.ejemplo.com/datos')
    .then(response => response.json())
    .then(data => {
      setEstado({ datos: data, cargando: false, error: null })
    })
    .catch(error => {
      setEstado({ datos: null, cargando: false, error })
    })
}, [])
```

---

## Hooks Personalizados para APIs

### useFetch Hook

```jsx
function useFetch(url) {
  const [datos, setDatos] = useState(null)
  const [cargando, setCargando] = useState(true)
  const [error, setError] = useState(null)
  
  useEffect(() => {
    const obtenerDatos = async () => {
      try {
        setCargando(true)
        const response = await fetch(url)
        const data = await response.json()
        setDatos(data)
        setError(null)
      } catch (err) {
        setError(err)
      } finally {
        setCargando(false)
      }
    }
    
    obtenerDatos()
  }, [url])
  
  return { datos, cargando, error }
}

// Uso
function Componente() {
  const { datos, cargando, error } = useFetch('https://api.ejemplo.com/datos')
  
  if (cargando) return <p>Cargando...</p>
  if (error) return <p>Error</p>
  
  return <div>{/* Mostrar datos */}</div>
}
```

---

## Variables de Entorno

### .env

```env
VITE_API_URL=https://api.ejemplo.com
VITE_API_KEY=tu-api-key
```

### Uso en Código

```jsx
const apiUrl = import.meta.env.VITE_API_URL
const apiKey = import.meta.env.VITE_API_KEY

fetch(`${apiUrl}/datos`, {
  headers: {
    'Authorization': `Bearer ${apiKey}`
  }
})
```

---

## Paginación

```jsx
function ListaProductos() {
  const [productos, setProductos] = useState([])
  const [pagina, setPagina] = useState(1)
  const [cargando, setCargando] = useState(false)
  
  useEffect(() => {
    setCargando(true)
    fetch(`https://api.ejemplo.com/productos?page=${pagina}`)
      .then(res => res.json())
      .then(data => {
        setProductos(data)
        setCargando(false)
      })
  }, [pagina])
  
  return (
    <div>
      {productos.map(producto => (
        <div key={producto.id}>{producto.nombre}</div>
      ))}
      <button onClick={() => setPagina(pagina + 1)}>
        Siguiente
      </button>
    </div>
  )
}
```

---

## Manejo de Errores

```jsx
function Componente() {
  const [error, setError] = useState(null)
  
  const obtenerDatos = async () => {
    try {
      const response = await fetch('https://api.ejemplo.com/datos')
      
      if (!response.ok) {
        throw new Error(`Error ${response.status}`)
      }
      
      const data = await response.json()
      return data
    } catch (err) {
      setError(err.message)
      throw err
    }
  }
  
  return (
    <div>
      {error && <p>Error: {error}</p>}
      {/* Resto del componente */}
    </div>
  )
}
```

---

## Axios vs Fetch

### Fetch (Nativo)

```jsx
fetch('https://api.ejemplo.com/datos', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ nombre: 'Juan' })
})
  .then(response => response.json())
  .then(data => console.log(data))
```

### Axios

```jsx
import axios from 'axios'

axios.post('https://api.ejemplo.com/datos', {
  nombre: 'Juan'
})
  .then(response => console.log(response.data))
```

**Ventajas de Axios**:
- ✅ Sintaxis más simple
- ✅ Transformación automática de JSON
- ✅ Interceptores
- ✅ Mejor manejo de errores

---

## Ejemplos Prácticos

### Ejemplo 1: Hook Personalizado Completo

```jsx
function useApi(url) {
  const [estado, setEstado] = useState({
    datos: null,
    cargando: true,
    error: null
  })
  
  useEffect(() => {
    let cancelado = false
    
    const obtenerDatos = async () => {
      try {
        setEstado(prev => ({ ...prev, cargando: true, error: null }))
        const response = await fetch(url)
        const data = await response.json()
        
        if (!cancelado) {
          setEstado({ datos: data, cargando: false, error: null })
        }
      } catch (err) {
        if (!cancelado) {
          setEstado({ datos: null, cargando: false, error: err })
        }
      }
    }
    
    obtenerDatos()
    
    return () => {
      cancelado = true
    }
  }, [url])
  
  return estado
}
```

---

## Conceptos Clave

1. **Fetch API**: Método nativo para hacer peticiones HTTP
2. **async/await**: Sintaxis moderna para código asíncrono
3. **Hooks Personalizados**: Reutilizar lógica de APIs
4. **Variables de Entorno**: Configuración externa
5. **Paginación**: Cargar datos por páginas
6. **Manejo de Errores**: Capturar y mostrar errores
7. **Axios**: Librería alternativa a Fetch

---

## Buenas Prácticas

- Usa hooks personalizados para reutilizar lógica de APIs
- Maneja estados de carga y error
- Cancela peticiones cuando el componente se desmonta
- Usa variables de entorno para URLs y keys
- Implementa paginación para grandes cantidades de datos
- Valida respuestas antes de usar los datos
- Considera usar Axios para proyectos complejos

