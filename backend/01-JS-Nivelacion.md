# JavaScript: Nivelación Backend 📚

## Variables

```javascript
// let: puede cambiar, scope de bloque
let edad = 20

// const: no puede cambiar, scope de bloque
const nombre = "Juan"

// var: puede cambiar, scope global o de función (evitar usar)
var antigua = "no recomendado"
```

## Tipos de Datos

- **Primitivos**: `number`, `string`, `boolean`, `undefined`, `null`, `symbol`
- **Complejos**: `object`, `array`, `function`

## Funciones

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

## Estructuras de Control

```javascript
// if/else
if (condicion) {
    // código
} else {
    // código
}

// Ternario
const resultado = condicion ? "verdadero" : "falso"

// Switch
switch (valor) {
    case 1:
        // código
        break
    default:
        // código
}
```

## Métodos de Array

```javascript
// forEach: ejecuta función por cada elemento
array.forEach(elemento => console.log(elemento))

// map: crea nuevo array transformado
const duplicados = array.map(num => num * 2)

// filter: crea nuevo array filtrado
const pares = array.filter(num => num % 2 === 0)

// reduce: reduce array a un valor
const suma = array.reduce((acc, num) => acc + num, 0)
```

## Scope (Alcance)

```javascript
// Scope global
let global = "soy global"

function ejemplo() {
    // Scope local
    let local = "soy local"
    console.log(global) // ✅ Accesible
}

console.log(local) // ❌ Error: no accesible
```

## Objetos

```javascript
const usuario = {
    nombre: "Juan",
    edad: 25,
    saludar() {
        return `Hola, soy ${this.nombre}`
    }
}

// Acceder a propiedades
usuario.nombre
usuario["nombre"]
```

## Conceptos Clave

1. **Variables**: `let` y `const` son preferibles a `var`
2. **Funciones**: Diferentes formas de declarar funciones
3. **Arrays**: Métodos modernos (`map`, `filter`, `reduce`)
4. **Scope**: Entender alcance global vs local
5. **Objetos**: Estructura y acceso a propiedades
6. **Truthy/Falsy**: Valores que se evalúan como booleanos
7. **Comparación**: Usar `===` en lugar de `==`

