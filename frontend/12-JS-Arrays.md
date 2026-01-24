# Master Guide: Arrays y Métodos de Iteración 🚀

## 1. Introducción a los Arrays

### ¿Qué es un Array?

Un **Array** es una colección ordenada de datos accesibles mediante un índice (empezando en 0). Es una estructura de datos fundamental en JavaScript que permite almacenar múltiples valores en una sola variable.

**Características principales**:
- ✅ Índices empiezan en 0 (no en 1)
- ✅ Último índice: `length - 1`
- ✅ Puede contener cualquier tipo de dato (números, strings, objetos, otros arrays)
- ✅ Tamaño dinámico (puede crecer o reducirse)

**Ejemplo básico**:
```javascript
let nums = [4, 5, 6, 7, 8, 9, 10, [3.14, 41], { nombre: "aristobulo", apellido: "del valle"} ]

// Acceso por índice
console.log(nums[0]);        // 4
console.log(nums[7][0]);      // 3.14 (acceso a array anidado)
console.log(nums[8].apellido); // "del valle" (acceso a objeto)
```

**Ejemplo del código modelo**:
```javascript
let nums = [4, 5, 6, 7, 8, 9, 10, [3.14, 41], { nombre: "aristobulo", apellido: "del valle"} ]

// Podemos reasignar el valor de una variable x indice
nums[0] = 15

// Podemos acceder al indice de un indice
console.log(nums[7][0])
console.log(nums[8].apellido);
```

---

## 2. Operaciones Básicas de Arrays

### Propiedades

#### `.length`

**¿Qué es?**: Propiedad que devuelve la cantidad de elementos en el array.

**Parámetros**: Ninguno (es una propiedad, no un método)

**Utilidad**: 
- Conocer el tamaño del array
- Iterar hasta el final del array
- Validar si el array está vacío

**Ejemplo**:
```javascript
let frutas = ["manzana", "banana", "naranja"];
console.log(frutas.length);  // 3
```

---

### Métodos de Modificación (Mutación)

Estos métodos **modifican el array original**.

#### `.push()`

**¿Qué es?**: Añade uno o más elementos al **final** del array.

**Parámetros**:
- `elemento1, elemento2, ...`: Uno o más elementos a añadir al final

**Retorna**: Nueva longitud del array

**Utilidad**:
- Agregar elementos al final de una lista
- Construir arrays dinámicamente
- Añadir múltiples elementos a la vez

**Ejemplo**:
```javascript
let listaDeCompras = ["Leche", "Pan", "Queso"];
listaDeCompras.push("Manzanas", "Huevos");
console.log(listaDeCompras);  // ["Leche", "Pan", "Queso", "Manzanas", "Huevos"]
```

**Ejemplo del código modelo**:
```javascript
const nombres = ["brandon", "miguel", "jose"]
console.log(nombres)

// El metodo push guarda un valor al final del array
nombres.push("javier")
console.log(nombres)
```

#### `.pop()`

**¿Qué es?**: Elimina el **último** elemento del array y lo devuelve.

**Parámetros**: Ninguno

**Retorna**: El elemento eliminado (o `undefined` si el array está vacío)

**Utilidad**:
- Eliminar el último elemento
- Implementar estructuras tipo pila (stack)
- Obtener y remover el último elemento

**Ejemplo**:
```javascript
let listaDeCompras = ["Leche", "Pan", "Queso", "Manzanas", "Huevos"];
let articuloRemovido = listaDeCompras.pop();
console.log(articuloRemovido);  // "Huevos"
console.log(listaDeCompras);    // ["Leche", "Pan", "Queso", "Manzanas"]
```

#### `.unshift()`

**¿Qué es?**: Añade uno o más elementos al **inicio** del array.

**Parámetros**:
- `elemento1, elemento2, ...`: Uno o más elementos a añadir al inicio

**Retorna**: Nueva longitud del array

**Utilidad**:
- Agregar elementos al principio de una lista
- Priorizar elementos (como tareas urgentes)
- Añadir múltiples elementos al inicio

**Ejemplo**:
```javascript
let tareas = ["Estudiar", "Comer"];
tareas.unshift("Urgente: Pagar factura");
console.log(tareas);  // ["Urgente: Pagar factura", "Estudiar", "Comer"]
```

#### `.shift()`

**¿Qué es?**: Elimina el **primer** elemento del array y lo devuelve.

**Parámetros**: Ninguno

**Retorna**: El elemento eliminado (o `undefined` si el array está vacío)

**Utilidad**:
- Eliminar el primer elemento
- Implementar estructuras tipo cola (queue)
- Procesar elementos en orden FIFO (First In, First Out)

**Ejemplo**:
```javascript
let tareas = ["Urgente: Pagar factura", "Estudiar", "Comer"];
let tareaCompletada = tareas.shift();
console.log(tareaCompletada);  // "Urgente: Pagar factura"
console.log(tareas);           // ["Estudiar", "Comer"]
```

#### `.splice()`

**¿Qué es?**: El método más versátil. Puede **eliminar**, **reemplazar** o **insertar** elementos en cualquier posición.

**Parámetros**:
- `inicio` (número): Índice donde comenzar (requerido)
- `cantidad_a_eliminar` (número): Cuántos elementos eliminar (opcional, default: 0)
- `...elementos_a_añadir` (cualquier tipo): Elementos a añadir (opcional)

**Retorna**: Array con los elementos eliminados

**Utilidad**:
- Reemplazar elementos en posición específica
- Insertar elementos sin eliminar
- Eliminar elementos en cualquier posición
- Modificar arrays de forma precisa

**Ejemplos**:
```javascript
let notas = [9, 6, 7, 5, 4, 8, 1, 2];

// Reemplazar: índice 1, eliminar 1, añadir 10
notas.splice(1, 1, 10);
// [9, 10, 7, 5, 4, 8, 1, 2]

// Insertar: índice 3, eliminar 0, añadir 8
notas.splice(3, 0, 8);
// [9, 10, 7, 8, 5, 4, 8, 1, 2]

// Eliminar: índice 6, eliminar 2
notas.splice(6, 2);
// [9, 10, 7, 8, 5, 4]
```

**Sintaxis completa**:
```javascript
array.splice(inicio, cantidad_a_eliminar, elemento1, elemento2, ...)
```

#### `.sort()`

**¿Qué es?**: Ordena los elementos del array **modificando el array original**.

**Parámetros**:
- `funcionComparadora` (función, opcional): Función que define el orden
  - Si no se proporciona: ordena alfabéticamente (problema con números)
  - `(a, b) => a - b`: Orden ascendente para números
  - `(a, b) => b - a`: Orden descendente para números

**Retorna**: El mismo array ordenado (modificado)

**Utilidad**:
- Ordenar arrays alfabéticamente
- Ordenar números de forma ascendente/descendente
- Ordenar objetos por una propiedad específica

**Ejemplos**:
```javascript
// Orden alfabético (strings)
let frutas = ["mandarina", "sandia", "uva", "banana"];
frutas.sort();
console.log(frutas);  // ["banana", "mandarina", "sandia", "uva"]

// Orden numérico ascendente
let nums6 = [0.5, -10, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
nums6.sort(function(a, b) {
    return a - b;  // Ascendente
});
console.log(nums6);  // [-10, 0, 0.5, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// Orden numérico descendente
nums6.sort(function(a, b) {
    return b - a;  // Descendente
});
console.log(nums6);  // [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0.5, 0, -10]
```

**Ejemplo del código modelo**:
```javascript
// Sort -> Ordenar alfabeticamente, y modifica el array original
let frutas = ["mandarina", "sandia", "uva", "banana"]
frutas.sort()
console.log(frutas)

// Si ordenas numeros hace falta aclarar la direccion
let nums6 = [0.5, -10, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
// Ascendente
nums6.sort(function(a, b) {
    return a - b
})
console.log(nums6)

// Descendente
nums6.sort(function(a, b) {
    return b - a
})
console.log(nums6)
```

**⚠️ Importante**: Sin función comparadora, `sort()` ordena alfabéticamente, lo que causa problemas con números:
```javascript
[10, 2, 5].sort();  // ❌ ["10", "2", "5"] (incorrecto)
[10, 2, 5].sort((a, b) => a - b);  // ✅ [2, 5, 10] (correcto)
```

---

### Métodos de Copia (No Mutación)

Estos métodos **no modifican el array original**, crean uno nuevo.

#### `.slice()`

**¿Qué es?**: Crea una **copia** de una porción del array (no modifica el original).

**Parámetros**:
- `inicio` (número): Índice donde comenzar (incluido)
- `fin` (número, opcional): Índice donde terminar (no incluido). Si se omite, copia hasta el final

**Retorna**: Nuevo array con los elementos copiados

**Utilidad**:
- Copiar una porción del array
- Crear copias sin modificar el original
- Extraer subarrays

**Ejemplos**:
```javascript
let numeros = [1, 2, 3, 4, 5];

// Copiar desde índice 1 hasta 3 (no incluye 3)
let copia = numeros.slice(1, 3);
console.log(copia);    // [2, 3]
console.log(numeros);  // [1, 2, 3, 4, 5] (sin cambios)

// Copiar desde índice 2 hasta el final
let resto = numeros.slice(2);
console.log(resto);    // [3, 4, 5]

// Copiar todo el array
let copiaCompleta = numeros.slice();
console.log(copiaCompleta);  // [1, 2, 3, 4, 5]
```

---

### Métodos de Conversión String ↔ Array

#### `.split()` (Método de String)

**¿Qué es?**: Convierte un string en un array separándolo por un carácter.

**Parámetros**:
- `separador` (string): Carácter o string por el cual separar
- `limite` (número, opcional): Número máximo de elementos en el array resultante

**Retorna**: Nuevo array con los elementos separados

**Utilidad**:
- Convertir strings a arrays
- Separar texto por comas, espacios, guiones, etc.
- Procesar datos de entrada del usuario

**Ejemplos**:
```javascript
// Separar por coma
let nums2 = "4, 5, 6, 7, 8, 9, 10";
console.log(nums2.split(","));  // ["4", " 5", " 6", " 7", " 8", " 9", " 10"]

// Separar por guión
let nums3 = "4 - 5 - 6 - 7 - 8 - 9 - 10";
console.log(nums3.split("-"));  // ["4 ", " 5 ", " 6 ", " 7 ", " 8 ", " 9 ", " 10"]
```

**Ejemplo del código modelo**:
```javascript
// Split - Se usa en strings - convirtiendo un string en un array separado por un caracter
let nums2 = "4, 5, 6, 7, 8, 9, 10"
console.log(nums2.split(","))

let nums3 = "4 - 5 - 6 - 7 - 8 - 9 - 10"
console.log(nums3.split("-"))
```

#### `.join()`

**¿Qué es?**: Une elementos del array en un string con el separador indicado.

**Parámetros**:
- `separador` (string, opcional): Carácter o string para unir elementos. Si se omite, usa coma (`,`)

**Retorna**: String con los elementos unidos

**Utilidad**:
- Convertir arrays a strings
- Crear strings formateados
- Generar URLs, rutas, etc.

**Ejemplos**:
```javascript
let names = ["pedro", "martina", "lucia"];
let namesJoined = names.join("-");
console.log(namesJoined);  // "pedro-martina-lucia"

let nombresComas = names.join(", ");
console.log(nombresComas);  // "pedro, martina, lucia"
```

**Ejemplo del código modelo**:
```javascript
// Join -> convertir un array a string y los separa con el caracter que le digamos
let names = ["pedro", "martina", "lucia"]
let namesJoined = names.join("-")
console.log(namesJoined)
```

---

## 3. Búsqueda y Orden

### Métodos de Búsqueda

#### `.indexOf()`

**¿Qué es?**: Devuelve el índice de la **primera coincidencia** de un valor.

**Parámetros**:
- `valor` (cualquier tipo): Valor a buscar
- `desdeIndice` (número, opcional): Índice desde donde comenzar la búsqueda

**Retorna**: Índice del elemento encontrado, o `-1` si no se encuentra

**Utilidad**:
- Encontrar la posición de un valor específico
- Verificar si un elemento existe en el array
- Buscar desde una posición específica

**Ejemplo**:
```javascript
let frutas = ["manzana", "banana", "naranja", "banana"];
console.log(frutas.indexOf("banana"));     // 1 (primera coincidencia)
console.log(frutas.indexOf("banana", 2));  // 3 (busca desde índice 2)
console.log(frutas.indexOf("uva"));        // -1 (no encontrado)
```

#### `.lastIndexOf()`

**¿Qué es?**: Devuelve el índice de la **última coincidencia** de un valor.

**Parámetros**:
- `valor` (cualquier tipo): Valor a buscar
- `desdeIndice` (número, opcional): Índice desde donde comenzar la búsqueda (hacia atrás)

**Retorna**: Índice del elemento encontrado, o `-1` si no se encuentra

**Utilidad**:
- Encontrar la última ocurrencia de un valor
- Buscar desde el final del array

**Ejemplo**:
```javascript
let frutas = ["manzana", "banana", "naranja", "banana"];
console.log(frutas.lastIndexOf("banana"));  // 3 (última coincidencia)
```

#### `.includes()`

**¿Qué es?**: Verifica si el array contiene un valor específico.

**Parámetros**:
- `valor` (cualquier tipo): Valor a buscar
- `desdeIndice` (número, opcional): Índice desde donde comenzar la búsqueda

**Retorna**: `true` si encuentra el valor, `false` si no

**Utilidad**:
- Verificar existencia de un valor
- Validar si un elemento está en el array
- Más legible que `indexOf() !== -1`

**Ejemplo**:
```javascript
let frutas = ["manzana", "banana", "naranja"];
console.log(frutas.includes("banana"));  // true
console.log(frutas.includes("uva"));    // false
```

#### `.reverse()`

**¿Qué es?**: Invierte el orden de los elementos del array **modificando el original**.

**Parámetros**: Ninguno

**Retorna**: El mismo array invertido (modificado)

**Utilidad**:
- Invertir el orden de elementos
- Mostrar listas en orden inverso
- Procesar datos de atrás hacia adelante

**Ejemplo**:
```javascript
let numeros = [1, 2, 3, 4, 5];
numeros.reverse();
console.log(numeros);  // [5, 4, 3, 2, 1]
```

---

## 4. Métodos Modernos de Iteración 🔄

Estos métodos son más legibles y fomentan la programación funcional. **No modifican el array original** (excepto cuando se indica).

### `.forEach()`

**¿Qué es?**: Ejecuta una función **por cada elemento** del array. No retorna nada.

**Parámetros**:
- `callback` (función): Función a ejecutar para cada elemento
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: `undefined` (no retorna nada)

**Utilidad**:
- Iterar sobre elementos para efectos secundarios (imprimir, modificar DOM, etc.)
- Ejecutar acciones para cada elemento
- No crear nuevos arrays, solo ejecutar código

**Ejemplo**:
```javascript
let frutas = ["manzana", "banana", "naranja"];

frutas.forEach(fruta => {
    console.log(fruta);
});
// "manzana"
// "banana"
// "naranja"

// Con índice
frutas.forEach((fruta, indice) => {
    console.log(`${indice}: ${fruta}`);
});
// "0: manzana"
// "1: banana"
// "2: naranja"
```

**Ejemplo del material del docente**:
```javascript
let invitados = ["Ana", "Luis", "Carlos"];
invitados.forEach((nombre) => {
    console.log(`¡Hola, ${nombre}, estás invitado a la fiesta!`);
});
```

**Cuándo usar**:
- ✅ Para efectos secundarios (imprimir, modificar DOM)
- ✅ Cuando no necesitas crear un nuevo array
- ❌ No usar si necesitas retornar un valor o crear un nuevo array

---

### `.map()` (Transformación) 🆕

**¿Qué es?**: Crea un **nuevo array** con los resultados de aplicar una función a cada elemento. Su tamaño siempre es igual al original.

**Parámetros**:
- `callback` (función): Función de transformación
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: Nuevo array con los elementos transformados

**Utilidad**:
- Transformar cada elemento del array
- Convertir datos de un formato a otro
- Aplicar operaciones a todos los elementos
- Extraer propiedades de objetos

**Ejemplo**:
```javascript
// Transformar números
let numeros = [1, 2, 3, 4];
const dobles = numeros.map(n => n * 2);
console.log(dobles);  // [2, 4, 6, 8]

// Convertir a mayúsculas
let frutas = ["manzana", "banana", "uva"];
let frutasMayusculas = frutas.map(fruta => fruta.toUpperCase());
console.log(frutasMayusculas);  // ["MANZANA", "BANANA", "UVA"]

// Convertir temperaturas Celsius a Fahrenheit
let temperaturasCelsius = [0, 15, 25, 30];
let temperaturasFahrenheit = temperaturasCelsius.map(c => c * 1.8 + 32);
console.log(temperaturasFahrenheit);  // [32, 59, 77, 86]
```

**Ejemplo del material del docente**:
```javascript
// Usar `map` para Convertir un Array de Palabras a un Array de sus Longitudes
let palabras = ["manzana", "banana", "naranja"];
let longitudes = palabras.map(palabra => palabra.length);
console.log(longitudes);  // [7, 6, 7]
```

**Características**:
- ✅ Siempre retorna un array de la misma longitud
- ✅ No modifica el array original
- ✅ Cada elemento se transforma según la función

---

### `.filter()` (Selección) 🔍

**¿Qué es?**: Crea un **nuevo array** solo con los elementos que cumplen una condición.

**Parámetros**:
- `callback` (función): Función de filtrado (debe retornar `true` o `false`)
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: Nuevo array con los elementos que cumplen la condición

**Utilidad**:
- Filtrar elementos según criterios
- Seleccionar elementos que cumplen condiciones
- Eliminar elementos no deseados
- Crear subconjuntos de datos

**Ejemplo**:
```javascript
// Filtrar edades
let edades = [22, 18, 20, 45, 70, 17, 12];
const adultos = edades.filter(edad => edad >= 18);
console.log(adultos);  // [22, 18, 20, 45, 70]

// Filtrar menores de 18 pero mayores de 13
let numsFiltered = edades.filter(function(edad) {
   return !(edad >= 18)  // Menores de 18
})
let adolescentes = numsFiltered.filter(function(edad) {
    return edad >= 13  // Mayores o iguales a 13
})
console.log(adolescentes);  // [17]

// Filtrar productos por precio
const productos = [
    {nombre: "Camiseta", precio: 25},
    {nombre: "Pantalón", precio: 75},
    {nombre: "Calcetines", precio: 10},
    {nombre: "Chaqueta", precio: 150},
    {nombre: "Gorra", precio: 45}
];
let ofertas = productos.filter(producto => producto.precio <= 50);
console.log(ofertas);
// [{nombre: "Camiseta", precio: 25}, {nombre: "Calcetines", precio: 10}, {nombre: "Gorra", precio: 45}]
```

**Ejemplo del código modelo**:
```javascript
// Filter - retorna un array con los valores que cumplen la condicion
// No modifica el array original
let num5 = [22, 18, 20, 45, 70, 17, 12]
let numsFiltered = num5.filter(function(edad) {
   return !(edad >= 18)
})
console.log(numsFiltered)

let numsFiltered2 = numsFiltered.filter(function(edad) {
    return edad >= 13
})
console.log(numsFiltered2)
```

**Ejemplo del material del docente**:
```javascript
// Usar `filter` para Obtener Números Pares de un Array
let numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let pares = numeros.filter(num => num % 2 === 0);
console.log(pares);  // [2, 4, 6, 8, 10]
```

**Características**:
- ✅ Puede retornar un array de diferente longitud
- ✅ No modifica el array original
- ✅ Solo incluye elementos donde la función retorna `true`

---

### `.find()` vs `.findIndex()`

#### `.find()`

**¿Qué es?**: Devuelve el **primer elemento** que cumple la condición.

**Parámetros**:
- `callback` (función): Función de búsqueda (debe retornar `true` o `false`)
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: El primer elemento encontrado, o `undefined` si no encuentra ninguno

**Utilidad**:
- Encontrar un elemento específico
- Buscar objetos por propiedades
- Obtener el primer elemento que cumple una condición

**Ejemplo**:
```javascript
const productos = [
    {nombre: "parlante", precio: 120000},
    {nombre: "laptop", precio: 800000},
    {nombre: "mouse gamer", precio: 80000},
    {nombre: "teclado", precio: 50000}
];

// Encontrar producto barato
let productoBarato = productos.find(function(producto) {
    return producto.precio < 100000
});
console.log(productoBarato);  // {nombre: "mouse gamer", precio: 80000}

// Encontrar por nombre (no existe)
let teclado = productos.find(function(producto) {
    return producto.nombre === "teclad"  // Error tipográfico
});
console.log(teclado);  // undefined
```

**Ejemplo del código modelo**:
```javascript
// Find -> Encontrar el primer elemento coincidente, retorna el registro
const productos = [
    {nombre: "parlante", precio: 120000},
    {nombre: "laptop", precio: 800000},
    {nombre: "mouse gamer", precio: 80000},
    {nombre: "teclado", precio: 50000}
]

let productoBarato = productos.find(function(producto) {
    return producto.precio < 100000
})
console.log(productoBarato)
```

**Ejemplo del material del docente**:
```javascript
const usuarios = [
    { id: 101, nombre: "Carlos" },
    { id: 102, nombre: "Andrea" },
    { id: 103, nombre: "Javier" },
    { id: 104, nombre: "Andrea" }
];
let usuarioEncontrado = usuarios.find(usuario => usuario.nombre === "Andrea");
console.log(usuarioEncontrado);  // { id: 102, nombre: "Andrea" } (primer coincidencia)
```

#### `.findIndex()`

**¿Qué es?**: Devuelve el **índice** del primer elemento que cumple la condición.

**Parámetros**:
- `callback` (función): Función de búsqueda (debe retornar `true` o `false`)
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: Índice del primer elemento encontrado, o `-1` si no encuentra ninguno

**Utilidad**:
- Encontrar la posición de un elemento
- Obtener el índice para usar con otros métodos
- Validar existencia y posición

**Ejemplo**:
```javascript
let indiceEncontrado = productos.findIndex(function(producto) {
    return producto.nombre === "mouse gamer"
});
console.log(indiceEncontrado);  // 2
```

**Ejemplo del código modelo**:
```javascript
// Find index -> retornar el indice del elemento coincidente
let indiceEncontrado = productos.findIndex(function(producto) {
    return producto.nombre === "mouse gamer"
})
console.log(indiceEncontrado)
```

**Diferencia clave**:
- `.find()`: Retorna el **elemento**
- `.findIndex()`: Retorna el **índice**

---

### `.every()` vs `.some()`

#### `.every()`

**¿Qué es?**: Verifica si **todos** los elementos cumplen una condición.

**Parámetros**:
- `callback` (función): Función de validación (debe retornar `true` o `false`)
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: `true` si **todos** cumplen, `false` si al menos uno no cumple

**Utilidad**:
- Validar que todos los elementos cumplan una regla
- Verificar condiciones globales
- Validar datos antes de procesar

**Ejemplo**:
```javascript
let socios = [
    {nombre: "javier", activo: true},
    {nombre: "gabriel", activo: true},
    {nombre: "marina", activo: true},
    {nombre: "aristobulo", activo: true}
];

let noDeudores = socios.every(function(socio) {
    return socio.activo === true
});
console.log(noDeudores);  // true (todos están activos)

// Validar edades
let edades = [22, 18, 30, 25];
let todosMayores = edades.every(edad => edad >= 18);
console.log(todosMayores);  // true
```

**Ejemplo del código modelo**:
```javascript
// Every -> comprueba si todos los elementos cumplen una condicion
let socios = [
    {nombre: "javier", activo: true},
    {nombre: "gabriel", activo: true},
    {nombre: "marina", activo: true},
    {nombre: "aristobulo", activo: true}
]

let noDeudores = socios.every(function(socio) {
    return socio.activo === true
})
console.log(noDeudores)
```

#### `.some()`

**¿Qué es?**: Verifica si **al menos uno** de los elementos cumple una condición.

**Parámetros**:
- `callback` (función): Función de validación (debe retornar `true` o `false`)
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo

**Retorna**: `true` si **al menos uno** cumple, `false` si ninguno cumple

**Utilidad**:
- Verificar si existe al menos un elemento que cumple condición
- Validar presencia de valores específicos
- Comprobar si hay elementos problemáticos

**Ejemplo**:
```javascript
let edades = [15, 17, 20, 25];
let hayMenores = edades.some(edad => edad < 18);
console.log(hayMenores);  // true (hay menores de 18)

let todosMayores = edades.some(edad => edad >= 18);
console.log(todosMayores);  // true (al menos uno es mayor o igual a 18)
```

**Diferencia clave**:
- `.every()`: **Todos** deben cumplir → `true`
- `.some()`: **Al menos uno** debe cumplir → `true`

---

### `.reduce()` (Acumulación) 🧶

**¿Qué es?**: Reduce el array a un **único valor** aplicando una función acumuladora.

**Parámetros**:
- `callback` (función): Función acumuladora
  - `acumulador` (cualquier tipo): Valor acumulado
  - `elemento` (cualquier tipo): Elemento actual
  - `indice` (número, opcional): Índice del elemento actual
  - `array` (array, opcional): Array completo
- `valorInicial` (cualquier tipo, opcional): Valor inicial del acumulador

**Retorna**: El valor acumulado final

**Utilidad**:
- Sumar todos los valores
- Calcular promedios
- Contar elementos
- Agrupar datos
- Transformar array en objeto
- Encontrar máximo/mínimo

**Ejemplo básico**:
```javascript
// Sumar precios
let precios = [20000, 50000, 30000];
let total = precios.reduce(function(suma, precio) {
    return suma + precio;
}, 0);  // Valor inicial: 0
console.log(total);  // 100000
```

**Ejemplo del código modelo**:
```javascript
// Reduce -> reduce cada elemento del array a un unico resultado
let precios = [20000, 50000, 30000]
let total = precios.reduce(function(suma, precio) {
    return suma + precio
})
console.log(total)
```

**Ejemplos avanzados**:
```javascript
// Calcular total de factura
let precios = [25.50, 10.00, 5.25, 50.00];
let montoTotal = precios.reduce((total, precio) => total + precio, 0);
console.log(montoTotal);  // 90.75

// Encontrar el máximo
let numeros = [5, 2, 8, 1, 9];
let maximo = numeros.reduce((max, num) => num > max ? num : max);
console.log(maximo);  // 9

// Contar elementos
let palabras = ["manzana", "banana", "manzana", "uva"];
let conteo = palabras.reduce((acc, palabra) => {
    acc[palabra] = (acc[palabra] || 0) + 1;
    return acc;
}, {});
console.log(conteo);  // {manzana: 2, banana: 1, uva: 1}
```

**Ejemplo del material del docente**:
```javascript
// Calcular el total de precios
let precios = [25.50, 10.00, 5.25, 50.00];
let montoTotal = precios.reduce((total, precio) => total + precio, 0);
console.log(montoTotal);  // 90.75
```

**⚠️ Importante**: 
- Si no proporcionas `valorInicial`, el primer elemento se usa como acumulador inicial
- Siempre proporciona `valorInicial` para evitar errores con arrays vacíos

---

## 5. Tabla Comparativa Completa de Métodos

### Tabla de Métodos de Arrays

| Método | Modifica Original | Retorna (Éxito) | Retorna (Fallo/No Encontrado) | Utilidad Principal | Parámetros |
|:---|:---|:---|:---|:---|:---|
| **`.push()`** | ✅ Sí | Nueva longitud (número) | N/A | Añadir al final | `...elementos` |
| **`.pop()`** | ✅ Sí | Elemento eliminado | `undefined` (si array vacío) | Eliminar último | Ninguno |
| **`.unshift()`** | ✅ Sí | Nueva longitud (número) | N/A | Añadir al inicio | `...elementos` |
| **`.shift()`** | ✅ Sí | Elemento eliminado | `undefined` (si array vacío) | Eliminar primero | Ninguno |
| **`.splice()`** | ✅ Sí | Array con elementos eliminados | Array vacío `[]` (si no elimina nada) | Modificar en cualquier posición | `inicio, cantidad, ...elementos` |
| **`.sort()`** | ✅ Sí | Array ordenado (mismo array) | N/A | Ordenar elementos | `funcionComparadora` (opcional) |
| **`.reverse()`** | ✅ Sí | Array invertido (mismo array) | N/A | Invertir orden | Ninguno |
| **`.slice()`** | ❌ No | Nuevo array con elementos copiados | Array vacío `[]` (si índices inválidos) | Copiar porción | `inicio, fin` (opcional) |
| **`.forEach()`** | ❌ No* | `undefined` | `undefined` | Iterar para efectos secundarios | `callback(elemento, indice, array)` |
| **`.map()`** | ❌ No | Nuevo array transformado | Array vacío `[]` (si array original vacío) | Transformar elementos | `callback(elemento, indice, array)` |
| **`.filter()`** | ❌ No | Nuevo array con elementos filtrados | Array vacío `[]` (si ninguno cumple condición) | Filtrar elementos | `callback(elemento, indice, array)` |
| **`.find()`** | ❌ No | Primer elemento encontrado | `undefined` (si no encuentra) | Encontrar primer elemento | `callback(elemento, indice, array)` |
| **`.findIndex()`** | ❌ No | Índice (número ≥ 0) | `-1` (si no encuentra) | Encontrar índice | `callback(elemento, indice, array)` |
| **`.indexOf()`** | ❌ No | Índice (número ≥ 0) | `-1` (si no encuentra) | Buscar valor específico | `valor, desdeIndice` (opcional) |
| **`.lastIndexOf()`** | ❌ No | Índice (número ≥ 0) | `-1` (si no encuentra) | Buscar última ocurrencia | `valor, desdeIndice` (opcional) |
| **`.includes()`** | ❌ No | `true` | `false` | Verificar existencia | `valor, desdeIndice` (opcional) |
| **`.every()`** | ❌ No | `true` (si todos cumplen) | `false` (si al menos uno no cumple) | Validar todos | `callback(elemento, indice, array)` |
| **`.some()`** | ❌ No | `true` (si al menos uno cumple) | `false` (si ninguno cumple) | Validar al menos uno | `callback(elemento, indice, array)` |
| **`.reduce()`** | ❌ No | Valor acumulado | Error si array vacío sin valor inicial | Reducir a un valor | `callback(acum, elemento, indice, array), valorInicial` |
| **`.join()`** | ❌ No | String unido | String vacío `""` (si array vacío) | Unir en string | `separador` (opcional) |

*Nota: `.forEach()` no modifica el array, pero puede modificar elementos si son objetos.

### ⚠️ Importante: Uso de Valores de Retorno en Condicionales

**Conocer el valor de retorno es crucial** para usar correctamente los métodos en condicionales. Algunos valores que parecen "falsos" son en realidad **truthy**.

#### Problema Común: `-1` es Truthy

**⚠️ Error común**:
```javascript
let indice = array.findIndex(elemento => elemento === "valor");

// ❌ MALO: -1 es truthy, esto siempre se ejecuta
if (indice) {
    console.log("Encontrado en índice:", indice);
}
// Si no encuentra, indice = -1, y -1 es truthy, entonces se ejecuta incorrectamente
```

**✅ Correcto**:
```javascript
let indice = array.findIndex(elemento => elemento === "valor");

// ✅ BUENO: Comparar explícitamente
if (indice > -1) {
    console.log("Encontrado en índice:", indice);
}

// O mejor aún, usar !== -1
if (indice !== -1) {
    console.log("Encontrado en índice:", indice);
}
```

#### Valores de Retorno y Truthy/Falsy

| Método | Retorno Éxito | Retorno Fallo | ¿Es Truthy el Fallo? | Uso Correcto en Condicional |
|:---|:---|:---|:---|:---|
| **`.find()`** | Elemento (objeto/valor) | `undefined` | ❌ No (`undefined` es falsy) | `if (resultado)` ✅ |
| **`.findIndex()`** | Número ≥ 0 | `-1` | ✅ **Sí** (`-1` es truthy) | `if (indice !== -1)` o `if (indice > -1)` ✅ |
| **`.indexOf()`** | Número ≥ 0 | `-1` | ✅ **Sí** (`-1` es truthy) | `if (indice !== -1)` o `if (indice > -1)` ✅ |
| **`.includes()`** | `true` | `false` | ❌ No (`false` es falsy) | `if (resultado)` ✅ |
| **`.every()`** | `true` | `false` | ❌ No (`false` es falsy) | `if (resultado)` ✅ |
| **`.some()`** | `true` | `false` | ❌ No (`false` es falsy) | `if (resultado)` ✅ |
| **`.pop()`** | Elemento | `undefined` | ❌ No (`undefined` es falsy) | `if (resultado !== undefined)` ✅ |
| **`.shift()`** | Elemento | `undefined` | ❌ No (`undefined` es falsy) | `if (resultado !== undefined)` ✅ |

#### Ejemplos de Uso Correcto en Condicionales

**`.findIndex()` y `.indexOf()` - ⚠️ Cuidado con `-1`**:
```javascript
let frutas = ["manzana", "banana", "naranja"];

// ❌ MALO: -1 es truthy, esto siempre se ejecuta
let indice = frutas.indexOf("uva");
if (indice) {
    console.log("Encontrado");  // Se ejecuta incorrectamente si no encuentra
}

// ✅ BUENO: Comparar explícitamente
let indice = frutas.indexOf("uva");
if (indice !== -1) {
    console.log("Encontrado en índice:", indice);
} else {
    console.log("No encontrado");
}

// ✅ BUENO: Usar > -1
let indice = frutas.findIndex(fruta => fruta === "uva");
if (indice > -1) {
    console.log("Encontrado en índice:", indice);
}
```

**`.find()` - ✅ `undefined` es falsy**:
```javascript
let productos = [
    {nombre: "laptop", precio: 800000},
    {nombre: "mouse", precio: 80000}
];

// ✅ BUENO: undefined es falsy, funciona correctamente
let producto = productos.find(p => p.nombre === "teclado");
if (producto) {
    console.log("Producto encontrado:", producto);
} else {
    console.log("Producto no encontrado");
}
```

**`.includes()`, `.every()`, `.some()` - ✅ `false` es falsy**:
```javascript
let frutas = ["manzana", "banana"];

// ✅ BUENO: false es falsy, funciona correctamente
if (frutas.includes("uva")) {
    console.log("Tiene uva");
} else {
    console.log("No tiene uva");
}

// ✅ BUENO: every y some también retornan booleanos
let edades = [18, 20, 25];
if (edades.every(edad => edad >= 18)) {
    console.log("Todos son mayores de edad");
}
```

**`.pop()` y `.shift()` - ⚠️ Cuidado con arrays vacíos**:
```javascript
let array = [];

// ⚠️ Cuidado: pop() retorna undefined si array está vacío
let elemento = array.pop();
if (elemento) {  // undefined es falsy, esto no se ejecuta
    console.log("Elemento:", elemento);
} else {
    console.log("Array vacío");
}

// ✅ MEJOR: Verificar longitud primero
if (array.length > 0) {
    let elemento = array.pop();
    console.log("Elemento:", elemento);
}
```

#### Resumen de Mejores Prácticas

1. **`.findIndex()` y `.indexOf()`**: Siempre usar `!== -1` o `> -1`
   ```javascript
   if (indice !== -1) { }  // ✅ Correcto
   if (indice) { }          // ❌ Incorrecto (falla si no encuentra)
   ```

2. **`.find()`**: Puedes usar directamente en condicional (undefined es falsy)
   ```javascript
   if (resultado) { }  // ✅ Correcto
   ```

3. **`.includes()`, `.every()`, `.some()`**: Puedes usar directamente (retornan booleanos)
   ```javascript
   if (array.includes(valor)) { }  // ✅ Correcto
   ```

4. **`.pop()` y `.shift()`**: Verificar longitud primero o comparar con `undefined`
   ```javascript
   if (array.length > 0) { }  // ✅ Mejor práctica
   if (elemento !== undefined) { }  // ✅ Alternativa
   ```

---

## 6. Utilidades y Casos de Uso

### Cuándo Usar Cada Método

#### Para Modificar el Array Original
- **`.push()`**: Agregar elementos al final (listas, colas)
- **`.pop()`**: Eliminar último elemento (pilas, deshacer)
- **`.unshift()`**: Agregar al inicio (prioridades)
- **`.shift()`**: Eliminar primero (colas, procesar en orden)
- **`.splice()`**: Modificaciones precisas (editar, insertar, eliminar)
- **`.sort()`**: Ordenar elementos
- **`.reverse()`**: Invertir orden

#### Para Crear Nuevos Arrays
- **`.map()`**: Transformar cada elemento (convertir formatos, aplicar operaciones)
- **`.filter()`**: Seleccionar elementos (filtrar por condición)
- **`.slice()`**: Copiar porción (crear copias, extraer subarrays)

#### Para Buscar
- **`.find()`**: Encontrar primer elemento (búsqueda de objetos)
- **`.findIndex()`**: Encontrar índice (para usar con otros métodos)
- **`.indexOf()`**: Buscar valor específico (valores primitivos)
- **`.includes()`**: Verificar existencia (validaciones rápidas)

#### Para Validar
- **`.every()`**: Validar que todos cumplan (validación global)
- **`.some()`**: Validar que al menos uno cumpla (verificar presencia)

#### Para Acumular
- **`.reduce()`**: Reducir a un valor (sumas, promedios, agrupaciones)

#### Para Iterar
- **`.forEach()`**: Ejecutar código para cada elemento (efectos secundarios, imprimir, modificar DOM)

---

## 7. Combinando Métodos

Los métodos de arrays se pueden **encadenar** para crear operaciones complejas:

```javascript
let productos = [
    {nombre: "parlante", precio: 120000},
    {nombre: "laptop", precio: 800000},
    {nombre: "mouse gamer", precio: 80000},
    {nombre: "teclado", precio: 50000}
];

// Filtrar productos baratos, obtener solo nombres, convertir a mayúsculas
let nombresBaratos = productos
    .filter(producto => producto.precio < 100000)  // Filtrar
    .map(producto => producto.nombre)              // Extraer nombres
    .map(nombre => nombre.toUpperCase());          // Convertir a mayúsculas

console.log(nombresBaratos);  // ["MOUSE GAMER", "TECLADO"]
```

**Ventajas del encadenamiento**:
- ✅ Código más legible
- ✅ Operaciones paso a paso
- ✅ Fácil de entender el flujo

---

## 8. Inmutabilidad: Métodos que NO Modifican el Original

**Importante**: Los métodos como `map`, `filter`, `find`, `every`, `some`, `reduce` **no modifican el array original**. Esto es una buena práctica de programación funcional.

**Ventajas de la inmutabilidad**:
- ✅ Evita efectos secundarios
- ✅ Facilita depuración
- ✅ Permite comparar arrays originales
- ✅ Mejor para React y frameworks modernos

**Ejemplo**:
```javascript
let original = [1, 2, 3, 4, 5];
let dobles = original.map(n => n * 2);

console.log(original);  // [1, 2, 3, 4, 5] (sin cambios)
console.log(dobles);    // [2, 4, 6, 8, 10] (nuevo array)
```

---

## 9. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: Filter con Objetos

```javascript
let socios = [
    {nombre: "javier", activo: true},
    {nombre: "gabriel", activo: true},
    {nombre: "marina", activo: true},
    {nombre: "aristobulo", activo: true}
];

let noDeudores = socios.every(function(socio) {
    return socio.activo === true
});
console.log(noDeudores);  // true
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-12-javascript-arrays-metodos/arrays.js`

### Ejemplo 2: Find y FindIndex

```javascript
const productos = [
    {nombre: "parlante", precio: 120000},
    {nombre: "laptop", precio: 800000},
    {nombre: "mouse gamer", precio: 80000},
    {nombre: "teclado", precio: 50000}
];

// Encontrar producto barato
let productoBarato = productos.find(function(producto) {
    return producto.precio < 100000
});

// Encontrar índice
let indiceEncontrado = productos.findIndex(function(producto) {
    return producto.nombre === "mouse gamer"
});
console.log(indiceEncontrado);  // 2
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-12-javascript-arrays-metodos/arrays.js`

### Ejemplo 3: Reduce para Sumar

```javascript
let precios = [20000, 50000, 30000];
let total = precios.reduce(function(suma, precio) {
    return suma + precio
});
console.log(total);  // 100000
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-12-javascript-arrays-metodos/arrays.js`

### Ejemplo 4: Split y Join

```javascript
// Split - convertir string a array
let nums2 = "4, 5, 6, 7, 8, 9, 10";
console.log(nums2.split(","));  // ["4", " 5", " 6", " 7", " 8", " 9", " 10"]

// Join - convertir array a string
let names = ["pedro", "martina", "lucia"];
let namesJoined = names.join("-");
console.log(namesJoined);  // "pedro-martina-lucia"
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-12-javascript-arrays-metodos/arrays.js`

---

## 10. Buenas Prácticas ✅

### Usar Métodos Inmutables cuando sea Posible

```javascript
// ❌ MALO: Modifica el original
let numeros = [1, 2, 3];
numeros.reverse();
console.log(numeros);  // [3, 2, 1] (modificado)

// ✅ BUENO: Crea copia
let numeros = [1, 2, 3];
let invertidos = [...numeros].reverse();
console.log(numeros);    // [1, 2, 3] (sin cambios)
console.log(invertidos); // [3, 2, 1] (nuevo array)
```

### Usar `.map()` en lugar de `.forEach()` cuando Necesites un Nuevo Array

```javascript
// ❌ MALO: forEach no retorna nada
let numeros = [1, 2, 3];
let dobles = [];
numeros.forEach(n => dobles.push(n * 2));

// ✅ BUENO: map retorna nuevo array
let numeros = [1, 2, 3];
let dobles = numeros.map(n => n * 2);
```

### Validar Arrays Vacíos con `.reduce()`

```javascript
// ✅ Siempre proporciona valor inicial
let precios = [];
let total = precios.reduce((suma, precio) => suma + precio, 0);
console.log(total);  // 0 (seguro, no error)
```

### Usar `.includes()` en lugar de `.indexOf() !== -1`

```javascript
// ❌ Menos legible
if (frutas.indexOf("banana") !== -1) { }

// ✅ Más legible
if (frutas.includes("banana")) { }
```

---



**Próximo tema**: JavaScript: DOM y Eventos
