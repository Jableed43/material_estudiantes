# JavaScript: Condicionales y Switch 🛣️

## Estructuras de Control

Las estructuras de control permiten que el código tome decisiones y ejecute diferentes bloques según condiciones.

---

## Condicionales

### if / else

```javascript
// Sintaxis básica
if (condición) {
  // Código si la condición es verdadera
} else {
  // Código si la condición es falsa
}

// Ejemplo
let edad = 18;

if (edad >= 18) {
  console.log("Eres mayor de edad");
} else {
  console.log("Eres menor de edad");
}
```

### if / else if / else

```javascript
let nota = 85;

if (nota >= 90) {
  console.log("Excelente");
} else if (nota >= 70) {
  console.log("Bueno");
} else if (nota >= 60) {
  console.log("Aprobado");
} else {
  console.log("Reprobado");
}
```

### Operador Ternario

```javascript
// Sintaxis: condición ? valorSiVerdadero : valorSiFalso
let edad = 20;
let mensaje = edad >= 18 ? "Mayor" : "Menor";
console.log(mensaje); // "Mayor"

// También se puede usar para asignaciones
let precio = 100;
let descuento = precio > 50 ? precio * 0.1 : 0;
```

### Operador Lógico AND (&&)

```javascript
// Ejecuta código solo si ambas condiciones son verdaderas
let usuario = { nombre: "Juan", edad: 25 };

if (usuario && usuario.edad >= 18) {
  console.log("Usuario válido");
}

// También se puede usar para ejecutar código condicionalmente
usuario && console.log(usuario.nombre);
```

### Operador Lógico OR (||)

```javascript
// Usa el primer valor verdadero
let nombre = usuario.nombre || "Invitado";
console.log(nombre); // "Juan" si existe, "Invitado" si no

// Ejemplo práctico
let puerto = process.env.PORT || 3000;
```

---

## Switch (Múltiples Casos)

Útil cuando tienes múltiples condiciones basadas en el mismo valor.

```javascript
let dia = 3;
let diaSemana;

switch (dia) {
  case 1:
    diaSemana = "Lunes";
    break;
  case 2:
    diaSemana = "Martes";
    break;
  case 3:
    diaSemana = "Miércoles";
    break;
  case 4:
    diaSemana = "Jueves";
    break;
  case 5:
    diaSemana = "Viernes";
    break;
  default:
    diaSemana = "Fin de semana";
}

console.log(diaSemana); // "Miércoles"
```

### Casos Múltiples

```javascript
let mes = 2;

switch (mes) {
  case 12:
  case 1:
  case 2:
    console.log("Invierno");
    break;
  case 3:
  case 4:
  case 5:
    console.log("Primavera");
    break;
  case 6:
  case 7:
  case 8:
    console.log("Verano");
    break;
  case 9:
  case 10:
  case 11:
    console.log("Otoño");
    break;
  default:
    console.log("Mes inválido");
}
```

---

## Truthy y Falsy

JavaScript evalúa valores como verdaderos o falsos en contextos booleanos.

### Valores Falsy (se evalúan como `false`):
- `false`
- `0`
- `""` (string vacío)
- `null`
- `undefined`
- `NaN`

### Valores Truthy (se evalúan como `true`):
- Cualquier número distinto de 0
- Cualquier string no vacío
- `[]` (array vacío)
- `{}` (objeto vacío)
- `true`

```javascript
// Ejemplos
if (0) { } // No se ejecuta (falsy)
if (1) { } // Se ejecuta (truthy)
if ("") { } // No se ejecuta (falsy)
if ("hola") { } // Se ejecuta (truthy)
if ([]) { } // Se ejecuta (truthy)
if (null) { } // No se ejecuta (falsy)
```

---

## Ejemplos Prácticos

### Ejemplo 1: Validación de Usuario

```javascript
let usuario = {
  nombre: "Juan",
  edad: 20,
  email: "juan@example.com"
};

if (usuario.edad >= 18 && usuario.email) {
  console.log("Usuario válido");
} else {
  console.log("Usuario inválido");
}
```

### Ejemplo 2: Sistema de Descuentos

```javascript
let total = 150;
let descuento = 0;

if (total > 200) {
  descuento = total * 0.2; // 20% de descuento
} else if (total > 100) {
  descuento = total * 0.1; // 10% de descuento
} else {
  descuento = 0;
}

let precioFinal = total - descuento;
console.log(`Precio final: $${precioFinal}`);
```

### Ejemplo 3: Día de la Semana con Switch

```javascript
let dia = new Date().getDay();
let mensaje;

switch (dia) {
  case 0:
    mensaje = "Domingo - Descanso";
    break;
  case 6:
    mensaje = "Sábado - Fin de semana";
    break;
  default:
    mensaje = "Día laboral";
}

console.log(mensaje);
```

### Ejemplo 4: Operador Ternario Anidado

```javascript
let edad = 20;
let tipo = edad < 13 ? "Niño" : 
           edad < 18 ? "Adolescente" : 
           edad < 65 ? "Adulto" : "Adulto mayor";

console.log(tipo); // "Adulto"
```

---

## Conceptos Clave

1. **if/else**: Estructura básica de decisión
2. **else if**: Múltiples condiciones
3. **Operador Ternario**: Forma corta de if/else
4. **Switch**: Múltiples casos basados en un valor
5. **Truthy/Falsy**: Valores que se evalúan como booleanos
6. **Operadores Lógicos**: `&&` (AND), `||` (OR), `!` (NOT)
7. **Break**: Sale del switch (importante para evitar fall-through)

---

## Buenas Prácticas

- Usa `switch` cuando tengas múltiples casos basados en el mismo valor
- Siempre incluye `break` en cada caso de `switch` (a menos que quieras fall-through)
- Usa operador ternario para asignaciones simples
- Valida datos antes de usarlos en condicionales
- Usa `&&` y `||` para valores por defecto
- Evita anidar demasiados `if/else` (considera funciones o switch)

