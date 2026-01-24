# React Fase 1: Fundamentos y Componentización 🧩

## 📋 Índice

1. [Introducción a React](#1-introducción-a-react)
2. [Configuración de Proyecto con Vite](#2-configuración-de-proyecto-con-vite)
3. [El Concepto de Componente](#3-el-concepto-de-componente)
4. [JSX (JavaScript XML)](#4-jsx-javascript-xml)
5. [Props (Propiedades)](#5-props-propiedades)
6. [Estados en React](#6-estados-en-react)
7. [Renderizado de Listas y Keys](#7-renderizado-de-listas-y-keys)
8. [Renderizado Condicional](#8-renderizado-condicional)
9. [Formularios Controlados](#9-formularios-controlados)
10. [Virtual DOM](#10-virtual-dom)
11. [Componentización](#11-componentización)
12. [Estructura de Proyecto React](#12-estructura-de-proyecto-react)
13. [Ejemplos Prácticos del Código Modelo](#13-ejemplos-prácticos-del-código-modelo)

---

## 1. Introducción a React

### ¿Qué es React? (Analogía del Mundo Real)

### 🧩 Analogía: Los Bloques de Construcción LEGO

Imagina que construyes con bloques LEGO:
- **Bloque individual** (Componente): Cada pieza tiene una forma y función específica
- **Construcción completa** (Aplicación): Combinas múltiples bloques para crear algo grande
- **Reutilización**: Usas el mismo tipo de bloque en diferentes lugares
- **Modularidad**: Si un bloque se rompe, solo cambias ese bloque, no toda la construcción

**React funciona igual**: Creas componentes (bloques) que puedes reutilizar y combinar para construir aplicaciones complejas.

### 🏗️ Analogía: La Construcción Modular

Piensa en construir una casa con módulos prefabricados:
- **Módulo** (Componente): Cada habitación es un módulo independiente
- **Casa completa** (Aplicación): Combinas módulos para crear la casa
- **Reutilización**: Puedes usar el mismo tipo de módulo (baño) en diferentes casas
- **Mantenimiento**: Si un módulo tiene problemas, solo reparas ese módulo

**React te permite construir aplicaciones** de la misma manera: módulos (componentes) que se combinan.

### 🎨 Analogía: El Kit de Herramientas de Diseño

Un kit de herramientas de diseño:
- **Herramienta individual** (Componente): Cada herramienta hace algo específico
- **Proyecto completo** (Aplicación): Usas múltiples herramientas para crear algo
- **Reutilización**: Usas la misma herramienta en diferentes proyectos
- **Organización**: Cada herramienta tiene su lugar y función

**React es como ese kit**: Cada componente es una herramienta que puedes usar una y otra vez.

### ¿Qué es React?

**React** es una biblioteca de JavaScript utilizada para construir **interfaces de usuario**, especialmente en aplicaciones **web de una sola página (SPA)**. Su enfoque principal es permitir a los desarrolladores **crear componentes reutilizables** que gestionen su propio estado, facilitando la construcción de interfaces complejas de manera eficiente y **modular**.

**En términos simples**: React es como tener bloques LEGO para construir interfaces web - creas piezas reutilizables (componentes) que puedes combinar para crear aplicaciones completas.

### Características Principales:

- ✅ **Componentes reutilizables**: Piezas de código que encapsulan lógica y presentación
- ✅ **Virtual DOM**: Representación en memoria del DOM real para optimizar actualizaciones
- ✅ **Flujo de datos unidireccional**: Datos fluyen de padre a hijo
- ✅ **Declarativo**: Describes cómo debe verse la UI, React se encarga del cómo
- ✅ **Biblioteca, no framework**: Se enfoca solo en la interfaz de usuario

### ¿Por qué React?

- **Eficiencia**: Actualiza solo las partes necesarias del DOM
- **Reutilización**: Componentes que puedes usar múltiples veces
- **Mantenibilidad**: Código organizado y fácil de mantener
- **Ecosistema**: Gran comunidad y muchas librerías disponibles
- **Popularidad**: Ampliamente usado en la industria

---

## 2. Configuración de Proyecto con Vite

### ¿Qué es Vite?

**Vite** es una herramienta de construcción moderna y rápida para proyectos frontend. Es la forma recomendada de crear proyectos React modernos.

### Crear un Proyecto React con Vite

#### Forma Común (Interactiva):
```bash
npm init vite@latest
```
- Elige el nombre del proyecto
- Selecciona "React" como framework
- Selecciona "JavaScript" (o TypeScript)

#### Forma Pro (Directa):
```bash
npm init vite@latest -- --template react
```

### Comandos Iniciales:

```bash
# 1. Navegar al proyecto
cd nombre-proyecto

# 2. Instalar dependencias
npm install

# 3. Ejecutar en modo desarrollo
npm run dev
```

### Estructura de Carpetas por Defecto:

```
proyecto-react/
├── public/              # Archivos estáticos (accesibles públicamente)
│   └── vite.svg         # Icono de Vite
├── src/                 # Código fuente de la aplicación
│   ├── assets/          # Recursos (imágenes, fuentes, CSS)
│   ├── App.jsx          # Componente principal
│   ├── App.css          # Estilos del componente App
│   ├── main.jsx         # Punto de entrada de la aplicación
│   └── index.css        # Estilos globales
├── .eslintrc.cjs        # Configuración de ESLint (linter)
├── vite.config.js       # Configuración de Vite
├── index.html           # HTML principal
└── package.json         # Dependencias y scripts del proyecto
```

### Estructura Recomendada para Proyectos Grandes:

```
src/
├── components/          # Componentes reutilizables
│   ├── Button.jsx
│   └── Card.jsx
├── pages/              # Páginas/vistas de la aplicación
│   ├── Home.jsx
│   └── About.jsx
├── styles/             # Archivos CSS
│   ├── global.css
│   └── components.css
├── assets/             # Recursos estáticos
│   ├── images/
│   └── fonts/
├── utils/              # Funciones utilitarias
│   └── helpers.js
├── App.jsx             # Componente raíz
└── main.jsx            # Punto de entrada
```

### Diferencias: `public/` vs `src/assets/`

**`public/`**:
- Archivos accesibles públicamente desde el servidor
- Se entregan directamente sin procesamiento
- Ejemplo: `public/logo.png` → accesible como `/logo.png`

**`src/assets/`**:
- Parte del código fuente
- Se procesan y optimizan durante la construcción
- Se importan en el código: `import logo from './assets/logo.png'`
- Mejor para imágenes, fuentes, CSS que se usan en componentes

---

## 3. El Concepto de Componente

### ¿Qué es un Componente?

Un **componente** en React es una pieza de código reutilizable que encapsula lógica y presentación. Los componentes son como "bloques de construcción" que puedes combinar para crear interfaces complejas.

### Ventajas de Componentizar:

- ✅ **Reutilización**: Escribe una vez, usa en muchas partes
- ✅ **Mantenibilidad**: Si hay un error, sabes exactamente en qué archivo buscar
- ✅ **Modularidad**: Capas con responsabilidades únicas (ej: un botón, un formulario, una lista)
- ✅ **Tiempo de trabajo**: No necesitas escribir el mismo código varias veces
- ✅ **Esfuerzo**: Reutilizas código ya probado y funcional
- ✅ **Líneas de código**: Un componente puede reemplazar muchas líneas repetidas
- ✅ **Errores**: Si en un lugar funciona, funciona en el resto
- ✅ **Conflictos con estilos**: Al tener componentes consistentes, los estilos se mantienen uniformes
- ✅ **Facilita las pruebas unitarias**: Componentes aislados son más fáciles de testear
- ✅ **Consistencia en el diseño**: Reutilizar componentes garantiza una interfaz coherente
- ✅ **Optimización del rendimiento**: React actualiza solo los componentes que realmente han cambiado

### Ejemplo Práctico:

**Sin Componentes** (Repetitivo):
```jsx
// ❌ SIN COMPONENTES: Repetir código 4 veces
<button onClick={() => handleOperacion("sumar")}>Sumar</button>
<button onClick={() => handleOperacion("restar")}>Restar</button>
<button onClick={() => handleOperacion("multiplicar")}>Multiplicar</button>
<button onClick={() => handleOperacion("dividir")}>Dividir</button>
```

**Con Componentes** (Reutilizable):
```jsx
// ✅ CON COMPONENTES: Un componente, múltiples usos
<OperationButton operation="sumar" onClick={handleOperacion} />
<OperationButton operation="restar" onClick={handleOperacion} />
<OperationButton operation="multiplicar" onClick={handleOperacion} />
<OperationButton operation="dividir" onClick={handleOperacion} />
```

### ¿Cuándo Componentizar?

**Debes componentizar cuando**:
- ✅ **Exceso de lógica en un solo componente**: Si un componente maneja demasiada lógica
- ✅ **Varias responsabilidades en un componente**: Cuando tiene múltiples funciones o responsabilidades

**NO componentizar cuando**:
- ❌ **Componentes muy pequeños**: No es necesario si tiene pocas líneas o lógica muy simple
- ❌ **Código demasiado acoplado**: Si los componentes dependen fuertemente entre sí

### Consecuencias de NO Componentizar:

- ❌ **Lógica desordenada**: Código confuso y desorganizado
- ❌ **Dificultad para encontrar errores**: Sin estructura clara
- ❌ **Complicaciones al buscar lógica específica**: Encontrar y modificar partes específicas se vuelve difícil
- ❌ **Mantenimiento más costoso**: Requiere más tiempo y recursos
- ❌ **Duplicación de código**: Escribir el mismo código en varios lugares aumenta el riesgo de errores

---

## 4. JSX (JavaScript XML)

### ¿Qué es JSX?

**JSX** (JavaScript XML) es una sintaxis que permite escribir HTML dentro de JavaScript. Es una extensión de JavaScript que se parece a HTML pero tiene capacidades de JavaScript.

### Reglas Fundamentales de JSX:

#### 1. Un Único Elemento Padre

**Regla de Oro**: Siempre debe retornar un único elemento padre.

```jsx
// ❌ INCORRECTO: Múltiples elementos sin padre
function Componente() {
  return (
    <h1>Título</h1>
    <p>Párrafo</p>
  )
}

// ✅ CORRECTO: Un solo elemento padre
function Componente() {
  return (
    <div>
      <h1>Título</h1>
      <p>Párrafo</p>
    </div>
  )
}

// ✅ CORRECTO: Usando Fragment (no crea elemento en el DOM)
function Componente() {
  return (
    <>
      <h1>Título</h1>
      <p>Párrafo</p>
    </>
  )
}
```

#### 2. JavaScript en JSX

Usa llaves `{}` para inyectar variables o lógica JavaScript:

```jsx
function Saludo({ nombre }) {
  const mensaje = "Hola"
  
  return (
    <div>
      <h1>{mensaje}, {nombre}!</h1>
      <p>Tu edad es: {25 + 5}</p>
      {nombre.length > 5 && <p>Tu nombre es largo</p>}
    </div>
  )
}
```

#### 3. Atributos Especiales

Algunos atributos HTML cambian en JSX:

- `class` → `className`
- `for` → `htmlFor`
- `onclick` → `onClick` (camelCase)
- `style` → objeto JavaScript: `style={{ color: 'red' }}`

```jsx
// ❌ INCORRECTO
<div class="container" onclick={handleClick}>
  <label for="input1">Nombre</label>
</div>

// ✅ CORRECTO
<div className="container" onClick={handleClick}>
  <label htmlFor="input1">Nombre</label>
</div>
```

#### 4. Comentarios en JSX

```jsx
function Componente() {
  return (
    <div>
      {/* Este es un comentario en JSX */}
      <h1>Título</h1>
    </div>
  )
}
```

---

## 5. Props (Propiedades)

### ¿Qué es una Prop?

Una **prop** (propiedad) es:
- ✅ **Valores**: Datos que deben viajar de un componente a otro
- ✅ **Contenido**: Información que se pasa entre componentes
- ✅ **Datos**: Cualquier tipo de dato que necesite ser compartido

### ¿Qué estructuras de datos pueden viajar por prop?

Las props pueden ser de **cualquier tipo de dato**:
- ✅ **Objeto**: `{nombre: "Juan", edad: 25}`
- ✅ **Array**: `[1, 2, 3, 4]`
- ✅ **Variables**: Strings, números, booleanos
- ✅ **Funciones**: Callbacks que permiten comunicación hijo → padre

### ¿Qué rol cumple en un componente?

La prop en un componente:
- ✅ **Funciona como un parámetro**: Recibe datos desde el componente padre
- ✅ **Opera sobre esa información**: El componente usa los datos para generar su resultado
- ✅ **Usa la información para dar un resultado**: Procesa los datos y los muestra o utiliza

### Características de las Props:

- ✅ **Inmutables (read-only)**: El hijo solo las lee, no las modifica
- ✅ **Flujo unidireccional**: Datos fluyen de padre → hijo
- ✅ **Pueden ser cualquier tipo de dato**: string, number, boolean, array, object, function

### Desestructuración (Sintaxis Pro)

En lugar de recibir `props` y usar `props.nombre`, desestructuramos en los parámetros:

```jsx
// ❌ Forma tradicional
function Usuario(props) {
  return <h1>Hola, {props.nombre}, tienes {props.edad} años.</h1>
}

// ✅ Forma con desestructuración (Recomendada)
function Usuario({ nombre, edad }) {
  return <h1>Hola, {nombre}, tienes {edad} años.</h1>
}
```

### Ejemplo Completo de Props:

```jsx
// Componente padre pasa datos (props)
function App() {
  const nombre = "Juan"
  const edad = 25
  
  const handleClick = () => {
    console.log("Click!")
  }
  
  return (
    <Usuario 
      nombre={nombre}        // Prop tipo string (variable)
      edad={edad}            // Prop tipo number (variable)
      onClick={handleClick}  // Prop tipo función (callback)
    />
  )
}

// Componente hijo recibe y usa los datos
function Usuario({ nombre, edad, onClick }) {
  // nombre, edad, onClick son parámetros (props)
  // El componente opera sobre esa información
  return (
    <div>
      <h1>Hola, {nombre}!</h1>
      <p>Tienes {edad} años</p>
      <button onClick={onClick}>Hacer clic</button>
    </div>
  )
}
```

### PropTypes (Validación de Props)

**PropTypes** permite validar los tipos de datos de las props en desarrollo:

```jsx
import PropTypes from 'prop-types'

function Usuario({ nombre, edad, onClick }) {
  return (
    <div>
      <h1>{nombre}</h1>
      <p>{edad}</p>
      <button onClick={onClick}>Click</button>
    </div>
  )
}

// Validación de props
Usuario.propTypes = {
  nombre: PropTypes.string.isRequired,      // Requerida: string
  edad: PropTypes.number.isRequired,        // Requerida: number
  onClick: PropTypes.func.isRequired,        // Requerida: función
  email: PropTypes.string                   // Opcional: string
}

// Valores por defecto
Usuario.defaultProps = {
  email: 'sin-email@ejemplo.com'
}
```

**Ventajas de PropTypes**:
- ✅ **Validación en desarrollo**: React verifica que las props sean del tipo correcto
- ✅ **Props requeridas vs opcionales**: `isRequired` marca obligatorias
- ✅ **Detección de errores**: En desarrollo, React avisa si pasas props incorrectas
- ✅ **Documentación**: Sirve como documentación del componente

### Flujo de Datos: Props y Callbacks

Las props permiten comunicación en ambas direcciones:

**Padre → Hijo** (Datos):
```jsx
// El padre pasa datos al hijo
<Usuario nombre="Juan" edad={25} />
```

**Hijo → Padre** (Eventos):
```jsx
// El padre pasa una función al hijo
function App() {
  const handleClick = () => {
    console.log("El hijo hizo clic")
  }
  
  return <Boton onClick={handleClick} />
}

// El hijo ejecuta la función del padre
function Boton({ onClick }) {
  return <button onClick={onClick}>Hacer clic</button>
}
```

---

## 6. Estados en React

### Concepto de Estados: Dual (Binario) vs Relativos

Antes de entender `useState`, es importante entender qué son los estados:

#### Estados Duales (Binarios):
Estados que tienen dos valores opuestos:
- ✅ **Estable - Alterado**
- ✅ **Prendido - Apagado**
- ✅ **Trabajo - Reposo**
- ✅ **Frío - Caliente**
- ✅ **Luz - Oscuridad**
- ✅ **Despierto - Dormido**
- ✅ **Activo - Inactivo**
- ✅ **Líquido - Sólido**
- ✅ **Victoria - Derrota**
- ✅ **Felicidad - Sufrimiento**

#### Estados Relativos:
Estados que tienen valores graduales o medibles:
- ✅ **No está vacío ni lleno** (porcentaje)
- ✅ **0km → 100km** (longitud)
- ✅ **1 litro** (volumen)
- ✅ **36°C** (temperatura)

### Estados en React: La Analogía de la Canilla

La API de React es como una **canilla** que puedes usar para llenar un **contenedor** (array, objetos, variables).

#### ¿Qué hago para llenar de agua el contenedor?

1. **El contenedor tiene un estado inicial por defecto (vacío)**
   - Cuando creas un estado, comienza con un valor inicial (vacío, 0, null, etc.)

2. **Colocar el contenido en el contenedor (almacenarlo para usarlo)**
   - Usas la función `set` para "llenar" el contenedor con datos

### useState → Hook

`useState` es un **hook** que nos permite:

- ✅ **Crear el recipiente**: Define una variable donde guardaremos los datos
- ✅ **Almacenar contenido en el recipiente**: Proporciona una función para "llenar" el recipiente

#### Estructura de useState:

```jsx
const [recipiente, setRecipiente] = useState(valorInicial);
```

- **Primer parámetro** (`recipiente`): El recipiente, una variable donde guardaremos los datos
- **Segundo parámetro** (`setRecipiente`): Es una función que permite llenar tu recipiente
- **useState(valorInicial)**: El estado inicial puede ser `null`, `[]`, `false`, `""`, `0` - ya que un recipiente nuevo siempre está vacío

#### Ejemplo Básico:

```jsx
import { useState } from 'react'

function Contador() {
  // Crear el recipiente con valor inicial 0
  const [count, setCount] = useState(0)
  
  const incrementar = () => {
    // Llenar el recipiente con un nuevo valor
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

#### Ejemplo con Diferentes Tipos:

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

### Destructuración en useState

Cuando llamas a `useState`, React devuelve un array con dos elementos:

1. **El valor actual del estado**: Este valor es lo que quieres mantener y utilizar dentro de tu componente
2. **Una función para actualizar ese estado**: Esta función permite cambiar el valor del estado, lo que provoca un re-renderizado del componente

```jsx
// useState devuelve un array: [valor, función]
const [users, setUsers] = useState([])

// Es equivalente a:
const estado = useState([])
const users = estado[0]      // Valor actual
const setUsers = estado[1]    // Función para actualizar
```

**Ejemplo de destructuración similar en JavaScript**:
```javascript
const fruits = ['apple', 'banana', 'cherry']
const [firstFruit, secondFruit] = fruits

console.log(firstFruit)   // 'apple'
console.log(secondFruit)  // 'banana'
```

### Actualización de Estado

#### Actualización Directa:
```jsx
const [count, setCount] = useState(0)

// Actualizar directamente
setCount(5)  // count ahora es 5
```

#### Actualización Basada en Valor Anterior (Recomendado):
```jsx
const [count, setCount] = useState(0)

// Usar función callback cuando el nuevo valor depende del anterior
setCount(prevCount => prevCount + 1)  // Incrementar
```

**¿Por qué usar función callback?**
- ✅ **Evita problemas de closure**: Garantiza que usas el valor más reciente
- ✅ **Mejor para actualizaciones múltiples**: Si actualizas el estado varias veces seguidas
- ✅ **Recomendado por React**: Es la forma recomendada cuando el nuevo valor depende del anterior

### Estado vs Variable Simple

| Característica | Variable Simple | Estado (useState) |
|:---|:---|:---|
| **Persistencia** | Se reinicia en cada renderizado | Persiste entre renderizados |
| **Re-renderizado** | No avisa a React que debe pintar de nuevo | Notifica a React para actualizar la interfaz |
| **Reactividad** | Cambiar el valor no actualiza la UI | Cambiar el estado actualiza automáticamente |
| **Uso** | Para valores que no afectan la UI | Para valores que afectan la UI |

**Ejemplo de la Diferencia**:

```jsx
function Componente() {
  // Variable simple
  let count = 0
  
  // Estado
  const [estadoCount, setEstadoCount] = useState(0)
  
  const incrementarVariable = () => {
    count = count + 1
    console.log(count)  // Se incrementa en consola
    // PERO: La UI NO se actualiza
  }
  
  const incrementarEstado = () => {
    setEstadoCount(estadoCount + 1)
    // La UI SÍ se actualiza automáticamente
  }
  
  return (
    <div>
      <p>Variable: {count}</p>  {/* No cambia en la UI */}
      <p>Estado: {estadoCount}</p>  {/* Sí cambia en la UI */}
      <button onClick={incrementarVariable}>Incrementar Variable</button>
      <button onClick={incrementarEstado}>Incrementar Estado</button>
    </div>
  )
}
```

### Beneficios de useState:

- ✅ **Persistencia del estado**: El estado persiste a través de re-renderizados
- ✅ **Re-renderizado automático**: Cuando el estado cambia, React actualiza la UI automáticamente
- ✅ **Sincronización con el ciclo de vida**: Trabaja de manera sincrónica con el ciclo de vida del componente
- ✅ **Optimizaciones internas**: React realiza optimizaciones para actualizar solo lo necesario

---

## 7. Renderizado de Listas y Keys

### Renderizar Listas con `.map()`

Cuando necesitas renderizar múltiples elementos similares, usas el método `.map()` de JavaScript:

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

Cuando usas `.map()` para crear elementos, React necesita una **`key`** única para cada uno por rendimiento.

#### ¿Por qué son importantes las Keys?

- ✅ **Identificación única**: React usa las keys para identificar qué elementos han cambiado
- ✅ **Optimización del rendimiento**: Permite a React actualizar solo los elementos que cambiaron
- ✅ **Mantenimiento del estado**: Preserva el estado de los componentes cuando la lista cambia

#### Reglas de Keys:

- ✅ **Usar IDs únicos**: Cuando sea posible, usar IDs únicos de la base de datos
- ✅ **Keys deben ser estables**: No deben cambiar entre renders
- ✅ **Keys deben ser únicas**: Entre hermanos (elementos del mismo nivel)
- ❌ **NO usar índices**: Si la lista puede cambiar de orden
- ❌ **NO usar valores aleatorios**: Como `Math.random()` o `Date.now()`

#### Ejemplos:

```jsx
// ✅ CORRECTO: Usar ID único
const items = [
  { id: 1, nombre: "Item 1" },
  { id: 2, nombre: "Item 2" }
]

return (
  <ul>
    {items.map(item => (
      <li key={item.id}>{item.nombre}</li>
    ))}
  </ul>
)

// ⚠️ ACEPTABLE: Usar índice solo si la lista NO cambia de orden
const items = ["Item 1", "Item 2"]

return (
  <ul>
    {items.map((item, index) => (
      <li key={index}>{item}</li>
    ))}
  </ul>
)

// ❌ INCORRECTO: Usar índice cuando la lista puede cambiar
function ListaTareas({ tareas }) {
  return (
    <ul>
      {tareas.map((tarea, index) => (
        <li key={index}>{tarea.texto}</li>  // ❌ Problema si se elimina/agrega tarea
      ))}
    </ul>
  )
}
```

### Renderizado de Arrays de Objetos

```jsx
function ListaUsuarios() {
  const usuarios = [
    { id: 1, nombre: "Juan", edad: 25 },
    { id: 2, nombre: "María", edad: 30 },
    { id: 3, nombre: "Pedro", edad: 28 }
  ]
  
  return (
    <div>
      {usuarios.map(usuario => (
        <div key={usuario.id}>
          <h3>{usuario.nombre}</h3>
          <p>Edad: {usuario.edad}</p>
        </div>
      ))}
    </div>
  )
}
```

---

## 8. Renderizado Condicional

Puedes decidir qué mostrar usando operadores lógicos o condicionales.

### Operador Ternario

```jsx
function Saludo({ usuario }) {
  return (
    <div>
      {usuario ? (
        <h1>Hola, {usuario.nombre}!</h1>
      ) : (
        <h1>Hola, invitado!</h1>
      )}
    </div>
  )
}
```

### Operador && (AND Lógico)

```jsx
function Componente({ mostrarMensaje }) {
  return (
    <div>
      {mostrarMensaje && <p>Este mensaje se muestra si mostrarMensaje es true</p>}
    </div>
  )
}
```

**Importante**: El operador `&&` retorna el segundo valor si el primero es `true`, o el primer valor si es `false`. Si el primer valor es `0` o `""`, se mostrará en la UI. Para evitar esto:

```jsx
// ⚠️ PROBLEMA: Si count es 0, se muestra "0" en la UI
{count && <p>Hay {count} items</p>}

// ✅ SOLUCIÓN: Convertir a booleano explícitamente
{count > 0 && <p>Hay {count} items</p>}
{!!count && <p>Hay {count} items</p>}
```

### Múltiples Condiciones

```jsx
function Mensaje({ tipo }) {
  if (tipo === 'error') {
    return <p className="error">Error!</p>
  } else if (tipo === 'advertencia') {
    return <p className="warning">Advertencia!</p>
  } else {
    return <p className="info">Información</p>
  }
}
```

### Renderizado Condicional con Variables

```jsx
function Componente({ usuario }) {
  let contenido
  
  if (usuario) {
    contenido = <PerfilUsuario usuario={usuario} />
  } else {
    contenido = <Login />
  }
  
  return <div>{contenido}</div>
}
```

---

## 9. Formularios Controlados

### ¿Qué es un Formulario Controlado?

Un **formulario controlado** es aquel donde el valor del input viene del estado de React. React controla completamente el input.

### Características:

- ✅ **El valor siempre viene del estado**: `value={estado}`
- ✅ **React controla el input completamente**: `onChange` actualiza el estado
- ✅ **Permite validación y transformación**: Puedes validar o transformar antes de guardar
- ✅ **Única fuente de verdad**: El estado es la única fuente de verdad

### Ejemplo Básico:

```jsx
function Formulario() {
  const [nombre, setNombre] = useState('')
  
  const handleChange = (e) => {
    setNombre(e.target.value)  // Actualizar estado con el valor del input
  }
  
  const handleSubmit = (e) => {
    e.preventDefault()
    console.log('Nombre:', nombre)
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text"
        value={nombre}              // Valor viene del estado
        onChange={handleChange}     // Actualizar estado cuando cambia
      />
      <button type="submit">Enviar</button>
    </form>
  )
}
```

### Formulario Controlado vs No Controlado

**Formulario Controlado** (Recomendado):
```jsx
const [valor, setValor] = useState('')

<input 
  value={valor}                    // Controlado por estado
  onChange={(e) => setValor(e.target.value)} 
/>
```

**Formulario No Controlado** (No recomendado en React):
```jsx
<input 
  defaultValue="valor inicial"     // No controlado
  // React no controla el valor
/>
```

### Múltiples Inputs Controlados:

```jsx
function FormularioCompleto() {
  const [formData, setFormData] = useState({
    nombre: '',
    email: '',
    edad: 0
  })
  
  const handleChange = (e) => {
    const { name, value } = e.target
    setFormData(prev => ({
      ...prev,
      [name]: value
    }))
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
        type="email"
        value={formData.email}
        onChange={handleChange}
      />
      <input
        name="edad"
        type="number"
        value={formData.edad}
        onChange={handleChange}
      />
    </form>
  )
}
```

### Validación en Formularios Controlados:

```jsx
function FormularioValidado() {
  const [email, setEmail] = useState('')
  const [error, setError] = useState('')
  
  const handleChange = (e) => {
    const valor = e.target.value
    setEmail(valor)
    
    // Validación en tiempo real
    if (valor && !valor.includes('@')) {
      setError('El email debe contener @')
    } else {
      setError('')
    }
  }
  
  return (
    <form>
      <input
        type="email"
        value={email}
        onChange={handleChange}
      />
      {error && <p className="error">{error}</p>}
    </form>
  )
}
```

---

## 10. Virtual DOM

### ¿Qué es el DOM?

El **DOM (Document Object Model)** es una representación estructurada del documento HTML en forma de árbol, donde cada elemento es un nodo. Manipular el DOM directamente **puede ser costoso en términos de rendimiento**, ya que cada cambio implica un re-renderizado de parte o toda la página.

### ¿Qué es el Virtual DOM?

El **Virtual DOM** es una **representación en memoria** del DOM real. Cuando se produce un cambio en un componente de React, **este se actualiza primero en el Virtual DOM**. React compara esta nueva versión del Virtual DOM con la anterior (un proceso llamado **"reconciliación"**) para identificar las diferencias, **aplicando sólo los cambios necesarios al DOM real**. Esto optimiza las actualizaciones y mejora la eficiencia de la aplicación.

### ¿Cómo Funciona el Virtual DOM?

1. **Renderizado Inicial**: React crea una representación virtual del DOM
2. **Cambio de Estado**: Cuando el estado cambia, React crea una nueva versión del Virtual DOM
3. **Comparación (Diffing)**: React compara el Virtual DOM anterior con el nuevo
4. **Actualización Selectiva**: React actualiza solo las partes del DOM real que cambiaron

### Ventajas del Virtual DOM:

- ✅ **Optimización del rendimiento**: Actualiza solo lo necesario
- ✅ **Eficiencia**: Evita re-renderizados completos de la página
- ✅ **Automatización**: React maneja las optimizaciones automáticamente
- ✅ **Mejor experiencia de usuario**: Interfaces más rápidas y fluidas

### useCallback y Optimización del Virtual DOM

`useCallback` memoriza funciones para evitar que se recreen en cada render. Esto es importante porque si pasamos funciones nuevas como props, React pensará que las props cambiaron y re-renderizará componentes innecesariamente.

```jsx
import { useState, useCallback } from 'react'

function App() {
  const [num1, setNum1] = useState(0)
  
  // Sin useCallback: Se crea una nueva función en cada render
  // const handleChange = (e) => { setNum1(Number(e.target.value)) }
  
  // Con useCallback: La función se memoriza y solo cambia si las dependencias cambian
  const handleChange = useCallback((e) => {
    setNum1(Number(e.target.value))
  }, [])  // Array vacío = la función nunca cambia
  
  return <InputNumber onChange={handleChange} />
}
```

**Beneficios**:
- ✅ **Evita re-renders innecesarios**: El componente hijo no se re-renderiza si la función no cambió
- ✅ **Optimización del Virtual DOM**: React puede optimizar mejor las actualizaciones
- ✅ **Mejor rendimiento**: Especialmente importante en listas grandes

---

## 11. Componentización

### ¿Por qué Componentizar?

Componentizar tiene múltiples beneficios:

- ✅ **Reutilización de componentes**: Permite volver a usar componentes en diferentes partes de la aplicación, reduciendo la duplicación de código
- ✅ **Reducción de código repetitivo**: Evita la necesidad de escribir el mismo código en múltiples lugares, ahorrando tiempo y minimizando errores
- ✅ **Facilita la detección y corrección de errores**: Al tener componentes bien definidos, es más sencillo identificar y corregir errores específicos
- ✅ **Mejora la mantenibilidad**: Hace que la aplicación sea más fácil de mantener, ya que cada componente tiene una responsabilidad clara
- ✅ **Capas con diferentes responsabilidades**: Permite crear componentes con finalidades específicas y bien definidas
- ✅ **Escalabilidad**: Facilita la expansión del proyecto, permitiendo agregar nuevas funcionalidades sin complicaciones
- ✅ **Modularidad**: Cada componente actúa como una unidad funcional independiente
- ✅ **Facilita las pruebas unitarias**: Componentes aislados son más fáciles de testear
- ✅ **Consistencia en el diseño**: Reutilizar componentes garantiza una interfaz de usuario coherente
- ✅ **Optimización del rendimiento**: Componentizar ayuda a React a optimizar la renderización, actualizando solo los componentes que realmente han cambiado

### Composición de Componentes

Los componentes se pueden combinar para crear interfaces más complejas:

```jsx
// Componentes pequeños y reutilizables
function Boton({ texto, onClick }) {
  return <button onClick={onClick}>{texto}</button>
}

function Input({ valor, onChange }) {
  return <input value={valor} onChange={onChange} />
}

// Componente que compone otros componentes
function Formulario() {
  const [nombre, setNombre] = useState('')
  
  return (
    <form>
      <Input valor={nombre} onChange={(e) => setNombre(e.target.value)} />
      <Boton texto="Enviar" onClick={() => console.log(nombre)} />
    </form>
  )
}
```

---

## 12. Estructura de Proyecto React

### Estructura Básica (Vite):

```
proyecto-react/
├── public/              # Archivos estáticos
│   └── vite.svg
├── src/                 # Código fuente
│   ├── assets/          # Recursos (imágenes, fuentes, CSS)
│   ├── App.jsx          # Componente principal
│   ├── App.css          # Estilos del componente App
│   ├── main.jsx         # Punto de entrada
│   └── index.css        # Estilos globales
├── .eslintrc.cjs        # Configuración de ESLint
├── vite.config.js       # Configuración de Vite
├── index.html           # HTML principal
└── package.json         # Dependencias y scripts
```

### Estructura Recomendada para Proyectos Grandes:

```
src/
├── components/          # Componentes reutilizables
│   ├── Button/
│   │   ├── Button.jsx
│   │   └── Button.css
│   └── Card/
│       ├── Card.jsx
│       └── Card.css
├── pages/              # Páginas/vistas de la aplicación
│   ├── Home.jsx
│   └── About.jsx
├── styles/             # Archivos CSS globales
│   ├── global.css
│   └── variables.css
├── assets/             # Recursos estáticos
│   ├── images/
│   └── fonts/
├── utils/              # Funciones utilitarias
│   └── helpers.js
├── hooks/              # Hooks personalizados
│   └── useCustomHook.js
├── App.jsx             # Componente raíz
└── main.jsx            # Punto de entrada
```

### Archivos Importantes:

**`.eslintrc.cjs`**: Configuración de ESLint (linter)
- Ayuda a detectar errores y problemas en el código
- Sigue reglas personalizadas
- Ofrece sugerencias en tiempo real

**`vite.config.js`**: Configuración de Vite
- Permite ajustes en el servidor de desarrollo
- Configuración de construcción
- Complementos y variables de entorno

**`package.json`**: Dependencias y scripts
- Lista todas las dependencias del proyecto
- Scripts disponibles (`npm run dev`, `npm run build`, etc.)

---

## 13. Ejemplos Prácticos del Código Modelo

### Ejemplo 01: Básico - Cambio de Contenido

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-17-introduccion-react/ejemplo-01-basico`

**Conceptos**:
- Componente funcional básico
- Hook `useState` simple
- Eventos `onClick`
- JSX básico

**Código**:
```jsx
import { useState } from 'react'

function App() {
  const [contenido, setContenido] = useState('Hola, soy el contenido inicial')
  
  const cambiarContenido = () => {
    setContenido('Hola, soy contenido nuevo')
  }
  
  return (
    <div>
      <h1>Ejemplo react</h1>
      <p>{contenido}</p>
      <button onClick={cambiarContenido}>Cambiar contenido</button>
    </div>
  )
}
```

### Ejemplo 02: Calculadora Completa con Componentes

**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-18-react-componentes-props-formularios/ejemplo-01-calculadora-completa`

**Conceptos**:
- Componentes funcionales reutilizables
- Props (string, number, function)
- PropTypes y defaultProps
- Formularios controlados
- Virtual DOM y optimización con useCallback
- Composición de componentes
- Renderizado condicional

**Estructura**:
```
src/
├── components/
│   ├── InputNumber.jsx      # Componente reutilizable de input
│   ├── OperationButton.jsx # Componente reutilizable de botón
│   └── ResultadoDisplay.jsx # Componente de presentación
├── utils/
│   └── mathOperations.js   # Funciones utilitarias
└── App.jsx                 # Componente principal
```

**Código Principal**:
```jsx
import { useState, useCallback } from 'react'
import InputNumber from './components/InputNumber'
import OperationButton from './components/OperationButton'
import ResultadoDisplay from './components/ResultadoDisplay'

function App() {
  const [num1, setNum1] = useState(0)
  const [num2, setNum2] = useState(0)
  const [resultado, setResultado] = useState(0)
  
  const handleNum1Change = useCallback((evento) => {
    setNum1(Number(evento.target.value))
  }, [])
  
  const handleOperacion = useCallback((operacion) => {
    switch (operacion) {
      case "sumar":
        setResultado(num1 + num2)
        break
      // ... más casos
    }
  }, [num1, num2])
  
  return (
    <div className='calculator-wrapper'>
      <InputNumber
        label="Numero 1"
        value={num1}
        onChange={handleNum1Change}
      />
      <InputNumber
        label="Numero 2"
        value={num2}
        onChange={handleNum2Change}
      />
      <OperationButton operation="sumar" onClick={handleOperacion} />
      <ResultadoDisplay resultado={resultado} />
    </div>
  )
}
```

---

## 📚 Índice por Temas del Código Modelo

### Tema 17: Introducción a React
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-17-introduccion-react`

**Conceptos cubiertos**:
- ✅ Componentes funcionales básicos
- ✅ Hook `useState` para manejar estado
- ✅ Hook `useEffect` para efectos secundarios
- ✅ Integración con localStorage
- ✅ Estructura de proyectos Vite + React
- ✅ Props básicas entre componentes

**Ejemplos incluidos**:
1. **Ejemplo 01**: Básico - Cambio de Contenido
2. **Ejemplo 02**: Like Counter con localStorage
3. **Ejemplo 03**: Componentes Separados
4. **Ejemplo 04**: Calculadora
5. **Ejemplo 05**: Todo List (Lista de Tareas)

### Tema 18: Componentes, Props y Formularios Controlados
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-18-react-componentes-props-formularios`

**Conceptos cubiertos**:
- ✅ Componentes funcionales reutilizables
- ✅ Props (string, number, function)
- ✅ PropTypes y defaultProps
- ✅ Formularios controlados
- ✅ Virtual DOM y optimización con useCallback
- ✅ Composición de componentes
- ✅ Renderizado condicional

**Ejemplos incluidos**:
1. **Ejemplo 01**: Calculadora Completa (con material teórico extenso)
2. **Ejemplo 02**: Greeting con Props

**Material teórico disponible**:
- `MATERIAL_TEORICO_REACT.md`: Más de 800 líneas explicando cada concepto
- `GUIA_RAPIDA_CONCEPTOS.md`: Mapa de conceptos con ubicaciones exactas

---

## 🎯 Resumen de Conceptos Clave

### Componentes
- Son funciones de JavaScript que retornan JSX
- Permiten reutilización de código
- Encapsulan lógica y presentación

### Props
- Datos que viajan de componente padre a hijo
- Son inmutables (read-only)
- Pueden ser cualquier tipo de dato (string, number, object, array, function)

### useState
- Hook para manejar estado en componentes funcionales
- Retorna `[valor, setValor]`
- Cada actualización causa un re-render

### JSX
- Sintaxis que permite escribir HTML en JavaScript
- Debe retornar un único elemento padre
- Usa `{}` para inyectar JavaScript

### Virtual DOM
- Representación en memoria del DOM real
- React compara y actualiza solo lo necesario
- Optimiza el rendimiento automáticamente

### Formularios Controlados
- El valor del input viene del estado
- React controla completamente el input
- Permite validación y transformación

---

## 📝 Buenas Prácticas

1. **Siempre usar keys únicas** en listas (no índices si la lista cambia)
2. **Usar PropTypes** para validar props en desarrollo
3. **Componentizar** cuando hay lógica repetida o múltiples responsabilidades
4. **Formularios controlados** en lugar de no controlados
5. **useCallback** para funciones que se pasan como props
6. **Desestructuración** de props en los parámetros
7. **Un solo elemento padre** en el return de JSX
8. **Estado inmutable**: Nunca mutar estado directamente, siempre usar setter

---

## 🚀 Próximos Pasos

Después de dominar estos conceptos, continúa con:
- **React Fase 2**: Hooks avanzados (`useEffect`, `useReducer`, hooks personalizados)
- **React Fase 3**: Routing y consumo de APIs
- **React Fase 4**: Estado global (Context API, Redux)

---

**Referencias del Código Modelo**:
- `cursadas/frontend/frontEnd_modelo/tema-17-introduccion-react/`
- `cursadas/frontend/frontEnd_modelo/tema-18-react-componentes-props-formularios/`
