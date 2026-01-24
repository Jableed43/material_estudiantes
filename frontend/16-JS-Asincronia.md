# Master Guide: Asincronía en JavaScript ⏳

## 1. Introducción a la Asincronía

### ¿Qué es la Asincronía?

La **asincronía en programación** se refiere a la capacidad de un programa para ejecutar operaciones que **pueden tardar en completarse** (como la lectura de archivos o llamadas a APIs), **sin bloquear el flujo principal de ejecución**. Esto permite que otras operaciones continúen mientras se espera la finalización de estas tareas.

En JavaScript, la asincronía es fundamental para manejar operaciones de E/S (entrada/salida), como:
- 📡 **Peticiones a servidores** (APIs)
- 📁 **Lectura de archivos**
- 🖼️ **Carga de imágenes**
- ⏱️ **Operaciones que requieren tiempo**

### ¿Por qué JavaScript es Mono-hilo?

JavaScript es **mono-hilo** (ejecuta una cosa a la vez). La asincronía permite que tareas lentas (APIs, archivos) **no congelen la pantalla** ni bloqueen la aplicación.

**Sin programación asíncrona:**
```javascript
// ❌ MALO: Bloquea toda la aplicación
const datos = obtenerDatosDelServidor(); // La app se congela aquí
console.log("Esto nunca se ejecuta hasta que termine la petición");
```

**Con programación asíncrona:**
```javascript
// ✅ BUENO: No bloquea la aplicación
obtenerDatosDelServidor().then(datos => {
    console.log("Datos recibidos:", datos);
});
console.log("Esto se ejecuta inmediatamente");
```

### Sincronía vs. Asincronía

**Código Síncrono (Bloqueante)**:
- Se ejecuta línea por línea, en orden
- Cada operación espera a que la anterior termine
- Bloquea la ejecución hasta completarse
- Simple pero puede congelar la aplicación

**Código Asíncrono (No Bloqueante)**:
- Las operaciones se ejecutan sin bloquear
- El programa continúa mientras espera resultados
- Mejor experiencia de usuario
- Más complejo de manejar

---

## 2. Callbacks 📞

### ¿Qué es un Callback?

Un **callback** es una función que se pasa como argumento a otra función y se ejecuta **después** de que la operación principal se haya completado. Fueron la primera solución para manejar la asincronía en JavaScript.

### Sintaxis Básica

```javascript
function hacerAlgo(callback) {
    // Operación que toma tiempo
    setTimeout(() => {
        callback("Operación completada");
    }, 1000);
}

hacerAlgo((resultado) => {
    console.log(resultado); // "Operación completada"
});
```

### ¿Por qué usar Callbacks?

**Ventajas**:
1. **Principio de Responsabilidad Única (SOLID)**: Cada componente tiene su propia responsabilidad
   - Función `cocinar` tiene responsabilidad de recibir ingredientes con una receta y devolver un plato cocinado
   - Función `lavar` tiene como responsabilidad recibir utensilios, vajillas, ollas sucias y devolver estos elementos limpios
2. **Reutilización de código**: Permite reutilizar funciones como `lavar` en diferentes contextos
3. **Compatibilidad**: Presente desde el inicio de JavaScript

**Ejemplo del material del docente**:
```javascript
function cocinar(ingredientes, receta, lavar) {
    const ingredientesCortados = preparar(ingredientes)
    const resultado = receta(ingredientesCortados)
    // Resultado por dentro -> {comida, platosSucios}
    const platosLimpios = lavar(resultado.platosSucios)
    return {comida, platosLimpios}
}
```

### Problema: Callback Hell

**¿Qué es?**: Cuando se anidan múltiples callbacks, el código se vuelve difícil de leer y mantener.

```javascript
// ❌ Callback Hell - Difícil de leer
obtenerUsuario(id, (usuario) => {
    obtenerPosts(usuario.id, (posts) => {
        obtenerComentarios(posts[0].id, (comentarios) => {
            obtenerLikes(comentarios[0].id, (likes) => {
                console.log(likes); // 😵 Código anidado profundamente
            });
        });
    });
});
```

**Desventajas**:
- ❌ Código difícil de leer y mantener
- ❌ Manejo de errores complicado
- ❌ Anidamiento excesivo (pirámide del nesting)
- ❌ Difícil depurar

---

## 3. Promesas (Promises) 🤝

### ¿Qué es una Promesa?

Una **Promesa** es un objeto que representa el resultado de una operación asíncrona. Es como un "vale" que te promete que en el futuro tendrás un resultado. Una promesa es un objeto que representa un valor que puede estar disponible ahora, en el futuro o nunca.

### Estados de una Promesa

Una promesa puede estar en uno de estos estados:

1. **🔄 PENDING (Pendiente)**: La operación está en progreso, aún no ha finalizado
2. **✅ FULFILLED (Cumplida/Resuelta)**: La operación se completó con éxito y devuelve un resultado
3. **❌ REJECTED (Rechazada)**: La operación falló y devuelve un error

```javascript
// Crear una promesa
const miPromesa = new Promise((resolve, reject) => {
    // resolve() cambia el estado a FULFILLED
    // reject() cambia el estado a REJECTED
});
```

### Crear una Promesa

**Estructura Básica**:
```javascript
function miOperacionAsincronica() {
    return new Promise((resolve, reject) => {
        // Tu código aquí
        
        if (todoSalioBien) {
            resolve("Resultado exitoso");
        } else {
            reject("Algo salió mal");
        }
    });
}
```

**Ejemplo Práctico del Código Modelo**:
```javascript
function operacionAsincronica(simularExito = true) {
    return new Promise((resolve, reject) => {
        // Simulamos una operación que toma tiempo (como una petición HTTP)
        setTimeout(() => {
            if (simularExito) {
                resolve("✅ La operación fue exitosa");
            } else {
                reject("❌ La operación ha fallado");
            }
        }, 1000); // Esperamos 1 segundo
    });
}
```

### Usar Promesas con .then() y .catch()

Una promesa se resuelve (`resolve`) o se rechaza (`reject`), y estos resultados se manejan con los métodos `.then()` y `.catch()`.

```javascript
miOperacionAsincronica()
    .then(resultado => {
        console.log("Éxito:", resultado);
    })
    .catch(error => {
        console.log("Error:", error);
    });
```

**Métodos**:
- **`.then()`**: Se utiliza para manejar el resultado de una promesa resuelta
- **`.catch()`**: Se utiliza para manejar errores en una promesa rechazada

**Ejemplo del Código Modelo**:
```javascript
operacionAsincronica(true).then(response => {
    console.log("Respuesta:", response);
}).catch(error => {
    console.log("Error:", error);
});
```

### Encadenar Promesas

Las promesas permiten encadenar operaciones asincrónicas de manera más lógica y legible que los callbacks.

```javascript
// Con .then() - Encadenamiento claro
obtenerUsuario(1)
    .then(usuario => obtenerPosts(usuario.id))
    .then(posts => obtenerComentarios(posts[0].id))
    .then(comentarios => console.log(comentarios))
    .catch(error => console.log("Error:", error));
```

**Ventajas del encadenamiento**:
- ✅ Más legible que callbacks anidados
- ✅ Manejo de errores centralizado con `.catch()`
- ✅ Flujo lógico y secuencial

---

## 4. Async / Await (Sintaxis Moderna) ✨

### ¿Qué es Async/Await?

**Async/Await** es una sintaxis introducida en **ECMAScript 2017** que permite escribir código asincrónico de manera más sencilla y similar a código síncrono. Permite escribir código asíncrono que se lee como síncrono. **Es la recomendada**.

### Características

- **`async`**: Marca una función como asíncrona (siempre devuelve una promesa automáticamente)
- **`await`**: Pausa la ejecución hasta que la promesa se resuelva (sin bloquear el flujo del programa)

### Sintaxis Básica

```javascript
async function obtenerDatos() {
  try {
    const respuesta = await fetch("https://api.example.com/data");
    const data = await respuesta.json(); // Espera a convertir a objeto JS
    console.log(data);
  } catch (error) {
    console.log("Algo salió mal:", error);
  }
}
```

**Ejemplo del Código Modelo**:
```javascript
async function ejemploAsincronico() {
    try {
        console.log("🔄 Iniciando operación asíncrona...");
        const resultado = await operacionAsincronica(true);
        console.log("📋 Resultado:", resultado);
    } catch (error) {
        console.log("🚨 Error:", error);
    }
}
```

### Ventajas de Async/Await

- ✅ **Código más legible**: Se parece más al código síncrono
- ✅ **Fácil manejo de errores**: Usa `try/catch` estándar
- ✅ **Elimina "callback hell"**: No hay anidamiento excesivo
- ✅ **Flujo secuencial claro**: Fácil de seguir

### Encadenar con Async/Await

```javascript
// Con async/await - Más legible
async function obtenerDatosCompletos() {
    try {
        const usuario = await obtenerUsuario(1);
        const posts = await obtenerPosts(usuario.id);
        const comentarios = await obtenerComentarios(posts[0].id);
        console.log(comentarios);
    } catch (error) {
        console.log("Error:", error);
    }
}
```

---

## 5. API Fetch 🌐

### ¿Qué es Fetch?

**`fetch`** es una API moderna y poderosa para realizar solicitudes HTTP en JavaScript. Devuelve una promesa que se resuelve con la respuesta a la solicitud, permitiendo el manejo de respuestas y errores con `.then()`, `.catch()`, o con `async/await`.

### Sintaxis Básica

```javascript
// Con .then() y .catch()
fetch("url...")
  .then(resp => resp.json()) // Primer paso: convertir a JSON
  .then(data => console.log(data)) // Segundo paso: usar la data
  .catch(err => console.log(err));
```

**Con async/await**:
```javascript
async function obtenerDatos() {
    try {
        const respuesta = await fetch("https://api.example.com/data");
        const data = await respuesta.json();
        console.log(data);
    } catch (error) {
        console.log("Error:", error);
    }
}
```

### Características de Fetch

- ✅ API nativa del navegador (no necesita librerías)
- ✅ Devuelve una promesa
- ✅ Soporta múltiples métodos HTTP (GET, POST, PUT, DELETE, etc.)
- ✅ Flexible y potente

---

## 6. Manejo de Errores

### Try/Catch con Async/Await

El bloque `try/catch` se utiliza en JavaScript para manejar errores de manera síncrona. En código asincrónico, `try/catch` se usa con `async/await` para manejar excepciones de manera más limpia.

```javascript
async function obtenerDatos() {
    try {
        // Código que podría fallar
        const datos = await obtenerDatosDelServidor();
        console.log(datos);
    } catch (error) {
        // Manejar el error
        console.log("Error:", error);
    }
}
```

**Ventajas**:
- ✅ Manejo de errores centralizado y limpio
- ✅ Sintaxis familiar (igual que código síncrono)
- ✅ Fácil de leer y mantener

### .catch() con Promesas

```javascript
fetch('/api/datos')
    .then(response => console.log(response))
    .catch(error => console.log("Error:", error));
```

**Ventajas**:
- ✅ Manejo de errores en la cadena de promesas
- ✅ Captura errores en cualquier punto de la cadena

---

## 7. Tabla Comparativa: Callbacks vs Promesas vs Async/Await

| Característica | **Callbacks** | **Promesas (.then/.catch)** | **Async/Await** |
|:---|:---|:---|:---|
| **Legibilidad** | ❌ Muy Baja (especialmente con anidamiento - "callback hell") | ⚠️ Media a Buena (con encadenamiento de `.then()`) | ✅ Muy Alta (código similar a síncrono) |
| **Manejo de Errores** | ❌ Difícil (manual, propenso a errores) | ✅ Bueno (con `.catch()`) | ✅ Excelente (con `try/catch`, más limpio y centralizado) |
| **Encadenamiento** | ❌ Anidado, difícil de seguir | ✅ Claro y lógico con `.then()` | ✅ Directo, secuencial |
| **Simplicidad** | ⚠️ Simple para operaciones básicas | ✅ Más estructurado | ✅ Simplifica el uso de promesas |
| **Ejecución** | ⚠️ Secuencial y paralela (con lógica adicional) | ✅ Secuencial y paralela (`Promise.all()`) | ⚠️ Principalmente secuencial (pero se basa en promesas) |
| **Sintaxis** | ❌ Funciones anidadas | ⚠️ `.then().catch()` | ✅ `async function() { await ... }` |
| **Compatibilidad** | ✅ Muy Alta (presente desde el inicio) | ✅ Amplia (introducido en ES6) | ⚠️ Buena (introducido en ES2017) |
| **Callback Hell** | ❌ Sí (problema común) | ✅ No (evita anidamiento) | ✅ No (elimina completamente) |
| **Mantenibilidad** | ❌ Baja (difícil de mantener) | ⚠️ Media (mejor que callbacks) | ✅ Alta (fácil de mantener) |
| **Curva de Aprendizaje** | ✅ Baja (concepto simple) | ⚠️ Media (requiere entender promesas) | ⚠️ Media (requiere entender async/await) |

### Resumen de Pros y Contras

#### Callbacks

**Pros**:
- ✅ Compatibilidad total (presente desde el inicio)
- ✅ Concepto simple
- ✅ Útil para operaciones básicas

**Contras**:
- ❌ Callback hell (anidamiento excesivo)
- ❌ Manejo de errores complicado
- ❌ Código difícil de leer y mantener
- ❌ Difícil depurar

#### Promesas (.then/.catch)

**Pros**:
- ✅ Evita callback hell
- ✅ Encadenamiento claro y lógico
- ✅ Manejo de errores con `.catch()`
- ✅ Compatible con navegadores modernos
- ✅ Permite ejecución paralela con `Promise.all()`

**Contras**:
- ⚠️ Puede volverse verboso con múltiples `.then()`
- ⚠️ Menos legible que async/await
- ⚠️ Requiere entender el concepto de promesas

#### Async/Await

**Pros**:
- ✅ Código más legible (similar a síncrono)
- ✅ Manejo de errores con `try/catch` (familiar)
- ✅ Elimina callback hell completamente
- ✅ Fácil de mantener y depurar
- ✅ Flujo secuencial claro

**Contras**:
- ⚠️ Requiere funciones marcadas como `async`
- ⚠️ No funciona en navegadores muy antiguos (ES2017+)
- ⚠️ Principalmente secuencial (aunque se basa en promesas)

---

## 8. Ejemplos Prácticos del Código Modelo

### Ejemplo 1: Promesa Básica

```javascript
function operacionAsincronica(simularExito = true) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (simularExito) {
                resolve("✅ La operación fue exitosa");
            } else {
                reject("❌ La operación ha fallado");
            }
        }, 1000);
    });
}

// Con .then() y .catch()
operacionAsincronica(true).then(response => {
    console.log("Respuesta:", response);
}).catch(error => {
    console.log("Error:", error);
});

// Con async/await
async function ejemploAsincronico() {
    try {
        const resultado = await operacionAsincronica(true);
        console.log("Resultado:", resultado);
    } catch (error) {
        console.log("Error:", error);
    }
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-16-javascript-poo-asincronia/promesas.js`

### Ejemplo 2: Simular Petición HTTP

```javascript
function simularPeticionHTTP(url, exito = true) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (exito) {
                resolve({
                    url: url,
                    status: 200,
                    data: { mensaje: "Datos obtenidos correctamente", timestamp: new Date() }
                });
            } else {
                reject({
                    url: url,
                    status: 404,
                    error: "No se encontró el recurso"
                });
            }
        }, 1500);
    });
}

async function obtenerDatos() {
    try {
        const respuesta = await simularPeticionHTTP("https://api.ejemplo.com/datos");
        console.log("Datos recibidos:", respuesta);
    } catch (error) {
        console.log("Error en la petición:", error);
    }
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-16-javascript-poo-asincronia/promesas.js`

### Ejemplo 3: Cargar Recursos

```javascript
function cargarRecurso(nombreRecurso, tiempoCarga = 1000) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(`📁 Recurso '${nombreRecurso}' cargado exitosamente`);
        }, tiempoCarga);
    });
}

async function cargarRecursos() {
    try {
        const recurso1 = await cargarRecurso("imagenes", 500);
        console.log(recurso1);
        
        const recurso2 = await cargarRecurso("estilos", 300);
        console.log(recurso2);
        
        const recurso3 = await cargarRecurso("scripts", 200);
        console.log(recurso3);
        
        console.log("🎉 Todos los recursos cargados!");
    } catch (error) {
        console.log("Error cargando recursos:", error);
    }
}
```

**Referencia**: `cursadas/frontend/frontEnd_modelo/tema-16-javascript-poo-asincronia/promesas.js`

---

## 9. Buenas Prácticas ✅

### Siempre Manejar Errores

```javascript
// ❌ MALO
fetch('/api/datos').then(response => console.log(response));

// ✅ BUENO
fetch('/api/datos')
    .then(response => console.log(response))
    .catch(error => console.log("Error:", error));
```

### Usar async/await para Código Más Limpio

```javascript
// ❌ MALO: Callback hell
obtenerDatos()
    .then(datos => {
        procesarDatos(datos)
            .then(resultado => {
                guardarResultado(resultado)
                    .then(() => console.log("Listo"));
            });
    });

// ✅ BUENO: async/await
async function procesoCompleto() {
    const datos = await obtenerDatos();
    const resultado = await procesarDatos(datos);
    await guardarResultado(resultado);
    console.log("Listo");
}
```

### Validar Datos Antes de Usarlos

```javascript
async function obtenerUsuario(id) {
    try {
        const respuesta = await fetch(`/api/usuarios/${id}`);
        
        if (!respuesta.ok) {
            throw new Error(`Error: ${respuesta.status}`);
        }
        
        const usuario = await respuesta.json();
        return usuario;
    } catch (error) {
        console.log("Error obteniendo usuario:", error);
        throw error;
    }
}
```

---

## 10. Comparaciones Clave

### Async/Await vs .then/.catch

**Legibilidad**: async/await ofrece una sintaxis más legible y estructurada en comparación con .then/.catch, especialmente en casos de múltiples operaciones asincrónicas encadenadas.

**Manejo de Errores**: async/await permite manejar errores de manera más sencilla usando try/catch, en lugar de tener que agregar un .catch() al final de una cadena de promesas.

### Async/Await vs Promesas

**Simplicidad**: Aunque async/await se basa en promesas, simplifica su uso eliminando la necesidad de encadenar .then() y .catch().

**Ejecución Secuencial vs Paralela**: async/await es ideal para la ejecución secuencial de operaciones asincrónicas, mientras que las promesas permiten manejar tareas en paralelo con métodos como `Promise.all()`.

### Promesas vs Callbacks

**Mantenimiento del Código**: Las promesas hacen que el código asincrónico sea más fácil de leer y mantener en comparación con los callbacks, evitando el problema del "callback hell".

**Encadenamiento**: Las promesas permiten encadenar operaciones de manera más clara y lógica, mientras que con los callbacks se tiende a anidar funciones.

### Try/Catch vs .then/.catch

**Manejo de Errores**: try/catch proporciona un manejo de errores más centralizado y limpio en código asincrónico cuando se usa con async/await, en comparación con la necesidad de usar .catch() en cada promesa.

---



**Próximo tema**: JavaScript: DOM y Eventos
