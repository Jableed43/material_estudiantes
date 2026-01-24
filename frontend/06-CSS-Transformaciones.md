# CSS: Transformaciones, Transiciones y Animaciones ✨

## Transformaciones 🌀

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

## Transiciones (Cambio entre 2 estados) ✨

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

## Animaciones con @keyframes

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

## Ejemplos Prácticos

### Ejemplo 1: Transformación en Hover

```css
.card {
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.05) rotate(2deg);
}
```

### Ejemplo 2: Transición de Color

```css
.button {
  background-color: blue;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.button:hover {
  background-color: darkblue;
  color: white;
}
```

### Ejemplo 3: Animación de Entrada

```css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.element {
  animation: fadeIn 0.5s ease forwards;
}
```

### Ejemplo 4: Animación Continua

```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.loader {
  animation: spin 1s linear infinite;
}
```

### Ejemplo 5: Animación con Múltiples Keyframes

```css
@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.1);
    opacity: 0.8;
  }
}

.element {
  animation: pulse 2s ease-in-out infinite;
}
```

---

## Conceptos Clave

1. **Transformaciones**: Modifican forma, posición u orientación sin afectar el layout
2. **Transiciones**: Cambios suaves entre estados (requieren interacción)
3. **Animaciones**: Movimientos complejos que pueden repetirse
4. **`@keyframes`**: Define los pasos de una animación
5. **`transform-origin`**: Punto desde donde se aplica la transformación
6. **Timing functions**: Controlan la velocidad de la animación
7. **`animation-fill-mode`**: Controla el estado antes/después de la animación

---

## Buenas Prácticas

- Usa transiciones para cambios simples (hover, focus)
- Usa animaciones para movimientos complejos o continuos
- Evita animar propiedades que causan reflow (width, height, margin)
- Prefiere animar `transform` y `opacity` (mejor rendimiento)
- Usa `will-change` con moderación para optimizar
- Considera `prefers-reduced-motion` para accesibilidad
- Limita la duración de animaciones (0.2s - 0.5s para transiciones)

