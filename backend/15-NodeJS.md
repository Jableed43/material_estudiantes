# Node.js: Entorno de Ejecución 🟢

## 📑 Índice

1. [¿Qué es Node.js? (Analogía del Mundo Real)](#qué-es-nodejs-analogía-del-mundo-real)
2. [Instalación](#instalación)
3. [Módulos](#módulos)
4. [Sistema de Archivos (fs)](#sistema-de-archivos-fs)
5. [HTTP Server](#http-server)
6. [Conceptos Clave](#conceptos-clave)
7. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es Node.js? (Analogía del Mundo Real)

### 🏠 Analogía: La Casa y el Servidor

Imagina que tienes una casa:
- **JavaScript en el navegador**: Como tener electricidad solo en algunas habitaciones (frontend)
- **Node.js**: Como tener electricidad en toda la casa, incluyendo el sótano (servidor)

**Antes de Node.js**: JavaScript solo funcionaba en el navegador (frontend).
**Con Node.js**: JavaScript también funciona en el servidor (backend).

**Analogía**: Como tener un idioma (JavaScript) que antes solo se hablaba en un país (navegador), y ahora se habla en todo el mundo (navegador + servidor).

### 🌍 Analogía: El Idioma Universal

Piensa en JavaScript como un idioma:
- **Antes**: Solo se hablaba en el "país navegador" (frontend)
- **Con Node.js**: Ahora se habla también en el "país servidor" (backend)
- **Ventaja**: Puedes usar el mismo idioma (JavaScript) en ambos lados

**Beneficio**: No necesitas aprender otro idioma (lenguaje) para el backend.

### 🚗 Analogía: El Motor Universal

Imagina un motor:
- **JavaScript en navegador**: Como un motor que solo funciona en autos (frontend)
- **Node.js**: Como el mismo motor pero que ahora también funciona en barcos y aviones (backend)

**El mismo motor (JavaScript) ahora funciona en diferentes lugares**.

### ¿Qué es Node.js?

Node.js es el entorno que permite ejecutar JavaScript en el servidor. Antes de Node.js, JavaScript solo se ejecutaba en el navegador.

**Características**:
- ✅ Ejecuta JavaScript fuera del navegador
- ✅ Basado en el motor V8 de Chrome
- ✅ Asíncrono y orientado a eventos
- ✅ Ecosistema enorme (npm)

## Instalación

```bash
# Verificar instalación
node --version

# Ejecutar archivo JavaScript
node archivo.js
```

## Módulos

### Exportar

```javascript
// module.js
function sumar(a, b) {
    return a + b
}

module.exports = sumar
// O
module.exports = { sumar, restar }
```

### Importar

```javascript
// app.js
const sumar = require('./module')
// O
const { sumar, restar } = require('./module')
```

## ES6 Modules

```javascript
// module.js
export function sumar(a, b) {
    return a + b
}

// app.js
import { sumar } from './module.js'
```

## Sistema de Archivos (fs)

```javascript
const fs = require('fs')

// Leer archivo
fs.readFile('archivo.txt', 'utf8', (err, data) => {
    if (err) throw err
    console.log(data)
})

// Escribir archivo
fs.writeFile('archivo.txt', 'contenido', (err) => {
    if (err) throw err
})
```

## HTTP Server

```javascript
const http = require('http')

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' })
    res.end('Hola desde Node.js')
})

server.listen(3000, () => {
    console.log('Servidor en puerto 3000')
})
```

## Conceptos Clave

1. **Node.js**: Entorno de ejecución de JavaScript
2. **Módulos**: Sistema de importación/exportación
3. **Asíncrono**: Operaciones no bloqueantes
4. **NPM**: Gestor de paquetes
5. **Event Loop**: Manejo de eventos asíncronos
6. **CommonJS**: Sistema de módulos tradicional
7. **ES Modules**: Sistema de módulos moderno

