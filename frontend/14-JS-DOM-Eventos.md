# Master Guide: DOM y Gestión de Eventos 🖱️

## ¿Qué es el DOM?

El **DOM (Document Object Model)** es una interfaz de programación que permite a los desarrolladores interactuar con el contenido y la estructura de un documento HTML o XML desde un script, como JavaScript. El DOM representa el documento como una estructura jerárquica de **nodos**, donde cada nodo corresponde a una parte del documento, como un elemento, atributo, o texto.

### Estructura del DOM

- **Nodos**: Todo en el DOM es un nodo, incluyendo elementos HTML, atributos, y el contenido de texto. Los nodos forman una estructura de árbol, donde el nodo superior es el `document`, y los elementos HTML son nodos hijos y descendientes.
- **Elementos**: Son los nodos que representan las etiquetas HTML, como `<div>`, `<p>`, `<a>`, etc.
- **Atributos**: Son los nodos que representan los atributos de los elementos HTML, como `id`, `class`, `src`, etc.
- **Texto**: Es un nodo que contiene el contenido de texto dentro de un elemento.

---

## 1. Selección de Elementos

### ¿Qué es un selector?

Un selector es una expresión que identifica uno o más elementos del DOM para poder manipularlos. JavaScript proporciona varios métodos para seleccionar elementos.

### Métodos de Selección

#### `document.getElementById("id")`
- **Descripción**: Selecciona un único elemento por su atributo `id`.
- **Retorna**: Un elemento HTML o `null` si no se encuentra.
- **Ventaja**: El más rápido y eficiente para selecciones por ID.
- **Ejemplo**:
```javascript
const miElemento = document.getElementById("miId");
```

#### `document.querySelector(".clase")`
- **Descripción**: Selecciona el **primer** elemento que coincida con el selector CSS especificado.
- **Retorna**: Un elemento HTML o `null` si no se encuentra.
- **Ventaja**: Permite usar cualquier selector CSS (clase, ID, atributo, etc.).
- **Ejemplo**:
```javascript
const primerParrafo = document.querySelector("p");
const primerBoton = document.querySelector(".mi-clase");
const elementoPorId = document.querySelector("#miId");
```

#### `document.querySelectorAll("li")`
- **Descripción**: Selecciona **todos** los elementos que coincidan con el selector CSS especificado.
- **Retorna**: Una `NodeList` (similar a un array, pero no es un array real).
- **Ventaja**: Permite trabajar con múltiples elementos a la vez.
- **Ejemplo**:
```javascript
const todosLosItems = document.querySelectorAll("li");
const todosLosBotones = document.querySelectorAll(".boton");
```

#### `document.getElementsByClassName("clase")`
- **Descripción**: Selecciona todos los elementos que tengan la clase especificada.
- **Retorna**: Una `HTMLCollection` (similar a un array).
- **Ejemplo**:
```javascript
const elementos = document.getElementsByClassName("mi-clase");
```

#### `document.getElementsByTagName("tag")`
- **Descripción**: Selecciona todos los elementos por nombre de etiqueta.
- **Retorna**: Una `HTMLCollection`.
- **Ejemplo**:
```javascript
const links = document.getElementsByTagName("a");
```

---

## 2. Manipulación de Contenido y Atributos

### Manipulación de Contenido

#### `.textContent`
- **Descripción**: Obtiene o establece el contenido de texto de un elemento (sin HTML).
- **Ventaja**: Más seguro que `innerHTML` (no ejecuta código HTML).
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.textContent = "Nuevo texto"; // Establece
console.log(elemento.textContent);    // Obtiene
```

#### `.innerHTML`
- **Descripción**: Obtiene o establece el contenido HTML de un elemento, incluyendo etiquetas HTML.
- **Ventaja**: Permite insertar HTML completo.
- **Precaución**: Puede ser vulnerable a XSS si se usa con contenido del usuario sin sanitizar.
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.innerHTML = "<strong>Texto en negrita</strong>";
```

#### `.value`
- **Descripción**: Obtiene o establece el valor de elementos de formulario (`<input>`, `<select>`, `<textarea>`).
- **Ejemplo**:
```javascript
const input = document.getElementById("miInput");
input.value = "Nuevo valor"; // Establece
console.log(input.value);    // Obtiene
```

### Manipulación de Atributos

#### `.setAttribute("attr", "valor")`
- **Descripción**: Establece el valor de un atributo en un elemento.
- **Ejemplo**:
```javascript
const imagen = document.getElementById("miImagen");
imagen.setAttribute("src", "nueva-imagen.jpg");
imagen.setAttribute("alt", "Descripción de la imagen");
```

#### `.getAttribute("attr")`
- **Descripción**: Obtiene el valor de un atributo en un elemento.
- **Ejemplo**:
```javascript
const imagen = document.getElementById("miImagen");
const src = imagen.getAttribute("src");
```

#### `.removeAttribute("attr")`
- **Descripción**: Elimina un atributo de un elemento.
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.removeAttribute("disabled");
```

---

## 3. Manipulación de Estilos

### `.style.property`
- **Descripción**: Cambia el CSS directamente en el elemento (estilos inline).
- **Ventaja**: Cambios inmediatos y específicos.
- **Desventaja**: Mezcla lógica con presentación (no es la mejor práctica).
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.style.color = "red";
elemento.style.backgroundColor = "blue";
elemento.style.fontSize = "20px";
```

### `.classList` (Recomendado)
- **Descripción**: Una lista que facilita la manipulación de clases CSS de forma profesional.
- **Ventaja**: Separa lógica de presentación, más mantenible.

#### Métodos de `classList`:

##### `.classList.add("clase")`
- **Descripción**: Agrega una o más clases al elemento.
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.classList.add("activo");
elemento.classList.add("clase1", "clase2"); // Múltiples clases
```

##### `.classList.remove("clase")`
- **Descripción**: Remueve una o más clases del elemento.
- **Ejemplo**:
```javascript
elemento.classList.remove("activo");
elemento.classList.remove("clase1", "clase2"); // Múltiples clases
```

##### `.classList.toggle("clase")`
- **Descripción**: Alterna una clase (la agrega si no existe, la remueve si existe).
- **Ejemplo**:
```javascript
elemento.classList.toggle("activo"); // Si tiene "activo", lo quita; si no, lo agrega
```

##### `.classList.contains("clase")`
- **Descripción**: Verifica si el elemento tiene una clase específica.
- **Retorna**: `true` o `false`.
- **Ejemplo**:
```javascript
if (elemento.classList.contains("activo")) {
    console.log("El elemento está activo");
}
```

---

## 4. Creación y Eliminación de Elementos

### Crear Elementos

#### `document.createElement("tagName")`
- **Descripción**: Crea un nuevo elemento HTML.
- **Ejemplo**:
```javascript
const nuevoDiv = document.createElement("div");
const nuevoParrafo = document.createElement("p");
```

### Insertar Elementos

#### `.appendChild(newNode)`
- **Descripción**: Añade un nuevo nodo como hijo del elemento, al final de los hijos existentes.
- **Ejemplo**:
```javascript
const contenedor = document.getElementById("contenedor");
const nuevoElemento = document.createElement("div");
nuevoElemento.textContent = "Hola";
contenedor.appendChild(nuevoElemento);
```

#### `.insertBefore(newNode, referenceNode)`
- **Descripción**: Inserta un nodo antes del nodo de referencia.
- **Ejemplo**:
```javascript
const contenedor = document.getElementById("contenedor");
const nuevoElemento = document.createElement("div");
const referencia = document.getElementById("referencia");
contenedor.insertBefore(nuevoElemento, referencia);
```

#### `.replaceChild(newNode, oldNode)`
- **Descripción**: Reemplaza un nodo hijo con un nuevo nodo.
- **Ejemplo**:
```javascript
const contenedor = document.getElementById("contenedor");
const nuevoElemento = document.createElement("div");
const viejoElemento = document.getElementById("viejo");
contenedor.replaceChild(nuevoElemento, viejoElemento);
```

### Eliminar Elementos

#### `.removeChild(childNode)`
- **Descripción**: Elimina un nodo hijo de un elemento.
- **Ejemplo**:
```javascript
const contenedor = document.getElementById("contenedor");
const hijo = document.getElementById("hijo");
contenedor.removeChild(hijo);
```

#### `.remove()`
- **Descripción**: Elimina el elemento directamente (método más moderno).
- **Ejemplo**:
```javascript
const elemento = document.getElementById("miElemento");
elemento.remove();
```

---

## 5. Gestión de Eventos 🎧

### ¿Qué son los Eventos?

Un **evento** en JavaScript es una acción o suceso que ocurre en el sistema (el navegador o el documento HTML) al que JavaScript puede "escuchar" y reaccionar. Estos eventos pueden ser desencadenados por el usuario (como hacer clic, presionar una tecla, mover el mouse) o por el propio navegador (como la carga de la página, un error en una imagen).

Para reaccionar a estos eventos, utilizamos **Event Listeners** (escuchadores de eventos), que son funciones de JavaScript que se "adjuntan" a elementos específicos del DOM y se ejecutan cuando el evento asociado ocurre.

### `addEventListener()`

La forma más común y recomendada de adjuntar eventos es con `addEventListener()`:

```javascript
elemento.addEventListener('nombreDelEvento', funcionManejadora);
```

#### Estructura Completa
```javascript
elemento.addEventListener("tipoEvento", (event) => {
    // Código que se ejecuta cuando ocurre el evento
}, opciones);
```

#### Ejemplo Básico
```javascript
const miBoton = document.getElementById("miBoton");
miBoton.addEventListener("click", function() {
    console.log("¡Botón clickeado!");
});
```

### Tipos Comunes de Eventos

#### Eventos del Mouse
- **`click`**: Se activa cuando el usuario hace clic (presiona y suelta el botón principal) sobre un elemento.
- **`dblclick`**: Se activa cuando el usuario hace doble clic sobre un elemento.
- **`mousedown`**: Se activa cuando se presiona cualquier botón del mouse sobre un elemento (antes de soltarlo).
- **`mouseup`**: Se activa cuando se suelta cualquier botón del mouse sobre un elemento (después de presionarlo).
- **`mousemove`**: Se activa continuamente mientras el puntero del mouse se mueve sobre un elemento.
- **`mouseover`**: Se activa cuando el puntero del mouse entra en el área de un elemento o de uno de sus hijos.
- **`mouseout`**: Se activa cuando el puntero del mouse sale del área de un elemento o de uno de sus hijos.
- **`mouseenter`**: Se activa cuando el puntero del mouse entra en el área de un elemento, sin burbujear desde sus hijos.
- **`mouseleave`**: Se activa cuando el puntero del mouse sale del área de un elemento, sin burbujear desde sus hijos.

#### Eventos de Teclado
- **`keydown`**: Se activa cuando se presiona cualquier tecla. Se dispara repetidamente si la tecla se mantiene pulsada.
- **`keyup`**: Se activa cuando se suelta una tecla.
- **`keypress`**: Se activa cuando se presiona una tecla que produce un carácter (obsoleto en gran medida, `keydown` y `keyup` son más robustos).

#### Eventos de Formulario
- **`submit`**: Se activa cuando se envía un formulario. Es fundamental usar `event.preventDefault()` aquí para evitar que el navegador recargue la página si quieres manejar el envío con JavaScript.
- **`change`**: Se activa cuando el valor de un elemento de formulario (`<input>`, `<select>`, `<textarea>`) cambia y el elemento pierde el foco (`blur`).
- **`input`**: Se activa inmediatamente cada vez que el valor de un `<input>` o `<textarea>` cambia, incluso mientras el usuario está escribiendo.
- **`focus`**: Se activa cuando un elemento (como un campo de texto o un botón) recibe el foco (ej. al hacer clic o usar `Tab`).
- **`blur`**: Se activa cuando un elemento pierde el foco.

#### Eventos de Carga y Descarga
- **`DOMContentLoaded`**: Se activa en el `document` cuando el HTML ha sido completamente cargado y parseado, y el DOM está listo para ser manipulado. No espera a que se carguen imágenes o stylesheets externos. Es el evento más común para iniciar tu script de JavaScript.
- **`load`**: Se activa en `window` cuando la página completa (incluyendo imágenes, stylesheets, scripts externos, etc.) ha terminado de cargarse.
- **`beforeunload`**: Se activa en `window` justo antes de que el usuario abandone la página (al cerrar la pestaña, navegar a otra URL, etc.).

### Propiedades Útiles del Evento

#### `event.target`
- **Descripción**: El elemento exacto que disparó el evento.
- **Ejemplo**:
```javascript
boton.addEventListener("click", function(event) {
    console.log(event.target); // El botón que fue clickeado
});
```

#### `event.preventDefault()`
- **Descripción**: Detiene la acción por defecto del evento (ej: no enviar formulario, no seguir un enlace).
- **Ejemplo**:
```javascript
formulario.addEventListener("submit", function(event) {
    event.preventDefault(); // Evita que el formulario se envíe
    // Tu lógica aquí
});
```

#### `event.stopPropagation()`
- **Descripción**: Evita que el evento se propague a otros elementos padres (detiene el burbujeo).
- **Ejemplo**:
```javascript
boton.addEventListener("click", function(event) {
    event.stopPropagation(); // El evento no subirá a elementos padres
    // Tu lógica aquí
});
```

---

## 6. Propagación de Eventos: Captura, Burbujeo y `stopPropagation()`

### ¿Qué es la Propagación de Eventos?

Cuando haces clic en un elemento dentro de una página web, ese clic no solo "ocurre" en el elemento que ves, sino que también viaja a través de la jerarquía del Document Object Model (DOM). Este viaje se conoce como **propagación de eventos**, y tiene dos fases principales: la fase de captura y la fase de burbujeo.

### Fase de Captura (Capturing Phase)

La fase de captura es cuando el evento "baja" desde el ancestro más alto del DOM (como `window`, `document`, `<html>`, `<body>`) hacia el elemento específico que fue clickeado (el elemento objetivo).

Para activar la fase de captura, debes pasar `true` como tercer parámetro en `addEventListener()`:

```javascript
externo.addEventListener("click", () => {
    console.log("Captura: click en DIV EXTERNO");
}, true); // El 'true' activa la fase de captura
```

**Orden**: En la consola, verías el mensaje de captura antes de cualquier mensaje de burbujeo.

### Fase de Burbujeo (Bubbling Phase)

La fase de burbujeo es la más común y el comportamiento por defecto. Después de que el evento ha alcanzado el elemento objetivo y se han ejecutado los listeners de captura (si los hay), el evento "sube" de vuelta por la jerarquía del DOM, desde el elemento objetivo hacia sus padres, abuelos, y así sucesivamente, hasta llegar a la raíz.

```javascript
externo.addEventListener("click", () => {
    console.log("Burbujeo: click en DIV EXTERNO");
}); // Sin el 'true', es burbujeo por defecto

interno.addEventListener("click", () => {
    console.log("Burbujeo: click en DIV INTERNO");
});
```

**Orden**: Si haces clic en `DIV INTERNO`, verías primero "Burbujeo: click en DIV INTERNO" y luego "Burbujeo: click en DIV EXTERNO".

### Detener la Propagación: `event.stopPropagation()`

A veces no quieres que un evento siga viajando por el DOM. Aquí es donde entra `event.stopPropagation()`:

```javascript
interno.addEventListener("click", (e) => {
    e.stopPropagation(); // Detiene el viaje del evento
    console.log("Evento detenido en DIV INTERNO");
});
```

**Comportamiento**: Cuando haces clic en `DIV INTERNO`:
1. Si hay un listener de captura en `DIV EXTERNO`, se dispara primero.
2. Luego, el evento llega a `DIV INTERNO` y se ejecuta el listener con `stopPropagation()`.
3. Después de ejecutar `stopPropagation()`, el evento no seguirá burbujeando hacia arriba hacia `DIV EXTERNO`.

---

## 7. DOMContentLoaded

### ¿Por qué es importante?

Cuando intentas manipular el DOM antes de que esté completamente cargado, puedes obtener errores porque los elementos aún no existen. `DOMContentLoaded` resuelve este problema.

### Uso

```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Tu código aquí - el DOM está listo
    const elemento = document.getElementById("miElemento");
    elemento.textContent = "Hola";
});
```

**Ventaja**: No espera a que se carguen imágenes o stylesheets externos, solo el HTML, por lo que es más rápido que `window.onload`.

---

## 8. localStorage (Persistencia de Datos)

### ¿Qué es localStorage?

`localStorage` es una API del navegador que permite almacenar datos de tipo clave-valor (pares de strings) de forma persistente en el cliente. Esto significa que los datos no se pierden cuando el usuario cierra el navegador o la pestaña, y están disponibles la próxima vez que visite la misma página.

### Métodos Principales

#### `localStorage.setItem(key, value)`
- **Descripción**: Guarda un par clave-valor. Tanto la clave como el valor deben ser strings.
- **Para objetos/arrays**: Primero deben convertirse a un string JSON usando `JSON.stringify()`.
- **Ejemplo**:
```javascript
localStorage.setItem("nombre", "Juan");
localStorage.setItem("tareas", JSON.stringify(["Tarea 1", "Tarea 2"]));
```

#### `localStorage.getItem(key)`
- **Descripción**: Recupera un valor asociado a una clave. Devuelve un string (o `null` si la clave no existe).
- **Para objetos/arrays**: Debe convertirse de nuevo usando `JSON.parse()`.
- **Ejemplo**:
```javascript
const nombre = localStorage.getItem("nombre");
const tareas = JSON.parse(localStorage.getItem("tareas"));
```

#### `localStorage.removeItem(key)`
- **Descripción**: Elimina un par clave-valor específico.
- **Ejemplo**:
```javascript
localStorage.removeItem("nombre");
```

#### `localStorage.clear()`
- **Descripción**: Elimina todos los datos de localStorage.
- **Ejemplo**:
```javascript
localStorage.clear();
```

**Ventaja**: Permite que las aplicaciones web "recuerden" datos del usuario entre sesiones, mejorando la experiencia (ej. gestores de tareas, preferencias de usuario).

---

## 9. `this` en Event Listeners

### ¿Qué es `this`?

`this` es una palabra clave en JavaScript cuyo valor no es fijo, sino que se determina dinámicamente al momento en que una función es ejecutada. Su valor depende de cómo se llama la función.

### En Funciones Tradicionales

El valor de `this` varía según el contexto de invocación:
- Si se llama como método de un objeto (`objeto.metodo()`), `this` referencia a ese objeto.
- Si se llama directamente (`funcion()`), `this` referencia al objeto global (`window` en navegadores, `undefined` en modo estricto).
- Si se usa con `new` (constructor), `this` referencia a la nueva instancia creada.
- Se puede forzar su valor con `call()`, `apply()`, o `bind()`.

### En Funciones Flecha

Las funciones flecha no tienen su propio `this`. En su lugar, capturan y heredan el valor de `this` del ámbito léxico (contexto circundante) donde fueron definidas. Esto las hace muy útiles en callbacks, ya que mantienen el `this` de su entorno padre sin importar cómo se las invoque.

**Ejemplo**:
```javascript
const boton = document.getElementById("miBoton");

// Función tradicional
boton.addEventListener("click", function() {
    console.log(this); // 'this' referencia al botón
});

// Función flecha
boton.addEventListener("click", () => {
    console.log(this); // 'this' referencia al contexto donde se definió la flecha
});
```

---

## 10. Casos de Uso del DOM

- **Manipulación Dinámica del Contenido**: Cambiar el texto, imágenes, y enlaces en respuesta a acciones del usuario, como en sitios web interactivos o aplicaciones SPA (Single Page Applications).
- **Validación de Formularios**: Validar entradas del usuario en formularios antes de enviarlos al servidor.
- **Creación y Manipulación de Elementos Dinámicos**: Añadir, modificar o eliminar elementos del DOM en respuesta a eventos, como en listas de tareas o interfaces de usuario dinámicas.
- **Gestión de Eventos**: Capturar y responder a eventos del usuario como clics, movimientos del ratón, y teclas pulsadas, para mejorar la interacción y la experiencia del usuario.
- **Accesibilidad**: Modificar atributos del DOM para mejorar la accesibilidad, como el uso de atributos `aria-` para que las aplicaciones sean más accesibles para personas con discapacidades.

---

## 11. Buenas Prácticas

1. **Siempre usar `DOMContentLoaded`**: Cuando manipulas el DOM, asegúrate de que esté completamente cargado.
2. **Preferir `classList` sobre `style` directo**: Mejor práctica para mantener la separación de lógica y presentación.
3. **Usar `textContent` en lugar de `innerHTML` cuando sea posible**: Más seguro y evita vulnerabilidades XSS.
4. **Usar `addEventListener` en lugar de atributos HTML**: Más flexible y mantenible.
5. **Limpiar event listeners cuando sea necesario**: Para evitar memory leaks en aplicaciones complejas.
6. **Validar que los elementos existan**: Antes de manipular elementos, verifica que no sean `null`.

---

## 12. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: Manipulación de Inputs en Tiempo Real

```javascript
document.addEventListener("DOMContentLoaded", function() {
    const textoInput = document.querySelector("#textoInput");
    const colorInput = document.querySelector("#colorInput");
    const fontSizeInput = document.querySelector("#fontSizeInput");
    const resultado = document.querySelector("#resultado");

    function actualizarTexto() {
        let texto = textoInput.value;
        resultado.textContent = texto;
    }

    function actualizarColor() {
        let color = colorInput.value;
        resultado.style.color = color;
    }

    function actualizarFontSize() {
        let fontSize = `${fontSizeInput.value}px`;
        resultado.style.fontSize = fontSize;
    }

    textoInput.addEventListener("input", actualizarTexto);
    colorInput.addEventListener("input", actualizarColor);
    fontSizeInput.addEventListener("input", actualizarFontSize);
});
```

### Ejemplo 2: Selector de Temas con classList

```javascript
document.addEventListener('DOMContentLoaded', function() {
    const nombreInput = document.getElementById('nombreInput');
    const nombre = document.getElementById('nombre');
    const tarjeta = document.getElementById('tarjeta');
    const temaBtns = document.querySelectorAll('.tema-btn');
    const resetBtn = document.getElementById('reset');

    // Actualizar nombre en tiempo real
    nombreInput.addEventListener('input', function() {
        nombre.textContent = nombreInput.value || 'Tu Nombre';
    });

    // Cambiar tema
    temaBtns.forEach(btn => {
        btn.addEventListener('click', function() {
            const tema = this.getAttribute('data-tema');
            tarjeta.classList.remove('tema-claro', 'tema-oscuro', 'tema-colorido');
            tarjeta.classList.add(`tema-${tema}`);
        });
    });

    // Resetear
    resetBtn.addEventListener('click', function() {
        nombreInput.value = '';
        nombre.textContent = 'Tu Nombre';
        tarjeta.classList.remove('tema-claro', 'tema-oscuro', 'tema-colorido');
    });
});
```

---

