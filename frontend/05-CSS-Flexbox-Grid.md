# CSS: Flexbox y Grid 🎯

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

---

## CSS Grid (Diseño Bidimensional) 🗺️

Pensado para estructuras completas de página (filas y columnas simultáneamente).

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

