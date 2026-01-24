# Master Guide: Autenticación con JWT y Bcrypt 🔐

## 📑 Índice
1. [Introducción a la Seguridad Web](#1-introducción-a-la-seguridad-web)
2. [Hashing con Bcrypt](#2-hashing-con-bcrypt)
3. [JWT (JSON Web Token)](#3-jwt-json-web-token)
4. [Flujo de Autenticación Completo](#4-flujo-de-autenticación-completo)
5. [Middleware de Autenticación](#5-middleware-de-autenticación)
6. [Sesiones: Backend vs Frontend](#6-sesiones-backend-vs-frontend)
7. [Implementación Completa](#7-implementación-completa)
8. [Buenas Prácticas de Seguridad](#8-buenas-prácticas-de-seguridad)

---

## 1. Introducción a la Seguridad Web

Manejar la seguridad es la parte más crítica del backend. Sin una seguridad adecuada, tu aplicación está expuesta a múltiples vulnerabilidades.

### Conceptos Clave

- **Autenticación**: Verificar quién es el usuario (login)
- **Autorización**: Verificar qué puede hacer el usuario (permisos)
- **Hashing**: Convertir contraseñas en valores irreversibles
- **Tokens**: Credenciales temporales para autenticación

### Principios Fundamentales

1. ✅ **Nunca guardar contraseñas en texto plano**
2. ✅ **Usar HTTPS en producción**
3. ✅ **Validar y sanitizar todos los inputs**
4. ✅ **Implementar rate limiting**
5. ✅ **Usar tokens seguros con expiración**

---

## 2. Hashing con Bcrypt

**Nunca** guardes contraseñas en texto plano. Si hackean tu base de datos, todos estarán expuestos.

### ¿Qué es Hashing? (Analogía del Mundo Real)

### 🔒 Analogía: La Caja Fuerte con Combinación

Imagina que tienes una caja fuerte:
- **Contraseña original**: "1234" (como la combinación que sabes)
- **Hash**: `$2b$10$S9...` (como el mecanismo interno de la caja fuerte)

**Características**:
- Puedes poner la combinación (contraseña) y abrir la caja
- Pero no puedes ver la combinación mirando el mecanismo interno (hash)
- Es unidireccional: combinación → mecanismo funciona, pero mecanismo → combinación no funciona

### 🍳 Analogía: Cocinar un Huevo

Piensa en cocinar un huevo:
- **Contraseña original**: El huevo crudo
- **Hash**: El huevo cocido

**Puedes**:
- Convertir huevo crudo en huevo cocido (contraseña → hash) ✅
- Verificar que un huevo cocido viene de un huevo crudo (comparar) ✅

**NO puedes**:
- Convertir huevo cocido de vuelta a huevo crudo (hash → contraseña) ❌

### 🔐 Analogía: La Huella Digital

Tu huella digital:
- **Contraseña original**: Tu dedo
- **Hash**: La huella digital

**Puedes**:
- Crear una huella de tu dedo (contraseña → hash) ✅
- Comparar una huella con tu dedo para verificar (comparar) ✅

**NO puedes**:
- Recrear tu dedo completo solo con la huella (hash → contraseña) ❌

**Hashing** es el proceso de convertir la contraseña "1234" en algo como `$2b$10$S9...` (un hash). Es una función unidireccional: puedes convertir la contraseña en hash, pero no puedes convertir el hash de vuelta a la contraseña original.

### ¿Qué es Bcrypt?

**Bcrypt** es un algoritmo de hashing diseñado específicamente para contraseñas. Es lento intencionalmente para hacer más difícil los ataques de fuerza bruta.

### Características de Bcrypt

- ✅ **Unidireccional**: No se puede revertir
- ✅ **Lento**: Dificulta ataques de fuerza bruta
- ✅ **Salt automático**: Añade ruido extra para mayor seguridad
- ✅ **Rounds configurable**: Más rounds = más seguro pero más lento

### Instalación

```bash
npm install bcrypt
```

### Uso de Bcrypt

#### Hash de Contraseña (Al Registrar)

```javascript
const bcrypt = require('bcrypt');

// Hash de contraseña con 10 rounds (recomendado)
const hash = await bcrypt.hash(password, 10);

// Guardar hash en base de datos
const usuario = new User({
  nombre: 'Juan',
  email: 'juan@example.com',
  password: hash  // Guardar el hash, NO la contraseña original
});
await usuario.save();
```

**Ejemplo Completo de Registro**:
```javascript
const bcrypt = require('bcrypt');
const User = require('../models/userModel');

exports.registerUser = async (req, res) => {
  try {
    const { username, email, password } = req.body;
    
    // Verificar si el usuario ya existe
    const existingUser = await User.findOne({ username });
    if (existingUser) {
      return res.status(400).json({ error: 'Usuario ya existe' });
    }
    
    // Hash de la contraseña
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Crear usuario
    const user = new User({
      username,
      email,
      password: hashedPassword  // Guardar hash, no la contraseña
    });
    
    await user.save();
    res.status(201).json({ message: 'Usuario registrado correctamente' });
    
  } catch (error) {
    res.status(500).json({ error: 'Error al registrar usuario' });
  }
};
```

#### Comparar Contraseña (Al Loguear)

```javascript
const bcrypt = require('bcrypt');

// Comparar contraseña ingresada con hash guardado
const match = await bcrypt.compare(password, hashEnDB);

if (match) {
  // Contraseña correcta
} else {
  // Contraseña incorrecta
}
```

**Ejemplo Completo de Login**:
```javascript
const bcrypt = require('bcrypt');
const User = require('../models/userModel');

exports.loginUser = async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // Buscar usuario
    const user = await User.findOne({ username });
    if (!user) {
      return res.status(400).json({ error: 'Usuario no encontrado' });
    }
    
    // Comparar contraseña
    const match = await bcrypt.compare(password, user.password);
    
    if (match) {
      // Contraseña correcta - generar token JWT
      const accessToken = jwt.sign(
        { username: user.username, userId: user._id },
        process.env.JWT_SECRET,
        { expiresIn: '24h' }
      );
      
      res.json({ accessToken: accessToken });
    } else {
      res.status(401).json({ error: 'Credenciales incorrectas' });
    }
    
  } catch (error) {
    res.status(500).json({ error: 'Error al iniciar sesión' });
  }
};
```

### Rounds (Nivel de Seguridad)

El número de "rounds" determina cuántas veces se ejecuta el algoritmo. Más rounds = más seguro pero más lento.

```javascript
// 10 rounds (recomendado - balance entre seguridad y velocidad)
const hash = await bcrypt.hash(password, 10);

// 12 rounds (más seguro pero más lento)
const hash = await bcrypt.hash(password, 12);
```

**Recomendación**: Usa 10 rounds para la mayoría de aplicaciones.

---

## 3. JWT (JSON Web Token)

### 🎫 Analogía: El Pase de Acceso Temporal

Imagina que vas a un evento:
- **Login**: Te identificas en la entrada (email y contraseña)
- **JWT**: Recibes un pase con tu información (nombre, tipo de acceso, validez)
- **Uso**: Muestras el pase cada vez que quieres entrar a una sección
- **Expiración**: El pase tiene una fecha de vencimiento

**Características**:
- No necesitas volver a identificarte cada vez
- El pase contiene tu información
- El pase expira después de un tiempo
- Si pierdes el pase, puedes pedir uno nuevo

### 🚗 Analogía: El Permiso de Conducir

Tu licencia de conducir:
- **Login**: Te identificas para obtenerla (documentos, examen)
- **JWT**: La licencia contiene tu información (nombre, tipo, fecha de vencimiento)
- **Uso**: La muestras cuando te la piden
- **Expiración**: Tiene una fecha de vencimiento

**Ventajas**:
- No necesitas llevar todos tus documentos cada vez
- La licencia contiene la información necesaria
- Es temporal (debe renovarse)

### 🎟️ Analogía: El Ticket de Cine

Un ticket de cine:
- **Login**: Compras el ticket (te identificas y pagas)
- **JWT**: El ticket contiene información (película, asiento, hora)
- **Uso**: Muestras el ticket para entrar
- **Expiración**: El ticket solo es válido para esa función

Es como un "pase VIP". Una vez que el usuario se loguea con éxito, el servidor le envía un token. El usuario lo guarda y lo envía en cada nueva petición.

### ¿Qué es JWT?

**JWT (JSON Web Token)** es un estándar abierto (RFC 7519) que define una forma compacta y autónoma de transmitir información de forma segura entre partes como un objeto JSON.

### Estructura de un JWT

Un JWT tiene **3 partes** separadas por puntos (`.`):

```
header.payload.signature
```

#### 1. Header (Encabezado)

Contiene información sobre el tipo de token y el algoritmo de firma.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### 2. Payload (Carga Útil)

Contiene los datos (claims). **¡No pongas información sensible aquí!** El payload puede ser leído por cualquiera.

```json
{
  "userId": "123",
  "username": "juan",
  "iat": 1516239022,
  "exp": 1516325422
}
```

**Claims comunes**:
- `userId`: ID del usuario
- `username`: Nombre de usuario
- `iat`: Fecha de emisión (issued at)
- `exp`: Fecha de expiración (expiration)

#### 3. Signature (Firma)

Valida que el token no ha sido alterado. Se crea usando el header, el payload y una clave secreta.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

### Ejemplo de JWT Completo

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjMiLCJ1c2VybmFtZSI6Imp1YW4iLCJpYXQiOjE1MTYyMzkwMjIsImV4cCI6MTUxNjMyNTQyMn0.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### Instalación

```bash
npm install jsonwebtoken
```

### Generar Token (Sign)

```javascript
const jwt = require('jsonwebtoken');

// Generar token
const accessToken = jwt.sign(
  { 
    username: user.username,
    userId: user._id 
  },
  process.env.JWT_SECRET,  // Clave secreta
  { expiresIn: '24h' }      // Tiempo de expiración
);

// Enviar al cliente
res.json({ accessToken: accessToken });
```

### Verificar Token (Verify)

```javascript
const jwt = require('jsonwebtoken');

// Verificar token
jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => {
  if (err) {
    // Token inválido o expirado
    return res.status(403).json({ error: 'Token inválido' });
  }
  
  // Token válido - decoded contiene los datos del payload
  req.user = decoded;
  next();
});
```

### Opciones de Expiración

```javascript
// 24 horas
jwt.sign(payload, secret, { expiresIn: '24h' });

// 7 días
jwt.sign(payload, secret, { expiresIn: '7d' });

// 15 minutos
jwt.sign(payload, secret, { expiresIn: '15m' });

// En segundos
jwt.sign(payload, secret, { expiresIn: 3600 }); // 1 hora
```

---

## 4. Flujo de Autenticación Completo

### Paso a Paso

1. **Frontend**: Usuario ingresa credenciales y envía a `/api/login`
2. **Backend**: Verifica credenciales en base de datos
3. **Backend**: Si son correctas, genera un `accessToken` usando `JWT_SECRET`
4. **Backend**: Envía el token al frontend
5. **Frontend**: Guarda el token (en `localStorage`, `sessionStorage`, o cookies)
6. **Peticiones Protegidas**: El frontend envía el token en el Header: `Authorization: Bearer <TOKEN>`
7. **Middleware Auth**: El backend valida el token. Si es válido, deja pasar (`next()`)

### Diagrama de Flujo

```
Usuario → Login → Backend verifica → Token generado
    ↓
Token guardado en Frontend
    ↓
Petición protegida → Header: Authorization: Bearer TOKEN
    ↓
Middleware valida token → Si válido: next() → Controller
```

### Ejemplo Completo de Login

**Controller** (`src/controllers/authController.js`):
```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const User = require('../models/userModel');

exports.loginUser = async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // 1. Buscar usuario
    const user = await User.findOne({ username });
    if (!user) {
      return res.status(400).json({ error: 'Usuario no encontrado' });
    }
    
    // 2. Comparar contraseña
    const match = await bcrypt.compare(password, user.password);
    if (!match) {
      return res.status(401).json({ error: 'Credenciales incorrectas' });
    }
    
    // 3. Generar token JWT
    const accessToken = jwt.sign(
      { 
        userId: user._id,
        username: user.username 
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    // 4. Enviar token al cliente
    res.json({ 
      accessToken: accessToken,
      user: {
        id: user._id,
        username: user.username,
        email: user.email
      }
    });
    
  } catch (error) {
    res.status(500).json({ error: 'Error al iniciar sesión' });
  }
};
```

### Uso del Token en el Frontend

```javascript
// 1. Login y guardar token
const response = await fetch('/api/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ username, password })
});

const { accessToken } = await response.json();
localStorage.setItem('token', accessToken);

// 2. Usar token en peticiones protegidas
const token = localStorage.getItem('token');
const response = await fetch('/api/datos-protegidos', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});
```

---

## 5. Middleware de Autenticación

El middleware de autenticación verifica el token antes de permitir el acceso a rutas protegidas.

### Middleware Básico

**`src/middlewares/authMiddleware.js`**:
```javascript
const jwt = require('jsonwebtoken');

module.exports = (req, res, next) => {
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
    req.user = user; // { userId, username, ... }
    next(); // Continuar con la siguiente función
  });
};
```

### Uso del Middleware

**En Routes**:
```javascript
const express = require('express');
const router = express.Router();
const authMiddleware = require('../middlewares/authMiddleware');
const { getProfile, updateProfile } = require('../controllers/userController');

// Ruta pública (sin middleware)
router.post('/register', registerUser);
router.post('/login', loginUser);

// Rutas protegidas (con middleware)
router.get('/profile', authMiddleware, getProfile);
router.put('/profile', authMiddleware, updateProfile);
router.get('/datos-protegidos', authMiddleware, (req, res) => {
  // req.user contiene la información del usuario autenticado
  res.json({ 
    message: 'Datos protegidos',
    user: req.user 
  });
});
```

### Versión con Async/Await

```javascript
const jwt = require('jsonwebtoken');

module.exports = async (req, res, next) => {
  try {
    const authHeader = req.headers['authorization'];
    const token = authHeader?.split(' ')[1];
    
    if (!token) {
      return res.status(401).json({ error: 'Token no proporcionado' });
    }
    
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
    
  } catch (error) {
    return res.status(403).json({ error: 'Token inválido' });
  }
};
```

---

## 6. Sesiones: Backend vs Frontend

Hay dos enfoques principales para manejar la autenticación: sesiones gestionadas en el backend o tokens gestionados en el frontend.

### Sesiones Gestionadas en el Backend

#### ¿Cómo Funciona?

1. **Inicio de Sesión**: Cuando un usuario inicia sesión, el backend genera un token JWT y lo guarda en la sesión del servidor (`req.session.token`).
2. **Cookie**: El servidor asigna una cookie al cliente. Esta cookie contiene un identificador único de sesión (generalmente un valor como `connect.sid`).
3. **Peticiones Posteriores**: El navegador envía automáticamente la cookie al servidor en cada solicitud al mismo dominio o subdominio.
4. **Middleware de Sesiones**: El middleware de sesiones en el backend (express-session) usa la cookie para identificar la sesión del usuario en el servidor.
5. **Protección de Rutas**: Las rutas protegidas verifican si existe el token en la sesión (`req.session.token`) y lo validan.

#### Ventajas

- ✅ **Transparencia para el cliente**: El cliente no necesita manejar directamente el token. Solo tiene que confiar en que el navegador envía la cookie automáticamente.
- ✅ **Seguridad**: Las cookies se pueden marcar como `HttpOnly` y `Secure` para reducir riesgos como el Cross-Site Scripting (XSS) y el envío por conexiones inseguras.
- ✅ **Token no expuesto**: El token no está expuesto en el frontend.
- ✅ **Control centralizado**: Toda la información de sesión se almacena en el backend, lo que facilita la gestión (como invalidar sesiones).

#### Desventajas

- ⚠️ **Escalabilidad**: Las sesiones gestionadas en el backend pueden no ser ideales para aplicaciones de alta escalabilidad. Necesitarás sincronizar las sesiones si usas múltiples servidores (por ejemplo, usando Redis o una base de datos compartida).
- ⚠️ **Dependencia del servidor**: El cliente depende completamente del backend para manejar la sesión. Si el servidor se reinicia, es posible que se pierdan las sesiones a menos que se persistan en una base de datos.
- ⚠️ **Compatibilidad con APIs externas**: Si tu aplicación consume APIs externas o deseas exponer tu API para integraciones, usar sesiones del lado del servidor puede ser menos flexible en comparación con los tokens gestionados en el frontend.

#### Ejemplo de Implementación

```javascript
const express = require('express');
const session = require('express-session');
const jwt = require('jsonwebtoken');

const app = express();

// Configurar sesiones
app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    httpOnly: true,
    secure: process.env.NODE_ENV === 'production',
    maxAge: 24 * 60 * 60 * 1000 // 24 horas
  }
}));

// Login
app.post('/login', async (req, res) => {
  // Verificar credenciales...
  const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET);
  
  // Guardar token en sesión
  req.session.token = token;
  res.json({ message: 'Login exitoso' });
});

// Ruta protegida
app.get('/datos-protegidos', (req, res) => {
  if (!req.session.token) {
    return res.status(401).json({ error: 'No autenticado' });
  }
  
  // Verificar token
  jwt.verify(req.session.token, process.env.JWT_SECRET, (err, decoded) => {
    if (err) {
      return res.status(403).json({ error: 'Token inválido' });
    }
    res.json({ message: 'Datos protegidos', user: decoded });
  });
});
```

### Tokens Gestionados en el Frontend

#### ¿Cómo Funciona?

1. **Inicio de Sesión**: El cliente (tu aplicación React) envía las credenciales del usuario al backend en el login.
2. **Backend Verifica**: El backend verifica las credenciales y, si son correctas, devuelve un token JWT al cliente.
3. **Almacenamiento del Token**: El cliente guarda el token en un lugar seguro:
   - `localStorage`: Persiste incluso si el usuario cierra la pestaña o recarga la página
   - `sessionStorage`: Solo persiste mientras el navegador esté abierto
   - Cookie segura: Similar a las sesiones, pero aquí el cliente gestiona el token
4. **Peticiones Autenticadas**: En cada solicitud protegida, el cliente incluye el token en los encabezados (header): `Authorization: Bearer <TOKEN>`.
5. **Backend Valida**: El backend verifica la validez del token antes de permitir el acceso.
6. **Cierre de Sesión**: El cliente elimina el token almacenado.

#### Ventajas

- ✅ **Escalabilidad**: No necesitas sincronizar sesiones en múltiples servidores porque toda la autenticación se basa en el token.
- ✅ **Desacoplamiento**: Puedes consumir APIs externas o permitir integraciones fácilmente, ya que el token puede ser enviado a cualquier servidor.
- ✅ **Flexibilidad**: Puedes implementar Single Page Applications (SPAs) sin depender del backend para almacenar sesiones.
- ✅ **Stateless**: El servidor no necesita mantener estado de sesión.

#### Desventajas

- ⚠️ **Riesgos de Seguridad**: Si guardas el token en `localStorage` o `sessionStorage`, podría ser vulnerable a ataques XSS (Cross-Site Scripting). Usar cookies marcadas como `HttpOnly` puede mitigar esto, pero necesitarás configurar correctamente tu backend.
- ⚠️ **Gestión del Token**: El cliente debe manejar la expiración del token (y renovarlo si usas tokens de refresco).
- ⚠️ **Complejidad Adicional**: Necesitas implementar lógica en el frontend para manejar autenticación y errores relacionados con el token.

#### Ejemplo de Implementación

**Backend**:
```javascript
// Login - retorna token
app.post('/login', async (req, res) => {
  // Verificar credenciales...
  const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, { expiresIn: '24h' });
  res.json({ accessToken: token });
});

// Ruta protegida
app.get('/datos-protegidos', authMiddleware, (req, res) => {
  res.json({ message: 'Datos protegidos', user: req.user });
});
```

**Frontend**:
```javascript
// Login
const response = await fetch('/api/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ username, password })
});

const { accessToken } = await response.json();
localStorage.setItem('token', accessToken);

// Petición protegida
const token = localStorage.getItem('token');
const response = await fetch('/api/datos-protegidos', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
});
```

### Comparación

| Aspecto | Sesiones Backend | Tokens Frontend |
|---------|------------------|-----------------|
| **Escalabilidad** | Requiere sincronización | No requiere sincronización |
| **Seguridad** | Token no expuesto | Token en frontend (riesgo XSS) |
| **Flexibilidad** | Menos flexible | Más flexible |
| **Complejidad** | Más simple para cliente | Más complejo para cliente |
| **Uso Ideal** | Aplicaciones tradicionales | SPAs, APIs externas |

### Conclusión

- **Sesiones Backend**: Ideal si prefieres no exponer el token al frontend y estás trabajando en un sistema que no necesita alta escalabilidad o integración con APIs externas.
- **Tokens Frontend**: Ideal si necesitas una solución más escalable o compatible con APIs, o si estás trabajando con SPAs.

---

## 7. Implementación Completa

### Estructura de Archivos

```
src/
├── controllers/
│   └── authController.js
├── middlewares/
│   └── authMiddleware.js
├── models/
│   └── userModel.js
└── routes/
    └── authRoutes.js
```

### Modelo de Usuario

**`src/models/userModel.js`**:
```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
    unique: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true
  }
}, {
  timestamps: true
});

module.exports = mongoose.model('User', userSchema);
```

### Controller de Autenticación

**`src/controllers/authController.js`**:
```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const User = require('../models/userModel');

exports.registerUser = async (req, res) => {
  try {
    const { username, email, password } = req.body;
    
    // Verificar si el usuario ya existe
    const existingUser = await User.findOne({ 
      $or: [{ username }, { email }] 
    });
    if (existingUser) {
      return res.status(400).json({ error: 'Usuario o email ya existe' });
    }
    
    // Hash de la contraseña
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Crear usuario
    const user = new User({
      username,
      email,
      password: hashedPassword
    });
    
    await user.save();
    res.status(201).json({ message: 'Usuario registrado correctamente' });
    
  } catch (error) {
    res.status(500).json({ error: 'Error al registrar usuario' });
  }
};

exports.loginUser = async (req, res) => {
  try {
    const { username, password } = req.body;
    
    // Buscar usuario
    const user = await User.findOne({ username });
    if (!user) {
      return res.status(400).json({ error: 'Usuario no encontrado' });
    }
    
    // Comparar contraseña
    const match = await bcrypt.compare(password, user.password);
    if (!match) {
      return res.status(401).json({ error: 'Credenciales incorrectas' });
    }
    
    // Generar token JWT
    const accessToken = jwt.sign(
      { 
        userId: user._id,
        username: user.username 
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    res.json({ 
      accessToken: accessToken,
      user: {
        id: user._id,
        username: user.username,
        email: user.email
      }
    });
    
  } catch (error) {
    res.status(500).json({ error: 'Error al iniciar sesión' });
  }
};
```

### Middleware de Autenticación

**`src/middlewares/authMiddleware.js`**:
```javascript
const jwt = require('jsonwebtoken');

module.exports = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Token de acceso no proporcionado' });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({ error: 'Token de acceso no válido' });
    }
    
    req.user = user;
    next();
  });
};
```

### Routes

**`src/routes/authRoutes.js`**:
```javascript
const express = require('express');
const router = express.Router();
const { registerUser, loginUser } = require('../controllers/authController');
const authMiddleware = require('../middlewares/authMiddleware');

// Rutas públicas
router.post('/register', registerUser);
router.post('/login', loginUser);

// Ruta protegida de ejemplo
router.get('/profile', authMiddleware, (req, res) => {
  res.json({ 
    message: 'Perfil del usuario',
    user: req.user 
  });
});

module.exports = router;
```

### Integración en `index.js`

```javascript
const express = require('express');
const cors = require('cors');
require('dotenv').config();

const app = express();

app.use(cors());
app.use(express.json());

// Rutas
app.use('/api/auth', require('./src/routes/authRoutes'));

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Servidor en puerto ${PORT}`);
});
```

---

## 8. Buenas Prácticas de Seguridad

### Contraseñas

- ✅ **Nunca guardar en texto plano**: Siempre usar bcrypt
- ✅ **Mínimo 8 caracteres**: Validar longitud mínima
- ✅ **Complejidad**: Requerir mayúsculas, minúsculas, números
- ✅ **No reutilizar**: No permitir contraseñas recientes

### Tokens JWT

- ✅ **Usar expiración corta**: 15 minutos a 1 hora para access tokens
- ✅ **Refresh tokens**: Para renovar access tokens sin re-login
- ✅ **Secret fuerte**: Usar una clave secreta larga y aleatoria
- ✅ **HTTPS en producción**: Nunca enviar tokens por HTTP

### Headers de Seguridad

```javascript
// Agregar headers de seguridad
app.use((req, res, next) => {
  res.setHeader('X-Content-Type-Options', 'nosniff');
  res.setHeader('X-Frame-Options', 'DENY');
  res.setHeader('X-XSS-Protection', '1; mode=block');
  next();
});
```

### Validación de Inputs

- ✅ Validar todos los inputs del usuario
- ✅ Sanitizar datos antes de guardar
- ✅ Usar librerías como `express-validator` o `joi`

### Rate Limiting

Limitar el número de peticiones para prevenir ataques de fuerza bruta.

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5 // máximo 5 intentos
});

app.use('/api/auth/login', limiter);
```

### Variables de Entorno

- ✅ **Nunca hardcodear secretos**: Usar variables de entorno
- ✅ **No commitear `.env`**: Añadir a `.gitignore`
- ✅ **Usar diferentes secretos**: Desarrollo vs Producción

---

