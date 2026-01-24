# CSS: Bootstrap - Framework CSS 🎨

## 📑 Índice

1. [¿Qué es Bootstrap? (Analogía del Mundo Real)](#qué-es-bootstrap-analogía-del-mundo-real)
2. [Inclusión de Bootstrap](#inclusión-de-bootstrap)
3. [Sistema de Grid de Bootstrap](#sistema-de-grid-de-bootstrap)
4. [Componentes Principales](#componentes-principales)
5. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es Bootstrap? (Analogía del Mundo Real)

### 🧱 Analogía: Los Bloques de Construcción Prefabricados

Imagina que estás construyendo una casa:
- **CSS puro**: Como construir todo desde cero, ladrillo por ladrillo
- **Bootstrap**: Como usar bloques prefabricados - ya están hechos, solo los colocas

**Bootstrap te da componentes ya hechos** (botones, cards, navegación) que solo necesitas usar.

### 🎨 Analogía: El Kit de Diseño

Piensa en un kit de diseño profesional:
- **CSS puro**: Como diseñar cada elemento desde cero
- **Bootstrap**: Como tener un kit con elementos ya diseñados profesionalmente

**Bootstrap es como un kit de diseño** con componentes profesionales listos para usar.

### 🏗️ Analogía: La Plantilla de Construcción

Una plantilla de construcción:
- **CSS puro**: Como diseñar cada parte de la casa
- **Bootstrap**: Como tener una plantilla con partes ya diseñadas

**Bootstrap te ahorra tiempo** usando componentes ya diseñados y probados.

Bootstrap es el framework CSS más popular para crear sitios web responsivos rápidamente.

**En términos simples**: Bootstrap es como tener un kit de herramientas CSS con componentes profesionales ya hechos, que solo necesitas usar en tu proyecto.

## Inclusión de Bootstrap

```html
<!-- CDN (recomendado para empezar) -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<!-- O archivos locales -->
<link rel="stylesheet" href="css/bootstrap.min.css">
```

---

## Sistema de Grid de Bootstrap

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

**Ejemplo Responsive**:
```html
<div class="row">
  <div class="col-12 col-md-6 col-lg-4">
    <!-- 12 columnas en móvil, 6 en tablet, 4 en desktop -->
  </div>
</div>
```

---

## Componentes Principales

### Navbar

```html
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
```

### Cards

```html
<div class="card" style="width: 18rem;">
  <img src="..." class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Título</h5>
    <p class="card-text">Contenido</p>
    <a href="#" class="btn btn-primary">Botón</a>
  </div>
</div>
```

### Carousel

```html
<div id="carouselExample" class="carousel slide">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="..." class="d-block w-100" alt="...">
    </div>
  </div>
</div>
```

### Botones

```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>
```

### Formularios

```html
<form>
  <div class="mb-3">
    <label for="email" class="form-label">Email</label>
    <input type="email" class="form-control" id="email">
  </div>
  <div class="mb-3">
    <label for="password" class="form-label">Contraseña</label>
    <input type="password" class="form-control" id="password">
  </div>
  <button type="submit" class="btn btn-primary">Enviar</button>
</form>
```

---

## Personalizar Bootstrap

### Sobrescribir Variables

```css
/* Sobrescribir variables de Bootstrap */
:root {
  --bs-primary: #your-color;
  --bs-font-sans-serif: 'Your Font', sans-serif;
}
```

### CSS Personalizado

```css
/* O con CSS personalizado */
.btn-custom {
  background-color: #custom-color;
  border-color: #custom-color;
}
```

---

## Ejemplos Prácticos

### Ejemplo 1: Layout Básico

```html
<div class="container">
  <div class="row">
    <div class="col-md-8">
      <h1>Contenido Principal</h1>
    </div>
    <div class="col-md-4">
      <h2>Sidebar</h2>
    </div>
  </div>
</div>
```

### Ejemplo 2: Grid de Productos

```html
<div class="container">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-4 mb-4">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Producto 1</h5>
        </div>
      </div>
    </div>
    <!-- Más productos -->
  </div>
</div>
```

### Ejemplo 3: Navegación Responsive

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container">
    <a class="navbar-brand" href="#">Mi Sitio</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <a class="nav-link" href="#">Inicio</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Acerca</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

---

## Utilidades de Bootstrap

### Espaciado

```html
<div class="m-3">Margin en todos los lados</div>
<div class="p-3">Padding en todos los lados</div>
<div class="mt-2">Margin top</div>
<div class="mb-4">Margin bottom</div>
```

### Display

```html
<div class="d-none">Oculto</div>
<div class="d-block">Block</div>
<div class="d-flex">Flex</div>
<div class="d-md-none">Oculto en desktop</div>
```

### Colores

```html
<div class="text-primary">Texto azul</div>
<div class="bg-success">Fondo verde</div>
<div class="border border-primary">Borde azul</div>
```

---

## Conceptos Clave

1. **Container**: Contenedor con ancho máximo y centrado
2. **Row**: Fila que contiene columnas
3. **Col**: Columnas que se adaptan al tamaño de pantalla
4. **Breakpoints**: Puntos donde cambia el layout (sm, md, lg, xl, xxl)
5. **Componentes**: Elementos pre-construidos (navbar, cards, buttons)
6. **Utilidades**: Clases helper para espaciado, colores, display
7. **Grid System**: 12 columnas que se pueden combinar

---

## Buenas Prácticas

- Usa `container` para contenido centrado con ancho máximo
- Usa `container-fluid` para ancho completo
- Combina clases de columna para diferentes breakpoints
- Aprovecha los componentes pre-construidos
- Personaliza con variables CSS cuando sea posible
- Usa utilidades de Bootstrap antes de escribir CSS custom
- Mantén consistencia usando las clases de Bootstrap

