# Node.js: Entorno de Ejecución 🟢

## ¿Qué es Node.js?

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

