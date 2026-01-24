# Master Guide: CSS3, Layouts y Animaciones 🎨

## 📑 Índice

1. [¿Qué es CSS? (Analogía del Mundo Real)](#qué-es-css-analogía-del-mundo-real)
2. [Introducción a CSS y Vinculación](#1-introducción-a-css-y-vinculación)
3. [El Modelo de Caja y Reset](#2-el-modelo-de-caja-y-reset)
4. [Selectores CSS](#3-selectores-css)
5. [Propiedades Fundamentales](#4-propiedades-fundamentales)
6. [Unidades de Medida en CSS](#35-unidades-de-medida-en-css)
7. [Posicionamiento](#5-posicionamiento)
8. [Pseudo-clases y Pseudo-elementos](#6-pseudo-clases-y-pseudo-elementos)
9. [Flexbox (Diseño Unidimensional)](#7-flexbox-diseño-unidimensional-)
10. [CSS Grid (Diseño Bidimensional)](#8-css-grid-diseño-bidimensional-)
11. [Transformaciones](#9-transformaciones-)
12. [Transiciones](#10-transiciones-cambio-entre-2-estados-)
13. [Animaciones con @keyframes](#11-animaciones-con-keyframes)
14. [Diseño Responsivo y Media Queries](#12-diseño-responsivo-y-media-queries-)
15. [Bootstrap - Framework CSS](#13-bootstrap---framework-css)
16. [DevTools del Navegador para CSS](#14-devtools-del-navegador-para-css-)
17. [Variables CSS con :root](#15-variables-css-con-root-)
18. [Buenas Prácticas y Recomendaciones](#16-buenas-prácticas-y-recomendaciones-)
19. [Ejemplos Prácticos del Código Modelo](#17-ejemplos-prácticos-del-código-modelo)
20. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es CSS? (Analogía del Mundo Real)

### 🎨 Analogía: HTML es la Estructura, CSS es la Decoración

Imagina construir una casa:
- **HTML**: Es como la estructura de la casa (paredes, puertas, ventanas)
- **CSS**: Es como la decoración (colores, muebles, estilo)
- **Resultado**: Una casa completa y bonita

**HTML define QUÉ hay** (estructura), **CSS define CÓMO se ve** (presentación).

### 👔 Analogía: El Traje y la Ropa

Piensa en una persona:
- **HTML**: Es como el cuerpo (estructura básica)
- **CSS**: Es como la ropa que usa (colores, estilos, diseño)
- **Resultado**: Una persona bien vestida

**CSS es como vestir tu HTML** - le das estilo, colores y diseño.

### 📝 Analogía: El Documento y el Formato

Un documento de Word:
- **HTML**: Es como el texto sin formato (solo el contenido)
- **CSS**: Es como aplicar formato (negrita, colores, tamaños, márgenes)
- **Resultado**: Un documento bien formateado

**CSS formatea tu HTML** - le da estilo y presentación.

### 🎭 Analogía: El Actor y el Maquillaje

En una obra de teatro:
- **HTML**: Es como el actor (la persona)
- **CSS**: Es como el maquillaje y vestuario (cómo se ve)
- **Resultado**: Un personaje completamente transformado

**CSS transforma la apariencia** de tu HTML.

---

## 1. Introducción a CSS y Vinculación

**CSS (Cascading Style Sheets)** es el lenguaje que define cómo se ven los elementos HTML. Permite controlar colores, fuentes, tamaños, posicionamiento y mucho más.

**En términos simples**: CSS es como el "diseñador" de tu página web - define los colores, tamaños, espacios y cómo se organizan visualmente todos los elementos.

### Vinculación de CSS a HTML

```html
<!-- En el <head> del HTML -->
<link rel="stylesheet" href="style.css">
```

**Alternativas**:
- **CSS interno**: `<style>` dentro del `<head>`
- **CSS inline**: `style="propiedad: valor;"` en el elemento (no recomendado)

### @import - Importar CSS en CSS

Puedes importar un archivo CSS dentro de otro usando `@import`. Esto es útil para organizar estilos en múltiples archivos.

```css
/* En tu archivo principal (style.css) */
@import url('reset.css');
@import url('variables.css');
@import url('components.css');
@import url('utilities.css');
```

**Ventajas**:
- ✅ Organización modular
- ✅ Separación de responsabilidades
- ✅ Fácil mantenimiento

**Desventajas**:
- ❌ Más peticiones HTTP (si no se combinan)
- ❌ Puede afectar el rendimiento
- ❌ Orden de carga importante

**Mejor práctica**: Usar `@import` para organización, pero combinar archivos en producción.

### CSS General con Clases/IDs en Body

Cuando usas un CSS general compartido entre múltiples páginas, es importante diferenciar los estilos para que no se "pisen" entre páginas.

**Solución: Clases o IDs en el `<body>`**

```html
<!-- Página de inicio -->
<body class="home-page">
  <!-- Contenido -->
</body>

<!-- Página de contacto -->
<body class="contact-page">
  <!-- Contenido -->
</body>

<!-- Página de productos -->
<body id="products-page">
  <!-- Contenido -->
</body>
```

**En CSS**:

```css
/* Estilos generales (aplican a todas las páginas) */
.header { background: blue; }
.footer { background: gray; }

/* Estilos específicos de la página de inicio */
.home-page .hero {
  background: linear-gradient(to right, blue, red);
}

.home-page .section-title {
  color: #333;
}

/* Estilos específicos de la página de contacto */
.contact-page .form-container {
  max-width: 600px;
}

.contact-page .button {
  background: green;
}

/* Estilos específicos de la página de productos */
#products-page .product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

**Estructura recomendada**:

```
css/
├── general.css      /* Estilos compartidos */
├── home.css         /* Estilos específicos de home */
├── contact.css      /* Estilos específicos de contacto */
└── products.css     /* Estilos específicos de productos */
```

**En el HTML**:

```html
<head>
  <link rel="stylesheet" href="css/general.css">
  <link rel="stylesheet" href="css/home.css"> <!-- Solo en home.html -->
</head>
<body class="home-page">
```

**O usando @import**:

```css
/* general.css */
@import url('reset.css');
@import url('variables.css');

/* home.css */
@import url('general.css');

.home-page .specific-style { }
```

**Ventajas de este enfoque**:
- ✅ Estilos compartidos sin duplicación
- ✅ Estilos específicos por página
- ✅ Fácil mantenimiento
- ✅ No hay conflictos entre páginas

**Cuándo usar clases vs IDs en body**:
- **Clases** (`.home-page`): Si una página puede tener múltiples "tipos"
- **IDs** (`#products-page`): Si es único por página (más específico)

---

## 2. El Modelo de Caja y Reset

Todas las etiquetas HTML son, en esencia, cajas. Para evitar errores de cálculo en el tamaño, siempre usa el reset de **`box-sizing`**:

```css
* {
  box-sizing: border-box; /* Incluye padding y border en el width/height definido */
  margin: 0;
  padding: 0;
}
```

**¿Por qué `border-box`?**
- Con `content-box` (por defecto): `width` + `padding` + `border` = tamaño total
- Con `border-box`: `width` = tamaño total (incluye padding y border)

**Componentes del modelo de caja**:
- **Content**: El contenido real
- **Padding**: Espacio interno
- **Border**: Borde
- **Margin**: Espacio externo

---

## 3. Selectores CSS

### ¿Qué es un Selector?

Un **selector** es la parte de una regla CSS que identifica qué elementos HTML se van a estilizar. Es como "seleccionar" elementos para aplicarles estilos.

**Estructura básica de una regla CSS**:
```css
selector {
  propiedad: valor;
  propiedad2: valor2;
}
```

### Selectores Básicos

```css
/* Por elemento - Selecciona todos los elementos de ese tipo */
h1 { color: blue; }
p { font-size: 16px; }

/* Por clase - Selecciona elementos con esa clase (puede haber múltiples) */
.section-title { color: red; }
/* En HTML: <h1 class="section-title">Título</h1> */

/* Por ID - Selecciona el elemento con ese ID (debe ser único) */
#header { background: gray; }
/* En HTML: <header id="header">...</header> */

/* Combinados - Selecciona elementos anidados */
h1 span { color: green; } /* span dentro de h1 */
section img { width: 100%; } /* img dentro de section */
```

### Selectores Avanzados

```css
/* Descendiente directo - Solo hijos directos */
.parent > .child { }
/* Selecciona .child que es hijo directo de .parent */

/* Hermano adyacente - Elemento siguiente inmediato */
.element + .next { }
/* Selecciona .next que viene justo después de .element */

/* Hermano general - Todos los hermanos siguientes */
.element ~ .sibling { }

/* Atributo */
input[type="text"] { } /* Inputs de tipo texto */
a[href^="https"] { } /* Enlaces que empiezan con https */
img[alt] { } /* Imágenes que tienen atributo alt */
```

### Especificidad de Selectores

El navegador usa la **especificidad** para decidir qué estilos aplicar cuando hay conflictos:

1. **Inline styles** (`style="..."`) - Especificidad más alta
2. **IDs** (`#id`) - Especificidad alta
3. **Clases, atributos, pseudo-clases** (`.class`, `[attr]`, `:hover`) - Especificidad media
4. **Elementos, pseudo-elementos** (`h1`, `::before`) - Especificidad baja

**Ejemplo**:
```css
/* Especificidad: 0,0,1,0 (1 clase) */
.title { color: blue; }

/* Especificidad: 0,1,0,0 (1 ID) */
#main-title { color: red; } /* Este gana */

/* Especificidad: 0,0,2,1 (2 clases + 1 elemento) */
div.title.special { color: green; }
```

---

## 4. Propiedades Fundamentales

### Color y Fondo

```css
/* Color de texto */
color: #333333;
color: rgb(51, 51, 51);
color: blue;

/* Fondo */
background-color: #ffffff;
background: linear-gradient(to right, blue, red);
background-image: url('imagen.jpg');
```

### Tipografía

```css
font-family: 'Arial', sans-serif;
font-size: 16px;
font-weight: bold; /* normal, bold, 100-900 */
font-style: italic;
text-align: center; /* left, right, center, justify */
text-decoration: underline; /* none, underline, line-through */
line-height: 1.5;
```

### Dimensiones

```css
width: 100%;
width: 500px;
width: 50vw; /* 50% del viewport width */
height: 200px;
height: 50vh; /* 50% del viewport height */
max-width: 1200px;
min-height: 400px;
```

---

## 3.5. Unidades de Medida en CSS

CSS ofrece diferentes unidades de medida, cada una con sus ventajas y casos de uso específicos.

### Unidades Absolutas

#### Pixels (`px`)

**¿Qué es?**: Unidad fija basada en píxeles de la pantalla.

```css
width: 500px;
font-size: 16px;
```

**Pros**:
- ✅ Precisión exacta
- ✅ Comportamiento predecible
- ✅ Ideal para bordes, sombras, elementos pequeños

**Contras**:
- ❌ No se adapta bien a diferentes tamaños de pantalla
- ❌ No respeta las preferencias de accesibilidad del usuario
- ❌ Menos flexible para diseño responsivo

**Cuándo usar**: Bordes, sombras, elementos que deben tener tamaño exacto.

---

### Unidades Relativas al Viewport

#### Viewport Width (`vw`)

**¿Qué es?**: 1vw = 1% del ancho del viewport (ventana del navegador).

```css
width: 50vw; /* 50% del ancho de la pantalla */
font-size: 5vw; /* Tamaño de fuente relativo al ancho */
```

**Pros**:
- ✅ Se adapta automáticamente al tamaño de la pantalla
- ✅ Útil para elementos que deben ocupar porcentaje del viewport
- ✅ Ideal para diseño full-screen

**Contras**:
- ❌ Puede causar problemas en móviles (texto muy pequeño o grande)
- ❌ No considera el tamaño del contenedor padre
- ❌ Puede ser problemático para accesibilidad

**Cuándo usar**: Hero sections, elementos que deben ocupar ancho completo de pantalla.

#### Viewport Height (`vh`)

**¿Qué es?**: 1vh = 1% del alto del viewport.

```css
height: 100vh; /* 100% del alto de la pantalla */
min-height: 50vh; /* Mínimo 50% del alto */
```

**Pros**:
- ✅ Perfecto para secciones full-height
- ✅ Se adapta al alto de la pantalla
- ✅ Útil para layouts que ocupan toda la altura

**Contras**:
- ❌ Problemas en móviles (barra de navegación del navegador)
- ❌ No considera contenido interno
- ❌ Puede causar scroll innecesario

**Cuándo usar**: Hero sections, modales, secciones que deben ocupar toda la altura.

#### Viewport Minimum/Maximum (`vmin` / `vmax`)

**¿Qué es?**: 
- `vmin`: El menor entre vw y vh
- `vmax`: El mayor entre vw y vh

```css
font-size: 5vmin; /* 5% del lado más pequeño */
width: 50vmax; /* 50% del lado más grande */
```

**Cuándo usar**: Elementos que deben adaptarse al lado más pequeño o más grande de la pantalla.

---

### Unidades Relativas al Contenedor

#### Porcentaje (`%`)

**¿Qué es?**: Porcentaje relativo al elemento padre.

```css
width: 50%; /* 50% del ancho del contenedor padre */
height: 100%; /* 100% del alto del contenedor padre */
```

**Pros**:
- ✅ Se adapta al tamaño del contenedor padre
- ✅ Muy flexible para layouts responsivos
- ✅ Comportamiento predecible

**Contras**:
- ❌ Depende del tamaño del padre (puede ser 0 si el padre no tiene tamaño)
- ❌ Para `height`, el padre debe tener altura definida
- ❌ Puede causar problemas con padding y border

**Cuándo usar**: Layouts flexibles, elementos que deben ocupar porcentaje del contenedor.

**Nota importante**: El porcentaje se calcula basado en el tamaño del **elemento padre**, no del viewport.

---

### Unidades Relativas a la Fuente

#### `rem` (Root EM)

**¿Qué es?**: Relativo al tamaño de fuente del elemento raíz (`<html>`). Por defecto, 1rem = 16px.

```css
font-size: 1.5rem; /* 1.5 × 16px = 24px (si el root es 16px) */
margin: 2rem; /* 2 × 16px = 32px */
```

**Pros**:
- ✅ Respeta las preferencias de accesibilidad del usuario
- ✅ Consistente en toda la página (siempre relativo al root)
- ✅ Fácil de escalar (cambiar el root afecta todo)
- ✅ Ideal para diseño responsivo

**Contras**:
- ❌ Puede ser confuso si el usuario cambia el tamaño del root
- ❌ Requiere entender la relación con el root

**Cuándo usar**: Tamaños de fuente, espaciados, márgenes, padding. **Recomendado para la mayoría de casos**.

#### `em` (EM)

**¿Qué es?**: Relativo al tamaño de fuente del elemento padre (o del mismo elemento para algunas propiedades).

```css
.parent {
  font-size: 20px;
}
.child {
  font-size: 1.5em; /* 1.5 × 20px = 30px */
  margin: 2em; /* 2 × 30px = 60px (relativo a su propio font-size) */
}
```

**Pros**:
- ✅ Se adapta al contexto del elemento padre
- ✅ Útil para componentes que deben escalar juntos
- ✅ Respeta jerarquía de tamaños

**Contras**:
- ❌ Puede causar problemas de escalado compuesto (em dentro de em)
- ❌ Menos predecible que `rem`
- ❌ Requiere cuidado con la anidación

**Cuándo usar**: Componentes que deben escalar juntos, elementos que dependen del tamaño del padre.

**Diferencia clave**: 
- `rem`: Siempre relativo al root
- `em`: Relativo al elemento padre (o a sí mismo para algunas propiedades)

---

### Comparación Rápida

| Unidad | Relativo a | Mejor para | Responsive |
|--------|-----------|------------|------------|
| `px` | Nada (absoluto) | Bordes, sombras | ❌ |
| `%` | Elemento padre | Layouts flexibles | ✅ |
| `vw` / `vh` | Viewport | Elementos full-screen | ✅ |
| `rem` | Root (`<html>`) | Fuentes, espaciados | ✅✅ |
| `em` | Elemento padre | Componentes escalables | ✅ |

### Recomendaciones Generales

1. **Para fuentes y espaciados**: Usa `rem` (más predecible)
2. **Para layouts**: Usa `%` o unidades de viewport según el caso
3. **Para elementos pequeños**: Usa `px` (bordes, sombras)
4. **Para diseño responsivo**: Combina `rem`, `%`, y media queries
5. **Evita**: `em` para espaciados (puede causar problemas de escalado)

---

### Ejemplos Prácticos

```css
/* Layout responsivo con diferentes unidades */
.container {
  width: 90%; /* 90% del contenedor padre */
  max-width: 1200px; /* Máximo 1200px */
  margin: 0 auto; /* Centrado */
  padding: 2rem; /* Espaciado relativo al root */
}

.hero {
  width: 100vw; /* Ancho completo del viewport */
  height: 100vh; /* Alto completo del viewport */
  font-size: clamp(1.5rem, 5vw, 3rem); /* Responsive entre 1.5rem y 3rem */
}

.card {
  width: 100%; /* 100% del contenedor */
  padding: 1.5rem; /* Espaciado en rem */
  border: 2px solid #000; /* Borde en px (fijo) */
  font-size: 1.125rem; /* Fuente en rem */
}
```

### Espaciado

```css
/* Margin (externo) */
margin: 10px; /* todos los lados */
margin: 10px 20px; /* vertical horizontal */
margin: 10px 20px 30px 40px; /* top right bottom left */
margin-top: 10px;

/* Padding (interno) */
padding: 10px;
padding: 10px 20px;
padding-left: 15px;
```

---

## 5. Posicionamiento

### Tipos de Posicionamiento

```css
/* Static (por defecto) */
position: static; /* Flujo normal del documento */

/* Relative */
position: relative; /* Relativo a su posición original */
top: 10px;
left: 20px;

/* Absolute */
position: absolute; /* Relativo al contenedor padre posicionado */
top: 0;
right: 0;

/* Fixed */
position: fixed; /* Fijo respecto al viewport (no se mueve con scroll) */
top: 0;
left: 0;

/* Sticky */
position: sticky; /* Se comporta como relative hasta un punto, luego como fixed */
top: 0;
```

**Casos de uso**:
- **Relative**: Ajustar posición sin afectar otros elementos
- **Absolute**: Elementos superpuestos, tooltips, menús
- **Fixed**: Headers que se quedan fijos, botones flotantes
- **Sticky**: Headers que se pegan al hacer scroll

---

## 6. Pseudo-clases y Pseudo-elementos

### Pseudo-clases (estado o posición)

```css
/* Estados de enlaces */
a:link { color: blue; }
a:visited { color: purple; }
a:hover { color: red; }
a:active { color: orange; }

/* Estados de inputs */
input:focus { border: 2px solid blue; }
input:disabled { opacity: 0.5; }

/* Posición en lista */
li:first-child { font-weight: bold; }
li:last-child { margin-bottom: 0; }
li:nth-child(2) { color: red; }
li:nth-child(even) { background: #f0f0f0; }
li:nth-child(odd) { background: white; }
```

### Pseudo-elementos (contenido o partes)

```css
/* Agregar contenido */
.element::before {
  content: "→ ";
  color: blue;
}

.element::after {
  content: " ←";
  color: red;
}

/* Partes del elemento */
p::first-line { font-weight: bold; }
p::first-letter { font-size: 2em; }
input::placeholder { color: gray; }
```

**Diferencia clave**:
- **Pseudo-clase** (`:`) - Estado o posición del elemento
- **Pseudo-elemento** (`::`) - Parte específica o contenido generado

---

## 7. Flexbox (Diseño Unidimensional) ↔️↕️

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

## 8. CSS Grid (Diseño Bidimensional) 🗺️

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

**Flexbox vs Grid**:
- **Flexbox**: Una dimensión (fila O columna) - Componentes pequeños
- **Grid**: Dos dimensiones (filas Y columnas) - Layouts completos
- **Pueden combinarse**: Grid para layout general, Flexbox para componentes internos

---

## 9. Transformaciones 🌀

Las transformaciones permiten modificar la forma, posición y orientación de elementos.

```css
/* Rotar */
transform: rotate(45deg);
transform: rotateX(45deg); /* 3D */
transform: rotateY(45deg);

/* Escalar */
transform: scale(1.2); /* 120% del tamaño */
transform: scale(0.5); /* 50% del tamaño */
transform: scaleX(1.5); /* Solo horizontal */
transform: scaleY(0.8); /* Solo vertical */

/* Mover */
transform: translate(50px, 100px); /* x, y */
transform: translateX(50px);
transform: translateY(100px);

/* Combinar */
transform: rotate(45deg) scale(1.2) translate(10px, 20px);

/* 3D - Perspectiva */
.parent {
  perspective: 500px;
}
.child {
  transform: rotateY(45deg);
}
```

**Punto de transformación**:
```css
transform-origin: center; /* center, top, bottom, left, right, o coordenadas */
```

---

## 10. Transiciones (Cambio entre 2 estados) ✨

Requieren una interacción previa (ej: `:hover`). Permiten cambios suaves entre estados.

```css
.element {
  background-color: blue;
  transition: background-color 0.5s ease;
}

.element:hover {
  background-color: darkblue; /* Cambio suave de 0.5s */
}
```

### Propiedades de Transición

```css
/* Propiedad específica */
transition: background-color 0.5s ease;

/* Múltiples propiedades */
transition: background-color 0.5s ease, transform 0.3s linear;

/* Todas las propiedades */
transition: all 0.5s ease;

/* Propiedades individuales */
transition-property: background-color, transform;
transition-duration: 0.5s, 0.3s;
transition-timing-function: ease, linear;
transition-delay: 0s, 0.2s;
```

### Funciones de Tiempo (Timing Functions)

```css
transition-timing-function: ease; /* Inicio lento, rápido, fin lento */
transition-timing-function: linear; /* Velocidad constante */
transition-timing-function: ease-in; /* Inicio lento */
transition-timing-function: ease-out; /* Fin lento */
transition-timing-function: ease-in-out; /* Inicio y fin lentos */
transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1); /* Personalizada */
```

---

## 11. Animaciones con @keyframes

No necesitan interacción; disparan solas o en bucle. Permiten animaciones complejas.

### Definir Animación

```css
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* O con porcentajes */
@keyframes bounce {
  0% { transform: translateY(0); }
  50% { transform: translateY(-50px); }
  100% { transform: translateY(0); }
}
```

### Aplicar Animación

```css
.element {
  animation-name: slideIn;
  animation-duration: 1s;
  animation-timing-function: ease;
  animation-delay: 0.5s;
  animation-iteration-count: 3; /* infinite para bucle infinito */
  animation-direction: normal; /* normal, reverse, alternate, alternate-reverse */
  animation-fill-mode: forwards; /* none, forwards, backwards, both */
  
  /* Atajo */
  animation: slideIn 1s ease 0.5s 3 forwards;
}
```

### Propiedades Clave

- **`duration`**: Cuánto dura la animación
- **`delay`**: Cuánto espera para empezar
- **`iteration-count`**: Cuántas veces se repite (`infinite` para bucle)
- **`fill-mode`**: 
  - `forwards`: Mantiene el estado final
  - `backwards`: Aplica estilos del primer keyframe antes de iniciar
  - `both`: Combina forwards y backwards
- **`direction`**: 
  - `normal`: De inicio a fin
  - `reverse`: De fin a inicio
  - `alternate`: Alterna entre normal y reverse

---

## 12. Diseño Responsivo y Media Queries 📱💻

Permiten adaptar los estilos según el ancho de la pantalla. Fundamental seguir la estrategia **Mobile-First**.

### Viewport Meta Tag

```html
<!-- En el <head> del HTML -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

**Es crucial** para que el diseño responsivo funcione correctamente en móviles.

### Media Queries

```css
/* Mobile-First: Estilos base para móvil */
.content {
  flex-direction: column;
  padding: 10px;
}

/* Tablet y superior */
@media (min-width: 768px) {
  .content {
    flex-direction: row;
    padding: 20px;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .content {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

### Breakpoints Comunes

```css
/* Móviles pequeños */
@media (max-width: 480px) { }

/* Móviles grandes / Tablets pequeñas */
@media (min-width: 481px) and (max-width: 767px) { }

/* Tablets */
@media (min-width: 768px) and (max-width: 1023px) { }

/* Laptops */
@media (min-width: 1024px) and (max-width: 1199px) { }

/* Escritorios grandes */
@media (min-width: 1200px) { }
```

### Unidades Relativas para Responsive

```css
/* Viewport */
width: 50vw; /* 50% del ancho del viewport */
height: 100vh; /* 100% del alto del viewport */

/* Porcentajes */
width: 100%; /* 100% del contenedor padre */

/* Rem y Em */
font-size: 1rem; /* Relativo al root (16px por defecto) */
font-size: 1em; /* Relativo al elemento padre */
```

### Imágenes Responsivas

```css
img {
  max-width: 100%; /* No más ancho que el contenedor */
  height: auto; /* Mantiene proporción */
}
```

---

## 13. Bootstrap - Framework CSS

Bootstrap es el framework CSS más popular para crear sitios web responsivos rápidamente.

### Inclusión de Bootstrap

```html
<!-- CDN (recomendado para empezar) -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<!-- O archivos locales -->
<link rel="stylesheet" href="css/bootstrap.min.css">
```

### Sistema de Grid de Bootstrap

```html
<div class="container">
  <div class="row">
    <div class="col">Columna 1</div>
    <div class="col">Columna 2</div>
    <div class="col">Columna 3</div>
  </div>
</div>
```

**Breakpoints de Bootstrap**:
- `col`: Extra pequeño (xs) - < 576px
- `col-sm`: Pequeño (sm) - ≥ 576px
- `col-md`: Mediano (md) - ≥ 768px
- `col-lg`: Grande (lg) - ≥ 992px
- `col-xl`: Extra grande (xl) - ≥ 1200px
- `col-xxl`: Extra extra grande (xxl) - ≥ 1400px

**Ejemplo Responsivo**:
```html
<div class="row">
  <div class="col-12 col-md-6 col-lg-4">
    <!-- 12 columnas en móvil, 6 en tablet, 4 en desktop -->
  </div>
</div>
```

### Componentes Principales

```html
<!-- Navbar -->
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Logo</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link" href="#">Inicio</a>
        </li>
      </ul>
    </div>
  </div>
</nav>

<!-- Cards -->
<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Título</h5>
    <p class="card-text">Contenido</p>
    <a href="#" class="btn btn-primary">Botón</a>
  </div>
</div>

<!-- Carousel -->
<div id="carouselExample" class="carousel slide">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="..." class="d-block w-100" alt="...">
    </div>
  </div>
</div>

<!-- Botones -->
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
```

### Personalizar Bootstrap

```css
/* Sobrescribir variables de Bootstrap */
:root {
  --bs-primary: #your-color;
  --bs-font-sans-serif: 'Your Font', sans-serif;
}

/* O con CSS personalizado */
.btn-custom {
  background-color: #custom-color;
  border-color: #custom-color;
}
```

---

## 14. DevTools del Navegador para CSS 🛠️

Las herramientas de desarrollador (DevTools) del navegador son esenciales para trabajar con CSS. Permiten inspeccionar, modificar y depurar estilos en tiempo real.

### Cómo Abrir DevTools

- **Chrome/Edge**: `F12` o `Ctrl + Shift + I` (Windows/Linux) / `Cmd + Option + I` (Mac)
- **Firefox**: `F12` o `Ctrl + Shift + I` (Windows/Linux) / `Cmd + Option + I` (Mac)
- **Safari**: `Cmd + Option + I` (Mac, requiere activar menú de desarrollador)

### Panel de Elementos (Elements/Inspector)

#### Inspeccionar Elementos

1. **Click derecho** en cualquier elemento → "Inspeccionar" o "Inspeccionar elemento"
2. O usar el **ícono de selección** (Ctrl+Shift+C) y hacer click en el elemento

**Qué verás**:
- HTML del elemento seleccionado
- Estilos CSS aplicados
- Modelo de caja (box model)
- Eventos asociados

#### Panel de Estilos

En el panel derecho verás:

**Estilos Computados**:
- Todos los estilos finales aplicados al elemento
- Ordenados por especificidad
- Muestra qué regla CSS está aplicada

**Estilos en Cascada**:
- Todas las reglas que afectan al elemento
- Reglas tachadas = sobrescritas por otras más específicas
- Reglas activas = aplicadas actualmente

#### Modificar Estilos en Tiempo Real

1. **Click en cualquier valor** para editarlo
2. **Agregar nuevas propiedades** haciendo click en el área de estilos
3. **Desactivar propiedades** haciendo click en el checkbox
4. **Ver cambios instantáneos** sin recargar la página

**Ejemplo**:
```css
/* Puedes cambiar */
color: blue;
/* a */
color: red;
/* Y ver el cambio inmediatamente */
```

#### Box Model Visual

Muestra visualmente:
- **Content**: Contenido del elemento
- **Padding**: Espacio interno
- **Border**: Borde
- **Margin**: Espacio externo

**Valores editables**: Puedes cambiar directamente los valores y ver el efecto.

### Utilidades Específicas para CSS

#### 1. Selector de Color

Al hacer click en un color (hex, rgb, etc.), se abre un **selector de color visual**:
- Paleta de colores
- Selector de matiz
- Valores RGB/HSL editables
- Vista previa en tiempo real

#### 2. Toggle de Propiedades

- **Checkbox**: Activar/desactivar propiedades
- **Icono de ojo**: Ocultar/mostrar elemento (`display: none`)
- **Icono de reloj**: Ver animaciones paso a paso

#### 3. Computed (Estilos Computados)

Muestra:
- **Valores finales** de todas las propiedades
- **Origen** de cada valor (qué regla CSS lo definió)
- **Box model** con medidas exactas

#### 4. Media Queries

En la barra superior del viewport:
- **Icono de dispositivo**: Activar modo responsive
- **Selector de tamaño**: Predefinidos (iPhone, iPad, etc.)
- **Editar breakpoints**: Ver y probar media queries

#### 5. Animations Panel

Para inspeccionar animaciones:
- **Timeline**: Ver duración y keyframes
- **Pausar/Reproducir**: Control de animación
- **Velocidad**: Acelerar o ralentizar

#### 6. Grid y Flexbox Overlays

**Para CSS Grid**:
- Muestra las líneas de la grilla
- Nombres de áreas
- Números de filas/columnas

**Para Flexbox**:
- Muestra el contenedor flex
- Dirección del eje
- Alineación de elementos

**Cómo activar**: Click en el icono de grid/flexbox junto a `display: grid` o `display: flex`

### Atajos Útiles

- **Ctrl + Shift + C**: Modo inspección (seleccionar elemento)
- **Ctrl + ] / [**: Navegar entre elementos hermanos
- **Ctrl + ↑ / ↓**: Cambiar valores numéricos
- **Ctrl + Click**: Editar múltiples valores
- **Esc**: Abrir/cerrar consola dentro de DevTools

### Depuración de CSS

#### Problemas Comunes y Cómo Resolverlos

1. **Estilos no se aplican**:
   - Revisa si la regla está tachada (sobrescrita)
   - Verifica la especificidad
   - Comprueba si hay errores de sintaxis

2. **Layout roto**:
   - Inspecciona el box model
   - Verifica `box-sizing`
   - Revisa `display`, `position`, `float`

3. **Responsive no funciona**:
   - Activa modo responsive
   - Verifica media queries
   - Comprueba viewport meta tag

4. **Animaciones no funcionan**:
   - Revisa el panel de animaciones
   - Verifica `@keyframes`
   - Comprueba `animation-*` properties

### Copiar Estilos

**Copiar regla completa**:
- Click derecho en la regla → "Copy rule"

**Copiar selector**:
- Click derecho → "Copy selector"

**Copiar estilos como JavaScript**:
- Click derecho → "Copy JS path"

### Exportar Cambios

Los cambios en DevTools son **temporales**. Para guardarlos:
1. Modifica en DevTools
2. Click derecho en el archivo CSS → "Save as"
3. O usa herramientas como "Local Overrides" (Chrome)

### Local Overrides (Chrome)

Permite guardar cambios localmente sin modificar archivos originales:

1. Abre DevTools → Sources
2. Activa "Local Overrides"
3. Selecciona carpeta para guardar
4. Modifica CSS en DevTools
5. Cambios se guardan automáticamente

---

## 15. Variables CSS con :root 🎨

Las **variables CSS** (también llamadas custom properties) permiten definir valores reutilizables que puedes usar en toda tu hoja de estilos. Esto facilita el mantenimiento y permite crear temas consistentes.

### ¿Qué es :root?

`:root` es un pseudo-selector que representa el elemento raíz del documento (normalmente `<html>`). Es el nivel más alto de especificidad, por lo que las variables definidas aquí están disponibles en todo el documento.

### Definir Variables

```css
:root {
  /* Colores principales */
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  --warning-color: #ffc107;
  --info-color: #17a2b8;
  
  /* Colores de texto */
  --text-primary: #212529;
  --text-secondary: #6c757d;
  --text-light: #ffffff;
  
  /* Colores de fondo */
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --bg-dark: #343a40;
  
  /* Tipografía */
  --font-main: 'Arial', sans-serif;
  --font-heading: 'Georgia', serif;
  --font-size-base: 16px;
  --font-size-small: 14px;
  --font-size-large: 18px;
  --font-size-h1: 2.5rem;
  --font-size-h2: 2rem;
  
  /* Espaciados */
  --spacing-xs: 0.25rem;   /* 4px */
  --spacing-sm: 0.5rem;    /* 8px */
  --spacing-md: 1rem;      /* 16px */
  --spacing-lg: 1.5rem;    /* 24px */
  --spacing-xl: 2rem;      /* 32px */
  --spacing-xxl: 3rem;     /* 48px */
  
  /* Bordes */
  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-radius-lg: 12px;
  --border-width: 1px;
  --border-color: #dee2e6;
  
  /* Sombras */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
  
  /* Transiciones */
  --transition-fast: 0.15s ease;
  --transition-normal: 0.3s ease;
  --transition-slow: 0.5s ease;
  
  /* Breakpoints (para uso en calc o JavaScript) */
  --breakpoint-sm: 576px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 992px;
  --breakpoint-xl: 1200px;
}
```

### Usar Variables

Para usar una variable, utiliza la función `var()`:

```css
.button {
  background-color: var(--primary-color);
  color: var(--text-light);
  padding: var(--spacing-md) var(--spacing-lg);
  border-radius: var(--border-radius-md);
  font-family: var(--font-main);
  font-size: var(--font-size-base);
  box-shadow: var(--shadow-md);
  transition: all var(--transition-normal);
}

.card {
  background-color: var(--bg-primary);
  border: var(--border-width) solid var(--border-color);
  border-radius: var(--border-radius-lg);
  padding: var(--spacing-lg);
  box-shadow: var(--shadow-sm);
}

.heading {
  font-family: var(--font-heading);
  color: var(--text-primary);
}

h1 {
  font-size: var(--font-size-h1);
}

h2 {
  font-size: var(--font-size-h2);
}
```

### Valores por Defecto

Puedes definir un valor por defecto si la variable no está definida:

```css
.element {
  color: var(--text-color, #333333); /* Si --text-color no existe, usa #333333 */
  margin: var(--spacing-custom, 1rem); /* Valor por defecto: 1rem */
}
```

### Variables en Variables

Puedes usar variables dentro de otras variables:

```css
:root {
  --base-spacing: 1rem;
  --spacing-sm: calc(var(--base-spacing) * 0.5);  /* 0.5rem */
  --spacing-md: var(--base-spacing);               /* 1rem */
  --spacing-lg: calc(var(--base-spacing) * 1.5);  /* 1.5rem */
  --spacing-xl: calc(var(--base-spacing) * 2);    /* 2rem */
}
```

### Alcance (Scope) de Variables

Las variables pueden ser **globales** o **locales**:

```css
/* Global - disponible en todo el documento */
:root {
  --primary-color: blue;
}

/* Local - solo disponible en .container y sus hijos */
.container {
  --local-color: red;
  background-color: var(--local-color);
}

.container .child {
  /* Puede usar --local-color porque es hijo */
  color: var(--local-color);
}

.other-element {
  /* NO puede usar --local-color (no es hijo de .container) */
  color: var(--local-color); /* No funcionará */
}
```

### Cambiar Variables con JavaScript

Las variables CSS pueden modificarse dinámicamente con JavaScript:

```javascript
// Cambiar una variable global
document.documentElement.style.setProperty('--primary-color', '#ff0000');

// Cambiar una variable local
const container = document.querySelector('.container');
container.style.setProperty('--local-spacing', '2rem');

// Obtener valor de una variable
const primaryColor = getComputedStyle(document.documentElement)
  .getPropertyValue('--primary-color');
```

### Temas (Dark Mode / Light Mode)

Las variables son perfectas para crear temas:

```css
/* Tema claro (por defecto) */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f8f9fa;
  --text-primary: #212529;
  --text-secondary: #6c757d;
  --border-color: #dee2e6;
}

/* Tema oscuro */
[data-theme="dark"] {
  --bg-primary: #212529;
  --bg-secondary: #343a40;
  --text-primary: #ffffff;
  --text-secondary: #adb5bd;
  --border-color: #495057;
}

/* Aplicar tema */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}
```

**Cambiar tema con JavaScript**:
```javascript
// Activar tema oscuro
document.documentElement.setAttribute('data-theme', 'dark');

// Activar tema claro
document.documentElement.setAttribute('data-theme', 'light');
```

### Ventajas de Variables CSS

✅ **Mantenimiento fácil**: Cambia un valor en un lugar, se actualiza en todos lados
✅ **Consistencia**: Valores uniformes en todo el sitio
✅ **Temas dinámicos**: Fácil cambiar colores/espaciados globalmente
✅ **Reutilización**: Define una vez, usa muchas veces
✅ **Legibilidad**: Nombres descriptivos (`--primary-color` vs `#007bff`)
✅ **Modificables con JS**: Puedes cambiar valores dinámicamente

### Ejemplo Completo

```css
:root {
  /* Sistema de colores */
  --color-primary: #007bff;
  --color-primary-dark: #0056b3;
  --color-primary-light: #66b3ff;
  
  /* Sistema de espaciado */
  --space-unit: 0.5rem;
  --space-1: var(--space-unit);      /* 0.5rem */
  --space-2: calc(var(--space-unit) * 2);  /* 1rem */
  --space-3: calc(var(--space-unit) * 3);  /* 1.5rem */
  --space-4: calc(var(--space-unit) * 4);  /* 2rem */
  
  /* Sistema de tipografía */
  --font-base: 16px;
  --font-scale: 1.25;
  --font-size-sm: calc(var(--font-base) / var(--font-scale));
  --font-size-base: var(--font-base);
  --font-size-lg: calc(var(--font-base) * var(--font-scale));
  --font-size-xl: calc(var(--font-base) * var(--font-scale) * var(--font-scale));
}

/* Uso */
.button {
  background: var(--color-primary);
  padding: var(--space-2) var(--space-4);
  font-size: var(--font-size-base);
  border-radius: var(--space-1);
}

.button:hover {
  background: var(--color-primary-dark);
}

.card {
  padding: var(--space-3);
  margin-bottom: var(--space-2);
}

.heading-large {
  font-size: var(--font-size-xl);
  color: var(--color-primary);
}
```

### Mejores Prácticas

1. **Usa nombres descriptivos**: `--primary-color` mejor que `--color1`
2. **Organiza por categorías**: Colores, espaciados, tipografía, etc.
3. **Define en `:root`**: Para variables globales
4. **Usa prefijos**: `--color-`, `--spacing-`, `--font-` para organización
5. **Valores por defecto**: Siempre que sea posible
6. **Documenta**: Comenta variables complejas o no obvias

```css
:root {
  /* ===== COLORES ===== */
  --color-primary: #007bff;      /* Color principal de la marca */
  --color-secondary: #6c757d;    /* Color secundario */
  
  /* ===== ESPACIADOS ===== */
  --spacing-base: 1rem;           /* Espaciado base (16px) */
  --spacing-sm: calc(var(--spacing-base) * 0.5);  /* 0.5rem */
  --spacing-lg: calc(var(--spacing-base) * 2);    /* 2rem */
}
```

---

## 16. Buenas Prácticas y Recomendaciones ✅

### Organización de CSS

```css
/* 1. Reset / Normalize */
* { box-sizing: border-box; }

/* 2. Variables CSS en :root */
:root {
  --primary-color: #007bff;
  --font-main: 'Arial', sans-serif;
  --spacing-base: 1rem;
}

/* 3. Estilos base */
body { 
  font-family: var(--font-main);
  color: var(--text-primary);
}

/* 4. Componentes */
.button { 
  background: var(--primary-color);
  padding: var(--spacing-base);
}
.card { }

/* 5. Utilidades */
.text-center { text-align: center; }
.mt-20 { margin-top: 20px; }
```

### Nomenclatura

- **BEM (Block Element Modifier)**: `.block__element--modifier`
- **Clases descriptivas**: `.header-nav`, `.product-card`
- **Evitar IDs para estilos**: Usar clases

### Performance

- **Minificar CSS** en producción
- **Usar variables CSS** para valores repetidos
- **Evitar selectores muy específicos**: `.header .nav .item .link` es innecesario
- **Usar `transform` y `opacity`** para animaciones (mejor performance)

### Accesibilidad

- **Contraste de colores**: Texto legible sobre fondo
- **Tamaños de fuente**: Mínimo 16px para texto principal
- **Focus visible**: Indicadores claros en elementos interactivos
- **Responsive**: Funciona en todos los dispositivos

---

## 17. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: CSS Básico (Tema 03)

```html
<!-- HTML -->
<header>
  <nav>
    <ul>
      <li><a href="#home">Inicio</a></li>
    </ul>
  </nav>
</header>
<main>
  <section id="home">
    <h1 class="section-title">Título</h1>
    <p>Contenido</p>
  </section>
</main>
```

```css
/* CSS */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

header {
  background: linear-gradient(to right, blue, red);
  padding: 20px;
}

.section-title {
  color: #333;
  font-size: 2em;
  text-align: center;
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-03-introduccion-css-estilos-basicos/`

### Ejemplo 2: Flexbox (Tema 05)

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  flex: 1 1 300px; /* grow shrink basis */
  min-width: 250px;
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-05-css-flexbox-grid/flex/`

### Ejemplo 3: Grid (Tema 05)

```css
.layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  gap: 20px;
  min-height: 100vh;
}

.header { grid-column: 1 / -1; }
.sidebar { grid-row: 2; }
.main { grid-row: 2; }
.footer { grid-column: 1 / -1; }
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-05-css-flexbox-grid/grid/`

### Ejemplo 4: Responsive (Tema 07)

```css
/* Mobile-First */
.container {
  padding: 10px;
}

.content {
  flex-direction: column;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    padding: 20px;
  }
  
  .content {
    flex-direction: row;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-07-responsive-design/`

---



### Tema 04: CSS - Posicionamiento y Pseudo-clases/elementos
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-04-css-posicionamiento-pseudo-clases-elementos/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 5: Posicionamiento
- ✅ Sección 6: Pseudo-clases y Pseudo-elementos

**Conceptos cubiertos en el código modelo**:
- Posicionamiento: static, relative, absolute, fixed, sticky
- Pseudo-clases: `:first-child`, `:last-child`, `:nth-child(n)`, `:focus`, `:hover`
- Pseudo-elementos: `::before`, `::after`, `::first-line`, `::placeholder`

---

### Tema 05: CSS - Flexbox y Grid
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-05-css-flexbox-grid/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 7: Flexbox (Diseño Unidimensional)
- ✅ Sección 8: CSS Grid (Diseño Bidimensional)
- ✅ Ejemplo 2: Flexbox
- ✅ Ejemplo 3: Grid

**Conceptos cubiertos en el código modelo**:
- **Flexbox**: `display: flex`, `justify-content`, `align-items`, `flex-direction`, `flex-wrap`, `flex-grow`, `flex-shrink`, `flex-basis`, `order`
- **CSS Grid**: `display: grid`, `grid-template-columns`, `grid-template-rows`, `gap`, `grid-column`, `grid-row`, `grid-area`
- Comparación y casos de uso de cada uno

---

### Tema 06: CSS - Transformaciones, Transiciones y Animaciones
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-06-css-transformaciones-transiciones-animaciones/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 9: Transformaciones
- ✅ Sección 10: Transiciones
- ✅ Sección 11: Animaciones con @keyframes

**Conceptos cubiertos en el código modelo**:
- Transformaciones: `rotate()`, `scale()`, `translate()`, `translateX()`, `translateY()`
- Transiciones: `transition-property`, `transition-duration`, `transition-delay`, `transition-timing-function`
- Animaciones: `@keyframes`, `animation-name`, `animation-duration`, `animation-iteration-count`, `animation-direction`, `animation-fill-mode`
- Animaciones complejas y efectos 3D

---

### Tema 07: Responsive Design
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-07-responsive-design/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 12: Diseño Responsivo y Media Queries
- ✅ Sección 3.5: Unidades de Medida (vw, vh, %, rem, em)
- ✅ Ejemplo 4: Responsive

**Conceptos cubiertos en el código modelo**:
- Viewport meta tag
- Media queries (`@media`)
- Breakpoints comunes (mobile, tablet, desktop)
- Unidades relativas: `vw`, `vh`, `%`, `rem`, `em`
- Imágenes responsivas
- Flexbox y Grid para layouts responsivos

---

### Tema 08: Maquetación Web - Bootstrap
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-08-maquetacion-web-bootstrap/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 13: Bootstrap - Framework CSS

**Conceptos cubiertos en el código modelo**:
- Inclusión de Bootstrap (CDN o archivos locales)
- Sistema de Grid de Bootstrap: `container`, `row`, `col`, `col-sm`, `col-md`, `col-lg`, `col-xl`
- Componentes principales: navbar, carousel, cards, buttons, modal
- Utilidades de Bootstrap
- Personalización de Bootstrap con CSS propio

---

### Tema 09: Práctica HTML, CSS y Bootstrap
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-09-practica-html-css-bootstrap/`

**Secciones de este resumen relacionadas**:
- ✅ Todas las secciones (proyecto integrador)
- ✅ Sección 1: @import y CSS General con Clases/IDs en Body

**Conceptos cubiertos en el código modelo**:
- Integración de HTML semántico, CSS y Bootstrap
- Organización de código en múltiples archivos CSS
- Combinación de CSS personalizado con Bootstrap
- Diseño responsivo funcional
- Uso de componentes de Bootstrap en contexto real

---

## Referencias Relacionadas

### 📚 Material del Docente
- **CSS Guía Maestra**: `material-docente/frontend/CSS-Guia-Maestra.md`
- **HTML Guía Maestra**: `material-docente/frontend/HTML-Guia-Maestra.md`
- **Índice Frontend**: `material-docente/frontend/INDICE-FRONTEND.md`

### 💻 Código Modelo
- **CSS Básico**: `CODIGO/frontend/tema-03-introduccion-css-estilos-basicos/`
- **CSS Flexbox y Grid**: `CODIGO/frontend/tema-05-css-flexbox-grid/`
- **CSS Posicionamiento**: `CODIGO/frontend/tema-04-css-posicionamiento-pseudo-clases-elementos/`
- **CSS Transformaciones**: `CODIGO/frontend/tema-06-css-transformaciones-transiciones-animaciones/`
- **CSS Responsive**: `CODIGO/frontend/tema-07-css-responsive-design/`
- **Bootstrap**: `CODIGO/frontend/tema-08-maquetacion-web-bootstrap/`

### 📖 Material para Estudiantes Relacionado
- **HTML Básico**: `MATERIAL-ESTUDIANTES/frontend/01-HTML-Basico.md`
- **CSS Posicionamiento**: `MATERIAL-ESTUDIANTES/frontend/04-CSS-Posicionamiento.md`
- **CSS Flexbox y Grid**: `MATERIAL-ESTUDIANTES/frontend/05-CSS-Flexbox-Grid.md`
- **CSS Transformaciones**: `MATERIAL-ESTUDIANTES/frontend/06-CSS-Transformaciones.md`
- **CSS Responsive**: `MATERIAL-ESTUDIANTES/frontend/07-CSS-Responsive.md`
- **CSS Bootstrap**: `MATERIAL-ESTUDIANTES/frontend/08-CSS-Bootstrap.md`

**Próximo tema**: JavaScript: Guía Maestra
