# CSS: Diseño Responsivo y Media Queries 📱💻

Permiten adaptar los estilos según el ancho de la pantalla. Fundamental seguir la estrategia **Mobile-First**.

## Viewport Meta Tag

```html
<!-- En el <head> del HTML -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

**Es crucial** para que el diseño responsivo funcione correctamente en móviles.

---

## Media Queries

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

---

## Breakpoints Comunes

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

---

## Unidades Relativas para Responsive

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

---

## Imágenes Responsivas

```css
img {
  max-width: 100%; /* No más ancho que el contenedor */
  height: auto; /* Mantiene proporción */
}
```

```html
<!-- HTML con srcset para diferentes resoluciones -->
<img 
  src="imagen.jpg" 
  srcset="imagen-small.jpg 480w, imagen-large.jpg 1200w"
  sizes="(max-width: 768px) 100vw, 50vw"
  alt="Descripción"
>
```

---

## Estrategia Mobile-First

**Mobile-First** significa diseñar primero para móviles y luego agregar estilos para pantallas más grandes.

### Ventajas:
- ✅ Mejor rendimiento (menos código para móviles)
- ✅ Enfoque en contenido esencial
- ✅ Más fácil escalar hacia arriba
- ✅ Mejor experiencia en móviles

### Ejemplo Mobile-First:

```css
/* Estilos base (móvil) */
.container {
  padding: 1rem;
  font-size: 1rem;
}

.card {
  width: 100%;
  margin-bottom: 1rem;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
  
  .card {
    width: 50%;
    display: inline-block;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }
  
  .card {
    width: 33.333%;
  }
}
```

---

## Flexbox y Grid Responsivos

### Flexbox Responsive

```css
.container {
  display: flex;
  flex-direction: column; /* Móvil: columna */
  gap: 1rem;
}

@media (min-width: 768px) {
  .container {
    flex-direction: row; /* Tablet: fila */
  }
}
```

### Grid Responsive

```css
.grid {
  display: grid;
  grid-template-columns: 1fr; /* Móvil: 1 columna */
  gap: 1rem;
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr); /* Tablet: 2 columnas */
  }
}

@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr); /* Desktop: 3 columnas */
  }
}
```

---

## Ejemplos Prácticos

### Ejemplo 1: Navegación Responsive

```css
.nav {
  display: flex;
  flex-direction: column; /* Móvil: vertical */
}

.nav-toggle {
  display: block; /* Visible en móvil */
}

@media (min-width: 768px) {
  .nav {
    flex-direction: row; /* Desktop: horizontal */
  }
  
  .nav-toggle {
    display: none; /* Oculto en desktop */
  }
}
```

### Ejemplo 2: Tipografía Responsive

```css
h1 {
  font-size: 1.5rem; /* Móvil */
}

@media (min-width: 768px) {
  h1 {
    font-size: 2rem; /* Tablet */
  }
}

@media (min-width: 1024px) {
  h1 {
    font-size: 3rem; /* Desktop */
  }
}
```

### Ejemplo 3: Grid de Productos Responsive

```css
.products {
  display: grid;
  grid-template-columns: 1fr; /* Móvil: 1 columna */
  gap: 1rem;
}

@media (min-width: 600px) {
  .products {
    grid-template-columns: repeat(2, 1fr); /* 2 columnas */
  }
}

@media (min-width: 1024px) {
  .products {
    grid-template-columns: repeat(4, 1fr); /* 4 columnas */
  }
}
```

---

## Conceptos Clave

1. **Mobile-First**: Diseña primero para móviles, luego escala
2. **Media Queries**: Aplican estilos según el tamaño de pantalla
3. **Breakpoints**: Puntos donde cambia el diseño
4. **Viewport Units**: `vw`, `vh` para tamaños relativos a la pantalla
5. **Unidades Relativas**: `rem`, `em`, `%` para escalabilidad
6. **Imágenes Responsivas**: `max-width: 100%` y `srcset`
7. **Flexbox/Grid**: Se adaptan naturalmente a diferentes tamaños

---

## Buenas Prácticas

- Siempre incluye el meta viewport tag
- Usa estrategia Mobile-First
- Prueba en diferentes dispositivos
- Usa unidades relativas (`rem`, `%`, `vw`, `vh`)
- Haz imágenes responsivas con `max-width: 100%`
- Usa `clamp()` para tipografía fluida
- Considera `prefers-reduced-motion` para accesibilidad
- Evita valores fijos en píxeles para elementos principales

