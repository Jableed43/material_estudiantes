# Master Guide: HTML5 y Estructura Web 🏗️

## 1. Fundamentos de HTML

**HTML (HyperText Markup Language)** es el lenguaje estándar que define la estructura y el contenido de una página web mediante una jerarquía de etiquetas.

### Estructura Base de un Documento

Cada archivo `.html` debe comenzar con esta plantilla mínima:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <!-- El viewport es CRUCIAL para el diseño responsivo -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Título en la Pestaña</title>
</head>
<body>
    <!-- El contenido visible para el usuario va aquí -->
</body>
</html>
```

**Elementos clave de la estructura**:
- `<!DOCTYPE html>`: Declara que es un documento HTML5
- `<html lang="es">`: Elemento raíz, `lang` ayuda a accesibilidad y SEO
- `<head>`: Metadatos (no visibles, pero importantes)
- `<body>`: Contenido visible de la página

---

## 2. Etiquetas Básicas de Texto y Estructura

### Encabezados (Headings)

Los encabezados crean una jerarquía visual y semántica. **Usa un solo `<h1>` por página** para el título principal.

```html
<h1>Soy un h1 - Título Principal</h1>
<h2>Soy un h2 - Subtítulo</h2>
<h3>Soy un h3 - Sección</h3>
<h4>Soy un h4 - Subsección</h4>
<h5>Soy un h5</h5>
<h6>Soy un h6</h6>
```

**Regla importante**: No te saltes niveles (ej: no uses `<h1>` seguido de `<h3>` sin `<h2>`).

### Párrafos y Texto

```html
<p>Soy un párrafo. El contenido va aquí.</p>
<em>Soy un em - texto enfatizado (cursiva semántica)</em>
<strong>Soy un strong - texto importante (negrita semántica)</strong>
<br> <!-- Salto de línea -->
```

**Diferencias**:
- `<em>` vs `<i>`: `<em>` es semántico (énfasis), `<i>` es solo visual
- `<strong>` vs `<b>`: `<strong>` es semántico (importancia), `<b>` es solo visual

### Enlaces (Links)

```html
<!-- Enlace básico -->
<a href="http://www.google.com">Ir a Google</a>

<!-- Enlace que abre en nueva pestaña -->
<a href="https://ejemplo.com" target="_blank">Abrir en nueva pestaña</a>
```

**Atributos importantes**:
- `href`: URL de destino (puede ser relativa o absoluta)
- `target="_blank"`: Abre en nueva pestaña

---

## 3. Semántica y Organización 🧠

La semántica ayuda a los buscadores (SEO) y a las tecnologías de asistencia (accesibilidad) a entender qué partes de tu web son las más importantes.

### Elementos Semánticos Principales

```html
<header>
    <!-- Introducción o grupo de navegación -->
    <nav>
        <!-- Enlaces de navegación principal -->
        <a href="/">Inicio</a>
        <a href="/sobre">Sobre</a>
    </nav>
</header>

<main>
    <!-- El contenido dominante y único de la página -->
    <section>
        <!-- Agrupamiento temático de contenido -->
        <article>
            <!-- Contenido independiente y distribuible (ej: un post) -->
        </article>
    </section>
    
    <aside>
        <!-- Contenido tangencial (barras laterales) -->
    </aside>
</main>

<footer>
    <!-- Información de autoría, contacto o copyright -->
</footer>
```

**Uso recomendado**:
- `<header>`: Logo, navegación principal, título
- `<nav>`: Menús de navegación
- `<main>`: Contenido principal (solo uno por página)
- `<section>`: Agrupa contenido relacionado
- `<article>`: Contenido independiente (posts, noticias)
- `<aside>`: Información complementaria
- `<footer>`: Información de pie de página

---

## 4. Comportamiento de los Elementos (Display) 📦

Es fundamental entender cómo se posicionan los elementos de forma natural en el documento.

### Elementos en Bloque (`block`)

**Características**:
- **Abarcan el 100% del ancho disponible** de su contenedor padre
- **Salto de línea automático**: Siempre comienzan en una fila nueva
- **Aceptan dimensiones**: Puedes definir `width` y `height`
- **Márgenes completos**: `margin` y `padding` funcionan en todas las direcciones

**Ejemplos comunes**:
- `<div>`: contenedor genérico
- `<p>`: párrafo
- `<h1>`, `<h2>`, ..., `<h6>`: encabezados
- `<ul>`, `<ol>`: listas
- `<li>`: ítems de lista
- `<section>`, `<article>`, `<aside>`, `<header>`, `<footer>`: elementos semánticos
- `<form>`, `<table>`, `<hr>`, `<nav>`

**Ejemplo**:
```html
<div>Este div ocupa todo el ancho disponible</div>
<p>Este párrafo está en una nueva línea</p>
```

### Elementos en Línea (`inline`)

**Características**:
- **Solo ocupan el ancho necesario** para su contenido
- **Sin salto de línea**: Pueden convivir varios en la misma fila
- **Limitaciones importantes**:
  - No aceptan `width` o `height` (se ignoran)
  - El `margin` y `padding` vertical no afecta al flujo de otros elementos
  - Solo `margin` y `padding` horizontal funcionan correctamente

**Ejemplos comunes**:
- `<span>`: contenedor en línea genérico para texto
- `<a>`: enlaces
- `<strong>`, `<b>`: texto en negrita
- `<em>`, `<i>`: texto en cursiva
- `<img>`: imágenes
- `<input>`, `<label>`, `<select>`, `<textarea>` (algunos se comportan como inline o inline-block)

**Ejemplo**:
```html
<span>Texto 1</span>
<span>Texto 2</span>
<span>Texto 3</span>
<!-- Los tres aparecen en la misma línea -->
```

### El Híbrido: `inline-block`

**Características**:
- Fluyen horizontalmente como los de línea (pueden estar en la misma fila)
- **Aceptan dimensiones** (ancho/alto) como los de bloque
- **Márgenes completos** como los de bloque
- Muy útil para botones, tarjetas pequeñas, o elementos que necesitas alinear horizontalmente pero con dimensiones

**Ejemplo**:
```html
<button>Botón 1</button>
<button>Botón 2</button>
<!-- Los botones están en la misma línea pero tienen width/height -->
```

**En CSS**:
```css
.elemento {
    display: inline-block;
    width: 200px;
    height: 100px;
}
```

---

## 5. Listas 📋

### Listas Desordenadas (`<ul>`)

```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
```

### Listas Ordenadas (`<ol>`)

```html
<ol>
    <li>Primer paso</li>
    <li>Segundo paso</li>
    <li>Tercer paso</li>
</ol>
```

**Uso común**: Navegación, menús, pasos de un proceso, características de un producto.

---

## 6. Multimedia e Interactividad 🎥

### Imágenes

```html
<!-- Imagen local -->
<img src="assets/img/imagen.jpg" alt="Descripción de la imagen" width="400" height="400">

<!-- Imagen remota -->
<img src="https://ejemplo.com/imagen.jpg" alt="Descripción">
```

**Atributos importantes**:
- `src`: Ruta de la imagen (local o URL)
- `alt`: **Obligatorio para accesibilidad** - descripción de la imagen
- `width` y `height`: Dimensiones (opcional, mejor usar CSS)

**Nota**: El atributo `alt` es crucial para:
- Accesibilidad (lectores de pantalla)
- SEO (optimización para buscadores)
- Cuando la imagen no carga, muestra el texto alternativo

### Audio

```html
<audio src="assets/audio/audio.mp3" controls>
    Tu navegador no soporta audio.
</audio>
```

**Atributos**:
- `src`: Ruta del archivo de audio
- `controls`: Muestra controles de reproducción
- `autoplay`: Reproduce automáticamente (usar con precaución)
- `loop`: Repite el audio

### Video

```html
<!-- Video con controles -->
<video src="assets/video/video.mp4" controls width="400" height="400"></video>

<!-- Video tipo kiosco (autoplay, loop, muted) -->
<video src="assets/video/video.mp4" autoplay loop muted width="400" height="400"></video>
```

**Atributos**:
- `src`: Ruta del archivo de video
- `controls`: Muestra controles de reproducción
- `width` y `height`: Dimensiones
- `autoplay`: Reproduce automáticamente
- `loop`: Repite el video
- `muted`: Sin sonido (útil con autoplay)

### Iframes (Elementos Embebidos)

Los `<iframe>` permiten embebir contenido de otras páginas web dentro de tu página.

```html
<!-- YouTube -->
<iframe 
    width="560" 
    height="315" 
    src="https://www.youtube.com/embed/VIDEO_ID" 
    title="YouTube video player" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
    referrerpolicy="strict-origin-when-cross-origin" 
    allowfullscreen>
</iframe>

<!-- Google Maps -->
<iframe 
    src="https://www.google.com/maps/embed?pb=..." 
    width="600" 
    height="450" 
    style="border:0;" 
    allowfullscreen="" 
    loading="lazy">
</iframe>

<!-- Spotify -->
<iframe 
    src="https://open.spotify.com/embed/track/TRACK_ID" 
    width="300" 
    height="380" 
    frameborder="0" 
    allowtransparency="true" 
    allow="encrypted-media">
</iframe>
```

**Uso común**: Embebir videos de YouTube, mapas de Google Maps, música de Spotify, contenido de otras plataformas.

**Nota**: Los iframes actúan como una "ventana" a otro sitio web. El contenido se carga desde el servidor externo.

---

## 7. Formularios Básicos 📝

Los formularios permiten la entrada de datos por parte del usuario.

```html
<form>
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="userName" placeholder="Escribe aquí...">
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="userEmail" placeholder="tu@email.com">
    
    <label for="mensaje">Mensaje:</label>
    <textarea id="mensaje" name="userMessage" placeholder="Escribe tu mensaje..."></textarea>
    
    <select name="pais">
        <option value="arg">Argentina</option>
        <option value="mx">México</option>
        <option value="es">España</option>
    </select>
    
    <input type="radio" id="opcion1" name="opcion" value="1">
    <label for="opcion1">Opción 1</label>
    
    <input type="checkbox" id="acepto" name="acepto">
    <label for="acepto">Acepto los términos</label>
    
    <button type="submit">Enviar</button>
    <button type="reset">Limpiar</button>
</form>
```

**Elementos importantes**:
- `<form>`: Contenedor del formulario
- `<label>`: Etiqueta asociada a un input (usar `for` con `id` del input)
- `<input>`: Campo de entrada (múltiples tipos: `text`, `email`, `password`, `radio`, `checkbox`, etc.)
- `<textarea>`: Área de texto multilínea
- `<select>`: Menú desplegable
- `<button>`: Botón (tipo `submit` para enviar, `reset` para limpiar)

**Tipos de input comunes**:
- `text`: Texto simple
- `email`: Email (validación básica)
- `password`: Contraseña (oculta el texto)
- `number`: Números
- `date`: Fecha
- `radio`: Opción única (mismo `name` para grupo)
- `checkbox`: Opción múltiple

**Nota**: Para que el formulario funcione, necesitas un backend que procese los datos. En HTML puro, solo defines la estructura.

---

## 8. Ocultando Elementos: La Diferencia Clave

### `display: none;`

**Comportamiento**:
- El elemento desaparece por completo
- **No se ve y no ocupa espacio**
- Es como si no existiera en el HTML para el navegador

**Uso común**:
- Ocultar elementos dinámicamente con JavaScript
- Menús que se muestran/ocultan al hacer clic
- Elementos que cambian según el estado de la aplicación

**Ejemplo**:
```html
<div id="menu" style="display: none;">
    <!-- Este div no se ve y no ocupa espacio -->
</div>
```

### `visibility: hidden;`

**Comportamiento**:
- El elemento es invisible
- **Pero el hueco que ocupaba se mantiene**
- El navegador reserva el espacio en el layout

**Uso común**:
- Cuando quieres ocultar algo pero mantener el lugar reservado
- Útil para interfaces donde no quieres que se muevan los elementos al ocultar algo
- Transiciones donde el espacio debe mantenerse

**Ejemplo**:
```html
<div style="visibility: hidden;">
    <!-- Este div es invisible pero ocupa espacio -->
</div>
```

**Comparación visual**:
```
display: none;        → [Elemento desaparece completamente]
visibility: hidden;   → [   ] (espacio vacío pero reservado)
```

---

## 9. Rutas: Locales vs Remotas

### Rutas Relativas (Locales)

```html
<!-- Archivo en la misma carpeta -->
<img src="imagen.jpg">

<!-- Archivo en subcarpeta -->
<img src="assets/img/imagen.jpg">

<!-- Archivo en carpeta padre -->
<img src="../imagen.jpg">
```

### Rutas Absolutas (Remotas)

```html
<!-- URL completa -->
<img src="https://ejemplo.com/imagen.jpg">
<a href="https://www.google.com">Google</a>
```

**Diferencia clave**:
- **Relativas**: Dependen de la ubicación del archivo HTML
- **Absolutas**: URL completa, independiente de la ubicación

---

## 10. Buenas Prácticas y Recomendaciones ✅

### Accesibilidad

1. **Siempre usa `alt` en imágenes**: Es obligatorio para accesibilidad
2. **Usa etiquetas semánticas**: `<header>`, `<nav>`, `<main>`, etc.
3. **Asocia labels con inputs**: Usa `for` en `<label>` con `id` en `<input>`
4. **Jerarquía de encabezados**: No saltes niveles (h1 → h2 → h3)

### SEO

1. **Un solo `<h1>` por página**: Título principal
2. **Usa `<title>` descriptivo**: Aparece en la pestaña y resultados de búsqueda
3. **Meta viewport**: Esencial para diseño responsivo
4. **Atributo `lang`**: Ayuda a los buscadores

### Estructura

1. **HTML semántico**: Usa `<header>`, `<main>`, `<footer>`, etc.
2. **Comentarios útiles**: `<!-- Comentario -->` para explicar secciones
3. **Indentación consistente**: Facilita la lectura del código
4. **Nombres descriptivos**: IDs y clases con nombres claros

---

## 11. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: Estructura Básica (Tema 01)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Soy un h1</h1>
    <h2>Soy un h2</h2>
    <h3>Soy un h3</h3>
    <p>Soy un parrafo</p>
    <em>Soy un em</em>
    <br>
    <strong>Soy un strong</strong>
    <br>
    <a href="http://www.google.com">Ir a google</a>
</body>
</html>
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-01-introduccion-html-etiquetas-basicas/`

### Ejemplo 2: Multimedia Completo (Tema 02)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multimedia</title>
</head>
<body>
    <header>
        <a href="http://www.google.com">Ir a google</a>
    </header>
    
    <main>
        <h1>Soy un h1</h1>
        <p>Hola soy un parrafo!</p>

        <!-- Multimedia locales -->
        <audio src="assets/audio/Naruto.wav" controls>Cancion</audio>
        <br>
        
        <video src="assets/video/medi.mp4" controls width="400" height="400"></video>
        <br>
        
        <img src="assets/img/Skate_Styles.jpg" alt="imagen skate" width="400" height="400">
    
        <!-- Multimedia remoto - YouTube -->
        <iframe 
            width="560" 
            height="315" 
            src="https://www.youtube.com/embed/G3LmiPpjMgY" 
            title="YouTube video player" 
            frameborder="0" 
            allowfullscreen>
        </iframe>
    </main>
</body>
</html>
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-02-html-multimedia-elementos-embebidos/`

---

## 12. Proyecto Práctico Sugerido 🎯

Basado en el material del docente, aquí tienes una consigna para practicar:

### Consigna: Mi Proyecto Web Personal

**Objetivo**: Crear un proyecto web personal para aplicar y reforzar los conceptos de HTML.

**Elementos a incluir**:

1. **Estructura y Títulos**:
   - Usa `<header>`, `<main>`, `<title>`
   - Diferentes niveles de encabezado (`<h1>` a `<h6>`)

2. **Contenido de Texto**:
   - Párrafos `<p>`
   - Al menos una lista (ordenada `<ol>` o desordenada `<ul>`)

3. **Elementos Multimedia**:
   - Una imagen `<img>` (con `alt` descriptivo)
   - Un video de YouTube embebido (`<iframe>`)
   - Un mapa de Google Maps embebido (`<iframe>`)
   - Opcional: Audio `<audio>`

4. **Interactividad**:
   - Enlaces `<a>` a páginas externas

5. **Formularios (Opcional)**:
   - Formulario simple `<form>` con:
     - `<label>` y `<input>` (texto)
     - `<textarea>` (área de texto)
     - `<select>` (menú desplegable)
     - Radio o checkbox

**Instrucciones**:
1. Crea tu entorno: Abre Visual Studio Code y un navegador
2. Escribe el código: Comienza con la estructura básica y ve añadiendo elementos
3. Guarda y actualiza: Guarda con extensión `.html` y actualiza el navegador
4. Experimenta: Si algo no funciona, consulta el material o busca en Google

---


