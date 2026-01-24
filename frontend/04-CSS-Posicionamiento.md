# CSS: Posicionamiento y Pseudo-clases/Pseudo-elementos 📍

## Posicionamiento

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

## Pseudo-clases y Pseudo-elementos

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

## Ejemplos Prácticos

### Ejemplo 1: Posicionamiento Relative

```css
.box {
  position: relative;
  top: 20px;
  left: 30px;
  background: blue;
}
```

### Ejemplo 2: Posicionamiento Absolute

```css
.parent {
  position: relative; /* Contenedor de referencia */
}

.child {
  position: absolute;
  top: 0;
  right: 0;
  background: red;
}
```

### Ejemplo 3: Posicionamiento Fixed

```css
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: white;
  z-index: 1000;
}
```

### Ejemplo 4: Pseudo-clases en Navegación

```css
nav a {
  color: black;
  text-decoration: none;
}

nav a:hover {
  color: blue;
  text-decoration: underline;
}

nav a:active {
  color: red;
}
```

### Ejemplo 5: Pseudo-elementos para Decoración

```css
.quote::before {
  content: '"';
  font-size: 3em;
  color: gray;
}

.quote::after {
  content: '"';
  font-size: 3em;
  color: gray;
}
```

---

## Conceptos Clave

1. **Posicionamiento Static**: Flujo normal del documento
2. **Posicionamiento Relative**: Se mueve desde su posición original
3. **Posicionamiento Absolute**: Se posiciona respecto al padre posicionado más cercano
4. **Posicionamiento Fixed**: Se posiciona respecto al viewport
5. **Posicionamiento Sticky**: Híbrido entre relative y fixed
6. **Pseudo-clases**: Seleccionan elementos en un estado específico
7. **Pseudo-elementos**: Seleccionan partes específicas de elementos

---

## Buenas Prácticas

- Usa `position: relative` en el contenedor padre cuando uses `position: absolute` en hijos
- `position: fixed` es útil para headers y botones flotantes
- `position: sticky` requiere un valor `top`, `bottom`, `left` o `right`
- Las pseudo-clases `:hover` y `:focus` mejoran la experiencia de usuario
- Los pseudo-elementos `::before` y `::after` requieren la propiedad `content`

