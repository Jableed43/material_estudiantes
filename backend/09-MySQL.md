# Master Guide: MySQL y Bases de Datos Relacionales

## 📑 Índice
1. [Fundamentos: ¿Qué es una Base de Datos Relacional?](#1-fundamentos-qué-es-una-base-de-datos-relacional)
2. [ACID: Propiedades de las Transacciones](#2-acid-propiedades-de-las-transacciones)
3. [Claves y Relaciones](#3-claves-y-relaciones)
4. [Tipos de Datos y Restricciones](#4-tipos-de-datos-y-restricciones)
5. [Normalización](#5-normalización)
6. [DDL - Data Definition Language](#6-ddl---data-definition-language)
7. [DML - Data Manipulation Language](#7-dml---data-manipulation-language)
8. [DCL - Data Control Language](#8-dcl---data-control-language)
9. [JOINs - Uniones entre Tablas](#9-joins---uniones-entre-tablas)
10. [Funciones de Agregación](#10-funciones-de-agregación)
11. [Triggers, Stored Procedures y Functions](#11-triggers-stored-procedures-y-functions)
12. [Conexión desde Node.js](#12-conexión-desde-nodejs)

---

## 1. Fundamentos: ¿Qué es una Base de Datos Relacional?

### 📚 Analogía: La Biblioteca Organizada

Imagina una biblioteca bien organizada:
- **Base de Datos**: La biblioteca completa
- **Tablas**: Diferentes secciones (libros, autores, préstamos)
- **Filas**: Cada libro, autor o préstamo individual
- **Columnas**: Características (título, autor, fecha)
- **Relaciones**: Los libros están conectados a sus autores

**La clave**: En lugar de repetir la información del autor en cada libro, tienes una tabla de autores y los libros solo referencian al autor. Eso evita duplicación y mantiene todo organizado.

### 🏢 Analogía: El Sistema de Archivos de una Empresa

Piensa en el sistema de archivos de una empresa:
- **Base de Datos**: El archivo completo de la empresa
- **Tablas**: Diferentes carpetas (empleados, departamentos, proyectos)
- **Relaciones**: Los empleados pertenecen a departamentos, trabajan en proyectos

**Ventaja**: Si cambia el nombre de un departamento, solo lo cambias en un lugar y todos los empleados se actualizan automáticamente.

### 🗂️ Analogía: El Organizador de Contactos

Tu agenda de contactos:
- **Tabla de Contactos**: Nombres, teléfonos, emails
- **Tabla de Empresas**: Empresas donde trabajan
- **Relación**: Cada contacto está vinculado a una empresa

**Beneficio**: Si una empresa cambia de dirección, solo actualizas un registro y todos los contactos de esa empresa se ven actualizados.

Una **Base de Datos Relacional** es una forma de organizar datos en **tablas** que están conectadas entre sí mediante relaciones. La clave del modelo relacional es que evita la repetición de datos, permitiendo una gestión más eficiente y segura.

### Estructura Básica

*   **Tabla (o Entidad)**: Una colección de datos sobre un tema específico. Se compone de filas y columnas. Por ejemplo, una tabla de `autores` o una tabla de `libros`.
*   **Fila (o Registro)**: Una única entrada en una tabla. En la tabla `autores`, una fila representaría a un autor específico.
*   **Columna (o Atributo)**: Un campo de datos que describe una característica de la entidad. En la tabla `autores`, las columnas serían `nombre` o `nacionalidad`.
*   **Base de Datos (Database)**: Contenedor que agrupa tablas relacionadas.

### Características de las Bases de Datos Relacionales

- ✅ **Esquema Estricto**: La estructura debe definirse antes de insertar datos
- ✅ **Integridad Referencial**: Las relaciones entre tablas se validan automáticamente
- ✅ **ACID**: Garantiza Atomicidad, Consistencia, Aislamiento y Durabilidad
- ✅ **Normalización**: Permite eliminar redundancia de datos
- ✅ **SQL**: Lenguaje estándar para consultas y operaciones

---

## 2. ACID: Propiedades de las Transacciones

**ACID** es un acrónimo que describe las propiedades fundamentales de las **transacciones** en bases de datos relacionales. Estas propiedades garantizan la confiabilidad y consistencia de los datos, incluso en entornos concurrentes y en caso de fallos del sistema.

### ¿Qué es una Transacción? (Analogía del Mundo Real)

### 💰 Analogía: La Transferencia Bancaria

Imagina que transfieres dinero de tu cuenta a la de un amigo:
- **Operación 1**: Descontar $1000 de tu cuenta
- **Operación 2**: Agregar $1000 a la cuenta de tu amigo

**Sin transacciones (PROBLEMA)**:
- Si falla la segunda operación, tu dinero se descontó pero no llegó a tu amigo
- El dinero se "pierde" en el proceso ❌

**Con transacciones (SOLUCIÓN)**:
- Si falla cualquier operación, TODO se revierte
- O ambas operaciones funcionan, o ninguna funciona
- El dinero nunca se "pierde" ✅

### 🛒 Analogía: La Compra en el Supermercado

Cuando compras en el supermercado:
- **Operación 1**: Escanean todos los productos
- **Operación 2**: Cobran de tu tarjeta
- **Operación 3**: Actualizan el inventario

**Si falla el cobro**: Todo se revierte - no se actualiza el inventario, no se cobra, no se registra la venta. Es "todo o nada".

### 🎫 Analogía: La Reserva de Vuelo

Cuando reservas un vuelo:
- **Operación 1**: Reservar el asiento
- **Operación 2**: Cobrar el pago
- **Operación 3**: Enviar confirmación

**Si falla el pago**: Se libera el asiento, no se cobra, no se envía confirmación. Todo se revierte.

Una **transacción** es un conjunto de operaciones de base de datos que se realizan como una sola unidad lógica e indivisible. Todas las operaciones deben completarse exitosamente, o ninguna de ellas debe aplicarse.

**Ejemplo de Transacción:**
```sql
START TRANSACTION;

-- Transferir dinero de una cuenta a otra
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 1000 WHERE id = 2;

-- Si todo está bien, confirmar
COMMIT;

-- Si hay error, revertir todo
-- ROLLBACK;
```

### A - Atomicity (Atomicidad)

**Definición**: Una transacción se lleva a cabo **completamente o no se lleva a cabo en absoluto**. No hay estados intermedios.

**Características:**
- ✅ Todas las operaciones de la transacción deben completarse exitosamente
- ✅ Si alguna operación falla, **todas** las operaciones se revierten
- ✅ No puede haber "estados a medias"
- ✅ Es "todo o nada"

**Ejemplo:**
```sql
START TRANSACTION;

-- Operación 1: Descontar dinero
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;

-- Operación 2: Acreditar dinero
UPDATE cuentas SET saldo = saldo + 1000 WHERE id = 2;

-- Si la segunda operación falla, la primera también se revierte
-- No puede quedar el dinero "en el aire"
COMMIT;
```

**Sin Atomicidad (Problema):**
- Si falla la segunda operación, el dinero se descontó pero no se acreditó
- El dinero se "pierde" en el proceso
- Estado inconsistente

**Con Atomicidad (Solución):**
- Si falla cualquier operación, todo se revierte
- El dinero nunca se "pierde"
- Estado siempre consistente

### C - Consistency (Consistencia)

**Definición**: La base de datos permanece en un **estado consistente antes y después** de la transacción. Todas las reglas de integridad se mantienen.

**Características:**
- ✅ Las restricciones (constraints) siempre se cumplen
- ✅ Las claves foráneas siempre son válidas
- ✅ Los datos siempre respetan las reglas de negocio
- ✅ No se pueden violar las reglas de integridad

**Ejemplo:**
```sql
START TRANSACTION;

-- Insertar pedido
INSERT INTO pedidos (usuario_id, total) VALUES (1, 100.00);

-- Insertar items del pedido
INSERT INTO items_pedido (pedido_id, producto_id, cantidad) 
VALUES (LAST_INSERT_ID(), 5, 2);

-- Si el usuario_id no existe, la transacción falla
-- Si el producto_id no existe, la transacción falla
-- La consistencia se mantiene

COMMIT;
```

**Reglas de Consistencia:**
- ✅ Claves primarias deben ser únicas
- ✅ Claves foráneas deben referenciar registros existentes
- ✅ Valores deben cumplir restricciones CHECK
- ✅ Campos NOT NULL no pueden estar vacíos

### I - Isolation (Aislamiento)

**Definición**: Las operaciones de una transacción **no son visibles para otras transacciones** hasta que se completen (COMMIT).

**Características:**
- ✅ Cada transacción ve una "instantánea" consistente de los datos
- ✅ Las transacciones concurrentes no interfieren entre sí
- ✅ Evita problemas de lectura/escritura simultánea
- ✅ Diferentes niveles de aislamiento (READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE)

**Problema sin Aislamiento (Dirty Read):**
```sql
-- Transacción 1
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;
-- Aún no hace COMMIT

-- Transacción 2 (lee datos no confirmados)
SELECT saldo FROM cuentas WHERE id = 1;
-- Lee el saldo modificado, pero la transacción 1 puede hacer ROLLBACK
```

**Solución con Aislamiento:**
```sql
-- Transacción 1
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;
-- Otros usuarios no ven este cambio hasta COMMIT

-- Transacción 2
SELECT saldo FROM cuentas WHERE id = 1;
-- Lee el saldo original (antes de la transacción 1)
-- Solo verá el cambio después de que la transacción 1 haga COMMIT
```

**Niveles de Aislamiento en MySQL:**

1. **READ UNCOMMITTED** (Menor aislamiento)
   - Permite leer datos no confirmados
   - Puede haber "dirty reads"

2. **READ COMMITTED**
   - Solo lee datos confirmados
   - Evita "dirty reads"
   - Puede tener "non-repeatable reads"

3. **REPEATABLE READ** (Por defecto en MySQL)
   - Garantiza lecturas consistentes durante la transacción
   - Evita "non-repeatable reads"
   - Puede tener "phantom reads"

4. **SERIALIZABLE** (Mayor aislamiento)
   - Transacciones completamente aisladas
   - Evita todos los problemas de concurrencia
   - Menor rendimiento

**Configurar Nivel de Aislamiento:**
```sql
-- Ver nivel actual
SELECT @@transaction_isolation;

-- Cambiar nivel
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### D - Durability (Durabilidad)

**Definición**: Los cambios realizados por una transacción **confirmada son permanentes**, incluso en caso de falla del sistema.

**Características:**
- ✅ Una vez confirmada (COMMIT), los cambios son permanentes
- ✅ Los datos se escriben en disco (no solo en memoria)
- ✅ Si el sistema falla, los datos confirmados no se pierden
- ✅ Los cambios sobreviven a reinicios, cortes de energía, etc.

**Ejemplo:**
```sql
START TRANSACTION;

UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 1000 WHERE id = 2;

COMMIT;  -- Los cambios se escriben en disco

-- Si el servidor se apaga ahora, los cambios están guardados
-- Al reiniciar, los cambios siguen ahí
```

**Mecanismos de Durabilidad:**
- ✅ **Write-Ahead Logging (WAL)**: Los cambios se escriben en un log antes de confirmarse
- ✅ **Checkpoints**: Puntos de control que aseguran que los datos en memoria se escriban en disco
- ✅ **Redundancia**: Múltiples copias de los datos en diferentes ubicaciones

### Ejemplo Completo de Transacción ACID

```sql
START TRANSACTION;

-- Verificar saldo suficiente (Consistency)
SELECT saldo INTO @saldo_actual FROM cuentas WHERE id = 1;
IF @saldo_actual < 1000 THEN
    ROLLBACK;  -- No hay suficiente saldo
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Saldo insuficiente';
END IF;

-- Descontar de cuenta origen (Atomicity)
UPDATE cuentas SET saldo = saldo - 1000 WHERE id = 1;

-- Acreditar en cuenta destino (Atomicity)
UPDATE cuentas SET saldo = saldo + 1000 WHERE id = 2;

-- Registrar la transacción (Consistency)
INSERT INTO transacciones (origen, destino, monto, fecha)
VALUES (1, 2, 1000, NOW());

-- Si todo está bien, confirmar (Durability)
COMMIT;

-- Si hay error, revertir todo (Atomicity)
-- ROLLBACK;
```

**Garantías ACID en este ejemplo:**
- ✅ **Atomicity**: Si falla cualquier operación, todo se revierte
- ✅ **Consistency**: Se verifica el saldo antes de transferir
- ✅ **Isolation**: Otras transacciones no ven los cambios hasta COMMIT
- ✅ **Durability**: Una vez confirmado, los cambios son permanentes

### Ventajas de ACID

- ✅ **Confiabilidad**: Los datos siempre están en un estado válido
- ✅ **Integridad**: Las reglas de negocio siempre se cumplen
- ✅ **Seguridad**: Los cambios no se pierden
- ✅ **Predecibilidad**: Comportamiento consistente y predecible

### Cuándo es Crítico ACID

- ✅ **Sistemas Financieros**: Bancos, transferencias, pagos
- ✅ **Reservas**: Hoteles, vuelos, eventos
- ✅ **Inventarios**: Control de stock crítico
- ✅ **Sistemas de Facturación**: Facturas, recibos
- ✅ **Cualquier sistema donde la integridad de datos es crítica**

---

## 3. Claves y Relaciones

Las claves son los identificadores que nos permiten conectar tablas y garantizar la integridad de los datos.

### Claves (Keys)

#### 1. Clave Primaria (`Primary Key - PK`)
Una columna (o conjunto de columnas) que identifica de forma **única** cada fila de una tabla.

**Características:**
- ✅ **Única**: No puede haber dos filas con el mismo valor
- ✅ **No Nula (`NOT NULL`)**: No puede estar vacía
- ✅ **Inmutable**: Idealmente no debería cambiar

**Ejemplo:**
```sql
CREATE TABLE autores (
    id_autor INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL
);
```

#### 2. Clave Foránea (`Foreign Key - FK`)
Una columna en una tabla que hace referencia a la clave primaria de otra tabla. Actúa como un "puente" o "enlace".

**Características:**
- ✅ Garantiza la integridad referencial
- ✅ Asegura que las relaciones entre tablas sean válidas
- ✅ Puede tener acciones en cascada (`ON DELETE`, `ON UPDATE`)

**Ejemplo:**
```sql
CREATE TABLE libros (
    id_libro INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255) NOT NULL,
    id_autor INT NOT NULL,
    FOREIGN KEY (id_autor) REFERENCES autores(id_autor)
        ON DELETE RESTRICT
);
```

#### 3. Clave Única (`UNIQUE`)
Garantiza que los valores en una columna sean únicos (permite NULL).

**Ejemplo:**
```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

### Cardinalidad

Describe la cantidad de instancias de una entidad que pueden estar asociadas con las instancias de otra.

#### 1. Relación Uno a Uno (1:1)
Una instancia de la entidad A está relacionada con una y solo una instancia de la entidad B.

**Ejemplo:** `persona` y `pasaporte` - cada persona tiene un solo pasaporte.

```sql
CREATE TABLE personas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);

CREATE TABLE pasaportes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    numero VARCHAR(50) UNIQUE,
    persona_id INT UNIQUE,
    FOREIGN KEY (persona_id) REFERENCES personas(id)
);
```

#### 2. Relación Uno a Muchos (1:N)
Una instancia de A puede estar relacionada con múltiples instancias de B, pero B solo puede estar relacionada con una A.

**Ejemplo:** `autor` y `libros` - un autor puede tener muchos libros, pero cada libro tiene un solo autor.

```sql
CREATE TABLE autores (
    id_autor INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);

CREATE TABLE libros (
    id_libro INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255),
    id_autor INT,
    FOREIGN KEY (id_autor) REFERENCES autores(id_autor)
);
```

#### 3. Relación Muchos a Muchos (N:M)
Múltiples instancias de A pueden estar relacionadas con múltiples instancias de B.

**Ejemplo:** `libros` y `lectores` - un libro puede ser leído por muchos lectores, y un lector puede leer muchos libros.

**Solución:** Se utiliza una **tabla intermedia o pivote** que contiene las claves foráneas de ambas tablas.

```sql
CREATE TABLE libros (
    id_libro INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255)
);

CREATE TABLE lectores (
    id_lector INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);

-- Tabla intermedia
CREATE TABLE prestamos (
    id_prestamo INT PRIMARY KEY AUTO_INCREMENT,
    id_libro INT,
    id_lector INT,
    fecha_prestamo DATE,
    FOREIGN KEY (id_libro) REFERENCES libros(id_libro),
    FOREIGN KEY (id_lector) REFERENCES lectores(id_lector)
);
```

### Tipos de Relaciones en Diagramas (ERD)

#### Relación Identifying (Línea Continua)
La clave foránea (FK) **también es parte de la clave primaria (PK)** en la tabla hija. Un registro hijo no puede existir sin el padre. Se usa para entidades débiles.

#### Relación Non-Identifying (Línea Punteada)
La clave foránea (FK) **no es parte de la clave primaria** en la tabla hija. Es la relación más común. El registro hijo puede ser identificado independientemente.

---

## 3. Tipos de Datos y Restricciones

Al definir columnas, debemos asignar un tipo de dato estricto.

### Tipos de Datos Comunes

| Categoría | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- |
| **Números** | `INT` | Enteros. | IDs, edad |
| | `DECIMAL(p, s)` | Exactos con decimales. `p`=total dígitos, `s`=decimales | Precios (`DECIMAL(10,2)`) |
| | `FLOAT` / `DOUBLE` | Decimales aproximados (punto flotante) | Cálculos científicos |
| | `TINYINT` | Enteros pequeños (-128 a 127) | Valores booleanos |
| | `BIGINT` | Enteros grandes | IDs muy grandes |
| **Texto** | `VARCHAR(n)` | Texto variable hasta *n* caracteres | Nombre, email |
| | `CHAR(n)` | Texto de longitud fija (rellena con espacios) | Códigos de país (`US`, `AR`) |
| | `TEXT` | Texto largo | Descripciones, reseñas |
| | `ENUM` | Conjunto de valores predefinidos | `'activo'`, `'inactivo'` |
| **Fecha** | `DATE` | Solo fecha | `'2025-08-12'` |
| | `DATETIME` | Fecha y hora | `'2025-08-12 10:30:00'` |
| | `TIMESTAMP` | Fecha y hora con zona horaria | Timestamps automáticos |
| | `YEAR` | Solo año | `2025` |
| | `TIME` | Solo hora | `'10:30:00'` |
| **Lógico** | `BOOLEAN` | Verdadero/Falso (usualmente `TINYINT(1)`) | `TRUE`, `FALSE` |
| **Binario** | `BLOB` | Datos binarios grandes | Imágenes, archivos |

### Restricciones (Constraints)

#### PRIMARY KEY
Identificador único y no nulo.

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);
```

#### FOREIGN KEY
Enlace a otra tabla con acciones en cascada.

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    usuario_id INT,
    FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
        ON DELETE CASCADE    -- Si se borra el usuario, se borran sus pedidos
        ON UPDATE CASCADE    -- Si se actualiza el ID, se actualiza en pedidos
);
```

**Acciones en Cascada (`ON DELETE` / `ON UPDATE`):**
- `CASCADE`: Si borras/actualizas el padre, se borran/actualizan los hijos
- `RESTRICT`: No permite borrar/actualizar el padre si tiene hijos
- `SET NULL`: Si borras/actualizas el padre, el campo FK del hijo queda en `NULL`
- `NO ACTION`: Similar a RESTRICT (comportamiento por defecto)

#### NOT NULL
No permite valores vacíos.

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL
);
```

#### UNIQUE
Valores únicos en la columna (permite nulos si no es PK).

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

#### DEFAULT
Valor por defecto si no se especifica.

```sql
CREATE TABLE productos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    precio DECIMAL(10,2) DEFAULT 0.00,
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### CHECK
Condición que debe cumplir el dato.

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    edad INT,
    CHECK (edad >= 18 AND edad <= 120)
);
```

#### AUTO_INCREMENT
Incremento automático (solo para claves primarias numéricas).

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);
```

---

## 4. Normalización

Proceso para organizar la base de datos reduciendo redundancia y mejorando la integridad de los datos.

### Problema de Redundancia

Si en `libros` guardamos el nombre del autor directamente:
- Al corregir el nombre del autor, tendríamos que editar todos sus libros
- Mayor riesgo de inconsistencias
- Mayor uso de almacenamiento

### Solución: Normalización

Crear tabla `autores` y referenciarla por ID:

```sql
-- ❌ MAL: Redundancia
CREATE TABLE libros (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255),
    autor_nombre VARCHAR(100)  -- Redundante
);

-- ✅ BIEN: Normalizado
CREATE TABLE autores (
    id_autor INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100)
);

CREATE TABLE libros (
    id_libro INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(255),
    id_autor INT,
    FOREIGN KEY (id_autor) REFERENCES autores(id_autor)
);
```

### Formas Normales

1. **Primera Forma Normal (1NF)**: Cada columna debe contener valores atómicos (no listas)
2. **Segunda Forma Normal (2NF)**: Debe estar en 1NF y todos los atributos no clave deben depender completamente de la clave primaria
3. **Tercera Forma Normal (3NF)**: Debe estar en 2NF y no debe haber dependencias transitivas

---

## 5. DDL - Data Definition Language

DDL (Lenguaje de Definición de Datos) se usa para crear, modificar y eliminar estructuras de base de datos.

### CREATE DATABASE

Crear una nueva base de datos.

```sql
-- Crear base de datos
CREATE DATABASE biblioteca;

-- Crear con especificaciones
CREATE DATABASE biblioteca
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

-- Usar una base de datos
USE biblioteca;
```

### CREATE TABLE

Crear una nueva tabla.

```sql
CREATE TABLE autores (
    id_autor INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50),
    fecha_nacimiento DATE
);
```

### ALTER TABLE

Modificar la estructura de una tabla existente.

#### Agregar Columna
```sql
ALTER TABLE autores
ADD COLUMN email VARCHAR(255);
```

#### Modificar Columna
```sql
ALTER TABLE autores
MODIFY COLUMN nombre VARCHAR(200);
```

#### Eliminar Columna
```sql
ALTER TABLE autores
DROP COLUMN nacionalidad;
```

#### Agregar Restricción
```sql
ALTER TABLE libros
ADD CONSTRAINT fk_autor
FOREIGN KEY (id_autor) REFERENCES autores(id_autor);
```

#### Eliminar Restricción
```sql
ALTER TABLE libros
DROP FOREIGN KEY fk_autor;
```

### DROP TABLE

Eliminar una tabla.

```sql
-- Eliminar tabla
DROP TABLE autores;

-- Eliminar si existe
DROP TABLE IF EXISTS autores;
```

### DROP DATABASE

Eliminar una base de datos.

```sql
-- Eliminar base de datos
DROP DATABASE biblioteca;

-- Eliminar si existe
DROP DATABASE IF EXISTS biblioteca;
```

### TRUNCATE TABLE

Vaciar una tabla (elimina todos los registros pero mantiene la estructura).

```sql
TRUNCATE TABLE autores;
```

**Diferencia con DELETE:**
- `TRUNCATE` es más rápido
- `TRUNCATE` reinicia AUTO_INCREMENT
- `TRUNCATE` no puede tener WHERE
- `TRUNCATE` no puede revertirse con ROLLBACK

### Índices

Los índices mejoran el rendimiento de las consultas.

#### Crear Índice
```sql
-- Índice simple
CREATE INDEX idx_nombre ON autores(nombre);

-- Índice único
CREATE UNIQUE INDEX idx_email ON usuarios(email);

-- Índice compuesto
CREATE INDEX idx_nombre_apellido ON autores(nombre, apellido);
```

#### Eliminar Índice
```sql
DROP INDEX idx_nombre ON autores;
```

---

## 6. DML - Data Manipulation Language

DML (Lenguaje de Manipulación de Datos) se usa para insertar, consultar, actualizar y eliminar datos.

### INSERT

Insertar nuevos registros.

#### Insertar un Registro
```sql
INSERT INTO autores (nombre, apellido, nacionalidad)
VALUES ('Gabriel', 'García Márquez', 'Colombiana');
```

#### Insertar Múltiples Registros
```sql
INSERT INTO autores (nombre, apellido, nacionalidad) VALUES
('Gabriel', 'García Márquez', 'Colombiana'),
('J.R.R.', 'Tolkien', 'Británica'),
('George', 'Orwell', 'Británica');
```

#### Insertar con SELECT
```sql
INSERT INTO autores_backup (nombre, apellido)
SELECT nombre, apellido FROM autores;
```

### SELECT

Consultar datos de una o más tablas.

#### SELECT Básico
```sql
-- Seleccionar todas las columnas
SELECT * FROM autores;

-- Seleccionar columnas específicas
SELECT nombre, apellido FROM autores;

-- Seleccionar con alias
SELECT nombre AS nombre_autor, apellido AS apellido_autor FROM autores;
```

#### WHERE - Filtrar Filas

**Operadores de Comparación:**
```sql
-- Igual
SELECT * FROM autores WHERE nacionalidad = 'Colombiana';

-- Diferente
SELECT * FROM autores WHERE nacionalidad != 'Colombiana';
-- O
SELECT * FROM autores WHERE nacionalidad <> 'Colombiana';

-- Mayor que / Menor que
SELECT * FROM libros WHERE año_publicacion > 2000;
SELECT * FROM libros WHERE año_publicacion < 1950;
SELECT * FROM libros WHERE año_publicacion >= 2000;
SELECT * FROM libros WHERE año_publicacion <= 1950;
```

**Operadores Lógicos:**
```sql
-- AND
SELECT * FROM autores 
WHERE nacionalidad = 'Colombiana' AND nombre LIKE 'G%';

-- OR
SELECT * FROM autores 
WHERE nacionalidad = 'Colombiana' OR nacionalidad = 'Británica';

-- NOT
SELECT * FROM autores 
WHERE NOT nacionalidad = 'Colombiana';
```

**LIKE - Patrones:**
```sql
-- Empieza con 'G'
SELECT * FROM autores WHERE nombre LIKE 'G%';

-- Termina con 'ez'
SELECT * FROM autores WHERE apellido LIKE '%ez';

-- Contiene 'arcía'
SELECT * FROM autores WHERE nombre LIKE '%arcía%';

-- Un solo carácter (guion bajo)
SELECT * FROM autores WHERE nombre LIKE 'G_';
```

**BETWEEN - Rangos:**
```sql
-- Entre dos valores (inclusive)
SELECT * FROM libros WHERE año_publicacion BETWEEN 1950 AND 2000;

-- Equivale a:
SELECT * FROM libros 
WHERE año_publicacion >= 1950 AND año_publicacion <= 2000;
```

**IN - Listas:**
```sql
-- Valores en una lista
SELECT * FROM autores 
WHERE nacionalidad IN ('Colombiana', 'Británica', 'Argentina');

-- NOT IN
SELECT * FROM autores 
WHERE nacionalidad NOT IN ('Colombiana', 'Británica');
```

**IS NULL / IS NOT NULL:**
```sql
-- Valores nulos
SELECT * FROM autores WHERE nacionalidad IS NULL;

-- Valores no nulos
SELECT * FROM autores WHERE nacionalidad IS NOT NULL;
```

#### DISTINCT - Valores Únicos
```sql
-- Valores únicos de una columna
SELECT DISTINCT nacionalidad FROM autores;

-- Múltiples columnas
SELECT DISTINCT nombre, apellido FROM autores;
```

#### ORDER BY - Ordenar
```sql
-- Orden ascendente (por defecto)
SELECT * FROM autores ORDER BY nombre ASC;

-- Orden descendente
SELECT * FROM autores ORDER BY nombre DESC;

-- Múltiples columnas
SELECT * FROM autores ORDER BY nacionalidad ASC, nombre ASC;
```

#### LIMIT / OFFSET - Paginación
```sql
-- Limitar resultados
SELECT * FROM autores LIMIT 10;

-- Con offset (paginación)
SELECT * FROM autores LIMIT 10 OFFSET 20;  -- Registros 21-30

-- Sintaxis alternativa
SELECT * FROM autores LIMIT 20, 10;  -- OFFSET 20, LIMIT 10
```

#### GROUP BY - Agrupar
```sql
-- Agrupar por nacionalidad
SELECT nacionalidad, COUNT(*) as total
FROM autores
GROUP BY nacionalidad;
```

#### HAVING - Filtrar Grupos
```sql
-- Filtrar grupos después de GROUP BY
SELECT nacionalidad, COUNT(*) as total
FROM autores
GROUP BY nacionalidad
HAVING total > 2;  -- Solo nacionalidades con más de 2 autores
```

**Diferencia WHERE vs HAVING:**
- `WHERE` filtra **filas individuales** antes de agrupar
- `HAVING` filtra **grupos** después de agrupar

```sql
-- WHERE filtra antes de agrupar
SELECT nacionalidad, COUNT(*) as total
FROM autores
WHERE nombre LIKE 'G%'  -- Solo autores que empiezan con G
GROUP BY nacionalidad;

-- HAVING filtra después de agrupar
SELECT nacionalidad, COUNT(*) as total
FROM autores
GROUP BY nacionalidad
HAVING total > 2;  -- Solo grupos con más de 2 autores
```

### UPDATE

Actualizar registros existentes.

```sql
-- Actualizar un registro
UPDATE autores
SET nacionalidad = 'Española'
WHERE id_autor = 1;

-- Actualizar múltiples columnas
UPDATE autores
SET nombre = 'Gabriel José', apellido = 'García Márquez'
WHERE id_autor = 1;

-- Actualizar múltiples registros
UPDATE libros
SET año_publicacion = 2000
WHERE año_publicacion IS NULL;
```

**⚠️ Importante:** Siempre usar `WHERE` en UPDATE para evitar actualizar todos los registros.

### DELETE

Eliminar registros.

```sql
-- Eliminar un registro
DELETE FROM autores WHERE id_autor = 1;

-- Eliminar múltiples registros
DELETE FROM libros WHERE año_publicacion < 1900;

-- Eliminar todos los registros (¡CUIDADO!)
DELETE FROM autores;
```

**⚠️ Importante:** Siempre usar `WHERE` en DELETE para evitar eliminar todos los registros.

---

## 7. DCL - Data Control Language

DCL (Lenguaje de Control de Datos) se usa para gestionar permisos y acceso a la base de datos.

### GRANT - Otorgar Permisos

```sql
-- Otorgar todos los permisos en una base de datos
GRANT ALL PRIVILEGES ON biblioteca.* TO 'usuario'@'localhost';

-- Otorgar permisos específicos
GRANT SELECT, INSERT, UPDATE ON biblioteca.* TO 'usuario'@'localhost';

-- Otorgar permisos en una tabla específica
GRANT SELECT, INSERT ON biblioteca.autores TO 'usuario'@'localhost';

-- Crear usuario y otorgar permisos
CREATE USER 'usuario'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON biblioteca.* TO 'usuario'@'localhost';
FLUSH PRIVILEGES;
```

### REVOKE - Revocar Permisos

```sql
-- Revocar todos los permisos
REVOKE ALL PRIVILEGES ON biblioteca.* FROM 'usuario'@'localhost';

-- Revocar permisos específicos
REVOKE INSERT, UPDATE ON biblioteca.* FROM 'usuario'@'localhost';
```

### Permisos Comunes

- `SELECT`: Leer datos
- `INSERT`: Insertar datos
- `UPDATE`: Actualizar datos
- `DELETE`: Eliminar datos
- `CREATE`: Crear tablas/bases de datos
- `DROP`: Eliminar tablas/bases de datos
- `ALTER`: Modificar estructura
- `INDEX`: Crear/eliminar índices
- `ALL PRIVILEGES`: Todos los permisos

---

## 8. JOINs - Uniones entre Tablas

Los JOINs permiten combinar datos de múltiples tablas basándose en relaciones.

### INNER JOIN

Devuelve solo los registros que tienen coincidencias en **ambas** tablas.

```sql
-- Ver nombres de autores y sus libros
SELECT autores.nombre, autores.apellido, libros.titulo
FROM autores
INNER JOIN libros ON autores.id_autor = libros.id_autor;
```

**Sintaxis alternativa:**
```sql
SELECT autores.nombre, libros.titulo
FROM autores, libros
WHERE autores.id_autor = libros.id_autor;
```

### LEFT JOIN (LEFT OUTER JOIN)

Devuelve **todos** los registros de la tabla izquierda y los registros coincidentes de la tabla derecha. Si no hay coincidencias, se muestran valores NULL.

```sql
-- Todos los autores, incluso si NO tienen libros
SELECT autores.nombre, autores.apellido, libros.titulo
FROM autores
LEFT JOIN libros ON autores.id_autor = libros.id_autor;
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Devuelve **todos** los registros de la tabla derecha y los registros coincidentes de la tabla izquierda. Si no hay coincidencias, se muestran valores NULL.

```sql
-- Todas las materias, incluso sin docentes
SELECT materias.nombreMateria, docentes.nombre
FROM materias
RIGHT JOIN docentes ON materias.id_docente = docentes.id_docente;
```

### FULL OUTER JOIN

Devuelve todos los registros cuando hay una coincidencia en una de las tablas. Incluye registros que no tienen coincidencias en ambas tablas.

**Nota:** MySQL no soporta FULL OUTER JOIN directamente. Se puede simular con UNION:

```sql
-- Simular FULL OUTER JOIN
SELECT autores.nombre, libros.titulo
FROM autores
LEFT JOIN libros ON autores.id_autor = libros.id_autor
UNION
SELECT autores.nombre, libros.titulo
FROM autores
RIGHT JOIN libros ON autores.id_autor = libros.id_autor;
```

### CROSS JOIN

Realiza un producto cartesiano entre las filas de dos tablas, combinando cada fila de la primera tabla con cada fila de la segunda.

```sql
-- Todas las combinaciones posibles
SELECT autores.nombre, generos.nombre
FROM autores
CROSS JOIN generos;
```

**⚠️ Cuidado:** CROSS JOIN puede generar muchos resultados (filas tabla1 × filas tabla2).

### Múltiples JOINs

```sql
-- JOINs encadenados
SELECT 
    alumnos.nombre,
    materias.nombreMateria,
    docentes.nombre AS docente
FROM alumnos
INNER JOIN asignaciones ON alumnos.id_alumno = asignaciones.id_alumno
INNER JOIN materias ON asignaciones.id_materia = materias.id_materia
LEFT JOIN docentes ON materias.id_docente = docentes.id_docente;
```

### Alias de Tablas

Usar alias para hacer las consultas más legibles:

```sql
SELECT a.nombre, a.apellido, l.titulo
FROM autores AS a
INNER JOIN libros AS l ON a.id_autor = l.id_autor;
```

O sin `AS`:
```sql
SELECT a.nombre, a.apellido, l.titulo
FROM autores a
INNER JOIN libros l ON a.id_autor = l.id_autor;
```

---

## 9. Funciones de Agregación

Las funciones de agregación realizan cálculos sobre un conjunto de filas y devuelven un único valor.

### COUNT

Cuenta el número de filas.

```sql
-- Contar todas las filas
SELECT COUNT(*) FROM autores;

-- Contar filas no nulas de una columna
SELECT COUNT(nacionalidad) FROM autores;

-- Contar con DISTINCT
SELECT COUNT(DISTINCT nacionalidad) FROM autores;
```

### SUM

Suma los valores de una columna numérica.

```sql
-- Sumar valores
SELECT SUM(precio) FROM productos;

-- Con GROUP BY
SELECT categoria, SUM(precio) as total
FROM productos
GROUP BY categoria;
```

### AVG

Calcula el promedio.

```sql
-- Promedio
SELECT AVG(precio) FROM productos;

-- Con GROUP BY
SELECT categoria, AVG(precio) as promedio
FROM productos
GROUP BY categoria;
```

### MIN / MAX

Devuelve el valor mínimo o máximo.

```sql
-- Mínimo y máximo
SELECT MIN(precio) as precio_min, MAX(precio) as precio_max
FROM productos;

-- Con GROUP BY
SELECT categoria, MIN(precio) as min_precio, MAX(precio) as max_precio
FROM productos
GROUP BY categoria;
```

### Ejemplos Combinados

```sql
-- Estadísticas por categoría
SELECT 
    categoria,
    COUNT(*) as total_productos,
    SUM(precio) as total_ventas,
    AVG(precio) as precio_promedio,
    MIN(precio) as precio_minimo,
    MAX(precio) as precio_maximo
FROM productos
GROUP BY categoria
HAVING total_productos > 5
ORDER BY precio_promedio DESC;
```

---

## 10. Triggers, Stored Procedures y Functions

### Triggers

Los triggers son procedimientos almacenados que se ejecutan automáticamente cuando ocurre un evento específico (INSERT, UPDATE, DELETE).

#### Sintaxis Básica

```sql
DELIMITER //

CREATE TRIGGER nombre_trigger
BEFORE/AFTER INSERT/UPDATE/DELETE ON tabla
FOR EACH ROW
BEGIN
    -- Código del trigger
END;

//
DELIMITER ;
```

#### Ejemplo: Actualizar Población de País

```sql
DELIMITER //

CREATE TRIGGER UpdateCountryPopulation
AFTER INSERT ON city
FOR EACH ROW
BEGIN
    UPDATE country
    SET Population = (
        SELECT SUM(Population) 
        FROM city 
        WHERE city.CountryCode = NEW.CountryCode
    )
    WHERE country.Code = NEW.CountryCode;
END;

//
DELIMITER ;
```

**Palabras Clave:**
- `BEFORE`: Se ejecuta antes del evento
- `AFTER`: Se ejecuta después del evento
- `NEW`: Referencia al nuevo registro (INSERT, UPDATE)
- `OLD`: Referencia al registro antiguo (UPDATE, DELETE)

#### Eliminar Trigger

```sql
DROP TRIGGER IF EXISTS UpdateCountryPopulation;
```

### Stored Procedures

Los stored procedures son conjuntos de sentencias SQL almacenadas que se pueden ejecutar como una unidad.

#### Crear Stored Procedure

```sql
DELIMITER //

CREATE PROCEDURE GetAuthorsByCountry(IN country_name VARCHAR(50))
BEGIN
    SELECT * FROM autores
    WHERE nacionalidad = country_name;
END;

//
DELIMITER ;
```

#### Ejecutar Stored Procedure

```sql
CALL GetAuthorsByCountry('Colombiana');
```

#### Stored Procedure con Parámetros

```sql
DELIMITER //

CREATE PROCEDURE InsertAuthor(
    IN p_nombre VARCHAR(100),
    IN p_apellido VARCHAR(100),
    IN p_nacionalidad VARCHAR(50)
)
BEGIN
    INSERT INTO autores (nombre, apellido, nacionalidad)
    VALUES (p_nombre, p_apellido, p_nacionalidad);
END;

//
DELIMITER ;

-- Ejecutar
CALL InsertAuthor('Mario', 'Vargas Llosa', 'Peruana');
```

#### Eliminar Stored Procedure

```sql
DROP PROCEDURE IF EXISTS GetAuthorsByCountry;
```

### Functions

Las functions son procedimientos que devuelven un valor.

#### Crear Function

```sql
DELIMITER //

CREATE FUNCTION GetCountryPopulation(CountryCode CHAR(3)) 
RETURNS INT 
DETERMINISTIC
BEGIN
    DECLARE totalPopulation INT;
    SELECT SUM(city.Population) INTO totalPopulation 
    FROM city
    JOIN country ON city.CountryCode = country.Code
    WHERE country.Code = CountryCode;
    RETURN totalPopulation;
END;

//
DELIMITER ;
```

#### Usar Function

```sql
SELECT GetCountryPopulation('ARG') as poblacion_total;
```

**Características:**
- `RETURNS tipo`: Especifica el tipo de dato que retorna
- `DETERMINISTIC`: Indica que siempre devuelve el mismo resultado para los mismos parámetros
- `RETURN valor`: Devuelve el valor

#### Eliminar Function

```sql
DROP FUNCTION IF EXISTS GetCountryPopulation;
```

---

## 11. Conexión desde Node.js

### Instalación

```bash
npm install mysql2
```

### Configuración de Conexión (Connection Pool)

**Recomendado:** Usar connection pool para mejor rendimiento.

```javascript
import mysql from 'mysql2/promise';

const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'biblioteca',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

export { pool };
```

**Ventajas del Pool:**
- ✅ Reutiliza conexiones
- ✅ Mejor rendimiento
- ✅ Manejo automático de conexiones
- ✅ Evita límite de conexiones

### Ejecutar Queries

#### SELECT (Consultar)

```javascript
import { pool } from './db.js';

// Consultar todos
const [rows] = await pool.query('SELECT * FROM autores');
console.log(rows);

// Consultar con parámetros
const [autores] = await pool.query(
  'SELECT * FROM autores WHERE nacionalidad = ?',
  ['Colombiana']
);
```

#### INSERT (Insertar)

```javascript
// Insertar un registro
await pool.query(
  'INSERT INTO autores (nombre, apellido, nacionalidad) VALUES (?, ?, ?)',
  ['Gabriel', 'García Márquez', 'Colombiana']
);

// Obtener ID insertado
const [result] = await pool.query(
  'INSERT INTO autores (nombre, apellido) VALUES (?, ?)',
  ['Mario', 'Vargas Llosa']
);
console.log(result.insertId);
```

#### UPDATE (Actualizar)

```javascript
await pool.query(
  'UPDATE autores SET nacionalidad = ? WHERE id_autor = ?',
  ['Española', 1]
);
```

#### DELETE (Eliminar)

```javascript
await pool.query(
  'DELETE FROM autores WHERE id_autor = ?',
  [1]
);
```

### Parámetros Preparados (Prepared Statements)

**⚠️ SIEMPRE usar parámetros preparados para prevenir SQL Injection:**

```javascript
// ❌ MAL - Vulnerable a SQL Injection
const nombre = "Juan'; DROP TABLE usuarios; --";
const query = `SELECT * FROM usuarios WHERE nombre = '${nombre}'`;

// ✅ BIEN - Seguro con parámetros preparados
const nombre = "Juan'; DROP TABLE usuarios; --";
const [rows] = await pool.query(
  'SELECT * FROM usuarios WHERE nombre = ?',
  [nombre]
);
```

### Integración con Express

```javascript
import express from 'express';
import { pool } from './db.js';

const app = express();
app.use(express.json());

// GET - Consultar
app.get('/autores', async (req, res) => {
  try {
    const [autores] = await pool.query('SELECT * FROM autores');
    res.json(autores);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// POST - Crear
app.post('/autores', async (req, res) => {
  try {
    const { nombre, apellido, nacionalidad } = req.body;
    await pool.query(
      'INSERT INTO autores (nombre, apellido, nacionalidad) VALUES (?, ?, ?)',
      [nombre, apellido, nacionalidad]
    );
    res.status(201).json({ message: 'Autor creado' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// PUT - Actualizar
app.put('/autores/:id', async (req, res) => {
  try {
    const { id } = req.params;
    const { nombre, apellido } = req.body;
    await pool.query(
      'UPDATE autores SET nombre = ?, apellido = ? WHERE id_autor = ?',
      [nombre, apellido, id]
    );
    res.json({ message: 'Autor actualizado' });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// DELETE - Eliminar
app.delete('/autores/:id', async (req, res) => {
  try {
    const { id } = req.params;
    await pool.query('DELETE FROM autores WHERE id_autor = ?', [id]);
    res.json({ message: 'Autor eliminado' });
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

### DDL (Data Definition Language)
- `CREATE`: Crear bases de datos, tablas, índices
- `ALTER`: Modificar estructura
- `DROP`: Eliminar estructuras
- `TRUNCATE`: Vaciar tablas

### DML (Data Manipulation Language)
- `SELECT`: Consultar datos
- `INSERT`: Insertar datos
- `UPDATE`: Actualizar datos
- `DELETE`: Eliminar datos

### DCL (Data Control Language)
- `GRANT`: Otorgar permisos
- `REVOKE`: Revocar permisos

### JOINs
- `INNER JOIN`: Coincidencias en ambas tablas
- `LEFT JOIN`: Todos de izquierda + coincidencias
- `RIGHT JOIN`: Todos de derecha + coincidencias
- `CROSS JOIN`: Producto cartesiano

### Funciones de Agregación
- `COUNT`: Contar filas
- `SUM`: Sumar valores
- `AVG`: Promedio
- `MIN`/`MAX`: Mínimo/Máximo

### Seguridad
- ✅ Siempre usar parámetros preparados (`?`)
- ✅ Validar datos antes de insertar
- ✅ Usar connection pool en producción
- ✅ Gestionar permisos con DCL

---

**Referencias del Código Modelo:**
- `cursadas/backend/backEnd_modelo/tema-08-mysql-scripts-sql/`
- `cursadas/backend/backEnd_modelo/tema-09-mysql-conexion-nodejs/`
