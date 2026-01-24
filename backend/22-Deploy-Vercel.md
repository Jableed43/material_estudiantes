# Master Guide: Despliegue con Vercel y MongoDB Atlas 🚀

## 📑 Índice
1. [Introducción al Despliegue](#1-introducción-al-despliegue)
2. [MongoDB Atlas: Base de Datos en la Nube](#2-mongodb-atlas-base-de-datos-en-la-nube)
3. [Vercel: Despliegue de Backend](#3-vercel-despliegue-de-backend)
4. [Despliegue del Frontend](#4-despliegue-del-frontend)
5. [Configuración Completa](#5-configuración-completa)
6. [Troubleshooting](#6-troubleshooting)
7. [Buenas Prácticas](#7-buenas-prácticas)

---

## 1. Introducción al Despliegue

### ¿Qué es el Despliegue? (Analogía del Mundo Real)

### 🚀 Analogía: Publicar un Libro

Imagina que escribes un libro:
- **Desarrollo local**: Escribes el libro en tu computadora (solo tú lo ves)
- **Despliegue**: Publicas el libro para que otros puedan leerlo

**El despliegue es como publicar tu aplicación** - la pones disponible en internet para que otros la usen.

### 🏠 Analogía: Construir y Abrir una Casa

Piensa en construir una casa:
- **Desarrollo local**: Construyes la casa (solo tú puedes entrar)
- **Despliegue**: Abres la casa al público (cualquiera puede entrar)

**El despliegue es como abrir tu aplicación al público** - la pones en un servidor accesible desde internet.

### 🎬 Analogía: Estrenar una Película

Cuando estrenas una película:
- **Desarrollo local**: Grabas y editas la película (solo tú la ves)
- **Despliegue**: La estrenas en cines (todos pueden verla)

**El despliegue es como estrenar tu aplicación** - la pones disponible para que todos la usen.

Este proceso permite poner en producción tu backend de Express.js y tu base de datos de manera profesional y escalable.

### ¿Qué es el Despliegue?

**Despliegue (Deploy)** es el proceso de poner tu aplicación en un servidor accesible desde internet, para que otros usuarios puedan usarla.

**En términos simples**: El despliegue es como "publicar" tu aplicación - la subes a un servidor en internet para que esté disponible para todos.

### Componentes del Despliegue

1. **Base de Datos**: MongoDB Atlas (en la nube)
2. **Backend**: Express.js en Vercel (serverless)
3. **Frontend**: React/Vite en Vercel (estático)

### Ventajas de Vercel + Atlas

- ✅ **Gratis**: Planes gratuitos generosos
- ✅ **Fácil**: Integración con GitHub
- ✅ **Automático**: Deploy automático en cada push
- ✅ **Escalable**: Crece con tu aplicación
- ✅ **HTTPS**: Certificado SSL automático

---

## 2. MongoDB Atlas: Base de Datos en la Nube

**MongoDB Atlas** es el servicio en la nube para gestionar clusters de MongoDB sin configurar servidores.

### ¿Por qué MongoDB Atlas?

- ✅ **Sin instalación**: No necesitas instalar MongoDB localmente
- ✅ **Gratis**: Plan M0 (FREE) disponible
- ✅ **Escalable**: Fácil de escalar cuando crezcas
- ✅ **Backup automático**: Copias de seguridad automáticas
- ✅ **Monitoreo**: Dashboard para ver estadísticas

### Paso 1: Creación del Cluster

1. **Acceder a MongoDB Atlas**:
   - Ve a [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
   - Click en **"Get Started"** o **"Sign Up"**

2. **Crear Proyecto**:
   - Si no se crea automáticamente, ve a **"New Project"**
   - Nombre: "Mi Proyecto" (o el que prefieras)
   - Click en **"Create Project"**

3. **Crear Cluster**:
   - Click en **"Create"** o **"Build a Database"**
   - En **"Deploy your cluster"**, selecciona **"M0 FREE"** (versión gratuita)
   - En **"Cloud Provider & Region"**, selecciona una región cercana:
     - **Sao Paulo** (Brasil) - Recomendado para Argentina
     - **N. Virginia** (USA)
     - **Otras regiones cercanas**
   - Click en **"Create Deployment"**

4. **Esperar**: El cluster tarda unos minutos en crearse

### Paso 2: Configuración de Seguridad

#### 2.1. Crear Usuario de Base de Datos

1. En el panel izquierdo, ve a **"Security"** > **"Database Access"**
2. Click en **"Add New Database User"**
3. **Authentication Method**: Password
4. **Username**: Crea un nombre de usuario (ej: `admin`)
5. **Password**: Crea una contraseña segura
6. **⚠️ IMPORTANTE**: **Guarda el usuario y contraseña**, los necesitarás
7. **Database User Privileges**: "Atlas admin" (para desarrollo)
8. Click en **"Add User"**

#### 2.2. Configurar Acceso de Red

1. En **"Security"**, ve a **"Network Access"**
2. Click en **"Add IP Address"**
3. Selecciona **"Allow Access from Anywhere"**
   - Esto establece el rango IP a `0.0.0.0/0`
   - Permite conexiones desde Vercel y tu máquina de desarrollo
4. Click en **"Confirm"**

**⚠️ Nota de Seguridad**: En producción, restringe las IPs solo a las necesarias.

### Paso 3: Obtener String de Conexión (URI)

1. En la vista general del cluster, click en **"Connect"**
2. Selecciona **"Connect your application"** (controladores/drivers)
3. **Driver**: Node.js
4. **Version**: 5.5 or later (o la más reciente)
5. **Copia la cadena de conexión**:
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
6. **Reemplaza manualmente**:
   - `<username>`: Con el usuario que creaste
   - `<password>`: Con la contraseña que creaste
   - Opcional: Agrega el nombre de la base de datos al final:
     ```
     mongodb+srv://admin:password123@cluster0.xxxxx.mongodb.net/mi-database?retryWrites=true&w=majority
     ```

**Ejemplo de URI Final**:
```
mongodb+srv://admin:MiPassword123@cluster0.abc123.mongodb.net/ecommerce_db?retryWrites=true&w=majority
```

### Paso 4: Crear Registros Iniciales (Opcional)

Para crear colecciones y documentos de prueba:

1. En tu cluster, click en **"Browse Collections"**
2. Si no tienes bases de datos, click en **"Add My Own Data"**
   - **Database Name**: `ecommerce_db` (o el nombre que prefieras)
   - **Collection Name**: `usuarios` (o el nombre que prefieras)
3. Click en **"Create"**
4. Para agregar documentos:
   - Click en **"Insert Document"**
   - Agrega datos en formato JSON
   - Click en **"Insert"**

---

## 3. Vercel: Despliegue de Backend

Vercel requiere una estructura específica para ejecutar una API de Node.js como funciones **serverless**.

### ¿Qué es Vercel?

**Vercel** es una plataforma de hosting especializada en:
- ✅ Aplicaciones frontend (React, Next.js, Vue)
- ✅ APIs serverless (Node.js, Python, etc.)
- ✅ Deploy automático desde Git
- ✅ CDN global
- ✅ HTTPS automático

### Paso 1: Preparación del Código

#### Estructura de Archivos

Vercel requiere una estructura específica:

```
proyecto/
├── api/
│   └── index.js          # ⭐ Punto de entrada para Vercel
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   └── ...
├── vercel.json           # ⭐ Configuración Vercel
├── package.json
└── .env                  # Local, no subir
```

#### 1.1. Crear Carpeta `api/`

1. En la raíz del proyecto, crea una carpeta llamada **`api/`**
2. Mueve tu archivo principal (`index.js` o `server.js`) dentro de `api/`
3. Ajusta las rutas de importación si es necesario

**Ejemplo**:
```javascript
// Antes: src/index.js
// Después: api/index.js
```

#### 1.2. Modificar `package.json`

Asegúrate de que apunte a la nueva ubicación:

```json
{
  "main": "./api/index.js",
  "scripts": {
    "start": "node ./api/index.js",
    "dev": "nodemon ./api/index.js"
  }
}
```

#### 1.3. Crear `vercel.json`

Crea este archivo en la **raíz del proyecto**:

```json
{
  "version": 2,
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/api/index.js"
    }
  ]
}
```

**Explicación**:
- `rewrites`: Redirige todas las peticiones (`(.*)`) a `/api/index.js`
- Esto permite que Vercel maneje todas las rutas de tu API

### Paso 2: Configuración en Vercel

#### 2.1. Subir a GitHub

1. Asegúrate de que tu código esté en un repositorio de GitHub
2. **⚠️ IMPORTANTE**: Verifica que `.env` esté en `.gitignore`
3. Haz commit y push de todos los cambios

#### 2.2. Importar en Vercel

1. Ve a [vercel.com](https://vercel.com)
2. **Sign Up** o **Log In** (puedes usar GitHub)
3. Click en **"Add New"** > **"Project"**
4. **Import Git Repository**: Selecciona tu repositorio
5. Click en **"Import"**

#### 2.3. Configurar Variables de Entorno (⚠️ CRUCIAL)

**Durante la configuración del proyecto**:

1. Busca la sección **"Environment Variables"**
2. Agrega las variables que usa tu backend:

   **MONGO_URI**:
   - **Name**: `MONGO_URI`
   - **Value**: Pega el string de conexión completo de MongoDB Atlas
   - **Environment**: Production, Preview, Development (marca todos)

   **JWT_SECRET**:
   - **Name**: `JWT_SECRET`
   - **Value**: Tu clave secreta (ej: `mi-secreto-super-seguro-12345`)
   - **Environment**: Production, Preview, Development

   **Otras variables**:
   - Agrega cualquier otra variable que uses (ej: `PORT`, `NODE_ENV`, etc.)

**⚠️ IMPORTANTE**: 
- No uses `localhost` en ninguna variable
- Usa la URI completa de MongoDB Atlas
- No subas contraseñas al código

#### 2.4. Configuración del Proyecto

1. **Framework Preset**: "Other" (Vercel detectará Node.js)
2. **Root Directory**: `.` (raíz del proyecto)
3. **Build Command**: (dejar vacío o `npm install`)
4. **Output Directory**: (dejar vacío)
5. **Install Command**: `npm install`

#### 2.5. Deploy

1. Click en **"Deploy"**
2. Vercel construirá y subirá tu backend
3. Espera a que termine (1-2 minutos)
4. Una vez listo, verás la URL de tu API:
   ```
   https://mi-api.vercel.app
   ```

### Paso 3: Verificar Despliegue

1. **Probar Endpoint**:
   ```
   GET https://mi-api.vercel.app/api/usuarios
   ```

2. **Ver Logs**:
   - En el dashboard de Vercel, ve a **"Deployments"**
   - Click en el deployment más reciente
   - Ve a la pestaña **"Logs"** para ver errores

3. **Verificar Conexión a MongoDB**:
   - Los logs deberían mostrar "Database connected" o similar
   - Si hay errores, revisa las variables de entorno

---

## 4. Despliegue del Frontend

Si tienes un frontend (React, Vue, etc.) separado:

### Paso 1: Ajustar URL de API

**⚠️ CRUCIAL**: Asegúrate de que tu código frontend **no use `localhost`**, sino la **URL desplegada** que te dio Vercel para el backend.

**Antes (Local)**:
```javascript
const API_URL = 'http://localhost:3000';
```

**Después (Producción)**:
```javascript
// Usar variable de entorno
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3000';
```

**`.env` (Frontend)**:
```env
VITE_API_URL=https://mi-api.vercel.app
```

### Paso 2: Desplegar Frontend en Vercel

1. **Subir a GitHub**: Asegúrate de que el frontend esté en un repositorio
2. **Importar en Vercel**:
   - Click en **"Add New"** > **"Project"**
   - Importa el repositorio del frontend
3. **Configuración**:
   - Vercel detectará automáticamente que es React/Vite
   - **Framework Preset**: Vite (o React)
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist` (para Vite)
4. **Variables de Entorno**:
   - Agrega `VITE_API_URL` con la URL de tu backend desplegado
5. **Deploy**: Click en **"Deploy"**

### Paso 3: Verificar Comunicación

1. Una vez desplegado, prueba la aplicación
2. Verifica que el frontend se comunique con el backend
3. Revisa la consola del navegador para errores

---

## 5. Configuración Completa

### Estructura Final del Proyecto

```
backend/
├── api/
│   └── index.js          # Punto de entrada
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   └── ...
├── vercel.json           # Configuración Vercel
├── package.json
└── .gitignore           # Incluye .env

frontend/
├── src/
├── .env                 # VITE_API_URL=https://mi-api.vercel.app
└── package.json
```

### Variables de Entorno

#### Backend (Vercel Dashboard)

```
MONGO_URI=mongodb+srv://admin:password@cluster0.xxx.mongodb.net/ecommerce_db?retryWrites=true&w=majority
JWT_SECRET=mi-secreto-super-seguro
NODE_ENV=production
```

#### Frontend (Vercel Dashboard)

```
VITE_API_URL=https://mi-api.vercel.app
```

### Ejemplo de `vercel.json` Completo

```json
{
  "version": 2,
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/api/index.js"
    }
  ],
  "env": {
    "NODE_ENV": "production"
  }
}
```

### Ejemplo de `api/index.js`

```javascript
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');

const app = express();

// Middlewares
app.use(cors());
app.use(express.json());

// Rutas
app.use('/api/usuarios', require('../src/routes/userRoutes'));

// Ruta de prueba
app.get('/', (req, res) => {
  res.json({ message: 'API funcionando en Vercel' });
});

// Conexión a MongoDB Atlas
mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
  .then(() => console.log('✅ MongoDB Atlas connected'))
  .catch(err => console.error('❌ MongoDB connection error:', err));

// Exportar para Vercel
module.exports = app;
```

---

## 6. Troubleshooting

### Error: "Cannot find module"

**Síntoma**:
```
Error: Cannot find module './src/routes/userRoutes'
```

**Solución**:
- Verificar rutas de importación desde `api/index.js`
- Ajustar rutas relativas:
  ```javascript
  // Desde api/index.js
  require('../src/routes/userRoutes')  // ✅ Correcto
  require('./src/routes/userRoutes')   // ❌ Incorrecto
  ```

### Error: "MongoDB connection failed"

**Síntoma**:
```
MongooseError: connect ECONNREFUSED
```

**Soluciones**:
1. **Verificar MONGO_URI**:
   - Debe ser la URI completa de Atlas
   - Debe incluir usuario y contraseña
   - Debe incluir el nombre de la base de datos

2. **Verificar Network Access**:
   - En MongoDB Atlas, verificar que `0.0.0.0/0` esté permitido

3. **Verificar Variables de Entorno**:
   - En Vercel Dashboard, verificar que `MONGO_URI` esté configurada
   - Verificar que esté marcada para "Production"

### Error: "CORS policy"

**Síntoma**:
```
Access to fetch at 'https://mi-api.vercel.app' from origin 'https://mi-frontend.vercel.app' has been blocked by CORS policy
```

**Solución**:
```javascript
// En api/index.js
const cors = require('cors');

app.use(cors({
  origin: [
    'https://mi-frontend.vercel.app',
    'http://localhost:3000'  // Para desarrollo local
  ],
  credentials: true
}));
```

### Error: "Function exceeded maximum duration"

**Síntoma**:
```
Function exceeded maximum duration
```

**Solución**:
- Vercel tiene límite de tiempo para funciones serverless
- Optimizar consultas a base de datos
- Usar conexiones persistentes
- Considerar usar servidor tradicional si necesitas más tiempo

### Ver Logs en Vercel

1. Ve a **"Deployments"** en Vercel Dashboard
2. Click en el deployment más reciente
3. Ve a la pestaña **"Logs"**
4. Revisa errores y warnings

### Redeploy

Si necesitas hacer cambios:

1. **Hacer cambios en código local**
2. **Commit y push a GitHub**:
   ```bash
   git add .
   git commit -m "Fix: Corregir error de conexión"
   git push origin main
   ```
3. **Vercel despliega automáticamente** (si está conectado a GitHub)
4. O **Redeploy manual** en Vercel Dashboard

---

## 7. Buenas Prácticas

### Seguridad

- ✅ **Nunca subir `.env`**: Verificar que esté en `.gitignore`
- ✅ **Contraseñas fuertes**: Usar contraseñas seguras en MongoDB Atlas
- ✅ **Restringir IPs en producción**: En producción, restringir IPs en Network Access
- ✅ **Variables de entorno**: Nunca hardcodear secretos

### CORS

- ✅ **Configurar CORS**: Permitir solo orígenes necesarios
- ✅ **No usar `*` en producción**: Especificar dominios exactos

**Ejemplo**:
```javascript
app.use(cors({
  origin: process.env.NODE_ENV === 'production' 
    ? ['https://mi-frontend.vercel.app']
    : ['http://localhost:3000'],
  credentials: true
}));
```

### Performance

- ✅ **Conexión persistente**: Reutilizar conexión a MongoDB
- ✅ **Optimizar consultas**: Usar índices, limitar resultados
- ✅ **Cache**: Implementar caché cuando sea apropiado

### Monitoreo

- ✅ **Ver logs regularmente**: Revisar logs en Vercel
- ✅ **MongoDB Atlas Dashboard**: Monitorear uso de base de datos
- ✅ **Alertas**: Configurar alertas en Atlas para uso excesivo

### Pruebas Locales

- ✅ **Vercel CLI**: Usar `vercel dev` para simular entorno localmente
- ✅ **Probar antes de deploy**: Probar con variables de producción localmente

**Instalación Vercel CLI**:
```bash
npm install -g vercel
vercel login
vercel dev
```

---

## 📚 Tips de Oro ⭐

### CORS

Asegúrate de que tu backend permita peticiones desde el dominio de tu frontend desplegado:

```javascript
app.use(cors({
  origin: [
    'https://mi-frontend.vercel.app',
    'http://localhost:3000'  // Desarrollo local
  ]
}));
```

### Pruebas Locales

Usa el CLI de Vercel (`vercel dev`) para simular el entorno de despliegue localmente:

```bash
npm install -g vercel
vercel login
vercel dev
```

### Contraseñas

Nunca subas tu contraseña real de Atlas al código de Git; usa siempre variables de entorno.

### Preview Deployments

Cada Pull Request en GitHub crea un "Preview Deployment" en Vercel, útil para probar cambios antes de mergear.

### Variables de Entorno por Entorno

Puedes configurar variables diferentes para:
- **Production**: Producción
- **Preview**: Pull requests
- **Development**: Desarrollo local con `vercel dev`

---

