# POO: Composición 🧩

## 📑 Índice

1. [¿Qué es la Composición? (Analogía del Mundo Real)](#qué-es-la-composición-analogía-del-mundo-real)
2. [Composición en Programación](#composición-en-programación)
3. [Composición Básica](#composición-básica)
4. [Composición vs Herencia](#composición-vs-herencia)
5. [Composición con Arrays](#composición-con-arrays)
6. [Cuándo Usar Cada Una](#cuándo-usar-cada-una)
7. [Beneficios de la Composición](#beneficios-de-la-composición)
8. [Conceptos Clave](#conceptos-clave)
9. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es la Composición? (Analogía del Mundo Real)

### 🧩 Composición = "Construir con Piezas"

La **composición** es construir algo complejo a partir de partes más simples. Es como armar un LEGO: tomas piezas pequeñas y las combinas para crear algo más grande.

### 🏠 Analogía: Construir una Casa

Imagina que estás construyendo una casa:
- **Casa** (objeto complejo)
  - **Tiene** paredes (objeto simple)
  - **Tiene** techo (objeto simple)
  - **Tiene** puertas (objeto simple)
  - **Tiene** ventanas (objeto simple)

**La casa NO es una pared, NO es un techo** - la casa **tiene** paredes, tiene techo. Eso es composición: relación "tiene un" (has-a).

### 🚗 Analogía: El Auto

Piensa en un auto:
- **Auto** (objeto complejo)
  - **Tiene** un motor (objeto simple)
  - **Tiene** ruedas (objeto simple)
  - **Tiene** volante (objeto simple)
  - **Tiene** asientos (objeto simple)

**El auto NO es un motor** - el auto **tiene un motor**. Eso es composición.

### 🎸 Analogía: La Guitarra

Una guitarra:
- **Guitarra** (objeto complejo)
  - **Tiene** cuerdas (objeto simple)
  - **Tiene** mástil (objeto simple)
  - **Tiene** caja de resonancia (objeto simple)

**La guitarra NO es una cuerda** - la guitarra **tiene cuerdas**.

### 🍕 Analogía: La Pizza

Una pizza:
- **Pizza** (objeto complejo)
  - **Tiene** masa (objeto simple)
  - **Tiene** queso (objeto simple)
  - **Tiene** salsa (objeto simple)
  - **Tiene** ingredientes (objetos simples)

**La pizza NO es queso** - la pizza **tiene queso**.

---

## Composición en Programación

### ¿Qué es la Composición?

La **composición** es un mecanismo que permite construir objetos complejos a partir de objetos más simples, creando relaciones **"tiene un"** (has-a).

**En términos simples**: Un objeto está formado por otros objetos. El objeto principal "tiene" o "contiene" otros objetos.

### ¿Por qué es Necesaria?

Imagina que estás creando un sistema para una biblioteca:

**Sin composición** (todo en un solo objeto):
```typescript
class Libro {
    titulo: string
    autorNombre: string
    autorApellido: string
    autorNacionalidad: string
    // ... muchos campos más
}
```

**Con composición** (organizado):
```typescript
class Autor {
    nombre: string
    apellido: string
    nacionalidad: string
}

class Libro {
    titulo: string
    autor: Autor  // El libro TIENE UN autor
}
```

**Ventajas**:
- ✅ Código más organizado
- ✅ Reutilización: el mismo `Autor` puede usarse en múltiples libros
- ✅ Mantenimiento fácil: cambios en `Autor` se reflejan en todos los libros

---

## Composición Básica

### Estructura General

```typescript
// Objeto simple (componente)
class ComponenteSimple {
    // Propiedades del componente
}

// Objeto complejo (compuesto)
class ObjetoComplejo {
    componente: ComponenteSimple  // "tiene un"
    
    constructor(componente: ComponenteSimple) {
        this.componente = componente
    }
}
```

### Ejemplo: Auto con Motor

```typescript
// Objeto simple: Motor
class Motor {
    potencia: number
    tipo: string
    
    constructor(potencia: number, tipo: string) {
        this.potencia = potencia
        this.tipo = tipo
    }
    
    encender(): void {
        console.log("Motor encendido")
    }
}

// Objeto complejo: Auto (tiene un motor)
class Auto {
    marca: string
    modelo: string
    motor: Motor  // Composición: Auto TIENE UN Motor
    
    constructor(marca: string, modelo: string, motor: Motor) {
        this.marca = marca
        this.modelo = modelo
        this.motor = motor  // El auto tiene un motor
    }
    
    arrancar(): void {
        this.motor.encender()  // Usa el motor que tiene
        console.log(`${this.marca} ${this.modelo} arrancó`)
    }
}

// Uso
const motorV8 = new Motor(400, "V8")
const miAuto = new Auto("Ford", "Mustang", motorV8)
miAuto.arrancar()  // Usa el motor que tiene
```

### Ejemplo: Libro con Autor

```typescript
// Objeto simple: Nacionalidad
class Nacionalidad {
    nombre: string
    codPais: string
    
    constructor(nombre: string, codPais: string) {
        this.nombre = nombre
        this.codPais = codPais
    }
}

// Objeto compuesto: Autor (tiene una nacionalidad)
class Autor {
    nombre: string
    nacionalidad: Nacionalidad  // Composición: Autor TIENE UNA Nacionalidad
    
    constructor(nombre: string, nacionalidad: Nacionalidad) {
        this.nombre = nombre
        this.nacionalidad = nacionalidad
    }
}

// Objeto más complejo: Libro (tiene un autor)
class Libro {
    titulo: string
    autor: Autor  // Composición: Libro TIENE UN Autor
    
    constructor(titulo: string, autor: Autor) {
        this.titulo = titulo
        this.autor = autor
    }
    
    mostrarInfo(): void {
        console.log(`${this.titulo} por ${this.autor.nombre} (${this.autor.nacionalidad.nombre})`)
    }
}

// Uso
const argentina = new Nacionalidad("Argentina", "AR")
const borges = new Autor("Jorge Luis Borges", argentina)
const ficciones = new Libro("Ficciones", borges)
ficciones.mostrarInfo()
```

---

## Composición vs Herencia

### La Diferencia Clave

**Herencia**: "**es un**" (is-a)
- Un `Perro` **es un** `Animal`
- Un `Auto` **es un** `Vehículo`

**Composición**: "**tiene un**" (has-a)
- Un `Auto` **tiene un** `Motor`
- Un `Libro` **tiene un** `Autor`

### Analogía: La Diferencia

**Herencia** (es un):
- Un **hijo** **es una** persona (hereda características de persona)
- Un **perro** **es un** animal (hereda características de animal)

**Composición** (tiene un):
- Una **casa** **tiene** paredes (la casa contiene paredes)
- Un **auto** **tiene** un motor (el auto contiene un motor)

### Ejemplo Comparativo

#### Herencia (es un)

```typescript
// "Un Perro ES UN Animal"
class Animal {
    nombre: string
    hacerSonido(): void {
        console.log("hace un sonido")
    }
}

class Perro extends Animal {  // Herencia: "es un"
    raza: string
    // Perro ES UN Animal, hereda todo de Animal
}

const perro = new Perro()
perro.hacerSonido()  // Heredado de Animal
```

#### Composición (tiene un)

```typescript
// "Un Auto TIENE UNA Marca"
class Marca {
    nombre: string
    pais: string
}

class Auto {
    modelo: string
    marca: Marca  // Composición: "tiene un"
    // Auto TIENE UNA Marca, pero NO ES UNA Marca
}

const toyota = new Marca("Toyota", "Japón")
const auto = new Auto("Corolla", toyota)
// auto NO es una marca, auto TIENE una marca
```

### ¿Cómo Decidir?

**Pregúntate**: ¿La relación es "es un" o "tiene un"?

- **"Un X es un Y"** → Usa **Herencia**
  - Un Perro es un Animal ✅
  - Un Auto es un Vehículo ✅
  - Un Estudiante es una Persona ✅

- **"Un X tiene un Y"** → Usa **Composición**
  - Un Auto tiene un Motor ✅
  - Un Estudiante tiene un Libro ✅
  - Un Libro tiene un Autor ✅

---

## Composición con Arrays

### Analogía: La Biblioteca

Una biblioteca:
- **Biblioteca** (objeto complejo)
  - **Tiene** muchos libros (array de objetos simples)

**La biblioteca NO es un libro** - la biblioteca **tiene muchos libros**.

### En Programación

```typescript
// Objeto simple: Libro
class Libro {
    titulo: string
    autor: string
    
    constructor(titulo: string, autor: string) {
        this.titulo = titulo
        this.autor = autor
    }
}

// Objeto complejo: Biblioteca (tiene muchos libros)
class Biblioteca {
    nombre: string
    private libros: Libro[] = []  // Composición: tiene muchos libros
    
    constructor(nombre: string) {
        this.nombre = nombre
    }
    
    agregarLibro(libro: Libro): void {
        this.libros.push(libro)  // Agrega un libro a la colección
    }
    
    listarLibros(): void {
        console.log(`Libros en ${this.nombre}:`)
        this.libros.forEach(libro => {
            console.log(`- ${libro.titulo} por ${libro.autor}`)
        })
    }
}

// Uso
const biblioteca = new Biblioteca("Biblioteca Central")
biblioteca.agregarLibro(new Libro("Cien años de soledad", "García Márquez"))
biblioteca.agregarLibro(new Libro("Ficciones", "Borges"))
biblioteca.listarLibros()
```

### Ejemplo: Carrito de Compras

```typescript
// Objeto simple: Producto
class Producto {
    nombre: string
    precio: number
    
    constructor(nombre: string, precio: number) {
        this.nombre = nombre
        this.precio = precio
    }
}

// Objeto complejo: Carrito (tiene muchos productos)
class Carrito {
    private productos: Producto[] = []  // Composición: tiene muchos productos
    
    agregarProducto(producto: Producto): void {
        this.productos.push(producto)
    }
    
    calcularTotal(): number {
        return this.productos.reduce((total, producto) => {
            return total + producto.precio
        }, 0)
    }
}

// Uso
const carrito = new Carrito()
carrito.agregarProducto(new Producto("Libro", 20))
carrito.agregarProducto(new Producto("Lápiz", 2))
console.log(`Total: $${carrito.calcularTotal()}`)
```

---

## Cuándo Usar Cada Una

### ✅ Usar Herencia Cuando:

1. **Hay relación "es un" (is-a)**:
   - Un `Perro` **es un** `Animal` ✅
   - Un `Auto` **es un** `Vehículo` ✅
   - Un `Estudiante` **es una** `Persona` ✅

2. **Hay código común que se reutiliza**:
   - Varias clases comparten métodos y propiedades
   - Puedes extraer lo común a una clase padre

3. **Hay jerarquía lógica**:
   - Las clases forman una jerarquía natural
   - Las clases hijas son "tipos especiales" de la clase padre

### ✅ Usar Composición Cuando:

1. **Hay relación "tiene un" (has-a)**:
   - Un `Auto` **tiene un** `Motor` ✅
   - Un `Estudiante` **tiene un** `Libro` ✅
   - Un `Libro` **tiene un** `Autor` ✅

2. **Quieres mayor flexibilidad**:
   - Puedes cambiar componentes sin afectar el objeto principal
   - Los objetos son más independientes

3. **Quieres evitar acoplamiento fuerte**:
   - El objeto principal no depende fuertemente de los componentes
   - Puedes intercambiar componentes fácilmente

4. **Los objetos son independientes**:
   - Un `Motor` puede existir sin un `Auto`
   - Un `Autor` puede existir sin un `Libro`

### Ejemplo: ¿Herencia o Composición?

**Escenario 1**: Sistema de vehículos
- `Auto` **es un** `Vehículo` → **Herencia** ✅
- `Auto` **tiene un** `Motor` → **Composición** ✅

```typescript
// Herencia
class Vehiculo { }
class Auto extends Vehiculo { }  // Auto ES UN Vehículo

// Composición
class Motor { }
class Auto {
    motor: Motor  // Auto TIENE UN Motor
}
```

**Escenario 2**: Sistema de biblioteca
- `Libro` **tiene un** `Autor` → **Composición** ✅
- `LibroDigital` **es un** `Libro` → **Herencia** ✅

```typescript
// Composición
class Autor { }
class Libro {
    autor: Autor  // Libro TIENE UN Autor
}

// Herencia
class Libro { }
class LibroDigital extends Libro { }  // LibroDigital ES UN Libro
```

---

## Beneficios de la Composición

### 1. Flexibilidad

**Analogía**: Como cambiar las ruedas de un auto sin cambiar todo el auto.

```typescript
class Motor {
    potencia: number
}

class Auto {
    motor: Motor  // Puedes cambiar el motor fácilmente
    
    cambiarMotor(nuevoMotor: Motor): void {
        this.motor = nuevoMotor  // Cambias el componente sin afectar el auto
    }
}
```

### 2. Reutilización

**Analogía**: Como usar el mismo motor en diferentes autos.

```typescript
const motorV8 = new Motor(400, "V8")

const auto1 = new Auto("Ford", "Mustang", motorV8)
const auto2 = new Auto("Chevrolet", "Camaro", motorV8)  // Mismo motor
```

### 3. Bajo Acoplamiento

**Analogía**: Como tener piezas intercambiables - si una se rompe, la cambias sin afectar el resto.

Los objetos son más independientes:
- Un `Motor` puede existir sin un `Auto`
- Un `Autor` puede existir sin un `Libro`

### 4. Fácil de Testear

**Analogía**: Como probar cada pieza del auto por separado antes de armarlo.

Puedes probar cada componente por separado:
```typescript
// Pruebas el motor por separado
const motor = new Motor(400, "V8")
motor.encender()  // Prueba independiente

// Pruebas el auto por separado
const auto = new Auto("Ford", "Mustang", motor)
auto.arrancar()  // Prueba del auto completo
```

---

## Conceptos Clave

### Términos Importantes

1. **Composición**: Relación "tiene un" (has-a)
2. **Herencia**: Relación "es un" (is-a)
3. **Componente**: Objeto simple que forma parte de otro
4. **Compuesto**: Objeto complejo formado por componentes
5. **Acoplamiento**: Grado de dependencia entre objetos

### Resumen Visual

```
┌─────────────┐
│  Componente │
│   Simple    │
└──────┬──────┘
       │
       │ "tiene un"
       │
┌──────▼──────┐
│  Objeto     │
│  Complejo   │
│  (Compuesto)│
└─────────────┘
```

### Regla de Oro

**Pregúntate**: ¿Es "es un" o "tiene un"?
- **"es un"** → Herencia
- **"tiene un"** → Composición

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [POO: Herencia](./05-POO-Herencia.md) - Alternativa a composición cuando hay relación "es un"
- 📚 [POO: Encapsulación](./04-POO-Encapsulacion.md) - Encapsulación ayuda a la composición
- 📚 [POO: Polimorfismo](./06-POO-Polimorfismo.md) - Composición puede usarse con polimorfismo

### Código Relacionado

- 💻 [Ejemplos de Composición](../../CODIGO/backend/tema-06-poo-composicion/)

---

## 🎯 Puntos Clave para Recordar

1. **Composición = "tiene un"**: Un objeto contiene otros objetos
2. **Herencia = "es un"**: Un objeto es un tipo de otro objeto
3. **Flexibilidad**: Composición permite cambiar componentes fácilmente
4. **Reutilización**: Los componentes pueden usarse en múltiples objetos
5. **Pregunta clave**: ¿"es un" o "tiene un"?

---

## 💡 Ejercicio Mental

Piensa en objetos de la vida real y sus relaciones:

**Herencia (es un)**:
- ¿Un `Gato` es un `Animal`? → ✅ Sí, herencia
- ¿Un `Auto` es un `Vehículo`? → ✅ Sí, herencia
- ¿Un `Estudiante` es una `Persona`? → ✅ Sí, herencia

**Composición (tiene un)**:
- ¿Un `Auto` tiene un `Motor`? → ✅ Sí, composición
- ¿Un `Estudiante` tiene un `Libro`? → ✅ Sí, composición
- ¿Una `Casa` tiene `Paredes`? → ✅ Sí, composición

**¡Practica identificando estas relaciones antes de programar!**
