# Master Guide: Express.js y Backend ⚙️

## 📑 Índice
1. [Introducción a Node.js y Express.js](#1-introducción-a-nodejs-y-expressjs)
2. [Estructura de un Servidor Express](#2-estructura-de-un-servidor-express)
3. [Middleware: El Intermediario](#3-middleware-el-intermediario)
4. [Routing y Controladores](#4-routing-y-controladores)
5. [El Objeto Request (`req`) y Response (`res`)](#5-el-objeto-request-req-y-response-res)
6. [Arquitectura Backend: MVC + Services](#6-arquitectura-backend-mvc--services)
7. [Variables de Entorno (`.env`)](#7-variables-de-entorno-env)
8. [CORS: Cross-Origin Resource Sharing](#8-cors-cross-origin-resource-sharing)
9. [Manejo de Errores](#9-manejo-de-errores)
10. [Buenas Prácticas](#10-buenas-prácticas)

---

## 1. Introducción a Node.js y Express.js

### Node.js (Analogía del Mundo Real)

### 🏠 Analogía: La Casa y el Servidor

Imagina que tienes una casa:
- **JavaScript en el navegador**: Como tener electricidad solo en algunas habitaciones
- **Node.js**: Como tener electricidad en toda la casa, incluyendo el sótano (servidor)

**Antes de Node.js**: JavaScript solo funcionaba en el navegador (frontend).
**Con Node.js**: JavaScript también funciona en el servidor (backend).

**Analogía**: Como tener un idioma (JavaScript) que antes solo se hablaba en un país (navegador), y ahora se habla en todo el mundo (navegador + servidor).

### Node.js

**Node.js** es el entorno que permite ejecutar JavaScript en el servidor. Antes de Node.js, JavaScript solo se ejecutaba en el navegador. Node.js abrió la posibilidad de usar JavaScript para desarrollo backend.

**Características**:
- ✅ Ejecuta JavaScript fuera del navegador
- ✅ Basado en el motor V8 de Chrome
- ✅ Asíncrono y orientado a eventos
- ✅ Ecosistema enorme (npm)

### Express.js (Analogía del Mundo Real)

### 🛠️ Analogía: El Kit de Herramientas

Imagina que quieres construir una casa:
- **Node.js**: Es como tener los materiales básicos (ladrillos, cemento)
- **Express.js**: Es como tener un kit de herramientas completo (martillo, destornillador, nivel)

**Sin Express**: Tienes que construir todo desde cero, paso a paso.
**Con Express**: Tienes herramientas que te facilitan el trabajo.

### 🚗 Analogía: El Auto y el Motor

Piensa en un auto:
- **Node.js**: Es el motor (la potencia)
- **Express.js**: Es el volante, los pedales, el tablero (las herramientas para controlar el motor)

**Express te da las herramientas** para controlar y usar Node.js de forma más fácil.

### Express.js

**Express.js** es el framework web más popular para Node.js. Facilita crear servidores y APIs REST de forma rápida y organizada.

**Características**:
- ✅ Minimalista y flexible
- ✅ Sistema de routing potente
- ✅ Middleware extensible
- ✅ Gran ecosistema de plugins
- ✅ Ideal para APIs REST

**Instalación**:
```bash
npm init -y
npm install express
```

---

## 2. Estructura de un Servidor Express

### Archivo de Entrada Básico (`app.js` o `index.js`)

```javascript
const express = require("express");
const app = express();

// Middlewares
app.use(express.json()); // Permite leer JSON en el body de las peticiones
app.use(express.urlencoded({ extended: true })); // Permite leer formularios

// Rutas
app.get("/", (req, res) => {
  res.send("¡Servidor Funcionando!");
});

// Inicio del servidor
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});
```

### Estructura de Carpetas Recomendada

```
proyecto/
├── src/
│   ├── controllers/     # Controladores (lógica de request/response)
│   ├── models/          # Modelos (Mongoose, esquemas)
│   ├── routes/          # Definición de rutas
│   ├── services/        # Lógica de negocio
│   ├── middlewares/     # Middlewares personalizados
│   ├── utils/           # Utilidades (validators, helpers)
│   ├── config.js        # Configuración
│   └── db.js            # Conexión a base de datos
├── .env                 # Variables de entorno
├── .gitignore          # Archivos a ignorar en Git
└── index.js            # Punto de entrada
```

### Ejemplo Completo con Estructura Organizada

**`index.js` (Punto de Entrada)**:
```javascript
const express = require('express');
const cors = require('cors');
require('dotenv').config();

const app = express();

// Middlewares globales
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Rutas
app.use('/api/usuarios', require('./src/routes/userRoutes'));
app.use('/api/productos', require('./src/routes/productRoutes'));

// Ruta de prueba
app.get('/', (req, res) => {
  res.json({ message: 'API funcionando' });
});

// Manejo de errores
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Error interno del servidor' });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});
```

---

## 3. Middleware: El Intermediario

### 🏭 Analogía: La Cadena de Producción

Piensa en una fábrica con una cadena de producción:
- **Paquete** (request): Va pasando por diferentes estaciones
- **Estación 1**: Verifica que el paquete esté bien empaquetado
- **Estación 2**: Le pone una etiqueta
- **Estación 3**: Lo pesa
- **Estación 4**: Lo envía al destino final

**Cada estación hace su trabajo y pasa el paquete a la siguiente**. Eso es middleware.

### 🚪 Analogía: El Guardia de Seguridad

Imagina que entras a un edificio:
- **Guardia 1** (middleware): Verifica tu identificación
- **Guardia 2** (middleware): Revisa tu bolso
- **Guardia 3** (middleware): Te da un pase
- **Recepción** (ruta final): Te atiende

**Cada guardia hace su trabajo y te deja pasar a la siguiente estación**. Si algún guardia te detiene, no llegas a la recepción.

### 🍕 Analogía: La Cocina de un Restaurante

En un restaurante:
- **Camarero** (middleware): Toma tu pedido
- **Cocinero** (middleware): Prepara la comida
- **Expedidor** (middleware): Verifica que esté bien
- **Camarero** (ruta final): Te trae la comida

**Cada persona en la cadena hace su parte** antes de que llegue a ti.

### ¿Qué es un Middleware?

Un **middleware** es una función que tiene acceso a los objetos de petición (`req`), respuesta (`res`) y a la siguiente función middleware en el ciclo de solicitud-respuesta de una aplicación Express.js.

**En términos simples**: Es como una estación de control que revisa, modifica o procesa la petición antes de que llegue a su destino final.

### Funciones de un Middleware

1. ✅ Ejecutar cualquier código
2. ✅ Modificar los objetos `req` y `res`
3. ✅ Finalizar el ciclo de solicitud-respuesta
4. ✅ Llamar a la siguiente función en la pila de middleware, utilizando la función `next()`

### Tipos de Middleware

#### 1. Middleware Global

Afecta a **todas las rutas**. Se define con `app.use()`.

```javascript
// Middleware global - se ejecuta en todas las peticiones
app.use((req, res, next) => {
  console.log(`Petición recibida: ${req.method} ${req.url}`);
  next(); // Pasa a la siguiente función
});

// Otro middleware global
app.use(express.json()); // Parsea JSON en todas las peticiones
```

#### 2. Middleware Local

Solo afecta a **una ruta específica** o grupo de rutas.

```javascript
// Middleware de autenticación
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) {
    return res.status(401).json({ error: 'Token no proporcionado' });
  }
  // Verificar token...
  next();
};

// Aplicar solo a rutas protegidas
router.get('/perfil', authMiddleware, (req, res) => {
  res.json({ message: 'Perfil del usuario' });
});
```

### Middleware de Terceros

#### express.json()
Parsea el body de las peticiones que vienen en formato JSON.

```javascript
app.use(express.json());
```

**Uso**:
```javascript
// POST /api/usuarios
// Body: { "nombre": "Juan", "email": "juan@example.com" }
router.post('/usuarios', (req, res) => {
  const { nombre, email } = req.body; // Acceso a los datos
  // ...
});
```

#### express.urlencoded()
Parsea el body de las peticiones que vienen de formularios HTML.

```javascript
app.use(express.urlencoded({ extended: true }));
```

#### cors()
Permite peticiones desde diferentes orígenes (necesario para APIs consumidas por frontend).

```javascript
const cors = require('cors');

app.use(cors({
  origin: "*",  // Permitir todas las conexiones
  methods: ["GET", "POST", "PUT", "DELETE", "PATCH"]
}));
```

### Ejemplo de Middleware de Autenticación

```javascript
const jwt = require('jsonwebtoken');

const authMiddleware = (req, res, next) => {
  // Obtener token del header
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // "Bearer TOKEN"
  
  if (!token) {
    return res.status(401).json({ error: 'Token de acceso no proporcionado' });
  }
  
  // Verificar token
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({ error: 'Token de acceso no válido' });
    }
    
    // Agregar información del usuario al request
    req.user = user;
    next(); // Continuar con la siguiente función
  });
};

// Uso
router.get('/datos-protegidos', authMiddleware, (req, res) => {
  // req.user contiene la información del usuario autenticado
  res.json({ message: 'Datos protegidos', user: req.user });
});
```

### ⚠️ Importante: La Función `next()` (Analogía)

**Analogía**: Como decir "siguiente" en una fila.

Imagina que estás en una fila:
- **Sin `next()`**: Es como quedarte parado en la fila sin avanzar. Nadie puede pasar.
- **Con `next()`**: Es como decir "siguiente" y dejar pasar a la siguiente persona.

**`next()` es vital**. Si no la llamas, la petición se queda "colgada" y nunca llega a la ruta final.

```javascript
// ❌ MAL - Sin next(), la petición se queda colgada
app.use((req, res, next) => {
  console.log('Middleware ejecutado');
  // Falta next()
});

// ✅ BIEN - Con next(), la petición continúa
app.use((req, res, next) => {
  console.log('Middleware ejecutado');
  next(); // Pasa a la siguiente función
});
```

---

## 4. Routing y Controladores

### 🏢 Analogía: La Organización de una Empresa

Imagina una empresa bien organizada:
- **Recepción** (Routes): Recibe a los visitantes y los dirige al departamento correcto
- **Gerente** (Controllers): Coordina y decide qué hacer con cada solicitud
- **Empleados Especializados** (Services): Hacen el trabajo real
- **Archivo** (Models): Donde se guarda la información

**Cada parte tiene su responsabilidad**. No mezcles las responsabilidades.

### ¿Por qué Separar?

**Analogía**: Como tener una cocina organizada:
- **Cuchillos** (Routes): Para cortar
- **Tabla de cortar** (Controllers): Para preparar
- **Sartén** (Services): Para cocinar
- **Nevera** (Models): Para guardar ingredientes

**Cada herramienta tiene su lugar**. Si mezclas todo, es un desastre.

No pongas toda la lógica en un solo archivo. Separa las rutas de la lógica de negocio.

### Estructura: Routes → Controllers → Services

```
Routes (Definición de endpoints)
    ↓
Controllers (Manejan req/res)
    ↓
Services (Lógica de negocio)
    ↓
Models (Base de datos)
```

### Routes (Definición de Endpoints)

**`src/routes/userRoutes.js`**:
```javascript
const express = require('express');
const router = express.Router();
const {
  createUserController,
  getAllUsersController,
  getUserByIdController,
  updateUserController,
  deleteUserController
} = require('../controllers/userController');

// Definir endpoints
router.post('/create', createUserController);
router.get('/get', getAllUsersController);
router.get('/get/:id', getUserByIdController);
router.put('/update/:id', updateUserController);
router.delete('/delete/:id', deleteUserController);

module.exports = router;
```

**En `index.js`**:
```javascript
app.use('/api/user', require('./src/routes/userRoutes'));
```

### Controllers (Manejan Request/Response)

**`src/controllers/userController.js`**:
```javascript
const { createUser, getAllUsers, getUserById } = require('../services/userService');

exports.createUserController = async (req, res) => {
  try {
    const { nombre, email, password } = req.body;
    const nuevoUsuario = await createUser({ nombre, email, password });
    res.status(201).json(nuevoUsuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

exports.getAllUsersController = async (req, res) => {
  try {
    const usuarios = await getAllUsers();
    res.status(200).json(usuarios);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

exports.getUserByIdController = async (req, res) => {
  try {
    const { id } = req.params;
    const usuario = await getUserById(id);
    
    if (!usuario) {
      return res.status(404).json({ error: 'Usuario no encontrado' });
    }
    
    res.status(200).json(usuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};
```

### Services (Lógica de Negocio)

**`src/services/userService.js`**:
```javascript
const User = require('../models/userModel');

exports.createUser = async (userData) => {
  const nuevoUsuario = new User(userData);
  await nuevoUsuario.save();
  return nuevoUsuario;
};

exports.getAllUsers = async () => {
  return await User.find();
};

exports.getUserById = async (id) => {
  return await User.findById(id);
};
```

### Ventajas de esta Arquitectura

- ✅ **Separación de responsabilidades**: Cada capa tiene un propósito claro
- ✅ **Reutilización**: Los services pueden ser usados por múltiples controllers
- ✅ **Mantenibilidad**: Fácil de encontrar y modificar código
- ✅ **Testabilidad**: Cada capa se puede testear independientemente

---

## 5. El Objeto Request (`req`) y Response (`res`)

### 📦 Analogía: La Carta y la Respuesta

Imagina que envías una carta:
- **Request (`req`)**: Es la carta que recibes. Contiene toda la información que el remitente envió.
- **Response (`res`)**: Es la carta que envías de vuelta. Contiene tu respuesta.

### El Objeto Request (`req`)

**Analogía**: Como una carta que recibes. Tiene:
- **El sobre** (`req.headers`): Información sobre quién la envió
- **El contenido** (`req.body`): Lo que está dentro de la carta
- **La dirección** (`req.url`): A dónde estaba dirigida
- **El método** (`req.method`): Cómo la enviaron (correo, mensajero, etc.)

Contiene información sobre la petición HTTP que el cliente envió.

#### Propiedades Principales

| Propiedad | Descripción | Ejemplo |
|-----------|-------------|---------|
| **`req.body`** | Datos enviados por el cliente (común en POST/PUT/PATCH) | `{ nombre: "Juan" }` |
| **`req.params`** | Datos de la URL dinámica (`:id`) | `/usuarios/:id` → `req.params.id` |
| **`req.query`** | Datos de búsqueda tras el signo `?` | `/usuarios?page=1` → `req.query.page` |
| **`req.headers`** | Encabezados HTTP de la petición | `req.headers.authorization` |
| **`req.method`** | Método HTTP (GET, POST, etc.) | `req.method = "GET"` |
| **`req.url`** | URL de la petición | `req.url = "/api/usuarios"` |
| **`req.path`** | Ruta de la petición | `req.path = "/api/usuarios"` |

#### Ejemplos de Uso

**`req.body`** (Datos en el cuerpo de la petición):
```javascript
// POST /api/usuarios
// Body: { "nombre": "Juan", "email": "juan@example.com" }
router.post('/usuarios', (req, res) => {
  const { nombre, email } = req.body;
  // nombre = "Juan"
  // email = "juan@example.com"
});
```

**`req.params`** (Parámetros de ruta):
```javascript
// GET /api/usuarios/5
router.get('/usuarios/:id', (req, res) => {
  const userId = req.params.id; // userId = "5"
});
```

**`req.query`** (Parámetros de consulta):
```javascript
// GET /api/usuarios?page=1&limit=10
router.get('/usuarios', (req, res) => {
  const page = req.query.page;    // page = "1"
  const limit = req.query.limit;  // limit = "10"
});
```

**`req.headers`** (Encabezados HTTP):
```javascript
// Petición con header: Authorization: Bearer token123
router.get('/datos-protegidos', (req, res) => {
  const authHeader = req.headers.authorization;
  // authHeader = "Bearer token123"
});
```

### El Objeto Response (`res`)

**Analogía**: Como escribir y enviar una carta de respuesta.

Tienes diferentes formas de responder:
- **`res.json()`**: Como enviar una carta con datos estructurados
- **`res.send()`**: Como enviar una carta simple con texto
- **`res.status()`**: Como poner un sello que indica el estado (urgente, normal, etc.)
- **`res.redirect()`**: Como decir "ve a otra dirección"

Contiene métodos para enviar respuestas al cliente.

#### Métodos Principales

| Método | Descripción | Ejemplo |
|--------|-------------|---------|
| **`res.status(code)`** | Define el código de estado HTTP | `res.status(404)` |
| **`res.json(data)`** | Envía datos formateados como JSON | `res.json({ usuario: {...} })` |
| **`res.send(data)`** | Envía una respuesta (texto, HTML, JSON) | `res.send("Hola")` |
| **`res.redirect(url)`** | Redirige a otra URL | `res.redirect('/login')` |
| **`res.render(view)`** | Renderiza una vista (con Handlebars) | `res.render('home')` |

#### Ejemplos de Uso

**`res.status()` y `res.json()`**:
```javascript
router.get('/usuarios/:id', (req, res) => {
  const usuario = obtenerUsuario(req.params.id);
  
  if (!usuario) {
    return res.status(404).json({ error: 'Usuario no encontrado' });
  }
  
  res.status(200).json(usuario);
});
```

**`res.send()`**:
```javascript
router.get('/', (req, res) => {
  res.send('¡Servidor Funcionando!');
});
```

**Encadenamiento**:
```javascript
// res.status() retorna el objeto res, permitiendo encadenar
res.status(201).json({ message: 'Usuario creado', usuario: nuevoUsuario });
```

---

## 6. Arquitectura Backend: MVC + Services

Para tus alumnos, es vital diferenciar los roles de cada componente en una aplicación de backend.

### Componentes de la Arquitectura

#### API vs Base de Datos (DB)

- **Base de Datos**: Es el sistema de almacenamiento persistente
- **API**: Es la interfaz de comunicación que permite a la aplicación interactuar con esa base de datos

#### API vs Servidor

- **Servidor**: Es la máquina o el software (Express.js) que se está ejecutando
- **API**: Es el conjunto de reglas y endpoints que define cómo interactuar con ese servidor

### Ciclo de Petición-Respuesta

```
Cliente (Frontend)
    ↓
API (Endpoints)
    ↓
Routes (Definición de rutas)
    ↓
Controllers (Manejan request/response)
    ↓
Services (Lógica de negocio)
    ↓
Models (Interacción con base de datos)
    ↓
Base de Datos (MongoDB/MySQL)
    ↓
Response (Respuesta JSON)
    ↓
Cliente (Frontend)
```

### Flujo Completo

1. **Cliente** envía una petición a un **servidor** Express.js a través de una **ruta**
2. El servidor usa un **controlador** para procesar la petición
3. El controlador puede invocar un **servicio** para ejecutar la lógica de negocio
4. El servicio interactúa con la **base de datos** a través de **modelos**
5. Finalmente, el servidor envía una **respuesta** al cliente

### Ejemplo Completo

**Route** (`src/routes/userRoutes.js`):
```javascript
router.post('/create', createUserController);
```

**Controller** (`src/controllers/userController.js`):
```javascript
exports.createUserController = async (req, res) => {
  try {
    const { nombre, email, password } = req.body;
    const nuevoUsuario = await createUser({ nombre, email, password });
    res.status(201).json(nuevoUsuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};
```

**Service** (`src/services/userService.js`):
```javascript
exports.createUser = async (userData) => {
  // Validar datos
  if (!userData.email || !userData.password) {
    throw new Error('Email y password son requeridos');
  }
  
  // Hash password
  const hashedPassword = await bcrypt.hash(userData.password, 10);
  
  // Crear usuario
  const nuevoUsuario = new User({
    ...userData,
    password: hashedPassword
  });
  
  await nuevoUsuario.save();
  return nuevoUsuario;
};
```

**Model** (`src/models/userModel.js`):
```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  nombre: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
}, {
  timestamps: true
});

module.exports = mongoose.model('User', userSchema);
```

---

## 7. Variables de Entorno (`.env`)

¡Nunca subas contraseñas o API Keys a GitHub!

### Configuración Básica

1. **Instalar dotenv**:
```bash
npm install dotenv
```

2. **Crear archivo `.env`** en la raíz:
```env
PORT=3000
DB_URL=mongodb://localhost:27017/mi-app
JWT_SECRET=mi_secreto_super_seguro
API_KEY=mi_api_key_secreta
```

3. **Cargar en `index.js`**:
```javascript
require('dotenv').config();

const PORT = process.env.PORT || 3000;
const DB_URL = process.env.DB_URL;
const JWT_SECRET = process.env.JWT_SECRET;
```

4. **Añadir `.env` a `.gitignore`**:
```
node_modules/
.env
.env.local
```

### Ejemplo de Uso

```javascript
require('dotenv').config();
const mongoose = require('mongoose');

// Conectar a MongoDB usando variable de entorno
mongoose.connect(process.env.DB_URL, {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor en puerto ${PORT}`);
});
```

### Variables de Entorno en Node.js vs Vite

| Aspecto | Node.js (Backend) | Vite (Frontend) |
|---------|-------------------|-----------------|
| **Acceso** | `process.env.VARIABLE` | `import.meta.env.VITE_VARIABLE` |
| **Prefijo** | Ninguno | `VITE_` (requerido) |
| **Archivo** | `.env` | `.env` |
| **Carga** | `require('dotenv').config()` | Automático |

---

## 8. CORS: Cross-Origin Resource Sharing

**CORS** permite que un frontend en un origen (ej. `http://localhost:3000`) haga peticiones a un backend en otro origen (ej. `http://localhost:8080`).

### ¿Por qué es Necesario?

Por defecto, los navegadores bloquean peticiones entre diferentes orígenes por seguridad. CORS permite configurar qué orígenes pueden hacer peticiones a tu API.

### Configuración Básica

```javascript
const cors = require('cors');

// Permitir todas las conexiones
app.use(cors());

// O configurar específicamente
app.use(cors({
  origin: "http://localhost:3000",  // Solo permitir este origen
  methods: ["GET", "POST", "PUT", "DELETE", "PATCH"],
  credentials: true  // Permitir cookies
}));
```

### Configuración Avanzada

```javascript
const cors = require('cors');

const corsOptions = {
  origin: function (origin, callback) {
    // Lista de orígenes permitidos
    const allowedOrigins = [
      'http://localhost:3000',
      'https://mi-app.com'
    ];
    
    if (!origin || allowedOrigins.indexOf(origin) !== -1) {
      callback(null, true);
    } else {
      callback(new Error('No permitido por CORS'));
    }
  },
  methods: ["GET", "POST", "PUT", "DELETE", "PATCH"],
  credentials: true
};

app.use(cors(corsOptions));
```

### Cuándo Usar CORS

- ✅ **API REST**: Cuando tu backend es una API consumida por un frontend en diferente puerto
- ❌ **Server con Handlebars**: No es necesario (mismo origen)

---

## 9. Manejo de Errores

### Try-Catch en Controllers

```javascript
exports.createUserController = async (req, res) => {
  try {
    const { nombre, email, password } = req.body;
    
    // Validar datos
    if (!nombre || !email || !password) {
      return res.status(400).json({ 
        error: 'Todos los campos son requeridos' 
      });
    }
    
    const nuevoUsuario = await createUser({ nombre, email, password });
    res.status(201).json(nuevoUsuario);
    
  } catch (error) {
    console.error('Error al crear usuario:', error);
    res.status(500).json({ 
      error: 'Error interno del servidor',
      message: error.message 
    });
  }
};
```

### Middleware de Manejo de Errores Global

```javascript
// Al final de todas las rutas
app.use((err, req, res, next) => {
  console.error(err.stack);
  
  res.status(err.status || 500).json({
    error: err.message || 'Error interno del servidor',
    stack: process.env.NODE_ENV === 'development' ? err.stack : undefined
  });
});
```

### Errores Personalizados

```javascript
// Crear error personalizado
class NotFoundError extends Error {
  constructor(message) {
    super(message);
    this.status = 404;
    this.name = 'NotFoundError';
  }
}

// Usar en service
exports.getUserById = async (id) => {
  const usuario = await User.findById(id);
  if (!usuario) {
    throw new NotFoundError('Usuario no encontrado');
  }
  return usuario;
};

// Manejar en controller
exports.getUserByIdController = async (req, res, next) => {
  try {
    const usuario = await getUserById(req.params.id);
    res.json(usuario);
  } catch (error) {
    next(error); // Pasa al middleware de errores
  }
};
```

---

## 10. Buenas Prácticas

### Estructura de Proyecto

- ✅ Separar routes, controllers, services, models
- ✅ Usar nombres descriptivos
- ✅ Agrupar por funcionalidad (usuarios, productos, etc.)

### Código

- ✅ Usar async/await para operaciones asíncronas
- ✅ Validar todos los inputs
- ✅ Manejar errores apropiadamente
- ✅ Usar variables de entorno para configuraciones sensibles
- ✅ Documentar endpoints

### Seguridad

- ✅ Nunca exponer información sensible
- ✅ Validar y sanitizar inputs
- ✅ Usar HTTPS en producción
- ✅ Implementar autenticación y autorización
- ✅ Rate limiting para prevenir abusos

### Performance

- ✅ Usar índices en base de datos
- ✅ Implementar paginación
- ✅ Cachear respuestas cuando sea apropiado
- ✅ Optimizar consultas a base de datos

---

## Referencias Relacionadas

### Temas Relacionados

- 📚 [API REST](./11-API-REST.md) - Express se usa para crear APIs REST
- 📚 [MongoDB](./10-MongoDB.md) - Conectar Express con MongoDB
- 📚 [MySQL](./09-MySQL.md) - Conectar Express con MySQL
- 📚 [Auth JWT](./13-Auth-JWT.md) - Autenticación en Express
- 📚 [MVC Handlebars](./14-MVC-Handlebars.md) - Express con Handlebars para renderizar HTML
- 📚 [Node.js](./15-NodeJS.md) - Fundamentos de Node.js antes de Express

### Código Relacionado

- 💻 [Ejemplos de Express](../../CODIGO/backend/tema-12-api-rest-basica/)

---

## 🎯 Puntos Clave para Recordar

1. **Express = Kit de herramientas**: Facilita crear servidores con Node.js
2. **Middleware = Estaciones de control**: Procesan peticiones antes de llegar a las rutas
3. **Routes → Controllers → Services**: Separación de responsabilidades
4. **`next()` = Pasar al siguiente**: Vital en middleware para continuar el flujo
5. **`req` = Petición recibida**: Contiene toda la información del cliente
6. **`res` = Respuesta a enviar**: Métodos para responder al cliente

---

## 💡 Ejercicio Mental

Piensa en Express como una empresa bien organizada:
- **Recepción** (Routes): Recibe visitantes
- **Gerentes** (Controllers): Coordinan
- **Empleados** (Services): Hacen el trabajo
- **Archivo** (Models): Guarda información

¡Practica identificando cada parte en tus proyectos!

---

