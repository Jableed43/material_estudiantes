# POO: Encapsulación 🔒

## ¿Qué es la Encapsulación?

La encapsulación oculta los detalles internos del objeto y expone solo lo necesario a través de métodos públicos.

## Modificadores de Acceso

```typescript
class Planeta {
    public nombre: string        // Accesible desde cualquier lugar
    private _masaKg: number      // Solo accesible desde dentro de la clase
    protected radioKm: number    // Accesible desde clase y clases hijas
}
```

## Getters y Setters

```typescript
class Planeta {
    private _masaKg: number
    
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

const saturno = new Planeta()
saturno.masaKg = 250000  // Usa setter
console.log(saturno.masaKg)  // Usa getter
```

## Métodos Privados

```typescript
class Planeta {
    private metodoPrivado(): void {
        console.log("Soy un método interno")
    }
    
    public metodoPublico(): void {
        this.metodoPrivado()  // ✅ Puede usar métodos privados
    }
}

// saturno.metodoPrivado()  // ❌ Error: No accesible desde afuera
```

## Beneficios

- ✅ **Protección de Datos**: Mantiene integridad de los datos
- ✅ **Control de Acceso**: Valida datos antes de asignarlos
- ✅ **Mantenibilidad**: Cambiar detalles internos sin afectar código externo

## Conceptos Clave

1. **Public**: Accesible desde cualquier lugar (por defecto)
2. **Private**: Solo accesible desde dentro de la clase
3. **Protected**: Accesible desde clase y clases hijas
4. **Getter**: Obtener valor (se accede como propiedad)
5. **Setter**: Asignar valor con validación
6. **Convención**: Usar `_` al inicio para propiedades privadas

