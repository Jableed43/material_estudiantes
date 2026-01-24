# POO: Herencia 👨‍👩‍👧

## ¿Qué es la Herencia?

La herencia permite que una clase hija herede propiedades y métodos de una clase padre.

## Sintaxis

```typescript
// Clase padre
class CuerpoCeleste {
    public nombre: string
    private codigo: string
    
    constructor(nombre: string, codigo: string) {
        this.nombre = nombre
        this.codigo = codigo
    }
}

// Clase hija
class Planeta extends CuerpoCeleste {
    esHabitable: boolean
    
    constructor(nombre: string, codigo: string, esHabitable: boolean) {
        super(nombre, codigo)  // Llama al constructor padre
        this.esHabitable = esHabitable
    }
}
```

## Palabra clave `super`

```typescript
class Planeta extends CuerpoCeleste {
    constructor(nombre: string, codigo: string) {
        super(nombre, codigo)  // Debe ser la primera línea
        // Asigna datos a variables heredadas
    }
}
```

## Modificadores de Acceso en Herencia

```typescript
class Padre {
    public publico: string        // ✅ Accesible
    protected protegido: string   // ✅ Accesible desde clase hija
    private privado: string      // ❌ No accesible
}

class Hijo extends Padre {
    constructor() {
        super()
        this.publico = "accesible"      // ✅ Funciona
        this.protegido = "accesible"    // ✅ Funciona
        // this.privado = "error"       // ❌ Error
    }
}
```

## Herencia Múltiple

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

## Beneficios

- ✅ **Reutilización**: Reutilizar código de la clase padre
- ✅ **Jerarquía**: Organizar código de forma lógica
- ✅ **Especialización**: Clases hijas pueden especializarse

## Conceptos Clave

1. **extends**: Sintaxis para herencia
2. **super**: Llama al constructor padre
3. **Public**: Heredable y accesible
4. **Protected**: Heredable pero no accesible desde fuera
5. **Private**: No heredable
6. **Herencia Múltiple**: Una clase puede heredar de otra que hereda de otra

