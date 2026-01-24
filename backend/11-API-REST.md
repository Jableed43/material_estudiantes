# Master Guide: API REST y Fundamentos 🌐

## 📑 Índice
1. [¿Qué es una API?](#1-qué-es-una-api)
2. [Arquitectura RESTful](#2-arquitectura-restful)
3. [Métodos HTTP y Operaciones CRUD](#3-métodos-http-y-operaciones-crud)
4. [Códigos de Estado HTTP](#4-códigos-de-estado-http)
5. [Parámetros en la URL](#5-parámetros-en-la-url)
6. [Formato de Datos: JSON](#6-formato-de-datos-json)
7. [API vs Base de Datos vs Servidor](#7-api-vs-base-de-datos-vs-servidor)
8. [MockAPI: Prototipado Rápido](#8-mockapi-prototipado-rápido)
9. [Variables de Entorno y API Keys](#9-variables-de-entorno-y-api-keys)
10. [Buenas Prácticas](#10-buenas-prácticas)

---

## 1. ¿Qué es una API?

Una **API (Application Programming Interface)** es un conjunto de reglas, protocolos y herramientas que permiten que diferentes aplicaciones o sistemas interactúen entre sí. Es una especie de "puente" o "camarero" que permite que dos programas se comuniquen de manera eficiente, intercambiando información o instrucciones sin que los usuarios vean lo que ocurre detrás de escena.

### Analogía del Restaurante

Imagina que estás en un restaurante:
- **Tú (Cliente)**: Quieres comida (los datos)
- **Camarero (API)**: Lleva tu pedido (petición) a la cocina (servidor/base de datos) y te trae la comida (datos)
- **Cocina (Servidor/Base de Datos)**: Prepara y almacena la comida

Este proceso te permite obtener lo que necesitas sin conocer los complejos procesos internos.

### Importancia de una API en el Desarrollo Web

Las APIs son esenciales en el desarrollo web moderno por varias razones:

#### 1. Interacción entre Frontend y Backend
Las APIs permiten que el frontend (la interfaz de usuario) interactúe con el backend (el servidor que maneja la lógica y los datos). Por ejemplo, cuando un usuario envía un formulario, la información pasa por una API al backend, que la procesa y devuelve una respuesta (éxito o error). Este modelo **cliente-servidor** es fundamental para las aplicaciones modernas.

**Flujo Request-Response (Solicitud-Respuesta)**:
```
Cliente (Frontend) → API → Servidor (Backend) → Base de Datos
Cliente (Frontend) ← API ← Servidor (Backend) ← Base de Datos
```

#### 2. Modularidad
Con una API, diferentes partes de una aplicación (o diferentes aplicaciones) pueden interactuar sin depender directamente entre sí. Esto mejora la escalabilidad y mantenibilidad del sistema.

#### 3. Conexión entre Servicios
Las APIs permiten que aplicaciones diferentes (como Facebook, Twitter, Google Maps, etc.) se integren fácilmente. Por ejemplo:
- Una página web puede mostrar mapas de Google usando la API de Google Maps
- Iniciar sesión con Facebook utilizando su API
- Mostrar tweets de Twitter usando su API

#### 4. Uso de Servicios Externos
Muchas APIs proporcionan acceso a datos o servicios que una aplicación no tiene que generar internamente. Por ejemplo:
- En lugar de crear un sistema de pago desde cero, puedes integrar la API de Stripe o PayPal
- En lugar de crear un sistema de mapas, puedes usar la API de Google Maps
- **Buscar no reinventar la rueda**

---

## 2. Arquitectura RESTful

**REST (Representational State Transfer)** es un estilo arquitectónico para diseñar APIs. Las APIs que siguen estos principios se llaman **RESTful**.

### Principios de REST

#### 1. Recursos
Todo en una API REST es un **recurso**, una entidad identificable (ej. un usuario, un producto, un pedido). Los recursos se representan mediante URLs únicas.

**Ejemplo de Recursos**:
- `/api/usuarios` - Colección de usuarios
- `/api/usuarios/1` - Usuario específico con ID 1
- `/api/productos` - Colección de productos
- `/api/productos/5` - Producto específico con ID 5

#### 2. Endpoints
Los recursos se acceden a través de URLs únicas, llamadas **endpoints**. Cada endpoint representa una operación específica sobre un recurso.

**Ejemplo de Endpoints**:
```
GET    /api/usuarios          → Obtener todos los usuarios
GET    /api/usuarios/1        → Obtener usuario con ID 1
POST   /api/usuarios          → Crear un nuevo usuario
PUT    /api/usuarios/1        → Actualizar usuario con ID 1
DELETE /api/usuarios/1        → Eliminar usuario con ID 1
```

#### 3. Stateless (Sin Estado)
Una API REST es **sin estado**. Cada solicitud del cliente debe contener toda la información necesaria para que el servidor la procese. El servidor no "recuerda" nada de las solicitudes anteriores del mismo cliente, lo que la hace robusta y escalable.

**Características**:
- ✅ Cada petición es independiente
- ✅ No hay sesiones del servidor
- ✅ Toda la información necesaria va en la petición
- ✅ Facilita la escalabilidad horizontal

**Ejemplo**:
```javascript
// Primera petición
fetch('/api/usuarios', {
  headers: { 'Authorization': 'Bearer token123' }
});

// Segunda petición (independiente)
fetch('/api/productos', {
  headers: { 'Authorization': 'Bearer token123' }
});
```

#### 4. Formato de Datos
El formato de datos más utilizado para el intercambio de información es **JSON (JavaScript Object Notation)**, un formato ligero y legible por humanos.

**Ejemplo de Respuesta JSON**:
```json
{
  "id": 1,
  "nombre": "Juan",
  "email": "juan@example.com",
  "edad": 30
}
```

#### 5. Escalabilidad y Desacoplamiento
La arquitectura REST permite la escalabilidad y el desacoplamiento entre clientes y servidores. Cada componente puede evolucionar de forma independiente sin afectar al otro.

---

## 3. Métodos HTTP y Operaciones CRUD

Los métodos HTTP son las "acciones" que el cliente pide al servidor que realice sobre un recurso. Estos métodos se correlacionan directamente con las operaciones **CRUD** (Create, Read, Update, Delete).

### Tabla de Métodos HTTP

| Método HTTP | Propósito | Operación CRUD | Uso Común | Ejemplo en Express |
|-------------|-----------|----------------|-----------|-------------------|
| **GET** | Obtener datos | **Read** (Leer) | Leer o acceder a información. **No modifica nada en el servidor**. | `router.get('/usuarios')` |
| **POST** | Enviar datos para crear | **Create** (Crear) | Crear nuevos elementos (usuarios, productos, posts). | `router.post('/usuarios')` |
| **PUT** | Reemplazar completamente | **Update** (Actualizar) | Modificar todo un recurso, sobreescribiendo sus datos. | `router.put('/usuarios/:id')` |
| **PATCH** | Modificar parcialmente | **Update** (Actualizar) | Actualizar solo una parte específica del recurso. | `router.patch('/usuarios/:id')` |
| **DELETE** | Eliminar un recurso | **Delete** (Eliminar) | Borrar elementos (usuarios, productos, posts). | `router.delete('/usuarios/:id')` |

### GET - Obtener Datos

**Propósito**: Obtener datos del servidor sin modificar nada.

**Características**:
- ✅ No modifica el servidor
- ✅ Puede llevar parámetros en la URL (query params)
- ✅ No lleva body (cuerpo de la petición)
- ✅ Puede ser cacheado

**Ejemplo**:
```javascript
// Obtener todos los usuarios
fetch('https://api.ejemplo.com/usuarios')
  .then(response => response.json())
  .then(data => console.log(data));

// Obtener un usuario específico
fetch('https://api.ejemplo.com/usuarios/1')
  .then(response => response.json())
  .then(data => console.log(data));
```

### POST - Crear Nuevos Recursos

**Propósito**: Enviar datos al servidor para crear un nuevo recurso.

**Características**:
- ✅ Crea nuevos recursos
- ✅ Lleva datos en el body (cuerpo de la petición)
- ✅ No es idempotente (cada llamada crea un nuevo recurso)
- ✅ Retorna código 201 (Created) en caso de éxito

**Ejemplo**:
```javascript
fetch('https://api.ejemplo.com/usuarios', {
  method: 'POST',
  headers: { 
    'Content-Type': 'application/json' 
  },
  body: JSON.stringify({ 
    nombre: 'Juan', 
    email: 'juan@example.com',
    edad: 30 
  })
})
  .then(response => response.json())
  .then(data => console.log(data));
```

### PUT - Reemplazar Completamente

**Propósito**: Enviar datos al servidor para reemplazar completamente un recurso existente.

**Características**:
- ✅ Reemplaza todo el recurso con los nuevos datos
- ✅ Si faltan campos en la solicitud, esos campos se eliminarán en el servidor
- ✅ Es idempotente (múltiples llamadas tienen el mismo efecto)
- ✅ Lleva datos en el body

**Ejemplo**:
```javascript
// Si un usuario tiene { nombre: 'Juan', edad: 30 }
// Y envías solo { nombre: 'Pedro' }
// La edad se perderá (se eliminará)
fetch('https://api.ejemplo.com/usuarios/1', {
  method: 'PUT',
  headers: { 
    'Content-Type': 'application/json' 
  },
  body: JSON.stringify({ 
    nombre: 'Pedro', 
    edad: 35 
  })
});
```

### PATCH - Modificar Parcialmente

**Propósito**: Enviar datos al servidor para modificar parcialmente un recurso existente.

**Características**:
- ✅ Solo modifica partes específicas del recurso
- ✅ Los campos que no envíes quedarán intactos
- ✅ Es idempotente
- ✅ Lleva datos en el body

**Ejemplo**:
```javascript
// Si un usuario tiene { nombre: 'Juan', edad: 30 }
// Y envías solo { nombre: 'Pedro' }
// La edad NO se verá afectada (se mantiene en 30)
fetch('https://api.ejemplo.com/usuarios/1', {
  method: 'PATCH',
  headers: { 
    'Content-Type': 'application/json' 
  },
  body: JSON.stringify({ 
    nombre: 'Pedro' 
  })
});
```

### DELETE - Eliminar Recursos

**Propósito**: Eliminar un recurso en el servidor.

**Características**:
- ✅ Elimina el recurso especificado
- ✅ No lleva body (generalmente)
- ✅ Es idempotente
- ✅ Retorna código 204 (No Content) o 200 en caso de éxito

**Ejemplo**:
```javascript
fetch('https://api.ejemplo.com/usuarios/1', { 
  method: 'DELETE' 
})
  .then(response => {
    if (response.ok) {
      console.log('Usuario eliminado');
    }
  });
```

### Diferencia entre PUT y PATCH

| Aspecto | PUT | PATCH |
|---------|-----|-------|
| **Alcance** | Reemplaza todo el recurso | Modifica solo campos especificados |
| **Campos faltantes** | Se eliminan | Se mantienen |
| **Ejemplo** | `{ nombre: 'Pedro' }` → elimina `edad` | `{ nombre: 'Pedro' }` → mantiene `edad` |
| **Uso** | Cuando quieres reemplazar completamente | Cuando quieres actualizar parcialmente |

**Resumen**:
- **GET**: Obtener datos
- **POST**: Crear nuevos datos
- **PUT**: Reemplazar completamente un recurso
- **PATCH**: Modificar parcialmente un recurso
- **DELETE**: Eliminar un recurso

---

## 4. Códigos de Estado HTTP

Los códigos de estado HTTP son la forma en que el servidor se comunica con el cliente para informarle sobre el resultado de una solicitud. Son números de 3 dígitos que indican si la petición fue exitosa, hubo un error, o requiere alguna acción adicional.

### Categorías de Códigos de Estado

| Categoría | Rango | Significado |
|-----------|-------|-------------|
| **2xx** | 200-299 | Respuestas satisfactorias (éxito) |
| **3xx** | 300-399 | Redirecciones |
| **4xx** | 400-499 | Errores del cliente |
| **5xx** | 500-599 | Errores del servidor |

### 2xx - Respuestas Satisfactorias

#### 200 OK
La solicitud ha tenido éxito. Es el código más común para una respuesta exitosa.

**Uso común**: GET, PUT, PATCH exitosos

**Ejemplo**:
```javascript
// GET /api/usuarios
// Respuesta: 200 OK
{
  "usuarios": [...]
}
```

#### 201 Created
La solicitud ha sido completada y ha resultado en la creación de un nuevo recurso. Se usa comúnmente en POST.

**Uso común**: POST exitoso

**Ejemplo**:
```javascript
// POST /api/usuarios
// Respuesta: 201 Created
{
  "id": 1,
  "nombre": "Juan",
  "email": "juan@example.com"
}
```

#### 204 No Content
La solicitud ha sido completada con éxito, pero **la respuesta no incluye un cuerpo de mensaje**. Este código es útil para solicitudes PUT o DELETE donde solo se necesita confirmar la acción.

**Uso común**: PUT, DELETE exitosos

**Ejemplo**:
```javascript
// DELETE /api/usuarios/1
// Respuesta: 204 No Content (sin body)
```

### 3xx - Redirecciones

#### 301 Moved Permanently
El recurso se ha movido permanentemente a una nueva URL.

#### 304 Not Modified
El recurso no ha sido modificado desde la última vez que el cliente lo solicitó. Es una respuesta de caché que **no tiene un cuerpo**.

### 4xx - Errores del Cliente

#### 400 Bad Request
El servidor no pudo entender la solicitud debido a una sintaxis incorrecta.

**Causas comunes**:
- JSON mal formado
- Campos requeridos faltantes
- Tipos de datos incorrectos

**Ejemplo**:
```javascript
// POST /api/usuarios
// Body: { "nombre": "Juan" }  // Falta "email" requerido
// Respuesta: 400 Bad Request
{
  "error": "El campo 'email' es requerido"
}
```

#### 401 Unauthorized
La solicitud requiere autenticación. El cliente debe proporcionar credenciales.

**Uso común**: Falta token de autenticación o token inválido

**Ejemplo**:
```javascript
// GET /api/usuarios (ruta protegida)
// Sin token en headers
// Respuesta: 401 Unauthorized
{
  "error": "Token de acceso no proporcionado"
}
```

#### 403 Forbidden
El servidor ha entendido la solicitud, pero se niega a autorizarla. A diferencia de 401, el cliente está autenticado pero no tiene permisos.

**Uso común**: Usuario autenticado pero sin permisos suficientes

#### 404 Not Found
El servidor no pudo encontrar el recurso solicitado.

**Uso común**: URL incorrecta o recurso eliminado

**Ejemplo**:
```javascript
// GET /api/usuarios/999
// Usuario con ID 999 no existe
// Respuesta: 404 Not Found
{
  "error": "Usuario no encontrado"
}
```

#### 409 Conflict
La solicitud no pudo ser completada debido a un conflicto (ej. intentar crear un recurso que ya existe).

**Uso común**: Email duplicado, recurso ya existe

**Ejemplo**:
```javascript
// POST /api/usuarios
// Body: { "email": "juan@example.com" }  // Email ya existe
// Respuesta: 409 Conflict
{
  "error": "El email ya está registrado"
}
```

#### 415 Unsupported Media Type
El servidor no acepta el formato de datos enviado.

**Uso común**: Content-Type incorrecto

### 5xx - Errores del Servidor

#### 500 Internal Server Error
El servidor ha encontrado una condición inesperada que le impidió completar la solicitud.

**Uso común**: Error en el código del servidor, excepción no manejada

**Ejemplo**:
```javascript
// Cualquier petición
// Error en el código del servidor
// Respuesta: 500 Internal Server Error
{
  "error": "Error interno del servidor"
}
```

#### 503 Service Unavailable
El servidor no está listo para manejar la solicitud, a menudo por sobrecarga o mantenimiento.

**Uso común**: Servidor en mantenimiento, sobrecarga

### Tabla Resumen de Códigos Comunes

| Código | Significado | Uso Común |
|--------|-------------|-----------|
| **200** | OK | Éxito total (GET, PUT, PATCH) |
| **201** | Created | Recurso creado (POST) |
| **204** | No Content | Éxito sin body (DELETE, PUT) |
| **400** | Bad Request | Solicitud mal formada |
| **401** | Unauthorized | Falta autenticación |
| **403** | Forbidden | Sin permisos |
| **404** | Not Found | Recurso no encontrado |
| **409** | Conflict | Conflicto (recurso duplicado) |
| **500** | Server Error | Error en el servidor |

---

## 5. Parámetros en la URL

En las APIs REST, los parámetros se pueden enviar de diferentes formas según su propósito.

### A. Path Params (Parámetros de Ruta)

Se usan para **identificar recursos específicos**. Forman parte de la URL misma.

**Sintaxis**: `/recurso/:id`

**Ejemplo**:
```
GET /usuarios/5        → El `5` es el ID del usuario
GET /productos/123     → El `123` es el ID del producto
```

**En Express**:
```javascript
router.get("/usuarios/:id", (req, res) => {
  const userId = req.params.id;  // Acceso al parámetro
  // userId = "5"
});
```

**Características**:
- ✅ Obligatorios (parte de la ruta)
- ✅ Identifican recursos específicos
- ✅ Se acceden con `req.params`

### B. Query Params (Parámetros de Consulta)

Se usan para **filtrar, buscar, paginar o ordenar** datos. Van después del signo `?` en la URL.

**Sintaxis**: `?clave=valor&clave2=valor2`

**Ejemplo**:
```
GET /productos?categoria=electronica&precioMax=500
GET /usuarios?page=1&limit=10&sort=nombre
GET /productos?buscar=laptop&orden=precio&ascendente=true
```

**En Express**:
```javascript
router.get("/productos", (req, res) => {
  const categoria = req.query.categoria;      // "electronica"
  const precioMax = req.query.precioMax;      // "500"
  const page = req.query.page;                // "1"
  const limit = req.query.limit;              // "10"
});
```

**Características**:
- ✅ Opcionales
- ✅ Múltiples parámetros separados por `&`
- ✅ Se acceden con `req.query`
- ✅ Siempre son strings (convertir si necesitas números)

**Ejemplo Completo**:
```javascript
// URL: /api/productos?categoria=electronica&precioMax=500&page=1
router.get("/productos", (req, res) => {
  const { categoria, precioMax, page } = req.query;
  
  // precioMax es string "500", convertir a número si es necesario
  const precioMaxNum = parseInt(precioMax) || Infinity;
  const pageNum = parseInt(page) || 1;
  
  // Usar en consulta a base de datos
  const productos = await Producto.find({
    categoria: categoria,
    precio: { $lte: precioMaxNum }
  })
    .skip((pageNum - 1) * 10)
    .limit(10);
    
  res.json(productos);
});
```

### C. Body (Cuerpo de la Petición)

Se usa para **enviar datos** en peticiones POST, PUT, PATCH. No va en la URL.

**Ejemplo**:
```javascript
// POST /api/usuarios
fetch('/api/usuarios', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    nombre: 'Juan',
    email: 'juan@example.com',
    edad: 30
  })
});
```

**En Express**:
```javascript
// Necesitas middleware para parsear JSON
app.use(express.json());

router.post("/usuarios", (req, res) => {
  const { nombre, email, edad } = req.body;
  // nombre = "Juan"
  // email = "juan@example.com"
  // edad = 30
});
```

### Comparación de Parámetros

| Tipo | Uso | Sintaxis | Acceso en Express | Ejemplo |
|------|-----|----------|-------------------|---------|
| **Path Params** | Identificar recursos | `/recurso/:id` | `req.params.id` | `/usuarios/5` |
| **Query Params** | Filtrar, buscar, paginar | `?clave=valor` | `req.query.clave` | `/productos?categoria=electronica` |
| **Body** | Enviar datos | En el cuerpo | `req.body` | `{ nombre: "Juan" }` |

---

## 6. Formato de Datos: JSON

**JSON (JavaScript Object Notation)** es un formato ligero de intercambio de datos, que se utiliza para almacenar y transferir información en aplicaciones web. Su estructura se basa en una lista organizada de información compuesta por pares de claves y valores.

### Características de JSON

- ✅ **Ligero**: Más pequeño que XML
- ✅ **Legible**: Fácil de leer y escribir por humanos
- ✅ **Independiente del lenguaje**: Aunque diseñado para JavaScript, funciona con cualquier lenguaje
- ✅ **Estructura simple**: Basado en objetos y arrays
- ✅ **Seguro**: No ejecuta código, solo datos

### Estructura de JSON

**Objeto JSON**:
```json
{
  "nombre": "Juan",
  "edad": 30,
  "email": "juan@example.com",
  "activo": true,
  "hobbies": ["leer", "programar", "viajar"]
}
```

**Array JSON**:
```json
[
  { "id": 1, "nombre": "Juan" },
  { "id": 2, "nombre": "María" },
  { "id": 3, "nombre": "Pedro" }
]
```

### Operaciones con JSON en JavaScript

#### JSON.stringify() - Convertir a JSON

Convierte un objeto de JavaScript a una cadena JSON.

```javascript
const usuario = {
  nombre: "Juan",
  edad: 30,
  email: "juan@example.com"
};

const jsonString = JSON.stringify(usuario);
console.log(jsonString);
// '{"nombre":"Juan","edad":30,"email":"juan@example.com"}'
```

#### JSON.parse() - Convertir de JSON

Convierte una cadena JSON a un objeto de JavaScript.

```javascript
const jsonString = '{"nombre":"Juan","edad":30,"email":"juan@example.com"}';
const usuario = JSON.parse(jsonString);
console.log(usuario.nombre);  // "Juan"
```

### Importancia de JSON

1. **Intercambio de Datos en la Web**: JSON se ha convertido en un estándar para el envío de datos entre servidores, navegadores y APIs.

2. **Facilidad de Uso**: Su sintaxis es simple y clara, lo que permite una rápida comprensión de la estructura de los datos.

3. **Compatibilidad Universal**: JSON es compatible con casi todos los lenguajes de programación, lo que lo hace ideal para aplicaciones que manejan diferentes tecnologías.

### Ventajas de Convertir JSON a Objetos de JavaScript

- ✅ **Lectura**: Facilita la lectura y comprensión de los datos
- ✅ **Métodos y Funciones**: Permite aplicar métodos sobre los datos (for, map, forEach, filter, find)
- ✅ **Seguridad**: Al ser una cadena de texto, no ejecuta código, lo que ayuda a evitar inyecciones de código malintencionado
- ✅ **Ligereza**: Es más ligero que otros formatos como XML, lo que reduce el tiempo de transferencia de datos

---

## 7. API vs Base de Datos vs Servidor

Es importante entender las diferencias entre estos conceptos:

### API vs Base de Datos

| Aspecto | API | Base de Datos |
|---------|-----|---------------|
| **Función** | Medio de comunicación | Almacenamiento de datos |
| **Acceso** | Permite enviar y recibir información | Guarda información estructurada |
| **Interacción** | Intercambia datos con aplicaciones | No interactúa directamente con aplicaciones |
| **Ejemplo** | `/api/usuarios` | Tabla `usuarios` en MySQL/MongoDB |

**Conclusión**: La API te permite enviar una solicitud para obtener información de una base de datos. La base de datos guarda la información, pero no puede interactuar directamente con tu aplicación sin una API o una consulta directa.

### API vs Servidor

| Aspecto | API | Servidor |
|---------|-----|----------|
| **Función** | Interfaz de comunicación | Infraestructura que procesa |
| **Rol** | Define cómo interactuar | Aloja y ejecuta la lógica |
| **Ejemplo** | Endpoints `/api/usuarios` | Express.js corriendo en Node.js |

**Conclusión**: El servidor aloja el código que maneja la lógica de la aplicación. La API actúa como un mensajero entre el servidor y los clientes (como el navegador del usuario), facilitando las interacciones.

### Flujo Completo

```
Cliente (Frontend)
    ↓
API (Endpoints)
    ↓
Servidor (Express.js)
    ↓
Base de Datos (MongoDB/MySQL)
```

**Ejemplo Práctico**:
1. Cliente solicita: `GET /api/usuarios`
2. API recibe la petición en el endpoint
3. Servidor (Express) procesa la petición
4. Servidor consulta la Base de Datos
5. Base de Datos retorna los datos
6. Servidor formatea la respuesta
7. API envía la respuesta al Cliente

---

## 8. MockAPI: Prototipado Rápido

**MockAPI** es una herramienta en línea que permite crear **APIs simuladas (mock APIs)** rápidamente para pruebas y desarrollo de aplicaciones sin la necesidad de construir un backend completo.

### Características Principales

1. **Generación Rápida de Datos**: MockAPI permite crear datos simulados como usuarios, productos, posts, etc., con estructuras personalizables.

2. **CRUD Completo**: Soporta operaciones CRUD (Crear, Leer, Actualizar y Eliminar) mediante endpoints HTTP (GET, POST, PUT, DELETE).

3. **Simulación de Bases de Datos**: Puedes almacenar y manipular datos a través de endpoints como si estuvieras trabajando con una base de datos real.

4. **Escalable**: Puedes definir varias "colecciones" de datos, ideal para manejar diferentes tipos de recursos como usuarios, tareas, productos, etc.

5. **Configuración Rápida de Endpoints**: Permite configurar rutas de API fácilmente para simular la interacción de frontend-backend.

6. **Accesible**: MockAPI es ideal para proyectos de aprendizaje o prototipado, ya que proporciona APIs públicas sin necesidad de configuración compleja de servidores.

### Casos de Uso

- ✅ **Pruebas de Frontend**: Puedes desarrollar y probar tu aplicación frontend sin tener un servidor backend listo
- ✅ **Prototipado Rápido**: Permite simular datos dinámicos durante la fase de prototipado de una aplicación
- ✅ **Desarrollo Colaborativo**: Diferentes equipos pueden trabajar de manera independiente sobre la misma API simulada sin bloquear el progreso por falta de un backend funcional

### Ejemplo de Uso

```javascript
// MockAPI proporciona una URL como:
// https://64f1a5b0edfa0459f6c6e9e4.mockapi.io/api/usuarios

// Puedes usarla en tu frontend:
fetch('https://64f1a5b0edfa0459f6c6e9e4.mockapi.io/api/usuarios')
  .then(response => response.json())
  .then(data => console.log(data));
```

---

## 9. Variables de Entorno y API Keys

### Variables de Entorno

Una **variable de entorno** es una clave-valor que está disponible en el entorno donde se ejecuta una aplicación, ya sea en tu máquina local, en un servidor o en otro entorno de ejecución.

#### ¿Por qué usar Variables de Entorno?

1. **Seguridad**: Permiten mantener información sensible (como claves de API, contraseñas, configuraciones) fuera del código fuente. Esto previene que estos datos sensibles se filtren accidentalmente al repositorio.

2. **Flexibilidad**: Facilitan la configuración de aplicaciones para diferentes entornos (desarrollo, pruebas, producción) sin modificar el código.

3. **Mantenimiento**: Al centralizar las configuraciones que cambian según el entorno en un archivo separado, es más fácil mantener el código.

4. **Portabilidad**: Permite mover fácilmente la aplicación entre diferentes entornos sin tener que reconfigurarla manualmente.

#### Ejemplos Comunes

- Base de datos: `DB_HOST`, `DB_USER`, `DB_PASSWORD`
- APIs: `API_KEY`, `VITE_API_URL`
- Configuración: `NODE_ENV=production` o `development`

### API Keys

Una **API Key** es una credencial para autenticar peticiones a una API. Es una cadena de caracteres única que identifica tu aplicación o usuario.

#### Uso de API Key en Fetch

**1. API Key en el Encabezado**:
```javascript
fetch('https://api.ejemplo.com/data', {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer TU_API_KEY_AQUI'
  }
})
  .then(response => response.json())
  .then(data => console.log(data));
```

**2. API Key como Parámetro de URL**:
```javascript
const apiKey = 'TU_API_KEY_AQUI';
const url = `https://api.ejemplo.com/data?api_key=${apiKey}`;

fetch(url, {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
})
  .then(response => response.json())
  .then(data => console.log(data));
```

#### Notas Importantes

- ⚠️ **Nunca expongas tu API Key en el frontend** en aplicaciones en producción
- ⚠️ Si tu API Key es sensible, utiliza un servidor para hacer las solicitudes y oculta la clave API en variables de entorno
- ⚠️ Verifica siempre la documentación de la API que estás utilizando para saber cómo manejar la API Key

### Variables de Entorno en Vite

Para usar variables de entorno en un proyecto con Vite:

1. **Crea un archivo `.env`** en la raíz del proyecto:
```env
VITE_API_URL=https://api.example.com
VITE_APP_TITLE=Mi Aplicación
```

2. **Accede a las variables** usando `import.meta.env`:
```javascript
const apiUrl = import.meta.env.VITE_API_URL;
const appTitle = import.meta.env.VITE_APP_TITLE;
```

3. **Reinicia el servidor** cada vez que agregues o modifiques variables en el archivo `.env`

4. **Prefijo `VITE_`**: Solo las variables que comienzan con este prefijo estarán disponibles en el código del cliente

5. **Jerarquía de archivos `.env`**:
   - `.env`: Se carga en todos los modos
   - `.env.local`: Se carga en todos los modos, pero está ignorado por Git (ideal para información sensible)
   - `.env.[mode]`: Solo se carga en el modo específico (ej. `.env.production`)
   - `.env.[mode].local`: Similar al anterior, pero también está ignorado por Git

---

## 10. Buenas Prácticas

### Nomenclatura de Endpoints

- ✅ Usar sustantivos en plural: `/api/usuarios`, `/api/productos`
- ✅ Usar minúsculas y guiones: `/api/usuarios-activos`
- ✅ Evitar verbos en la URL: ❌ `/api/obtener-usuarios` ✅ `/api/usuarios`

### Estructura de Respuestas

**Respuesta Exitosa**:
```json
{
  "success": true,
  "data": {
    "id": 1,
    "nombre": "Juan"
  }
}
```

**Respuesta de Error**:
```json
{
  "success": false,
  "error": {
    "message": "Usuario no encontrado",
    "code": "USER_NOT_FOUND"
  }
}
```

### Versionado de API

Usar versiones en la URL:
```
/api/v1/usuarios
/api/v2/usuarios
```

### Documentación

- ✅ Documentar todos los endpoints
- ✅ Incluir ejemplos de peticiones y respuestas
- ✅ Especificar códigos de estado posibles
- ✅ Usar herramientas como Swagger/OpenAPI

### Seguridad

- ✅ Usar HTTPS en producción
- ✅ Validar y sanitizar todos los inputs
- ✅ Implementar autenticación (JWT, OAuth)
- ✅ Usar rate limiting para prevenir abusos
- ✅ Nunca exponer información sensible en respuestas

### Performance

- ✅ Implementar paginación para listas grandes
- ✅ Usar caché cuando sea apropiado
- ✅ Optimizar consultas a base de datos
- ✅ Comprimir respuestas (gzip)

---

