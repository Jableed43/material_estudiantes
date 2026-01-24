# JavaScript: Condicionales y Switch 🛣️

## 📑 Índice

1. [¿Qué son las Estructuras de Control? (Analogía del Mundo Real)](#qué-son-las-estructuras-de-control-analogía-del-mundo-real)
2. [Condicionales: Tomar Decisiones](#condicionales-tomar-decisiones)
   - if / else
   - if / else if / else
   - Operador Ternario
3. [Operadores Lógicos](#operadores-lógicos)
   - AND (&&)
   - OR (||)
   - NOT (!)
4. [Switch: Múltiples Casos](#switch-múltiples-casos)
5. [Truthy y Falsy](#truthy-y-falsy)
6. [Ejemplos Prácticos](#ejemplos-prácticos)
7. [Conceptos Clave](#conceptos-clave)
8. [Buenas Prácticas](#buenas-prácticas)
9. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué son las Estructuras de Control? (Analogía del Mundo Real)

### 🛣️ Analogía: Las Bifurcaciones en el Camino

Imagina que estás conduciendo y llegas a una bifurcación:
- **Si** el semáforo está en verde → **Entonces** sigues recto
- **Si no** (el semáforo está en rojo) → **Entonces** te detienes

**Las estructuras de control son como esas bifurcaciones**: Te permiten tomar decisiones y elegir qué camino seguir según las condiciones.

### 🎯 Analogía: El Guardia de Seguridad

Piensa en un guardia de seguridad en la entrada de un edificio:
- **Si** tienes identificación → **Entonces** te deja pasar
- **Si no** → **Entonces** te detiene

**El código funciona igual**: Evalúa condiciones y decide qué hacer.

### 🎲 Analogía: El Dado

Cuando lanzas un dado:
- **Si** sale 6 → Ganas
- **Si** sale 5 → Pierdes
- **Si** sale otro número → Vuelves a lanzar

**Cada condición tiene una acción diferente**. Eso es lo que hacen las estructuras de control.

### ¿Qué son las Estructuras de Control?

Las **estructuras de control** permiten que el código tome decisiones y ejecute diferentes bloques según condiciones.

**En términos simples**: Son como las reglas que sigues en la vida diaria: "Si llueve, llevo paraguas. Si no, no lo llevo."

---

## Condicionales: Tomar Decisiones

### if / else

**Analogía**: Como una pregunta de sí o no.

Imagina que preguntas: "¿Tengo hambre?"
- **Si** la respuesta es sí → Como algo
- **Si no** → No como nada

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
  console.log("Eres mayor de edad");  // Se ejecuta si edad >= 18
} else {
  console.log("Eres menor de edad");   // Se ejecuta si edad < 18
}
```

**Analogía del mundo real**: Como un semáforo:
- **Si** está en verde → Avanzas
- **Si no** (está en rojo) → Te detienes

### if / else if / else

**Analogía**: Como un sistema de calificaciones.

Imagina que estás calificando un examen:
- **Si** la nota es 90 o más → Excelente
- **Si no, si** la nota es 70 o más → Bueno
- **Si no, si** la nota es 60 o más → Aprobado
- **Si no** → Reprobado

```javascript
let nota = 85;

if (nota >= 90) {
  console.log("Excelente");
} else if (nota >= 70) {
  console.log("Bueno");      // Se ejecuta este (85 >= 70)
} else if (nota >= 60) {
  console.log("Aprobado");
} else {
  console.log("Reprobado");
}
```

**Importante**: JavaScript evalúa las condiciones **en orden**. La primera que sea verdadera se ejecuta y las demás se ignoran.

**Analogía**: Como una escalera. Subes escalón por escalón hasta encontrar el que corresponde a tu altura.

### Operador Ternario

**Analogía**: Como una forma rápida de decidir.

Imagina que preguntas: "¿Llevo paraguas?"
- **Si** está lloviendo → Sí, llevo paraguas
- **Si no** → No, no lo llevo

**El operador ternario es como una forma corta de escribir esto**:

```javascript
// Sintaxis: condición ? valorSiVerdadero : valorSiFalso

// Forma larga (if/else)
let mensaje;
if (edad >= 18) {
  mensaje = "Mayor";
} else {
  mensaje = "Menor";
}

// Forma corta (ternario)
let mensaje = edad >= 18 ? "Mayor" : "Menor";
console.log(mensaje); // "Mayor" si edad >= 18, "Menor" si no
```

**Cuándo usar**:
- ✅ Para asignaciones simples
- ✅ Para decisiones rápidas
- ❌ No usar para lógica compleja (mejor if/else)

**Analogía**: Como un atajo. En lugar de dar la vuelta completa (if/else), tomas el camino directo (ternario).

---

## Operadores Lógicos

### AND (&&) - "Y"

**Analogía**: Como dos condiciones que DEBEN cumplirse.

Imagina que quieres entrar a un club:
- **Debes tener** 18 años **Y** debes tener identificación
- Si te falta cualquiera de las dos → No entras

```javascript
// Ejecuta código solo si ambas condiciones son verdaderas
let usuario = { nombre: "Juan", edad: 25 };

if (usuario && usuario.edad >= 18) {
  console.log("Usuario válido");  // Solo si usuario existe Y edad >= 18
}

// También se puede usar para ejecutar código condicionalmente
usuario && console.log(usuario.nombre);  // Solo ejecuta si usuario existe
```

**Tabla de verdad**:
- `true && true` → `true` ✅
- `true && false` → `false` ❌
- `false && true` → `false` ❌
- `false && false` → `false` ❌

**Regla**: **TODAS** las condiciones deben ser verdaderas para que el resultado sea verdadero.

**Analogía**: Como un candado con dos llaves. Necesitas **ambas** llaves para abrirlo.

### OR (||) - "O"

**Analogía**: Como tener opciones.

Imagina que puedes entrar a un evento si:
- Tienes entrada **O** eres invitado especial
- Con cualquiera de las dos opciones → Entras

```javascript
// Usa el primer valor verdadero
let nombre = usuario.nombre || "Invitado";
console.log(nombre); // "Juan" si existe, "Invitado" si no

// Ejemplo práctico
let puerto = process.env.PORT || 3000;  // Usa PORT si existe, si no usa 3000
```

**Tabla de verdad**:
- `true || true` → `true` ✅
- `true || false` → `true` ✅
- `false || true` → `true` ✅
- `false || false` → `false` ❌

**Regla**: Si **AL MENOS UNA** condición es verdadera, el resultado es verdadero.

**Analogía**: Como tener múltiples formas de entrar. Con **cualquiera** de ellas puedes entrar.

### NOT (!) - "NO"

**Analogía**: Como decir lo opuesto.

Imagina que preguntas: "¿Está lloviendo?"
- **NO** está lloviendo → Puedo salir sin paraguas
- Está lloviendo → **NO** puedo salir sin paraguas

```javascript
let estaLloviendo = false;

if (!estaLloviendo) {  // Si NO está lloviendo
  console.log("Puedo salir sin paraguas");
} else {
  console.log("Necesito paraguas");
}
```

**Tabla de verdad**:
- `!true` → `false`
- `!false` → `true`

**Regla**: Invierte el valor. Si es verdadero, lo hace falso. Si es falso, lo hace verdadero.

**Analogía**: Como un interruptor. Si está encendido, lo apagas. Si está apagado, lo enciendes.

---

## Switch: Múltiples Casos

### ¿Qué es Switch?

**Analogía**: Como un menú de restaurante.

Imagina que estás en un restaurante con un menú:
- **Opción 1**: Pizza
- **Opción 2**: Pasta
- **Opción 3**: Ensalada
- **Opción 4**: Postre
- **Otra cosa**: No disponible

**Switch funciona igual**: Compara un valor con múltiples opciones y ejecuta la que coincida.

```javascript
let dia = 3;
let diaSemana;

switch (dia) {
  case 1:
    diaSemana = "Lunes";
    break;  // ⚠️ IMPORTANTE: Sale del switch
  case 2:
    diaSemana = "Martes";
    break;
  case 3:
    diaSemana = "Miércoles";  // Se ejecuta este
    break;
  case 4:
    diaSemana = "Jueves";
    break;
  case 5:
    diaSemana = "Viernes";
    break;
  default:
    diaSemana = "Fin de semana";  // Si no coincide con ningún case
}

console.log(diaSemana); // "Miércoles"
```

### ¿Por qué `break` es Importante?

**Analogía**: Como salir de una habitación.

Imagina que entras a una habitación, haces lo que necesitas, y luego **sales** (break). Sin `break`, seguirías entrando a todas las habitaciones siguientes.

```javascript
// SIN break (PROBLEMA)
switch (dia) {
  case 1:
    console.log("Lunes");
    // Sin break - continúa al siguiente
  case 2:
    console.log("Martes");  // También se ejecuta
    // Sin break - continúa
  case 3:
    console.log("Miércoles");  // También se ejecuta
}

// Si dia = 1, imprime: "Lunes", "Martes", "Miércoles" ❌

// CON break (CORRECTO)
switch (dia) {
  case 1:
    console.log("Lunes");
    break;  // Sale del switch
  case 2:
    console.log("Martes");
    break;
}

// Si dia = 1, imprime solo: "Lunes" ✅
```

### Casos Múltiples

**Analogía**: Como agrupar estaciones del año.

Imagina que agrupas meses por estación:
- Diciembre, Enero, Febrero → Invierno
- Marzo, Abril, Mayo → Primavera
- Etc.

```javascript
let mes = 2;

switch (mes) {
  case 12:
  case 1:
  case 2:
    console.log("Invierno");  // Se ejecuta para 12, 1 o 2
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

**Cuándo usar Switch**:
- ✅ Múltiples condiciones basadas en el mismo valor
- ✅ Valores fijos (no rangos)
- ❌ No usar para rangos (mejor `if/else`)

---

## Truthy y Falsy

### ¿Qué son Truthy y Falsy?

**Analogía**: Como valores que se comportan como verdadero o falso.

Imagina que tienes una caja:
- **Caja vacía** (falsy) → No tiene valor
- **Caja con algo** (truthy) → Tiene valor

**JavaScript evalúa valores como verdaderos o falsos** en contextos booleanos, incluso si no son explícitamente `true` o `false`.

### Valores Falsy (se evalúan como `false`)

**Analogía**: Como cosas que "no existen" o están "vacías".

```javascript
// Valores Falsy
false
0
-0
""          // String vacío
null
undefined
NaN
```

**Ejemplos**:
```javascript
if (0) { }           // No se ejecuta (falsy)
if ("") { }          // No se ejecuta (falsy)
if (null) { }        // No se ejecuta (falsy)
if (undefined) { }   // No se ejecuta (falsy)
```

**Analogía**: Como una caja vacía. No tiene nada, por lo tanto es "falsa" (no tiene valor).

### Valores Truthy (se evalúan como `true`)

**Analogía**: Como cosas que "existen" o tienen "contenido".

```javascript
// Valores Truthy
"hola"      // String no vacío
" "         // String con espacio (aunque sea solo un espacio)
true
1           // Cualquier número != 0
-20
{}          // Objetos (aunque estén vacíos)
[]          // Arrays (aunque estén vacíos)
function(){} // Funciones
Symbol('id')
```

**Ejemplos**:
```javascript
if ("hola") { }      // Se ejecuta (truthy)
if (1) { }           // Se ejecuta (truthy)
if ([]) { }          // Se ejecuta (truthy - array vacío es truthy)
if ({}) { }          // Se ejecuta (truthy - objeto vacío es truthy)
```

**Analogía**: Como una caja con algo dentro. Tiene contenido, por lo tanto es "verdadera" (tiene valor).

### ⚠️ Curiosidad: Arrays y Objetos Vacíos

**Importante**: Aunque un array o objeto esté vacío (`[]` o `{}`), JavaScript los considera **truthy**.

```javascript
if ([]) {
  console.log("Se ejecuta");  // ✅ Se ejecuta (array vacío es truthy)
}

if ({}) {
  console.log("Se ejecuta");  // ✅ Se ejecuta (objeto vacío es truthy)
}
```

**¿Por qué?** Porque el array/objeto **existe** (tiene una referencia en memoria), aunque esté vacío.

**Analogía**: Como una caja vacía pero que existe. La caja existe (truthy), aunque no tenga nada dentro.

---

## Ejemplos Prácticos

### Ejemplo 1: Validación de Usuario

```javascript
let usuario = {
  nombre: "Juan",
  edad: 20,
  email: "juan@example.com"
};

// Validar que el usuario tenga edad >= 18 Y tenga email
if (usuario.edad >= 18 && usuario.email) {
  console.log("Usuario válido");
} else {
  console.log("Usuario inválido");
}
```

**Analogía**: Como verificar que alguien puede entrar a un evento: debe ser mayor de edad **Y** tener entrada.

### Ejemplo 2: Sistema de Descuentos

```javascript
let total = 150;
let descuento = 0;

if (total > 200) {
  descuento = total * 0.2;  // 20% de descuento
} else if (total > 100) {
  descuento = total * 0.1;  // 10% de descuento (se ejecuta este)
} else {
  descuento = 0;
}

let precioFinal = total - descuento;
console.log(`Precio final: $${precioFinal}`);
```

**Analogía**: Como un sistema de descuentos en una tienda:
- Compras más de $200 → 20% de descuento
- Compras más de $100 → 10% de descuento
- Compras menos → Sin descuento

### Ejemplo 3: Día de la Semana con Switch

```javascript
let dia = new Date().getDay();  // 0 = Domingo, 6 = Sábado
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

**Analogía**: Como un calendario que te dice qué hacer según el día.

### Ejemplo 4: Operador Ternario Anidado

```javascript
let edad = 20;
let tipo = edad < 13 ? "Niño" : 
           edad < 18 ? "Adolescente" : 
           edad < 65 ? "Adulto" : "Adulto mayor";

console.log(tipo); // "Adulto"
```

**Analogía**: Como una escalera de decisiones rápidas. Subes escalón por escalón hasta encontrar tu categoría.

---

## Conceptos Clave

1. **if/else**: Estructura básica de decisión
2. **else if**: Múltiples condiciones en orden
3. **Operador Ternario**: Forma corta de if/else para asignaciones
4. **Switch**: Múltiples casos basados en un valor fijo
5. **Truthy/Falsy**: Valores que se evalúan como booleanos
6. **Operadores Lógicos**: `&&` (AND), `||` (OR), `!` (NOT)
7. **Break**: Sale del switch (importante para evitar fall-through)

---

## Buenas Prácticas

- ✅ Usa `switch` cuando tengas múltiples casos basados en el mismo valor
- ✅ Siempre incluye `break` en cada caso de `switch` (a menos que quieras fall-through)
- ✅ Usa operador ternario para asignaciones simples
- ✅ Valida datos antes de usarlos en condicionales
- ✅ Usa `&&` y `||` para valores por defecto
- ❌ Evita anidar demasiados `if/else` (considera funciones o switch)
- ✅ Usa nombres descriptivos para variables en condiciones

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [JavaScript: Variables](./10-JS-Variables.md) - Variables usadas en condicionales
- 📚 [JavaScript: Funciones](./13-JS-Funciones.md) - Funciones con condicionales
- 📚 [JavaScript: Arrays](./12-JS-Arrays.md) - Condicionales con arrays

### Código Relacionado

- 💻 [Tema 11: Condicionales](../../CODIGO/frontend/tema-11-javascript-condicionales-operadores-arreglos/)

---

## 🎯 Puntos Clave para Recordar

1. **Condicionales = Decisiones**: Como las decisiones que tomas en la vida diaria
2. **if/else = Sí/No**: La estructura más básica de decisión
3. **Switch = Menú**: Para múltiples opciones fijas
4. **Truthy/Falsy = Caja vacía/llena**: Valores que se comportan como booleanos
5. **Operadores Lógicos = Reglas**: Combinan condiciones
6. **Break = Salir**: Importante en switch para no ejecutar todos los casos

---

## 💡 Ejercicio Mental

Piensa en situaciones de la vida real como condicionales:

- **Si** está lloviendo → Llevo paraguas
- **Si** tengo hambre **Y** tengo dinero → Como algo
- **Si** es fin de semana **O** es feriado → Descanso
- **Si NO** tengo tarea → Salgo a jugar

¡Practica identificando condiciones en tu vida diaria!
