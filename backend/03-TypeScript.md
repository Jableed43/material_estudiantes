# TypeScript: Introducción 🎯

## ¿Qué es TypeScript?

TypeScript es un superset de JavaScript que añade tipado estático opcional.

**Características**:
- Todo código JS válido es TS válido
- Tipado estático opcional
- Transpilación a JavaScript

## Tipos Básicos

```typescript
let nombre: string = "Juan"
let edad: number = 25
let activo: boolean = true
let datos: any = "cualquier cosa"
```

## Arrays y Objetos

```typescript
// Arrays
let numeros: number[] = [1, 2, 3]
let nombres: string[] = ["Juan", "María"]

// Objetos
interface Usuario {
    nombre: string
    edad: number
    email?: string  // Opcional
}

const usuario: Usuario = {
    nombre: "Juan",
    edad: 25
}
```

## Funciones Tipadas

```typescript
function sumar(a: number, b: number): number {
    return a + b
}

// Parámetros opcionales
function saludar(nombre: string, edad?: number): void {
    console.log(`Hola, ${nombre}`)
}
```

## Clases

```typescript
class Persona {
    nombre: string
    edad: number
    
    constructor(nombre: string, edad: number) {
        this.nombre = nombre
        this.edad = edad
    }
    
    saludar(): void {
        console.log(`Hola, soy ${this.nombre}`)
    }
}

const juan = new Persona("Juan", 25)
```

## Interfaces

```typescript
interface Vehiculo {
    marca: string
    modelo: string
    acelerar(): void
}

class Auto implements Vehiculo {
    marca: string
    modelo: string
    
    acelerar(): void {
        console.log("Acelerando...")
    }
}
```

## Conceptos Clave

1. **Tipos**: Definir tipos de variables
2. **Interfaces**: Contratos para objetos
3. **Clases**: POO con TypeScript
4. **Tipado Opcional**: `?` para propiedades opcionales
5. **Transpilación**: TS se compila a JS
6. **Type Inference**: TypeScript infiere tipos automáticamente
7. **Any**: Tipo que acepta cualquier valor (usar con cuidado)

