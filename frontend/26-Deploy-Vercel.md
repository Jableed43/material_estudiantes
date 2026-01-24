# Deploy Frontend: Vercel 🚀

## Introducción a Vercel

Vercel es una plataforma de hosting optimizada para aplicaciones frontend modernas. Es especialmente popular para proyectos React, Next.js, Vue y otros frameworks.

### Características

- ✅ Deploy automático desde Git
- ✅ HTTPS automático
- ✅ CDN global
- ✅ Serverless Functions
- ✅ Preview deployments
- ✅ Analytics integrado

---

## Preparación del Proyecto

### Build del Proyecto

Asegúrate de tener scripts de build configurados:

```json
// package.json
{
  "scripts": {
    "build": "vite build",
    "dev": "vite"
  }
}
```

### Archivo de Configuración (vercel.json)

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

---

## Deploy desde Git

### Paso 1: Conectar Repositorio

1. Ve a [vercel.com](https://vercel.com)
2. Inicia sesión con GitHub/GitLab/Bitbucket
3. Click en "Add New Project"
4. Selecciona tu repositorio

### Paso 2: Configurar Proyecto

Vercel detecta automáticamente:
- Framework (React, Vue, etc.)
- Build command
- Output directory

### Paso 3: Deploy

Vercel automáticamente:
- Instala dependencias
- Ejecuta el build
- Publica los archivos

---

## Variables de Entorno

### Configurar en Vercel

1. Ve a **Project Settings** → **Environment Variables**
2. Agrega variables:
   - `VITE_API_URL`
   - `VITE_API_KEY`

### Usar en Código

```jsx
const apiUrl = import.meta.env.VITE_API_URL
```

---

## Deploy Manual

### Vercel CLI

```bash
# Instalar CLI
npm install -g vercel

# Login
vercel login

# Deploy
vercel

# Deploy a producción
vercel --prod
```

---

## Serverless Functions

### Crear API Route

```javascript
// api/hello.js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello from Vercel!' })
}
```

### Llamar desde Frontend

```jsx
const response = await fetch('/api/hello')
const data = await response.json()
```

---

## Preview Deployments

Cada push a una rama crea un preview deployment:

- URL única para cada PR
- Pruebas antes de merge
- Compartir con equipo

---

## Ejemplos Prácticos

### Ejemplo 1: Deploy Básico

```bash
# Desde la terminal
vercel
```

### Ejemplo 2: Deploy con Configuración

```json
// vercel.json
{
  "framework": "vite",
  "buildCommand": "npm run build",
  "outputDirectory": "dist"
}
```

---

## Conceptos Clave

1. **Deploy Automático**: Deploy al hacer push
2. **Preview Deployments**: URLs temporales para PRs
3. **Serverless Functions**: API routes sin servidor
4. **CDN**: Distribución global de contenido
5. **Variables de Entorno**: Configuración externa
6. **HTTPS**: Certificado SSL automático
7. **Analytics**: Métricas de rendimiento

---

## Buenas Prácticas

- Usa preview deployments para probar cambios
- Configura variables de entorno correctamente
- Optimiza imágenes y assets
- Revisa los logs de build
- Usa dominio personalizado
- Configura redirects para SPA
- Monitorea el rendimiento con Analytics

