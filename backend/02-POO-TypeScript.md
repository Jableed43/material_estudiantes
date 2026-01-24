# TypeScript y Programación Orientada a Objetos (POO): Guía Maestra 🚀

## 📋 Índice

### Parte 1: TypeScript
1. [Introducción a TypeScript](#1-introducción-a-typescript)
2. [Instalación y Configuración](#2-instalación-y-configuración)
3. [Módulos (Import y Export)](#25-módulos-import-y-export)
4. [Tipos Básicos](#3-tipos-básicos)
   - [Inferencia de Tipos](#inferencia-de-tipos)
   - [Optional Chaining](#optional-chaining-)
   - [Nullish Coalescing](#nullish-coalescing-)
   - [Destructuring con Tipos](#destructuring-con-tipos)
5. [Tipos Avanzados](#4-tipos-avanzados)
6. [Interfaces y Types](#5-interfaces-y-types)
   - [Propiedades Opcionales](#propiedades-opcionales)
   - [Métodos Opcionales](#métodos-opcionales)
7. [Clases en TypeScript](#6-clases-en-typescript)
   - [Modificadores de Acceso](#modificadores-de-acceso)
   - [Getters y Setters](#getters-y-setters)
8. [Funciones Tipadas](#7-funciones-tipadas)
   - [Parámetros Opcionales](#parámetros-opcionales)
   - [Parámetros por Defecto](#parámetros-por-defecto)
9. [Generics (Genéricos)](#75-generics-genéricos)
10. [Type Assertions y Type Guards](#76-type-assertions-y-type-guards)

### Parte 2: Programación Orientada a Objetos (POO)
8. [Introducción a POO](#8-introducción-a-poo)
9. [Encapsulación](#9-encapsulación)
10. [Herencia](#10-herencia)
11. [Polimorfismo](#11-polimorfismo)
12. [Composición](#12-composición)
13. [Proyecto Integrador: Concesionario](#13-proyecto-integrador-concesionario)

### Parte 3: Ejemplos Prácticos
14. [Ejemplos del Código Modelo](#14-ejemplos-del-código-modelo)

---

## Parte 1: TypeScript

## 1. Introducción a TypeScript

### ¿Qué es TypeScript?

**TypeScript** es un **superset de JavaScript** desarrollado por Microsoft que añade **tipado estático opcional**. No es un lenguaje nuevo, sino una capa adicional sobre JavaScript.

### Características Principales:

- ✅ **Superset**: Todo código JS válido es código TS válido. Puedes migrar de `.js` a `.ts` gradualmente.
- ✅ **Tipado Estático Opcional**: Permite definir tipos de datos en tiempo de desarrollo. TS puede **inferir** muchos tipos automáticamente.
- ✅ **Transpilación**: El compilador de TS (`tsc`) transforma el código `.ts` en `.js` compatible con navegadores o Node.js.

### ¿Por qué usar TypeScript?

| Aspecto | JavaScript (JS) | TypeScript (TS) |
|---------|----------------|-----------------|
| **Tipado** | Dinámico (verificado en ejecución) | Estático (verificado en compilación) |
| **Errores** | Detectados en tiempo de ejecución | Detectados antes de ejecutar |
| **Escalabilidad** | Difícil de mantener en proyectos grandes | Ideal para equipos grandes y refactorizaciones |
| **Ejecución** | Directa en navegadores | Requiere transpilación a JS (`.ts` -> `.js`) |
| **Autocompletado** | Básico | Inteligente y preciso |
| **Documentación** | Manual | Los tipos actúan como documentación viva |

### Beneficios:

1. **Detección temprana de errores**: Captura errores de tipo antes de ejecutar el código.
2. **Mantenibilidad**: Los tipos sirven como documentación viva.
3. **DX (Developer Experience)**: Autocompletado inteligente y refactorización segura en el IDE.
4. **Interoperabilidad**: Completamente compatible con JavaScript, permite adopción gradual.

---

## 2. Instalación y Configuración

### Paso 1: Instalar TypeScript Globalmente

```bash
npm install -g typescript
```

**Verificar instalación:**
```bash
tsc --version
```

Deberías ver algo como: `Version 5.x.x`

### Paso 2: Configurar PowerShell (Solo Windows)

Si estás usando PowerShell en Windows y tienes problemas con la ejecución de scripts:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**¿Qué hace esto?**
- Permite ejecutar scripts npm de forma segura
- Solo afecta a tu usuario actual (no requiere permisos de administrador)
- Es necesario para que npm pueda ejecutar scripts de paquetes instalados

### Paso 3: Instalar ts-node (Opcional pero Recomendado)

`ts-node` permite ejecutar archivos TypeScript directamente sin compilarlos primero.

**Instalación local (Recomendado para proyectos):**
```bash
npm init -y
npm install ts-node --save-dev
npm install @types/node --save-dev
```

**Instalación global (Alternativa):**
```bash
npm install -g ts-node
```

**¿Para qué sirve ts-node?**
- Ejecuta archivos `.ts` directamente: `ts-node archivo.ts`
- No necesitas compilar manualmente con `tsc`
- Ideal para desarrollo y aprendizaje

**Uso:**
```bash
# Con ts-node instalado localmente
npx ts-node index.ts

# Con ts-node instalado globalmente
ts-node index.ts
```

### Paso 4: Configurar TypeScript (tsconfig.json)

**Crear automáticamente:**
```bash
tsc --init
```

**O crear manualmente** un archivo `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "lib": ["ES2022", "DOM"],
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "isolatedModules": true,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["**/*.ts"],
  "exclude": ["node_modules"]
}
```

**Configuraciones importantes:**
- `target`: Versión de JavaScript a generar
- `module`: Sistema de módulos a usar
- `strict`: Activa verificaciones estrictas de tipos
- `outDir`: Carpeta donde se guarda el JavaScript compilado
- `rootDir`: Carpeta donde está el código TypeScript fuente

### Comandos Útiles:

| Comando | Descripción |
|---------|-------------|
| `npm install -g typescript` | Instala TypeScript globalmente |
| `tsc --version` | Verifica la versión instalada |
| `tsc archivo.ts` | Compila TypeScript a JavaScript |
| `ts-node archivo.ts` | Ejecuta TypeScript directamente |
| `tsc --init` | Crea un tsconfig.json básico |
| `tsc --watch` | Compila automáticamente al guardar cambios |
| `tsc` | Compila todo el proyecto según tsconfig.json |

---

## 2.5. Módulos (Import y Export)

### ¿Qué son los Módulos?

**Definición**: Los **módulos** permiten dividir el código en archivos separados y reutilizables. TypeScript soporta el sistema de módulos de ES6 (`import`/`export`) para organizar y compartir código entre archivos.

### Export (Exportar)

**Exportar valores, funciones, clases, interfaces y tipos:**

```typescript
// archivo: utils.ts

// Exportar función (named export)
export function sumar(a: number, b: number): number {
  return a + b
}

// Exportar clase
export class Usuario {
  constructor(public nombre: string) {}
}

// Exportar interface
export interface Persona {
  nombre: string
  edad: number
}

// Exportar type
export type ID = string | number

// Exportar constante
export const PI = 3.14159

// Exportar por defecto (default export)
export default function saludar(nombre: string): void {
  console.log(`Hola, ${nombre}`)
}
```

**Exportar múltiples elementos:**

```typescript
// archivo: tipos.ts

// Exportar múltiples elementos
export interface Animal {
  nombre: string
}

export interface Perro extends Animal {
  raza: string
}

export type Color = "rojo" | "verde" | "azul"
```

**Exportar todo desde un archivo (re-export):**

```typescript
// archivo: index.ts

// Re-exportar todo desde otros archivos
export * from "./utils"
export * from "./tipos"

// O exportar específicos
export { sumar, Usuario } from "./utils"
export type { Persona } from "./tipos"
```

### Import (Importar)

**Importar elementos exportados:**

```typescript
// archivo: main.ts

// Importar named exports
import { sumar, Usuario, PI } from "./utils"

// Importar con alias
import { sumar as sumarNumeros, Usuario as User } from "./utils"

// Importar default export
import saludar from "./utils"

// Importar todo desde un módulo
import * as utils from "./utils"
utils.sumar(1, 2)
utils.PI

// Importar tipos e interfaces
import type { Persona, ID } from "./tipos"

// Importar tipo con alias
import type { Persona as Person } from "./tipos"
```

**Import Type (Solo para Tipos)**

**Definición**: `import type` se usa para importar **únicamente tipos, interfaces y tipos alias**. Estos imports se eliminan completamente en el JavaScript compilado, ya que los tipos no existen en tiempo de ejecución.

**Sintaxis:**
```typescript
// Importar solo tipos (se elimina en compilación)
import type { Persona, ID, Color } from "./tipos"

// Importar tipo con alias
import type { Persona as Person } from "./tipos"

// Importar tipo por defecto
import type Persona from "./tipos"
```

**¿Cuándo usar `import type`?**

- ✅ Cuando solo necesitas tipos, interfaces o types
- ✅ Para reducir el tamaño del bundle (se elimina en compilación)
- ✅ Para hacer explícito que solo estás importando tipos
- ✅ Cuando quieres evitar importaciones circulares de tipos

**Comparación: `import` vs `import type`**

```typescript
// archivo: utils.ts
export function sumar(a: number, b: number): number {
  return a + b
}

export interface Persona {
  nombre: string
}

// archivo: main.ts

// import: Importa valores (funciones, clases, etc.)
import { sumar } from "./utils"  // ✅ Funciona en runtime
sumar(1, 2)  // ✅ Funciona

// import type: Solo importa tipos (se elimina en compilación)
import type { Persona } from "./utils"  // ✅ Solo para tipos
let persona: Persona = { nombre: "Juan" }  // ✅ Funciona

// ❌ Error: No puedes usar import type para valores
// import type { sumar } from "./utils"  // ❌ Error: sumar no es un tipo
// sumar(1, 2)  // ❌ No existe en runtime
```

**Ejemplos completos:**

```typescript
// archivo: tipos.ts
export interface Usuario {
  id: number
  nombre: string
  email?: string
}

export type Estado = "activo" | "inactivo"

export class Persona {
  constructor(public nombre: string) {}
}

// archivo: funciones.ts
export function crearUsuario(nombre: string): Usuario {
  return {
    id: 1,
    nombre
  }
}

// archivo: main.ts
// Importar tipos con import type
import type { Usuario, Estado } from "./tipos"

// Importar valores con import normal
import { Persona } from "./tipos"
import { crearUsuario } from "./funciones"

// Usar tipos
let usuario: Usuario = {
  id: 1,
  nombre: "Juan"
}

let estado: Estado = "activo"

// Usar valores
const persona = new Persona("María")
const nuevoUsuario = crearUsuario("Pedro")
```

**Importar tipos y valores juntos:**

```typescript
// Opción 1: Separar imports
import { crearUsuario } from "./funciones"
import type { Usuario } from "./tipos"

// Opción 2: Usar type en el mismo import
import { crearUsuario, type Usuario } from "./funciones"
```

**Características de los Módulos:**

- ✅ Permiten organizar código en múltiples archivos
- ✅ Evitan conflictos de nombres con namespaces
- ✅ Facilitan la reutilización de código
- ✅ Los tipos se eliminan en compilación (no existen en runtime)
- ✅ `import type` es solo para tipos (se elimina completamente)

**Estructura de Proyecto con Módulos:**

```
src/
├── tipos/
│   ├── usuario.ts      # export interface Usuario
│   └── index.ts        # export * from "./usuario"
├── clases/
│   ├── Persona.ts      # export class Persona
│   └── index.ts        # export * from "./Persona"
├── utils/
│   └── helpers.ts      # export function sumar
└── main.ts             # import { ... } from "./tipos"
```

---

## 3. Tipos Básicos

### Inferencia de Tipos

**Definición**: La **inferencia de tipos** es la capacidad de TypeScript de determinar automáticamente el tipo de una variable basándose en su valor inicial, sin necesidad de especificarlo explícitamente.

**Ejemplos:**

```typescript
// TypeScript infiere que 'edad' es number
let edad = 25  // Es lo mismo que: let edad: number = 25

// TypeScript infiere que 'nombre' es string
let nombre = "Juan"  // Es lo mismo que: let nombre: string = "Juan"

// TypeScript infiere que 'activo' es boolean
let activo = true  // Es lo mismo que: let activo: boolean = true

// TypeScript infiere el tipo del array
let numeros = [1, 2, 3]  // TypeScript infiere: number[]

// TypeScript infiere el tipo del objeto
let persona = {
  nombre: "Juan",
  edad: 25
}  // TypeScript infiere: { nombre: string, edad: number }
```

**Cuándo TypeScript infiere automáticamente:**
- ✅ Cuando inicializas una variable con un valor
- ✅ En el retorno de funciones (si no especificas el tipo de retorno)
- ✅ En parámetros de funciones (si tienen valores por defecto)

**Cuándo es mejor especificar el tipo explícitamente:**
- ✅ Cuando la variable se declara sin inicializar
- ✅ Cuando quieres ser más explícito para claridad
- ✅ Cuando el tipo inferido no es el que necesitas
- ✅ En funciones públicas para documentación

**Ejemplos de cuándo especificar explícitamente:**

```typescript
// Sin inicializar - DEBES especificar el tipo
let nombre: string
nombre = "Juan"  // ✅ Funciona
// nombre = 25    // ❌ Error

// Función - especificar tipos para claridad
function sumar(a: number, b: number): number {
  return a + b
}

// Array vacío - especificar tipo
let numeros: number[] = []
numeros.push(1)  // ✅ Funciona
// numeros.push("uno")  // ❌ Error
```

**Ventajas de la inferencia:**
- ✅ Menos código que escribir
- ✅ TypeScript mantiene la seguridad de tipos
- ✅ Código más limpio y legible

**Desventajas:**
- ❌ Puede ser menos explícito
- ❌ A veces el tipo inferido no es el esperado

### Primitivos

TypeScript extiende los tipos primitivos de JavaScript:

```typescript
let nombre: string = "Ana"
let edad: number = 30
let esAdmin: boolean = true
let cualquiera: any = "Peligroso"  // Evitar 'any' siempre que sea posible
let nada: void = undefined  // Para funciones que no retornan nada
let nulo: null = null
let indefinido: undefined = undefined
```

### Arrays

```typescript
// Array de números
let notas: number[] = [10, 8, 9]

// Array de strings (sintaxis alternativa)
let tags: Array<string> = ["React", "Node"]

// Array mixto (unión de tipos)
let mixto: (string | number)[] = ["uno", 2, 3]
```

### Destructuring con Tipos

**Definición**: El **destructuring** permite extraer valores de arrays u objetos de forma concisa. En TypeScript, puedes tipar las variables durante el destructuring.

**Destructuring de Objetos:**

```typescript
interface Persona {
  nombre: string
  edad: number
  email?: string
}

const persona: Persona = {
  nombre: "Juan",
  edad: 25,
  email: "juan@example.com"
}

// Destructuring básico
const { nombre, edad } = persona
console.log(nombre)  // "Juan"
console.log(edad)    // 25

// Destructuring con renombrado
const { nombre: nombreCompleto, edad: años } = persona
console.log(nombreCompleto)  // "Juan"
console.log(años)            // 25

// Destructuring con valores por defecto
const { nombre, edad, email = "sin-email" } = persona
console.log(email)  // "juan@example.com" o "sin-email" si no existe

// Destructuring con tipos explícitos
const { nombre, edad }: { nombre: string, edad: number } = persona

// Destructuring en parámetros de función
function mostrarPersona({ nombre, edad }: Persona): void {
  console.log(`${nombre} tiene ${edad} años`)
}

mostrarPersona(persona)  // "Juan tiene 25 años"
```

**Destructuring de Arrays:**

```typescript
// Destructuring básico
const numeros: number[] = [1, 2, 3]
const [primero, segundo, tercero] = numeros
console.log(primero)   // 1
console.log(segundo)   // 2
console.log(tercero)   // 3

// Destructuring con tipos
const [a, b, c]: [number, number, number] = [1, 2, 3]

// Omitir elementos
const [primero2, , tercero2] = numeros
console.log(primero2)  // 1
console.log(tercero2)   // 3

// Rest en destructuring
const [primero3, ...resto] = numeros
console.log(primero3)  // 1
console.log(resto)     // [2, 3]

// Valores por defecto
const [x = 0, y = 0, z = 0] = [1, 2]
console.log(x)  // 1
console.log(y)  // 2
console.log(z)  // 0 (valor por defecto)
```

**Destructuring en Clases:**

```typescript
class Usuario {
  constructor(
    public nombre: string,
    public edad: number,
    public email?: string
  ) {}
  
  // Método que usa destructuring
  actualizar({ nombre, edad }: { nombre: string, edad: number }): void {
    this.nombre = nombre
    this.edad = edad
  }
}

const usuario = new Usuario("Juan", 25)
usuario.actualizar({ nombre: "María", edad: 30 })
```

**Destructuring Anidado:**

```typescript
interface Direccion {
  calle: string
  ciudad: string
  codigoPostal: number
}

interface Persona {
  nombre: string
  direccion: Direccion
}

const persona: Persona = {
  nombre: "Juan",
  direccion: {
    calle: "Av. Principal",
    ciudad: "Buenos Aires",
    codigoPostal: 1234
  }
}

// Destructuring anidado
const { nombre, direccion: { ciudad, calle } } = persona
console.log(nombre)  // "Juan"
console.log(ciudad)  // "Buenos Aires"
console.log(calle)   // "Av. Principal"
```

**Características:**
- ✅ Permite extraer valores de forma concisa
- ✅ Puedes tipar las variables durante el destructuring
- ✅ Útil en parámetros de función
- ✅ Permite renombrar variables
- ✅ Permite valores por defecto

### Tuplas

Arrays de longitud fija con tipos específicos por posición:

```typescript
// Tupla: [string, number]
let usuario: [string, number] = ["Juan", 25]

// Tupla con más elementos
let coordenadas: [number, number, number] = [10, 20, 30]

// Tupla con tipos opcionales
let opcional: [string, number?] = ["Juan"]  // El número es opcional
```

### Enums

Conjunto de constantes con nombres para hacer el código más legible:

```typescript
// Enum numérico (por defecto)
enum Rol {
  Admin,  // 0
  User,   // 1
  Guest   // 2
}

let miRol: Rol = Rol.Admin

// Enum string
enum Estado {
  Activo = "ACT",
  Inactivo = "INA",
  Pendiente = "PEND"
}

let miEstado: Estado = Estado.Activo
```

### Tipos Especiales

```typescript
// any: Desactiva la comprobación de tipos (usar con precaución)
let cualquiera: any = "puede ser cualquier cosa"
cualquiera = 42  // ✅ Funciona (pero no recomendado)

// unknown: Similar a any, pero más seguro (requiere verificación)
let desconocido: unknown = "algo"
// desconocido.toUpperCase()  // ❌ Error: necesita verificación
if (typeof desconocido === "string") {
  desconocido.toUpperCase()  // ✅ Funciona después de verificar
}

// void: Para funciones que no retornan nada
function logMensaje(msg: string): void {
  console.log(msg)
}

// never: Para funciones que nunca retornan (lanzan error o loop infinito)
function error(): never {
  throw new Error("Error fatal")
}
```

### Optional Chaining (`?.`)

**Definición**: El **optional chaining** (`?.`) permite acceder de forma segura a propiedades anidadas de un objeto, evitando errores si alguna propiedad es `null` o `undefined`.

**Sintaxis:**
```typescript
// Acceso seguro a propiedades anidadas
let usuario = {
  nombre: "Juan",
  direccion: {
    calle: "Av. Principal",
    ciudad: "Buenos Aires"
  }
}

// Sin optional chaining (puede fallar)
let ciudad = usuario.direccion.ciudad  // ✅ Funciona
// let ciudad2 = usuario.direccion?.ciudad  // Si direccion es undefined, retorna undefined

// Con optional chaining (seguro)
let usuario2: { nombre: string, direccion?: { calle: string, ciudad: string } } = {
  nombre: "María"
  // direccion puede ser undefined
}

let ciudad3 = usuario2.direccion?.ciudad  // ✅ Retorna undefined si direccion es undefined
// let ciudad4 = usuario2.direccion.ciudad  // ❌ Error: direccion puede ser undefined
```

**Características:**
- ✅ Retorna `undefined` si alguna propiedad en la cadena es `null` o `undefined`
- ✅ Evita errores de "Cannot read property of undefined"
- ✅ Útil con propiedades opcionales
- ✅ Se puede usar con métodos también

**Ejemplos:**

```typescript
// Con propiedades opcionales
interface Usuario {
  nombre: string
  direccion?: {
    calle: string
    ciudad?: string
  }
}

let usuario: Usuario = {
  nombre: "Juan"
  // direccion es opcional
}

// Acceso seguro
let ciudad = usuario.direccion?.ciudad  // undefined (direccion no existe)
let calle = usuario.direccion?.calle    // undefined (direccion no existe)

// Con métodos opcionales
interface Calculadora {
  sumar?(a: number, b: number): number
}

let calc: Calculadora = {}

let resultado = calc.sumar?.(5, 3)  // undefined (sumar no existe)
// let resultado2 = calc.sumar(5, 3)  // ❌ Error: sumar puede ser undefined
```

### Nullish Coalescing (`??`)

**Definición**: El **nullish coalescing** (`??`) es un operador que retorna el valor del lado derecho solo si el valor del lado izquierdo es `null` o `undefined`. A diferencia de `||`, no considera otros valores falsy como `0`, `""`, o `false`.

**Sintaxis:**
```typescript
// Nullish coalescing
let valor = valorIzquierdo ?? valorPorDefecto
```

**Comparación: `??` vs `||`**

```typescript
// Con ||
let nombre = "" || "Sin nombre"        // "Sin nombre" ("" es falsy)
let edad = 0 || 18                     // 18 (0 es falsy)
let activo = false || true              // true (false es falsy)

// Con ??
let nombre2 = "" ?? "Sin nombre"       // "" ("" no es null ni undefined)
let edad2 = 0 ?? 18                     // 0 (0 no es null ni undefined)
let activo2 = false ?? true             // false (false no es null ni undefined)

// Solo con null o undefined
let nombre3 = null ?? "Sin nombre"      // "Sin nombre"
let nombre4 = undefined ?? "Sin nombre" // "Sin nombre"
```

**Ejemplos prácticos:**

```typescript
// Valores por defecto
function saludar(nombre?: string): void {
  const nombreFinal = nombre ?? "Usuario"
  console.log(`Hola, ${nombreFinal}`)
}

saludar()           // "Hola, Usuario"
saludar("Juan")     // "Hola, Juan"
saludar("")         // "Hola, " (no usa el valor por defecto)

// Con propiedades opcionales
interface Configuracion {
  puerto?: number
  host?: string
  debug?: boolean
}

function crearServidor(config: Configuracion) {
  const puerto = config.puerto ?? 3000      // 3000 si puerto es undefined
  const host = config.host ?? "localhost"   // "localhost" si host es undefined
  const debug = config.debug ?? false        // false si debug es undefined
  
  console.log(`Servidor en ${host}:${puerto}, debug: ${debug}`)
}

crearServidor({})                    // Servidor en localhost:3000, debug: false
crearServidor({ puerto: 0 })         // Servidor en localhost:0, debug: false (0 no es null/undefined)
crearServidor({ puerto: 8080 })      // Servidor en localhost:8080, debug: false
```

**Cuándo usar `??` vs `||`:**

| Situación | Usar `??` | Usar `||` |
|-----------|-----------|-----------|
| **Valores por defecto** | Cuando quieres preservar `0`, `""`, `false` | Cuando quieres reemplazar cualquier valor falsy |
| **Configuraciones** | Para valores numéricos que pueden ser 0 | Para valores que nunca deberían ser falsy |
| **Props opcionales** | Cuando la prop puede ser `null` o `undefined` | Cuando quieres un valor por defecto para cualquier falsy |

**Combinando con Optional Chaining:**

```typescript
interface Usuario {
  nombre: string
  email?: string
  direccion?: {
    ciudad?: string
  }
}

let usuario: Usuario = {
  nombre: "Juan"
}

// Combinar ?. con ??
let ciudad = usuario.direccion?.ciudad ?? "Sin ciudad"
let email = usuario.email ?? "Sin email"

console.log(ciudad)  // "Sin ciudad"
console.log(email)   // "Sin email"
```

---

## 4. Tipos Avanzados

### Uniones de Tipos (Union Types)

Permite que una variable tenga uno de varios tipos:

```typescript
let id: string | number
id = "abc123"  // ✅ Válido
id = 123       // ✅ Válido
// id = true   // ❌ Error: boolean no está permitido
```

### Intersección de Tipos (Intersection Types)

Combina múltiples tipos en uno:

```typescript
type Nombre = { nombre: string }
type Edad = { edad: number }
type Persona = Nombre & Edad  // Tiene nombre Y edad

let persona: Persona = {
  nombre: "Juan",
  edad: 25
}
```

### Tipos Literales

Valores específicos como tipos:

```typescript
type Color = "rojo" | "verde" | "azul"
let miColor: Color = "rojo"  // ✅ Válido
// let otroColor: Color = "amarillo"  // ❌ Error

type Numero = 1 | 2 | 3
let miNumero: Numero = 2  // ✅ Válido
```

### Utility Types

TypeScript proporciona tipos útiles predefinidos que permiten transformar tipos existentes:

#### Pick<T, K>

**Definición**: Selecciona solo algunas propiedades de un tipo existente. Útil cuando necesitas un subconjunto de propiedades.

```typescript
type Usuario = {
  id: number
  nombre: string
  email: string
  edad: number
  password: string
  rol: string
}

// Seleccionar solo nombre y email
type UsuarioBasico = Pick<Usuario, "nombre" | "email">
// Resultado: { nombre: string, email: string }

// Uso práctico
function mostrarUsuarioBasico(usuario: UsuarioBasico) {
  console.log(`${usuario.nombre} - ${usuario.email}`)
  // No podemos acceder a usuario.id, usuario.password, etc.
}

// Ejemplo: Crear tipo para formulario de edición
type UsuarioEditable = Pick<Usuario, "nombre" | "email" | "edad">
// { nombre: string, email: string, edad: number }
```

**Características:**
- ✅ Selecciona propiedades específicas
- ✅ Mantiene los tipos originales
- ✅ Útil para crear tipos más pequeños
- ✅ Evita duplicar código

#### Omit<T, K>

**Definición**: Crea un tipo omitiendo propiedades específicas. Útil cuando necesitas todas las propiedades excepto algunas.

```typescript
type Usuario = {
  id: number
  nombre: string
  email: string
  edad: number
  password: string
  rol: string
}

// Omitir id y password (útil para crear usuario sin datos sensibles)
type UsuarioPublico = Omit<Usuario, "id" | "password">
// Resultado: { nombre: string, email: string, edad: number, rol: string }

// Uso práctico
function mostrarUsuarioPublico(usuario: UsuarioPublico) {
  console.log(`${usuario.nombre} - ${usuario.email}`)
  // No podemos acceder a usuario.id ni usuario.password
}

// Ejemplo: Crear tipo para registro (sin id que se genera automáticamente)
type UsuarioRegistro = Omit<Usuario, "id">
// { nombre: string, email: string, edad: number, password: string, rol: string }
```

**Características:**
- ✅ Omite propiedades específicas
- ✅ Mantiene el resto de propiedades
- ✅ Útil para excluir datos sensibles
- ✅ Evita duplicar código

#### Otros Utility Types

```typescript
// Partial<T>: Hace todas las propiedades opcionales
type UsuarioParcial = Partial<Usuario>
// { id?: number, nombre?: string, email?: string, ... }

// Required<T>: Hace todas las propiedades requeridas
type UsuarioRequerido = Required<UsuarioParcial>
// { id: number, nombre: string, email: string, ... }

// Readonly<T>: Hace todas las propiedades de solo lectura
type UsuarioSoloLectura = Readonly<Usuario>
// Todas las propiedades son readonly
```

**Comparación: Pick vs Omit**

| Aspecto | Pick<T, K> | Omit<T, K> |
|---------|------------|------------|
| **Propósito** | Seleccionar propiedades específicas | Excluir propiedades específicas |
| **Uso** | Cuando necesitas pocas propiedades | Cuando necesitas todas excepto algunas |
| **Ejemplo** | `Pick<Usuario, "nombre" \| "email">` | `Omit<Usuario, "id" \| "password">` |
| **Cuándo usar** | Tipo pequeño con pocas propiedades | Tipo grande donde excluyes pocas |

---

## 5. Interfaces y Types

### Interfaces

Las **Interfaces** definen la "forma" o contrato que debe tener un objeto. No existen en JS, desaparecen al compilar.

```typescript
interface Usuario {
  id: number
  nombre: string
  email?: string  // ? indica opcional
  saludar(): void  // Definición de método
}

const user1: Usuario = {
  id: 1,
  nombre: "Carlos",
  saludar() {
    console.log(`Hola soy ${this.nombre}`)
  }
}
```

**Características de Interfaces:**
- ✅ Pueden extenderse (herencia)
- ✅ Permiten fusión de declaraciones
- ✅ Ideales para contratos de clases
- ✅ Pueden tener propiedades opcionales (`?`)

### Propiedades Opcionales

**Definición**: Las **propiedades opcionales** son propiedades que pueden estar presentes o no en un objeto. Se indican con el símbolo `?` después del nombre de la propiedad.

**Sintaxis:**
```typescript
interface Usuario {
  id: number
  nombre: string
  email?: string        // Propiedad opcional
  telefono?: string     // Propiedad opcional
  edad?: number         // Propiedad opcional
}

// ✅ Válido: sin propiedades opcionales
const usuario1: Usuario = {
  id: 1,
  nombre: "Juan"
}

// ✅ Válido: con algunas propiedades opcionales
const usuario2: Usuario = {
  id: 2,
  nombre: "María",
  email: "maria@example.com"
}

// ✅ Válido: con todas las propiedades
const usuario3: Usuario = {
  id: 3,
  nombre: "Pedro",
  email: "pedro@example.com",
  telefono: "123456789",
  edad: 25
}
```

**Características:**
- ✅ Se indica con `?` después del nombre: `propiedad?: tipo`
- ✅ Su valor puede ser `undefined` si no se proporciona
- ✅ Útil para propiedades que no siempre son necesarias
- ✅ Permite flexibilidad en la estructura de objetos

**Verificar propiedades opcionales:**
```typescript
function mostrarUsuario(usuario: Usuario): void {
  console.log(`Nombre: ${usuario.nombre}`)
  
  // Verificar si existe antes de usar
  if (usuario.email) {
    console.log(`Email: ${usuario.email}`)
  } else {
    console.log("Email: No proporcionado")
  }
  
  // Usar optional chaining
  console.log(`Teléfono: ${usuario.telefono ?? "No disponible"}`)
}
```

**Propiedades opcionales en clases:**
```typescript
class Usuario {
  id: number
  nombre: string
  email?: string  // Propiedad opcional

  constructor(id: number, nombre: string, email?: string) {
    this.id = id
    this.nombre = nombre
    this.email = email  // Puede ser undefined
  }
  
  tieneEmail(): boolean {
    return this.email !== undefined
  }
}
```

**Propiedades opcionales en types:**
```typescript
type Persona = {
  nombre: string
  edad?: number      // Opcional
  email?: string     // Opcional
}

const persona1: Persona = { nombre: "Juan" }  // ✅ Válido
const persona2: Persona = { nombre: "María", edad: 25 }  // ✅ Válido
```

### Métodos Opcionales

**Definición**: Los **métodos opcionales** son métodos que pueden estar presentes o no en una interface o clase.

**Sintaxis:**
```typescript
interface Usuario {
  nombre: string
  saludar(): void           // Método requerido
  despedir?(): void         // Método opcional
  obtenerEdad?(): number    // Método opcional con retorno
}

class UsuarioImplementado implements Usuario {
  nombre: string

  constructor(nombre: string) {
    this.nombre = nombre
  }

  saludar(): void {  // ✅ Requerido
    console.log(`Hola, soy ${this.nombre}`)
  }

  // despedir() no es requerido, puede omitirse
  // obtenerEdad() no es requerido, puede omitirse
}
```

**Verificar métodos opcionales:**
```typescript
function interactuar(usuario: Usuario): void {
  usuario.saludar()  // ✅ Siempre existe
  
  // Verificar si existe antes de llamar
  if (usuario.despedir) {
    usuario.despedir()
  }
  
  // Usar optional chaining
  usuario.obtenerEdad?.()  // Solo se ejecuta si existe
}
```

### Resumen: Todo lo que puede ser Opcional

1. **Propiedades de objetos** (`propiedad?: tipo`)
   ```typescript
   interface Usuario {
     nombre: string
     email?: string  // Propiedad opcional
   }
   ```

2. **Parámetros de funciones** (`parametro?: tipo`)
   ```typescript
   function saludar(nombre: string, edad?: number): void {
     // edad es opcional
   }
   ```

3. **Métodos en interfaces** (`metodo?(): void`)
   ```typescript
   interface Usuario {
     saludar(): void
     despedir?(): void  // Método opcional
   }
   ```

4. **Propiedades en clases** (`propiedad?: tipo`)
   ```typescript
   class Usuario {
     nombre: string
     email?: string  // Propiedad opcional
   }
   ```

5. **Elementos en tuplas** (`[string, number?]`)
   ```typescript
   let usuario: [string, number?] = ["Juan"]  // El número es opcional
   ```

### Types

Los **Types** son más flexibles y permiten alias, uniones e intersecciones:

```typescript
// Alias de tipo
type ID = string | number

// Unión de tipos
type Estado = "activo" | "inactivo" | "pendiente"

// Intersección
type Persona = {
  nombre: string
  edad: number
}

type Empleado = Persona & {
  salario: number
}
```

**Características de Types:**
- ✅ Más flexibles que interfaces
- ✅ Permiten uniones (`|`) e intersecciones (`&`)
- ✅ Útiles para alias y tipos complejos
- ❌ No permiten fusión de declaraciones

### Interfaces vs Types: ¿Cuándo usar cada uno?

**Usar Interface cuando:**
- ✅ Defines la forma de un objeto
- ✅ Necesitas que una clase implemente un contrato
- ✅ Quieres que pueda extenderse fácilmente
- ✅ Trabajas con objetos y clases

**Usar Type cuando:**
- ✅ Necesitas uniones o intersecciones
- ✅ Creas alias de tipos primitivos
- ✅ Defines tipos más complejos
- ✅ Trabajas con tipos que no son objetos

**Ejemplo de diferencia:**

```typescript
// Interface: Puede extenderse
interface Animal {
  nombre: string
}

interface Perro extends Animal {
  raza: string
}

// Type: Puede usar uniones
type ID = string | number
type Estado = "activo" | "inactivo"
```

### Extensión de Interfaces (Múltiples Interfaces)

Las interfaces pueden extenderse de múltiples interfaces, combinando sus propiedades:

```typescript
// Interface base
interface Persona {
  nombre: string
  edad: number
}

// Otra interface base
interface Empleado {
  salario: number
  departamento: string
}

// Interface que extiende múltiples interfaces
interface EmpleadoCompleto extends Persona, Empleado {
  id: number
}

// Resultado: EmpleadoCompleto tiene todas las propiedades
const empleado: EmpleadoCompleto = {
  id: 1,
  nombre: "Juan",
  edad: 30,
  salario: 50000,
  departamento: "IT"
}
```

**Características:**
- ✅ Puede extender múltiples interfaces
- ✅ Combina todas las propiedades
- ✅ Útil para crear interfaces complejas
- ✅ Mantiene la herencia de interfaces

### Unión de Interfaces (Union Types con Interfaces)

Puedes crear tipos que acepten una de varias interfaces usando union types:

```typescript
// Interfaces diferentes
interface Perro {
  tipo: "perro"
  raza: string
  ladrar(): void
}

interface Gato {
  tipo: "gato"
  color: string
  maullar(): void
}

// Union type: puede ser Perro O Gato
type Mascota = Perro | Gato

// Función que acepta cualquier mascota
function interactuar(mascota: Mascota) {
  if (mascota.tipo === "perro") {
    mascota.ladrar()  // TypeScript sabe que es Perro
  } else {
    mascota.maullar()  // TypeScript sabe que es Gato
  }
}

// Uso
const miPerro: Mascota = {
  tipo: "perro",
  raza: "Labrador",
  ladrar() {
    console.log("Guau!")
  }
}

const miGato: Mascota = {
  tipo: "gato",
  color: "Negro",
  maullar() {
    console.log("Miau!")
  }
}
```

**Características:**
- ✅ Permite que una variable sea una de varias interfaces
- ✅ TypeScript puede inferir el tipo con type guards
- ✅ Útil para tipos que pueden tener diferentes formas
- ✅ Permite polimorfismo con union types

### Intersección de Interfaces (Intersection Types)

Combina múltiples interfaces en una sola:

```typescript
interface Nombre {
  nombre: string
}

interface Edad {
  edad: number
}

// Intersección: debe tener nombre Y edad
type Persona = Nombre & Edad

const persona: Persona = {
  nombre: "Juan",
  edad: 25
}
```

**Características:**
- ✅ Combina todas las propiedades de las interfaces
- ✅ Todas las propiedades son requeridas
- ✅ Similar a `extends` pero con types

---

## 6. Clases en TypeScript

### Clases Básicas

TypeScript lleva la POO de JS al siguiente nivel con modificadores de acceso y contratos estrictos.

```typescript
class Persona {
  // Propiedades con tipos
  nombre: string
  edad: number

  constructor(nombre: string, edad: number) {
    this.nombre = nombre
    this.edad = edad
  }

  presentarse(): string {
    return `Hola, soy ${this.nombre}`
  }
}

// Crear instancia
const juan = new Persona("Juan", 25)
console.log(juan.presentarse())  // "Hola, soy Juan"
```

### Clases Abstractas

**Definición**: Una **clase abstracta** es una clase que **NO se puede instanciar** directamente. Sirve como base para otras clases y puede tener métodos abstractos (sin implementación) y métodos concretos (con implementación).

**Características:**
- ❌ No se puede instanciar directamente (`new ClaseAbstracta()` da error)
- ✅ Puede tener métodos **abstractos** (sin código, solo la firma)
- ✅ Puede tener métodos **concretos** (con código completo)
- ✅ Las clases hijas **deben** implementar los métodos abstractos
- ✅ Útil para definir estructura común con implementación parcial

**Ejemplo Completo:**

```typescript
// CLASE ABSTRACTA (no se puede crear directamente)
abstract class CuerpoCeleste {
  public nombre: string
  protected masaKg: number

  constructor(nombre: string, masaKg: number) {
    this.nombre = nombre
    this.masaKg = masaKg
  }

  // MÉTODO CONCRETO (con implementación)
  public getNombre(): string {
    return this.nombre
  }

  // MÉTODO ABSTRACTO (sin código, solo la firma)
  // Las clases hijas DEBEN implementarlo
  abstract describirDetalles(): string
}

// Clase hija que extiende la clase abstracta
class Planeta extends CuerpoCeleste {
  private cantLunas: number

  constructor(nombre: string, masaKg: number, cantLunas: number) {
    super(nombre, masaKg)
    this.cantLunas = cantLunas
  }

  // IMPLEMENTACIÓN OBLIGATORIA del método abstracto
  describirDetalles(): string {
    return `${this.nombre} tiene ${this.cantLunas} lunas y masa de ${this.masaKg} kg`
  }
}

// Clase hija diferente
class Estrella extends CuerpoCeleste {
  private temperatura: number

  constructor(nombre: string, masaKg: number, temperatura: number) {
    super(nombre, masaKg)
    this.temperatura = temperatura
  }

  // IMPLEMENTACIÓN OBLIGATORIA del método abstracto
  describirDetalles(): string {
    return `${this.nombre} tiene temperatura de ${this.temperatura}°C`
  }
}

// Uso
const tierra = new Planeta("Tierra", 5.97e24, 1)
const sol = new Estrella("Sol", 1.99e30, 5778)

console.log(tierra.describirDetalles())  // "Tierra tiene 1 lunas..."
console.log(sol.describirDetalles())     // "Sol tiene temperatura de 5778°C"

// ❌ Error: no se puede crear CuerpoCeleste directamente
// let cuerpo = new CuerpoCeleste("Sol", 1000)  // Error!
```

**Comparación: Clase Abstracta vs Interface**

| Aspecto | Clase Abstracta | Interface |
|---------|----------------|-----------|
| **Instanciación** | ❌ No se puede instanciar | ❌ No se puede instanciar |
| **Métodos con código** | ✅ Puede tener métodos concretos | ❌ Solo firma de métodos |
| **Métodos abstractos** | ✅ Puede tener métodos abstractos | ✅ Todos los métodos son abstractos |
| **Propiedades** | ✅ Puede tener propiedades | ✅ Solo puede definir propiedades |
| **Constructor** | ✅ Puede tener constructor | ❌ No puede tener constructor |
| **Herencia** | `extends` | `implements` |
| **Uso** | Cuando necesitas código común | Cuando solo necesitas contrato |

**Cuándo usar Clase Abstracta:**

- ✅ Necesitas código común que las clases hijas hereden
- ✅ Quieres forzar que las clases hijas implementen ciertos métodos
- ✅ Necesitas constructor con lógica común
- ✅ Quieres combinar herencia de código con contratos

**Cuándo usar Interface:**

- ✅ Solo necesitas definir un contrato (sin código)
- ✅ Múltiples clases no relacionadas deben cumplir el mismo contrato
- ✅ No necesitas código común

### Constructor con Atajos

TypeScript permite declarar e inicializar propiedades directamente en el constructor:

```typescript
class Persona {
  constructor(
    public nombre: string,  // Declara e inicializa automáticamente
    public edad: number
  ) {
    // No necesitas this.nombre = nombre
  }
}
```

### Modificadores de Acceso

**Definición**: Los modificadores de acceso controlan quién puede ver o modificar las propiedades y métodos de una clase. Son fundamentales para la encapsulación en POO.

**Modificadores disponibles:**

#### Public

```typescript
class CuentaBancaria {
  public numero: string  // Accesible desde cualquier lugar (por defecto)
}
```

**Características:**
- ✅ Accesible desde cualquier lugar (dentro y fuera de la clase)
- ✅ Es el modificador por defecto (no requiere palabra clave explícita)
- ✅ Permite lectura y escritura directa

#### Private

**Definición**: Solo accesible dentro de la **misma clase**. No se puede acceder desde fuera de la clase ni desde clases hijas.

**Sintaxis con `private` (TypeScript):**
```typescript
class CuentaBancaria {
  private saldo: number  // Solo accesible dentro de la misma clase
  
  constructor(saldoInicial: number) {
    this.saldo = saldoInicial
  }
}

const cuenta = new CuentaBancaria(1000)
// cuenta.saldo = 2000  // ❌ Error: No se puede acceder desde afuera
```

**Sintaxis con `#` (Private Fields - JavaScript/TypeScript moderno):**
```typescript
class CuentaBancaria {
  #saldo: number  // Private field con sintaxis # (JavaScript nativo)
  
  constructor(saldoInicial: number) {
    this.#saldo = saldoInicial
  }
  
  public verSaldo(): number {
    return this.#saldo  // ✅ Accesible dentro de la clase
  }
}

const cuenta = new CuentaBancaria(1000)
// cuenta.#saldo = 2000  // ❌ Error: No se puede acceder desde afuera
```

**Comparación: `private` vs `#`**

| Aspecto | `private` (TypeScript) | `#` (JavaScript nativo) |
|---------|----------------------|------------------------|
| **Compilación** | Se elimina al compilar (solo verificación en tiempo de compilación) | Se mantiene en JavaScript (verdadero privado) |
| **Acceso en runtime** | Puede accederse con trucos (no es realmente privado) | Realmente privado, no accesible en runtime |
| **Compatibilidad** | Solo TypeScript | JavaScript moderno (ES2022+) |
| **Recomendación** | Para proyectos TypeScript | Para privacidad real en runtime |

**Buena Práctica: Convención `_` para propiedades privadas**

```typescript
class CuentaBancaria {
  private _saldo: number  // Convención: usar _ antes del nombre
  
  constructor(saldoInicial: number) {
    this._saldo = saldoInicial
  }
  
  public get saldo(): number {
    return this._saldo  // Getter accede a _saldo
  }
  
  public set saldo(nuevoSaldo: number) {
    if (nuevoSaldo >= 0) {
      this._saldo = nuevoSaldo  // Setter modifica _saldo
    }
  }
}
```

**¿Por qué usar `_`?**
- ✅ Indica visualmente que es una propiedad privada
- ✅ Diferencia la propiedad privada del getter/setter público
- ✅ Convención ampliamente aceptada en la comunidad
- ✅ Facilita la lectura del código

**Características de Private:**
- ❌ Solo accesible dentro de la misma clase
- ❌ No accesible desde clases hijas
- ❌ No accesible desde fuera de la clase
- ✅ Oculta detalles de implementación
- ✅ Protege datos sensibles

#### Protected

```typescript
class CuerpoCeleste {
  protected codigo: string  // Accesible dentro de la clase y sus herederas
}

class Planeta extends CuerpoCeleste {
  constructor(codigo: string) {
    super()
    this.codigo = codigo  // ✅ Funciona (desde clase hija)
  }
}

const planeta = new Planeta("P001")
// planeta.codigo = "P002"  // ❌ Error (no accesible desde fuera)
```

**Características:**
- ✅ Accesible dentro de la clase
- ✅ Accesible desde clases hijas (herederas)
- ❌ No accesible desde fuera de la jerarquía
- ✅ Útil para herencia

#### Readonly

```typescript
class Usuario {
  readonly id: number  // Solo lectura
  
  constructor(id: number) {
    this.id = id  // ✅ Se puede asignar en constructor
  }
  
  cambiarId(nuevoId: number): void {
    // this.id = nuevoId  // ❌ Error: readonly no se puede modificar
  }
}

const usuario = new Usuario(1)
// usuario.id = 2  // ❌ Error: readonly
```

**Características:**
- ✅ Se puede asignar en constructor o declaración
- ❌ No se puede modificar después
- ✅ Útil para IDs, constantes, fechas de creación

### Ejemplo Completo con Todos los Modificadores:

```typescript
class CuentaBancaria {
  public numero: string        // Accesible desde cualquier lugar
  private _saldo: number        // Solo accesible dentro de la misma clase (convención _)
  protected codigo: string      // Accesible dentro de la clase y sus herederas
  readonly id: number          // Solo lectura
  #pin: number                 // Private field con # (JavaScript nativo)

  constructor(numero: string, saldoInicial: number, id: number, pin: number) {
    this.numero = numero
    this._saldo = saldoInicial
    this.codigo = "BANK"
    this.id = id
    this.#pin = pin
  }

  public depositar(monto: number): void {
    if (monto > 0) this._saldo += monto
  }

  public verSaldo(): number {
    return this._saldo
  }
  
  private validarPin(pin: number): boolean {
    return this.#pin === pin
  }
}

const cuenta = new CuentaBancaria("123", 1000, 1, 1234)
cuenta.numero = "456"      // ✅ Funciona (public)
// cuenta._saldo = 2000    // ❌ Error (private)
// cuenta.codigo = "ABC"   // ❌ Error (protected)
// cuenta.id = 2           // ❌ Error (readonly)
// cuenta.#pin = 5678      // ❌ Error (private field)
```

### Readonly

**Definición**: El modificador `readonly` hace que una propiedad sea de solo lectura. Se puede asignar solo en el constructor o en la declaración.

```typescript
class Usuario {
  readonly id: number
  readonly fechaCreacion: Date
  nombre: string

  constructor(id: number, nombre: string) {
    this.id = id  // ✅ Se puede asignar en constructor
    this.fechaCreacion = new Date()  // ✅ Se puede asignar en constructor
    this.nombre = nombre
  }

  cambiarNombre(nuevoNombre: string): void {
    this.nombre = nuevoNombre  // ✅ Funciona
    // this.id = 999  // ❌ Error: readonly no se puede modificar
  }
}

const usuario = new Usuario(1, "Juan")
// usuario.id = 2  // ❌ Error: readonly
```

**Características:**
- ✅ Se puede asignar en constructor o declaración
- ❌ No se puede modificar después
- ✅ Útil para IDs, constantes, fechas de creación

### Getters y Setters

**Definición**: Los **getters** y **setters** son métodos especiales que permiten acceder y modificar propiedades privadas de forma controlada. Se usan como propiedades, no como métodos.

#### Getter

**Definición**: Un **getter** es un método que permite **obtener, leer o recuperar** el valor de una propiedad privada. Se accede como si fuera una propiedad, no como un método.

**Sintaxis:**
```typescript
class Planeta {
  private _masaKg: number

  constructor(masaKg: number) {
    this._masaKg = masaKg
  }

  // Getter: Traer, obtener, leer
  public get masaKg(): number {
    return this._masaKg
  }
}

const saturno = new Planeta(200000)
console.log(saturno.masaKg)  // ✅ Usa getter - se accede como propiedad
// console.log(saturno.masaKg())  // ❌ Error: no se llama como método
```

**Características:**
- ✅ Se accede como propiedad (sin paréntesis)
- ✅ Sintaxis: `get nombrePropiedad(): tipoRetorno`
- ✅ Puede tener lógica adicional (cálculos, transformaciones)
- ✅ Útil para propiedades calculadas

**Ejemplo con lógica adicional:**
```typescript
class Planeta {
  private _masaKg: number
  private _radioKm: number

  constructor(masaKg: number, radioKm: number) {
    this._masaKg = masaKg
    this._radioKm = radioKm
  }

  // Getter con cálculo
  public get densidad(): number {
    const volumen = (4/3) * Math.PI * Math.pow(this._radioKm, 3)
    return this._masaKg / volumen
  }
}

const tierra = new Planeta(5.97e24, 6371)
console.log(tierra.densidad)  // Calcula la densidad automáticamente
```

#### Setter

**Definición**: Un **setter** es un método que permite **definir, configurar o asignar** un valor a una propiedad privada. Permite validación antes de asignar.

**Sintaxis:**
```typescript
class Planeta {
  private _masaKg: number

  constructor(masaKg: number) {
    this._masaKg = masaKg
  }

  // Setter: Definir, configurar, asignar
  public set masaKg(nuevaMasa: number) {
    if (nuevaMasa <= 0) {
      throw new Error("La masa debe ser mayor a 0")
    }
    this._masaKg = nuevaMasa
  }
}

const saturno = new Planeta(200000)
saturno.masaKg = 250000  // ✅ Usa setter - se asigna como propiedad
// saturno.masaKg = -100  // ❌ Error: lanza excepción por validación
```

**Características:**
- ✅ Se asigna como propiedad (no como método)
- ✅ Sintaxis: `set nombrePropiedad(valor: tipo): void`
- ✅ Permite validación antes de asignar
- ✅ No tiene retorno (void implícito)
- ✅ Útil para proteger datos

**Ejemplo con validación compleja:**
```typescript
class Usuario {
  private _email: string = ""

  public set email(nuevoEmail: string) {
    // Validar formato de email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!emailRegex.test(nuevoEmail)) {
      throw new Error("Email inválido")
    }
    this._email = nuevoEmail
  }

  public get email(): string {
    return this._email
  }
}

const usuario = new Usuario()
usuario.email = "juan@example.com"  // ✅ Válido
// usuario.email = "email-invalido"  // ❌ Error: email inválido
```

#### Getter y Setter Juntos

```typescript
class Planeta {
  private _masaKg: number

  constructor(masaKg: number) {
    this._masaKg = masaKg
  }

  // Getter: obtener valor
  public get masaKg(): number {
    return this._masaKg
  }

  // Setter: asignar valor con validación
  public set masaKg(nuevaMasa: number) {
    if (nuevaMasa <= 0) {
      throw new Error("La masa debe ser mayor a 0")
    }
    this._masaKg = nuevaMasa
  }
}

const saturno = new Planeta(200000)
console.log(saturno.masaKg)      // ✅ Usa getter
saturno.masaKg = 250000          // ✅ Usa setter
```

**Ventajas de Getters y Setters:**

- ✅ **Encapsulación**: Protegen propiedades privadas
- ✅ **Validación**: Permiten validar antes de asignar
- ✅ **Flexibilidad**: Pueden tener lógica adicional
- ✅ **Sintaxis natural**: Se usan como propiedades
- ✅ **Control**: Controlan cómo se accede y modifica

**Cuándo usar Getters y Setters:**

- ✅ Cuando necesitas validar antes de asignar
- ✅ Cuando necesitas calcular valores dinámicamente
- ✅ Cuando quieres ocultar la implementación interna
- ✅ Cuando necesitas logging o efectos secundarios

**Buena Práctica: Convención `_` para propiedades privadas**

```typescript
class Planeta {
  private _masaKg: number  // ✅ Convención: usar _ antes del nombre
  
  constructor(masaKg: number) {
    this._masaKg = masaKg
  }
  
  // Getter público sin _
  public get masaKg(): number {
    return this._masaKg  // Accede a la propiedad privada _masaKg
  }
  
  // Setter público sin _
  public set masaKg(nuevaMasa: number) {
    this._masaKg = nuevaMasa  // Modifica la propiedad privada _masaKg
  }
}
```

**¿Por qué usar `_`?**
- ✅ Diferencia visual entre propiedad privada (`_masaKg`) y getter/setter público (`masaKg`)
- ✅ Indica claramente que es una propiedad interna
- ✅ Convención ampliamente aceptada
- ✅ Facilita la lectura y mantenimiento del código

---

## 7. Funciones Tipadas

### Funciones con Tipos Explícitos

Definir explícitamente qué entra y qué sale de una función:

```typescript
// Función con parámetros y retorno tipados
function sumar(a: number, b: number): number {
  return a + b
}

// Función sin retorno (void)
function logMensaje(msg: string): void {
  console.log(msg)
}
```

### Parámetros Opcionales

**Definición**: Los **parámetros opcionales** son parámetros que pueden omitirse al llamar a una función. Se indican con el símbolo `?` después del nombre del parámetro.

**Sintaxis:**
```typescript
// Parámetro opcional con ?
function saludar(nombre: string, edad?: number): string {
  if (edad !== undefined) {
    return `Hola ${nombre}, tienes ${edad} años`
  }
  return `Hola ${nombre}`
}

// Uso
saludar("Juan")              // ✅ Funciona: edad es opcional
saludar("Juan", 25)          // ✅ Funciona: edad proporcionada
// saludar()                 // ❌ Error: nombre es requerido
```

**Características:**
- ✅ Se indica con `?` después del nombre: `parametro?: tipo`
- ✅ Deben ir después de los parámetros requeridos
- ✅ Su valor es `undefined` si no se proporciona
- ✅ Útil para funciones con parámetros que no siempre son necesarios

**Ejemplo con múltiples parámetros opcionales:**
```typescript
function crearUsuario(
  nombre: string,
  email?: string,
  edad?: number,
  activo?: boolean
): Usuario {
  return {
    nombre,
    email: email ?? "sin-email",
    edad: edad ?? 0,
    activo: activo ?? true
  }
}

// Todos estos son válidos:
crearUsuario("Juan")
crearUsuario("Juan", "juan@example.com")
crearUsuario("Juan", "juan@example.com", 25)
crearUsuario("Juan", "juan@example.com", 25, false)
```

**Parámetros opcionales en métodos de clase:**
```typescript
class Usuario {
  nombre: string
  email?: string  // Propiedad opcional

  constructor(nombre: string, email?: string) {
    this.nombre = nombre
    this.email = email
  }

  // Método con parámetro opcional
  actualizarEmail(nuevoEmail?: string): void {
    if (nuevoEmail) {
      this.email = nuevoEmail
    }
  }
}
```

### Parámetros por Defecto

**Definición**: Los **parámetros por defecto** tienen un valor predeterminado que se usa si no se proporciona un valor al llamar a la función.

**Sintaxis:**
```typescript
function multiplicar(a: number, b: number = 1): number {
  return a * b
}

multiplicar(5)      // ✅ 5 * 1 = 5 (usa valor por defecto)
multiplicar(5, 3)    // ✅ 5 * 3 = 15 (usa valor proporcionado)
```

**Diferencia: Parámetro Opcional vs Parámetro por Defecto**

| Aspecto | Parámetro Opcional (`?`) | Parámetro por Defecto (`=`) |
|---------|-------------------------|----------------------------|
| **Sintaxis** | `parametro?: tipo` | `parametro: tipo = valor` |
| **Valor si no se proporciona** | `undefined` | Valor por defecto especificado |
| **Uso** | Cuando el parámetro puede no existir | Cuando quieres un valor predeterminado |
| **Ejemplo** | `saludar(nombre: string, edad?: number)` | `multiplicar(a: number, b: number = 1)` |

**Ejemplo combinando ambos:**
```typescript
function configurar(
  nombre: string,           // Requerido
  puerto: number = 3000,    // Por defecto
  debug?: boolean           // Opcional
): void {
  console.log(`Configurando ${nombre} en puerto ${puerto}`)
  if (debug) {
    console.log("Modo debug activado")
  }
}

configurar("API")                    // ✅ puerto=3000, debug=undefined
configurar("API", 8080)              // ✅ puerto=8080, debug=undefined
configurar("API", 8080, true)        // ✅ puerto=8080, debug=true
```

### Funciones como Tipos

```typescript
// Tipo de función
type Operacion = (a: number, b: number) => number

const sumar: Operacion = (a, b) => a + b
const restar: Operacion = (a, b) => a - b
```

### Funciones con Rest Parameters

```typescript
function sumarTodos(...numeros: number[]): number {
  return numeros.reduce((total, num) => total + num, 0)
}

sumarTodos(1, 2, 3, 4, 5)  // 15
```

---

## 7.5. Generics (Genéricos)

### ¿Qué son los Generics?

Los **Generics** permiten crear componentes reutilizables que funcionan con múltiples tipos en lugar de un solo tipo. Son como "variables de tipo" que se especifican cuando se usa el componente.

### Sintaxis Básica

```typescript
// Función genérica
function obtenerPrimero<T>(array: T[]): T {
  return array[0]
}

// Uso con diferentes tipos
const numeros = [1, 2, 3]
const primeroNumero = obtenerPrimero<number>(numeros)  // number

const palabras = ["uno", "dos", "tres"]
const primeraPalabra = obtenerPrimero<string>(palabras)  // string
```

**Características:**
- `<T>` es el parámetro de tipo genérico
- `T` puede ser cualquier nombre (común: `T`, `U`, `V`, `K`, `V`)
- TypeScript infiere el tipo automáticamente si no se especifica

### Generics con Múltiples Tipos

```typescript
function combinar<T, U>(primero: T, segundo: U): [T, U] {
  return [primero, segundo]
}

const resultado = combinar<string, number>("Juan", 25)
// [string, number]
```

### Generics en Clases

```typescript
class Contenedor<T> {
  private items: T[] = []

  agregar(item: T): void {
    this.items.push(item)
  }

  obtener(index: number): T {
    return this.items[index]
  }
}

// Uso con diferentes tipos
const contenedorNumeros = new Contenedor<number>()
contenedorNumeros.agregar(1)
contenedorNumeros.agregar(2)

const contenedorStrings = new Contenedor<string>()
contenedorStrings.agregar("uno")
contenedorStrings.agregar("dos")
```

### Generics con Constraints (Restricciones)

```typescript
// T debe tener una propiedad 'id'
interface ConId {
  id: number
}

function obtenerId<T extends ConId>(item: T): number {
  return item.id
}

const usuario = { id: 1, nombre: "Juan" }
obtenerId(usuario)  // ✅ Funciona
```

**Características:**
- `extends` restringe el tipo genérico
- Garantiza que el tipo tenga ciertas propiedades
- Útil para asegurar que tipos genéricos cumplan contratos

### Generics en Interfaces

```typescript
interface Repositorio<T> {
  guardar(item: T): void
  obtener(id: number): T | undefined
  eliminar(id: number): void
}

class UsuarioRepositorio implements Repositorio<Usuario> {
  guardar(usuario: Usuario): void {
    // implementación
  }
  
  obtener(id: number): Usuario | undefined {
    // implementación
  }
  
  eliminar(id: number): void {
    // implementación
  }
}
```

---

## 7.6. Type Assertions y Type Guards

### Type Assertions (Aserciones de Tipo)

Permiten decirle a TypeScript que confíe en ti sobre el tipo de un valor:

```typescript
// Sintaxis 1: as
let valor: unknown = "Hola"
let longitud = (valor as string).length  // TypeScript confía que es string

// Sintaxis 2: <tipo>
let otroValor: unknown = 42
let numero = <number>otroValor  // TypeScript confía que es number
```

**Cuándo usar:**
- Cuando sabes más sobre el tipo que TypeScript
- Al trabajar con APIs externas
- Al migrar código JavaScript a TypeScript

**Precaución**: Las aserciones no verifican el tipo en tiempo de ejecución, solo le dicen a TypeScript que confíe en ti.

### Type Guards (Guardias de Tipo)

Funciones que verifican el tipo en tiempo de ejecución:

```typescript
// Type guard simple
function esString(valor: unknown): valor is string {
  return typeof valor === "string"
}

// Type guard con objeto
interface Perro {
  tipo: "perro"
  ladrar(): void
}

interface Gato {
  tipo: "gato"
  maullar(): void
}

function esPerro(mascota: Perro | Gato): mascota is Perro {
  return mascota.tipo === "perro"
}

// Uso
function interactuar(mascota: Perro | Gato) {
  if (esPerro(mascota)) {
    mascota.ladrar()  // TypeScript sabe que es Perro
  } else {
    mascota.maullar()  // TypeScript sabe que es Gato
  }
}
```

**Características:**
- ✅ Verifican el tipo en tiempo de ejecución
- ✅ TypeScript entiende el tipo después de la verificación
- ✅ Útiles con union types
- ✅ Sintaxis: `valor is Tipo`

---

## Parte 2: Programación Orientada a Objetos (POO)

## 8. Introducción a POO

### ¿Qué es un Paradigma de Programación?

**Definición**: Un **paradigma de programación** es un modelo o estilo de programación que proporciona un marco conceptual y una forma de pensar para resolver problemas. Define cómo estructurar y organizar el código, qué conceptos usar y cómo pensar sobre la solución.

**Características:**
- ✅ Proporciona un enfoque para resolver problemas
- ✅ Define reglas y principios de diseño
- ✅ Establece patrones de organización del código
- ✅ Influye en cómo pensamos y estructuramos las soluciones

### Paradigmas de Programación Principales:

#### Paradigma Imperativo

**Definición**: Se centra en el **"cómo"**. Describe paso a paso **cómo** lograr un resultado, especificando los pasos exactos que la computadora debe seguir.

**Características:**
- ✅ Describe el proceso paso a paso
- ✅ Se enfoca en el flujo de control (secuencias, bucles, condiciones)
- ✅ El programador controla cada detalle de la ejecución
- ✅ Ejemplos: C, Pascal, FORTRAN

**Subparadigmas:**
- **Estructurada**: Organiza el código en estructuras lógicas secuenciales (if/else, bucles, funciones)
- **Procedimental**: Se centra en rutinas o funciones para tareas específicas

**Ejemplo:**
```typescript
// Imperativo: describe CÓMO sumar números
function sumarArray(numeros: number[]): number {
  let suma = 0
  for (let i = 0; i < numeros.length; i++) {
    suma = suma + numeros[i]  // Paso a paso
  }
  return suma
}
```

#### Paradigma Declarativo

**Definición**: Se centra en el **"qué"**. Describe el resultado deseado sin detallar los pasos específicos de cómo lograrlo.

**Características:**
- ✅ Describe qué se quiere lograr
- ✅ No especifica el proceso paso a paso
- ✅ El sistema decide cómo ejecutar
- ✅ Ejemplos: SQL, HTML, CSS, lenguajes funcionales

**Ejemplo:**
```typescript
// Declarativo: describe QUÉ queremos (la suma)
function sumarArray(numeros: number[]): number {
  return numeros.reduce((suma, num) => suma + num, 0)  // Qué queremos, no cómo
}
```

**Comparación:**

| Aspecto | Imperativo | Declarativo |
|---------|------------|-------------|
| **Enfoque** | "Cómo" hacerlo | "Qué" queremos |
| **Control** | Programador controla pasos | Sistema decide cómo |
| **Legibilidad** | Más verboso | Más conciso |
| **Ejemplos** | C, Pascal, loops tradicionales | SQL, HTML, métodos de array |

### ¿Qué es POO?

La **Programación Orientada a Objetos (POO)** es un paradigma de programación que organiza el código en **objetos** que encapsulan datos (propiedades) y comportamiento (métodos). Facilita la gestión del código y su mantenimiento.

**Características de POO:**

1. **Modelado de la Realidad**: Permite representar entidades del mundo real en el software
2. **Modularidad**: Las clases proporcionan estructura modular al código
3. **Reutilización**: Permite reutilizar código mediante herencia y composición
4. **Mantenibilidad**: Facilita el mantenimiento y la extensión del código
5. **Organización**: Organiza el código de forma lógica y jerárquica

**Usos de POO:**

- ✅ **Sistemas Complejos**: Ideal para proyectos grandes y complejos
- ✅ **Modelado de Dominio**: Representar entidades del mundo real (Usuario, Producto, etc.)
- ✅ **Reutilización de Código**: Crear componentes reutilizables
- ✅ **Trabajo en Equipo**: Facilita la colaboración en equipos grandes
- ✅ **Mantenimiento**: Facilita actualizar y extender código existente
- ✅ **Testing**: Facilita crear pruebas unitarias

**Cuándo usar POO:**

- ✅ Cuando necesitas modelar entidades del mundo real
- ✅ Proyectos grandes que requieren organización
- ✅ Cuando necesitas reutilizar código
- ✅ Sistemas que requieren mantenimiento a largo plazo

**Cuándo NO usar POO:**

- ❌ Proyectos muy pequeños y simples
- ❌ Cuando la complejidad de POO no aporta valor
- ❌ Algoritmos simples que no requieren estructura compleja

### Conceptos Fundamentales:

#### 1. Clase
- **Definición**: El "plano" o plantilla. Define propiedades (datos) y métodos (funciones).
- **Ejemplo**: `class Persona { nombre: string, edad: number }`

#### 2. Objeto
- **Definición**: Una instancia concreta de la clase (la "casa" construida con el plano).
- **Ejemplo**: `const juan = new Persona("Juan", 25)`

#### 3. Instancia
- **Definición**: El proceso de crear un objeto (`new Clase()`).
- **Ejemplo**: `new Persona()` crea una instancia de Persona

#### 4. Método
- **Definición**: Una función definida dentro de una clase que describe comportamientos.
- **Ejemplo**: `saludar(): void { console.log("Hola") }`

### Los 4 Pilares de la POO:

1. **Encapsulamiento**: Ocultar detalles internos y proteger el estado del objeto
2. **Herencia**: Una clase "hija" hereda de una clase "padre" (`extends`)
3. **Polimorfismo**: Un mismo método puede comportarse de diferentes formas
4. **Abstracción**: Simplificar la complejidad enfocándose solo en lo esencial

---

## 9. Encapsulación

### ¿Qué es la Encapsulación?

La **encapsulación** es el principio de ocultar los detalles internos del objeto y exponer solo lo necesario a través de métodos públicos. Protege datos controlando cómo se accede y modifica la información.

### Modificadores de Acceso

#### Public
```typescript
class Planeta {
  public nombre: string  // Accesible desde cualquier lugar
  // Equivalente a: nombre: string; (public es por defecto)
}
```

**Características:**
- Accesible desde cualquier lugar
- Permite lectura y escritura directa
- No requiere palabra clave explícita

#### Private
```typescript
class Planeta {
  private _masaKg: number  // Solo accesible desde dentro de la clase
}
```

**Características:**
- Solo accesible desde dentro de la clase
- Oculta detalles de implementación
- Convención: usar `_` al inicio (`_masaKg`)
- No se puede leer ni modificar desde afuera sin métodos

#### Protected
```typescript
class CuerpoCeleste {
  protected radioKm: number  // Accesible desde clase y clases hijas
}
```

**Características:**
- Accesible desde la clase y sus clases hijas
- Útil para herencia
- No accesible desde fuera

### Getters y Setters

#### Getter
```typescript
class Planeta {
  private _masaKg: number
  
  public get masaKg(): number {
    return this._masaKg
  }
}

let saturno = new Planeta("Saturno", 200000)
console.log(saturno.masaKg)  // Usa getter - se accede como propiedad
```

**Características:**
- Traer, obtener, leer, recuperar un valor
- Se accede como propiedad (no como método)
- Sintaxis: `get nombrePropiedad()`

#### Setter
```typescript
class Planeta {
  private _masaKg: number
  
  public set masaKg(nuevaMasa: number) {
    if (nuevaMasa <= 0) {
      throw new Error("La masa debe ser mayor a 0")
    }
    this._masaKg = nuevaMasa
  }
}

saturno.masaKg = 250000  // Usa setter - se asigna como propiedad
```

**Características:**
- Definir, configurar, asignar un valor
- Permite validación antes de asignar
- Sintaxis: `set nombrePropiedad(valor)`
- No tiene retorno (void implícito)

### Métodos Privados

```typescript
class Planeta {
  private metodoPrivado(): void {
    console.log("Soy un método interno")
  }
  
  public getMasaKg(): number {
    this.metodoPrivado()  // Puede usar métodos privados
    return this._masaKg
  }
}

// saturno.metodoPrivado()  // ❌ Error: No se puede acceder desde afuera
```

**Características:**
- Solo accesibles desde dentro de la clase
- Útiles para lógica interna
- Disminuyen complejidad de la clase
- Protegen funcionalidad interna

### Beneficios de la Encapsulación:

- ✅ **Protección de Datos**: Ayuda a mantener la integridad de los datos del objeto
- ✅ **Control de la Lógica de Acceso**: Permite validar los datos antes de asignarlos
- ✅ **Facilidad de Mantenimiento**: Se pueden cambiar detalles internos sin afectar código externo

---

## 10. Herencia

### ¿Qué es la Herencia?

La **herencia** permite que una clase (hija) herede propiedades y métodos de otra clase (padre), permitiendo reutilización de código y creación de jerarquías de clases.

### Sintaxis de Herencia

```typescript
// Clase padre (superclase)
class CuerpoCeleste {
  public nombre: string
  private codigo: string
  
  constructor(nombre: string, codigo: string) {
    this.nombre = nombre
    this.codigo = codigo
  }
  
  get getCodigo() {
    return this.codigo
  }
}

// Clase hija (subclase)
class Planeta extends CuerpoCeleste {
  esHabitable: boolean
  cantLunas: number
  
  constructor(nombre: string, codigo: string, esHabitable: boolean, cantLunas: number) {
    super(nombre, codigo)  // Llama al constructor padre
    this.esHabitable = esHabitable
    this.cantLunas = cantLunas
  }
}
```

### Palabra clave `super`

```typescript
class Planeta extends CuerpoCeleste {
  constructor(nombre: string, codigo: string, esHabitable: boolean) {
    super(nombre, codigo)  // Llama al constructor de CuerpoCeleste
    // super toma los datos del constructor para asignarlos a las variables internas heredadas
    this.esHabitable = esHabitable
  }
}
```

**Características:**
- Se refiere a la **superclase** (clase padre)
- Se usa para llamar al constructor padre
- Debe ser la primera línea del constructor hijo
- Toma los datos del constructor para asignarlos a las variables internas heredadas

### ¿Qué se puede heredar?

1. **Métodos**: Todos los métodos públicos y protegidos
2. **Propiedades**: Todas las propiedades públicas y protegidas
3. **Constructor**: Se llama con `super()`
4. **Modificadores de acceso**: Se respetan (public, protected, private)

### Modificadores de Acceso en Herencia

```typescript
class Padre {
  public publico: string        // Accesible desde cualquier lugar
  protected protegido: string   // Accesible desde clase y clases hijas
  private privado: string       // Solo accesible desde dentro de la clase
}

class Hijo extends Padre {
  constructor() {
    super()
    this.publico = "accesible"      // ✅ Funciona
    this.protegido = "accesible"    // ✅ Funciona (desde clase hija)
    // this.privado = "error"       // ❌ Error: No accesible
  }
}
```

### Herencia de Múltiples Niveles

```typescript
class CuerpoCeleste {
  nombre: string
}

class Estrella extends CuerpoCeleste {
  tamanio: number
}

class EnanaBlanca extends Estrella {
  edad: number
  // Hereda de Estrella (que hereda de CuerpoCeleste)
}
```

### Beneficios de la Herencia:

- ✅ **Reutilización del Código**: Las subclases pueden utilizar y extender funcionalidades de las superclases
- ✅ **Relaciones Jerárquicas**: Facilita la organización del código en una estructura más lógica y jerárquica
- ✅ **Especialización**: Las subclases pueden especializarse a partir de características comunes

---

## 11. Polimorfismo

### ¿Qué es el Polimorfismo?

El **polimorfismo** permite que diferentes clases implementen el mismo método de diferentes formas, permitiendo tratar objetos de diferentes clases de manera uniforme.

### Polimorfismo con Interfaces (`implements`)

```typescript
// Interface -> contrato (no tiene implementación)
interface Vehiculo {
  conducir(): void
}

// Múltiples clases implementan la misma interface
class Auto implements Vehiculo {
  conducir() {
    console.log("Conduce por carretera")
  }
}

class Avion implements Vehiculo {
  conducir(): void {
    console.log("Conduce por aire")
  }
}

class Barco implements Vehiculo {
  conducir(): void {
    console.log("Conduce por agua")
  }
}

// Todas pueden tratarse como Vehiculo
let vehiculos: Vehiculo[] = [
  new Auto(),
  new Avion(),
  new Barco()
]

vehiculos.forEach(v => v.conducir())  // Cada uno se comporta diferente
```

**Características:**
- Interface define contrato sin implementación
- Múltiples clases pueden implementar la misma interface
- Cada clase implementa el método según sus necesidades
- Interface no se instancia (no se puede hacer `new Vehiculo()`)

### Polimorfismo con Clases (`extends`) - Override

```typescript
// Clase padre con método implementado
class Animal {
  hacerSonido(): void {
    console.log("hace un sonido")
  }
}

// Clases hijas sobreescriben el método
class Perro extends Animal {
  hacerSonido(): void {  // Override: mismo nombre, diferente implementación
    console.log("Guau guau")
  }
}

class Gato extends Animal {
  hacerSonido(): void {  // Override
    console.log("Miau miau")
  }
}

// Todas pueden tratarse como Animal
let animales: Animal[] = [
  new Perro(),
  new Gato()
]

animales.forEach(a => a.hacerSonido())  // Cada uno hace su sonido
```

**Características:**
- Clase padre tiene método implementado
- Clase hija sobreescribe (override) el método
- Mismo nombre de método, diferente comportamiento
- Permite personalizar comportamiento manteniendo estructura

### Diferencia: `implements` vs `extends`

#### `implements` (Interfaces)
```typescript
interface Vehiculo {
  conducir(): void  // Sin implementación
}

class Auto implements Vehiculo {
  conducir(): void {  // DEBE implementar
    // implementación
  }
}
```

**Características:**
- Interface no tiene implementación
- Clase DEBE implementar todos los métodos
- Define contrato que debe cumplirse

#### `extends` (Clases)
```typescript
class Animal {
  hacerSonido(): void {  // Con implementación
    console.log("hace un sonido")
  }
}

class Perro extends Animal {
  hacerSonido(): void {  // PUEDE sobreescribir (override)
    console.log("Guau guau")
  }
}
```

**Características:**
- Clase padre tiene implementación
- Clase hija PUEDE sobreescribir (no obligatorio)
- Hereda implementación si no sobreescribe

### Beneficios del Polimorfismo:

- ✅ **Flexibilidad**: Permite agregar nuevas clases sin modificar código existente
- ✅ **Tratamiento Uniforme**: Objetos diferentes pueden tratarse igual
- ✅ **Extensibilidad**: Fácil agregar nuevos comportamientos

---

## 12. Composición

### ¿Qué es la Composición?

La **composición** permite construir objetos complejos a partir de objetos más simples, creando relaciones "tiene un" (has-a) en lugar de "es un" (is-a) como en la herencia.

### Composición Básica

```typescript
// Objeto simple
class Nacionalidad {
  nombre: string
  codPais: string
}

// Objeto compuesto (tiene una Nacionalidad)
class Marca {
  nombre: string
  nacionalidad: Nacionalidad  // Composición: "tiene un"
  
  constructor(nombre: string, nacionalidad: Nacionalidad) {
    this.nombre = nombre
    this.nacionalidad = nacionalidad
  }
}

// Objeto más complejo (tiene una Marca)
class Auto {
  marca: Marca  // Composición: "tiene un"
  modelo: string
  
  constructor(marca: Marca, modelo: string) {
    this.marca = marca
    this.modelo = modelo
  }
}
```

**Características:**
- Un objeto contiene instancias de otros objetos
- Relación "tiene un" (has-a)
- Permite construir objetos complejos desde simples
- Mayor flexibilidad que herencia

### Composición vs Herencia

#### Herencia (is-a)
```typescript
// "Un Perro ES UN Animal"
class Animal {
  nombre: string
}

class Perro extends Animal {  // Herencia: "es un"
  raza: string
}
```

**Relación**: "es un" (is-a)

#### Composición (has-a)
```typescript
// "Un Auto TIENE UNA Marca"
class Marca {
  nombre: string
}

class Auto {
  marca: Marca  // Composición: "tiene un"
  modelo: string
}
```

**Relación**: "tiene un" (has-a)

### Composición con Arrays

```typescript
class Libro {
  titulo: string
}

class Biblioteca {
  private libros: Libro[] = []  // Composición: tiene muchos libros
  
  agregarLibro(libro: Libro): void {
    this.libros.push(libro)
  }
  
  mostrarCatalogo(): void {
    this.libros.forEach(libro => console.log(libro.titulo))
  }
}
```

**Características:**
- Composición puede ser con un objeto o con muchos (arrays)
- Permite gestionar colecciones de objetos

### ¿Cuándo usar Composición vs Herencia?

**Usar Herencia cuando:**
- ✅ Hay relación "es un" (is-a)
- ✅ Quieres reutilizar código
- ✅ Necesitas polimorfismo
- ✅ Hay jerarquía clara

**Usar Composición cuando:**
- ✅ Hay relación "tiene un" (has-a)
- ✅ Quieres mayor flexibilidad
- ✅ Quieres evitar acoplamiento fuerte
- ✅ Los objetos son independientes

### Beneficios de la Composición:

- ✅ **Flexibilidad**: Permite cambiar componentes sin afectar el objeto principal
- ✅ **Reutilización**: Permite construir objetos complejos desde simples
- ✅ **Bajo Acoplamiento**: Los objetos son más independientes

---

## 13. Proyecto Integrador: Concesionario

### Descripción

Este proyecto integra todos los conceptos de POO aprendidos: encapsulación, polimorfismo, composición, interfaces, enums y validación de reglas de negocio.

### Conceptos Aplicados:

1. **Encapsulación**: Propiedades privadas, getters/setters
2. **Polimorfismo**: Interface Motor con múltiples implementaciones
3. **Composición**: Auto compuesto por motor, chasis, ruedas
4. **Interfaces**: Contrato Motor
5. **Enums**: TipoCombustible, TipoRueda
6. **Validación**: Reglas de negocio en constructor

### Estructura del Proyecto:

```
src/
├── interfaces.ts          # Interface Motor
├── partes.ts              # Chasis, Rueda, enums
├── MotorDeCombustion.ts   # Motor de combustión
├── MotorElectrico.ts      # Motor eléctrico
├── Auto.ts                # Clase Auto con composición
├── Concesionario.ts       # Sistema administrador
└── index.ts               # Archivo principal
```

### Ejemplo de Código:

```typescript
// Interface para polimorfismo
interface Motor {
  encender(): void
  apagar(): void
  obtenerPotencia(): number
}

// Enum para tipos
enum TipoCombustible {
  Nafta = "Nafta",
  Gasoil = "Gasoil",
  Electrico = "Electrico"
}

// Clase con composición
class Auto {
  private motor: Motor
  private chasis: Chasis
  private ruedas: Rueda[]
  
  constructor(motor: Motor, chasis: Chasis, ruedas: Rueda[]) {
    this.motor = motor
    this.chasis = chasis
    this.ruedas = ruedas
  }
  
  // Getters con encapsulación
  public get getMotor(): Motor {
    return this.motor
  }
}
```

---

## Parte 3: Ejemplos Prácticos

## 14. Ejemplos del Código Modelo

### Tema 02: TypeScript - Introducción

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-02-typescript-introduccion`

**Conceptos cubiertos**:
- ✅ Instalación y configuración de TypeScript
- ✅ Tipos básicos (string, number, boolean, arrays)
- ✅ Tipado de objetos (inline, type, interface)
- ✅ Clases y constructores
- ✅ Funciones tipadas
- ✅ Material teórico extenso (845+ líneas)

**Ejemplos incluidos**:
1. **ejemplo-01-tipos-basicos.ts**: Tipos básicos completos
2. **ejemplo-02-clase-planeta.ts**: Clase simple
3. **ejemplo-03-poo-basico.ts**: Introducción a POO
4. **ejemplo-04-tipos-objetos.ts**: Tipado de objetos
5. **ejemplo-05-clase-planeta-simple.ts**: Clase simplificada
6. **ejemplo-06-interfaces.ts**: Interfaces básicas
7. **ejemplo-07-proyecto-completo/**: Proyecto completo con múltiples clases

### Tema 03: POO - Encapsulación

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-03-poo-encapsulacion`

**Conceptos cubiertos**:
- ✅ Modificadores de acceso (public, private, protected)
- ✅ Getters y setters
- ✅ Métodos privados
- ✅ Validación en setters

**Ejemplos incluidos**:
1. **ejemplo-01-encapsulacion-basica.ts**: Encapsulación básica
2. **ejemplo-02-encapsulacion-completa.ts**: Encapsulación completa con métodos privados

### Tema 04: POO - Herencia

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-04-poo-herencia`

**Conceptos cubiertos**:
- ✅ Sintaxis `extends`
- ✅ Palabra clave `super()`
- ✅ Herencia de múltiples niveles
- ✅ Modificadores de acceso en herencia

**Ejemplos incluidos**:
1. **ejemplo-01-herencia-basica.ts**: Herencia simple
2. **ejemplo-02-herencia-multiple.ts**: Herencia múltiple y jerarquía
3. **ejemplo-03-proyecto-biblioteca/**: Proyecto completo con herencia
4. **ejemplo-04-clase-person.ts**: Clase Person (padre)
5. **ejemplo-05-clase-owner.ts**: Clase Owner extends Person
6. **ejemplo-06-uso-herencia.ts**: Uso completo del sistema

### Tema 05: POO - Polimorfismo

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-05-poo-polimorfismo`

**Conceptos cubiertos**:
- ✅ Polimorfismo con interfaces (`implements`)
- ✅ Polimorfismo con clases (`extends`) y override
- ✅ Diferencia entre `implements` e `extends`

**Ejemplos incluidos**:
1. **ejemplo-01-polimorfismo-completo.ts**: Polimorfismo completo (ambos tipos)
2. **ejemplo-02-polimorfismo-basico.ts**: Polimorfismo básico

### Tema 06: POO - Composición

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-06-poo-composicion`

**Conceptos cubiertos**:
- ✅ Relación "tiene un" (has-a)
- ✅ Composición con objetos simples
- ✅ Composición con arrays
- ✅ Composición vs Herencia

**Ejemplos incluidos**:
1. **ejemplo-01-composicion-basica.ts**: Composición básica (Auto-Marca-Nacionalidad)
2. **ejemplo-02-biblioteca-composicion.ts**: Biblioteca con composición
3. **ejemplo-03-uso-biblioteca.ts**: Uso completo del sistema

### Tema 07: POO - Proyecto Integrador (Concesionario)

**Ubicación**: `cursadas/backend/backEnd_modelo/tema-07-poo-proyecto-integrador-concesionario`

**Conceptos cubiertos**:
- ✅ Integración de todos los conceptos de POO
- ✅ Encapsulación, polimorfismo, composición
- ✅ Interfaces y enums
- ✅ Validación de reglas de negocio

**Ejemplos incluidos**:
1. **ejemplo-01-concesionario-completo/**: Sistema completo con composición
2. **ejemplo-02-concesionario-con-herencia/**: Sistema con herencia adicional

---

## 🎯 Resumen de Conceptos Clave

### TypeScript
- **Superset de JavaScript**: Todo código JS es válido en TS
- **Tipado estático opcional**: Detecta errores antes de ejecutar
- **Transpilación**: Se convierte a JavaScript antes de ejecutar
- **Inferencia de tipos**: TypeScript infiere tipos automáticamente
- **Módulos**: Import/export para organizar código (`import type` para tipos)
- **Interfaces y Types**: Definen contratos y formas de objetos
- **Clases**: POO mejorada con modificadores de acceso
- **Optional Chaining (`?.`)**: Acceso seguro a propiedades anidadas
- **Nullish Coalescing (`??`)**: Valores por defecto solo para null/undefined
- **Destructuring**: Extraer valores de objetos/arrays con tipos

### POO - Los 4 Pilares
1. **Encapsulación**: Proteger datos con modificadores de acceso
2. **Herencia**: Reutilizar código con `extends` y `super()`
3. **Polimorfismo**: Mismo método, diferentes comportamientos
4. **Composición**: Construir objetos complejos desde simples

### Modificadores de Acceso
- `public`: Accesible desde cualquier lugar (por defecto)
- `private`: Solo accesible dentro de la misma clase
- `protected`: Accesible dentro de la clase y sus herederas

### Herencia vs Composición
- **Herencia**: "es un" (is-a) - `extends`
- **Composición**: "tiene un" (has-a) - Objeto contiene otros objetos

### Interfaces vs Types
- **Interface**: Para contratos, puede extenderse, múltiples interfaces
- **Type**: Para uniones, intersecciones, alias

### Clases Abstractas
- **Clase Abstracta**: No se puede instanciar, puede tener métodos concretos y abstractos
- **Método Abstracto**: Sin implementación, las clases hijas deben implementarlo
- **Uso**: Cuando necesitas código común con contratos obligatorios

### Generics
- **Generics**: Componentes reutilizables que funcionan con múltiples tipos
- **Sintaxis**: `<T>` donde T es el tipo genérico
- **Constraints**: `extends` para restringir tipos genéricos

---

## 📝 Buenas Prácticas

1. **Evitar `any`**: Usar tipos específicos siempre que sea posible
2. **Usar interfaces para contratos**: Cuando una clase debe implementar algo
3. **Usar types para uniones**: Cuando necesitas combinar tipos
4. **Encapsular datos privados**: Usar `private` y getters/setters
5. **Validar en setters**: Siempre validar antes de asignar
6. **Preferir composición sobre herencia**: Cuando sea posible
7. **Usar `super()` correctamente**: Primera línea del constructor hijo
8. **Documentar con tipos**: Los tipos actúan como documentación
9. **Usar Pick/Omit para evitar duplicación**: Crear tipos derivados en lugar de duplicar
10. **Clases abstractas para código común**: Cuando necesitas herencia con contratos
11. **Generics para reutilización**: Crear componentes que funcionen con múltiples tipos
12. **Type guards para union types**: Verificar tipos en tiempo de ejecución

---


**Referencias del Código Modelo**:
- `cursadas/backend/backEnd_modelo/tema-02-typescript-introduccion/`
- `cursadas/backend/backEnd_modelo/tema-03-poo-encapsulacion/`
- `cursadas/backend/backEnd_modelo/tema-04-poo-herencia/`
- `cursadas/backend/backEnd_modelo/tema-05-poo-polimorfismo/`
- `cursadas/backend/backEnd_modelo/tema-06-poo-composicion/`
- `cursadas/backend/backEnd_modelo/tema-07-poo-proyecto-integrador-concesionario/`
