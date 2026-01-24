# Swagger/OpenAPI: Documentación de APIs 📚

## 📑 Índice

1. [¿Qué es Swagger? (Analogía del Mundo Real)](#qué-es-swagger-analogía-del-mundo-real)
2. [Instalación](#instalación)
3. [Configuración Básica](#configuración-básica)
4. [Documentar Endpoint](#documentar-endpoint)
5. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es Swagger? (Analogía del Mundo Real)

### 📚 Analogía: El Manual de Instrucciones

Imagina que compras un electrodoméstico:
- **API**: Es el electrodoméstico
- **Swagger**: Es el manual de instrucciones que explica cómo usarlo
- **Documentación**: Te dice qué botones presionar, qué funciones tiene, qué esperar

**Swagger es como el manual de instrucciones** de tu API - explica cómo usarla.

### 🗺️ Analogía: El Mapa de la Ciudad

Piensa en un mapa de la ciudad:
- **API**: Es la ciudad
- **Swagger**: Es el mapa que muestra todas las calles (endpoints)
- **Documentación**: Te dice cómo llegar a cada lugar, qué encontrarás allí

**Swagger es como un mapa** que te muestra todos los endpoints de tu API y cómo usarlos.

### 📋 Analogía: El Menú de Restaurante

Un menú de restaurante:
- **API**: Es el restaurante
- **Swagger**: Es el menú que muestra todos los platos (endpoints)
- **Documentación**: Te dice qué platos hay, qué ingredientes tienen, cuánto cuestan

**Swagger es como el menú** de tu API - muestra todas las opciones disponibles.

### ¿Qué es Swagger?

Swagger (OpenAPI) es un estándar para documentar APIs REST. Permite describir endpoints, parámetros, respuestas y más.

**En términos simples**: Swagger es como el "manual de instrucciones" de tu API - documenta todos los endpoints, cómo usarlos, qué parámetros necesitan y qué respuestas dan.

## Instalación

```bash
npm install swagger-jsdoc swagger-ui-express
```

## Configuración Básica

```javascript
const swaggerJsdoc = require('swagger-jsdoc')
const swaggerUi = require('swagger-ui-express')

const swaggerOptions = {
    definition: {
        openapi: '3.0.0',
        info: {
            title: 'Mi API',
            version: '1.0.0',
            description: 'Documentación de mi API'
        },
        servers: [
            {
                url: 'http://localhost:3000',
                description: 'Servidor de desarrollo'
            }
        ]
    },
    apis: ['./routes/*.js']  // Archivos con documentación
}

const swaggerSpec = swaggerJsdoc(swaggerOptions)

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec))
```

## Documentar Endpoint

```javascript
/**
 * @swagger
 * /api/users:
 *   get:
 *     summary: Obtener todos los usuarios
 *     tags: [Users]
 *     responses:
 *       200:
 *         description: Lista de usuarios
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 $ref: '#/components/schemas/User'
 */
router.get('/', userController.getAll)
```

## Esquemas

```javascript
/**
 * @swagger
 * components:
 *   schemas:
 *     User:
 *       type: object
 *       required:
 *         - name
 *         - email
 *       properties:
 *         id:
 *           type: string
 *         name:
 *           type: string
 *         email:
 *           type: string
 */
```

## Endpoints Completos

```javascript
/**
 * @swagger
 * /api/users/{id}:
 *   get:
 *     summary: Obtener usuario por ID
 *     tags: [Users]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: Usuario encontrado
 *       404:
 *         description: Usuario no encontrado
 */
```

## Conceptos Clave

1. **Swagger**: Estándar para documentar APIs
2. **OpenAPI**: Especificación de Swagger
3. **swagger-jsdoc**: Genera documentación desde comentarios
4. **swagger-ui**: Interfaz visual para documentación
5. **Schemas**: Definir estructura de datos
6. **Tags**: Agrupar endpoints
7. **Responses**: Documentar respuestas posibles

