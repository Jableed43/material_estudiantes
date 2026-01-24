# POO: Herencia 👨‍👩‍👧

## 📑 Índice

1. [¿Qué es la Herencia? (Analogía del Mundo Real)](#qué-es-la-herencia-analogía-del-mundo-real)
2. [Herencia en Programación](#herencia-en-programación)
3. [Sintaxis Básica](#sintaxis-básica)
4. [La Palabra Clave `super`](#la-palabra-clave-super)
5. [Modificadores de Acceso en Herencia](#modificadores-de-acceso-en-herencia)
6. [Herencia Múltiple (Cadena de Herencia)](#herencia-múltiple-cadena-de-herencia)
7. [Cuándo Usar Herencia](#cuándo-usar-herencia)
8. [Beneficios y Ventajas](#beneficios-y-ventajas)
9. [Conceptos Clave](#conceptos-clave)
10. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es la Herencia? (Analogía del Mundo Real)

### 🧬 La Herencia en la Vida Real

Imagina que tienes una familia. Un hijo hereda características de sus padres:
- **Características físicas**: Color de ojos, altura, tipo de cabello
- **Características de personalidad**: Valores, gustos, habilidades
- **Bienes materiales**: Una casa, un auto, dinero

**Pero el hijo también puede tener sus propias características únicas**:
- Puede ser más alto que sus padres
- Puede tener habilidades que sus padres no tienen (ej: tocar guitarra)
- Puede agregar nuevas características a su vida

### 🏠 Analogía: La Casa Familiar

Piensa en una **casa familiar** (clase padre):
- Tiene: paredes, techo, puertas, ventanas
- Funcionalidades: proteger del clima, dar privacidad

Ahora imagina diferentes tipos de casas que **heredan** de la casa familiar:
- **Casa de playa** (clase hija): Tiene todo lo de la casa familiar + terraza, vista al mar
- **Casa de montaña** (clase hija): Tiene todo lo de la casa familiar + chimenea, aislamiento térmico
- **Casa inteligente** (clase hija): Tiene todo lo de la casa familiar + sistema de domótica

Todas son "casas", pero cada una tiene características especiales además de las básicas.

### 🚗 Analogía: Los Vehículos

Piensa en un **vehículo** (clase padre):
- Tiene: ruedas, motor, volante, frenos
- Funcionalidades: moverse, transportar personas

Diferentes vehículos que **heredan** del vehículo básico:
- **Auto** (clase hija): Tiene todo del vehículo + 4 puertas, maletero
- **Moto** (clase hija): Tiene todo del vehículo + 2 ruedas, manillar
- **Camión** (clase hija): Tiene todo del vehículo + caja de carga, más potencia

Todos son "vehículos", pero cada uno tiene características propias.

---

## Herencia en Programación

### ¿Qué es la Herencia en POO?

La **herencia** es un mecanismo que permite que una clase (llamada **clase hija** o **subclase**) **herede** propiedades y métodos de otra clase (llamada **clase padre** o **superclase**).

**En términos simples**: La clase hija "nace" con todo lo que tiene la clase padre, y puede agregar sus propias características o modificar algunas.

### ¿Por qué es Útil?

Imagina que estás creando un sistema para una tienda. Tienes diferentes tipos de productos:
- **Libros**: título, autor, precio, ISBN
- **Electrónicos**: nombre, marca, precio, garantía
- **Ropa**: nombre, talla, precio, material

Todos tienen algo en común: **nombre** y **precio**. En lugar de repetir esto en cada clase, puedes crear una clase padre `Producto` con nombre y precio, y las clases hijas heredan esto y agregan sus características específicas.

### Ventajas de la Herencia

1. **Evita duplicación de código**: No necesitas escribir lo mismo varias veces
2. **Mantenimiento fácil**: Si cambias algo en la clase padre, todas las hijas se actualizan
3. **Organización lógica**: Código más organizado y fácil de entender
4. **Reutilización**: Puedes reutilizar código existente

---

## Sintaxis Básica

### Estructura General

```typescript
// Clase padre (superclase)
class ClasePadre {
    // Propiedades y métodos comunes
}

// Clase hija (subclase) - usa 'extends'
class ClaseHija extends ClasePadre {
    // Propiedades y métodos propios
    // + todo lo que hereda de ClasePadre
}
```

### Ejemplo: Cuerpos Celestes

```typescript
// Clase padre: CuerpoCeleste
class CuerpoCeleste {
    public nombre: string
    private codigo: string
    
    constructor(nombre: string, codigo: string) {
        this.nombre = nombre
        this.codigo = codigo
    }
    
    public describir(): string {
        return `Soy ${this.nombre}`
    }
}

// Clase hija: Planeta (hereda de CuerpoCeleste)
class Planeta extends CuerpoCeleste {
    esHabitable: boolean
    
    constructor(nombre: string, codigo: string, esHabitable: boolean) {
        super(nombre, codigo)  // Llama al constructor padre
        this.esHabitable = esHabitable
    }
    
    // Método propio de Planeta
    public puedeVivir(): string {
        return this.esHabitable ? "Sí, es habitable" : "No, no es habitable"
    }
}

// Uso
const tierra = new Planeta("Tierra", "TER-001", true)
console.log(tierra.describir())      // "Soy Tierra" (heredado)
console.log(tierra.puedeVivir())    // "Sí, es habitable" (propio)
console.log(tierra.nombre)          // "Tierra" (heredado)
```

### ¿Qué se Hereda?

✅ **Se hereda**:
- Propiedades `public` y `protected`
- Métodos `public` y `protected`
- Comportamiento de la clase padre

❌ **NO se hereda**:
- Propiedades `private`
- Métodos `private`
- Constructores (pero se pueden llamar con `super()`)

---

## La Palabra Clave `super`

### ¿Qué es `super`?

`super` es una palabra clave que hace referencia a la **clase padre**. Es como decir "lo que tiene mi padre".

### Analogía: Construir una Casa

Imagina que estás construyendo una casa de playa:
1. **Primero** construyes la base (lo que tiene la casa familiar) → `super()`
2. **Después** agregas la terraza y la vista al mar (características propias)

En programación es igual:
```typescript
class CasaPlaya extends CasaFamiliar {
    constructor() {
        super()  // Primero construyes lo básico (casa familiar)
        // Luego agregas características propias
        this.terraza = true
        this.vistaAlMar = true
    }
}
```

### Regla Importante: `super()` Debe Ser Primero

```typescript
class Planeta extends CuerpoCeleste {
    constructor(nombre: string, codigo: string, esHabitable: boolean) {
        super(nombre, codigo)  // ✅ DEBE ser la primera línea
        this.esHabitable = esHabitable  // Luego puedes agregar lo tuyo
    }
}
```

**¿Por qué?** Porque antes de agregar características propias, necesitas "construir" lo que heredas del padre.

### `super` para Llamar Métodos del Padre

También puedes usar `super` para llamar métodos de la clase padre:

```typescript
class Planeta extends CuerpoCeleste {
    public describir(): string {
        const descripcionPadre = super.describir()  // Llama al método del padre
        return `${descripcionPadre} y soy un planeta`
    }
}
```

---

## Modificadores de Acceso en Herencia

### Analogía: La Casa Familiar y sus Habitaciones

Imagina una casa familiar con diferentes habitaciones:

- **Sala (public)**: Todos pueden entrar, incluso visitantes
- **Cocina (protected)**: Solo la familia puede entrar (padres e hijos)
- **Caja fuerte (private)**: Solo el dueño puede acceder, ni siquiera los hijos

### En Programación

```typescript
class Padre {
    public publico: string        // ✅ Accesible desde cualquier lugar
    protected protegido: string   // ✅ Accesible desde clase y clases hijas
    private privado: string      // ❌ No accesible (ni siquiera para hijos)
    
    public metodoPublico(): void {
        console.log("Todos pueden usar esto")
    }
    
    protected metodoProtegido(): void {
        console.log("Solo familia puede usar esto")
    }
    
    private metodoPrivado(): void {
        console.log("Solo yo puedo usar esto")
    }
}

class Hijo extends Padre {
    constructor() {
        super()
        this.publico = "accesible"      // ✅ Funciona (público)
        this.protegido = "accesible"    // ✅ Funciona (protegido - es familia)
        // this.privado = "error"        // ❌ Error (privado - ni familia)
        
        this.metodoPublico()            // ✅ Funciona
        this.metodoProtegido()          // ✅ Funciona (es familia)
        // this.metodoPrivado()         // ❌ Error
    }
}

// Desde fuera
const hijo = new Hijo()
console.log(hijo.publico)              // ✅ Funciona
// console.log(hijo.protegido)          // ❌ Error (no es público)
// console.log(hijo.privado)            // ❌ Error (es privado)
```

### Resumen de Modificadores

| Modificador | Accesible desde | Heredable | Ejemplo Real |
|-------------|----------------|-----------|--------------|
| `public` | Cualquier lugar | ✅ Sí | Sala de la casa (todos entran) |
| `protected` | Clase y clases hijas | ✅ Sí | Cocina (solo familia) |
| `private` | Solo la clase misma | ❌ No | Caja fuerte (solo dueño) |

---

## Herencia Múltiple (Cadena de Herencia)

### Analogía: La Familia Extendida

Imagina una familia de varias generaciones:
- **Abuelo** → tiene: apellido, nacionalidad
- **Padre** (hereda de Abuelo) → tiene: todo del abuelo + profesión, casa
- **Hijo** (hereda de Padre) → tiene: todo del padre + estudios, habilidades

El hijo tiene características de **todas las generaciones anteriores**.

### En Programación

```typescript
// Generación 1: Abuelo
class CuerpoCeleste {
    nombre: string
    constructor(nombre: string) {
        this.nombre = nombre
    }
}

// Generación 2: Padre (hereda de Abuelo)
class Estrella extends CuerpoCeleste {
    tamanio: number
    constructor(nombre: string, tamanio: number) {
        super(nombre)  // Construye lo del abuelo
        this.tamanio = tamanio
    }
}

// Generación 3: Hijo (hereda de Padre, que hereda de Abuelo)
class EnanaBlanca extends Estrella {
    edad: number
    constructor(nombre: string, tamanio: number, edad: number) {
        super(nombre, tamanio)  // Construye lo del padre (que incluye abuelo)
        this.edad = edad
    }
}

// EnanaBlanca tiene:
// - nombre (de CuerpoCeleste)
// - tamanio (de Estrella)
// - edad (propio)
```

### Cadena de Herencia

```
CuerpoCeleste (abuelo)
    ↓
Estrella (padre) - hereda de CuerpoCeleste
    ↓
EnanaBlanca (hijo) - hereda de Estrella (que ya heredó de CuerpoCeleste)
```

---

## Cuándo Usar Herencia

### ✅ Usa Herencia Cuando:

1. **Hay una relación "es un" (is-a)**:
   - Un `Perro` **es un** `Animal`
   - Un `Auto` **es un** `Vehículo`
   - Un `Estudiante` **es una** `Persona`

2. **Hay código común que se repite**:
   - Varias clases tienen las mismas propiedades/métodos
   - Puedes extraer lo común a una clase padre

3. **Hay una jerarquía lógica**:
   - Las clases forman una jerarquía natural
   - Las clases hijas son "tipos especiales" de la clase padre

### ❌ NO Uses Herencia Cuando:

1. **Solo quieres reutilizar código sin relación lógica**:
   - Mejor usa **Composición** (ver: [POO: Composición](./07-POO-Composicion.md))

2. **La relación es "tiene un" (has-a)**:
   - Un `Auto` **tiene un** `Motor` (no es un motor)
   - Un `Estudiante` **tiene un** `Libro` (no es un libro)

### Ejemplo: ¿Herencia o Composición?

**Herencia (es un)**:
```typescript
class Animal { }
class Perro extends Animal { }  // ✅ Perro ES UN Animal
```

**Composición (tiene un)**:
```typescript
class Motor { }
class Auto {
    motor: Motor  // Auto tiene un Motor (no es un motor)
}
```

---

## Beneficios y Ventajas

### 1. Reutilización de Código

**Sin herencia** (código duplicado):
```typescript
class Libro {
    titulo: string
    precio: number
    calcularDescuento(): number { }
}

class Electronico {
    nombre: string
    precio: number
    calcularDescuento(): number { }  // Mismo código duplicado
}
```

**Con herencia** (código reutilizado):
```typescript
class Producto {
    nombre: string
    precio: number
    calcularDescuento(): number { }  // Una sola vez
}

class Libro extends Producto { }
class Electronico extends Producto { }  // Heredan el método
```

### 2. Mantenibilidad

Si necesitas cambiar `calcularDescuento()`, solo lo cambias en `Producto` y todas las clases hijas se actualizan automáticamente.

### 3. Organización Lógica

El código refleja la realidad: los libros y electrónicos **son** productos, por lo tanto heredan de `Producto`.

### 4. Especialización

Cada clase hija puede agregar características propias:
```typescript
class Libro extends Producto {
    autor: string  // Solo los libros tienen autor
    isbn: string   // Solo los libros tienen ISBN
}
```

---

## Conceptos Clave

### Términos Importantes

1. **Clase Padre (Superclase)**: La clase de la que se hereda
2. **Clase Hija (Subclase)**: La clase que hereda
3. **`extends`**: Palabra clave para indicar herencia
4. **`super`**: Referencia a la clase padre
5. **Herencia Simple**: Una clase solo puede heredar de una clase padre
6. **Cadena de Herencia**: Una clase puede heredar de otra que hereda de otra

### Resumen Visual

```
┌─────────────────┐
│  Clase Padre    │
│  (Superclase)   │
└────────┬────────┘
         │ extends
         │ hereda
         ↓
┌─────────────────┐
│  Clase Hija     │
│  (Subclase)    │
│  + características│
│    propias      │
└─────────────────┘
```

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [POO: Encapsulación](./04-POO-Encapsulacion.md) - Antes de herencia, entiende encapsulación
- 📚 [POO: Polimorfismo](./06-POO-Polimorfismo.md) - Usa herencia para polimorfismo
- 📚 [POO: Composición](./07-POO-Composicion.md) - Alternativa a herencia cuando no hay relación "es un"
- 📚 [TypeScript: Introducción](./03-TypeScript.md) - Sintaxis TypeScript para clases

### Código Relacionado

- 💻 [Ejemplos de Herencia](../../CODIGO/backend/tema-04-poo-herencia/)

---

## 🎯 Puntos Clave para Recordar

1. **Herencia = "es un"**: Si algo "es un" tipo de otra cosa, usa herencia
2. **`super()` primero**: Siempre llama a `super()` antes de agregar características propias
3. **`public` y `protected` se heredan**: `private` NO se hereda
4. **Reutilización**: Evita duplicar código usando herencia
5. **Especialización**: Las clases hijas pueden agregar características propias

---

## 💡 Ejercicio Mental

Piensa en objetos de la vida real y sus relaciones:
- ¿Un `Gato` es un `Animal`? → ✅ Sí, usa herencia
- ¿Un `Auto` tiene un `Motor`? → ❌ No, usa composición
- ¿Un `Estudiante` es una `Persona`? → ✅ Sí, usa herencia
- ¿Un `Estudiante` tiene un `Libro`? → ❌ No, usa composición

¡Practica identificando estas relaciones antes de programar!
