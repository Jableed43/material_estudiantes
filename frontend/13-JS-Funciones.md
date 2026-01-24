# JavaScript: Funciones y Bucles 🛠️

## 📑 Índice

1. [¿Qué es una Función? (Analogía del Mundo Real)](#qué-es-una-función-analogía-del-mundo-real)
2. [¿Por qué Usar Funciones?](#por-qué-usar-funciones)
3. [Tipos de Funciones](#tipos-de-funciones)
   - Función Declarada
   - Función Expresada
   - Arrow Function
4. [Parámetros y Argumentos](#parámetros-y-argumentos)
5. [Return: Retornar Valores](#return-retornar-valores)
6. [Bucles (Loops)](#bucles-loops)
   - for
   - for...of
   - for...in
   - while
   - do...while
7. [Métodos de Array con Funciones](#métodos-de-array-con-funciones)
8. [Ejemplos Prácticos](#ejemplos-prácticos)
9. [Conceptos Clave](#conceptos-clave)
10. [Buenas Prácticas](#buenas-prácticas)
11. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es una Función? (Analogía del Mundo Real)

### 🍳 Analogía: La Receta de Cocina

Imagina que tienes una receta de cocina para hacer pizza:
- **Nombre de la receta** (nombre de función): "Hacer Pizza"
- **Ingredientes** (parámetros): Harina, queso, tomate
- **Pasos** (código): Mezclar, amasar, hornear
- **Resultado** (return): Una pizza lista

**Cada vez que quieres pizza, sigues la misma receta** - no inventas una nueva cada vez. Eso es una función: código reutilizable.

### 🏭 Analogía: La Máquina de la Fábrica

Piensa en una máquina de una fábrica que hace galletas:
- **Entrada** (parámetros): Harina, azúcar, mantequilla
- **Proceso** (código): La máquina mezcla, corta y hornea
- **Salida** (return): Galletas listas

**Cada vez que pones los mismos ingredientes, obtienes el mismo resultado**. Eso es una función: misma entrada, mismo proceso, misma salida.

### 🎯 Analogía: El Lanzador de Dardos

Un lanzador de dardos profesional:
- **Preparación** (definir función): Aprende la técnica
- **Lanzamiento** (llamar función): Ejecuta la técnica
- **Resultado** (return): Dardo en el blanco

**Cada vez que lanza, usa la misma técnica** - no inventa una nueva cada vez.

### 🧮 Analogía: La Calculadora

Una calculadora tiene funciones predefinidas:
- **Función "Sumar"**: Le das dos números, te devuelve la suma
- **Función "Multiplicar"**: Le das dos números, te devuelve el producto

**Cada vez que quieres sumar, usas la misma función** - no escribes el proceso completo cada vez.

### ¿Qué es una Función en Programación?

Una **función** es un bloque de código reutilizable que realiza una tarea específica. Puede recibir parámetros y retornar un valor.

**En términos simples**: Es como una receta o una máquina que puedes usar una y otra vez con diferentes ingredientes (parámetros) para obtener resultados.

```javascript
// Declaración de función
function saludar(nombre) {
  return `Hola, ${nombre}!`;
}

// Llamada a la función
let mensaje = saludar("Juan");
console.log(mensaje); // "Hola, Juan!"
```

---

## ¿Por qué Usar Funciones?

### ❌ Problema: Repetir Código

**Analogía**: Como escribir la misma receta una y otra vez cada vez que quieres cocinar.

```javascript
// Sin funciones - código repetido
let area1 = 5 * 3;
let area2 = 10 * 4;
let area3 = 7 * 2;
// ... repetir para cada cálculo
```

**Problemas**:
- ❌ Si la fórmula cambia, debes cambiarla en todas partes
- ❌ Aumenta la cantidad de bugs
- ❌ Dificulta el mantenimiento
- ❌ Código más largo y difícil de leer

### ✅ Solución: Usar Funciones

**Analogía**: Como tener una receta escrita una vez y usarla cada vez que necesitas.

```javascript
// Con funciones - código reutilizable
function calcularArea(ancho, alto) {
  return ancho * alto;
}

let area1 = calcularArea(5, 3);
let area2 = calcularArea(10, 4);
let area3 = calcularArea(7, 2);
```

**Ventajas**:
- ✅ Mantenible: un solo lugar para cambios
- ✅ Única fuente de la verdad
- ✅ Separación de responsabilidades
- ✅ Facilita lectura del código
- ✅ Ahorra tiempo, disminuye errores

### El Principio DRY (Don't Repeat Yourself)

**DRY = "No te repitas"**

**Analogía**: Como tener una herramienta que usas para múltiples tareas, en lugar de crear una nueva herramienta cada vez.

---

## Tipos de Funciones

### Función Declarada

**Analogía**: Como una herramienta que siempre está disponible, incluso antes de que la necesites.

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

**Analogía del hoisting**: Como tener una herramienta que siempre está en tu caja de herramientas, sin importar dónde la pongas.

### Función Expresada (Function Expression)

**Analogía**: Como una herramienta que solo está disponible después de que la compras y la guardas.

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

**Analogía**: Como comprar una herramienta. Solo la puedes usar después de comprarla.

### Arrow Function (Función Flecha)

**Analogía**: Como una herramienta moderna y compacta.

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

// Sin parámetros
const saludar = () => "Hola!";
```

**Analogía**: Como una herramienta plegable. Hace lo mismo pero ocupa menos espacio.

---

## Parámetros y Argumentos

### ¿Cuál es la Diferencia?

**Analogía**: Como la diferencia entre la receta (parámetros) y los ingredientes reales (argumentos).

- **Parámetro**: Variable en la definición de la función (la "receta")
- **Argumento**: Valor pasado al invocar la función (los "ingredientes reales")

```javascript
// Parámetros: 'nombre' y 'edad' (las variables en la receta)
function presentar(nombre, edad) {
  return `Soy ${nombre} y tengo ${edad} años`;
}

// Argumentos: "Javier" y 33 (los valores reales que usas)
presentar("Javier", 33);
```

**Analogía**: 
- **Parámetro** = "Harina" (en la receta)
- **Argumento** = "500g de harina" (lo que realmente usas)

### Parámetros por Defecto

**Analogía**: Como tener ingredientes opcionales en una receta. Si no los tienes, usas los que están por defecto.

```javascript
function saludar(nombre = "Invitado") {
  return `Hola, ${nombre}!`;
}

console.log(saludar());        // "Hola, Invitado!" (usa el valor por defecto)
console.log(saludar("Juan"));   // "Hola, Juan!" (usa el argumento)
```

**Analogía**: Como una receta que dice "opcional: especias". Si no las tienes, la receta funciona igual.

### Rest Parameters

**Analogía**: Como una receta que acepta cualquier cantidad de ingredientes adicionales.

```javascript
function sumar(...numeros) {
  return numeros.reduce((total, num) => total + num, 0);
}

console.log(sumar(1, 2, 3, 4));  // 10 (puedes pasar cualquier cantidad)
```

**Analogía**: Como una ensalada. Puedes agregar tantos ingredientes como quieras.

---

## Return: Retornar Valores

### ¿Qué hace Return?

**Analogía**: Como el resultado final de tu receta.

**Return hace dos cosas**:
1. **Retorna un valor** de la función
2. **Termina la ejecución** de la función (código después de `return` no se ejecuta)

```javascript
function calcularArea(largo, ancho) {
  return largo * ancho;  // Retorna el resultado
  console.log("Esto nunca se ejecuta");  // ❌ No se ejecuta
}
```

**Analogía**: Como terminar una receta. Una vez que tienes el resultado final, no sigues cocinando.

### Funciones Sin Return

**Analogía**: Como una receta que no produce un resultado que puedas usar después.

```javascript
function sinReturn() {
  console.log("Hola");
  // No hay return - retorna undefined
}

let resultado = sinReturn();  // undefined
```

**Analogía**: Como hacer algo que no produce un objeto físico. Haces la acción, pero no obtienes algo que puedas guardar.

---

## Bucles (Loops)

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

### for

**¿Cuándo usar?**: Cuando conoces el número de iteraciones o quieres control completo.

**Analogía**: Como contar hasta 10. Sabes exactamente cuántas veces vas a contar.

```javascript
// Sintaxis: for (inicialización; condición; actualización)
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

**Analogía**: Como contar hacia atrás desde 10 hasta 0.

### for...of (Arrays)

**Analogía**: Como revisar cada elemento de una lista sin preocuparte por el índice.

```javascript
let frutas = ["manzana", "banana", "naranja"];

for (let fruta of frutas) {
  console.log(fruta);
}
// "manzana"
// "banana"
// "naranja"
```

**Analogía**: Como revisar cada libro en una estantería. No necesitas saber en qué posición está, solo lo revisas.

### for...in (Objetos)

**Analogía**: Como revisar cada propiedad de un objeto.

```javascript
let persona = {
  nombre: "Juan",
  edad: 25,
  ciudad: "Buenos Aires"
};

for (let clave in persona) {
  console.log(`${clave}: ${persona[clave]}`);
}
// "nombre: Juan"
// "edad: 25"
// "ciudad: Buenos Aires"
```

**Analogía**: Como revisar cada compartimento de una caja. Ves qué hay en cada uno.

### while

**¿Cuándo usar?**: Cuando no conoces el número exacto de iteraciones, pero sabes la condición.

**Analogía**: Como seguir intentando hasta que algo cambie.

```javascript
let contador = 0;

while (contador < 5) {
  console.log(contador);  // 0, 1, 2, 3, 4
  contador++;  // ⚠️ IMPORTANTE: Actualizar para evitar loop infinito
}
```

**⚠️ Cuidado con loops infinitos**: Siempre actualiza la condición dentro del bucle.

**Analogía**: Como seguir intentando abrir una puerta hasta que se abra. Debes intentar (actualizar) o nunca se abrirá.

### do...while

**¿Cuándo usar?**: Cuando quieres que el código se ejecute **al menos una vez** antes de comprobar la condición.

**Analogía**: Como probar algo al menos una vez antes de decidir si seguir.

```javascript
let numero;
do {
  numero = prompt("Ingresa un número mayor a 10:");
} while (numero <= 10);
```

**Diferencia con `while`**:
- `while`: Comprueba condición primero
- `do...while`: Ejecuta código primero, luego comprueba

**Analogía**: 
- `while` = "Si está lloviendo, llevo paraguas" (verificas primero)
- `do...while` = "Llevo paraguas, y si no llueve, lo dejo" (actúas primero)

---

## Métodos de Array con Funciones

### forEach

**Analogía**: Como revisar cada elemento de una lista y hacer algo con cada uno.

```javascript
let numeros = [1, 2, 3, 4, 5];

numeros.forEach((numero) => {
  console.log(numero * 2);
});
```

**Analogía**: Como revisar cada libro en una biblioteca y leer su título.

### map

**Analogía**: Como transformar cada elemento de una lista en algo nuevo.

```javascript
let numeros = [1, 2, 3, 4, 5];
let duplicados = numeros.map(num => num * 2);
console.log(duplicados); // [2, 4, 6, 8, 10]
```

**Analogía**: Como tomar una lista de números y crear una nueva lista con cada número duplicado.

### filter

**Analogía**: Como filtrar elementos de una lista según una condición.

```javascript
let numeros = [1, 2, 3, 4, 5, 6];
let pares = numeros.filter(num => num % 2 === 0);
console.log(pares); // [2, 4, 6]
```

**Analogía**: Como separar los números pares de una lista de números.

---

## Ejemplos Prácticos

### Ejemplo 1: Función para Calcular Promedio

```javascript
function calcularPromedio(...numeros) {
  if (numeros.length === 0) return 0;
  
  let suma = numeros.reduce((total, num) => total + num, 0);
  return suma / numeros.length;
}

console.log(calcularPromedio(10, 20, 30)); // 20
```

**Analogía**: Como calcular el promedio de tus calificaciones. Sumas todas y divides por la cantidad.

### Ejemplo 2: Función para Validar Email

```javascript
function validarEmail(email) {
  return email.includes("@") && email.includes(".");
}

console.log(validarEmail("juan@example.com")); // true
console.log(validarEmail("juan")); // false
```

**Analogía**: Como verificar que un email tiene el formato correcto (debe tener @ y punto).

### Ejemplo 3: Bucle para Crear Tabla

```javascript
function crearTablaMultiplicar(numero) {
  for (let i = 1; i <= 10; i++) {
    console.log(`${numero} x ${i} = ${numero * i}`);
  }
}

crearTablaMultiplicar(5);
```

**Analogía**: Como crear una tabla de multiplicar. Repites la multiplicación 10 veces.

### Ejemplo 4: Función con Arrow Function

```javascript
const procesarDatos = (datos) => {
  return datos
    .filter(item => item.activo)
    .map(item => item.nombre.toUpperCase())
    .sort();
};

let datos = [
  { nombre: "juan", activo: true },
  { nombre: "maria", activo: false },
  { nombre: "pedro", activo: true }
];

console.log(procesarDatos(datos)); // ["JUAN", "PEDRO"]
```

**Analogía**: Como procesar una lista de personas: filtrar las activas, obtener sus nombres en mayúsculas y ordenarlos.

---

## Conceptos Clave

1. **Función**: Bloque de código reutilizable
2. **Parámetros**: Variables en la definición
3. **Argumentos**: Valores pasados al llamar
4. **Return**: Retorna un valor (o undefined)
5. **for**: Bucle con contador
6. **while**: Bucle con condición
7. **Arrow Functions**: Sintaxis moderna y concisa
8. **DRY**: Don't Repeat Yourself (No te repitas)

---

## Buenas Prácticas

- ✅ Usa nombres descriptivos para funciones
- ✅ Mantén las funciones pequeñas y enfocadas
- ✅ Usa arrow functions para funciones simples
- ✅ Evita bucles infinitos (asegúrate de que la condición cambie)
- ✅ Usa `map`, `filter`, `forEach` en lugar de `for` cuando sea posible
- ✅ Documenta funciones complejas con comentarios
- ✅ Usa parámetros por defecto para mayor flexibilidad
- ✅ Una función debe hacer una sola cosa (principio de responsabilidad única)

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [JavaScript: Variables](./10-JS-Variables.md) - Variables usadas en funciones
- 📚 [JavaScript: Condicionales](./11-JS-Condicionales.md) - Condicionales dentro de funciones
- 📚 [JavaScript: Arrays](./12-JS-Arrays.md) - Métodos de array que usan funciones

### Código Relacionado

- 💻 [Tema 13: Funciones y Ciclos](../../CODIGO/frontend/tema-13-javascript-funciones-ciclos/)

---

## 🎯 Puntos Clave para Recordar

1. **Función = Receta**: Código reutilizable que hace una tarea específica
2. **Parámetros = Ingredientes**: Lo que la función necesita
3. **Return = Resultado**: Lo que la función produce
4. **Bucles = Repetición**: Ejecuta código múltiples veces
5. **DRY = No te repitas**: Usa funciones para evitar código duplicado

---

## 💡 Ejercicio Mental

Piensa en acciones de la vida real como funciones:

- **"Cocinar"**: Ingredientes → Proceso → Comida lista
- **"Lavar ropa"**: Ropa sucia → Proceso → Ropa limpia
- **"Calcular total"**: Precios → Suma → Total

¡Practica identificando funciones en tu vida diaria!
