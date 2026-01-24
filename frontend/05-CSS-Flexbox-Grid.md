# CSS: Flexbox y Grid 🎯

## 📑 Índice

1. [¿Qué son Flexbox y Grid? (Analogía del Mundo Real)](#qué-son-flexbox-y-grid-analogía-del-mundo-real)
2. [Flexbox (Diseño Unidimensional)](#flexbox-diseño-unidimensional-)
3. [Grid (Diseño Bidimensional)](#grid-diseño-bidimensional-)
4. [¿Cuándo Usar Cada Uno?](#cuándo-usar-cada-uno)
5. [Combinando Flexbox y Grid](#combinando-flexbox-y-grid)
6. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué son Flexbox y Grid? (Analogía del Mundo Real)

### 📦 Analogía: Organizar Objetos en una Caja

**Flexbox** - Como organizar objetos en una fila o columna:
- Imagina una caja donde pones objetos uno al lado del otro (fila) o uno encima del otro (columna)
- Puedes centrarlos, espaciarlos uniformemente, alinearlos
- **Una dimensión**: Solo fila O columna a la vez

**Grid** - Como organizar objetos en una cuadrícula:
- Imagina una cuadrícula como un tablero de ajedrez
- Puedes colocar objetos en cualquier celda
- **Dos dimensiones**: Filas Y columnas simultáneamente

### 🏠 Analogía: Organizar Muebles

**Flexbox** - Como organizar muebles en una pared:
- Pones los muebles en una fila (horizontal) o columna (vertical)
- Puedes centrarlos, espaciarlos, alinearlos
- Ideal para: navegación, botones en fila, elementos apilados

**Grid** - Como organizar muebles en una habitación:
- Tienes una cuadrícula completa (paredes, espacios)
- Puedes colocar muebles en cualquier posición
- Ideal para: layouts completos de página, galerías, dashboards

### 📐 Analogía: El Organizador de Escritorio

**Flexbox** - Como un organizador de lápices:
- Los lápices están en una fila o columna
- Puedes espaciarlos uniformemente
- Una dimensión

**Grid** - Como un organizador de escritorio con compartimentos:
- Tienes una cuadrícula de compartimentos
- Puedes colocar cosas en cualquier compartimento
- Dos dimensiones

---

## Flexbox (Diseño Unidimensional) ↔️↕️

Ideal para alinear elementos en una sola fila o columna. Se activa en el **contenedor padre**.

### Propiedades del Contenedor (`display: flex`)

```css
.container {
  display: flex;
  
  /* Dirección */
  flex-direction: row; /* row, row-reverse, column, column-reverse */
  
  /* Alineación en eje principal (horizontal si row) */
  justify-content: center; 
  /* flex-start, flex-end, center, space-between, space-around, space-evenly */
  
  /* Alineación en eje transversal (vertical si row) */
  align-items: center;
  /* flex-start, flex-end, center, stretch, baseline */
  
  /* Envolver elementos */
  flex-wrap: wrap; /* nowrap, wrap, wrap-reverse */
  
  /* Alineación de líneas múltiples */
  align-content: space-between;
}
```

### Propiedades de los Hijos

```css
.item {
  /* Capacidad de crecer */
  flex-grow: 1; /* 0 = no crece, 1+ = crece proporcionalmente */
  
  /* Capacidad de encogerse */
  flex-shrink: 1; /* 0 = no se encoge */
  
  /* Tamaño base */
  flex-basis: 200px; /* Tamaño inicial antes de distribuir espacio */
  
  /* Atajo */
  flex: 1 1 200px; /* grow shrink basis */
  
  /* Orden visual */
  order: 2; /* Cambia el orden sin tocar HTML */
  
  /* Alineación individual */
  align-self: flex-end; /* Sobrescribe align-items del padre */
}
```

**Casos de uso comunes**:
- Navegación horizontal
- Centrar elementos (vertical y horizontal)
- Distribuir espacio uniformemente
- Cards en fila
- Footer pegajoso

### 🎯 Analogía: Centrar un Objeto

**Flexbox para centrar** - Como centrar un cuadro en una pared:
```css
.container {
  display: flex;
  justify-content: center;  /* Centra horizontalmente */
  align-items: center;       /* Centra verticalmente */
}
```

**Es como tener un sistema de guías** que automáticamente centra el objeto en ambas direcciones.

---

## ¿Cuándo Usar Cada Uno?

### ✅ Usa Flexbox cuando:
- Necesitas alinear elementos en una fila o columna
- Quieres centrar contenido
- Necesitas distribuir espacio uniformemente
- Trabajas con componentes pequeños (botones, navegación)

**Analogía**: Como organizar objetos en una estantería (una fila o columna).

### ✅ Usa Grid cuando:
- Necesitas un layout completo de página
- Quieres controlar filas Y columnas simultáneamente
- Necesitas áreas nombradas
- Trabajas con estructuras complejas

**Analogía**: Como diseñar el plano completo de una casa (múltiples habitaciones en una cuadrícula).

### 🔄 Combinando Ambos

**Puedes usar ambos juntos**:
- **Grid** para el layout general de la página
- **Flexbox** para componentes dentro de las celdas del grid

**Analogía**: Como tener una casa (Grid) y dentro de cada habitación organizar los muebles con Flexbox.

---

## Combinando Flexbox y Grid

Puedes usar ambos juntos para crear layouts complejos:

```css
/* Grid para el layout general */
.page {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}

/* Flexbox para componentes dentro */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

**Analogía**: Como tener una casa (Grid) y dentro de cada habitación organizar los muebles con Flexbox.

---

## Grid (Diseño Bidimensional) 🗺️

Pensado para estructuras completas de página (filas y columnas simultáneamente).

### 🗺️ Analogía: El Mapa de una Ciudad

Imagina el mapa de una ciudad con calles y manzanas:
- **Grid**: Es como las calles que forman una cuadrícula
- **Celdas**: Cada manzana es una celda donde puedes colocar algo
- **Áreas**: Puedes nombrar áreas (centro, barrio norte, etc.)

### 📐 Analogía: El Tablero de Ajedrez

Un tablero de ajedrez:
- **Grid**: La cuadrícula de 8x8
- **Celdas**: Cada casilla donde puedes colocar una pieza
- **Posicionamiento**: Puedes colocar piezas en cualquier casilla

### 🏗️ Analogía: El Plano Arquitectónico

Un plano de construcción:
- **Grid**: La cuadrícula de referencia
- **Áreas**: Habitaciones, cocina, baño (áreas nombradas)
- **Layout**: La estructura completa de la casa

### Propiedades del Contenedor

```css
.container {
  display: grid;
  
  /* Definir columnas */
  grid-template-columns: 250px 1fr 2fr;
  /* 250px fija, 1fr flexible, 2fr doble de flexible */
  
  grid-template-columns: repeat(3, 1fr); /* 3 columnas iguales */
  grid-template-columns: 1fr 2fr 1fr; /* Proporción 1:2:1 */
  
  /* Definir filas */
  grid-template-rows: auto 1fr auto; /* Header, Body, Footer */
  grid-template-rows: repeat(4, 100px); /* 4 filas de 100px */
  
  /* Espacio entre celdas */
  gap: 20px; /* Espacio entre filas y columnas */
  row-gap: 10px;
  column-gap: 15px;
  
  /* Áreas nombradas */
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
}
```

### Propiedades de los Hijos

```css
.item {
  /* Posición en columnas */
  grid-column: 1 / 3; /* Desde columna 1 hasta 3 */
  grid-column: span 2; /* Ocupa 2 columnas */
  
  /* Posición en filas */
  grid-row: 2 / 4; /* Desde fila 2 hasta 4 */
  grid-row: span 3; /* Ocupa 3 filas */
  
  /* Área nombrada */
  grid-area: header; /* Usa área definida en grid-template-areas */
}
```

**Unidad `fr` (Fraction)**:
- Ocupa una porción del espacio disponible
- `1fr` = 1 parte, `2fr` = 2 partes (el doble)

**Casos de uso comunes**:
- Layouts de página completos
- Galerías de imágenes
- Dashboards
- Tablas complejas
- Grids de productos

---

## Flexbox vs Grid

- **Flexbox**: Una dimensión (fila O columna) - Componentes pequeños
- **Grid**: Dos dimensiones (filas Y columnas) - Layouts completos
- **Pueden combinarse**: Grid para layout general, Flexbox para componentes internos

---

## Ejemplos Prácticos

### Ejemplo 1: Flexbox - Navegación Horizontal

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

.navbar ul {
  display: flex;
  gap: 2rem;
  list-style: none;
}
```

### Ejemplo 2: Flexbox - Centrar Contenido

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
```

### Ejemplo 3: Grid - Layout de Página

```css
.layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  gap: 20px;
  min-height: 100vh;
}

.header { 
  grid-column: 1 / -1; 
  background: blue;
}

.sidebar { 
  grid-row: 2; 
  background: gray;
}

.main { 
  grid-row: 2; 
  background: white;
}

.footer { 
  grid-column: 1 / -1; 
  background: black;
}
```

### Ejemplo 4: Grid - Galería de Imágenes

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
}

.gallery img {
  width: 100%;
  height: auto;
}
```

### Ejemplo 5: Combinando Flexbox y Grid

```css
/* Grid para el layout general */
.page {
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 2rem;
}

/* Flexbox para componentes internos */
.card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
```

---

## Conceptos Clave

1. **Flexbox**: Una dimensión, ideal para componentes y alineación
2. **Grid**: Dos dimensiones, ideal para layouts completos
3. **`justify-content`**: Alinea en el eje principal (horizontal en row)
4. **`align-items`**: Alinea en el eje transversal (vertical en row)
5. **`fr` (Fraction)**: Unidad flexible de Grid
6. **`gap`**: Espacio entre elementos (Flexbox y Grid)
7. **Áreas nombradas**: Facilita la organización en Grid

---

## Buenas Prácticas

- Usa Flexbox para componentes pequeños (botones, cards, navegación)
- Usa Grid para layouts de página completos
- Combina ambos: Grid para estructura, Flexbox para componentes
- Usa `gap` en lugar de márgenes para espaciado consistente
- `1fr` es más flexible que porcentajes fijos en Grid
- `flex: 1` es útil para distribuir espacio uniformemente

