# POO: Polimorfismo 🎭

## ¿Qué es el Polimorfismo?

El polimorfismo permite que diferentes clases implementen el mismo método de diferentes formas.

## Polimorfismo con Interfaces

```typescript
// Interface -> contrato
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
    conducir() {
        console.log("Conduce por aire")
    }
}

// Todas pueden tratarse como Vehiculo
let vehiculos: Vehiculo[] = [
    new Auto(),
    new Avion()
]

vehiculos.forEach(v => v.conducir())  // Cada uno se comporta diferente
```

## Polimorfismo con Herencia (Override)

```typescript
// Clase padre
class Animal {
    hacerSonido(): void {
        console.log("hace un sonido")
    }
}

// Clases hijas sobreescriben el método
class Perro extends Animal {
    hacerSonido(): void {  // Override
        console.log("Guau guau")
    }
}

class Gato extends Animal {
    hacerSonido(): void {  // Override
        console.log("Miau miau")
    }
}

let animales: Animal[] = [new Perro(), new Gato()]
animales.forEach(a => a.hacerSonido())  // Cada uno hace su sonido
```

## `implements` vs `extends`

### `implements` (Interfaces)
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

### `extends` (Clases)
```typescript
class Animal {
    hacerSonido(): void {  // Con implementación
        console.log("hace un sonido")
    }
}

class Perro extends Animal {
    hacerSonido(): void {  // PUEDE sobreescribir
        console.log("Guau guau")
    }
}
```

## Beneficios

- ✅ **Flexibilidad**: Agregar nuevas clases sin modificar código existente
- ✅ **Tratamiento Uniforme**: Objetos diferentes pueden tratarse igual
- ✅ **Extensibilidad**: Fácil agregar nuevos comportamientos

## Conceptos Clave

1. **Polimorfismo**: Mismo método, diferentes implementaciones
2. **Interface**: Contrato sin implementación
3. **Override**: Sobreescribir método de clase padre
4. **implements**: Implementar interface (obligatorio)
5. **extends**: Heredar de clase (opcional override)

