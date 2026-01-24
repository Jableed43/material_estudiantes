# Master Guide: Docker y Contenedores 🐳

## 📑 Índice
1. [Introducción a Docker](#1-introducción-a-docker)
2. [Conceptos Fundamentales](#2-conceptos-fundamentales)
3. [Dockerfile: Crear Imágenes](#3-dockerfile-crear-imágenes)
4. [Docker Compose: Orquestación](#4-docker-compose-orquestación)
5. [Comandos Esenciales](#5-comandos-esenciales)
6. [Variables de Entorno y Redes](#6-variables-de-entorno-y-redes)
7. [Volúmenes: Persistencia de Datos](#7-volúmenes-persistencia-de-datos)
8. [Docker Hub: Compartir Imágenes](#8-docker-hub-compartir-imágenes)
9. [Troubleshooting](#9-troubleshooting)
10. [Buenas Prácticas](#10-buenas-prácticas)

---

## 1. Introducción a Docker

**Docker** es una plataforma que permite empaquetar una aplicación y todas sus dependencias en **contenedores** aislados, garantizando que el software funcione igual en cualquier entorno.

### ¿Por qué Docker?

**Problema sin Docker**:
- ❌ "Funciona en mi máquina" - Diferentes entornos, diferentes problemas
- ❌ Configuración manual compleja
- ❌ Dependencias conflictivas
- ❌ Difícil de reproducir el entorno

**Solución con Docker**:
- ✅ **Consistencia**: Funciona igual en desarrollo, testing y producción
- ✅ **Aislamiento**: Cada aplicación en su propio contenedor
- ✅ **Portabilidad**: Funciona en cualquier máquina con Docker
- ✅ **Escalabilidad**: Fácil de escalar horizontalmente
- ✅ **Reproducibilidad**: Cualquiera puede ejecutar tu aplicación

### Analogía

Imagina Docker como **cajas de envío**:
- **Dockerfile**: Las instrucciones para empaquetar
- **Imagen**: La caja empaquetada (plantilla)
- **Contenedor**: La caja abierta y funcionando (instancia)
- **Docker Hub**: El almacén donde guardas las cajas

---

## 2. Conceptos Fundamentales

### Dockerfile

Un **Dockerfile** es un archivo de texto con las instrucciones para construir una imagen. Es como una "receta" que define:
- Qué imagen base usar
- Qué dependencias instalar
- Qué código copiar
- Qué comandos ejecutar

### Imagen

Una **imagen** es una plantilla de solo lectura que contiene:
- El código de la aplicación
- Las librerías y dependencias
- Las configuraciones
- El sistema operativo base

**Características**:
- ✅ **Read-only**: No se modifica directamente
- ✅ **Reutilizable**: Una imagen puede crear múltiples contenedores
- ✅ **Ligera**: Solo contiene lo necesario

### Contenedor

Un **contenedor** es una instancia ejecutable de una imagen. Es la imagen "en ejecución".

**Características**:
- ✅ **Aislado**: Cada contenedor es independiente
- ✅ **Efímero**: Se puede crear y destruir fácilmente
- ✅ **Portátil**: Funciona igual en cualquier máquina

### Volumen

Un **volumen** es almacenamiento persistente fuera del contenedor. Útil para:
- Bases de datos (persistir datos)
- Archivos de configuración
- Logs

**Características**:
- ✅ **Persistente**: Los datos sobreviven a la eliminación del contenedor
- ✅ **Compartible**: Múltiples contenedores pueden compartir un volumen

### Red (Network)

Una **red** permite que los contenedores se comuniquen entre sí (ej. la App con la DB).

**Características**:
- ✅ **Aislamiento**: Contenedores en diferentes redes no se comunican
- ✅ **DNS automático**: Los contenedores se encuentran por nombre

### Relación entre Conceptos

```
Dockerfile
    ↓ (docker build)
Imagen
    ↓ (docker run)
Contenedor
```

---

## 3. Dockerfile: Crear Imágenes

Un **Dockerfile** define cómo construir una imagen.

### Estructura Básica

```dockerfile
# Imagen base
FROM node:18-alpine

# Directorio de trabajo
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm install

# Copiar código fuente
COPY . .

# Exponer puerto
EXPOSE 3000

# Comando para ejecutar
CMD ["npm", "start"]
```

### Instrucciones del Dockerfile

#### FROM

Especifica la imagen base.

```dockerfile
FROM node:18-alpine        # Node.js 18 en Alpine Linux (ligero)
FROM node:18               # Node.js 18 completo
FROM ubuntu:20.04          # Ubuntu 20.04
```

**Recomendación**: Usa imágenes `-alpine` (más pequeñas).

#### WORKDIR

Establece el directorio de trabajo.

```dockerfile
WORKDIR /app
# Todos los comandos siguientes se ejecutan en /app
```

#### COPY

Copia archivos del host al contenedor.

```dockerfile
COPY package.json ./       # Copiar archivo específico
COPY . .                   # Copiar todo
COPY src/ ./src/          # Copiar carpeta
```

#### RUN

Ejecuta comandos durante la construcción de la imagen.

```dockerfile
RUN npm install
RUN apt-get update && apt-get install -y git
```

#### EXPOSE

Documenta qué puerto usa la aplicación (no abre el puerto).

```dockerfile
EXPOSE 3000
EXPOSE 8080
```

#### CMD

Comando por defecto cuando se ejecuta el contenedor.

```dockerfile
CMD ["npm", "start"]       # Forma recomendada (array)
CMD npm start              # Forma alternativa (string)
```

**Diferencia con RUN**:
- `RUN`: Se ejecuta durante la construcción
- `CMD`: Se ejecuta cuando se inicia el contenedor

### Ejemplo Completo: Backend Node.js

```dockerfile
# Imagen base
FROM node:18-alpine

# Instalar dependencias del sistema si es necesario
RUN apk add --no-cache git

# Directorio de trabajo
WORKDIR /app

# Copiar archivos de dependencias
COPY package.json package-lock.json ./

# Instalar dependencias de producción
RUN npm ci --only=production

# Copiar código fuente
COPY . .

# Crear usuario no-root (seguridad)
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001
USER nodejs

# Exponer puerto
EXPOSE 3001

# Comando de inicio
CMD ["node", "index.js"]
```

### .dockerignore

Similar a `.gitignore`, pero para Docker. Evita copiar archivos innecesarios.

**`.dockerignore`**:
```
node_modules
.env
.git
.gitignore
*.md
coverage
dist
build
.DS_Store
```

**Ventajas**:
- ✅ Construcción más rápida
- ✅ Imagen más pequeña
- ✅ No copiar archivos sensibles

---

## 4. Docker Compose: Orquestación

**Docker Compose** permite definir y ejecutar múltiples contenedores con un solo archivo. Ideal para aplicaciones con múltiples servicios (App + DB + Cache, etc.).

### ¿Por qué Docker Compose?

- ✅ **Múltiples servicios**: Orquestar App + MongoDB + Redis, etc.
- ✅ **Configuración centralizada**: Un solo archivo YAML
- ✅ **Redes automáticas**: Los servicios se comunican automáticamente
- ✅ **Volúmenes compartidos**: Compartir datos entre servicios

### Estructura Básica

**`docker-compose.yml`**:
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3001:3001"
    env_file:
      - .env
    depends_on:
      - mongo
    restart: unless-stopped

  mongo:
    image: mongo:7.0
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    restart: unless-stopped

volumes:
  mongo_data:
```

### Configuración Completa: Stack Node + Mongo

#### docker-compose.yml

```yaml
version: '3.8'

services:
  # Aplicación Backend
  app:
    build: .                    # Construir desde Dockerfile
    container_name: mi-api      # Nombre del contenedor
    ports:
      - "3001:3001"             # host:contenedor
    env_file:
      - .env                    # Cargar variables de entorno
    environment:
      - NODE_ENV=production
    depends_on:
      - mongo                   # Esperar a que mongo esté listo
    restart: unless-stopped     # Reiniciar si falla
    networks:
      - app-network

  # Base de Datos MongoDB
  mongo:
    image: mongo:7.0            # Imagen oficial de MongoDB
    container_name: mi-mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db     # Persistir datos
    restart: unless-stopped
    networks:
      - app-network

# Volúmenes (persistencia)
volumes:
  mongo_data:
    driver: local

# Redes (comunicación)
networks:
  app-network:
    driver: bridge
```

### Variables de Entorno

Dentro de Docker, la URL de conexión **no usa `localhost`**, sino el **nombre del servicio** definido en compose.

**`.env`**:
```env
PORT=3001
MONGODB_URI=mongodb://mongo:27017   # 'mongo' es el nombre del servicio
UTN_DB=ecommerce_db
SECRET=mi-secreto-desarrollo-12345
```

**⚠️ Clave**: `MONGODB_URI=mongodb://mongo:27017` (no `localhost`)

### Conexión con Reintentos

**`db.js`** (con reintentos para esperar a que MongoDB esté listo):
```javascript
import mongoose from 'mongoose';
import { MONGODB_URI, UTN_DB } from './config.js';

export const connectDB = async () => {
  try {
    if (!MONGODB_URI || !UTN_DB) {
      throw new Error('Variables de entorno no definidas');
    }
    
    // Esperar 3 segundos para que MongoDB esté listo
    await new Promise(resolve => setTimeout(resolve, 3000));
    
    await mongoose.connect(`${MONGODB_URI}/${UTN_DB}`);
    console.log('✅ Database connected');
  } catch (error) {
    console.error('❌ Error:', error.message);
    // Reintentar después de 5 segundos
    setTimeout(connectDB, 5000);
  }
};
```

### Indentación YAML

**⚠️ Importante**: El archivo YAML fallará si los niveles de espacios son incorrectos.

**Correcto** (2 espacios por nivel):
```yaml
services:
  app:              # 2 espacios
    build: .        # 4 espacios
    ports:          # 4 espacios
      - "3001:3001" # 6 espacios
  mongo:            # 2 espacios (mismo nivel que app)
    image: mongo    # 4 espacios
```

**Incorrecto**:
```yaml
services:
  app:
    build: .
  mongo:            # ❌ Debe estar al mismo nivel que app
    image: mongo
```

---

## 5. Comandos Esenciales

### Docker Compose

| Comando | Propósito |
|---------|-----------|
| `docker-compose up --build` | Construye y levanta los servicios |
| `docker-compose up -d` | Levanta los servicios en segundo plano |
| `docker-compose up -d --build` | Construye y levanta en segundo plano |
| `docker-compose down` | Detiene y elimina los contenedores |
| `docker-compose down -v` | Detiene y elimina contenedores y volúmenes |
| `docker-compose ps` | Verifica el estado de los contenedores |
| `docker-compose logs -f app` | Ver logs del servicio 'app' en tiempo real |
| `docker-compose logs app` | Ver logs del servicio 'app' |
| `docker-compose restart app` | Reinicia un servicio específico |
| `docker-compose stop` | Detiene servicios (sin eliminar) |
| `docker-compose start` | Inicia servicios detenidos |

### Docker Básico

| Comando | Propósito |
|---------|-----------|
| `docker build -t nombre:tag .` | Construir imagen |
| `docker run -p 3000:3000 nombre` | Ejecutar contenedor |
| `docker ps` | Ver contenedores en ejecución |
| `docker ps -a` | Ver todos los contenedores |
| `docker images` | Ver imágenes |
| `docker stop <id>` | Detener contenedor |
| `docker rm <id>` | Eliminar contenedor |
| `docker rmi <id>` | Eliminar imagen |

### Ejemplos de Uso

#### Construir y Ejecutar con Dockerfile

```bash
# Construir imagen
docker build -t mi-api:1.0 .

# Ejecutar contenedor
docker run -p 3001:3001 --env-file .env mi-api:1.0
```

#### Usar Docker Compose

```bash
# Construir y ejecutar todos los servicios
docker-compose up --build

# En segundo plano
docker-compose up -d --build

# Ver logs
docker-compose logs -f app

# Detener servicios
docker-compose down

# Reconstruir (forzar)
docker-compose up --build --force-recreate
```

#### Acceder a Contenedor

```bash
# Ejecutar comando en contenedor
docker-compose exec app sh

# Acceder a MongoDB
docker-compose exec mongo mongosh

# Dentro de mongosh:
show dbs
use ecommerce_db
show collections
db.users.find().pretty()
exit
```

---

## 6. Variables de Entorno y Redes

### Variables de Entorno

#### En docker-compose.yml

```yaml
services:
  app:
    env_file:
      - .env                    # Cargar desde archivo
    environment:
      - NODE_ENV=production     # O definir directamente
      - PORT=3001
```

#### En .env

```env
PORT=3001
MONGODB_URI=mongodb://mongo:27017
UTN_DB=ecommerce_db
SECRET=mi-secreto
```

### Redes (Networks)

Los servicios en el mismo `docker-compose.yml` se comunican automáticamente por nombre.

```yaml
services:
  app:
    networks:
      - app-network
  mongo:
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

**Comunicación**:
```javascript
// En el código de la app
mongoose.connect('mongodb://mongo:27017/mi-db');
// 'mongo' es el nombre del servicio, no 'localhost'
```

### Puertos

```yaml
ports:
  - "3001:3001"    # host:contenedor
  # Puerto del host : Puerto del contenedor
```

**Ejemplo**:
- `"8080:3001"`: Accedes desde `localhost:8080`, pero el contenedor escucha en `3001`
- `"3001:3001"`: Mismo puerto en ambos lados

---

## 7. Volúmenes: Persistencia de Datos

Los **volúmenes** permiten persistir datos fuera del contenedor.

### ¿Por qué Volúmenes?

Sin volúmenes:
- ❌ Si eliminas el contenedor, pierdes los datos
- ❌ Los datos están dentro del contenedor

Con volúmenes:
- ✅ Los datos persisten aunque elimines el contenedor
- ✅ Los datos están fuera del contenedor
- ✅ Múltiples contenedores pueden compartir un volumen

### Tipos de Volúmenes

#### 1. Volumen Nombrado (Recomendado)

```yaml
services:
  mongo:
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:    # Volumen nombrado
```

#### 2. Bind Mount (Montaje Directo)

```yaml
services:
  app:
    volumes:
      - ./src:/app/src    # Montar carpeta local
```

#### 3. Volumen Anónimo

```yaml
services:
  mongo:
    volumes:
      - /data/db    # Volumen anónimo (sin nombre)
```

### Ejemplo Completo

```yaml
services:
  mongo:
    image: mongo:7.0
    volumes:
      - mongo_data:/data/db        # Persistir datos de MongoDB
      - ./mongo-init:/docker-entrypoint-initdb.d  # Scripts de inicialización

volumes:
  mongo_data:
    driver: local
```

### Gestionar Volúmenes

```bash
# Ver volúmenes
docker volume ls

# Inspeccionar volumen
docker volume inspect mongo_data

# Eliminar volumen
docker volume rm mongo_data

# Eliminar todos los volúmenes no usados
docker volume prune
```

---

## 8. Docker Hub: Compartir Imágenes

**Docker Hub** es el registro donde puedes subir tus imágenes para compartirlas o desplegarlas.

### Crear Cuenta y Repositorio

1. Ve a [hub.docker.com](https://hub.docker.com)
2. **Sign Up** para crear cuenta
3. **Repositories** > **Create Repository**
4. Nombre: `usuario/nombre-proyecto`
5. Visibilidad: Public o Private
6. Descripción (opcional)

### Subir una Imagen

```bash
# 1. Login
docker login

# 2. Taggear (etiquetar) la imagen
docker build -t usuario/nombre-proyecto:latest .

# O taggear imagen existente
docker tag mi-imagen:1.0 usuario/nombre-proyecto:latest

# 3. Subir
docker push usuario/nombre-proyecto:latest
```

### Usar Imagen de Docker Hub

#### Opción 1: Pull Manual

```bash
# Bajar imagen
docker pull usuario/nombre-proyecto:latest

# Ejecutar
docker run -p 3001:3001 usuario/nombre-proyecto:latest
```

#### Opción 2: En docker-compose.yml

```yaml
services:
  app:
    image: usuario/nombre-proyecto:latest  # En lugar de build
    ports:
      - "3001:3001"
    environment:
      - MONGODB_URI=mongodb://mongo:27017
    depends_on:
      - mongo
```

### Tags (Etiquetas)

```bash
# Crear tag con versión
docker build -t usuario/proyecto:v1.0 .
docker push usuario/proyecto:v1.0

# Múltiples tags
docker build -t usuario/proyecto:latest -t usuario/proyecto:v1.0 .
docker push usuario/proyecto:latest
docker push usuario/proyecto:v1.0
```

### Compartir Proyectos

#### Opción 1: Código Fuente (Git)

Incluir en el repositorio:
- ✅ `Dockerfile`
- ✅ `docker-compose.yml`
- ✅ `.dockerignore`
- ✅ `env.example`
- ✅ Código fuente

**README.md ejemplo**:
```markdown
## Instalación

1. git clone [url]
2. cd proyecto
3. cp env.example .env
4. docker-compose up --build
```

#### Opción 2: Docker Hub (Solo Imagen)

Crear `docker-compose.yml` que use la imagen:

```yaml
version: '3.8'
services:
  app:
    image: usuario/proyecto:latest
    ports:
      - "3001:3001"
    environment:
      - MONGODB_URI=mongodb://mongo:27017
      - UTN_DB=ecommerce_db
      - SECRET=mi-secreto
    depends_on:
      - mongo
  mongo:
    image: mongo:7.0
    volumes:
      - mongo_data:/data/db
volumes:
  mongo_data:
```

**Ejecutar**: `docker-compose up`

---

## 9. Troubleshooting

### Errores Comunes y Soluciones

#### Error: Indentación YAML

**Síntoma**:
```
services.app additional properties 'mongo' not allowed
```

**Solución**:
- Verificar que `mongo:` esté al mismo nivel que `app:`
- Usar exactamente 2 espacios por nivel
- No mezclar tabs y espacios

#### Error: Puerto Ocupado

**Síntoma**:
```
Error: Port 3001 is already in use
```

**Solución**:
```yaml
# Cambiar puerto izquierdo
ports:
  - "8080:3001"    # Usar puerto diferente
```

#### Error: MONGODB_URI no Definida

**Síntoma**:
```
Error: MONGODB_URI is not defined
```

**Solución**:
- Verificar que `.env` existe
- Verificar formato YAML correcto en `docker-compose.yml`
- Verificar que `env_file:` esté correctamente indentado

#### Error: No se Puede Conectar a MongoDB

**Síntoma**:
```
MongooseError: connect ECONNREFUSED
```

**Solución**:
1. **Esperar**: MongoDB tarda unos segundos en iniciar
2. **Verificar nombre**: Usar `mongo` (nombre del servicio), no `localhost`
3. **Verificar logs**: `docker-compose logs mongo`
4. **Agregar reintentos**: Ver sección "Conexión con Reintentos"

#### Error: Cannot Connect to MongoDB

**Solución**:
```javascript
// En db.js, agregar reintentos
export const connectDB = async () => {
  try {
    await new Promise(resolve => setTimeout(resolve, 3000));
    await mongoose.connect(`${MONGODB_URI}/${UTN_DB}`);
    console.log('✅ Database connected');
  } catch (error) {
    console.error('❌ Error:', error.message);
    setTimeout(connectDB, 5000);  // Reintentar
  }
};
```

### Comandos de Debugging

```bash
# Ver logs de un servicio
docker-compose logs app
docker-compose logs -f app    # Seguir logs en tiempo real

# Ver estado de contenedores
docker-compose ps

# Reiniciar servicio
docker-compose restart app

# Reconstruir desde cero
docker-compose down -v
docker-compose up --build

# Ejecutar comando en contenedor
docker-compose exec app sh
docker-compose exec mongo mongosh

# Ver variables de entorno del contenedor
docker-compose exec app env
```

### Verificar Configuración

```bash
# Validar sintaxis de docker-compose.yml
docker-compose config

# Ver qué se construiría
docker-compose config --services
```

---

## 10. Buenas Prácticas

### Dockerfile

- ✅ **Usar imágenes oficiales**: `node:18-alpine` en lugar de construir desde cero
- ✅ **Multi-stage builds**: Para reducir tamaño de imagen final
- ✅ **Orden de instrucciones**: Copiar dependencias antes del código (mejor caché)
- ✅ **Usuario no-root**: Crear usuario sin privilegios
- ✅ **.dockerignore**: Excluir archivos innecesarios

### Docker Compose

- ✅ **Indentación correcta**: 2 espacios por nivel
- ✅ **Nombres descriptivos**: `app`, `mongo`, `redis`
- ✅ **Variables de entorno**: Usar `env_file` para `.env`
- ✅ **Restart policies**: `unless-stopped` para producción
- ✅ **Healthchecks**: Verificar que servicios estén listos

### Seguridad

- ✅ **No hardcodear secretos**: Usar variables de entorno
- ✅ **Usuario no-root**: No ejecutar como root
- ✅ **Imágenes actualizadas**: Usar versiones específicas, no `latest`
- ✅ **Scan de vulnerabilidades**: `docker scan imagen`

### Performance

- ✅ **Caché de capas**: Ordenar instrucciones para maximizar caché
- ✅ **Imágenes pequeñas**: Usar `-alpine` cuando sea posible
- ✅ **Multi-stage builds**: Reducir tamaño final
- ✅ **.dockerignore**: Excluir archivos grandes

### Organización

- ✅ **README claro**: Instrucciones de uso
- ✅ **env.example**: Template de variables de entorno
- ✅ **Versionado**: Tags en Docker Hub
- ✅ **Documentación**: Comentar docker-compose.yml

---

## 📚 Checklist de Dockerización

### Archivos Necesarios

- [ ] `Dockerfile`
- [ ] `docker-compose.yml` (indentación correcta)
- [ ] `.dockerignore`
- [ ] `.env` (local, no subir)
- [ ] `env.example` (para compartir)
- [ ] `db.js` (con reintentos)
- [ ] `.gitignore` (incluye `.env`)

### Configuración

- [ ] `MONGODB_URI=mongodb://mongo:27017` (no localhost)
- [ ] Variables de entorno correctas
- [ ] Puertos configurados
- [ ] Volúmenes para persistencia

### Pruebas

- [ ] `docker-compose up --build` funciona
- [ ] App conecta a MongoDB
- [ ] API responde en `http://localhost:3001`
- [ ] Logs sin errores

---

