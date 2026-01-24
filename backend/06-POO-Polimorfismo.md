# POO: Polimorfismo 🎭

## 📑 Índice

1. [¿Qué es el Polimorfismo? (Analogía del Mundo Real)](#qué-es-el-polimorfismo-analogía-del-mundo-real)
2. [Polimorfismo en Programación](#polimorfismo-en-programación)
3. [Polimorfismo con Interfaces](#polimorfismo-con-interfaces)
4. [Polimorfismo con Herencia (Override)](#polimorfismo-con-herencia-override)
5. [`implements` vs `extends`](#implements-vs-extends)
6. [Cuándo Usar Polimorfismo](#cuándo-usar-polimorfismo)
7. [Beneficios del Polimorfismo](#beneficios-del-polimorfismo)
8. [Conceptos Clave](#conceptos-clave)
9. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es el Polimorfismo? (Analogía del Mundo Real)

### 🎨 Polimorfismo = "Muchas Formas"

La palabra "polimorfismo" viene del griego: **"poli"** (muchos) + **"morphos"** (formas) = **"muchas formas"**.

### 🚗 Analogía: Los Vehículos

Imagina que tienes diferentes vehículos:
- **Auto**: Se conduce por carretera
- **Avión**: Se conduce por aire
- **Barco**: Se conduce por agua

**Todos "se conducen"**, pero cada uno lo hace de forma diferente. Eso es polimorfismo: **la misma acción (conducir) se realiza de diferentes formas** según el tipo de vehículo.

### 🐾 Analogía: Los Animales

Piensa en diferentes animales:
- **Perro**: Hace "Guau guau"
- **Gato**: Hace "Miau miau"
- **Pájaro**: Hace "Pío pío"

**Todos "hacen sonido"**, pero cada uno hace un sonido diferente. La acción es la misma (hacer sonido), pero la implementación es diferente.

### 🎵 Analogía: Los Instrumentos Musicales

Imagina diferentes instrumentos:
- **Guitarra**: Se toca con los dedos o púa
- **Piano**: Se toca con los dedos en teclas
- **Batería**: Se toca con baquetas

**Todos "se tocan"**, pero cada uno tiene una forma diferente de tocarse. El concepto es el mismo (tocar un instrumento), pero la técnica es diferente.

### 🍕 Analogía: La Cocina

Piensa en diferentes formas de cocinar:
- **Horno**: Cocina con calor seco
- **Sartén**: Cocina con aceite caliente
- **Olla**: Cocina con agua hirviendo

**Todos "cocinan"**, pero cada método es diferente. El resultado es similar (comida cocida), pero el proceso varía.

---

## Polimorfismo en Programación

### ¿Qué es el Polimorfismo?

El **polimorfismo** permite que **diferentes clases implementen el mismo método de diferentes formas**. 

**En términos simples**: Diferentes objetos pueden responder a la misma "pregunta" o "acción", pero cada uno lo hace a su manera.

### ¿Por qué es Útil?

Imagina que tienes un sistema de dibujo con diferentes formas:
- **Círculo**: Se dibuja con un radio
- **Rectángulo**: Se dibuja con ancho y alto
- **Triángulo**: Se dibuja con tres puntos

**Sin polimorfismo** (complicado):
```typescript
function dibujar(forma: any): void {
    if (forma.tipo === "circulo") {
        // código para círculo
    } else if (forma.tipo === "rectangulo") {
        // código para rectángulo
    } else if (forma.tipo === "triangulo") {
        // código para triángulo
    }
}
```

**Con polimorfismo** (simple):
```typescript
interface Forma {
    dibujar(): void
}

class Circulo implements Forma {
    dibujar(): void {
        // código para círculo
    }
}

class Rectangulo implements Forma {
    dibujar(): void {
        // código para rectángulo
    }
}

// Todas las formas se tratan igual
formas.forEach(forma => forma.dibujar())
```

### El Principio de "Un Solo Interfaz, Múltiples Implementaciones"

El polimorfismo permite que uses **un solo interfaz** (la misma forma de llamar) pero con **múltiples implementaciones** (cada clase lo hace diferente).

---

## Polimorfismo con Interfaces

### Analogía: El Contrato de Trabajo

Imagina que tienes un contrato de trabajo que dice "debes presentarte a trabajar". Diferentes personas cumplen este contrato de diferentes formas:
- **Oficinista**: Va a una oficina
- **Médico**: Va a un hospital
- **Profesor**: Va a una escuela

**Todos cumplen el contrato** (presentarse a trabajar), pero cada uno lo hace en un lugar diferente.

### En Programación

Una **interface** es como un contrato: define qué métodos debe tener una clase, pero no cómo implementarlos.

```typescript
// Interface -> contrato que todas las clases deben cumplir
interface Vehiculo {
    conducir(): void  // Todas deben tener este método
}

// Auto cumple el contrato de su manera
class Auto implements Vehiculo {
    conducir(): void {
        console.log("Conduce por carretera a 120 km/h")
    }
}

// Avión cumple el contrato de su manera
class Avion implements Vehiculo {
    conducir(): void {
        console.log("Vuela por aire a 900 km/h")
    }
}

// Barco cumple el contrato de su manera
class Barco implements Vehiculo {
    conducir(): void {
        console.log("Navega por agua a 50 km/h")
    }
}

// Polimorfismo: todos se tratan igual
let vehiculos: Vehiculo[] = [
    new Auto(),
    new Avion(),
    new Barco()
]

// Cada uno se comporta diferente, pero se llama igual
vehiculos.forEach(v => v.conducir())
// Salida:
// "Conduce por carretera a 120 km/h"
// "Vuela por aire a 900 km/h"
// "Navega por agua a 50 km/h"
```

### Ventajas de Interfaces

1. **Flexibilidad**: Puedes agregar nuevos tipos sin cambiar código existente
2. **Tratamiento uniforme**: Todos los objetos se tratan igual
3. **Fácil de extender**: Agregar un nuevo vehículo es solo crear una nueva clase

---

## Polimorfismo con Herencia (Override)

### Analogía: La Familia y los Talentos

Imagina una familia donde todos tienen el talento de "hacer música":
- **Padre**: Toca guitarra
- **Hijo mayor**: Toca piano (hereda el talento pero lo desarrolla diferente)
- **Hija**: Canta (hereda el talento pero lo desarrolla diferente)

**Todos tienen el mismo talento base** (heredado), pero cada uno lo desarrolla de forma única.

### En Programación: Override (Sobreescritura)

Cuando una clase hija **sobreescribe** un método del padre, está usando polimorfismo.

```typescript
// Clase padre: define el método base
class Animal {
    hacerSonido(): void {
        console.log("hace un sonido")  // Implementación genérica
    }
}

// Clase hija: sobreescribe el método (polimorfismo)
class Perro extends Animal {
    hacerSonido(): void {  // Override: misma firma, diferente implementación
        console.log("Guau guau")
    }
}

// Otra clase hija: también sobreescribe
class Gato extends Animal {
    hacerSonido(): void {  // Override: misma firma, diferente implementación
        console.log("Miau miau")
    }
}

// Polimorfismo: todos son Animal, pero cada uno hace su sonido
let animales: Animal[] = [
    new Perro(),
    new Gato(),
    new Animal()  // Usa la implementación del padre
]

animales.forEach(a => a.hacerSonido())
// Salida:
// "Guau guau"
// "Miau miau"
// "hace un sonido"
```

### ¿Qué es Override?

**Override** (sobreescribir) significa que la clase hija **reemplaza** la implementación del método del padre con su propia implementación.

**Analogía**: Como cuando un hijo aprende a cocinar mejor que su padre - usa la misma receta base, pero la mejora.

---

## `implements` vs `extends`

### Analogía: Contrato vs Herencia Familiar

- **`implements` (Interface)**: Como firmar un contrato - debes cumplir ciertas reglas, pero puedes hacerlo como quieras
- **`extends` (Herencia)**: Como heredar de tu familia - obtienes características y puedes mejorarlas

### `implements` (Interfaces) - Contrato

**Analogía**: Como un contrato de trabajo que dice "debes presentarte a trabajar". No te dice cómo llegar, solo que debes cumplir.

```typescript
// Interface -> contrato sin implementación
interface Vehiculo {
    conducir(): void  // Sin implementación, solo la firma
}

// DEBES implementar el método (es obligatorio)
class Auto implements Vehiculo {
    conducir(): void {  // DEBE estar presente
        console.log("Conduce por carretera")
    }
}
```

**Características**:
- ✅ Define qué métodos debe tener
- ❌ No tiene implementación
- ✅ Obligatorio implementar todos los métodos
- ✅ Una clase puede implementar múltiples interfaces

### `extends` (Herencia) - Herencia Familiar

**Analogía**: Como heredar el talento musical de tu padre. Ya tienes la base, pero puedes mejorarla o cambiarla.

```typescript
// Clase padre -> tiene implementación
class Animal {
    hacerSonido(): void {  // Con implementación
        console.log("hace un sonido")
    }
}

// PUEDES sobreescribir el método (es opcional)
class Perro extends Animal {
    hacerSonido(): void {  // PUEDE sobreescribir (opcional)
        console.log("Guau guau")
    }
    // Si no lo sobreescribes, usa la del padre
}
```

**Características**:
- ✅ Tiene implementación por defecto
- ✅ Puedes sobreescribir (override) si quieres
- ✅ Si no sobreescribes, usa la del padre
- ❌ Una clase solo puede extender una clase padre

### Tabla Comparativa

| Aspecto | `implements` (Interface) | `extends` (Herencia) |
|---------|-------------------------|----------------------|
| **Analogía** | Contrato de trabajo | Herencia familiar |
| **Implementación** | ❌ No tiene | ✅ Tiene por defecto |
| **Obligatorio** | ✅ Debes implementar | ⚠️ Opcional override |
| **Múltiples** | ✅ Puedes implementar varias | ❌ Solo una clase padre |
| **Uso** | Cuando quieres un contrato | Cuando hay jerarquía |

---

## Cuándo Usar Polimorfismo

### ✅ Usa Polimorfismo Cuando:

1. **Tienes objetos que comparten comportamiento pero lo implementan diferente**:
   - Diferentes formas de dibujar
   - Diferentes formas de calcular
   - Diferentes formas de guardar datos

2. **Quieres tratar objetos diferentes de forma uniforme**:
   - Todos los vehículos se "conducen"
   - Todos los animales "hacen sonido"
   - Todas las formas se "dibujan"

3. **Quieres agregar nuevos tipos sin modificar código existente**:
   - Agregar un nuevo vehículo sin cambiar el código que usa vehículos
   - Agregar un nuevo animal sin cambiar el código que maneja animales

### Ejemplo Práctico: Sistema de Pagos

```typescript
interface MetodoPago {
    pagar(monto: number): void
}

class TarjetaCredito implements MetodoPago {
    pagar(monto: number): void {
        console.log(`Pagando $${monto} con tarjeta de crédito`)
    }
}

class PayPal implements MetodoPago {
    pagar(monto: number): void {
        console.log(`Pagando $${monto} con PayPal`)
    }
}

class Transferencia implements MetodoPago {
    pagar(monto: number): void {
        console.log(`Pagando $${monto} por transferencia`)
    }
}

// Polimorfismo: todos se tratan igual
function procesarPago(metodo: MetodoPago, monto: number): void {
    metodo.pagar(monto)  // No importa cuál método sea
}

// Puedes agregar nuevos métodos sin modificar procesarPago
```

---

## Beneficios del Polimorfismo

### 1. Flexibilidad

**Analogía**: Como tener un control remoto universal que funciona con cualquier TV, sin importar la marca.

Puedes agregar nuevos tipos sin cambiar el código que los usa:

```typescript
// Código existente no cambia
function procesarVehiculo(v: Vehiculo): void {
    v.conducir()  // Funciona con cualquier vehículo
}

// Puedes agregar nuevos vehículos
class Bicicleta implements Vehiculo {
    conducir(): void {
        console.log("Pedalea")
    }
}

// El código existente funciona automáticamente
procesarVehiculo(new Bicicleta())  // ✅ Funciona sin cambios
```

### 2. Tratamiento Uniforme

**Analogía**: Como tratar a todos los empleados igual (todos tienen que trabajar), aunque cada uno hace un trabajo diferente.

```typescript
let animales: Animal[] = [new Perro(), new Gato(), new Pajaro()]
animales.forEach(a => a.hacerSonido())  // Todos se tratan igual
```

### 3. Extensibilidad

**Analogía**: Como agregar nuevos canales a tu TV sin cambiar el control remoto.

Puedes extender el sistema fácilmente agregando nuevas clases que implementen la misma interface.

### 4. Código Más Limpio

**Sin polimorfismo** (muchos if/else):
```typescript
function hacerSonido(animal: any): void {
    if (animal.tipo === "perro") {
        console.log("Guau")
    } else if (animal.tipo === "gato") {
        console.log("Miau")
    } else if (animal.tipo === "pajaro") {
        console.log("Pío")
    }
}
```

**Con polimorfismo** (simple):
```typescript
animales.forEach(a => a.hacerSonido())  // Cada uno sabe qué hacer
```

---

## Conceptos Clave

### Términos Importantes

1. **Polimorfismo**: Mismo método, diferentes implementaciones
2. **Interface**: Contrato que define qué métodos debe tener una clase
3. **Override**: Sobreescribir un método de la clase padre
4. **`implements`**: Implementar una interface (obligatorio)
5. **`extends`**: Heredar de una clase (opcional override)

### Resumen Visual

```
┌─────────────────┐
│   Interface     │
│   (Contrato)    │
└────────┬────────┘
         │ implements
         │
    ┌────┴────┬─────────┬─────────┐
    │         │         │         │
┌───▼───┐ ┌──▼───┐ ┌───▼───┐ ┌───▼───┐
│ Clase │ │Clase │ │ Clase │ │ Clase │
│   A   │ │  B   │ │   C   │ │   D   │
└───────┘ └──────┘ └───────┘ └───────┘
    │         │         │         │
    └─────────┴─────────┴─────────┘
              │
         Mismo método
    Diferentes formas
```

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [POO: Herencia](./05-POO-Herencia.md) - Polimorfismo usa herencia con override
- 📚 [POO: Encapsulación](./04-POO-Encapsulacion.md) - Encapsulación ayuda al polimorfismo
- 📚 [TypeScript: Interfaces](./03-TypeScript.md) - Sintaxis de interfaces en TypeScript

### Código Relacionado

- 💻 [Ejemplos de Polimorfismo](../../CODIGO/backend/tema-05-poo-polimorfismo/)

---

## 🎯 Puntos Clave para Recordar

1. **Polimorfismo = "Muchas formas"**: El mismo método se comporta diferente según la clase
2. **Interface = Contrato**: Define qué debe hacer, no cómo
3. **Override = Mejorar**: La clase hija puede mejorar el método del padre
4. **`implements` = Obligatorio**: Debes implementar todos los métodos
5. **`extends` = Opcional**: Puedes sobreescribir si quieres

---

## 💡 Ejercicio Mental

Piensa en acciones que se hacen de diferentes formas:

- **"Comunicarse"**: Hablar, escribir, señas, código morse
- **"Transportar"**: Auto, avión, barco, bicicleta
- **"Cocinar"**: Horno, sartén, olla, parrilla
- **"Iluminar"**: Bombilla, vela, linterna, sol

¡Todos hacen lo mismo (comunicar, transportar, cocinar, iluminar) pero de formas diferentes! Eso es polimorfismo.
