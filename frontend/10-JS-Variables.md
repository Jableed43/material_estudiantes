# JavaScript: Variables, Operadores y Tipos de Datos 📦

## 📑 Índice

1. [Introducción a JavaScript](#1-introducción-a-javascript)
2. [Tipos de Datos](#2-tipos-de-datos)
   - Tipos Primitivos (Inmutables)
   - Tipos Objetos (Colecciones)
3. [Variables y Scope (Alcance)](#3-variables-y-scope-alcance-)
   - ¿Qué es una Variable? (Analogía)
   - Tipos de Variables: var, let, const
   - Scope (Alcance): Global, Bloque
4. [Operadores Clave](#4-operadores-clave)
   - Operadores Aritméticos
   - Operadores de Comparación
   - Operadores Lógicos
5. [Estructuras de Control](#5-estructuras-de-control-)
   - Condicionales: if/else, ternario
   - Switch
   - Truthy y Falsy
6. [Funciones](#6-funciones-)
   - ¿Qué es una Función? (Analogía)
   - Tipos de Funciones
   - Parámetros y Return
7. [Bucles (Loops)](#7-bucles-loops-)
   - for, while, do...while
8. [Destructuring (Desestructuración)](#8-destructuring-desestructuración)
9. [Trabajar con Objetos](#9-trabajar-con-objetos)
10. [Buenas Prácticas](#10-buenas-prácticas-y-recomendaciones-)
11. [Ejemplos Prácticos del Código Modelo](#11-ejemplos-prácticos-del-código-modelo)
12. [Referencias Relacionadas](#referencias-relacionadas)

---

## 1. Introducción a JavaScript

**JavaScript** es un lenguaje de programación de **alto nivel, interpretado y dinámico**. Es el lenguaje esencial para añadir interactividad a la web, permitiendo manipular el contenido (DOM), responder a eventos y comunicarse con servicios externos.

### ¿Qué es JavaScript?

JavaScript es un lenguaje de programación que:
- **Se ejecuta en el navegador**: Permite crear páginas web interactivas
- **Es interpretado**: No necesita compilación, se ejecuta directamente
- **Es dinámico**: Los tipos de datos se determinan en tiempo de ejecución
- **Es multiparadigma**: Soporta programación orientada a objetos, funcional e imperativa

### JavaScript vs Otros Lenguajes

- **Node.js**: Permite ejecutar JavaScript fuera del navegador (entorno de servidor), facilitando el desarrollo Full-Stack
- **TypeScript**: Es un superconjunto de JavaScript que añade tipos estáticos
- **Java**: Aunque el nombre es similar, son lenguajes completamente diferentes

### ¿Dónde se ejecuta JavaScript?

1. **En el navegador**: Para interactividad web (DOM, eventos, APIs)
2. **En el servidor**: Con Node.js para backend
3. **En aplicaciones móviles**: Con frameworks como React Native
4. **En aplicaciones de escritorio**: Con Electron

---

## 2. Tipos de Datos

Los tipos de datos en JavaScript se dividen en dos categorías principales: **primitivos** (inmutables) y **objetos** (colecciones).

### ¿Qué es un Tipo de Dato?

Un **tipo de dato** define qué tipo de valor puede almacenar una variable y qué operaciones se pueden realizar con él.

### Tipos Primitivos (Inmutables)

Los tipos primitivos son valores simples que **no pueden modificarse** una vez creados. Si "modificas" un primitivo, en realidad estás creando un nuevo valor.

#### 1. String (Cadena de Texto)

**¿Qué es?**: Secuencia de caracteres (letras, números, símbolos).

```javascript
// Tres formas de crear strings
let nombre1 = "Hola";        // Comillas dobles
let nombre2 = 'Mundo';       // Comillas simples
let nombre3 = `Template`;    // Template literals (backticks)

// Template literals permiten interpolación
let mensaje = `Hola, ${nombre1}!`; // "Hola, Hola!"
```

**Características**:
- ✅ Inmutables (no se pueden modificar directamente)
- ✅ Se pueden concatenar con `+` o template literals
- ✅ Tienen propiedades y métodos (`.length`, `.toUpperCase()`, etc.)

**Ejemplos del código modelo**:
```javascript
"hola", "hola", `hola`;  // Tres formas válidas
```


#### 2. Number (Número)

**¿Qué es?**: Números enteros o decimales. JavaScript no diferencia entre enteros y flotantes.

```javascript
let entero = 42;
let decimal = 3.14;
let notacionCientifica = 1e9;        // 1000000000
let negativo = -10;
let infinito = Infinity;
let noEsNumero = NaN;                // Not a Number
```

**Características**:
- ✅ Incluye valores especiales: `Infinity`, `-Infinity`, `NaN`
- ✅ Precisión limitada para números muy grandes
- ✅ Operaciones aritméticas estándar

**Ejemplos del código modelo**:
```javascript
20, 1.5, 1e9, 1.6666666;  // Entero, decimal, notación científica, decimal largo
```

#### 3. Boolean (Booleano)

**¿Qué es?**: Valores lógicos que representan verdadero o falso.

```javascript
let esVerdadero = true;
let esFalso = false;
```

**Uso común**: Condicionales, validaciones, lógica de control.

#### 4. null

**¿Qué es?**: Valor intencional que representa "ausencia de valor" o "valor nulo". Es un valor asignado explícitamente por el desarrollador.

```javascript
let mascota = null;  // Campo existe pero no tiene valor
```

**Diferencia con `undefined`**: `null` es un valor asignado intencionalmente, `undefined` significa que nunca se asignó un valor.

#### 5. undefined

**¿Qué es?**: Valor que indica que una variable fue declarada pero nunca se le asignó un valor.

```javascript
let variable;        // undefined
let otra = undefined; // undefined (explícito)
```

**Cuándo aparece**:
- Variable declarada sin valor
- Propiedad de objeto que no existe
- Función sin return

#### 6. Symbol (Símbolo)

**¿Qué es?**: Identificador único e inmutable. Se usa principalmente para crear propiedades privadas en objetos.

```javascript
let id = Symbol('id');
let usuario = {
  [id]: 123
};
```

**Uso**: Avanzado, principalmente para propiedades privadas.

### Tipos Objetos (Colecciones)

Los objetos son estructuras de datos que pueden contener múltiples valores y métodos.

#### 1. Object (Objeto)

**¿Qué es?**: Colección de pares **clave-valor** (propiedades). Permite representar entidades del mundo real.

```javascript
// Sintaxis básica
let persona = {
  nombre: "Javier",
  apellido: "Lopez",
  edad: 33,
  activo: true
};

// Acceso a propiedades
console.log(persona.nombre);        // "Javier"
console.log(persona["apellido"]);  // "Lopez"

// Modificar propiedades
persona.edad = 34;

// Agregar nuevas propiedades
persona.ciudad = "Buenos Aires";
```

**Características**:
- ✅ Pares clave-valor: `{ clave: valor }`
- ✅ Acceso con `.` o `[]`
- ✅ Puede contener cualquier tipo de dato
- ✅ Puede contener funciones (métodos)
- ✅ Puede anidarse (objetos dentro de objetos)

**Ejemplos del código modelo**:
```javascript
var profesor1 = {
  nombre: "javier",
  apellido: "lopez",
  foto: "https://..."
};

// Objetos anidados
let persona = {
  nombre: "javier",
  profesion: {
    nombre: "Desarrollo web",
    desde: 2020,
    seniority: "ssr"
  },
  hobbies: ["lectura", "musica", "cocina"]
};
```

**Convenciones de nombres**:
- **Camel Case** (recomendado): `esTitular`, `nombreCompleto`
- **Snake Case**: `es_titular`, `nombre_completo`

#### 2. Array (Arreglo)

**¿Qué es?**: Lista ordenada de elementos. Los elementos pueden ser de cualquier tipo.

```javascript
// Crear array
let frutas = ["manzana", "banana", "naranja"];

// Acceso por índice (empieza en 0)
console.log(frutas[0]);  // "manzana"
console.log(frutas[1]);  // "banana"

// Longitud
console.log(frutas.length);  // 3

// Modificar elementos
frutas[0] = "pera";

// Agregar elementos
frutas.push("uva");  // Al final
```

**Características**:
- ✅ Índices empiezan en 0 (no en 1)
- ✅ Último índice: `length - 1`
- ✅ Puede contener cualquier tipo de dato
- ✅ Puede contener objetos y otros arrays

**Ejemplos del código modelo**:
```javascript
var profesores = [
  { nombre: "leandro", apellido: "gutierrez", mascota: null, es_titular: true },
  { nombre: "camila", apellido: "gonzalez", mascota: 1, es_titular: false },
  { nombre: "javier", apellido: "lopez", mascota: 2, es_titular: true }
];
```

#### 3. Function (Función)

**¿Qué es?**: Bloque de código reutilizable que puede recibir parámetros y retornar valores.

```javascript
function saludar(nombre) {
  return `Hola, ${nombre}`;
}
```

**Características**:
- ✅ Es un tipo de dato (puede asignarse a variables)
- ✅ Puede pasarse como argumento
- ✅ Puede retornarse desde otras funciones

---

## 3. Variables y Scope (Alcance) 📦

### ¿Qué es una Variable? (Analogía del Mundo Real)

### 📦 Analogía: La Caja de Almacenamiento

Imagina que tienes una **caja de almacenamiento** con una etiqueta:
- **Etiqueta** (nombre de la variable): "Libros"
- **Contenido** (valor): Los libros que guardas

Puedes:
- **Guardar cosas** en la caja (asignar valor)
- **Cambiar el contenido** (reasignar valor)
- **Ver qué hay dentro** (leer el valor)
- **Usar la etiqueta** para referirte a la caja (usar el nombre de la variable)

### 🏷️ Analogía: Las Etiquetas en el Supermercado

En un supermercado, cada producto tiene una etiqueta con su precio:
- **Etiqueta del producto** (nombre de variable): "Leche"
- **Precio** (valor): $150

Si el precio cambia, solo cambias la etiqueta, pero el nombre del producto sigue siendo "Leche". La variable funciona igual: el nombre no cambia, pero el valor sí puede cambiar.

### 🎒 Analogía: La Mochila Escolar

Piensa en tu mochila:
- **Nombre de la mochila** (nombre de variable): "MiMochila"
- **Contenido** (valor): Libros, cuadernos, lápices

Puedes:
- **Agregar cosas** (asignar valores)
- **Sacar cosas** (cambiar valores)
- **Ver qué hay dentro** (leer valores)
- **Usar el nombre** para referirte a ella (usar la variable)

### ¿Qué es una Variable en Programación?

Una **variable** es un contenedor con nombre que almacena un valor. Permite guardar datos para usarlos más tarde.

**En términos simples**: Es como una caja etiquetada donde guardas información que puedes usar, cambiar o leer cuando la necesites.

### Declaración vs Asignación

- **Declaración**: Crear la variable (`let x;`)
- **Asignación**: Darle un valor (`x = 5;`)
- **Inicialización**: Declarar y asignar en una línea (`let x = 5;`)

### Tipos de Variables

| Palabra Clave | Scope (Ámbito) | ¿Reasignable? | ¿Redeclarable? | Nota |
| :--- | :--- | :--- | :--- | :--- |
| `var` | Función / Global | Sí | Sí | ⚠️ No recomendada (Hoisting confuso) |
| `let` | Bloque `{}` | Sí | No | ✅ Ideal para valores que cambian |
| `const` | Bloque `{}` | No | No | ✅ Protege el valor o la referencia |

### var (No Recomendada)

**Analogía**: Como tener múltiples cajas con el mismo nombre en diferentes lugares de tu casa. Puede ser confuso saber cuál estás usando.

```javascript
var colorTaza = "gris";
colorTaza = "rojo";        // Reasignación permitida
var colorTaza = "azul";     // Redeclaración permitida (PROBLEMA)
```

**Características**:
- ❌ Permite redeclaración (puede causar errores)
- ❌ Scope de función (no de bloque)
- ❌ Hoisting confuso
- ⚠️ **No usar en código moderno**

**Problema real**: Si accidentalmente declaras `var` dos veces con el mismo nombre, no te avisa del error. Es como tener dos cajas con la misma etiqueta y no saber cuál usar.

**Ejemplo del código modelo**:
```javascript
var colorTaza = "gris"
colorTaza = "rojo"
var colorTaza = "azul"  // Permite redeclaración (problema)
```

### let (Recomendada para Valores que Cambian)

**Analogía**: Como una caja con una etiqueta única. Puedes cambiar lo que hay dentro, pero no puedes tener dos cajas con la misma etiqueta en el mismo lugar.

```javascript
let contador = 0;
contador = 1;        // ✅ Reasignación permitida (cambias el contenido)
let contador = 2;    // ❌ Error: no permite redeclaración (no puedes tener dos cajas con el mismo nombre)
```

**Características**:
- ✅ Scope de bloque `{}` (solo existe dentro de las llaves)
- ✅ No permite redeclaración (protege contra errores)
- ✅ Más segura que `var`

**Analogía del scope**: Como una habitación en tu casa. Lo que está dentro de la habitación (bloque `{}`) solo se ve dentro de esa habitación. No puedes verlo desde otras habitaciones.

**Ejemplo del código modelo**:
```javascript
// Variable global
let color_calzado = "negro"

function calzado() {
    // Variable local (bloque)
    let color_calzado = "Blanco"  // No afecta la global
    console.log("Dentro de la funcion calzado ", color_calzado)
}
```

### const (Recomendada para Valores Constantes)

**Analogía**: Como una caja fuerte sellada. No puedes cambiar la caja (la referencia), pero si la caja contiene objetos, puedes modificar esos objetos.

```javascript
const PI = 3.14159;
PI = 3.14;  // ❌ Error: no permite reasignación (no puedes cambiar la caja)
```

**Características**:
- ✅ Scope de bloque `{}`
- ✅ No permite reasignación (no puedes cambiar la caja)
- ✅ No permite redeclaración
- ⚠️ **Importante**: Si es objeto o array, el contenido SÍ se puede modificar

**Analogía con objetos/arrays**:
```javascript
// La caja (const) no se puede cambiar
const miCaja = ["libro", "lápiz"]  // Caja sellada

// Pero el contenido SÍ se puede modificar
miCaja.push("goma")  // ✅ Permitido (agregas cosas a la caja)
// miCaja = []       // ❌ Error (no puedes cambiar la caja misma)
```

**Piensa en**: Una caja fuerte (const) que contiene objetos. No puedes cambiar la caja, pero sí puedes agregar o quitar objetos de dentro.

**Ejemplo del código modelo**:
```javascript
const nombre = "brandon"  // No se puede cambiar

// Pero con arrays/objetos, el contenido sí se puede modificar
const nombres = ["brandon", "miguel", "jose"]
nombres.push("javier")  // ✅ Permitido (modifica contenido, no referencia)
```

### Scope (Alcance)

**¿Qué es el Scope?**: El **scope** (alcance) determina dónde una variable es accesible en el código.

### 🏠 Analogía: Las Habitaciones de una Casa

Imagina una casa con diferentes habitaciones:

- **Habitación Global** (scope global): Como el jardín - todos pueden verlo
- **Habitación Local** (scope de bloque): Como tu habitación - solo tú puedes ver lo que hay dentro

**Regla**: Lo que está en una habitación privada (bloque `{}`) no se ve desde otras habitaciones. Lo que está en el jardín (global) se ve desde todas partes.

#### Scope Global

Variables declaradas fuera de cualquier función o bloque.

```javascript
let global = "Soy global";

function miFuncion() {
  console.log(global);  // ✅ Accesible
}
```

#### Scope de Bloque

Variables declaradas dentro de `{}` (bloques, funciones, condicionales).

```javascript
if (true) {
  let bloque = "Soy local";
  console.log(bloque);  // ✅ Accesible
}
console.log(bloque);  // ❌ Error: no accesible fuera del bloque
```

**Ejemplo del código modelo**:
```javascript
// Variable global
let color_calzado = "negro"

function calzado() {
    // Variable local - en bloque
    let color_calzado = "Blanco"  // No afecta la global
    console.log("Dentro de la funcion calzado ", color_calzado)
}

function calzado2() {
    console.log(color_calzado)  // Accede a la global
}
```

### console.log() - Herramienta de Depuración

**¿Qué es?**: `console.log()` es una herramienta que permite ver datos, resultados de operaciones y valores de variables en la consola del navegador.

```javascript
let nombre = "Javier";
console.log(nombre);              // "Javier"
console.log("Hola", nombre);      // "Hola Javier"
console.log(2 + 2);               // 4
```

**Cómo usar**:
1. Abre la consola del navegador: `F12` → pestaña "Console"
2. Escribe `console.log()` en tu código
3. Los valores aparecerán en la consola

**Ejemplo del código modelo**:
```javascript
// Es una herramienta que permite ver datos, resultados de operaciones, valores de una variable
console.log("antes de la funcion calzado ", color_calzado)
```

---

## 4. Operadores Clave

### ¿Qué es un Operador? (Analogía)

### 🧮 Analogía: Las Operaciones Matemáticas

Imagina que tienes una calculadora:
- **Números** (operandos): Los valores que usas
- **Símbolos** (operadores): `+`, `-`, `*`, `/` que realizan operaciones

**En programación es igual**: Los operadores realizan operaciones sobre valores.

### 🔢 Analogía: Comparar Objetos

Piensa en comparar dos objetos:
- **¿Son iguales?** → `===` (igualdad estricta)
- **¿Uno es mayor?** → `>` (mayor que)
- **¿Uno es menor?** → `<` (menor que)

**En programación**: Los operadores de comparación te ayudan a tomar decisiones comparando valores.

### ¿Qué es un Operador?

Un **operador** es un símbolo que realiza una operación sobre uno o más valores (operandos).

**En términos simples**: Son como las herramientas que usas para trabajar con datos: sumar, comparar, combinar, etc.

### Operadores Aritméticos

Realizan operaciones matemáticas.

```javascript
// Básicos
let suma = 5 + 3;        // 8
let resta = 10 - 4;      // 6
let multiplicacion = 3 * 4;  // 12
let division = 15 / 3;   // 5

// Módulo (resto de división)
let resto = 10 % 3;      // 1 (útil para pares/impares)
let esPar = 8 % 2 === 0;  // true

// Exponenciación
let potencia = 2 ** 3;   // 8 (2 elevado a 3)

// Raíz cuadrada (método Math)
let raiz = Math.sqrt(16);  // 4
```

**Operadores de asignación compuestos**:
```javascript
let numero = 10;
numero += 5;   // numero = numero + 5  → 15
numero -= 3;   // numero = numero - 3  → 12
numero *= 2;   // numero = numero * 2  → 24
numero /= 4;   // numero = numero / 4  → 6
```

**Ejemplos del código modelo**:
```javascript
// Operadores básicos
let suma = 5 + 3;
let resta = 10 - 4;
let multiplicacion = 3 * 4;
let division = 15 / 3;

// Módulo (útil para pares/impares)
let resto = 10 % 3;  // 1
let esPar = 8 % 2 === 0;  // true

// Exponenciación
let potencia = 2 ** 3;  // 8

// Math.sqrt() - Raíz cuadrada
let raiz = Math.sqrt(16);  // 4
```

### Operadores de Comparación

Comparan valores y retornan `true` o `false`.

```javascript
// Igualdad (NO recomendado - compara solo valores)
5 == "5"   // true (convierte tipos)

// Igualdad estricta (RECOMENDADO - compara valor Y tipo)
5 === "5"  // false
5 === 5    // true

// Desigualdad
5 != "5"   // false (convierte tipos)
5 !== "5"  // true (estricto)

// Mayor/Menor
10 > 5     // true
10 < 5     // false
10 >= 10   // true
10 <= 9    // false
```

**⚠️ Importante**: Siempre usar `===` y `!==` en lugar de `==` y `!=` para evitar errores de tipo.

**Ejemplos del código modelo**:
```javascript
// Igualdad (solo valor)
5 == "5"   // true (convierte tipos)

// Igualdad estricta (valor Y tipo) - RECOMENDADO
5 === "5"  // false
5 === 5    // true

// Desigualdad
5 != "5"   // false
5 !== "5"  // true
```

### Operadores Lógicos

**Analogía**: Como las reglas de decisión en la vida real.

#### 🚦 Analogía: El Semáforo

Imagina un semáforo:
- **AND (&&)**: Para cruzar, el semáforo DEBE estar en verde **Y** no debe venir ningún auto
  - Si el semáforo está verde **Y** no viene auto → ✅ Puedes cruzar
  - Si el semáforo está verde **PERO** viene un auto → ❌ No cruzas
  - **Ambas condiciones deben ser verdaderas**

- **OR (||)**: Puedes cruzar si el semáforo está en verde **O** si no viene ningún auto
  - Si el semáforo está verde → ✅ Puedes cruzar (aunque venga auto)
  - Si no viene auto → ✅ Puedes cruzar (aunque el semáforo esté rojo)
  - **Al menos una condición debe ser verdadera**

- **NOT (!)**: Lo opuesto
  - Si NO está lloviendo → Puedes salir
  - Si está lloviendo → NO puedes salir

Combinan o invierten valores booleanos.

#### Tabla de Verdad

**AND (&&) - Conjunción**:

| A | B | A && B |
|---|---|--------|
| `true` | `true` | `true` |
| `true` | `false` | `false` |
| `false` | `true` | `false` |
| `false` | `false` | `false` |

**OR (||) - Disyunción**:

| A | B | A \|\| B |
|---|---|---------|
| `true` | `true` | `true` |
| `true` | `false` | `true` |
| `false` | `true` | `true` |
| `false` | `false` | `false` |

**NOT (!) - Negación**:

| A | !A |
|---|----|
| `true` | `false` |
| `false` | `true` |

#### Resumen de Operadores Lógicos

- **NOT (`!`)**: Es lo opuesto al resultado lógico. Si el valor es `true`, retorna `false`; si es `false`, retorna `true`.
- **AND (`&&`)**: Si es AND, entonces **todos los valores deben dar `true` para que sea `true`**. Si cualquier valor es `false`, el resultado es `false`.
- **OR (`||`)**: Si es OR, **todos los valores deben ser `false` para que sea `false`**. Si al menos uno es `true`, el resultado es `true`.

#### Ejemplos Prácticos

```javascript
// AND (&&) - Todos deben ser verdaderos
true && true    // true
true && false   // false (uno es false, resultado false)
false && true   // false
false && false  // false
5 > 3 && 10 > 5  // true (ambas condiciones son true)

// OR (||) - Al menos uno debe ser verdadero
true || false   // true (uno es true, resultado true)
false || true   // true
false || false  // false (ambos son false, resultado false)
5 > 10 || 10 > 5  // true (segunda condición es true)

// NOT (!) - Invierte el valor
!true   // false (opuesto de true)
!false  // true (opuesto de false)
!(5 > 10)  // true (opuesto de false)
```

**Ejemplos del código modelo**:
```javascript
// AND / && / Y - Todos los términos deben ser verdaderos
if (temperatura <= 10 && frio) {
    console.log("Me pongo campera")
}

// OR / || / O - Al menos uno debe ser verdadero
if (temperatura > 10 || !frio) {
    console.log("No me abrigo")
}
```

---

## 5. Estructuras de Control 🛣️

### ¿Qué son las Estructuras de Control?

Las **estructuras de control** permiten que el programa tome decisiones y ejecute código condicionalmente o repetidamente.

### Condicionales

Permiten ejecutar código solo si se cumple una condición.

#### if / else / else if

```javascript
// Estructura básica
if (edad >= 18) {
  console.log("Acceso Permitido");
} else if (edad > 13) {
  console.log("Acceso con tutor");
} else {
  console.log("Acceso Denegado");
}
```

**Ejemplos del código modelo**:
```javascript
let frio = true
let temperatura = 10

if(frio){
    console.log("Me abrigo")
} else {
    console.log("No me abrigo")
}

// Else if - Múltiples condiciones
if(temperatura <= 10){
    console.log("Me pongo campera")
} else if (temperatura > 10 && temperatura < 18){
    console.log("Me pongo un buzo")
} else {
    console.log("No me abrigo")
}
```

#### Truthy y Falsy

**¿Qué son?**: Valores que se evalúan como `true` o `false` en contextos booleanos.

**Valores Falsy** (se evalúan como `false`):
```javascript
false
0
-0
""          // String vacío
null
undefined
NaN
```

**Valores Truthy** (se evalúan como `true`):
```javascript
"hola"      // String no vacío
" "         // String con espacio
true
1           // Cualquier número != 0
-20
{}          // Objetos (aunque estén vacíos)
[]          // Arrays (aunque estén vacíos)
function(){} // Funciones
Symbol('id')
```

**Ejemplos del código modelo**:
```javascript
// True - Truthy
let truthy = [ "hola", " ", true, 1, -20, {}, [], function(){}, Symbol('id') ]

// False - Falsy
let falsy = [ false, 0, -0, "", null, undefined, NaN ]

let condicion = false

if (condicion) {
    console.log("Es truthy")
} else {
    console.log("Es falsy")
}
```

#### Operador Ternario

**¿Qué es?**: Forma abreviada de escribir un `if/else` simple.

```javascript
// Sintaxis: condicion ? valorSiTrue : valorSiFalse

// Forma larga
if (frio) {
    console.log("Me abrigo")
} else {
    console.log("No me abrigo")
}

// Forma corta (ternario)
frio ? console.log("Me abrigo") : console.log("No me abrigo")

// Con retorno de valor
let mensaje = edad >= 18 ? "Mayor de edad" : "Menor de edad";
```

**Ejemplos del código modelo**:
```javascript
// If ternario - reducido
frio ? console.log("Me abrigo") : console.log("No me abrigo")

// Ternario anidado
temperatura <= 10 ? console.log("Me pongo campera")
: temperatura > 10 && temperatura < 18 ?
console.log("Me pongo un buzo")
: console.log("No me abrigo")
```

### Switch (Múltiples Casos)

**¿Qué es?**: Estructura para comparar una variable contra múltiples valores fijos.

**⚠️ Importante**: En `switch`, el valor del parámetro se espera que sea el resultado en el `case` del mismo valor. Es decir, `switch` compara el valor de la variable con cada `case` usando igualdad estricta (`===`).

```javascript
let dia = "lunes";

switch (dia) {
  case "lunes":
    console.log("Inicio de semana");
    break;  // ⚠️ IMPORTANTE: Sin break, continúa al siguiente caso
  case "viernes":
    console.log("Fin de semana");
    break;
  case "sabado":
  case "domingo":
    console.log("Fin de semana");
    break;
  default:
    console.log("Día no reconocido");
}
```

**Cómo funciona**:
1. Evalúa el valor de la variable (`dia` en el ejemplo)
2. Compara ese valor con cada `case` usando `===` (igualdad estricta)
3. Si encuentra coincidencia, ejecuta el código de ese `case`
4. Continúa ejecutando los siguientes `case` hasta encontrar `break` o llegar al final
5. Si no hay coincidencias, ejecuta `default` (si existe)

**⚠️ Importante**: Usa `break` para evitar que el código "caiga" al siguiente caso.

**Cuándo usar**:
- ✅ Múltiples condiciones posibles para una variable
- ✅ Valores fijos (no rangos)
- ❌ No usar para rangos (mejor `if/else`)

**Ejemplo del código modelo**:
```javascript
// Cuando utilizamos switch-case?
// Cuando tenemos muchos casos posibles 

let anioNacimiento = 1993

switch(true){
    case anioNacimiento >= 1920 && anioNacimiento <= 1945:
        console.log("Generacion silenciosa")
    break;
    case anioNacimiento >= 1946 && anioNacimiento <= 1964:
        console.log("Baby boomer")
    break;
    default:
        console.log("Sos de otra generacion")
    break;
}
```

---

## 6. Funciones 🛠️

### ¿Qué es una Función? (Analogía del Mundo Real)

### 🍳 Analogía: La Receta de Cocina

Imagina una receta de cocina:
- **Nombre de la receta** (nombre de función): "Hacer Pizza"
- **Ingredientes** (parámetros): Harina, queso, tomate
- **Pasos** (código): Mezclar, amasar, hornear
- **Resultado** (return): Una pizza lista

**Cada vez que quieres pizza, sigues la misma receta** - no inventas una nueva cada vez. Eso es una función: código reutilizable.

### 🏭 Analogía: La Máquina de la Fábrica

Piensa en una máquina de una fábrica:
- **Entrada** (parámetros): Materias primas
- **Proceso** (código): La máquina trabaja
- **Salida** (return): Producto terminado

**Cada vez que pones las mismas materias primas, obtienes el mismo producto**. Eso es una función: misma entrada, mismo proceso, misma salida.

### 🎯 Analogía: El Lanzador de Dardos

Un lanzador de dardos profesional:
- **Preparación** (definir función): Aprende la técnica
- **Lanzamiento** (llamar función): Ejecuta la técnica
- **Resultado** (return): Dardo en el blanco

**Cada vez que lanza, usa la misma técnica** - no inventa una nueva cada vez.

### ¿Qué es una Función en Programación?

Una **función** es un bloque de código reutilizable que realiza una tarea específica. Permite:
- **DRY (Don't Repeat Yourself)**: No repetir código
- **Mantenibilidad**: Un solo lugar para cambios
- **Modularización**: Separar responsabilidades
- **Reutilización**: Usar el mismo código múltiples veces

### ¿Por qué usar Funciones?

**Problemas de repetir código**:
- ❌ Si la función cambia, debes cambiarla en todas partes
- ❌ Aumenta la cantidad de bugs
- ❌ Dificulta el mantenimiento

**Ventajas de usar funciones**:
- ✅ Mantenible: un solo lugar para cambios
- ✅ Única fuente de la verdad
- ✅ Separación de responsabilidades
- ✅ Facilita lectura del código
- ✅ Ahorra tiempo, disminuye errores

**Ejemplos del código modelo**:
```javascript
/* Funciones
Codigo reutilizable para tareas especificas
Dont repeat yourself - DRY

Que permiten las funciones?
- Estructurar codigo con buenas practicas
- Modularizar / Componetizacion
- Facil lectura del codigo
- Mejor mantenimiento
- Reutilizar codigo
- Ahorramos tiempo, disminuimos errores
*/
```

### Momentos de una Función

1. **Definición (Implementación)**: Donde creamos la función y definimos sus pasos
2. **Invocación (Llamado/Call)**: Donde nombramos la función para ejecutarla

```javascript
// 1. Definición
function saludar(nombre) {
  return `Hola, ${nombre}`;
}

// 2. Invocación
saludar("Javier");  // "Hola, Javier"
```

### Tipos de Funciones

#### 1. Función Declarada

**Características**:
- ✅ Tiene **hoisting** (se puede usar antes de declararla)
- ✅ Sintaxis tradicional
- ✅ Puede ser llamada desde cualquier lugar

```javascript
// Se puede llamar antes de declararla (hoisting)
console.log(calcularArea(7, 2));  // ✅ Funciona

function calcularArea(largo, ancho) {
  return largo * ancho;
}
```

**Ejemplos del código modelo**:
```javascript
// 1. Funcion declarada - La mas comun
// Te permite ejecutarla antes de declararla
// Tiene hoisting

function calcularArea(largo, ancho){
    // El retorno devuelve el resultado de la funcion
    // El retorno finaliza la ejecucion
    // Permite reutilizar el resultado de la funcion
    return largo * ancho
}

// El argumento son los valores que usan los parametros cuando se ejecuta la funcion
console.log(calcularArea(7, 2))
```

#### 2. Función de Expresión

**Características**:
- ❌ No tiene hoisting (no se puede usar antes de declararla)
- ✅ Asignada a una variable
- ✅ Útil para funciones anónimas

```javascript
// ❌ Error: no se puede usar antes
console.log(multiplicar(7, 7));  // Error

const multiplicar = function (a, b) {
  return a * b;
};

// ✅ Funciona después de la declaración
console.log(multiplicar(7, 7));  // 49
```

**Ejemplos del código modelo**:
```javascript
// 2. Funcion de expresion (Funcion asignada)
// No posee hoisting - No se puede ejecutar antes de su declaracion

const multiplicar = function (a, b) {
    return a * b
}

console.log(multiplicar(7,7))
```

#### 3. Función Flecha (Arrow Function)

**Características**:
- ❌ No tiene hoisting
- ✅ Sintaxis más concisa
- ❌ No tiene su propio `this` (importante en objetos)
- ✅ Muy usada en JavaScript moderno

**Sintaxis con llaves** (necesita `return`):
```javascript
const saludarUsuario = (nombre) => {
  return `Hola ${nombre}`;
};
```

**Sintaxis concisa** (retorno implícito):
```javascript
const restar = (a, b) => a - b;

// Un solo parámetro: no necesita paréntesis
const incrementar = a => a + 1;
```

**Ejemplos del código modelo**:
```javascript
// 3. Funcion flecha - Arrow Function
// Version Standard
// No usa la palabra reservada function
// No posee hoisting
// SI USAS LLAVES USAS RETURN
const saludarUsuario = (nombre) => {
    return `Hola ${nombre}`
}

// Funcion flecha sintaxis concisa // Retorno implicito
// No usa function, no usa llaves, no usa retorno
const restar = (a, b) => a - b

// Si tenes un solo parametro no es obligatorio utilizar parentesis
const incrementar = a => a + 1
```

### Parámetros y Argumentos

- **Parámetro**: Variable en la definición de la función
- **Argumento**: Valor pasado al invocar la función

```javascript
// Parámetros: 'nombre' y 'edad'
function presentar(nombre, edad) {
  return `Soy ${nombre} y tengo ${edad} años`;
}

// Argumentos: "Javier" y 33
presentar("Javier", 33);
```

### Return

**¿Qué hace?**:
- Retorna un valor de la función
- **Termina la ejecución** de la función (código después de `return` no se ejecuta)

```javascript
function calcularArea(largo, ancho) {
  return largo * ancho;  // Retorna el resultado
  console.log("Esto nunca se ejecuta");  // ❌ No se ejecuta
}
```

### Métodos en Objetos

Las funciones dentro de objetos se llaman **métodos**.

**Sintaxis tradicional**:
```javascript
const usuario = {
  nombre: "javier",
  saludar: function(mensaje) {
    console.log(`${mensaje}, mi nombre es ${this.nombre}`)
  }
};
```

**Sintaxis abreviada** (recomendada):
```javascript
const auto = {
  color: "gris",
  // Función normal, sin clave
  // Muy usado en programación orientada a objetos
  arrancar() { 
    return "Se encendio el auto" 
  },
  marca: "volkswagen"
};
```

**Ejemplos del código modelo**:
```javascript
// Funciones dentro de objetos - Metodo
const usuario = {
    nombre: "javier",
    saludar: function(mensaje) {
        console.log(`${mensaje}, mi nombre es ${this.nombre}`)
    }
}

// Sintaxis abreviada
const auto = {
    color: "gris",
    // Funcion normal, sin clave
    // Muy usado en programacion orientada a objetos
    arrancar() { 
        return "Se encendio el auto" 
    }
}
```

---

## 7. Bucles (Loops) 🔄

### ¿Qué es un Bucle? (Analogía del Mundo Real)

### 🔄 Analogía: La Rutina Diaria

Imagina tu rutina de la mañana:
1. Levantarte
2. Ducharte
3. Desayunar
4. Ir al trabajo

**Repites estos pasos cada día** mientras sea día laboral. Eso es un bucle: repetir acciones mientras se cumple una condición.

### 🏃 Analogía: Correr Vueltas

Piensa en correr vueltas en una pista:
- **Condición**: Mientras no hayas completado 5 vueltas
- **Acción**: Correr una vuelta
- **Actualización**: Contar la vuelta completada

**Repites la acción** (correr) hasta cumplir la condición (5 vueltas).

### 📚 Analogía: Leer un Libro

Cuando lees un libro:
- **Condición**: Mientras haya páginas por leer
- **Acción**: Leer una página
- **Actualización**: Pasar a la siguiente página

**Repites la acción** (leer) hasta cumplir la condición (terminar el libro).

### ¿Qué es un Bucle en Programación?

Un **bucle** (loop) permite ejecutar un bloque de código repetidamente mientras se cumple una condición.

**En términos simples**: Es como decirle a la computadora "haz esto una y otra vez hasta que algo cambie".

### for

**¿Cuándo usar?**: Cuando conoces el número de iteraciones o quieres control completo.

**Sintaxis**:
```javascript
for (inicialización; condición; actualización) {
  // Bloque de código
}
```

**Ejemplo básico**:
```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);  // 0, 1, 2, 3, 4
}
```

**Iterar arrays**:
```javascript
let frutas = ["uva", "manzana", "pera", "mandarina", "naranja"];

for (let i = 0; i < frutas.length; i++) {
  console.log(frutas[i]);
}
```

**Decremento**:
```javascript
// De mayor a menor
for (let i = frutas.length - 1; i >= 0; i--) {
  console.log(frutas[i], i);
}
```

**Ejemplos del código modelo**:
```javascript
// for (inicialización; condición; incremento/decremento) {
//   // Bloque de código a ejecutar
// }

let array = ["uva", "manzana", "pera", "mandarina", "naranja"]
let arraySinM = [];

for (let index = 0; index < array.length; index++) {
    const element = array[index][0];
    if(element !== "m"){
        arraySinM.push(array[index])
    }
}

// Decreciente
for (let index = array.length - 1; index >= 0; index--) {
    const element = array[index];
    console.log(element, index)
}
```

### while

**¿Cuándo usar?**: Cuando no conoces el número exacto de iteraciones, pero sabes la condición.

**Sintaxis**:
```javascript
while (condición) {
  // Bloque de código
  // ⚠️ IMPORTANTE: Actualizar la condición para evitar loops infinitos
}
```

**Ejemplo**:
```javascript
let contador = 0;
let frutas = ["uva", "manzana", "pera", "mandarina", "naranja"];

while (contador < frutas.length) {
  console.log(`Fruta ${contador + 1}: ${frutas[contador]}`);
  contador++;  // ⚠️ Actualizar para evitar loop infinito
}
```

**⚠️ Cuidado con loops infinitos**: Siempre actualiza la condición dentro del bucle.

**Ejemplos del código modelo**:
```javascript
// While
let contador = 0;
let frutas = ["uva", "manzana", "pera", "mandarina", "naranja"]
while (contador < frutas.length) {
  console.log(`Fruta ${contador + 1}: ${frutas[contador]}`);
  contador++;
}
```

### do...while

**¿Cuándo usar?**: Cuando quieres que el código se ejecute **al menos una vez** antes de comprobar la condición.

**Sintaxis**:
```javascript
do {
  // Bloque de código (se ejecuta al menos una vez)
} while (condición);
```

**Ejemplo**:
```javascript
let numero;
do {
  numero = prompt("Ingresa un número mayor a 10:");
} while (numero <= 10);
```

**Diferencia con `while`**:
- `while`: Comprueba condición primero
- `do...while`: Ejecuta código primero, luego comprueba

---

## 8. Destructuring (Desestructuración)

### ¿Qué es Destructuring? (Analogía del Mundo Real)

### 📦 Analogía: Desempacar una Caja

Imagina que recibes una caja con varios objetos:
- **Caja** (objeto/array): `{ nombre: "Juan", edad: 25, ciudad: "BA" }`
- **Desempacar** (destructuring): Sacas cada objeto y lo pones en su lugar

**Sin destructuring** (manual):
```javascript
const persona = { nombre: "Juan", edad: 25, ciudad: "BA" }
const nombre = persona.nombre    // Sacas nombre
const edad = persona.edad        // Sacas edad
const ciudad = persona.ciudad    // Sacas ciudad
```

**Con destructuring** (automático):
```javascript
const persona = { nombre: "Juan", edad: 25, ciudad: "BA" }
const { nombre, edad, ciudad } = persona  // Desempacas todo de una vez
```

### 🎁 Analogía: Abrir un Regalo

Cuando abres un regalo envuelto:
- **Regalo** (objeto): Tiene varios elementos dentro
- **Desenvolver** (destructuring): Sacas cada elemento y lo pones en su lugar

**Es más rápido** desempacar todo de una vez que ir sacando elemento por elemento.

### ¿Qué es Destructuring en Programación?

**Destructuring** permite extraer valores de objetos o arrays y asignarlos a variables de forma más concisa.

**En términos simples**: Es como "desempacar" un objeto o array y sacar sus elementos directamente a variables individuales.

### Destructuring de Objetos

**Sintaxis básica**:
```javascript
const persona = {
  nombre: "Javier",
  apellido: "Lopez",
  edad: 33
};

// Extraer propiedades
const { nombre, apellido } = persona;
console.log(nombre);   // "Javier"
console.log(apellido);  // "Lopez"
```

**Valores por defecto**:
```javascript
const { ciudad = "Buenos Aires" } = persona;
console.log(ciudad);  // "Buenos Aires" (si no existe en persona)
```

**Renombrar variables**:
```javascript
const { nombre: nombreCompleto } = persona;
console.log(nombreCompleto);  // "Javier"
```

### Destructuring de Arrays

**Sintaxis básica**:
```javascript
const frutas = ["manzana", "banana", "naranja"];

// Extraer elementos por posición
const [primera, segunda] = frutas;
console.log(primera);  // "manzana"
console.log(segunda);  // "banana"
```

**Omitir elementos**:
```javascript
const [, , tercera] = frutas;  // Omitir primero y segundo
console.log(tercera);  // "naranja"
```

**Rest operator**:
```javascript
const [primera, ...resto] = frutas;
console.log(primera);  // "manzana"
console.log(resto);    // ["banana", "naranja"]
```

### Spread Operator (`...`)

**¿Qué es?**: Esparce (spread) los elementos de un array u objeto.

**Con arrays**:
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combinado = [...arr1, ...arr2];  // [1, 2, 3, 4, 5, 6]
```

**Con objetos**:
```javascript
const persona = { nombre: "Javier", edad: 33 };
const actualizado = { ...persona, ciudad: "BA" };
// { nombre: "Javier", edad: 33, ciudad: "BA" }
```

---

## 9. Trabajar con Objetos

### ¿Qué es un Objeto? (Analogía)

### 🏠 Analogía: La Ficha de una Casa

Imagina la ficha de una casa en una inmobiliaria:
- **Nombre de la propiedad** (clave): "Dirección", "Habitaciones", "Precio"
- **Valor de la propiedad**: "Av. Corrientes 123", 3, "$150,000"

**Un objeto es como esa ficha**: Tiene propiedades (características) con valores.

### 📋 Analogía: La Tarjeta de Identificación

Piensa en tu tarjeta de identificación:
- **Nombre**: "Juan Pérez"
- **Edad**: 25
- **ID**: "12345678"

**Cada característica tiene un nombre y un valor**. Eso es un objeto: pares de nombre-valor.

### Acceso a Propiedades

```javascript
const persona = {
  nombre: "javier",
  apellido: "lopez",
  hobbies: ["lectura", "musica", "cocina"],
  profesion: {
    nombre: "Desarrollo web",
    desde: 2020
  }
};

// Acceso con punto
console.log(persona.nombre);  // "javier"

// Acceso con corchetes
console.log(persona["apellido"]);  // "lopez"

// Acceso a propiedades anidadas
console.log(persona.profesion.nombre);  // "Desarrollo web"

// Acceso a arrays en objetos
console.log(persona.hobbies[0]);  // "lectura"
```

### Modificar Propiedades

```javascript
// Modificar valor existente
persona.nombre = "Fernando";

// Agregar nueva propiedad
persona.ciudad = "Buenos Aires";

// Eliminar propiedad
delete persona.apellido;
```

### Métodos en Objetos

```javascript
const persona = {
  nombre: "javier",
  saludar: () => {
    console.log("Hola ", persona.nombre);
  }
};

persona.saludar();  // "Hola javier"
```

**Ejemplos del código modelo**:
```javascript
// Los objetos pueden representar cualquier cosa de la vida real en terminos de datos
// Sirven para moldear entidades

let persona = {
    nombre: "javier",
    apellido: "lopez",
    hobbies: ["lectura", "musica", "cocina"],
    profesion: {
        nombre: "Desarrollo web",
        desde: 2020,
        seniority: "ssr"
    },
    edad: 33,
    saludar: () => {
        console.log("Hola ", persona.nombre)
    }
}

console.log(persona.nombre)
persona.nombre = "Fernando"
console.log(persona.hobbies[2])
console.log(persona.profesion.nombre)
```

---

## 10. Buenas Prácticas y Recomendaciones ✅

### Nomenclatura

- **Variables y funciones**: `camelCase` (`nombreCompleto`, `calcularArea`)
- **Constantes**: `UPPER_SNAKE_CASE` (`PI`, `MAX_USUARIOS`)
- **Nombres descriptivos**: `edad` mejor que `e`, `calcularTotal` mejor que `calc`

### Uso de Variables

- ✅ Usar `const` por defecto
- ✅ Usar `let` solo cuando necesites reasignar
- ❌ Evitar `var` en código moderno
- ✅ Declarar variables al inicio del scope

### Comparaciones

- ✅ Siempre usar `===` y `!==` (comparación estricta)
- ❌ Evitar `==` y `!=` (pueden causar errores de tipo)

### Funciones

- ✅ Funciones pequeñas y con una sola responsabilidad
- ✅ Nombres descriptivos que indiquen qué hace
- ✅ Usar arrow functions cuando sea apropiado
- ✅ Documentar funciones complejas con comentarios

### Bucles

- ✅ Usar `for` cuando conozcas el número de iteraciones
- ✅ Usar `while` cuando la condición sea dinámica
- ⚠️ Siempre actualizar la condición en `while` para evitar loops infinitos

### Depuración

- ✅ Usar `console.log()` para ver valores
- ✅ Usar la consola del navegador (F12)
- ✅ Revisar errores en la consola
- ✅ Probar código paso a paso

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [JavaScript: Condicionales](./11-JS-Condicionales.md) - Usa variables en condicionales
- 📚 [JavaScript: Arrays](./12-JS-Arrays.md) - Arrays como tipo de dato
- 📚 [JavaScript: Funciones](./13-JS-Funciones.md) - Funciones como tipo de dato
- 📚 [JavaScript: DOM y Eventos](./14-JS-DOM-Eventos.md) - Usa variables para manipular DOM

### Código Relacionado

- 💻 [Tema 10: Variables y Operadores](../../CODIGO/frontend/tema-10-javascript-variables-operadores/)

---

## 🎯 Puntos Clave para Recordar

1. **Variable = Caja etiquetada**: Guarda valores con un nombre
2. **`const` por defecto**: Usa `const` a menos que necesites cambiar el valor
3. **`let` para valores que cambian**: Usa `let` cuando necesites reasignar
4. **Scope = Habitaciones**: Variables locales solo se ven dentro de su bloque
5. **Operadores = Herramientas**: Usa operadores para trabajar con datos
6. **Funciones = Recetas**: Código reutilizable que hace una tarea específica
7. **Bucles = Repetición**: Ejecuta código múltiples veces

---

## 💡 Ejercicio Mental

Piensa en objetos de la vida real como variables:
- **Tu nombre**: `const miNombre = "Juan"` (no cambia)
- **Tu edad**: `let miEdad = 25` (cambia cada año)
- **Tu mochila**: `const miMochila = ["libro", "lápiz"]` (la mochila no cambia, pero puedes agregar cosas)

¡Practica identificando qué debería ser `const` y qué `let`!

---

## 10. Buenas Prácticas y Recomendaciones ✅

### Nomenclatura

- **Variables y funciones**: `camelCase` (`nombreCompleto`, `calcularArea`)
- **Constantes**: `UPPER_SNAKE_CASE` (`PI`, `MAX_USUARIOS`)
- **Nombres descriptivos**: `edad` mejor que `e`, `calcularTotal` mejor que `calc`

### Uso de Variables

- ✅ Usar `const` por defecto
- ✅ Usar `let` solo cuando necesites reasignar
- ❌ Evitar `var` en código moderno
- ✅ Declarar variables al inicio del scope

### Comparaciones

- ✅ Siempre usar `===` y `!==` (comparación estricta)
- ❌ Evitar `==` y `!=` (pueden causar errores de tipo)

### Funciones

- ✅ Funciones pequeñas y con una sola responsabilidad
- ✅ Nombres descriptivos que indiquen qué hace
- ✅ Usar arrow functions cuando sea apropiado
- ✅ Documentar funciones complejas con comentarios

### Bucles

- ✅ Usar `for` cuando conozcas el número de iteraciones
- ✅ Usar `while` cuando la condición sea dinámica
- ⚠️ Siempre actualizar la condición en `while` para evitar loops infinitos

### Depuración

- ✅ Usar `console.log()` para ver valores
- ✅ Usar la consola del navegador (F12)
- ✅ Revisar errores en la consola
- ✅ Probar código paso a paso

---

## 11. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: Variables y Scope (Tema 10)

```javascript
// Variable global
let color_calzado = "negro"

console.log("antes de la funcion calzado ", color_calzado)

function calzado() {
    // Variable local - en bloque
    let color_calzado = "Blanco"  // No afecta la global
    console.log("Dentro de la funcion calzado ", color_calzado)
}

function calzado2() {
    console.log(color_calzado)  // Accede a la global
}

calzado()
console.log("Despues de la funcion calzado ", color_calzado)
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-10-javascript-variables-operadores/variables/variables.js`

### Ejemplo 2: Condicionales y Truthy/Falsy (Tema 11)

```javascript
// True - Truthy
let truthy = [ "hola", " ", true, 1, -20, {}, [], function(){}, Symbol('id') ]

// False - Falsy
let falsy = [ false, 0, -0, "", null, undefined, NaN ]

let condicion = false

if (condicion) {
    console.log("Es truthy")
} else {
    console.log("Es falsy")
}

// Operador ternario
let frio = true
frio ? console.log("Me abrigo") : console.log("No me abrigo")
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-11-javascript-condicionales-operadores-arreglos/if.js`

### Ejemplo 3: Funciones y DRY (Tema 13)

```javascript
/* Funciones
Codigo reutilizable para tareas especificas
Dont repeat yourself - DRY

Porque es malo repetir codigo?
 - Si la funcion cambia, deberias cambiarla en todas partes (Dificulta mantenimiento)
 - Corres riesgo de errores (Aumentamos la cantidad de bugs)

Porque es bueno reutilizar codigo?
- Mantenible - Hay un solo lugar donde deberias hacer tus cambios
- Unica fuente de la verdad
- Separacion de responsabilidades (Solid -> S)
*/

// Función declarada
function calcularArea(largo, ancho){
    return largo * ancho
}

// Función de expresión
const multiplicar = function (a, b) {
    return a * b
}

// Arrow function
const restar = (a, b) => a - b
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-13-javascript-funciones-ciclos/funciones.js`

### Ejemplo 4: Bucles con Arrays (Tema 13)

```javascript
let array = ["uva", "manzana", "pera", "mandarina", "naranja"]
let arraySinM = [];

// Bucle for - Incremento
for (let index = 0; index < array.length; index++) {
    const element = array[index][0];
    if(element !== "m"){
        arraySinM.push(array[index])
    }
}

// Bucle for - Decremento
for (let index = array.length - 1; index >= 0; index--) {
    const element = array[index];
    console.log(element, index)
}

// Bucle while
let contador = 0;
while (contador < array.length) {
  console.log(`Fruta ${contador + 1}: ${array[contador]}`);
  contador++;
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-13-javascript-funciones-ciclos/iteracion.js`

---


### Tema 11: JavaScript - Condicionales, Operadores y Arreglos
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-11-javascript-condicionales-operadores-arreglos/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 5: Estructuras de Control
- ✅ Sección 9: Trabajar con Objetos
- ✅ Ejemplo 2: Condicionales y Truthy/Falsy

**Conceptos cubiertos en el código modelo**:
- Condicionales: `if`, `else`, `else if`
- Truthy y Falsy: Valores que se evalúan como true/false
- Operadores lógicos: `&&` (AND), `||` (OR), `!` (NOT)
- Operador ternario: `condicion ? valorSiTrue : valorSiFalse`
- Switch-case: Estructura para múltiples casos con `break` y `default`
- Objetos: Pares clave-valor, acceso con `.` o `[]`, objetos anidados, arrays en objetos
- Bucles básicos: `for` y `while` para iterar arrays

---

### Tema 13: JavaScript - Funciones y Ciclos
**Ubicación**: `cursadas/frontend/frontEnd_modelo/tema-13-javascript-funciones-ciclos/`

**Secciones de este resumen relacionadas**:
- ✅ Sección 6: Funciones
- ✅ Sección 7: Bucles (Loops)
- ✅ Sección 8: Destructuring
- ✅ Ejemplo 3: Funciones y DRY
- ✅ Ejemplo 4: Bucles con Arrays

**Conceptos cubiertos en el código modelo**:
- Funciones: ¿Por qué usarlas? (DRY, mantenibilidad, modularización)
- Tipos de funciones: Declaradas (con hoisting), de expresión, arrow functions
- Parámetros y argumentos: Diferencia entre parámetro y argumento
- Return: Retorna valor y termina ejecución
- Métodos en objetos: Funciones dentro de objetos, sintaxis abreviada
- Bucles: `for` (incremento y decremento), `while` (con actualización de condición)
- Destructuring: De objetos y arrays, valores por defecto, rest operator
- Spread operator: `...` para esparcir elementos

---

