# React: Proyecto E-commerce 🛒

## 📑 Índice

1. [¿Qué es un E-commerce? (Analogía del Mundo Real)](#qué-es-un-e-commerce-analogía-del-mundo-real)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Componentes Principales](#componentes-principales)
4. [Context API para Carrito](#context-api-para-carrito)
5. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es un E-commerce? (Analogía del Mundo Real)

### 🛒 Analogía: La Tienda Online

Imagina una tienda física:
- **E-commerce**: Es como tener una tienda pero en internet
- **Productos**: Los artículos que vendes
- **Carrito**: Donde los clientes guardan lo que quieren comprar
- **Checkout**: El proceso de pago

**Un e-commerce es como una tienda online** donde los usuarios pueden ver productos, agregarlos al carrito y comprarlos.

### 🏪 Analogía: El Supermercado Digital

Piensa en un supermercado:
- **E-commerce**: Es como un supermercado pero en internet
- **Catálogo**: Los productos disponibles
- **Carrito**: Donde guardas lo que quieres comprar
- **Pago**: El proceso de compra

**Un e-commerce replica la experiencia de compra** pero en formato digital.

### 📱 Analogía: La App de Compras

Una app de compras:
- **E-commerce**: Es como tener una app donde puedes comprar
- **Navegación**: Ver diferentes categorías y productos
- **Carrito**: Agregar productos que quieres comprar
- **Checkout**: Completar la compra

**Un e-commerce es como una app de compras** donde puedes navegar, agregar al carrito y comprar.

### Introducción

Este documento cubre los conceptos clave para construir un proyecto e-commerce completo con React.

**En términos simples**: Un e-commerce es como una tienda online donde los usuarios pueden ver productos, agregarlos al carrito y comprarlos.

---

## Estructura del Proyecto

### Carpetas Recomendadas

```
ecommerce/
├── src/
│   ├── components/
│   │   ├── ProductCard.jsx
│   │   ├── Cart.jsx
│   │   ├── Navbar.jsx
│   │   └── Footer.jsx
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Products.jsx
│   │   ├── ProductDetail.jsx
│   │   └── Checkout.jsx
│   ├── context/
│   │   └── CartContext.jsx
│   ├── hooks/
│   │   └── useCart.js
│   └── App.jsx
```

---

## Componentes Principales

### ProductCard

```jsx
function ProductCard({ producto, onAddToCart }) {
  return (
    <div className="product-card">
      <img src={producto.imagen} alt={producto.nombre} />
      <h3>{producto.nombre}</h3>
      <p>${producto.precio}</p>
      <button onClick={() => onAddToCart(producto)}>
        Agregar al Carrito
      </button>
    </div>
  )
}
```

### Cart

```jsx
function Cart({ items, onRemove, onUpdateQuantity }) {
  const total = items.reduce((sum, item) => 
    sum + (item.precio * item.cantidad), 0
  )
  
  return (
    <div className="cart">
      <h2>Carrito</h2>
      {items.map(item => (
        <div key={item.id}>
          <span>{item.nombre}</span>
          <span>Cantidad: {item.cantidad}</span>
          <span>${item.precio * item.cantidad}</span>
          <button onClick={() => onRemove(item.id)}>Eliminar</button>
        </div>
      ))}
      <p>Total: ${total}</p>
    </div>
  )
}
```

---

## Context API para Carrito

### CartContext

```jsx
import { createContext, useContext, useState } from 'react'

const CartContext = createContext()

export function CartProvider({ children }) {
  const [items, setItems] = useState([])
  
  const addToCart = (producto) => {
    setItems(prev => {
      const existente = prev.find(item => item.id === producto.id)
      if (existente) {
        return prev.map(item =>
          item.id === producto.id
            ? { ...item, cantidad: item.cantidad + 1 }
            : item
        )
      }
      return [...prev, { ...producto, cantidad: 1 }]
    })
  }
  
  const removeFromCart = (id) => {
    setItems(prev => prev.filter(item => item.id !== id))
  }
  
  return (
    <CartContext.Provider value={{ items, addToCart, removeFromCart }}>
      {children}
    </CartContext.Provider>
  )
}

export const useCart = () => useContext(CartContext)
```

---

## Routing

### Configuración de Rutas

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import Products from './pages/Products'
import ProductDetail from './pages/ProductDetail'
import Checkout from './pages/Checkout'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/products" element={<Products />} />
        <Route path="/products/:id" element={<ProductDetail />} />
        <Route path="/checkout" element={<Checkout />} />
      </Routes>
    </BrowserRouter>
  )
}
```

---

## Consumo de API

### Obtener Productos

```jsx
function Products() {
  const [productos, setProductos] = useState([])
  const [cargando, setCargando] = useState(true)
  
  useEffect(() => {
    fetch('https://api.ejemplo.com/productos')
      .then(res => res.json())
      .then(data => {
        setProductos(data)
        setCargando(false)
      })
  }, [])
  
  if (cargando) return <p>Cargando...</p>
  
  return (
    <div className="products-grid">
      {productos.map(producto => (
        <ProductCard key={producto.id} producto={producto} />
      ))}
    </div>
  )
}
```

---

## Funcionalidades Clave

### 1. Búsqueda de Productos

```jsx
const [busqueda, setBusqueda] = useState('')
const productosFiltrados = productos.filter(p =>
  p.nombre.toLowerCase().includes(busqueda.toLowerCase())
)
```

### 2. Filtros

```jsx
const [filtroCategoria, setFiltroCategoria] = useState('todas')
const productosFiltrados = productos.filter(p =>
  filtroCategoria === 'todas' || p.categoria === filtroCategoria
)
```

### 3. Ordenamiento

```jsx
const [orden, setOrden] = useState('nombre')
const productosOrdenados = [...productos].sort((a, b) => {
  if (orden === 'precio') return a.precio - b.precio
  return a.nombre.localeCompare(b.nombre)
})
```

---

## Conceptos Clave

1. **Context API**: Estado global del carrito
2. **Routing**: Navegación entre páginas
3. **API Integration**: Obtener productos desde backend
4. **State Management**: Manejo de estado del carrito
5. **Componentización**: Componentes reutilizables
6. **Filtros y Búsqueda**: Funcionalidades de UX
7. **Formularios**: Checkout y validación

---

## Buenas Prácticas

- Usa Context API para estado del carrito
- Componentiza para reutilización
- Maneja estados de carga y error
- Implementa validación en formularios
- Optimiza imágenes y assets
- Usa localStorage para persistir carrito
- Implementa manejo de errores

