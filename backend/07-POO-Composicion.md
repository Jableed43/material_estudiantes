# POO: Composición 🧩

## ¿Qué es la Composición?

La composición permite construir objetos complejos a partir de objetos más simples, creando relaciones "tiene un" (has-a).

## Composición Básica

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

## Composición vs Herencia

### Herencia (is-a)
```typescript
// "Un Perro ES UN Animal"
class Animal {
    nombre: string
}

class Perro extends Animal {  // Herencia: "es un"
    raza: string
}
```

### Composición (has-a)
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

## Composición con Arrays

```typescript
class Libro {
    titulo: string
}

class Biblioteca {
    private libros: Libro[] = []  // Composición: tiene muchos libros
    
    agregarLibro(libro: Libro): void {
        this.libros.push(libro)
    }
}
```

## ¿Cuándo usar cada una?

**Usar Herencia cuando**:
- ✅ Hay relación "es un" (is-a)
- ✅ Quieres reutilizar código
- ✅ Necesitas polimorfismo
- ✅ Hay jerarquía clara

**Usar Composición cuando**:
- ✅ Hay relación "tiene un" (has-a)
- ✅ Quieres mayor flexibilidad
- ✅ Quieres evitar acoplamiento fuerte
- ✅ Los objetos son independientes

## Beneficios

- ✅ **Flexibilidad**: Cambiar componentes sin afectar el objeto principal
- ✅ **Reutilización**: Construir objetos complejos desde simples
- ✅ **Bajo Acoplamiento**: Los objetos son más independientes

## Conceptos Clave

1. **Composición**: Relación "tiene un" (has-a)
2. **Herencia**: Relación "es un" (is-a)
3. **Flexibilidad**: Composición es más flexible
4. **Acoplamiento**: Composición reduce acoplamiento
5. **Arrays**: Composición puede ser con múltiples objetos

