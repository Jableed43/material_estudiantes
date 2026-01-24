# SQL vs NoSQL: Introducción a Bases de Datos 🗄️

## 📑 Índice

1. [¿Qué es una Base de Datos? (Analogía del Mundo Real)](#qué-es-una-base-de-datos-analogía-del-mundo-real)
2. [Relacional (SQL) vs. No Relacional (NoSQL)](#1-relacional-sql-vs-no-relacional-nosql)
3. [Comparación Detallada: MySQL vs MongoDB](#2-comparación-detallada-mysql-vs-mongodb)
4. [¿Cuándo usar cada una?](#4-cuándo-usar-cada-una)
5. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es una Base de Datos? (Analogía del Mundo Real)

### 📚 Analogía: La Biblioteca

Imagina una biblioteca:
- **Base de Datos**: La biblioteca completa
- **Tablas/Colecciones**: Las diferentes secciones (libros, autores, préstamos)
- **Datos**: Los libros, información de autores, registros de préstamos
- **Sistema de organización**: Las reglas para encontrar y organizar la información

**Una base de datos es como una biblioteca organizada** donde guardas información de forma estructurada y fácil de encontrar.

### 🗂️ Analogía: El Archivo de una Empresa

Piensa en el archivo de una empresa:
- **Base de Datos**: El archivo completo
- **Tablas/Archivos**: Diferentes carpetas (empleados, clientes, productos)
- **Datos**: La información de cada empleado, cliente, producto
- **Búsqueda**: Puedes buscar información rápidamente

**Una base de datos te permite guardar, organizar y buscar información** de forma eficiente.

### 💾 Analogía: El Disco Duro Organizado

Tu disco duro:
- **Base de Datos**: Todo el disco duro
- **Tablas/Carpetas**: Diferentes carpetas organizadas
- **Datos**: Los archivos dentro de cada carpeta
- **Sistema de archivos**: Las reglas para organizar y encontrar archivos

**Pero una base de datos es más inteligente**: Puede relacionar información, validar datos, y hacer búsquedas complejas.

Este documento sirve como **puente de transición** entre bases de datos relacionales (SQL) y no relacionales (NoSQL), ayudando a entender cuándo y por qué elegir cada tipo.

Una base de datos es el almacén persistente donde guardamos la información de nuestra app (usuarios, productos, pedidos). 

## 📑 Índice
1. [Relacional (SQL) vs. No Relacional (NoSQL)](#1-relacional-sql-vs-no-relacional-nosql)
2. [Comparación Detallada: MySQL vs MongoDB](#2-comparación-detallada-mysql-vs-mongodb)
3. [Conceptos Fundamentales](#3-conceptos-fundamentales)
4. [¿Cuándo usar cada una?](#4-cuándo-usar-cada-una)
5. [Conexión desde Node.js](#5-conexión-desde-nodejs)

---

## 1. Relacional (SQL) vs. No Relacional (NoSQL)

### Definición

**Bases de Datos Relacionales (SQL)**:
- Organizan datos en **tablas** con filas y columnas
- Establecen **relaciones** entre tablas mediante claves primarias y foráneas
- Utilizan el lenguaje **SQL** (Structured Query Language)
- Ejemplos: MySQL, PostgreSQL, SQL Server, Oracle

**Bases de Datos No Relacionales (NoSQL)**:
- Almacenan datos en formatos **flexibles** (documentos, clave-valor, grafos)
- No requieren esquema predefinido estricto
- Optimizadas para **escalabilidad horizontal** y **alto rendimiento**
- Ejemplos: MongoDB, Redis, Cassandra, Neo4j

### Comparación General

| Característica | Relacional (MySQL, PostgreSQL) | No Relacional (MongoDB) |
| :--- | :--- | :--- |
| **Estructura** | Tablas, Filas y Columnas | Documentos (similares a JSON) |
| **Esquema** | **Estricto**: Debes definirlo antes | **Flexible**: Los datos pueden variar |
| **Escalabilidad** | Vertical (más potencia a un servidor) | Horizontal (más servidores económicos) |
| **Relaciones** | Maneja vínculos complejos (JOINs) | Incrusta datos o usa referencias |
| **Transacciones** | Soporte ACID completo (seguridad total) | Flexibilidad priorizada sobre ACID |
| **Lenguaje** | SQL (Structured Query Language) | Lenguaje específico (MongoDB Query Language) |
| **Integridad Referencial** | Claves foráneas nativas | Sin integridad referencial nativa |
| **Normalización** | Requiere normalización | Permite desnormalización |

---

## 2. Comparación Detallada: MySQL vs MongoDB

### 2.1. Estructura de Datos

#### MySQL (Relacional)
```sql
-- Estructura: Tablas con filas y columnas
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    edad INT,
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Datos almacenados como filas
INSERT INTO usuarios (nombre, email, edad) 
VALUES ('Juan', 'juan@example.com', 25);
```

**Características:**
- ✅ Estructura fija y predefinida
- ✅ Todas las filas tienen las mismas columnas
- ✅ Tipos de datos estrictos (INT, VARCHAR, DATETIME)
- ✅ Restricciones de integridad (NOT NULL, UNIQUE, FOREIGN KEY)

#### MongoDB (No Relacional)
```javascript
// Estructura: Documentos JSON/BSON
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "nombre": "Juan",
  "email": "juan@example.com",
  "edad": 25,
  "fecha_creacion": ISODate("2025-01-15T10:30:00Z"),
  "direccion": {  // Documento anidado
    "calle": "Av. Principal",
    "ciudad": "Buenos Aires"
  }
}
```

**Características:**
- ✅ Estructura flexible (cada documento puede tener campos diferentes)
- ✅ Permite documentos anidados
- ✅ No requiere esquema predefinido (aunque Mongoose permite definirlo)
- ✅ Formato JSON/BSON (Binary JSON)

### 2.2. Esquema y Validación

#### MySQL
```sql
-- Esquema estricto: DEBES definirlo antes de insertar datos
CREATE TABLE productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(255) NOT NULL,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0,
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);

-- ❌ Error: No puedes insertar sin definir todas las columnas
-- ❌ Error: No puedes agregar campos nuevos sin ALTER TABLE
```

**Características:**
- ✅ Esquema **obligatorio** y **estricto**
- ✅ Validación a nivel de base de datos
- ✅ Cambios requieren `ALTER TABLE` (puede ser costoso en producción)
- ✅ Todos los registros siguen el mismo esquema

#### MongoDB
```javascript
// Esquema flexible: Puedes definir o no
// Opción 1: Sin esquema (MongoDB puro)
db.productos.insertOne({
  nombre: "Laptop",
  precio: 999.99,
  stock: 10
});

// Opción 2: Con esquema (usando Mongoose)
const productoSchema = new mongoose.Schema({
  nombre: { type: String, required: true },
  precio: { type: Number, required: true },
  stock: { type: Number, default: 0 }
});

const Producto = mongoose.model('Producto', productoSchema);
```

**Características:**
- ✅ Esquema **opcional** y **flexible**
- ✅ Puedes agregar campos nuevos sin modificar estructura
- ✅ Cada documento puede tener estructura diferente
- ✅ Mongoose permite validación opcional a nivel de aplicación

### 2.3. Relaciones entre Datos

#### MySQL (JOINs)
```sql
-- Relaciones explícitas con JOINs
SELECT 
    u.nombre,
    u.email,
    p.titulo as post_titulo,
    p.contenido
FROM usuarios u
INNER JOIN posts p ON u.id = p.usuario_id
WHERE u.id = 1;

-- Múltiples JOINs
SELECT 
    a.nombre as autor,
    l.titulo as libro,
    c.nombre as categoria
FROM autores a
INNER JOIN libros l ON a.id = l.autor_id
INNER JOIN categorias c ON l.categoria_id = c.id;
```

**Características:**
- ✅ JOINs nativos y eficientes
- ✅ Integridad referencial con FOREIGN KEY
- ✅ Consultas complejas con múltiples tablas
- ✅ Relaciones explícitas y validadas

#### MongoDB (Referencias o Embebido)
```javascript
// Opción 1: Referencias (similar a FK)
const usuarioSchema = new mongoose.Schema({
  nombre: String,
  email: String
});

const postSchema = new mongoose.Schema({
  titulo: String,
  contenido: String,
  autor: { type: mongoose.Schema.Types.ObjectId, ref: 'Usuario' }
});

// Usar populate para "hacer JOIN"
const posts = await Post.find().populate('autor');

// Opción 2: Documentos embebidos
const usuarioSchema = new mongoose.Schema({
  nombre: String,
  email: String,
  direcciones: [{  // Array de documentos embebidos
    calle: String,
    ciudad: String,
    codigoPostal: String
  }]
});
```

**Características:**
- ✅ Referencias con `populate()` (similar a JOIN)
- ✅ Documentos embebidos (datos relacionados en el mismo documento)
- ✅ Sin integridad referencial nativa (debe manejarse en aplicación)
- ✅ Modelado basado en cómo se consultan los datos

### 2.4. Escalabilidad

#### MySQL (Escalabilidad Vertical)
```
┌─────────────────┐
│  Servidor Único │
│  ┌───────────┐  │
│  │   MySQL   │  │
│  │  (Más RAM)│  │
│  │  (Más CPU)│  │
│  └───────────┘  │
└─────────────────┘
```

**Características:**
- ✅ Escala **verticalmente** (más recursos al mismo servidor)
- ✅ Mejor para cargas predecibles
- ❌ Limitado por hardware del servidor
- ❌ Más costoso (hardware potente)

#### MongoDB (Escalabilidad Horizontal)
```
┌──────────┐  ┌──────────┐  ┌──────────┐
│ Servidor │  │ Servidor │  │ Servidor │
│  MongoDB │  │  MongoDB │  │  MongoDB │
│  (Shard) │  │  (Shard) │  │  (Shard) │
└──────────┘  └──────────┘  └──────────┘
       │            │            │
       └────────────┴────────────┘
              │
         Load Balancer
```

**Características:**
- ✅ Escala **horizontalmente** (más servidores)
- ✅ Mejor para grandes volúmenes de datos
- ✅ Más económico (servidores estándar)
- ✅ Distribución automática de datos (sharding)

### 2.5. Transacciones: ACID vs BASE

#### MySQL (ACID Completo)

**ACID** es un acrónimo que describe las propiedades fundamentales de las transacciones:
- **A**tomicity (Atomicidad): Todo o nada
- **C**onsistency (Consistencia): Estado válido antes y después
- **I**solation (Aislamiento): Transacciones no interfieren
- **D**urability (Durabilidad): Cambios permanentes

```sql
-- Transacciones ACID completas
START TRANSACTION;

INSERT INTO usuarios (nombre, email) VALUES ('Juan', 'juan@example.com');
INSERT INTO pedidos (usuario_id, total) VALUES (LAST_INSERT_ID(), 100.00);
UPDATE productos SET stock = stock - 1 WHERE id = 1;

-- Si algo falla, todo se revierte
COMMIT;
-- O
ROLLBACK;
```

**Características:**
- ✅ **ACID completo**: Garantías fuertes de consistencia
- ✅ Transacciones multi-tabla
- ✅ Integridad garantizada
- ✅ Consistencia inmediata
- ✅ Ideal para operaciones críticas (bancos, reservas)

**Ver detalles completos en**: `MYSQL/SQL-Guia-Maestra.md#2-acid-propiedades-de-las-transacciones`

#### MongoDB (BASE)

**BASE** es el modelo de consistencia utilizado por MongoDB:
- **B**asically **A**vailable (Básicamente Disponible): Sistema siempre responde
- **S**oft state (Estado Suave): Estado puede cambiar sin input
- **E**ventually consistent (Consistencia Eventual): Consistencia se logra eventualmente

```javascript
// Operación atómica a nivel de documento
await Usuario.updateOne(
  { _id: userId },
  { $inc: { edad: 1 } }
);

// Transacciones multi-documento (desde MongoDB 4.0, limitadas)
const session = await mongoose.startSession();
session.startTransaction();

try {
  await Usuario.create([{ nombre: 'Juan', email: 'juan@example.com' }], { session });
  await Pedido.create([{ usuario_id: usuarioId, total: 100.00 }], { session });
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
} finally {
  session.endSession();
}
```

**Características:**
- ✅ **BASE**: Prioriza disponibilidad y escalabilidad
- ⚠️ **Consistencia eventual**: No garantiza consistencia inmediata
- ✅ Atomicidad a nivel de **un solo documento** (por defecto)
- ⚠️ Transacciones multi-documento limitadas (desde v4.0)
- ✅ Ideal para sistemas de alto volumen (redes sociales, logs)

**Ver detalles completos en**: `MONGODB/MongoDB-Guia-Maestra.md#2-base-modelo-de-consistencia`

### Comparación ACID vs BASE

| Aspecto | ACID (MySQL) | BASE (MongoDB) |
|---------|--------------|----------------|
| **Consistencia** | Inmediata y estricta | Eventual |
| **Disponibilidad** | Puede sacrificarse | Prioritaria |
| **Atomicidad** | Multi-tabla garantizada | Solo documento (o limitada) |
| **Uso Ideal** | Sistemas críticos | Sistemas de alto volumen |
| **Garantías** | Fuertes | Débiles pero flexibles |
| **Escalabilidad** | Vertical (limitada) | Horizontal (ilimitada) |

### 2.6. Consultas y Lenguaje

#### MySQL (SQL)
```sql
-- Consultas complejas con SQL
SELECT 
    u.nombre,
    COUNT(p.id) as total_posts,
    AVG(p.likes) as promedio_likes
FROM usuarios u
LEFT JOIN posts p ON u.id = p.usuario_id
WHERE u.activo = 1
GROUP BY u.id
HAVING total_posts > 5
ORDER BY promedio_likes DESC
LIMIT 10;
```

**Características:**
- ✅ Lenguaje **estándar** (SQL)
- ✅ Consultas complejas con JOINs, GROUP BY, HAVING
- ✅ Funciones de agregación potentes
- ✅ Optimización automática de queries

#### MongoDB (MongoDB Query Language)
```javascript
// Consultas con MongoDB Query Language
const usuarios = await Usuario.aggregate([
  { $match: { activo: true } },
  { $lookup: {  // Similar a JOIN
      from: 'posts',
      localField: '_id',
      foreignField: 'autor',
      as: 'posts'
    }
  },
  { $project: {
      nombre: 1,
      total_posts: { $size: '$posts' },
      promedio_likes: { $avg: '$posts.likes' }
    }
  },
  { $match: { total_posts: { $gt: 5 } } },
  { $sort: { promedio_likes: -1 } },
  { $limit: 10 }
]);
```

**Características:**
- ✅ Lenguaje **específico** de MongoDB
- ✅ Pipeline de agregación potente
- ⚠️ JOINs más costosos (usar `$lookup`)
- ✅ Consultas basadas en documentos

### 2.7. Tabla Comparativa Completa

| Aspecto | MySQL | MongoDB |
|---------|-------|---------|
| **Tipo** | Relacional (SQL) | No Relacional (NoSQL) |
| **Estructura** | Tablas, Filas, Columnas | Colecciones, Documentos |
| **Esquema** | Estricto y obligatorio | Flexible y opcional |
| **Relaciones** | JOINs nativos | Referencias o embebido |
| **Escalabilidad** | Vertical | Horizontal |
| **ACID** | Completo | Parcial (desde v4.0) |
| **Lenguaje** | SQL (estándar) | MongoDB Query Language |
| **Integridad Referencial** | Nativa (FOREIGN KEY) | Manual (en aplicación) |
| **Normalización** | Requerida | Permite desnormalización |
| **Rendimiento** | Predecible, consistente | Muy rápido para lecturas |
| **Caso de Uso** | Datos estructurados, relaciones complejas | Datos flexibles, alto volumen |
| **Conexión Node.js** | `mysql2` (pool) | `mongoose` (ODM) |
| **Validación** | A nivel de BD | A nivel de aplicación (Mongoose) |

---

## 3. Conceptos Fundamentales

### 3.1. Índice
**Definición**: Estructura que acelera las búsquedas. Como el índice de un libro, permite encontrar datos rápidamente sin recorrer toda la tabla/colección.

**Ejemplos:**
- **MySQL**: `CREATE INDEX idx_email ON usuarios(email);`
- **MongoDB**: `db.usuarios.createIndex({ email: 1 })`

### 3.2. Esquema
**Definición**: El "plano" o diseño que define qué datos se guardan y cómo.

- **MySQL**: Esquema **obligatorio** y **estricto** (tablas con columnas definidas)
- **MongoDB**: Esquema **opcional** y **flexible** (puede variar entre documentos)

### 3.3. Transacción
**Definición**: Conjunto de acciones que deben ocurrir todas o ninguna (Atomicidad).

- **MySQL**: Transacciones **ACID completas** multi-tabla
- **MongoDB**: Transacciones **ACID parciales** (desde v4.0, limitadas)

### 3.4. ACID
**Definición**: Propiedades que garantizan que los datos sean confiables:

- **A**tomicidad: Una transacción se lleva a cabo completamente o no se lleva a cabo en absoluto
- **C**onsistencia: La base de datos permanece en un estado consistente antes y después de la transacción
- **I**slamiento: Las operaciones de una transacción no son visibles para otras hasta que se completen
- **D**urabilidad: Los cambios realizados por una transacción confirmada son permanentes, incluso en caso de falla del sistema

### 3.5. Escalabilidad
**Definición**: La capacidad de un sistema para manejar un aumento en la carga de trabajo o en el tamaño de los datos.

- **Escalabilidad Vertical**: Aumento de recursos en un solo servidor (CPU, RAM, almacenamiento)
- **Escalabilidad Horizontal**: Distribución de la carga en múltiples servidores

---

## 4. ¿Cuándo usar cada una?

### Usar MySQL (Relacional) cuando:
- ✅ **Integridad de datos crítica**: Sistemas bancarios, contabilidad, reservas
- ✅ **Relaciones complejas**: Múltiples tablas relacionadas con JOINs frecuentes
- ✅ **Transacciones ACID**: Operaciones que deben ser atómicas y consistentes
- ✅ **Datos estructurados**: Esquema estable y predecible
- ✅ **Consultas complejas**: Necesitas JOINs, GROUP BY, funciones de agregación
- ✅ **Casos de uso**: ERPs, sistemas de gestión, e-commerce tradicional

### Usar MongoDB (No Relacional) cuando:
- ✅ **Alto volumen de datos**: Big Data, logs, IoT, redes sociales
- ✅ **Esquema flexible**: Estructura de datos variable o que cambia frecuentemente
- ✅ **Escalabilidad horizontal**: Necesitas distribuir datos en múltiples servidores
- ✅ **Lecturas rápidas**: Optimizado para operaciones de lectura masivas
- ✅ **Datos semi-estructurados**: JSON, documentos, contenido variado
- ✅ **Desarrollo ágil**: Startups, prototipos, aplicaciones que evolucionan rápido
- ✅ **Casos de uso**: Catálogos de productos variados, sistemas de contenido, analytics

### Casos Híbridos
Muchas aplicaciones modernas usan **ambas**:
- **MySQL**: Para datos estructurados y críticos (usuarios, pedidos, pagos)
- **MongoDB**: Para datos flexibles y de alto volumen (logs, analytics, contenido)

---

## 5. Conexión desde Node.js

### MySQL con `mysql2`
```javascript
import mysql from 'mysql2/promise';

// Connection Pool (recomendado)
const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'mi_base_datos',
  waitForConnections: true,
  connectionLimit: 10
});

// Ejecutar query
const [rows] = await pool.query('SELECT * FROM usuarios WHERE id = ?', [1]);
```

**Características:**
- ✅ Connection pool para mejor rendimiento
- ✅ Parámetros preparados (previene SQL Injection)
- ✅ Soporte async/await nativo

### MongoDB con `mongoose`
```javascript
import mongoose from 'mongoose';

// Conectar
await mongoose.connect('mongodb://localhost:27017/mi_base_datos');

// Definir esquema y modelo
const usuarioSchema = new mongoose.Schema({
  nombre: String,
  email: String
});

const Usuario = mongoose.model('Usuario', usuarioSchema);

// Operaciones
const usuario = await Usuario.findOne({ email: 'juan@example.com' });
```

**Características:**
- ✅ ODM (Object Document Mapper) para Node.js
- ✅ Esquemas opcionales con validación
- ✅ Métodos convenientes (find, save, populate)

---

## 🎯 Resumen

| Aspecto | MySQL | MongoDB |
|---------|-------|---------|
| **Mejor para** | Datos estructurados, relaciones complejas | Datos flexibles, alto volumen |
| **Fortaleza** | Integridad, ACID, JOINs | Escalabilidad, flexibilidad, velocidad |
| **Debilidad** | Escalabilidad vertical limitada | Sin integridad referencial nativa |
| **Caso típico** | Sistema bancario, ERP | Red social, catálogo, analytics |

**Conclusión**: No hay una base de datos "mejor". La elección depende de tus necesidades específicas. Muchas aplicaciones modernas usan ambas para diferentes propósitos.
