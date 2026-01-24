# JavaScript: Ejercicios Prácticos 💪

## Ejercicios para Practicar

### Ejercicio 1: Calculadora Simple

```javascript
function calculadora(operacion, a, b) {
  switch (operacion) {
    case "suma":
      return a + b;
    case "resta":
      return a - b;
    case "multiplicacion":
      return a * b;
    case "division":
      return b !== 0 ? a / b : "Error: División por cero";
    default:
      return "Operación no válida";
  }
}

console.log(calculadora("suma", 10, 5)); // 15
console.log(calculadora("division", 10, 0)); // "Error: División por cero"
```

### Ejercicio 2: Validar Contraseña

```javascript
function validarContraseña(contraseña) {
  if (contraseña.length < 8) {
    return "La contraseña debe tener al menos 8 caracteres";
  }
  
  if (!/[A-Z]/.test(contraseña)) {
    return "La contraseña debe tener al menos una mayúscula";
  }
  
  if (!/[0-9]/.test(contraseña)) {
    return "La contraseña debe tener al menos un número";
  }
  
  return "Contraseña válida";
}

console.log(validarContraseña("MiPass123")); // "Contraseña válida"
console.log(validarContraseña("corta")); // "La contraseña debe tener al menos 8 caracteres"
```

### Ejercicio 3: Contador de Palabras

```javascript
function contarPalabras(texto) {
  let palabras = texto.trim().split(/\s+/);
  return palabras.length;
}

console.log(contarPalabras("Hola mundo JavaScript")); // 3
console.log(contarPalabras("  Uno   Dos   Tres  ")); // 3
```

### Ejercicio 4: Buscar en Array

```javascript
function buscarEnArray(array, valor) {
  for (let i = 0; i < array.length; i++) {
    if (array[i] === valor) {
      return i; // Retorna el índice
    }
  }
  return -1; // No encontrado
}

let frutas = ["manzana", "banana", "naranja"];
console.log(buscarEnArray(frutas, "banana")); // 1
console.log(buscarEnArray(frutas, "uva")); // -1
```

### Ejercicio 5: Invertir String

```javascript
function invertirString(texto) {
  let invertido = "";
  for (let i = texto.length - 1; i >= 0; i--) {
    invertido += texto[i];
  }
  return invertido;
}

console.log(invertirString("Hola")); // "aloH
```

### Ejercicio 6: Factorial

```javascript
function factorial(n) {
  if (n === 0 || n === 1) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120 (5 * 4 * 3 * 2 * 1)
```

### Ejercicio 7: Números Primos

```javascript
function esPrimo(numero) {
  if (numero < 2) return false;
  
  for (let i = 2; i < numero; i++) {
    if (numero % i === 0) {
      return false;
    }
  }
  return true;
}

console.log(esPrimo(7)); // true
console.log(esPrimo(10)); // false
```

### Ejercicio 8: Palíndromo

```javascript
function esPalindromo(texto) {
  let limpio = texto.toLowerCase().replace(/\s/g, "");
  let invertido = limpio.split("").reverse().join("");
  return limpio === invertido;
}

console.log(esPalindromo("anilina")); // true
console.log(esPalindromo("hola")); // false
```

---

## Proyectos Prácticos

### Proyecto 1: Lista de Tareas

```javascript
let tareas = [];

function agregarTarea(tarea) {
  tareas.push({ id: Date.now(), texto: tarea, completada: false });
}

function completarTarea(id) {
  let tarea = tareas.find(t => t.id === id);
  if (tarea) {
    tarea.completada = true;
  }
}

function eliminarTarea(id) {
  tareas = tareas.filter(t => t.id !== id);
}

function listarTareas() {
  tareas.forEach(tarea => {
    let estado = tarea.completada ? "✓" : "○";
    console.log(`${estado} ${tarea.texto}`);
  });
}

agregarTarea("Aprender JavaScript");
agregarTarea("Practicar funciones");
listarTareas();
```

### Proyecto 2: Calculadora de Edad

```javascript
function calcularEdad(fechaNacimiento) {
  let hoy = new Date();
  let nacimiento = new Date(fechaNacimiento);
  let edad = hoy.getFullYear() - nacimiento.getFullYear();
  let mes = hoy.getMonth() - nacimiento.getMonth();
  
  if (mes < 0 || (mes === 0 && hoy.getDate() < nacimiento.getDate())) {
    edad--;
  }
  
  return edad;
}

console.log(calcularEdad("1990-05-15")); // Edad actual
```

---

## Conceptos Clave

1. **Práctica constante**: Resuelve ejercicios regularmente
2. **Debugging**: Usa `console.log()` para entender el flujo
3. **Paso a paso**: Divide problemas complejos en partes pequeñas
4. **Revisa tu código**: Verifica lógica y sintaxis
5. **Experimenta**: Prueba diferentes soluciones
6. **Lee código**: Estudia ejemplos de otros desarrolladores
7. **Construye proyectos**: Aplica lo aprendido en proyectos reales

---

## Consejos para Practicar

- Empieza con ejercicios simples y aumenta la dificultad
- Resuelve el mismo problema de diferentes formas
- Usa funciones para organizar tu código
- Comenta tu código para entender mejor
- Prueba casos extremos (valores vacíos, negativos, etc.)
- Comparte tu código y pide feedback
- No te rindas, la práctica hace al maestro

