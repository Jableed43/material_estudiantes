# Deploy Frontend: Netlify 🚀

## 📑 Índice

1. [¿Qué es Netlify? (Analogía del Mundo Real)](#qué-es-netlify-analogía-del-mundo-real)
2. [Preparación del Proyecto](#preparación-del-proyecto)
3. [Deploy desde Git](#deploy-desde-git)
4. [Variables de Entorno](#variables-de-entorno)
5. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es Netlify? (Analogía del Mundo Real)

### 🚀 Analogía: Publicar tu Aplicación en Internet

Imagina que creaste una aplicación:
- **Desarrollo local**: La tienes en tu computadora (solo tú la ves)
- **Netlify**: Es como el "servidor público" donde la publicas para que todos la vean

**Netlify es como publicar tu aplicación** en internet para que cualquiera pueda acceder a ella.

### 🏠 Analogía: Abrir tu Casa al Público

Piensa en abrir tu casa:
- **Desarrollo local**: Construyes la casa (solo tú puedes entrar)
- **Netlify**: Es como abrir la casa al público (cualquiera puede entrar)

**Netlify hace tu aplicación accesible** para todos en internet.

### 📺 Analogía: Transmitir en Vivo

Transmitir en vivo:
- **Desarrollo local**: Grabas el video (solo tú lo ves)
- **Netlify**: Es como transmitir en vivo (todos pueden verlo)

**Netlify "transmite" tu aplicación** para que todos puedan acceder a ella.

### Introducción a Netlify

Netlify es una plataforma de hosting para aplicaciones web estáticas y sitios generados estáticamente. Es ideal para aplicaciones React, Vue, Angular y otros frameworks frontend.

**En términos simples**: Netlify es como el "servidor público" donde publicas tu aplicación frontend para que esté disponible en internet.

### Características

- ✅ Deploy automático desde Git
- ✅ HTTPS automático
- ✅ CDN global
- ✅ Serverless Functions
- ✅ Formularios sin backend
- ✅ Build automático

---

## Preparación del Proyecto

### Build del Proyecto

Antes de hacer deploy, asegúrate de que tu proyecto tenga un script de build:

```json
// package.json
{
  "scripts": {
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

### Archivo de Configuración (netlify.toml)

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

---

## Deploy desde Git

### Paso 1: Conectar Repositorio

1. Ve a [netlify.com](https://netlify.com)
2. Inicia sesión con GitHub/GitLab/Bitbucket
3. Click en "New site from Git"
4. Selecciona tu repositorio

### Paso 2: Configurar Build

- **Build command**: `npm run build`
- **Publish directory**: `dist` (o `build` según tu proyecto)

### Paso 3: Deploy

Netlify automáticamente:
- Instala dependencias (`npm install`)
- Ejecuta el build (`npm run build`)
- Publica los archivos estáticos

---

## Variables de Entorno

### Configurar en Netlify

1. Ve a **Site settings** → **Environment variables**
2. Agrega tus variables:
   - `VITE_API_URL=https://api.ejemplo.com`
   - `VITE_API_KEY=tu-key`

### Usar en Código

```jsx
const apiUrl = import.meta.env.VITE_API_URL
```

---

## Deploy Manual

### Netlify CLI

```bash
# Instalar CLI
npm install -g netlify-cli

# Login
netlify login

# Deploy
netlify deploy

# Deploy a producción
netlify deploy --prod
```

---

## Serverless Functions

### Crear Función

```javascript
// netlify/functions/hello.js
exports.handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: 'Hello from Netlify!' })
  }
}
```

### Llamar desde Frontend

```jsx
const response = await fetch('/.netlify/functions/hello')
const data = await response.json()
```

---

## Ejemplos Prácticos

### Ejemplo 1: Deploy Básico

1. Build del proyecto: `npm run build`
2. Arrastra la carpeta `dist` a Netlify
3. ¡Listo! Tu sitio está en línea

### Ejemplo 2: Deploy con Git

1. Conecta tu repositorio de GitHub
2. Configura build settings
3. Cada push a `main` hace deploy automático

---

## Conceptos Clave

1. **Build**: Compilar proyecto para producción
2. **Deploy**: Publicar archivos en servidor
3. **CDN**: Red de distribución de contenido
4. **Serverless Functions**: Funciones sin servidor
5. **Variables de Entorno**: Configuración externa
6. **Deploy Automático**: Deploy al hacer push
7. **HTTPS**: Certificado SSL automático

---

## Buenas Prácticas

- Usa variables de entorno para configuración
- Configura redirects para SPA (Single Page Apps)
- Revisa los logs de build si hay errores
- Usa preview deployments para probar antes de producción
- Configura dominio personalizado
- Optimiza imágenes y assets antes del deploy
- Revisa el tamaño del bundle

