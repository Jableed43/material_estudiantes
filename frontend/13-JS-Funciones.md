# JavaScript: Funciones y Bucles 🛠️

## Funciones

### ¿Qué es una Función?

Una función es un bloque de código reutilizable que realiza una tarea específica. Puede recibir parámetros y retornar un valor.

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

## Tipos de Funciones

### Función Declarada

```javascript
function sumar(a, b) {
  return a + b;
}

let resultado = sumar(5, 3);
console.log(resultado); // 8
```

### Función Expresada (Function Expression)

```javascript
const multiplicar = function(a, b) {
  return a * b;
};

let resultado = multiplicar(4, 5);
console.log(resultado); // 20
```

### Arrow Function (Función Flecha)

```javascript
// Sintaxis básica
const dividir = (a, b) => {
  return a / b;
};

// Si solo tiene una línea, se puede simplificar
const restar = (a, b) => a - b;

// Si solo tiene un parámetro, se pueden omitir los paréntesis
const cuadrado = x => x * x;

// Sin parámetros
const saludar = () => "Hola!";
```

---

## Parámetros y Argumentos

```javascript
// Parámetros: variables en la definición
function calcularArea(ancho, alto) {
  return ancho * alto;
}

// Argumentos: valores pasados al llamar la función
let area = calcularArea(10, 5);
console.log(area); // 50
```

### Parámetros por Defecto

```javascript
function saludar(nombre = "Invitado") {
  return `Hola, ${nombre}!`;
}

console.log(saludar()); // "Hola, Invitado!"
console.log(saludar("Juan")); // "Hola, Juan!"
```

### Rest Parameters

```javascript
function sumar(...numeros) {
  return numeros.reduce((total, num) => total + num, 0);
}

console.log(sumar(1, 2, 3, 4)); // 10
```

---

## Return

```javascript
function esMayor(edad) {
  if (edad >= 18) {
    return true;
  }
  return false;
}

// Si no hay return, la función retorna undefined
function sinReturn() {
  console.log("Hola");
}

let resultado = sinReturn(); // undefined
```

---

## Bucles (Loops)

### for

```javascript
// Sintaxis: for (inicio; condición; incremento)
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// Recorrer array
let frutas = ["manzana", "banana", "naranja"];
for (let i = 0; i < frutas.length; i++) {
  console.log(frutas[i]);
}
```

### for...of (Arrays)

```javascript
let frutas = ["manzana", "banana", "naranja"];

for (let fruta of frutas) {
  console.log(fruta);
}
// "manzana"
// "banana"
// "naranja"
```

### for...in (Objetos)

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

### while

```javascript
let contador = 0;

while (contador < 5) {
  console.log(contador); // 0, 1, 2, 3, 4
  contador++;
}
```

### do...while

```javascript
let numero;

do {
  numero = prompt("Ingresa un número mayor a 10:");
} while (numero <= 10);
```

---

## Métodos de Array con Funciones

### forEach

```javascript
let numeros = [1, 2, 3, 4, 5];

numeros.forEach((numero) => {
  console.log(numero * 2);
});
```

### map

```javascript
let numeros = [1, 2, 3, 4, 5];
let duplicados = numeros.map(num => num * 2);
console.log(duplicados); // [2, 4, 6, 8, 10]
```

### filter

```javascript
let numeros = [1, 2, 3, 4, 5, 6];
let pares = numeros.filter(num => num % 2 === 0);
console.log(pares); // [2, 4, 6]
```

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

### Ejemplo 2: Función para Validar Email

```javascript
function validarEmail(email) {
  return email.includes("@") && email.includes(".");
}

console.log(validarEmail("juan@example.com")); // true
console.log(validarEmail("juan")); // false
```

### Ejemplo 3: Bucle para Crear Tabla

```javascript
function crearTablaMultiplicar(numero) {
  for (let i = 1; i <= 10; i++) {
    console.log(`${numero} x ${i} = ${numero * i}`);
  }
}

crearTablaMultiplicar(5);
```

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

---

## Conceptos Clave

1. **Función**: Bloque de código reutilizable
2. **Parámetros**: Variables en la definición
3. **Argumentos**: Valores pasados al llamar
4. **Return**: Retorna un valor (o undefined)
5. **for**: Bucle con contador
6. **while**: Bucle con condición
7. **Arrow Functions**: Sintaxis moderna y concisa

---

## Buenas Prácticas

- Usa nombres descriptivos para funciones
- Mantén las funciones pequeñas y enfocadas
- Usa arrow functions para funciones simples
- Evita bucles infinitos (asegúrate de que la condición cambie)
- Usa `map`, `filter`, `forEach` en lugar de `for` cuando sea posible
- Documenta funciones complejas con comentarios
- Usa parámetros por defecto para mayor flexibilidad

