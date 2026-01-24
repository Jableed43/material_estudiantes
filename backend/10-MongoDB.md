# Master Guide: MongoDB - Base de Datos NoSQL Orientada a Documentos

## 📑 Índice
1. [Introducción a MongoDB](#1-introducción-a-mongodb)
2. [BASE: Modelo de Consistencia](#2-base-modelo-de-consistencia)
3. [Conceptos Fundamentales](#3-conceptos-fundamentales)
4. [Instalación y Herramientas](#4-instalación-y-herramientas)
5. [Operaciones CRUD Básicas](#5-operaciones-crud-básicas)
6. [Operadores de Consulta](#6-operadores-de-consulta)
7. [Expresiones de Consulta](#7-expresiones-de-consulta)
8. [Tipos de Datos](#8-tipos-de-datos)
9. [Esquemas y Validaciones con Mongoose](#9-esquemas-y-validaciones-con-mongoose)
10. [Modificadores de Campos](#10-modificadores-de-campos)
11. [Validators y Custom Validators](#11-validators-y-custom-validators)
12. [Virtuals (Valores Virtuales)](#12-virtuals-valores-virtuales)
13. [Documentos Embebidos](#13-documentos-embebidos)
14. [Aggregation Pipeline](#14-aggregation-pipeline)
15. [Índices](#15-índices)
16. [Conexión desde Node.js con Mongoose](#16-conexión-desde-nodejs-con-mongoose)

---

## 1. Introducción a MongoDB

### 📦 Analogía: El Archivo de Documentos vs El Archivo de Tablas

**MySQL (Relacional)** - Como un archivo de tablas Excel:
- Cada hoja es una tabla
- Filas y columnas fijas
- Si quieres agregar una columna nueva, debes modificar toda la estructura
- Todo debe estar perfectamente organizado antes de empezar

**MongoDB (NoSQL)** - Como un archivo de documentos Word:
- Cada documento es independiente
- Puedes tener documentos con diferentes campos
- Si quieres agregar información nueva, simplemente la agregas al documento
- Flexible y adaptable

### 🗂️ Analogía: La Carpeta de Archivos vs La Base de Datos Relacional

**MySQL**: Como un sistema de archivos con carpetas estrictamente organizadas:
- Carpeta "Autores" - solo archivos de autores
- Carpeta "Libros" - solo archivos de libros
- Si un libro necesita información del autor, debes ir a otra carpeta

**MongoDB**: Como una carpeta donde guardas documentos completos:
- Cada documento puede tener toda la información relacionada junta
- Un documento "Libro" puede incluir la información del autor dentro del mismo documento
- Todo está junto, fácil de encontrar

### 📋 Analogía: El Formulario Rígido vs El Formulario Flexible

**MySQL**: Como un formulario con campos fijos:
- Todos deben llenar los mismos campos
- Si falta un campo, no puedes guardar
- Estructura rígida pero organizada

**MongoDB**: Como un formulario flexible:
- Puedes agregar campos según necesites
- Algunos documentos pueden tener campos que otros no tienen
- Estructura flexible y adaptable

**MongoDB** es una base de datos NoSQL orientada a documentos, en la que los datos se almacenan en estructuras **BSON** (Binary JSON). Es muy popular en aplicaciones modernas por su flexibilidad y escalabilidad.

### Características Principales

*   **Modelo de Documento**: Los datos se organizan en documentos (similar a objetos JSON), no en filas y columnas.
*   **Esquema Flexible**: No requiere definir estructura a priori. Los documentos pueden tener campos diferentes.
*   **Escalabilidad Horizontal**: Diseñada para clústers distribuidos (sharding).
*   **Índices Potentes**: Soporta índices simples, compuestos, de texto y geográficos.
*   **Aggregation Pipeline**: Framework para procesar y transformar datos en el servidor.
*   **Alto Rendimiento**: Optimizado para operaciones de lectura y escritura rápidas.

**Historia**: Lanzado en 2009 por 10gen (hoy MongoDB Inc.), buscando manejar datos en entornos distribuidos para aplicaciones web de alta carga y "Big Data".

### Ventajas de MongoDB

- ✅ **Flexibilidad**: Esquema dinámico que se adapta a cambios
- ✅ **Escalabilidad**: Escala horizontalmente agregando servidores
- ✅ **Rendimiento**: Muy rápido para lecturas masivas
- ✅ **Desarrollo Ágil**: Ideal para prototipos y desarrollo rápido
- ✅ **Documentos Anidados**: Permite estructuras complejas en un solo documento
- ✅ **JSON Nativo**: Formato familiar para desarrolladores web

---

## 2. BASE: Modelo de Consistencia

**BASE** es un acrónimo que describe el modelo de consistencia utilizado por muchas bases de datos NoSQL, incluyendo MongoDB. A diferencia de ACID (usado en bases de datos relacionales), BASE prioriza la **disponibilidad y escalabilidad** sobre la consistencia inmediata.

### ¿Qué es BASE?

**BASE** significa:
- **B**asically **A**vailable (Básicamente Disponible)
- **S**oft state (Estado Suave)
- **E**ventually consistent (Consistencia Eventual)

### Comparación: ACID vs BASE

| Aspecto | ACID (SQL) | BASE (NoSQL/MongoDB) |
|---------|-----------|----------------------|
| **Consistencia** | Inmediata y estricta | Eventual |
| **Disponibilidad** | Puede sacrificarse por consistencia | Prioritaria |
| **Escalabilidad** | Vertical (limitada) | Horizontal (ilimitada) |
| **Transacciones** | Multi-tabla garantizadas | Limitadas o inexistentes |
| **Uso** | Sistemas críticos (bancos) | Sistemas de alto volumen (redes sociales) |

### B - Basically Available (Básicamente Disponible)

**Definición**: El sistema está **disponible la mayor parte del tiempo**, incluso durante actualizaciones o fallos parciales.

**Características:**
- ✅ El sistema responde a las peticiones incluso si algunos nodos fallan
- ✅ Prioriza la disponibilidad sobre la consistencia perfecta
- ✅ Permite que el sistema funcione con datos potencialmente inconsistentes temporalmente
- ✅ Mejor para sistemas distribuidos y de alta carga

**Ejemplo:**
```javascript
// Sistema de likes en una red social
// Si un servidor falla, otros siguen funcionando
// Puede haber pequeñas inconsistencias temporales en el conteo de likes
// Pero el sistema sigue disponible para los usuarios
```

**Ventajas:**
- ✅ Alta disponibilidad (99.9%+ uptime)
- ✅ Tolerancia a fallos parciales
- ✅ Mejor experiencia de usuario (siempre responde)

**Desventajas:**
- ⚠️ Puede haber datos temporalmente inconsistentes
- ⚠️ No garantiza lectura de datos más recientes

### S - Soft State (Estado Suave)

**Definición**: El estado del sistema **puede cambiar sin input** debido a la eventual consistencia. Los datos pueden estar en un estado "intermedio" o "suave".

**Características:**
- ✅ El estado puede cambiar sin nuevas operaciones
- ✅ Los datos pueden estar en proceso de sincronización
- ✅ No hay garantía de que el estado sea "duro" (definitivo) en todo momento
- ✅ El estado se "endurece" eventualmente

**Ejemplo:**
```javascript
// Sistema de replicación en MongoDB
// Documento en servidor A: { likes: 100 }
// Documento en servidor B: { likes: 102 }  // Estado suave, aún sincronizando
// Eventualmente ambos tendrán el mismo valor (estado duro)
```

**Ventajas:**
- ✅ Permite optimizaciones de rendimiento
- ✅ Facilita la distribución de datos
- ✅ Mejor escalabilidad

**Desventajas:**
- ⚠️ Puede haber estados intermedios
- ⚠️ Requiere manejo de conflictos

### E - Eventually Consistent (Consistencia Eventual)

**Definición**: El sistema **eventualmente alcanzará un estado consistente**, pero no garantiza consistencia inmediata en todas las lecturas.

**Características:**
- ✅ Los datos se sincronizarán eventualmente
- ✅ No hay garantía de consistencia inmediata
- ✅ Diferentes nodos pueden mostrar datos diferentes temporalmente
- ✅ La consistencia se logra "eventualmente" (segundos, minutos, o más)

**Ejemplo:**
```javascript
// Sistema distribuido de MongoDB con réplicas
// Usuario A escribe: db.posts.insertOne({ titulo: "Nuevo Post" })
// Usuario B lee inmediatamente: db.posts.find()  // Puede no ver el post aún
// Después de unos segundos: db.posts.find()  // Ahora sí ve el post (consistencia eventual)
```

**Ventajas:**
- ✅ Alta disponibilidad
- ✅ Mejor rendimiento
- ✅ Escalabilidad horizontal

**Desventajas:**
- ⚠️ Puede haber lecturas "stale" (datos antiguos)
- ⚠️ No garantiza consistencia inmediata
- ⚠️ Requiere diseño cuidadoso de la aplicación

### Ejemplo Práctico: Sistema de Likes

**Con ACID (SQL):**
```sql
START TRANSACTION;
UPDATE posts SET likes = likes + 1 WHERE id = 1;
COMMIT;
-- Garantiza que el siguiente usuario siempre verá el like incrementado
-- Consistencia inmediata
```

**Con BASE (MongoDB):**
```javascript
// Usuario A incrementa like
await Post.updateOne(
  { _id: postId },
  { $inc: { likes: 1 } }
);

// Usuario B lee inmediatamente
const post = await Post.findById(postId);
// Puede ver likes: 100 (aún no sincronizado)

// Después de unos segundos
const post2 = await Post.findById(postId);
// Ahora ve likes: 101 (consistencia eventual)
```

### Teorema CAP

El **Teorema CAP** establece que en un sistema distribuido, solo puedes garantizar **2 de 3** propiedades:

- **C**onsistency (Consistencia): Todos los nodos ven los mismos datos al mismo tiempo
- **A**vailability (Disponibilidad): El sistema responde a todas las peticiones
- **P**artition tolerance (Tolerancia a particiones): El sistema funciona incluso si hay fallos de red

**MongoDB (BASE):**
- ✅ **Availability**: Prioritaria
- ✅ **Partition Tolerance**: Prioritaria
- ⚠️ **Consistency**: Sacrificada (eventual)

**MySQL (ACID):**
- ✅ **Consistency**: Prioritaria
- ✅ **Partition Tolerance**: Prioritaria
- ⚠️ **Availability**: Puede sacrificarse (si hay partición, puede no responder)

### Atomicidad en MongoDB

Aunque MongoDB sigue BASE, **las operaciones son atómicas a nivel de un solo documento**:

```javascript
// Esta operación es atómica
await Usuario.updateOne(
  { _id: userId },
  { 
    $inc: { edad: 1 },
    $set: { ultimaActualizacion: new Date() }
  }
);
// Ambas operaciones se ejecutan juntas o ninguna
```

**Limitaciones:**
- ⚠️ Atomicidad solo dentro de un documento
- ⚠️ Operaciones multi-documento no son atómicas por defecto
- ✅ Transacciones multi-documento disponibles desde MongoDB 4.0 (limitadas)

### Transacciones en MongoDB (Desde v4.0)

MongoDB soporta transacciones ACID desde la versión 4.0, pero con limitaciones:

```javascript
const session = await mongoose.startSession();
session.startTransaction();

try {
  await Usuario.create([{ nombre: 'Juan', email: 'juan@example.com' }], { session });
  await Pedido.create([{ usuario_id: usuarioId, total: 100.00 }], { session });
  await Producto.updateOne({ _id: productoId }, { $inc: { stock: -1 } }, { session });
  
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
} finally {
  session.endSession();
}
```

**Limitaciones de Transacciones en MongoDB:**
- ⚠️ Solo funcionan en réplicas (no en standalone)
- ⚠️ Pueden afectar el rendimiento
- ⚠️ Tiempo máximo de transacción limitado
- ⚠️ No tan robustas como en bases de datos relacionales

### Cuándo Usar BASE (MongoDB)

**Ideal para:**
- ✅ **Redes Sociales**: Likes, comentarios, feeds
- ✅ **Sistemas de Logging**: Logs, métricas, analytics
- ✅ **Catálogos de Productos**: E-commerce, inventarios no críticos
- ✅ **Sistemas de Contenido**: CMS, blogs, wikis
- ✅ **IoT**: Datos de sensores, telemetría
- ✅ **Big Data**: Análisis de grandes volúmenes

**No ideal para:**
- ❌ **Sistemas Financieros**: Transferencias bancarias, pagos
- ❌ **Sistemas de Reservas Críticas**: Asientos de avión, habitaciones de hotel
- ❌ **Sistemas donde la consistencia inmediata es crítica**

### Ventajas de BASE

- ✅ **Alta Disponibilidad**: Sistema siempre responde
- ✅ **Escalabilidad Horizontal**: Agregar servidores fácilmente
- ✅ **Mejor Rendimiento**: Optimizado para lecturas masivas
- ✅ **Tolerancia a Fallos**: Funciona con nodos caídos
- ✅ **Flexibilidad**: Esquema dinámico

### Desventajas de BASE

- ⚠️ **Consistencia Eventual**: Puede haber datos temporales inconsistentes
- ⚠️ **Complejidad**: Requiere manejo de conflictos y sincronización
- ⚠️ **Lecturas Stale**: Puedes leer datos antiguos
- ⚠️ **Diseño Complejo**: Requiere pensar en cómo se leerán los datos

### Resumen: ACID vs BASE

| Característica | ACID (SQL) | BASE (MongoDB) |
|----------------|------------|----------------|
| **Consistencia** | Inmediata y estricta | Eventual |
| **Disponibilidad** | Puede sacrificarse | Prioritaria |
| **Atomicidad** | Multi-tabla | Solo documento (o transacciones limitadas) |
| **Uso Ideal** | Sistemas críticos | Sistemas de alto volumen |
| **Garantías** | Fuertes | Débiles pero flexibles |
| **Complejidad** | Media | Alta (manejo de conflictos) |

**Conclusión**: BASE prioriza la disponibilidad y escalabilidad sobre la consistencia inmediata, lo que lo hace ideal para sistemas distribuidos de alto volumen donde pequeñas inconsistencias temporales son aceptables.

---

## 3. Conceptos Fundamentales

### Base de Datos (Database)
Contenedor de colecciones. Se crea automáticamente al insertar el primer documento.

```javascript
use mi_base_de_datos  // Cambiar a una base de datos
```

### Colección (Collection)
Un grupo de documentos. Similar a una "tabla" pero sin esquema fijo.

```javascript
db.usuarios  // Acceder a la colección 'usuarios'
```

### Documento (Document)
La unidad básica de almacenamiento. Similar a un objeto JSON `{ clave: valor }`.

```javascript
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "nombre": "Juan",
  "edad": 30,
  "ciudad": "Madrid"
}
```

### BSON (Binary JSON)
El formato interno en que MongoDB guarda los datos. Soporta más tipos que JSON, como Fechas reales, ObjectId, BinData, etc.

### ObjectId
Identificador único generado automáticamente para cada documento (`_id`). Consta de:
- Timestamp (4 bytes)
- Máquina (3 bytes)
- PID (2 bytes)
- Contador (3 bytes)

```javascript
{
  "_id": ObjectId("507f1f77bcf86cd799439011")
}
```

### Cluster
Conjunto de servidores que trabajan juntos:
- **Replica Sets**: Para disponibilidad y redundancia
- **Shards**: Para distribución de datos (sharding)

### Atlas
Servicio de MongoDB en la nube (DBaaS - Database as a Service).

---

## 4. Instalación y Herramientas

### MongoDB Community Server
El motor de la base de datos (el "servidor").

**Instalación:**
- **Windows**: Descargar desde mongodb.com
- **Mac**: `brew install mongodb-community`
- **Linux**: `apt-get install mongodb` o `yum install mongodb`

### MongoDB Compass
Interfaz gráfica (GUI) oficial. Permite:
- Visualizar datos
- Crear índices
- Ejecutar agregaciones visualmente
- Administrar bases de datos

### MongoShell (`mongosh`)
La consola de comandos moderna para interactuar con la base de datos mediante código JavaScript.

**Iniciar:**
```bash
mongosh
```

**Conectar a servidor remoto:**
```bash
mongosh "mongodb://localhost:27017"
```

---

## 5. Operaciones CRUD Básicas

### CREATE - Insertar Documentos

#### insertOne()
Insertar un solo documento.

```javascript
db.usuarios.insertOne({
  nombre: "Juan",
  edad: 30,
  ciudad: "Madrid"
});
```

#### insertMany()
Insertar múltiples documentos.

```javascript
db.usuarios.insertMany([
  { nombre: "Juan", edad: 30, ciudad: "Madrid" },
  { nombre: "María", edad: 25, ciudad: "Barcelona" },
  { nombre: "Pedro", edad: 35, ciudad: "Valencia" }
]);
```

### READ - Consultar Documentos

#### find()
Buscar documentos. Sin parámetros, devuelve todos.

```javascript
// Todos los documentos
db.usuarios.find();

// Con filtro
db.usuarios.find({ ciudad: "Madrid" });

// Con proyección (solo ciertos campos)
db.usuarios.find({}, { nombre: 1, edad: 1, _id: 0 });

// Limitar resultados
db.usuarios.find().limit(10);

// Ordenar
db.usuarios.find().sort({ edad: -1 });  // -1 descendente, 1 ascendente

// Combinar
db.usuarios.find({ ciudad: "Madrid" })
  .sort({ edad: -1 })
  .limit(5);
```

#### findOne()
Buscar un solo documento.

```javascript
db.usuarios.findOne({ nombre: "Juan" });
```

#### findById()
Buscar por ObjectId (usando Mongoose).

```javascript
const usuario = await Usuario.findById(id);
```

### UPDATE - Actualizar Documentos

#### updateOne()
Actualizar un documento.

```javascript
db.usuarios.updateOne(
  { nombre: "Juan" },
  { $set: { edad: 31 } }
);
```

#### updateMany()
Actualizar múltiples documentos.

```javascript
db.usuarios.updateMany(
  { ciudad: "Madrid" },
  { $set: { pais: "España" } }
);
```

#### Operadores de Actualización

**$set**: Establecer valor
```javascript
{ $set: { edad: 31, ciudad: "Barcelona" } }
```

**$inc**: Incrementar valor numérico
```javascript
{ $inc: { edad: 1 } }  // Incrementa edad en 1
```

**$push**: Agregar elemento a array
```javascript
{ $push: { hobbies: "lectura" } }
```

**$pull**: Eliminar elemento de array
```javascript
{ $pull: { hobbies: "lectura" } }
```

**$unset**: Eliminar campo
```javascript
{ $unset: { ciudad: "" } }
```

### DELETE - Eliminar Documentos

#### deleteOne()
Eliminar un documento.

```javascript
db.usuarios.deleteOne({ nombre: "Juan" });
```

#### deleteMany()
Eliminar múltiples documentos.

```javascript
db.usuarios.deleteMany({ ciudad: "Madrid" });
```

---

## 6. Operadores de Consulta

Los operadores de consulta permiten realizar búsquedas complejas y precisas.

### Operadores de Comparación

#### $eq - Equal (Igual)
Selecciona documentos donde el valor del campo sea igual al valor especificado.

```javascript
db.usuarios.find({ nombre: { $eq: "Juan" } });
// Equivale a:
db.usuarios.find({ nombre: "Juan" });
```

#### $gt - Greater Than (Mayor que)
Selecciona documentos donde el valor del campo sea mayor que el valor especificado.

```javascript
db.usuarios.find({ edad: { $gt: 25 } });
```

#### $lt - Lower Than (Menor que)
Selecciona documentos donde el valor del campo sea menor que el valor especificado.

```javascript
db.usuarios.find({ edad: { $lt: 30 } });
```

#### $gte - Greater Than Equal (Mayor o igual)
Selecciona documentos donde el valor del campo sea mayor o igual que el valor especificado.

```javascript
db.usuarios.find({ edad: { $gte: 18 } });
```

#### $lte - Lower Than Equal (Menor o igual)
Selecciona documentos donde el valor del campo sea menor o igual que el valor especificado.

```javascript
db.usuarios.find({ edad: { $lte: 65 } });
```

#### $ne - Not Equal (No igual)
Selecciona documentos donde el valor del campo no sea igual al valor especificado.

```javascript
db.usuarios.find({ ciudad: { $ne: "Madrid" } });
```

### Operadores de Array

#### $in - In (Por dentro)
Selecciona documentos donde el valor del campo coincida con alguno de los valores especificados en un array.

```javascript
db.usuarios.find({ ciudad: { $in: ["Madrid", "Barcelona", "Valencia"] } });
```

#### $nin - Not In (No hay por dentro)
Selecciona documentos donde el valor del campo no coincida con ninguno de los valores especificados en un array.

```javascript
db.usuarios.find({ ciudad: { $nin: ["Madrid", "Barcelona"] } });
```

#### $all - All (Todos)
Selecciona documentos donde todos los elementos de un array coincidan con los valores especificados.

```javascript
db.usuarios.find({ hobbies: { $all: ["lectura", "deporte"] } });
```

#### $elemMatch - Element Match
Selecciona documentos donde al menos un elemento de un array coincida con los criterios especificados.

```javascript
db.usuarios.find({
  calificaciones: {
    $elemMatch: { materia: "Matemáticas", nota: { $gte: 7 } }
  }
});
```

#### $size - Size (Tamaño)
Selecciona documentos donde el tamaño de un array sea igual al valor especificado.

```javascript
db.usuarios.find({ hobbies: { $size: 3 } });
```

### Operadores de Existencia

#### $exists - Existe
Selecciona documentos donde el campo especificado existe (true) o no existe (false).

```javascript
// Documentos que tienen el campo 'email'
db.usuarios.find({ email: { $exists: true } });

// Documentos que NO tienen el campo 'email'
db.usuarios.find({ email: { $exists: false } });
```

#### $type - Type (Tipo)
Selecciona documentos donde el tipo de datos del campo coincida con el tipo especificado.

```javascript
// Documentos donde 'edad' es Number
db.usuarios.find({ edad: { $type: "number" } });

// Tipos comunes: "string", "number", "boolean", "date", "array", "object"
```

### Operadores de Patrón

#### $regex - Regex (Expresiones Regulares)
Selecciona documentos donde el valor del campo coincida con una expresión regular especificada.

```javascript
// Buscar nombres que terminen con 'M' (case insensitive)
db.usuarios.find({ nombre: { $regex: /M$/i } });

// Buscar nombres que empiecen con 'M'
db.usuarios.find({ nombre: { $regex: /^M/i } });

// Buscar nombres que contengan 'Juan'
db.usuarios.find({ nombre: { $regex: /Juan/i } });
```

**Opciones:**
- `i`: Case insensitive (no distingue mayúsculas/minúsculas)
- `m`: Multiline
- `s`: Dotall

### Operadores Lógicos

#### $and - And (Y)
Selecciona documentos que satisfagan todos los criterios especificados.

```javascript
db.usuarios.find({
  $and: [
    { edad: { $gte: 18 } },
    { ciudad: "Madrid" }
  ]
});

// Equivale a:
db.usuarios.find({
  edad: { $gte: 18 },
  ciudad: "Madrid"
});
```

**Regla:** Cuando dos valores son usados en operación AND, para que el resultado sea válido ambos deben ser verdaderos. Si un valor de los dos es falso, el resultado es falso.

#### $or - Or (O)
Selecciona documentos que satisfagan al menos uno de los criterios especificados.

```javascript
db.usuarios.find({
  $or: [
    { ciudad: "Madrid" },
    { ciudad: "Barcelona" }
  ]
});
```

**Regla:** Cuando dos valores son usados en operación OR, para que el resultado sea inválido ambos deben ser falsos. Si un valor de los dos es verdadero, el resultado es verdadero.

#### $not - Not (No)
Selecciona documentos que no satisfagan los criterios especificados.

```javascript
db.usuarios.find({
  edad: { $not: { $lt: 18 } }  // Edad NO menor que 18
});
```

#### $nor - Nor (Ni)
Selecciona documentos que no satisfacen ninguno de los criterios especificados.

```javascript
db.usuarios.find({
  $nor: [
    { ciudad: "Madrid" },
    { ciudad: "Barcelona" }
  ]
});
```

---

## 6. Expresiones de Consulta

### Ejemplos Prácticos de Consultas

#### Búsqueda Básica
```javascript
// Buscar usuario por nombre
db.usuarios.find({ nombre: { $eq: "Juan" } });
```

#### Búsqueda con Comparación
```javascript
// Usuarios mayores de 25 años
db.usuarios.find({ edad: { $gt: 25 } });

// Usuarios entre 18 y 65 años
db.usuarios.find({
  edad: { $gte: 18, $lte: 65 }
});
```

#### Búsqueda con Arrays
```javascript
// Usuarios que tienen 'lectura' en sus hobbies
db.usuarios.find({ hobbies: "lectura" });

// Usuarios que tienen TODOS estos hobbies
db.usuarios.find({ hobbies: { $all: ["lectura", "deporte"] } });

// Usuarios con exactamente 3 hobbies
db.usuarios.find({ hobbies: { $size: 3 } });
```

#### Búsqueda con Expresiones Regulares
```javascript
// Nombres que empiezan con 'J'
db.usuarios.find({ nombre: { $regex: /^J/i } });

// Nombres que terminan con 'n'
db.usuarios.find({ nombre: { $regex: /n$/i } });

// Nombres que contienen 'uan'
db.usuarios.find({ nombre: { $regex: /uan/i } });
```

#### Búsqueda Combinada
```javascript
// Usuarios de Madrid o Barcelona, mayores de 25 años
db.usuarios.find({
  $and: [
    { ciudad: { $in: ["Madrid", "Barcelona"] } },
    { edad: { $gt: 25 } }
  ]
});
```

---

## 7. Tipos de Datos

MongoDB soporta los siguientes tipos de datos en los esquemas:

### Tipos Básicos

#### String
Almacena cadenas de texto.

```javascript
nombre: String
// O
nombre: { type: String }
```

#### Number
Puede almacenar números enteros o de punto flotante.

```javascript
edad: Number
precio: { type: Number }
```

#### Boolean
Almacena valores booleanos (true o false).

```javascript
activo: Boolean
esAdmin: { type: Boolean, default: false }
```

#### Date
Almacena fechas.

```javascript
fechaCreacion: Date
fechaNacimiento: { type: Date }
```

#### Array
Almacena una lista o matriz de valores.

```javascript
hobbies: [String]  // Array de strings
calificaciones: [Number]  // Array de números
direcciones: [{  // Array de objetos
  calle: String,
  ciudad: String
}]
```

#### Object
Almacena objetos BSON (Binary JSON), que son similares a objetos JSON.

```javascript
direccion: {
  calle: String,
  ciudad: String,
  codigoPostal: String
}
```

#### ObjectId
Almacena el identificador único de un documento en una colección.

```javascript
autor: { type: mongoose.Schema.Types.ObjectId, ref: 'Usuario' }
```

#### Mixed
Permite almacenar cualquier tipo de dato.

```javascript
datosVariados: mongoose.Schema.Types.Mixed
```

### Tipos Especiales

#### Buffer
Almacena datos binarios.

```javascript
imagen: Buffer
```

#### Decimal128
Números decimales de alta precisión.

```javascript
precio: mongoose.Schema.Types.Decimal128
```

---

## 8. Esquemas y Validaciones con Mongoose

Aunque MongoDB es flexible, en una app real queremos que nuestros datos tengan una estructura consistente. **Mongoose** es el ODM (Object Document Mapper) que usamos en Node.js para modelar y validar esos datos.

### Definir Esquema Básico

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const usuarioSchema = new Schema({
  nombre: String,
  email: String,
  edad: Number
});

const Usuario = mongoose.model('Usuario', usuarioSchema);
```

### Restricciones y Validaciones

#### required
Requerir que el campo esté presente en todos los documentos.

```javascript
nombre: { type: String, required: true }
```

#### default
Especificar un valor predeterminado para el campo si no se proporciona ninguno.

```javascript
fechaCreacion: { type: Date, default: Date.now }
rol: { type: String, default: 'user' }
```

#### unique
Garantiza que los valores del campo sean únicos en la colección.

```javascript
email: { type: String, unique: true }
```

#### index
Crea un índice en ese campo para mejorar la eficiencia de las consultas.

```javascript
nombre: { type: String, index: true }
```

**Combinación:**
```javascript
email: { type: String, unique: true, index: true }
```

---

## 9. Modificadores de Campos

Los modificadores permiten transformar valores antes de guardarlos o después de recuperarlos.

### minlength y maxlength
Se utilizan para especificar la longitud mínima y máxima de la cadena para un campo.

```javascript
const usuarioSchema = new Schema({
  username: {
    type: String,
    minlength: 3,  // Mínimo 3 caracteres
    maxlength: 20  // Máximo 20 caracteres
  }
});
```

### trim
Elimina cualquier espacio en blanco adicional al principio o al final de los valores del campo.

```javascript
nombre: { type: String, trim: true }
```

### match
Se usa para aplicar una expresión regular que valida el formato.

```javascript
email: {
  type: String,
  match: /^\S+@\S+\.\S+$/  // Validar formato de email
}
```

**Casos de uso comunes de regex:**
- Validación de formatos de datos como correos electrónicos, números de teléfono y URLs
- Búsqueda de palabras clave específicas dentro del texto
- Validación de contraseñas según criterios de seguridad
- Extracción de información específica de cadenas de texto
- Reemplazo de texto en documentos
- Validación de formatos de fechas

### lowercase / uppercase
Convierte automáticamente los valores del campo a minúsculas o mayúsculas.

```javascript
email: { type: String, lowercase: true }
nombre: { type: String, uppercase: true }
```

### min y max
Se aplican al campo para establecer un rango permitido (se puede usar para fechas o números).

```javascript
edad: {
  type: Number,
  min: 18,   // Edad mínima permitida
  max: 120   // Edad máxima permitida
}

fechaRegistro: {
  type: Date,
  min: '2020-01-01',  // Fecha mínima permitida
  max: Date.now       // Fecha máxima permitida
}
```

### enum
Restringe los valores a un conjunto predefinido.

```javascript
rol: {
  type: String,
  enum: ['admin', 'user', 'guest'],
  default: 'user'
}
```

### set
Modificador que se ejecuta al asignar un valor. Permite transformar el valor antes de guardarlo.

```javascript
const usuarioSchema = new Schema({
  fullName: {
    type: String,
    set: function(value) {
      return value.trim();  // Eliminar espacios en blanco
    }
  }
});
```

### get
Modificador que se ejecuta al acceder al valor. Permite transformar el valor al recuperarlo.

```javascript
const usuarioSchema = new Schema({
  fullName: {
    type: String,
    get: function(value) {
      return value.toUpperCase();  // Convertir a mayúsculas al recuperar
    }
  }
});

// Habilitar getters en JSON
usuarioSchema.set('toJSON', { getters: true });
```

---

## 10. Validators y Custom Validators

### Validator Básico

Puedes utilizar funciones predefinidas o expresiones regulares para validar los valores de los campos.

```javascript
const usuarioSchema = new Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    validate: {
      validator: function(value) {
        return mongoose.models.Usuario.countDocuments({ email: value })
          .exec()
          .then(count => {
            return !count;  // Devuelve verdadero si no hay ningún documento con el mismo email
          });
      },
      message: 'El correo electrónico ya está en uso'  // Mensaje de error personalizado
    }
  }
});
```

### Custom Validator

Puedes definir una función personalizada para validar el valor del campo según tus propios criterios.

```javascript
// Función de validación personalizada
function passwordValidator(value) {
  // La contraseña debe tener al menos 8 caracteres y contener al menos una letra minúscula, una letra mayúscula y un número
  const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;
  return passwordRegex.test(value);
}

const usuarioSchema = new Schema({
  password: {
    type: String,
    required: true,
    validate: {
      validator: passwordValidator,
      message: 'La contraseña debe tener al menos 8 caracteres, una letra minúscula, una letra mayúscula y un número'
    }
  }
});
```

### Ejemplo Completo de Esquema con Validaciones

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const usuarioSchema = new Schema({
  username: {
    type: String,
    required: true,
    minlength: 3,
    maxlength: 20,
    unique: true,
    trim: true
  },
  email: {
    type: String,
    required: true,
    unique: true,
    match: /^\S+@\S+\.\S+$/,
    lowercase: true
  },
  edad: {
    type: Number,
    min: 18,
    max: 120
  },
  rol: {
    type: String,
    enum: ['admin', 'user'],
    default: 'user'
  },
  fechaCreacion: {
    type: Date,
    default: Date.now,
    min: '2020-01-01',
    max: Date.now
  }
});

const Usuario = mongoose.model('Usuario', usuarioSchema);
```

---

## 11. Virtuals (Valores Virtuales)

Un valor virtual es un campo que no se almacena en la base de datos, pero que puede ser calculado o derivado de otros campos en el documento.

### Definir Virtual

```javascript
const usuarioSchema = new Schema({
  firstName: String,
  lastName: String
});

// Virtual para nombre completo
usuarioSchema.virtual('fullName').get(function() {
  return this.firstName + ' ' + this.lastName;
});

// Habilitar virtuals en JSON
usuarioSchema.set('toJSON', { virtuals: true });
```

### Usar Virtual

```javascript
const usuario = await Usuario.findOne({ firstName: 'Juan' });
console.log(usuario.fullName);  // "Juan Pérez"
```

### Virtual con Setter

```javascript
usuarioSchema.virtual('fullName')
  .get(function() {
    return this.firstName + ' ' + this.lastName;
  })
  .set(function(v) {
    const parts = v.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  });

// Usar setter
const usuario = new Usuario();
usuario.fullName = 'Juan Pérez';
console.log(usuario.firstName);  // "Juan"
console.log(usuario.lastName);    // "Pérez"
```

---


## 13. Documentos Embebidos

Los documentos embebidos permiten almacenar documentos completos dentro de otros documentos.

### Documento Embebido Simple

```javascript
const usuarioSchema = new Schema({
  nombre: String,
  direccion: {
    calle: String,
    ciudad: String,
    codigoPostal: String
  }
});
```

### Array de Documentos Embebidos

```javascript
const usuarioSchema = new Schema({
  nombre: String,
  direcciones: [{
    calle: String,
    ciudad: String,
    codigoPostal: String,
    principal: { type: Boolean, default: false }
  }]
});
```

### Cuándo Usar Embebido vs Referencia

**Usar Embebido cuando:**
- ✅ Los datos se leen juntos frecuentemente
- ✅ Los datos anidados no se reutilizan en otros documentos
- ✅ Los datos anidados son pequeños
- ✅ Necesitas atomicidad en el documento completo

**Usar Referencia cuando:**
- ✅ Los datos se actualizan frecuentemente
- ✅ Los datos se reutilizan en múltiples documentos
- ✅ Los datos pueden crecer mucho
- ✅ Necesitas consultar los datos referenciados por separado

---

## 14. Aggregation Pipeline

El Aggregation Pipeline es un framework para procesar datos (filtrar, agrupar, transformar) en el servidor.

### Etapas Comunes

#### $match
Filtra documentos (similar a WHERE).

```javascript
db.usuarios.aggregate([
  { $match: { ciudad: "Madrid" } }
]);
```

#### $group
Agrupa documentos y aplica operadores de agregación.

```javascript
db.usuarios.aggregate([
  {
    $group: {
      _id: "$ciudad",
      total: { $sum: 1 },
      promedioEdad: { $avg: "$edad" }
    }
  }
]);
```

#### $project
Selecciona y transforma campos (similar a SELECT).

```javascript
db.usuarios.aggregate([
  {
    $project: {
      nombre: 1,
      edad: 1,
      esMayor: { $gte: ["$edad", 18] }
    }
  }
]);
```

#### $sort
Ordena documentos.

```javascript
db.usuarios.aggregate([
  { $sort: { edad: -1 } }
]);
```

#### $limit
Limita el número de documentos.

```javascript
db.usuarios.aggregate([
  { $limit: 10 }
]);
```

#### $lookup
Realiza una "unión" con otra colección (similar a JOIN).

```javascript
db.posts.aggregate([
  {
    $lookup: {
      from: "usuarios",
      localField: "autor",
      foreignField: "_id",
      as: "autorInfo"
    }
  }
]);
```

### Ejemplo Completo de Pipeline

```javascript
db.ventas.aggregate([
  // Filtrar ventas del último mes
  {
    $match: {
      fecha: { $gte: new Date("2025-01-01") }
    }
  },
  // Agrupar por producto
  {
    $group: {
      _id: "$producto",
      totalVentas: { $sum: "$cantidad" },
      ingresosTotales: { $sum: { $multiply: ["$cantidad", "$precio"] } }
    }
  },
  // Ordenar por ingresos
  {
    $sort: { ingresosTotales: -1 }
  },
  // Limitar a top 10
  {
    $limit: 10
  }
]);
```

---

## 15. Índices

Los índices mejoran el rendimiento de las consultas.

### Crear Índice Simple

```javascript
// En MongoShell
db.usuarios.createIndex({ email: 1 });  // 1 = ascendente, -1 = descendente

// En Mongoose
usuarioSchema.index({ email: 1 });
```

### Crear Índice Único

```javascript
db.usuarios.createIndex({ email: 1 }, { unique: true });
```

### Crear Índice Compuesto

```javascript
db.usuarios.createIndex({ nombre: 1, apellido: 1 });
```

### Ver Índices

```javascript
db.usuarios.getIndexes();
```

### Eliminar Índice

```javascript
db.usuarios.dropIndex({ email: 1 });
```

---

## 16. Conexión desde Node.js con Mongoose

### Instalación

```bash
npm install mongoose
```

### Configuración de Conexión

```javascript
import mongoose from 'mongoose';

// URI de conexión
const MONGODB_URI = 'mongodb://localhost:27017/mi_base_datos';

// Conectar
mongoose.connect(MONGODB_URI)
  .then(() => {
    console.log('Conectado a MongoDB');
  })
  .catch((error) => {
    console.error('Error al conectar:', error);
  });

// Eventos de conexión
mongoose.connection.on('connected', () => {
  console.log('Mongoose conectado a MongoDB');
});

mongoose.connection.on('error', (error) => {
  console.error('Error de conexión:', error);
});

mongoose.connection.on('disconnected', () => {
  console.log('Mongoose desconectado');
});
```

### Función de Conexión Reutilizable

```javascript
export const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/mi_db');
    console.log('Database connected');
  } catch (error) {
    console.error('Error connecting to database', error);
    process.exit(1);
  }
};
```

### Operaciones CRUD con Mongoose

#### CREATE

```javascript
// Opción 1: new + save
const usuario = new Usuario({
  nombre: 'Juan',
  email: 'juan@example.com',
  edad: 25
});
await usuario.save();

// Opción 2: create
const usuario = await Usuario.create({
  nombre: 'Juan',
  email: 'juan@example.com',
  edad: 25
});
```

#### READ

```javascript
// Todos los documentos
const usuarios = await Usuario.find();

// Con filtro
const usuario = await Usuario.findOne({ email: 'juan@example.com' });

// Por ID
const usuario = await Usuario.findById(id);

// Con condiciones
const adultos = await Usuario.find({ edad: { $gte: 18 } })
  .select('nombre email')
  .sort('-fechaCreacion')
  .limit(10);
```

#### UPDATE

```javascript
// Opción 1: findByIdAndUpdate
const usuario = await Usuario.findByIdAndUpdate(
  id,
  { edad: 26 },
  { new: true }  // Devuelve el documento actualizado
);

// Opción 2: updateOne / updateMany
await Usuario.updateOne(
  { email: 'juan@example.com' },
  { edad: 26 }
);

// Opción 3: Modificar y guardar
const usuario = await Usuario.findOne({ email: 'juan@example.com' });
usuario.edad = 26;
await usuario.save();
```

#### DELETE

```javascript
// Opción 1: findByIdAndDelete
await Usuario.findByIdAndDelete(id);

// Opción 2: deleteOne / deleteMany
await Usuario.deleteOne({ email: 'juan@example.com' });
await Usuario.deleteMany({ activo: false });
```

### Integración con Express

```javascript
import express from 'express';
import mongoose from 'mongoose';
import { connectDB } from './db.js';
import Usuario from './models/Usuario.js';

connectDB();

const app = express();
app.use(express.json());

// GET - Consultar
app.get('/usuarios', async (req, res) => {
  try {
    const usuarios = await Usuario.find();
    res.json(usuarios);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// POST - Crear
app.post('/usuarios', async (req, res) => {
  try {
    const { nombre, email, edad } = req.body;
    const nuevoUsuario = new Usuario({ nombre, email, edad });
    await nuevoUsuario.save();
    res.status(201).json(nuevoUsuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// PUT - Actualizar
app.put('/usuarios/:id', async (req, res) => {
  try {
    const { id } = req.params;
    const usuario = await Usuario.findByIdAndUpdate(
      id,
      req.body,
      { new: true }
    );
    res.json(usuario);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// DELETE - Eliminar
app.delete('/usuarios/:id', async (req, res) => {
  try {
    const { id } = req.params;
    await Usuario.findByIdAndDelete(id);
    res.json({ message: 'Usuario eliminado' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.listen(3000, () => {
  console.log('Servidor en puerto 3000');
});
```

---

## 🎯 Resumen de Conceptos Clave

### Estructura
- **Base de Datos**: Contenedor de colecciones
- **Colección**: Grupo de documentos
- **Documento**: Unidad básica de datos (JSON/BSON)

### Operaciones CRUD
- `insertOne()` / `insertMany()`: Crear
- `find()` / `findOne()`: Leer
- `updateOne()` / `updateMany()`: Actualizar
- `deleteOne()` / `deleteMany()`: Eliminar

### Operadores de Consulta
- Comparación: `$eq`, `$gt`, `$lt`, `$gte`, `$lte`, `$ne`
- Array: `$in`, `$nin`, `$all`, `$elemMatch`, `$size`
- Lógicos: `$and`, `$or`, `$not`, `$nor`
- Patrón: `$regex`
- Existencia: `$exists`, `$type`

### Mongoose
- **Schema**: Define la estructura del documento
- **Model**: Representa una colección
- **Validators**: Validación de datos
- **Virtuals**: Campos calculados
- **Populate**: Cargar referencias

### Mejores Prácticas
- ✅ Usar esquemas con Mongoose para validación
- ✅ Crear índices en campos de búsqueda frecuente
- ✅ Usar documentos embebidos para datos que se leen juntos
- ✅ Usar referencias para datos que se actualizan frecuentemente
- ✅ Usar aggregation pipeline para consultas complejas

---

**Referencias del Código Modelo:**
- `cursadas/backend/backEnd_modelo/tema-10-mongodb-introduccion/`
