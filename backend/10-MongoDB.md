# Master Guide: MongoDB - Base de Datos NoSQL Orientada a Documentos

## 📑 Índice
1. [Introducción a MongoDB](#1-introducción-a-mongodb)
   - [1.1 Transición desde MySQL](#11-transición-desde-mysql)
   - [1.2 Diccionario de Equivalencias: MySQL ↔ MongoDB](#12-diccionario-de-equivalencias-mysql--mongodb)
   - [1.3 Comparación Detallada: Conceptos Fundamentales](#13-comparación-detallada-conceptos-fundamentales)
   - [1.4 Tabla Comparativa Completa](#14-tabla-comparativa-completa)
   - [1.5 ¿Cuándo usar cada una?](#15-cuándo-usar-cada-una)
2. [Instalación y Herramientas](#2-instalación-y-herramientas)
3. [BASE: Modelo de Consistencia](#3-base-modelo-de-consistencia)
4. [Conceptos Fundamentales](#4-conceptos-fundamentales)
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

## 2. Instalación y Herramientas

> **⚠️ Importante**: Antes de continuar con el resto del contenido, asegúrate de tener MongoDB instalado y funcionando. Esta sección te guiará paso a paso.

### MongoDB Community Server
El motor de la base de datos (el "servidor"). Es necesario instalarlo para que MongoDB funcione.

**Descarga e Instalación:**
- **Enlace de descarga**: [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
- **Windows**: Descargar el instalador `.msi` desde el enlace, ejecutar y seguir el asistente de instalación
- **Mac**: 
  ```bash
  brew install mongodb-community
  ```
- **Linux**: 
  ```bash
  # Ubuntu/Debian
  apt-get install mongodb
  
  # CentOS/RHEL
  yum install mongodb
  ```

**Iniciar el servidor:**
```bash
# Windows (después de la instalación, se inicia automáticamente como servicio)
# O manualmente desde la línea de comandos:
mongod

# Mac/Linux
mongod
```

### MongoDB Compass
Interfaz gráfica (GUI) oficial de MongoDB. Permite:
- Visualizar datos de forma gráfica
- Crear y gestionar índices
- Ejecutar agregaciones visualmente
- Administrar bases de datos y colecciones
- Ejecutar consultas y ver resultados
- **Incluye un shell integrado** para ejecutar comandos directamente

**Descarga e Instalación:**
- **Enlace de descarga**: [https://www.mongodb.com/try/download/compass](https://www.mongodb.com/try/download/compass)
- **Windows**: Descargar el instalador `.msi`, ejecutar y seguir el asistente
- **Mac**: Descargar el archivo `.dmg` e instalar
- **Linux**: Descargar el paquete `.deb` o `.rpm` según tu distribución

**Uso:**
- Abre MongoDB Compass
- Conecta a `mongodb://localhost:27017` (o tu servidor remoto)
- Explora tus bases de datos y colecciones visualmente

**⚠️ Nota importante sobre MongoDB Shell:**
- **Compass incluye un shell integrado** en su interfaz, por lo que puedes ejecutar comandos de MongoDB directamente desde Compass sin necesidad de instalar `mongosh` por separado.
- Sin embargo, si prefieres usar la **terminal/consola de comandos** para trabajar con MongoDB, necesitarás instalar `mongosh` por separado (ver sección siguiente).

### MongoDB Shell (`mongosh`)
La consola de comandos moderna para interactuar con la base de datos mediante código JavaScript desde la terminal.

**¿Necesito instalar mongosh si ya tengo Compass?**
- **NO es estrictamente necesario** si solo usas Compass, ya que Compass incluye un shell integrado.
- **SÍ es recomendable** si prefieres trabajar desde la terminal, necesitas automatizar tareas con scripts, o quieres usar MongoDB en entornos sin interfaz gráfica.

**Descarga e Instalación:**
- **Enlace de descarga**: [https://www.mongodb.com/try/download/shell](https://www.mongodb.com/try/download/shell)
- **Windows**: Descargar el instalador `.msi` y seguir el asistente
- **Mac**: Descargar el archivo `.tgz` o usar Homebrew:
  ```bash
  brew install mongosh
  ```
- **Linux**: Descargar el paquete `.deb` o `.rpm` según tu distribución

**Iniciar:**
```bash
mongosh
```

**Conectar a servidor local:**
```bash
mongosh "mongodb://localhost:27017"
```

**Conectar a servidor remoto:**
```bash
mongosh "mongodb://usuario:password@servidor:27017/nombre_db"
```

**Conectar a MongoDB Atlas (nube):**
```bash
mongosh "mongodb+srv://usuario:password@cluster.mongodb.net/nombre_db"
```

### Resumen de Herramientas

| Herramienta | ¿Qué es? | ¿Cuándo usarla? | ¿Es obligatoria? |
|-------------|----------|-----------------|------------------|
| **MongoDB Community Server** | El servidor de base de datos | Siempre (necesario para que MongoDB funcione) | ✅ Sí, obligatorio |
| **MongoDB Compass** | Interfaz gráfica (GUI) | Para visualizar datos, administrar bases de datos, trabajar con interfaz gráfica | ⚠️ Opcional pero muy recomendado |
| **MongoDB Shell (mongosh)** | Consola de comandos | Para trabajar desde terminal, automatizar tareas, scripts | ⚠️ Opcional (Compass incluye shell integrado) |

**Recomendación para principiantes:**
1. Instala **MongoDB Community Server** (obligatorio)
2. Instala **MongoDB Compass** (muy recomendado para empezar)
3. Instala **mongosh** solo si prefieres trabajar desde la terminal o necesitas automatizar tareas

---

## 1.1 Transición desde MySQL

### Recordando lo que ya sabemos de MySQL

Si vienes de aprender MySQL, ya conoces:
- ✅ **Tablas** con filas y columnas
- ✅ **Esquema estricto** (debes definir estructura antes)
- ✅ **Relaciones** con claves foráneas (FOREIGN KEY)
- ✅ **JOINs** para consultar múltiples tablas
- ✅ **SQL** como lenguaje de consulta
- ✅ **ACID** completo para transacciones
- ✅ **Normalización** de datos

### ¿Qué es MongoDB?

**MongoDB** es una base de datos **NoSQL orientada a documentos**. A diferencia de MySQL que organiza datos en tablas, MongoDB organiza datos en **documentos** (similares a objetos JSON).

**Analogía simple:**
- **MySQL**: Como un archivo Excel con hojas (tablas) organizadas en filas y columnas
- **MongoDB**: Como una carpeta con documentos Word, cada uno con su propia estructura

---

## 1.2 Diccionario de Equivalencias: MySQL ↔ MongoDB

Esta tabla te ayudará a entender cómo se llaman las cosas en cada sistema:

| Concepto MySQL (SQL) | Equivalente MongoDB (NoSQL) | Explicación |
|---------------------|----------------------------|-------------|
| **Base de Datos** | **Base de Datos** | Mismo concepto |
| **Tabla** | **Colección** | Grupo de datos relacionados |
| **Fila / Registro** | **Documento** | Unidad básica de datos |
| **Columna / Campo** | **Campo** | Atributo dentro del documento |
| **PRIMARY KEY** | **_id (ObjectId)** | Identificador único automático |
| **FOREIGN KEY** | **Referencia (ObjectId)** | Relación con otro documento |
| **JOIN** | **populate() o $lookup** | Combinar datos de múltiples colecciones |
| **Esquema Estricto** | **Esquema Flexible** | Estructura de datos |
| **ACID** | **BASE** | Modelo de consistencia |
| **Normalización** | **Desnormalización** | Organización de datos |
| **SQL** | **MongoDB Query Language** | Lenguaje de consulta |
| **Transacción Multi-tabla** | **Transacción Multi-documento (limitada)** | Operaciones atómicas |
| **Escalabilidad Vertical** | **Escalabilidad Horizontal** | Cómo crecer |
| **mysql2** (Node.js) | **mongoose** (Node.js) | Librería de conexión |

---

## 1.3 Comparación Detallada: Conceptos Fundamentales

### 1.3.1 Estructura de Datos

#### MySQL (Tablas, Filas, Columnas)
```sql
-- PASO 1: DEBES crear la tabla primero (estructura obligatoria)
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    edad INT,
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- PASO 2: Luego puedes insertar datos
INSERT INTO usuarios (nombre, email, edad) 
VALUES ('Juan', 'juan@example.com', 25);
```

**Características:**
- ✅ Estructura **fija** y **predefinida**
- ✅ **DEBES crear la tabla antes** de insertar datos
- ✅ Todas las filas tienen las **mismas columnas**
- ✅ Tipos de datos **estrictos** (INT, VARCHAR, DATETIME)
- ✅ Restricciones de integridad (NOT NULL, UNIQUE, FOREIGN KEY)

#### MongoDB (Colecciones, Documentos, Campos)
```javascript
// PASO 1: NO necesitas crear la colección, simplemente insertas
// Si la colección no existe, MongoDB la crea automáticamente al insertar el primer documento

// PASO 2: Insertar datos (la colección se crea automáticamente si no existe)
db.usuarios.insertOne({
  nombre: "Juan",
  email: "juan@example.com",
  edad: 25,
  fecha_creacion: new Date("2025-01-15T10:30:00Z"),
  direccion: {  // Documento anidado
    calle: "Av. Principal",
    ciudad: "Buenos Aires"
  }
});

// La colección 'usuarios' ahora existe y contiene el documento insertado
```

**Características:**
- ✅ Estructura **flexible** (cada documento puede tener campos diferentes)
- ✅ **La colección se crea automáticamente** al insertar el primer documento
- ✅ No necesitas crear la colección explícitamente (a diferencia de MySQL)
- ✅ Permite **documentos anidados**
- ✅ No requiere esquema predefinido (aunque Mongoose permite definirlo)
- ✅ Formato **JSON/BSON** (Binary JSON)

**Diferencia Crucial:**
- **MySQL**: DEBES crear la tabla antes de insertar datos (`CREATE TABLE` es obligatorio)
- **MongoDB**: La colección se crea automáticamente al insertar el primer documento (no necesitas `CREATE COLLECTION`)

### 1.3.2 Esquema: Estricto vs Flexible

#### MySQL: Esquema Obligatorio
```sql
-- ❌ DEBES definir el esquema antes de insertar
CREATE TABLE productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(255) NOT NULL,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0
);

-- ❌ Error: No puedes insertar sin definir todas las columnas
-- ❌ Error: No puedes agregar campos nuevos sin ALTER TABLE
INSERT INTO productos (nombre, precio, stock, nuevo_campo) 
VALUES ('Laptop', 999.99, 10, 'valor');  -- ❌ Error: nuevo_campo no existe
```

#### MongoDB: Esquema Opcional
```javascript
// ✅ Puedes insertar sin definir esquema
db.productos.insertOne({
  nombre: "Laptop",
  precio: 999.99,
  stock: 10
});

// ✅ Puedes agregar campos nuevos sin modificar estructura
db.productos.insertOne({
  nombre: "Mouse",
  precio: 25.50,
  stock: 50,
  color: "negro",  // Campo nuevo, sin problema
  garantia: "1 año"  // Otro campo nuevo
});
```

### 1.3.3 Relaciones: JOINs vs Referencias/Embebido

#### MySQL: Relaciones Explícitas con JOINs

**Características:**
- ✅ **Relaciones explícitas** mediante claves foráneas (FOREIGN KEY)
- ✅ **Integridad referencial nativa**: La base de datos garantiza que las relaciones sean válidas
- ✅ **JOINs nativos y eficientes**: Puedes combinar datos de múltiples tablas en una sola consulta
- ✅ **Validación automática**: No puedes eliminar un registro si tiene referencias en otras tablas
- ✅ **Consultas complejas**: Puedes hacer JOINs entre múltiples tablas fácilmente
- ✅ **Normalización**: Los datos relacionados se mantienen en tablas separadas para evitar duplicación

**Ejemplo conceptual:**
- Tabla `usuarios` con `id` como PRIMARY KEY
- Tabla `posts` con `usuario_id` como FOREIGN KEY que referencia a `usuarios.id`
- Al consultar, usas `JOIN` para combinar ambas tablas y obtener datos relacionados

#### MongoDB: Referencias o Documentos Embebidos

**Importante:** MongoDB **NO tiene relaciones** como MySQL. En su lugar, usa dos estrategias:

**Opción 1: Referencias (Similar a Foreign Key, pero sin integridad referencial)**

**Características:**
- ✅ Almacenas el `ObjectId` de otro documento como referencia
- ✅ Similar conceptualmente a una FOREIGN KEY, pero **sin validación automática**
- ✅ **Sin integridad referencial nativa**: Puedes tener referencias a documentos que no existen
- ✅ **Consultas separadas**: Necesitas hacer múltiples consultas o usar `populate()` (con Mongoose) o `$lookup` (en aggregation)
- ✅ **Datos normalizados**: Los datos relacionados se mantienen en colecciones separadas
- ✅ **Flexibilidad**: Puedes actualizar un documento sin afectar las referencias

**Cuándo usar referencias:**
- Cuando los datos relacionados se actualizan frecuentemente
- Cuando los datos relacionados se reutilizan en múltiples documentos
- Cuando los datos relacionados pueden crecer mucho
- Cuando necesitas consultar los datos relacionados por separado

**Opción 2: Documentos Embebidos (Todo en un solo documento)**

**Características:**
- ✅ Almacenas los datos relacionados **dentro del mismo documento**
- ✅ **Una sola consulta**: Obtienes todos los datos relacionados de una vez
- ✅ **Mejor rendimiento**: No necesitas hacer JOINs o consultas adicionales
- ✅ **Atomicidad**: Las operaciones en el documento completo son atómicas
- ✅ **Desnormalización**: Los datos pueden estar duplicados en múltiples documentos

**Cuándo usar documentos embebidos:**
- Cuando los datos relacionados se leen juntos frecuentemente
- Cuando los datos relacionados no se reutilizan en otros documentos
- Cuando los datos relacionados son pequeños y no cambian frecuentemente
- Cuando necesitas atomicidad en el documento completo

**Diferencias Clave:**

| Aspecto | MySQL (Relaciones) | MongoDB (Referencias) | MongoDB (Embebido) |
|---------|-------------------|----------------------|-------------------|
| **Integridad Referencial** | Nativa y automática | Manual (en aplicación) | No aplica |
| **Validación** | Automática por BD | Manual | No aplica |
| **Consultas** | JOIN nativo | Múltiples consultas o $lookup | Una sola consulta |
| **Rendimiento** | Depende de JOINs | Puede ser más lento | Más rápido para lecturas |
| **Atomicidad** | Multi-tabla | Limitada | Solo documento |
| **Duplicación** | Evitada (normalización) | Evitada | Permitida (desnormalización) |

### 1.3.4 Transacciones: ACID vs BASE

#### MySQL: ACID Completo

**ACID** es un acrónimo que describe las propiedades fundamentales de las transacciones en bases de datos relacionales:

- **A**tomicity (Atomicidad): Una transacción se ejecuta completamente o no se ejecuta en absoluto. No hay estados intermedios. Si cualquier parte de la transacción falla, toda la transacción se revierte (ROLLBACK).

- **C**onsistency (Consistencia): La base de datos permanece en un estado válido antes y después de la transacción. Todas las reglas de integridad, restricciones y validaciones se cumplen. No se pueden violar las reglas de negocio.

- **I**solation (Aislamiento): Las transacciones concurrentes no interfieren entre sí. Cada transacción ve una "instantánea" consistente de los datos, incluso si otras transacciones están ejecutándose simultáneamente. Los cambios de una transacción no son visibles para otras hasta que se confirma (COMMIT).

- **D**urability (Durabilidad): Una vez que una transacción se confirma (COMMIT), los cambios son permanentes y sobreviven a fallos del sistema. Los datos se guardan en almacenamiento persistente y no se pierden.

**Características de ACID en MySQL:**
- ✅ **Garantías fuertes**: Los datos siempre están en un estado consistente y válido
- ✅ **Transacciones multi-tabla**: Puedes modificar múltiples tablas en una sola transacción
- ✅ **Consistencia inmediata**: Todos los usuarios ven los cambios inmediatamente después del COMMIT
- ✅ **Integridad garantizada**: Las restricciones y claves foráneas se validan automáticamente
- ✅ **Ideal para operaciones críticas**: Sistemas bancarios, reservas, contabilidad, donde la integridad es esencial

**Ejemplo conceptual:**
Si transfieres dinero entre dos cuentas bancarias, ACID garantiza que:
- O ambas cuentas se actualizan (débito y crédito), o ninguna (atomicidad)
- El saldo total siempre es correcto (consistencia)
- Otras transacciones no ven estados intermedios (aislamiento)
- Una vez confirmado, el cambio es permanente (durabilidad)

#### MongoDB: BASE

**BASE** es el modelo de consistencia utilizado por MongoDB y muchas bases de datos NoSQL:

- **B**asically **A**vailable (Básicamente Disponible): El sistema está disponible la mayor parte del tiempo, incluso durante actualizaciones o fallos parciales. Prioriza que el sistema responda a las peticiones sobre tener datos perfectamente consistentes en todo momento.

- **S**oft state (Estado Suave): El estado del sistema puede cambiar sin input debido a la eventual consistencia. Los datos pueden estar en un estado "intermedio" o "suave" mientras se sincronizan entre diferentes nodos o servidores.

- **E**ventually consistent (Consistencia Eventual): El sistema eventualmente alcanzará un estado consistente, pero no garantiza consistencia inmediata en todas las lecturas. Diferentes usuarios o servidores pueden ver datos ligeramente diferentes temporalmente, pero eventualmente todos verán el mismo estado.

**Características de BASE en MongoDB:**
- ✅ **Alta disponibilidad**: El sistema siempre responde, incluso con fallos parciales
- ✅ **Escalabilidad horizontal**: Puede distribuir datos en múltiples servidores fácilmente
- ✅ **Mejor rendimiento**: Optimizado para operaciones de lectura masivas
- ⚠️ **Consistencia eventual**: Puede haber pequeñas inconsistencias temporales entre nodos
- ⚠️ **Atomicidad limitada**: Por defecto, solo garantiza atomicidad a nivel de un solo documento
- ⚠️ **Transacciones limitadas**: Transacciones multi-documento disponibles desde MongoDB 4.0, pero con limitaciones
- ✅ **Ideal para sistemas de alto volumen**: Redes sociales, logs, analytics, donde pequeñas inconsistencias temporales son aceptables

**Ejemplo conceptual:**
En un sistema de likes en una red social con BASE:
- Si un usuario da like, el sistema responde inmediatamente (disponibilidad)
- El conteo de likes puede variar ligeramente entre servidores temporalmente (estado suave)
- Después de unos segundos, todos los servidores mostrarán el mismo conteo (consistencia eventual)
- Esto es aceptable porque un like más o menos temporalmente no es crítico

**Diferencias Clave entre ACID y BASE:**

| Aspecto | ACID (MySQL) | BASE (MongoDB) |
|---------|--------------|----------------|
| **Prioridad** | Consistencia inmediata | Disponibilidad y escalabilidad |
| **Atomicidad** | Multi-tabla garantizada | Solo documento (o limitada) |
| **Consistencia** | Inmediata y estricta | Eventual (puede haber retraso) |
| **Disponibilidad** | Puede sacrificarse por consistencia | Prioritaria (siempre responde) |
| **Transacciones** | Robustas y completas | Limitadas o inexistentes |
| **Uso Ideal** | Sistemas críticos (bancos, reservas) | Sistemas de alto volumen (redes sociales, logs) |
| **Garantías** | Fuertes y estrictas | Débiles pero flexibles |
| **Complejidad** | Media | Alta (requiere manejo de conflictos) |

### 1.3.5 Escalabilidad: Vertical vs Horizontal

#### MySQL: Escalabilidad Vertical

**Definición:** Escalar verticalmente significa aumentar la capacidad de un **solo servidor** agregando más recursos (CPU, RAM, almacenamiento, velocidad de disco).

```
┌─────────────────┐
│  Servidor Único │
│  ┌───────────┐  │
│  │   MySQL   │  │
│  │  (Más RAM)│  │
│  │  (Más CPU)│  │
│  │  (Más SSD)│  │
│  └───────────┘  │
└─────────────────┘
```

**Características:**
- ✅ Escala **verticalmente** (más recursos al mismo servidor)
- ✅ **Más simple de implementar**: Solo necesitas actualizar un servidor
- ✅ **Mejor para cargas predecibles**: Cuando sabes cuánto crecimiento esperar
- ✅ **Sin cambios en la aplicación**: No necesitas modificar código para escalar
- ❌ **Limitado por hardware**: Eventualmente alcanzas el límite físico del servidor
- ❌ **Más costoso**: Hardware potente (servidores con mucha RAM y CPU) es caro
- ❌ **Punto único de fallo**: Si el servidor falla, todo el sistema falla
- ❌ **Límites físicos**: No puedes agregar recursos infinitamente a un solo servidor

**Ejemplo:**
- Servidor inicial: 8GB RAM, 4 CPU cores
- Escalamiento vertical: Actualizar a 32GB RAM, 16 CPU cores
- Límite: No puedes ir más allá de lo que el hardware permite

#### MongoDB: Escalabilidad Horizontal

**Definición:** Escalar horizontalmente significa agregar **más servidores** al sistema y distribuir la carga entre ellos. Cada servidor maneja una porción de los datos.

```
┌──────────┐  ┌──────────┐  ┌──────────┐
│ Servidor │  │ Servidor │  │ Servidor │
│  MongoDB │  │  MongoDB │  │  MongoDB │
│  (Shard) │  │  (Shard) │  │  (Shard) │
│  Datos A │  │  Datos B │  │  Datos C │
└──────────┘  └──────────┘  └──────────┘
       │            │            │
       └────────────┴────────────┘
              │
         Load Balancer
         (Distribuye consultas)
```

**Características:**
- ✅ Escala **horizontalmente** (más servidores)
- ✅ **Más económico**: Servidores estándar son más baratos que servidores súper potentes
- ✅ **Escalabilidad casi ilimitada**: Puedes agregar tantos servidores como necesites
- ✅ **Mejor para grandes volúmenes**: Ideal para Big Data y sistemas con millones de documentos
- ✅ **Distribución automática**: MongoDB distribuye datos automáticamente mediante sharding
- ✅ **Tolerancia a fallos**: Si un servidor falla, los otros siguen funcionando
- ⚠️ **Más complejo de implementar**: Requiere configuración de sharding y balanceo de carga
- ⚠️ **Requiere cambios en la aplicación**: Puede necesitar ajustes para trabajar con datos distribuidos
- ⚠️ **Consistencia eventual**: Los datos pueden estar ligeramente desincronizados entre servidores

**Sharding en MongoDB:**
- **Shard**: Cada servidor que almacena una porción de los datos
- **Shard Key**: Campo usado para determinar en qué shard se almacena cada documento
- **Config Server**: Mantiene metadatos sobre la distribución de datos
- **Mongos**: Router que dirige las consultas al shard correcto

**Ejemplo:**
- Sistema inicial: 1 servidor con 100GB de datos
- Escalamiento horizontal: Agregar 2 servidores más, distribuir datos (33GB cada uno)
- Crecimiento: Agregar más servidores según sea necesario (4, 5, 10 servidores...)

**Comparación Práctica:**

| Aspecto | Escalabilidad Vertical (MySQL) | Escalabilidad Horizontal (MongoDB) |
|---------|-------------------------------|-----------------------------------|
| **Costo inicial** | Más bajo (un servidor) | Más alto (múltiples servidores) |
| **Costo a largo plazo** | Más alto (hardware potente) | Más bajo (servidores estándar) |
| **Límite de crecimiento** | Limitado por hardware | Prácticamente ilimitado |
| **Complejidad** | Baja (un servidor) | Alta (múltiples servidores) |
| **Tolerancia a fallos** | Baja (punto único) | Alta (múltiples puntos) |
| **Mejor para** | Cargas predecibles, datos estructurados | Big Data, alto volumen, datos distribuidos |

### 1.3.6 Lenguaje de Consulta: SQL vs MongoDB Query Language

#### MySQL: SQL (Structured Query Language)

**Características:**
- ✅ **Lenguaje estándar**: SQL es un estándar internacional (ANSI/ISO) usado por todas las bases de datos relacionales
- ✅ **Declarativo**: Describes QUÉ quieres, no CÓMO obtenerlo (la BD decide cómo optimizar)
- ✅ **Maduro y estable**: SQL existe desde los años 70, muy probado y documentado
- ✅ **Portable**: El mismo SQL funciona (con pequeñas variaciones) en MySQL, PostgreSQL, SQL Server, Oracle
- ✅ **JOINs nativos**: Combinar datos de múltiples tablas es natural y eficiente
- ✅ **Funciones de agregación potentes**: COUNT, SUM, AVG, MAX, MIN, GROUP BY, HAVING
- ✅ **Optimización automática**: El motor de base de datos optimiza las consultas automáticamente
- ✅ **Subconsultas**: Puedes anidar consultas dentro de otras consultas
- ✅ **Vistas**: Puedes crear vistas (consultas guardadas) para simplificar consultas complejas

**Ejemplo:**
```sql
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

**Estructura típica de SQL:**
- `SELECT`: Qué columnas quieres
- `FROM`: De qué tablas
- `WHERE`: Filtros antes de agrupar
- `JOIN`: Combinar tablas
- `GROUP BY`: Agrupar por campos
- `HAVING`: Filtros después de agrupar
- `ORDER BY`: Ordenar resultados
- `LIMIT`: Limitar cantidad de resultados

#### MongoDB: MongoDB Query Language

**Características:**
- ✅ **Lenguaje específico**: Diseñado específicamente para MongoDB y documentos JSON/BSON
- ✅ **Basado en JavaScript**: Usa sintaxis similar a JavaScript, familiar para desarrolladores web
- ✅ **Consultas basadas en documentos**: Trabajas con objetos JSON, no con filas y columnas
- ✅ **Pipeline de agregación**: Procesa datos en etapas (similar a pipes en Unix)
- ✅ **Flexible**: Puedes consultar campos anidados y arrays fácilmente
- ⚠️ **JOINs más costosos**: `$lookup` (equivalente a JOIN) es más lento que JOINs nativos de SQL
- ⚠️ **Menos estándar**: Cada base de datos NoSQL tiene su propio lenguaje de consulta
- ✅ **Operadores potentes**: `$match`, `$group`, `$project`, `$sort`, `$limit`, `$unwind`, etc.
- ✅ **Expresiones complejas**: Puedes usar expresiones JavaScript en algunas operaciones
- ⚠️ **Curva de aprendizaje**: Requiere aprender sintaxis específica de MongoDB

**Ejemplo equivalente:**
```javascript
// Consultas con MongoDB Query Language
const usuarios = await Usuario.aggregate([
  { $match: { activo: true } },  // WHERE
  { $lookup: {  // JOIN
      from: 'posts',
      localField: '_id',
      foreignField: 'autor',
      as: 'posts'
    }
  },
  { $project: {  // SELECT
      nombre: 1,
      total_posts: { $size: '$posts' },  // COUNT
      promedio_likes: { $avg: '$posts.likes' }  // AVG
    }
  },
  { $match: { total_posts: { $gt: 5 } } },  // HAVING
  { $sort: { promedio_likes: -1 } },  // ORDER BY
  { $limit: 10 }  // LIMIT
]);
```

**Estructura típica de MongoDB Query:**
- **Consultas simples**: `find({ campo: valor })` - similar a `WHERE`
- **Pipeline de agregación**: Array de etapas que procesan datos secuencialmente
- `$match`: Filtra documentos (equivalente a WHERE)
- `$lookup`: Combina colecciones (equivalente a JOIN)
- `$group`: Agrupa documentos (equivalente a GROUP BY)
- `$project`: Selecciona campos (equivalente a SELECT)
- `$sort`: Ordena resultados (equivalente a ORDER BY)
- `$limit`: Limita resultados (equivalente a LIMIT)

**Diferencias Clave:**

| Aspecto | SQL (MySQL) | MongoDB Query Language |
|---------|-------------|------------------------|
| **Sintaxis** | Declarativa, basada en palabras clave | Basada en JavaScript/JSON |
| **Estructura** | Filas y columnas | Documentos y campos |
| **JOINs** | Nativos y eficientes | `$lookup` más costoso |
| **Consultas anidadas** | Subconsultas nativas | Pipeline de agregación |
| **Campos anidados** | Requiere JOINs o funciones especiales | Acceso directo con notación de punto |
| **Arrays** | Requiere funciones especiales | Manejo nativo y potente |
| **Estándar** | ANSI/ISO (portable) | Específico de MongoDB |
| **Curva de aprendizaje** | Media (si conoces SQL) | Media-Alta (sintaxis nueva) |
| **Optimización** | Automática por el motor | Automática pero menos madura |
| **Flexibilidad** | Estructura fija (tablas) | Estructura flexible (documentos) |

**Ventajas de SQL:**
- Más maduro y probado
- Estándar internacional
- Mejor para consultas complejas con múltiples JOINs
- Más documentación y recursos disponibles

**Ventajas de MongoDB Query Language:**
- Más natural para desarrolladores JavaScript
- Mejor para consultar documentos anidados y arrays
- Pipeline de agregación muy potente y flexible
- Sintaxis más concisa para operaciones simples

---

## 1.4 Tabla Comparativa Completa

| Aspecto | MySQL (SQL) | MongoDB (NoSQL) |
|---------|-------------|-----------------|
| **Tipo** | Relacional | No Relacional |
| **Estructura** | Tablas, Filas, Columnas | Colecciones, Documentos |
| **Esquema** | Estricto y obligatorio | Flexible y opcional |
| **Relaciones** | JOINs nativos | Referencias o embebido |
| **Escalabilidad** | Vertical | Horizontal |
| **ACID** | Completo | Parcial (desde v4.0) |
| **BASE** | No aplica | Sí (modelo de consistencia) |
| **Lenguaje** | SQL (estándar) | MongoDB Query Language |
| **Integridad Referencial** | Nativa (FOREIGN KEY) | Manual (en aplicación) |
| **Normalización** | Requerida | Permite desnormalización |
| **Rendimiento** | Predecible, consistente | Muy rápido para lecturas |
| **Caso de Uso** | Datos estructurados, relaciones complejas | Datos flexibles, alto volumen |
| **Conexión Node.js** | `mysql2` (pool) | `mongoose` (ODM) |
| **Validación** | A nivel de BD | A nivel de aplicación (Mongoose) |

---

## 1.5 ¿Cuándo usar cada una?

### Usar MySQL cuando:
- ✅ **Integridad de datos crítica**: Sistemas bancarios, contabilidad, reservas
- ✅ **Relaciones complejas**: Múltiples tablas relacionadas con JOINs frecuentes
- ✅ **Transacciones ACID**: Operaciones que deben ser atómicas y consistentes
- ✅ **Datos estructurados**: Esquema estable y predecible
- ✅ **Consultas complejas**: Necesitas JOINs, GROUP BY, funciones de agregación

### Usar MongoDB cuando:
- ✅ **Alto volumen de datos**: Big Data, logs, IoT, redes sociales
- ✅ **Esquema flexible**: Estructura de datos variable o que cambia frecuentemente
- ✅ **Escalabilidad horizontal**: Necesitas distribuir datos en múltiples servidores
- ✅ **Lecturas rápidas**: Optimizado para operaciones de lectura masivas
- ✅ **Datos semi-estructurados**: JSON, documentos, contenido variado
- ✅ **Desarrollo ágil**: Startups, prototipos, aplicaciones que evolucionan rápido

### Casos Híbridos
Muchas aplicaciones modernas usan **ambas**:
- **MySQL**: Para datos estructurados y críticos (usuarios, pedidos, pagos)
- **MongoDB**: Para datos flexibles y de alto volumen (logs, analytics, contenido)

---

## 3. BASE: Modelo de Consistencia

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

## 4. Conceptos Fundamentales

### Base de Datos (Database)
Contenedor de colecciones. Se crea automáticamente al insertar el primer documento.

```javascript
use mi_base_de_datos  // Cambiar a una base de datos
```

### Colección (Collection)
Un grupo de documentos. Similar a una "tabla" pero sin esquema fijo.

**⚠️ Diferencia crucial con MySQL:** En MongoDB, **NO necesitas crear la colección explícitamente**. La colección se crea automáticamente al insertar el primer documento con `insertOne()` o `insertMany()`.

```javascript
// NO necesitas hacer CREATE COLLECTION
// Simplemente insertas y la colección se crea automáticamente:
db.usuarios.insertOne({ nombre: "Juan" });  // La colección 'usuarios' se crea aquí

// Acceder a la colección
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

## 7. Expresiones de Consulta

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

## 8. Tipos de Datos

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

#### Enum
Restringe los valores a un conjunto predefinido de opciones.

```javascript
rol: {
  type: String,
  enum: ['admin', 'user', 'guest'],
  default: 'user'
}
```

---

## 9. Esquemas y Validaciones con Mongoose

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

## 10. Modificadores de Campos

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

## 11. Validators y Custom Validators

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

## 12. Virtuals (Valores Virtuales)

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
