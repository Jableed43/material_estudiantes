# POO: Encapsulación 🔒

## 📑 Índice

1. [¿Qué es la Encapsulación? (Analogía del Mundo Real)](#qué-es-la-encapsulación-analogía-del-mundo-real)
2. [Encapsulación en Programación](#encapsulación-en-programación)
3. [Modificadores de Acceso](#modificadores-de-acceso)
4. [Getters y Setters](#getters-y-setters)
5. [Métodos Privados](#métodos-privados)
6. [¿Por qué es Importante?](#por-qué-es-importante)
7. [Beneficios de la Encapsulación](#beneficios-de-la-encapsulación)
8. [Conceptos Clave](#conceptos-clave)
9. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es la Encapsulación? (Analogía del Mundo Real)

### 🏠 Analogía: Tu Casa

Imagina tu casa. Tienes diferentes niveles de acceso:

- **Jardín (público)**: Cualquiera puede verlo desde la calle
- **Sala (público)**: Los invitados pueden entrar
- **Cocina (protegido)**: Solo familia puede entrar
- **Habitación (privado)**: Solo tú puedes entrar

**La encapsulación es como tener control sobre quién puede acceder a qué partes de tu casa.**

### 🚗 Analogía: El Auto

Piensa en un auto. Tiene:

- **Volante, pedales (público)**: El conductor los usa directamente
- **Motor (protegido)**: No lo tocas directamente, pero el auto lo usa internamente
- **Sistema de combustible (privado)**: Está completamente oculto, solo el auto lo maneja

**No puedes poner gasolina directamente en el motor** - tienes que usar el tanque (interfaz pública). El auto se encarga internamente de llevarla al motor.

### 🏦 Analogía: El Banco

En un banco:
- **Cajero automático (público)**: Todos pueden usarlo para retirar dinero
- **Bóveda (privado)**: Solo empleados autorizados pueden acceder
- **Sistema interno (privado)**: El banco maneja las transacciones internamente

**No puedes entrar directamente a la bóveda** - debes usar el cajero (interfaz pública) que internamente accede a la bóveda de forma segura.

---

## Encapsulación en Programación

### ¿Qué es la Encapsulación?

La **encapsulación** es el principio de **ocultar los detalles internos** de un objeto y **exponer solo lo necesario** a través de una interfaz controlada.

**En términos simples**: Es como tener una "cápsula" que protege lo que está adentro. Solo puedes interactuar con ella a través de los "botones" o "controles" que expone.

### ¿Por qué es Necesaria?

Imagina que tienes una clase `CuentaBancaria`:

**Sin encapsulación** (peligroso):
```typescript
class CuentaBancaria {
    saldo: number  // Cualquiera puede modificarlo directamente
}

const cuenta = new CuentaBancaria()
cuenta.saldo = 1000000  // ❌ ¡Alguien puede poner cualquier cantidad!
cuenta.saldo = -500     // ❌ ¡Puede tener saldo negativo!
```

**Con encapsulación** (seguro):
```typescript
class CuentaBancaria {
    private _saldo: number  // Privado, no accesible directamente
    
    public retirar(cantidad: number): void {
        if (cantidad > 0 && cantidad <= this._saldo) {
            this._saldo -= cantidad
        } else {
            throw new Error("Cantidad inválida")
        }
    }
}
```

### El Principio de "Caja Negra"

La encapsulación convierte tu objeto en una **"caja negra"**:
- **Desde afuera**: Solo ves los controles (métodos públicos)
- **Por dentro**: Tiene su lógica interna (propiedades privadas)
- **No necesitas saber** cómo funciona por dentro, solo cómo usarlo

---

## Modificadores de Acceso

### Los Tres Niveles de Acceso

Imagina una casa con tres niveles de seguridad:

#### 1. `public` - Público (Jardín)

**Analogía**: El jardín de tu casa - cualquiera puede verlo y acceder.

```typescript
class Planeta {
    public nombre: string  // Todos pueden ver y modificar
    
    constructor(nombre: string) {
        this.nombre = nombre
    }
}

const tierra = new Planeta("Tierra")
console.log(tierra.nombre)  // ✅ Cualquiera puede acceder
tierra.nombre = "Marte"     // ✅ Cualquiera puede modificar
```

**Cuándo usar**: Para información que no necesita protección.

#### 2. `private` - Privado (Habitación)

**Analogía**: Tu habitación - solo tú puedes entrar.

```typescript
class Planeta {
    public nombre: string
    private _masaKg: number  // Solo la clase misma puede acceder
    
    constructor(nombre: string, masaKg: number) {
        this.nombre = nombre
        this._masaKg = masaKg
    }
    
    public getMasa(): number {
        return this._masaKg  // ✅ La clase puede acceder a sus privados
    }
}

const tierra = new Planeta("Tierra", 5972000000)
console.log(tierra.nombre)        // ✅ Funciona (público)
// console.log(tierra._masaKg)     // ❌ Error (privado)
console.log(tierra.getMasa())      // ✅ Funciona (método público)
```

**Cuándo usar**: Para datos sensibles que no deben modificarse directamente.

#### 3. `protected` - Protegido (Cocina Familiar)

**Analogía**: La cocina - solo la familia puede entrar (padres e hijos).

```typescript
class CuerpoCeleste {
    protected codigo: string  // Accesible desde clase y clases hijas
    
    constructor(codigo: string) {
        this.codigo = codigo
    }
}

class Planeta extends CuerpoCeleste {
    public mostrarCodigo(): string {
        return this.codigo  // ✅ Funciona (es clase hija)
    }
}

const tierra = new Planeta("TER-001")
// console.log(tierra.codigo)        // ❌ Error (no es público)
console.log(tierra.mostrarCodigo())  // ✅ Funciona
```

**Cuándo usar**: Cuando quieres que las clases hijas accedan, pero no desde fuera.

### Tabla Comparativa

| Modificador | Acceso desde | Analogía | Ejemplo |
|-------------|--------------|----------|---------|
| `public` | Cualquier lugar | Jardín público | `nombre` de un planeta |
| `protected` | Clase y clases hijas | Cocina familiar | `codigo` interno |
| `private` | Solo la clase misma | Habitación privada | `_saldo` de cuenta bancaria |

### Convención: El Prefijo `_`

Por convención, las propiedades privadas suelen empezar con `_`:

```typescript
class Planeta {
    private _masaKg: number      // ✅ Convención: _ al inicio
    private _radioKm: number     // ✅ Indica que es privado
}
```

Esto ayuda a identificar rápidamente qué es privado en el código.

---

## Getters y Setters

### Analogía: El Control Remoto

Imagina que tienes un televisor:
- **No puedes cambiar el canal directamente** tocando los circuitos internos
- **Usas el control remoto** (interfaz pública) para cambiar el canal
- **El control remoto valida** que el canal exista antes de cambiarlo

**Getters y Setters son como el control remoto** - te permiten acceder y modificar datos de forma controlada.

### ¿Qué son Getters y Setters?

- **Getter**: Método para **obtener** un valor (como leer)
- **Setter**: Método para **asignar** un valor con validación (como escribir con control)

### Ejemplo: Planeta con Masa Protegida

```typescript
class Planeta {
    private _masaKg: number  // Privado - no accesible directamente
    
    // GETTER: Obtener el valor
    public get masaKg(): number {
        return this._masaKg
    }
    
    // SETTER: Asignar valor con validación
    public set masaKg(nuevaMasa: number) {
        if (nuevaMasa <= 0) {
            throw new Error("La masa debe ser mayor a 0")
        }
        if (nuevaMasa > 1000000000) {
            throw new Error("La masa es demasiado grande")
        }
        this._masaKg = nuevaMasa
    }
}

// Uso
const saturno = new Planeta()
saturno.masaKg = 250000  // ✅ Usa setter (valida y asigna)
console.log(saturno.masaKg)  // ✅ Usa getter (obtiene valor)

// saturno.masaKg = -100  // ❌ Error: setter valida y rechaza
// saturno._masaKg = 250000  // ❌ Error: _masaKg es privado
```

### Analogía: La Caja Fuerte

Imagina una caja fuerte:
- **No puedes abrirla directamente** (propiedad privada)
- **Usas una llave** (getter) para ver qué hay dentro
- **Usas una combinación** (setter) para guardar algo, pero solo si cumple las reglas

### Ventajas de Getters y Setters

1. **Validación**: Puedes validar datos antes de asignarlos
2. **Control**: Sabes cuándo se lee o escribe un valor
3. **Flexibilidad**: Puedes cambiar la implementación interna sin afectar el código externo
4. **Seguridad**: Proteges datos sensibles

### Ejemplo Completo: Cuenta Bancaria

```typescript
class CuentaBancaria {
    private _saldo: number = 0
    
    // Getter
    public get saldo(): number {
        return this._saldo
    }
    
    // Setter con validación
    public set saldo(nuevoSaldo: number) {
        if (nuevoSaldo < 0) {
            throw new Error("El saldo no puede ser negativo")
        }
        this._saldo = nuevoSaldo
    }
    
    // Métodos públicos para operaciones
    public depositar(cantidad: number): void {
        if (cantidad > 0) {
            this._saldo += cantidad
        }
    }
    
    public retirar(cantidad: number): boolean {
        if (cantidad > 0 && cantidad <= this._saldo) {
            this._saldo -= cantidad
            return true
        }
        return false
    }
}

const cuenta = new CuentaBancaria()
cuenta.depositar(1000)
console.log(cuenta.saldo)  // 1000
cuenta.retirar(300)
console.log(cuenta.saldo)  // 700
// cuenta.saldo = -100  // ❌ Error: setter rechaza valores negativos
```

---

## Métodos Privados

### Analogía: Procesos Internos

Imagina una fábrica:
- **Público**: La tienda donde compras (métodos públicos)
- **Privado**: Los procesos internos de fabricación (métodos privados)

Los clientes no necesitan saber cómo se fabrica el producto, solo cómo comprarlo.

### Métodos Privados en Código

```typescript
class Planeta {
    private _masaKg: number
    
    // Método privado - solo la clase puede usarlo
    private calcularGravedad(): number {
        // Cálculo complejo interno
        return this._masaKg * 9.8
    }
    
    // Método público - cualquiera puede usarlo
    public obtenerGravedad(): number {
        return this.calcularGravedad()  // ✅ Puede usar métodos privados
    }
    
    public describir(): string {
        const gravedad = this.calcularGravedad()  // ✅ También aquí
        return `Planeta con gravedad ${gravedad}`
    }
}

const tierra = new Planeta()
console.log(tierra.obtenerGravedad())  // ✅ Funciona
// tierra.calcularGravedad()           // ❌ Error: método privado
```

### ¿Por qué Métodos Privados?

1. **Ocultar complejidad**: Los detalles internos no son relevantes para quien usa la clase
2. **Mantenibilidad**: Puedes cambiar la implementación interna sin afectar código externo
3. **Organización**: Separas lo que es "interno" de lo que es "público"

### Ejemplo: Validación Interna

```typescript
class Usuario {
    private _email: string
    
    // Método privado para validar email
    private validarEmail(email: string): boolean {
        return email.includes("@") && email.includes(".")
    }
    
    // Setter público que usa validación privada
    public set email(nuevoEmail: string) {
        if (this.validarEmail(nuevoEmail)) {
            this._email = nuevoEmail
        } else {
            throw new Error("Email inválido")
        }
    }
}
```

---

## ¿Por qué es Importante?

### Problema Sin Encapsulación

Imagina una clase `Auto` sin encapsulación:

```typescript
class Auto {
    velocidad: number
    combustible: number
}

const miAuto = new Auto()
miAuto.velocidad = 1000  // ❌ Velocidad imposible
miAuto.combustible = -50  // ❌ Combustible negativo
```

**Problemas**:
- Datos inválidos pueden entrar
- No hay control sobre los cambios
- Código difícil de mantener

### Solución Con Encapsulación

```typescript
class Auto {
    private _velocidad: number = 0
    private _combustible: number = 0
    
    public acelerar(): void {
        if (this._combustible > 0) {
            this._velocidad = Math.min(this._velocidad + 10, 120)  // Máximo 120
            this._combustible -= 1
        }
    }
    
    public repostar(cantidad: number): void {
        if (cantidad > 0) {
            this._combustible = Math.min(this._combustible + cantidad, 50)  // Máximo 50
        }
    }
}
```

**Ventajas**:
- ✅ Datos siempre válidos
- ✅ Control total sobre cambios
- ✅ Código más seguro y mantenible

---

## Beneficios de la Encapsulación

### 1. Protección de Datos

**Analogía**: Como una caja fuerte que protege tus objetos valiosos.

```typescript
class CuentaBancaria {
    private _saldo: number  // Protegido - no puede modificarse directamente
}
```

### 2. Control de Acceso

**Analogía**: Como un portero que controla quién entra al edificio.

```typescript
class Planeta {
    private _masaKg: number
    
    public set masaKg(valor: number) {
        if (valor > 0) {  // El "portero" valida antes de permitir
            this._masaKg = valor
        }
    }
}
```

### 3. Mantenibilidad

**Analogía**: Como cambiar el motor de un auto sin cambiar todo el auto.

```typescript
// Puedes cambiar la implementación interna
// sin afectar el código que usa la clase
class Planeta {
    private _masaKg: number
    
    // Cambias esto internamente
    public get masaKg(): number {
        return this._masaKg * 1000  // Ahora retorna en gramos
    }
    // El código externo sigue funcionando igual
}
```

### 4. Flexibilidad

Puedes cambiar cómo funciona internamente sin romper código existente.

---

## Conceptos Clave

### Resumen de Modificadores

1. **`public`**: Accesible desde cualquier lugar (por defecto en JavaScript/TypeScript)
2. **`private`**: Solo accesible desde dentro de la clase
3. **`protected`**: Accesible desde clase y clases hijas
4. **Getter (`get`)**: Obtener valor (se accede como propiedad)
5. **Setter (`set`)**: Asignar valor con validación
6. **Convención `_`**: Prefijo para propiedades privadas

### Reglas Importantes

- ✅ Usa `private` para datos sensibles
- ✅ Usa `public` para interfaces que otros necesitan
- ✅ Usa `protected` cuando las clases hijas necesitan acceso
- ✅ Valida datos en setters
- ✅ Usa getters/setters para controlar acceso

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [POO: Herencia](./05-POO-Herencia.md) - Usa `protected` en herencia
- 📚 [POO: Polimorfismo](./06-POO-Polimorfismo.md) - Encapsulación ayuda al polimorfismo
- 📚 [TypeScript: Introducción](./03-TypeScript.md) - Sintaxis TypeScript para modificadores

### Código Relacionado

- 💻 [Ejemplos de Encapsulación](../../CODIGO/backend/tema-03-poo-encapsulacion/)

---

## 🎯 Puntos Clave para Recordar

1. **Encapsulación = Protección**: Como una caja fuerte que protege tus datos
2. **`private` para datos sensibles**: No dejes que cualquiera modifique datos importantes
3. **Getters/Setters para control**: Valida antes de asignar
4. **Métodos privados para lógica interna**: Oculta la complejidad
5. **Piensa en niveles de acceso**: Público, protegido, privado - como una casa

---

## 💡 Ejercicio Mental

Piensa en objetos de la vida real y qué debería ser público, protegido o privado:

- **Auto**: 
  - Público: acelerar, frenar
  - Privado: presión de los pistones, temperatura del motor
  
- **Cuenta Bancaria**:
  - Público: depositar, retirar
  - Privado: saldo interno, número de cuenta completo

- **Teléfono**:
  - Público: llamar, enviar mensaje
  - Privado: señal de radio, frecuencia de operación

¡Practica identificando qué debe ser público y qué privado!
