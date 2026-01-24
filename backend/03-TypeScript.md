# TypeScript: Introducción 🎯

## 📑 Índice

1. [¿Qué es TypeScript? (Analogía del Mundo Real)](#qué-es-typescript-analogía-del-mundo-real)
2. [Tipos Básicos](#tipos-básicos)
3. [Arrays y Objetos](#arrays-y-objetos)
4. [Funciones Tipadas](#funciones-tipadas)
5. [Clases](#clases)
6. [Interfaces](#interfaces)
7. [Conceptos Clave](#conceptos-clave)
8. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es TypeScript? (Analogía del Mundo Real)

### 🛡️ Analogía: El Traje de Seguridad

Imagina que JavaScript es como trabajar sin protección:
- **JavaScript**: Puedes hacer cualquier cosa, pero si cometes un error, te das cuenta cuando ya está funcionando (en tiempo de ejecución)
- **TypeScript**: Es como usar un traje de seguridad - te avisa ANTES de que algo salga mal (en tiempo de desarrollo)

**TypeScript es JavaScript con "seguridad de tipos"** - te ayuda a evitar errores antes de que ocurran.

### 📝 Analogía: El Contrato de Trabajo

Piensa en un contrato de trabajo:
- **JavaScript**: Como trabajar sin contrato - puedes hacer lo que quieras, pero puede haber malentendidos
- **TypeScript**: Como tener un contrato escrito - define claramente qué se espera de cada parte

**TypeScript define "contratos"** (tipos) que dicen qué tipo de datos puede usar cada función.

### 🏗️ Analogía: El Plano Arquitectónico

Imagina construir una casa:
- **JavaScript**: Como construir sin plano - puedes hacerlo, pero puede haber errores
- **TypeScript**: Como tener un plano detallado - define la estructura antes de construir

**TypeScript es como un "plano"** que define la estructura de tu código antes de ejecutarlo.

### ¿Qué es TypeScript en Programación?

TypeScript es un **superset de JavaScript** que añade **tipado estático opcional**. Esto significa que puedes definir qué tipo de datos puede tener cada variable, función, etc.

**En términos simples**: Es JavaScript con "etiquetas" que dicen qué tipo de dato es cada cosa, ayudando a evitar errores.

**Características**:
- Todo código JS válido es TS válido
- Tipado estático opcional
- Transpilación a JavaScript (se convierte a JS para ejecutarse)

---

## Tipos Básicos

### 🏷️ Analogía: Las Etiquetas en las Cajas

Imagina que tienes cajas con etiquetas que dicen qué hay dentro:
- **Caja con etiqueta "Texto"** (`string`): Solo puedes guardar texto
- **Caja con etiqueta "Número"** (`number`): Solo puedes guardar números
- **Caja con etiqueta "Sí/No"** (`boolean`): Solo puedes guardar true o false

**TypeScript es como esas etiquetas** - te dice qué tipo de dato puede ir en cada "caja" (variable).

```typescript
let nombre: string = "Juan"      // ✅ Correcto: "Juan" es texto
let edad: number = 25            // ✅ Correcto: 25 es número
let activo: boolean = true        // ✅ Correcto: true es booleano
let datos: any = "cualquier cosa" // ⚠️ any acepta cualquier cosa (usar con cuidado)

// nombre = 25  // ❌ Error: nombre es string, no number
// edad = "veinticinco"  // ❌ Error: edad es number, no string
```

**Analogía**: Como tener cajas etiquetadas. Si intentas poner un número en una caja etiquetada "Texto", TypeScript te avisa que está mal.

---

## Arrays y Objetos

### 📋 Analogía: La Lista Tipada

Imagina una lista de compras con reglas:
- **Lista de números** (`number[]`): Solo puedes agregar números
- **Lista de nombres** (`string[]`): Solo puedes agregar texto

**TypeScript define qué tipo de elementos puede tener cada lista**.

```typescript
// Arrays - Listas tipadas
let numeros: number[] = [1, 2, 3]           // ✅ Solo números
let nombres: string[] = ["Juan", "María"]    // ✅ Solo texto
// numeros.push("texto")  // ❌ Error: no puedes agregar texto a una lista de números
```

### 📝 Analogía: El Formulario con Campos Obligatorios

Piensa en un formulario:
- **Campos obligatorios**: Debes llenarlos (sin `?`)
- **Campos opcionales**: Puedes dejarlos vacíos (con `?`)

**Interfaces en TypeScript funcionan igual** - definen qué campos son obligatorios y cuáles opcionales.

```typescript
// Objetos - Con contrato (interface)
interface Usuario {
    nombre: string      // Obligatorio
    edad: number        // Obligatorio
    email?: string      // Opcional (el ? lo hace opcional)
}

const usuario: Usuario = {
    nombre: "Juan",
    edad: 25
    // email es opcional, no es necesario incluirlo
}

// const usuario2: Usuario = {
//     nombre: "Juan"
//     // ❌ Error: falta edad (es obligatorio)
// }
```

**Analogía**: Como un formulario donde algunos campos son obligatorios y otros opcionales.

---

## Funciones Tipadas

### 🎯 Analogía: La Máquina con Especificaciones

Imagina una máquina de café:
- **Entrada** (parámetros): Define qué tipo de ingredientes acepta (café, agua)
- **Proceso** (código): Lo que hace la máquina
- **Salida** (return): Define qué tipo de resultado produce (café)

**TypeScript define el "tipo" de entrada y salida** de cada función.

```typescript
// Función que acepta dos números y retorna un número
function sumar(a: number, b: number): number {
    return a + b
}

// sumar(5, 3)        // ✅ Correcto: retorna 8 (number)
// sumar("5", "3")    // ❌ Error: espera numbers, no strings
// sumar(5)           // ❌ Error: falta el segundo parámetro

// Parámetros opcionales - Como ingredientes opcionales en una receta
function saludar(nombre: string, edad?: number): void {
    console.log(`Hola, ${nombre}`)
    // edad es opcional, puedes llamar la función sin él
}

saludar("Juan")           // ✅ Funciona (edad es opcional)
saludar("Juan", 25)       // ✅ También funciona
```

**Analogía**: Como una máquina que tiene especificaciones claras de qué acepta y qué produce.

---

## Clases

### 🏭 Analogía: La Fábrica con Especificaciones

Imagina una fábrica que hace autos:
- **Plano** (clase): Define cómo se construye un auto
- **Especificaciones** (tipos): Define qué tipo de materiales usa (motor: string, ruedas: number)
- **Auto construido** (instancia): Un auto real hecho según el plano

**TypeScript añade tipos a las clases** para definir qué tipo de datos puede tener cada propiedad.

```typescript
class Persona {
    nombre: string      // Propiedad de tipo string
    edad: number       // Propiedad de tipo number
    
    constructor(nombre: string, edad: number) {
        this.nombre = nombre
        this.edad = edad
    }
    
    saludar(): void {  // Método que no retorna nada (void)
        console.log(`Hola, soy ${this.nombre}`)
    }
}

const juan = new Persona("Juan", 25)  // ✅ Correcto
// const pedro = new Persona(25, "Pedro")  // ❌ Error: tipos en orden incorrecto
```

**Analogía**: Como un plano de construcción con especificaciones claras de qué materiales usar.

---

## Interfaces

### 📋 Analogía: El Contrato de Trabajo

Imagina un contrato de trabajo:
- **Contrato** (interface): Define qué se espera (nombre, edad, habilidades)
- **Empleado** (clase): Debe cumplir el contrato

**Las interfaces definen "contratos"** que las clases deben cumplir.

```typescript
// Interface - Contrato que define qué debe tener un vehículo
interface Vehiculo {
    marca: string
    modelo: string
    acelerar(): void  // Debe tener un método acelerar
}

// Clase que cumple el contrato
class Auto implements Vehiculo {
    marca: string
    modelo: string
    
    // DEBE tener el método acelerar (está en el contrato)
    acelerar(): void {
        console.log("Acelerando...")
    }
}

// class Moto implements Vehiculo {
//     marca: string
//     // ❌ Error: falta modelo y acelerar() - no cumple el contrato
// }
```

**Analogía**: Como un contrato que dice "si eres un vehículo, DEBES tener marca, modelo y poder acelerar".

---

## Conceptos Clave

1. **Tipos**: Definir tipos de variables
   - `string`, `number`, `boolean`, etc.
   - Ayuda a evitar errores

2. **Interfaces**: Contratos para objetos
   - Define qué propiedades debe tener un objeto
   - Las clases deben "cumplir" el contrato

3. **Clases**: POO con TypeScript
   - Clases con tipos definidos
   - Propiedades y métodos tipados

4. **Tipado Opcional**: `?` para propiedades opcionales
   - `email?: string` significa que email es opcional

5. **Transpilación**: TS se compila a JS
   - TypeScript se convierte a JavaScript para ejecutarse
   - Los navegadores ejecutan JavaScript, no TypeScript

6. **Type Inference**: TypeScript infiere tipos automáticamente
   - Si escribes `let x = 5`, TypeScript sabe que `x` es `number`
   - No siempre necesitas escribir el tipo explícitamente

7. **Any**: Tipo que acepta cualquier valor (usar con cuidado)
   - `let datos: any = "cualquier cosa"`
   - Pierde los beneficios de TypeScript

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [JavaScript: Nivelación](./01-JS-Nivelacion.md) - Base de JavaScript antes de TypeScript
- 📚 [POO: Encapsulación](./04-POO-Encapsulacion.md) - Usa TypeScript para POO
- 📚 [POO: Herencia](./05-POO-Herencia.md) - Herencia con TypeScript
- 📚 [POO: Polimorfismo](./06-POO-Polimorfismo.md) - Interfaces y polimorfismo

### Código Relacionado

- 💻 [Ejemplos de TypeScript](../../CODIGO/backend/tema-02-typescript/)

---

## 🎯 Puntos Clave para Recordar

1. **TypeScript = JavaScript + Tipos**: JavaScript con etiquetas de tipo
2. **Tipos = Etiquetas**: Definen qué tipo de dato puede ir en cada variable
3. **Interfaces = Contratos**: Definen qué debe tener un objeto
4. **Transpilación**: TypeScript se convierte a JavaScript para ejecutarse
5. **Type Inference**: TypeScript puede adivinar tipos automáticamente

---

## 💡 Ejercicio Mental

Piensa en situaciones de la vida real como tipos:
- **Nombre**: Siempre es texto (`string`)
- **Edad**: Siempre es número (`number`)
- **Está activo**: Siempre es sí/no (`boolean`)
- **Email**: Puede ser texto o no existir (`string | undefined`)

¡Practica identificando qué tipo de dato es cada cosa en tu vida diaria!
