# JavaScript: Nivelación Backend 📚

## 📑 Índice

1. [¿Qué es JavaScript? (Analogía del Mundo Real)](#qué-es-javascript-analogía-del-mundo-real)
2. [Variables](#variables)
3. [Tipos de Datos](#tipos-de-datos)
4. [Funciones](#funciones)
5. [Estructuras de Control](#estructuras-de-control)
6. [Métodos de Array](#métodos-de-array)
7. [Scope (Alcance)](#scope-alcance)
8. [Objetos](#objetos)
9. [Conceptos Clave](#conceptos-clave)
10. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es JavaScript? (Analogía del Mundo Real)

### 🧠 Analogía: El Cerebro de la Página Web

Imagina una página web como una persona:
- **HTML**: Es el esqueleto (estructura básica)
- **CSS**: Es la ropa y apariencia (estilos, colores)
- **JavaScript**: Es el cerebro (hace que todo funcione, reacciona, piensa)

**Sin JavaScript**: La página es estática, como una foto.
**Con JavaScript**: La página es interactiva, como una persona que responde.

### 🎮 Analogía: El Videojuego

Piensa en un videojuego:
- **Gráficos** (HTML/CSS): Lo que ves en pantalla
- **Motor del juego** (JavaScript): La lógica que hace que todo funcione
  - Cuando presionas un botón, algo pasa
  - Cuando chocas con algo, reacciona
  - Cuando ganas puntos, se actualiza el marcador

**JavaScript es el "motor"** que hace que tu página web reaccione a las acciones del usuario.

### 🏠 Analogía: La Casa Inteligente

Imagina una casa inteligente:
- **Estructura** (HTML): Las paredes, puertas, ventanas
- **Decoración** (CSS): Colores, muebles, estilo
- **Sistema inteligente** (JavaScript): 
  - Cuando abres la puerta, se enciende la luz
  - Cuando hace calor, se enciende el aire acondicionado
  - Cuando presionas un botón, algo sucede

**JavaScript hace que tu página "reaccione"** a lo que hace el usuario.

### ¿Qué es JavaScript en Programación?

**JavaScript** es un lenguaje de programación que permite hacer páginas web interactivas. Es el lenguaje que hace que las páginas "piensen" y "reaccionen".

**En términos simples**: Es como el "cerebro" de tu página web que le dice qué hacer cuando el usuario hace algo.

---

## Variables

### ¿Qué es una Variable? (Analogía)

### 📦 Analogía: La Caja Etiquetada

Imagina una caja con una etiqueta:
- **Etiqueta** (nombre de variable): "Edad"
- **Contenido** (valor): 20

**Puedes cambiar el contenido** (la edad puede cambiar), pero la etiqueta (nombre) se mantiene.

```javascript
// let: puede cambiar, scope de bloque
let edad = 20
edad = 21  // ✅ Puedes cambiar el valor

// const: no puede cambiar, scope de bloque
const nombre = "Juan"
// nombre = "Pedro"  // ❌ Error: no puedes cambiar

// var: puede cambiar, scope global o de función (evitar usar)
var antigua = "no recomendado"
```

**Analogía**:
- **`let`**: Como una caja que puedes abrir y cambiar su contenido
- **`const`**: Como una caja sellada - no puedes cambiar lo que hay dentro
- **`var`**: Como una caja antigua que puede causar problemas

---

## Tipos de Datos

### 🎁 Analogía: Diferentes Tipos de Cajas

Imagina que tienes diferentes tipos de cajas para guardar diferentes cosas:
- **Caja pequeña** (primitivos): Guarda una sola cosa simple (número, texto, sí/no)
- **Caja grande** (complejos): Guarda múltiples cosas o cosas complejas (objetos, listas)

### Tipos Primitivos

**Analogía**: Como valores simples que no puedes "abrir" más.

- **`number`**: Como un número escrito en un papel
- **`string`**: Como una palabra o frase escrita
- **`boolean`**: Como una luz (encendida = true, apagada = false)
- **`undefined`**: Como una caja vacía que nunca se llenó
- **`null`**: Como una caja que intencionalmente dejaste vacía
- **`symbol`**: Como una etiqueta única e irrepetible

### Tipos Complejos

**Analogía**: Como contenedores que pueden tener múltiples cosas dentro.

- **`object`**: Como una caja con compartimentos (cada compartimento tiene un nombre y un valor)
- **`array`**: Como una lista numerada (posición 0, 1, 2...)
- **`function`**: Como una máquina que hace algo (le das ingredientes, te da un resultado)

---

## Funciones

### 🍳 Analogía: La Receta de Cocina

Imagina una receta de cocina:
- **Nombre de la receta** (nombre de función): "Sumar"
- **Ingredientes** (parámetros): Dos números
- **Pasos** (código): Sumar los números
- **Resultado** (return): El total

**Cada vez que quieres sumar, usas la misma receta** - no escribes los pasos cada vez.

```javascript
// Función declarada (tiene hoisting)
function sumar(a, b) {
    return a + b
}

// Función expresión (no tiene hoisting)
const multiplicar = function(a, b) {
    return a * b
}

// Función flecha (no tiene hoisting, no tiene this)
const restar = (a, b) => a - b
```

**Analogía de hoisting**:
- **Función declarada**: Como tener una herramienta que siempre está disponible, incluso antes de que la necesites
- **Función expresión/flecha**: Como comprar una herramienta - solo la puedes usar después de comprarla

---

## Estructuras de Control

### 🛣️ Analogía: Las Bifurcaciones en el Camino

Imagina que estás conduciendo:
- **Bifurcación** (if/else): Si el semáforo está verde → sigues, si no → te detienes
- **Múltiples caminos** (switch): Diferentes rutas según el día de la semana

```javascript
// if/else - Decisión simple
if (condicion) {
    // código si es verdadero
} else {
    // código si es falso
}

// Ternario - Decisión rápida
const resultado = condicion ? "verdadero" : "falso"

// Switch - Múltiples opciones
switch (valor) {
    case 1:
        // código
        break
    default:
        // código
}
```

**Analogía**:
- **if/else**: Como una pregunta de sí/no
- **Ternario**: Como un atajo para decisiones rápidas
- **Switch**: Como un menú con múltiples opciones

---

## Métodos de Array

### 📋 Analogía: La Lista de Tareas

Imagina una lista de tareas:
- **Lista** (array): `["Comprar leche", "Llamar a mamá", "Estudiar"]`
- **Acciones** (métodos): Puedes revisar cada tarea, filtrar las importantes, transformar la lista

```javascript
// forEach: ejecuta función por cada elemento
// Como revisar cada tarea de la lista
array.forEach(elemento => console.log(elemento))

// map: crea nuevo array transformado
// Como convertir cada tarea en "Tarea: [nombre]"
const duplicados = array.map(num => num * 2)

// filter: crea nuevo array filtrado
// Como separar solo las tareas urgentes
const pares = array.filter(num => num % 2 === 0)

// reduce: reduce array a un valor
// Como sumar el tiempo total de todas las tareas
const suma = array.reduce((acc, num) => acc + num, 0)
```

**Analogía de cada método**:
- **`forEach`**: Revisar cada elemento (como leer cada tarea)
- **`map`**: Transformar cada elemento (como agregar "Urgente: " a cada tarea)
- **`filter`**: Seleccionar elementos (como separar solo las tareas importantes)
- **`reduce`**: Acumular valores (como sumar el tiempo de todas las tareas)

---

## Scope (Alcance)

### 🏠 Analogía: Las Habitaciones de una Casa

Imagina una casa:
- **Habitación global** (scope global): Como el jardín - todos pueden verlo
- **Habitación local** (scope de bloque): Como tu habitación - solo tú puedes ver lo que hay dentro

```javascript
// Scope global - Como el jardín
let global = "soy global"

function ejemplo() {
    // Scope local - Como tu habitación
    let local = "soy local"
    console.log(global) // ✅ Accesible (puedes ver el jardín desde tu habitación)
}

console.log(local) // ❌ Error: no accesible (no puedes ver dentro de la habitación desde afuera)
```

**Regla**: Lo que está en una habitación privada (bloque `{}`) no se ve desde otras habitaciones. Lo que está en el jardín (global) se ve desde todas partes.

---

## Objetos

### 📋 Analogía: La Ficha de Identificación

Imagina una ficha de identificación:
- **Nombre de la propiedad** (clave): "Nombre", "Edad", "Email"
- **Valor de la propiedad**: "Juan", 25, "juan@example.com"

**Un objeto es como esa ficha**: Tiene propiedades (características) con valores.

```javascript
const usuario = {
    nombre: "Juan",
    edad: 25,
    saludar() {
        return `Hola, soy ${this.nombre}`
    }
}

// Acceder a propiedades
usuario.nombre        // "Juan"
usuario["nombre"]     // "Juan"
usuario.saludar()     // "Hola, soy Juan"
```

**Analogía de métodos**:
- **Método** (`saludar()`): Como una acción que el objeto puede hacer
- **`this`**: Como decir "yo mismo" - el objeto se refiere a sí mismo

---

## Conceptos Clave

1. **Variables**: `let` y `const` son preferibles a `var`
   - `let`: Para valores que cambian
   - `const`: Para valores constantes

2. **Funciones**: Diferentes formas de declarar funciones
   - Declaradas: Disponibles antes de declararlas (hoisting)
   - Expresión/Flecha: Disponibles después de declararlas

3. **Arrays**: Métodos modernos (`map`, `filter`, `reduce`)
   - `map`: Transformar cada elemento
   - `filter`: Seleccionar elementos
   - `reduce`: Acumular valores

4. **Scope**: Entender alcance global vs local
   - Global: Accesible desde cualquier lugar
   - Local: Solo accesible dentro del bloque

5. **Objetos**: Estructura y acceso a propiedades
   - Pares clave-valor
   - Métodos (funciones dentro de objetos)

6. **Truthy/Falsy**: Valores que se evalúan como booleanos
   - Falsy: `false`, `0`, `""`, `null`, `undefined`, `NaN`
   - Truthy: Todo lo demás

7. **Comparación**: Usar `===` en lugar de `==`
   - `===`: Compara valor Y tipo (más seguro)
   - `==`: Solo compara valor (puede causar errores)

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [JavaScript: Variables](./10-JS-Variables.md) - Explicación detallada de variables
- 📚 [JavaScript: Funciones](./13-JS-Funciones.md) - Funciones en detalle
- 📚 [JavaScript: Arrays](./12-JS-Arrays.md) - Métodos de array en detalle
- 📚 [TypeScript: Introducción](./03-TypeScript.md) - JavaScript con tipos

### Código Relacionado

- 💻 [Código Backend](../../CODIGO/backend/)

---

## 🎯 Puntos Clave para Recordar

1. **JavaScript = Cerebro**: Hace que la página reaccione
2. **Variables = Cajas etiquetadas**: Guardan valores con nombres
3. **Funciones = Recetas**: Código reutilizable
4. **Arrays = Listas**: Colecciones ordenadas de elementos
5. **Objetos = Fichas**: Propiedades con valores
6. **Scope = Habitaciones**: Variables locales vs globales

---

## 💡 Ejercicio Mental

Piensa en situaciones de la vida real como código:
- **Variable `edad`**: Como tu edad que cambia cada año
- **Función `sumar`**: Como una calculadora que siempre suma
- **Array `tareas`**: Como tu lista de cosas por hacer
- **Objeto `usuario`**: Como tu ficha de identificación

¡Practica identificando estos conceptos en tu vida diaria!
