# Master Guide: Postman y Testing de APIs 🚀

## 📑 Índice
1. [Introducción a Postman](#1-introducción-a-postman)
2. [Tipos de Peticiones HTTP](#2-tipos-de-peticiones-http)
3. [Configuración de Peticiones](#3-configuración-de-peticiones)
4. [Variables de Entorno (Environments)](#4-variables-de-entorno-environments)
5. [Colecciones (Collections)](#5-colecciones-collections)
6. [Scripts de Test Automatizados](#6-scripts-de-test-automatizados)
7. [Autenticación en Postman](#7-autenticación-en-postman)
8. [Pre-request Scripts](#8-pre-request-scripts)
9. [Mejores Prácticas](#9-mejores-prácticas)

---

## 1. Introducción a Postman

### ¿Qué es Postman? (Analogía del Mundo Real)

### 🧪 Analogía: El Laboratorio de Pruebas

Imagina que tienes un laboratorio para probar cosas:
- **Postman**: Es como tu laboratorio de pruebas
- **Backend (API)**: Es lo que quieres probar
- **Peticiones**: Son los experimentos que haces
- **Respuestas**: Son los resultados que obtienes

**Postman es como un laboratorio** donde puedes probar tu API sin necesidad de tener el frontend listo.

### 📞 Analogía: El Teléfono para Llamar a Servicios

Piensa en un teléfono especial:
- **Postman**: Es como un teléfono para llamar a servicios
- **API**: Es el servicio que llamas
- **Peticiones**: Son las llamadas que haces
- **Respuestas**: Es lo que te responden

**Postman te permite "llamar" a tu API** y ver qué responde, sin necesidad de tener una interfaz visual.

### 🎮 Analogía: El Control Remoto Universal

Un control remoto universal:
- **Postman**: Es como un control remoto para tu API
- **API**: Es el dispositivo que controlas
- **Botones (peticiones)**: Son los diferentes comandos que puedes enviar
- **Pantalla (respuesta)**: Muestra el resultado

**Postman es como un control remoto** que te permite probar todas las funciones de tu API.

**Postman** es la herramienta esencial para probar tu backend sin necesidad de tener el frontend terminado. Es una aplicación que permite crear, enviar y probar peticiones HTTP de forma visual e interactiva.

**En términos simples**: Postman es como un "laboratorio de pruebas" donde puedes probar tu API enviando peticiones y viendo las respuestas, sin necesidad de tener el frontend desarrollado.

### ¿Por qué usar Postman?

- ✅ **Probar APIs sin frontend**: Puedes probar tu backend antes de tener el frontend listo
- ✅ **Documentación visual**: Organiza y documenta tus endpoints
- ✅ **Automatización**: Scripts para validar respuestas automáticamente
- ✅ **Colaboración**: Compartir colecciones con tu equipo
- ✅ **Variables**: Reutilizar URLs y datos en múltiples peticiones

### Instalación

1. Descargar desde [postman.com](https://www.postman.com/downloads/)
2. Crear cuenta (opcional, pero recomendado para sincronización)
3. Instalar aplicación de escritorio o usar versión web

---

## 2. Tipos de Peticiones HTTP

En Postman seleccionas el **Método HTTP** (GET, POST, PUT, PATCH, DELETE) y la **URL** de tu endpoint.

### Métodos HTTP Disponibles

| Método | Uso | Ejemplo |
|--------|-----|---------|
| **GET** | Obtener datos | `GET /api/usuarios` |
| **POST** | Crear recursos | `POST /api/usuarios` |
| **PUT** | Actualizar completo | `PUT /api/usuarios/1` |
| **PATCH** | Actualizar parcial | `PATCH /api/usuarios/1` |
| **DELETE** | Eliminar recursos | `DELETE /api/usuarios/1` |

### Configuración Básica

1. Selecciona el método HTTP en el dropdown
2. Ingresa la URL del endpoint
3. Haz clic en **Send**

**Ejemplo**:
```
GET http://localhost:3000/api/usuarios
```

---

## 3. Configuración de Peticiones

Cada petición en Postman tiene varias pestañas para configurar diferentes aspectos:

### Tabla de Pestañas

| Pestaña | Propósito | Ejemplo |
|---------|-----------|---------|
| **Params** | Para Query Params (`?id=5`) | `?page=1&limit=10` |
| **Authorization** | Para enviar el Token JWT (usar `Bearer Token`) | `Authorization: Bearer token123` |
| **Headers** | Metadatos (ej: `Content-Type: application/json`) | `Content-Type: application/json` |
| **Body** | El JSON que envías al servidor (usar `raw` -> `JSON`) | `{ "nombre": "Juan" }` |
| **Pre-request Script** | Scripts que se ejecutan antes de enviar | Variables dinámicas |
| **Tests** | Scripts que validan la respuesta | Validar status code |

### Params (Query Parameters)

Para agregar parámetros de consulta en la URL:

1. Ve a la pestaña **Params**
2. Agrega clave y valor
3. Se agregarán automáticamente a la URL como `?clave=valor`

**Ejemplo**:
```
Key: page    Value: 1
Key: limit   Value: 10
URL resultante: /api/usuarios?page=1&limit=10
```

### Headers

Para agregar encabezados HTTP:

1. Ve a la pestaña **Headers**
2. Agrega clave y valor
3. Postman incluye algunos por defecto (como `Content-Type`)

**Headers Comunes**:
```
Content-Type: application/json
Authorization: Bearer <token>
Accept: application/json
```

### Body

Para enviar datos en POST, PUT, PATCH:

1. Ve a la pestaña **Body**
2. Selecciona **raw**
3. Selecciona **JSON** en el dropdown
4. Escribe tu JSON

**Ejemplo**:
```json
{
  "nombre": "Juan",
  "email": "juan@example.com",
  "edad": 30
}
```

**Tipos de Body**:
- **none**: Sin body (GET, DELETE)
- **form-data**: Formularios multipart
- **x-www-form-urlencoded**: Formularios HTML
- **raw**: JSON, XML, texto plano
- **binary**: Archivos

---

## 4. Variables de Entorno (Environments)

No escribas `http://localhost:3000` en cada petición. Usa variables de entorno para reutilizar valores.

### Crear un Environment

1. Click en el ícono de **Environments** (ojo) en la esquina superior derecha
2. Click en **+** para crear nuevo
3. Nombre: "Desarrollo" o "Producción"
4. Agrega variables:
   - `base_url`: `http://localhost:3000`
   - `token`: (se llenará automáticamente con scripts)
   - `user_id`: `123`

### Usar Variables

En la URL o en cualquier campo, usa `{{variable_name}}`:

```
{{base_url}}/api/usuarios
```

**Ejemplo Completo**:
```
Variable: base_url = http://localhost:3000
URL: {{base_url}}/api/usuarios
Resultado: http://localhost:3000/api/usuarios
```

### Cambiar entre Environments

1. Selecciona el environment en el dropdown superior derecho
2. Todas las variables `{{variable}}` se reemplazarán automáticamente

**Ejemplo de Múltiples Environments**:
```
Desarrollo:
  base_url: http://localhost:3000
  
Producción:
  base_url: https://mi-api.vercel.app
```

---

## 5. Colecciones (Collections)

Agrupa tus peticiones por proyecto o por entidad (ej: "Auth", "Productos").

### Crear una Colección

1. Click en **New** > **Collection**
2. Nombre: "API Usuarios" o "Mi Proyecto"
3. Agrega descripción (opcional)

### Organizar Peticiones

1. Crea carpetas dentro de la colección para agrupar
2. Arrastra peticiones a las carpetas
3. Ejemplo de estructura:
   ```
   API E-commerce
   ├── Auth
   │   ├── Login
   │   ├── Register
   │   └── Logout
   ├── Usuarios
   │   ├── GET Todos
   │   ├── GET Por ID
   │   └── POST Crear
   └── Productos
       ├── GET Todos
       └── POST Crear
   ```

### Exportar e Importar

**Exportar**:
1. Click derecho en la colección
2. **Export**
3. Selecciona formato (Collection v2.1 recomendado)
4. Guarda el archivo `.json`

**Importar**:
1. Click en **Import**
2. Selecciona el archivo `.json`
3. La colección se agregará a tu workspace

**Ventajas**:
- ✅ Compartir con tu equipo
- ✅ Versionar en Git
- ✅ Backup de tus peticiones

---

## 6. Scripts de Test Automatizados

Puedes automatizar validaciones básicas en la pestaña **Tests**. Los scripts se ejecutan después de recibir la respuesta.

### Sintaxis Básica

Postman usa JavaScript para los tests. Tienes acceso a:
- `pm.response`: La respuesta recibida
- `pm.request`: La petición enviada
- `pm.environment`: Variables de entorno
- `pm.test()`: Función para crear tests

### Ejemplos de Tests

#### Validar Status Code

```javascript
// Validar que el status sea 200
pm.test("Status es 200", function () {
    pm.response.to.have.status(200);
});

// Validar que el status sea 201 (Created)
pm.test("Status es 201", function () {
    pm.response.to.have.status(201);
});
```

#### Validar Response Time

```javascript
// Validar que la respuesta sea rápida (< 200ms)
pm.test("Response time es menor a 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});
```

#### Validar Estructura de JSON

```javascript
// Validar que la respuesta sea JSON
pm.test("Response es JSON", function () {
    pm.response.to.be.json;
});

// Validar que tenga ciertos campos
pm.test("Response tiene campo 'usuarios'", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('usuarios');
});
```

#### Guardar Token Automáticamente

```javascript
// Guardar el token automáticamente tras el login
pm.test("Guardar token", function () {
    var jsonData = pm.response.json();
    if (jsonData.accessToken) {
        pm.environment.set("token", jsonData.accessToken);
    }
});
```

#### Validar Datos Específicos

```javascript
// Validar que el usuario creado tenga el nombre correcto
pm.test("Usuario tiene nombre correcto", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.nombre).to.eql("Juan");
});
```

#### Validar Array

```javascript
// Validar que la respuesta sea un array con elementos
pm.test("Response es array con elementos", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an('array');
    pm.expect(jsonData.length).to.be.above(0);
});
```

### Tests Avanzados

#### Validar Schema (Estructura Completa)

```javascript
pm.test("Schema es correcto", function () {
    var schema = {
        "type": "object",
        "properties": {
            "id": {"type": "string"},
            "nombre": {"type": "string"},
            "email": {"type": "string"}
        },
        "required": ["id", "nombre", "email"]
    };
    
    pm.response.to.have.jsonSchema(schema);
});
```

#### Validar Múltiples Condiciones

```javascript
pm.test("Validaciones múltiples", function () {
    var jsonData = pm.response.json();
    
    // Status 200
    pm.response.to.have.status(200);
    
    // Tiene datos
    pm.expect(jsonData).to.have.property('data');
    
    // Data es array
    pm.expect(jsonData.data).to.be.an('array');
    
    // Array no está vacío
    pm.expect(jsonData.data.length).to.be.above(0);
});
```

### Resultados de Tests

Después de enviar una petición:
- Ve a la pestaña **Test Results**
- Verás qué tests pasaron (✓) y cuáles fallaron (✗)
- Los tests fallidos mostrarán el error

---

## 7. Autenticación en Postman

Postman soporta múltiples tipos de autenticación.

### Bearer Token (JWT)

**Configuración**:
1. Ve a la pestaña **Authorization**
2. Tipo: **Bearer Token**
3. Token: `{{token}}` (usa variable de entorno)

**Automático con Script**:
```javascript
// En Tests del login
pm.test("Guardar token", function () {
    var jsonData = pm.response.json();
    if (jsonData.accessToken) {
        pm.environment.set("token", jsonData.accessToken);
    }
});
```

### API Key

**Configuración**:
1. Tipo: **API Key**
2. Key: `X-API-Key` (o el nombre que use tu API)
3. Value: `tu-api-key`

### Basic Auth

**Configuración**:
1. Tipo: **Basic Auth**
2. Username: `usuario`
3. Password: `contraseña`

### OAuth 2.0

**Configuración**:
1. Tipo: **OAuth 2.0**
2. Sigue el asistente de Postman
3. Configura callback URL, client ID, etc.

---

## 8. Pre-request Scripts

Los **Pre-request Scripts** se ejecutan **antes** de enviar la petición. Útiles para:
- Generar valores dinámicos
- Calcular timestamps
- Generar tokens temporales

### Ejemplos

#### Generar Timestamp

```javascript
// Generar timestamp actual
pm.environment.set("timestamp", Date.now());
```

#### Generar UUID

```javascript
// Generar UUID aleatorio
pm.environment.set("uuid", pm.variables.replaceIn('{{$randomUUID}}'));
```

#### Calcular Hash

```javascript
// Calcular hash MD5 (requiere crypto-js)
const CryptoJS = require('crypto-js');
const hash = CryptoJS.MD5('texto').toString();
pm.environment.set("hash", hash);
```

#### Headers Dinámicos

```javascript
// Agregar header con timestamp
pm.request.headers.add({
    key: 'X-Timestamp',
    value: Date.now().toString()
});
```

---

## 9. Mejores Prácticas

### Nomenclatura

- ✅ **Nombres Claros**: En vez de "POST Request", pon "Login de Usuario"
- ✅ **Descriptivos**: "GET Usuario por ID" en lugar de "GET /usuarios/:id"
- ✅ **Organizados**: Usa carpetas para agrupar por funcionalidad

### Documentación

- ✅ **Descripciones**: Usa la descripción de Postman para explicar qué campos son obligatorios
- ✅ **Ejemplos**: Incluye ejemplos de request y response
- ✅ **Notas**: Agrega notas sobre casos especiales

**Ejemplo de Descripción**:
```
Endpoint: POST /api/usuarios

Campos requeridos:
- nombre: string (obligatorio)
- email: string (obligatorio, formato email)
- edad: number (opcional)

Ejemplo de respuesta exitosa (201):
{
  "id": "123",
  "nombre": "Juan",
  "email": "juan@example.com",
  "edad": 30
}
```

### Testing Completo

- ✅ **Prueba Errores**: No solo pruebes el camino feliz (200). Envía datos malos para ver si tu backend responde 400 o 500 correctamente
- ✅ **Casos Límite**: Prueba valores extremos (strings muy largos, números negativos, etc.)
- ✅ **Validaciones**: Prueba que las validaciones funcionen (campos requeridos, formatos, etc.)

**Casos de Prueba Recomendados**:
```
✅ Éxito (200/201)
✅ Error de validación (400)
✅ No encontrado (404)
✅ No autorizado (401)
✅ Error del servidor (500)
✅ Datos faltantes
✅ Formatos incorrectos
✅ Valores límite
```

### Organización

- ✅ **Colecciones por Proyecto**: Una colección por proyecto
- ✅ **Carpetas por Módulo**: Agrupa endpoints relacionados
- ✅ **Variables de Entorno**: Usa environments para desarrollo/producción
- ✅ **Versionado**: Exporta colecciones y guárdalas en Git

### Automatización

- ✅ **Tests Automáticos**: Agrega tests a todas las peticiones críticas
- ✅ **Pre-request Scripts**: Usa para valores dinámicos
- ✅ **Runner**: Usa Collection Runner para ejecutar todas las peticiones de una vez

### Colaboración

- ✅ **Compartir Colecciones**: Exporta y comparte con tu equipo
- ✅ **Documentar**: Agrega descripciones claras
- ✅ **Sincronización**: Usa cuenta de Postman para sincronizar entre dispositivos

---

