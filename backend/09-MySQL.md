# Master Guide: MySQL y Bases de Datos Relacionales

## 📑 Índice
1. [Fundamentos: ¿Qué es una Base de Datos Relacional?](#1-fundamentos-qué-es-una-base-de-datos-relacional)
2. [ACID: Propiedades de las Transacciones](#2-acid-propiedades-de-las-transacciones)
3. [Claves y Relaciones](#3-claves-y-relaciones)
   - [ON DELETE: Comportamiento al Eliminar Registros Relacionados](#-on-delete-comportamiento-al-eliminar-registros-relacionados)
   - [Visualización de Relaciones N:M con Tabla Intermedia](#-visualización-de-relaciones-nm-con-tabla-intermedia)
4. [Tipos de Datos y Restricciones](#4-tipos-de-datos-y-restricciones)
5. [Normalización](#5-normalización)
6. [DDL - Data Definition Language](#6-ddl---data-definition-language)
7. [DML - Data Manipulation Language](#7-dml---data-manipulation-language)
8. [DCL - Data Control Language](#8-dcl---data-control-language)
9. [JOINs - Uniones entre Tablas](#9-joins---uniones-entre-tablas)
   - [Comparación Visual: INNER vs LEFT vs RIGHT JOIN](#comparación-visual-inner-vs-left-vs-right-join)
   - [¿Cuándo usar cada tipo de JOIN?](#cuándo-usar-cada-tipo-de-join)
   - [Encontrar Datos Sin Relaciones](#encontrar-datos-sin-relaciones)
   - [Errores Comunes con JOINs y Relaciones](#errores-comunes-con-joins-y-relaciones)
10. [Funciones de Agregación](#10-funciones-de-agregación)
11. [Triggers, Stored Procedures y Functions](#11-triggers-stored-procedures-y-functions)
12. [Conexión desde Node.js](#12-conexión-desde-nodejs)
13. [Ejercicios Prácticos con Base de Datos Escuela](#-ejercicios-prácticos-con-base-de-datos-escuela)

---

## 1. Fundamentos: ¿Qué es una Base de Datos Relacional?

### 📜 Origen Histórico

**Edgar F. Codd (IBM) - 1970**: Pionero de las bases de datos relacionales. 
En 1970, Codd publicó el modelo relacional que sentó las bases de las bases de datos modernas que conocemos hoy. Su trabajo revolucionó la forma en que almacenamos y gestionamos información.

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

### 🏗️ ¿Dónde está la Base de Datos en un Sistema?

**Arquitectura simplificada**:

```
Frontend
├── Muestra datos
└── Solicita información

    ↓ (HTTP/API)

Backend
├── Calcula (Servicios)
└── Consulta datos

    ↓ (SQL)

Base de Datos
├── Almacena datos
├── Organiza datos
└── Gestiona datos
```

**Flujo de información**:
1. **Frontend**: Muestra y solicita información al usuario
2. **API**: Comunica entre frontend y backend
3. **Backend**: Procesa lógica de negocio y realiza consultas
4. **Base de Datos**: Almacena, organiza y gestiona los datos permanentemente

Una **Base de Datos Relacional** es una forma de organizar datos en **tablas** que están conectadas entre sí mediante relaciones. La clave del modelo relacional es que evita la repetición de datos, permitiendo una gestión más eficiente y segura.

### Estructura Básica

*   **Tabla (o Entidad)**: Una colección de datos sobre un tema específico. Se compone de filas y columnas. Por ejemplo, una tabla de `autores` o una tabla de `libros`.
*   **Fila (o Registro)**: Una única entrada en una tabla. En la tabla `autores`, una fila representaría a un autor específico.
*   **Columna (o Atributo)**: Un campo de datos que describe una característica de la entidad. En la tabla `autores`, las columnas serían `nombre` o `nacionalidad`.
*   **Base de Datos (Database)**: Contenedor que agrupa tablas relacionadas.

### Características de las Bases de Datos Relacionales

- ✅ **Esquema Estricto**: La estructura debe definirse antes de insertar datos
  
  **Ventajas (PRO)**:
  - ✅ Ayuda en el orden y organización
  - ✅ Brinda una estructura sólida para construir
  - ✅ Garantiza consistencia de datos
  - ✅ Facilita el mantenimiento
  
  **Desventajas (CONTRA)**:
  - ⚠️ Gran inversión en diseño inicial
  - ⚠️ Una vez que tenemos datos, cambiar la estructura es complejo
  - ⚠️ Requiere planificación cuidadosa
  
  **Solución para cambios**: Migraciones
  - Las migraciones permiten modificar la estructura de la base de datos
  - Se crean scripts que transforman el esquema de forma controlada
  - Permiten versionar los cambios en la estructura

- ✅ **Integridad Referencial**: Las relaciones entre tablas se validan automáticamente
  
  **Beneficio clave**: 
  - Permite complejizar datos, sin complejizar una tabla
  - Se fragmenta la información, haciendo que sea más manejable
  - Cada tabla se enfoca en un tema específico
  - Las relaciones conectan la información de forma controlada

- ✅ **ACID**: Garantiza Atomicidad, Consistencia, Aislamiento y Durabilidad
- ✅ **Normalización**: Permite eliminar redundancia de datos
- ✅ **SQL**: Lenguaje estándar para consultas y operaciones
  
  **Bases de Datos Relacionales que usan SQL**:
  - MySQL
  - PostgreSQL
  - SQLite
  - MariaDB
  - Oracle
  
  **Nota**: Todas las bases de datos relacionales utilizan el mismo lenguaje SQL (con pequeñas diferencias entre ellas)

### Tipos de Lenguajes de Programación

**Lenguajes Imperativos**: Describen **CÓMO** hacer algo paso a paso
- Ejemplos: JavaScript, TypeScript, Java, Python, Go, Ruby, Rust, C, C++
- Describen una secuencia de comandos para alcanzar el objetivo
- El programador controla cada paso del proceso

**Lenguajes Declarativos**: Describen **QUÉ** se quiere obtener
- Ejemplos: SQL, HTML, CSS
- Describen el resultado final deseado
- El lenguaje gestiona los pasos necesarios para lograrlo

**SQL es Declarativo**:
- ❌ No decimos: "itera por cada fila, compara, filtra, ordena..."
- ✅ Decimos: "quiero todos los estudiantes mayores de 20 años"
- MySQL decide la mejor forma de obtenerlos automáticamente

**Ejemplo comparativo**:

```javascript
// IMPERATIVO (JavaScript): Cómo hacerlo
const estudiantesMayores = [];
for (let i = 0; i < estudiantes.length; i++) {
    if (estudiantes[i].edad > 20) {
        estudiantesMayores.push(estudiantes[i]);
    }
}
```

```sql
-- DECLARATIVO (SQL): Qué queremos
SELECT * FROM estudiantes WHERE edad > 20;
```

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

**⚠️ Importante**: Es recomendable que la PRIMARY KEY tenga AUTO_INCREMENT
- Facilita la inserción de datos (no necesitas especificar el ID)
- Evita errores de duplicación
- Garantiza valores únicos y secuenciales

**Ejemplo:**
```sql
CREATE TABLE autores (
    id_autor INT PRIMARY KEY AUTO_INCREMENT,  -- ✅ Recomendado con AUTO_INCREMENT
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

##### 🔄 ON DELETE: Comportamiento al Eliminar Registros Relacionados

**¿Qué pasa si NO especificas ON DELETE?**

Si creas una clave foránea sin especificar `ON DELETE`:
```sql
FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id)
-- Sin ON DELETE especificado
```

MySQL usa `ON DELETE RESTRICT` por defecto. Esto significa:
- ❌ **NO puedes eliminar** un registro padre si tiene hijos que lo referencian
- La eliminación **falla con error**
- No se elimina nada, **no quedan datos NULL ni huérfanos**
- Garantiza **integridad referencial**

**Ejemplo práctico:**
```sql
-- Intentar eliminar estudiante con inscripciones (SIN CASCADE)
DELETE FROM estudiantes WHERE id = 1;

-- Resultado: ERROR
-- Error Code: 1451. Cannot delete or update a parent row: 
-- a foreign key constraint fails
```

**¿Es obligatorio especificar ON DELETE RESTRICT?**

**No**, no es obligatorio. Si no lo especificas, MySQL usa `RESTRICT` por defecto.

```sql
-- Opción 1: Sin especificar (RESTRICT por defecto)
FOREIGN KEY (id_materia) REFERENCES materias(id)

-- Opción 2: Explícitamente RESTRICT (más claro)
FOREIGN KEY (id_materia) REFERENCES materias(id)
ON DELETE RESTRICT
```

**Comparación de Comportamientos:**

| Comportamiento | ¿Se elimina el padre? | ¿Qué pasa con los hijos? | ¿Datos NULL? | ¿Datos huérfanos? |
|----------------|----------------------|--------------------------|--------------|-------------------|
| **RESTRICT** (por defecto) | ❌ No (error) | Se mantienen | ❌ No | ❌ No |
| **CASCADE** | ✅ Sí | Se eliminan automáticamente | ❌ No | ❌ No |
| **SET NULL** | ✅ Sí | Clave foránea = NULL | ✅ Sí | ⚠️ Sí (huérfanos) |
| **NO ACTION** | ❌ No (error) | Se mantienen | ❌ No | ❌ No |

**Ejemplos Prácticos:**

**Ejemplo 1: RESTRICT (por defecto)**
```sql
-- Intentar eliminar materia con inscripciones
DELETE FROM materias WHERE id = 1;
-- ❌ ERROR: Cannot delete or update a parent row
-- La materia NO se elimina, las inscripciones se mantienen
```

**Ejemplo 2: CASCADE**
```sql
-- Crear tabla con CASCADE
CREATE TABLE inscripciones (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_estudiante INT NOT NULL,
    id_materia INT NOT NULL,
    FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id) 
        ON DELETE CASCADE  -- Si eliminas estudiante, elimina sus inscripciones
);

-- Eliminar estudiante con inscripciones
DELETE FROM estudiantes WHERE id = 1;
-- ✅ Estudiante eliminado
-- ✅ Las inscripciones se eliminan automáticamente
```

**Ejemplo 3: SET NULL**
```sql
-- Crear tabla con SET NULL (requiere que la columna permita NULL)
CREATE TABLE inscripciones (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_estudiante INT NULL,  -- Debe permitir NULL
    id_materia INT NOT NULL,
    FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id) 
        ON DELETE SET NULL
);

-- Eliminar estudiante
DELETE FROM estudiantes WHERE id = 1;
-- ✅ Estudiante eliminado
-- ⚠️ Las inscripciones quedan con id_estudiante = NULL (datos huérfanos)
```

**¿Cuándo usar cada uno?**

- **CASCADE**: Cuando los registros hijos no tienen sentido sin el padre
  - Ejemplo: Inscripciones sin estudiante no tienen sentido
  - Ejemplo: Pedidos sin usuario no tienen sentido
  
- **RESTRICT**: Cuando necesitas proteger datos importantes
  - Ejemplo: No eliminar una materia si tiene estudiantes inscritos
  - Ejemplo: No eliminar un autor si tiene libros publicados
  
- **SET NULL**: Cuando quieres mantener los registros hijos pero sin relación
  - Ejemplo: Mantener historial de pedidos aunque se elimine el usuario
  - ⚠️ Cuidado: Puede dejar datos huérfanos

**En la práctica:**
```sql
-- Ejemplo común: Base de datos escuela
CREATE TABLE inscripciones (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_estudiante INT NOT NULL,
    id_materia INT NOT NULL,
    fecha_inscripcion DATE NOT NULL,
    nota DECIMAL(4,2) DEFAULT NULL,
    -- CASCADE: Si eliminas estudiante, elimina sus inscripciones
    FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id) 
        ON DELETE CASCADE,
    -- RESTRICT: No puedes eliminar materia si tiene inscripciones
    FOREIGN KEY (id_materia) REFERENCES materias(id) 
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

##### 📊 Visualización de Relaciones N:M con Tabla Intermedia

**Ejemplo: Estudiantes y Materias**

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│ estudiantes │         │ inscripciones│         │   materias  │
├─────────────┤         ├──────────────┤         ├─────────────┤
│ id (PK)     │◄──┐     │ id (PK)      │     ┌──►│ id (PK)     │
│ nombre      │   │     │ id_estudiante│     │   │ nombre      │
│ apellido    │   │     │ id_materia   │◄────┘   │ codigo      │
│ email       │   │     │ fecha        │         │ creditos    │
│ edad        │   │     │ nota         │         └─────────────┘
└─────────────┘   │     └──────────────┘
                  │
                  └─── FOREIGN KEY (ON DELETE CASCADE)
```

**Flujo de datos en una relación N:M:**
1. Estudiante (id=1) se inscribe en Materia (id=1)
2. Se crea registro en `inscripciones` (id_estudiante=1, id_materia=1)
3. La clave foránea garantiza que ambos IDs existan
4. Si eliminas el estudiante (CASCADE), se eliminan sus inscripciones automáticamente
5. Si intentas eliminar la materia (RESTRICT), MySQL no te permite si tiene inscripciones

**Ejemplo concreto:**
```
Estudiante: Juan Pérez (id=1)
  ↓ (tiene inscripciones)
Inscripción 1: id_estudiante=1, id_materia=1 (Programación I)
Inscripción 2: id_estudiante=1, id_materia=2 (Base de Datos)
  → Juan está inscrito en 2 materias ✅

Materia: Programación I (id=1)
  ↓ (tiene inscripciones)
Inscripción 1: id_estudiante=1, id_materia=1 (Juan Pérez)
Inscripción 3: id_estudiante=2, id_materia=1 (María González)
  → Programación I tiene 2 estudiantes ✅
```

**¿Por qué necesitamos una tabla intermedia para N:M?**
- Si intentáramos poner `id_materia` en la tabla `estudiantes`, solo podríamos guardar UNA materia por estudiante
- Si intentáramos poner `id_estudiante` en la tabla `materias`, solo podríamos guardar UN estudiante por materia
- La tabla intermedia permite: **muchos estudiantes ↔ muchas materias**

### Tipos de Relaciones en Diagramas (ERD)

#### Relación Identifying (Línea Continua)
La clave foránea (FK) **también es parte de la clave primaria (PK)** en la tabla hija. Un registro hijo no puede existir sin el padre. Se usa para entidades débiles.

#### Relación Non-Identifying (Línea Punteada)
La clave foránea (FK) **no es parte de la clave primaria** en la tabla hija. Es la relación más común. El registro hijo puede ser identificado independientemente.

---

## 4. Tipos de Datos y Restricciones

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

### Tipos de Datos Numéricos - Detalles y Rangos

#### Enteros (Integer Types)

| Tipo | Tamaño | Rango (con signo) | Rango (sin signo) | Uso recomendado |
|------|--------|-------------------|-------------------|-----------------|
| `TINYINT` | 1 byte | -128 a 127 | 0 a 255 | Booleanos, estados pequeños, códigos |
| `SMALLINT` | 2 bytes | -32,768 a 32,767 | 0 a 65,535 | Contadores pequeños, años, códigos |
| `INT` o `INTEGER` | 4 bytes | -2,147,483,648 a 2,147,483,647 | 0 a 4,294,967,295 | IDs, edades, cantidades (más común) |
| `BIGINT` | 8 bytes | -9,223,372,036,854,775,808 a 9,223,372,036,854,775,807 | 0 a 18,446,744,073,709,551,615 | IDs muy grandes, timestamps grandes |

**Ejemplos:**
```sql
-- TINYINT para booleanos
activo TINYINT(1)  -- 0 o 1

-- SMALLINT para años
año SMALLINT  -- Años (1900-2155)

-- INT para IDs (más común)
id INT PRIMARY KEY AUTO_INCREMENT

-- BIGINT para sistemas de gran escala
id_usuario BIGINT PRIMARY KEY AUTO_INCREMENT
```

#### Decimales Exactos (Fixed-Point)

| Tipo | Precisión | Tamaño | Uso recomendado |
|------|-----------|--------|------------------|
| `DECIMAL(p, s)` o `NUMERIC(p, s)` | Exacta | Variable según `p` | Dinero, precios, medidas que requieren precisión exacta |

**Parámetros:**
- `p` = Precisión total (total de dígitos)
- `s` = Escala (dígitos después del punto decimal)

**Ejemplos:**
```sql
precio DECIMAL(10, 2)      -- 99999999.99 (8 dígitos antes, 2 después)
nota DECIMAL(4, 2)         -- 99.99 (2 dígitos antes, 2 después)
porcentaje DECIMAL(5, 2)   -- 999.99 (3 dígitos antes, 2 después)
```

#### Decimales Aproximados (Floating-Point)

| Tipo | Tamaño | Precisión | Rango aproximado | Uso recomendado |
|------|--------|-----------|------------------|-----------------|
| `FLOAT` | 4 bytes | ~7 dígitos decimales | ±3.4E38 | Cálculos científicos, mediciones |
| `DOUBLE` o `DOUBLE PRECISION` | 8 bytes | ~15 dígitos decimales | ±1.7E308 | Cálculos científicos más precisos |

**⚠️ Importante:** `FLOAT` y `DOUBLE` tienen precisión aproximada (pueden tener errores de redondeo). Para dinero o valores que requieren precisión exacta, usar `DECIMAL`.

**Ejemplos:**
```sql
-- FLOAT para mediciones
temperatura FLOAT
altura FLOAT

-- DOUBLE para coordenadas
coordenada_latitud DOUBLE
coordenada_longitud DOUBLE
```

#### Comparación: DECIMAL vs FLOAT/DOUBLE

| Característica | DECIMAL | FLOAT/DOUBLE |
|----------------|---------|--------------|
| **Precisión** | Exacta | Aproximada |
| **Uso para dinero** | ✅ Recomendado | ❌ No recomendado |
| **Uso para cálculos científicos** | ⚠️ Posible pero lento | ✅ Recomendado |
| **Errores de redondeo** | ❌ No tiene | ✅ Puede tener |

**Regla de oro:**
- 💰 **Para dinero/precios**: Usa `DECIMAL(10,2)` o `DECIMAL(19,4)`
- 🔬 **Para cálculos científicos**: Usa `FLOAT` o `DOUBLE`
- 🔢 **Para IDs**: Usa `INT` (suficiente en la mayoría de casos)
- ✅ **Para booleanos**: Usa `TINYINT(1)` o `BOOLEAN`

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

## 5. Normalización

Proceso para organizar la base de datos reduciendo redundancia y mejorando la integridad de los datos.

### Problema de Redundancia

Si en `libros` guardamos el nombre del autor directamente:
- Al corregir el nombre del autor, tendríamos que editar todos sus libros
- Mayor riesgo de inconsistencias
- Mayor uso de almacenamiento

**Ejemplo práctico**: Si tengo usuarios que poseen nacionalidad, no tiene sentido que los datos del país estén en la tabla usuario.

### Solución: Normalización

Crear tablas separadas y referenciarlas por ID:

```sql
-- ❌ MAL: Redundancia - Datos del país en cada usuario
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    nacionalidad VARCHAR(50),
    codigo_pais VARCHAR(2),
    capital VARCHAR(100)  -- Se repite para cada usuario del mismo país
);

-- ✅ BIEN: Normalizado - Tabla separada de países
CREATE TABLE paises (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    codigo VARCHAR(2) UNIQUE,
    capital VARCHAR(100)
);

CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(100),
    pais_id INT,
    FOREIGN KEY (pais_id) REFERENCES paises(id)
);
```

**Otro ejemplo con autores y libros**:

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

## 6. DDL - Data Definition Language

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

## 7. DML - Data Manipulation Language

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

## 8. DCL - Data Control Language

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

## 9. JOINs - Uniones entre Tablas

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

### Comparación Visual: INNER vs LEFT vs RIGHT JOIN

Usando la misma base de datos `escuela` con:
- 30 estudiantes (algunos sin inscripciones)
- 30 materias (algunas sin estudiantes)
- Inscripciones variadas

#### Ejemplo 1: INNER JOIN - Solo Coincidencias

```sql
SELECT 
    e.nombre, 
    e.apellido, 
    m.nombre as materia,
    i.nota
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id
ORDER BY e.apellido, m.nombre;
```

**Resultado**: Aproximadamente 60-70 filas (solo coincidencias)
- ✅ Muestra SOLO estudiantes que tienen inscripciones
- ✅ Muestra SOLO materias que tienen estudiantes
- ❌ NO muestra estudiantes sin inscripciones (ej: estudiantes 16-20, 25, 28, 30)
- ❌ NO muestra materias sin estudiantes (ej: materias 24, 27-30)

**Visualización**:
```
Estudiantes con inscripciones → ✅ Aparecen
Estudiantes sin inscripciones → ❌ NO aparecen
Materias con estudiantes → ✅ Aparecen
Materias sin estudiantes → ❌ NO aparecen
```

#### Ejemplo 2: LEFT JOIN - Todos los Estudiantes

```sql
SELECT 
    e.nombre, 
    e.apellido, 
    m.nombre as materia,
    i.nota
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
LEFT JOIN materias m ON i.id_materia = m.id
ORDER BY e.apellido, m.nombre;
```

**Resultado**: Más de 70 filas (todos los estudiantes)
- ✅ Muestra TODOS los estudiantes (30 estudiantes)
- ✅ Estudiantes con inscripciones: muestran sus materias
- ⚠️ Estudiantes SIN inscripciones: muestran NULL en materia y nota
- ❌ NO muestra materias sin estudiantes

**Ejemplo de filas con NULL**:
```
nombre    | apellido  | materia | nota
Diego     | Morales   | NULL    | NULL  ← Estudiante sin inscripciones
Emma      | Rivera    | NULL    | NULL  ← Estudiante sin inscripciones
Benjamin  | Ortiz     | NULL    | NULL  ← Estudiante sin inscripciones
```

**Visualización**:
```
Estudiantes con inscripciones → ✅ Aparecen con sus materias
Estudiantes sin inscripciones → ⚠️ Aparecen con NULL
Materias con estudiantes → ✅ Aparecen
Materias sin estudiantes → ❌ NO aparecen
```

#### Ejemplo 3: RIGHT JOIN - Todas las Materias

```sql
SELECT 
    e.nombre, 
    e.apellido, 
    m.nombre as materia,
    m.codigo,
    i.nota
FROM estudiantes e
RIGHT JOIN inscripciones i ON e.id = i.id_estudiante
RIGHT JOIN materias m ON i.id_materia = m.id
ORDER BY m.nombre, e.apellido;
```

**Resultado**: Más de 70 filas (todas las materias)
- ✅ Muestra TODAS las materias (30 materias)
- ✅ Materias con estudiantes: muestran los estudiantes inscritos
- ⚠️ Materias SIN estudiantes: muestran NULL en nombre, apellido y nota
- ❌ NO muestra estudiantes sin inscripciones

**Ejemplo de filas con NULL**:
```
nombre | apellido | materia                    | codigo | nota
NULL   | NULL     | Comunicación               | COM1   | NULL  ← Materia sin estudiantes
NULL   | NULL     | Proyecto Integrador II     | PI2    | NULL  ← Materia sin estudiantes
NULL   | NULL     | Prácticas Profesionales    | PP1    | NULL  ← Materia sin estudiantes
```

**Visualización**:
```
Estudiantes con inscripciones → ✅ Aparecen
Estudiantes sin inscripciones → ❌ NO aparecen
Materias con estudiantes → ✅ Aparecen con sus estudiantes
Materias sin estudiantes → ⚠️ Aparecen con NULL
```

#### Tabla Comparativa de JOINs

| Tipo de JOIN | ¿Qué muestra? | ¿Muestra NULL? | Casos de uso |
|--------------|---------------|----------------|--------------|
| **INNER JOIN** | Solo coincidencias | ❌ No | Ver solo datos relacionados |
| **LEFT JOIN** | Todos de la izquierda | ✅ Sí (derecha) | Ver todos los estudiantes |
| **RIGHT JOIN** | Todos de la derecha | ✅ Sí (izquierda) | Ver todas las materias |

#### Comparación de Cantidades

```sql
-- 1. INNER JOIN - Solo coincidencias
SELECT COUNT(*) as total_filas_inner
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id;
-- Resultado: ~60-70 filas

-- 2. LEFT JOIN - Todos los estudiantes
SELECT COUNT(*) as total_filas_left
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
LEFT JOIN materias m ON i.id_materia = m.id;
-- Resultado: ~75-85 filas (más porque incluye estudiantes sin inscripciones)

-- 3. RIGHT JOIN - Todas las materias
SELECT COUNT(*) as total_filas_right
FROM estudiantes e
RIGHT JOIN inscripciones i ON e.id = i.id_estudiante
RIGHT JOIN materias m ON i.id_materia = m.id;
-- Resultado: ~75-85 filas (más porque incluye materias sin estudiantes)
```

**¿Por qué LEFT y RIGHT tienen más filas?**
- **LEFT**: Incluye estudiantes sin inscripciones (aparecen con NULL)
- **RIGHT**: Incluye materias sin estudiantes (aparecen con NULL)

### ¿Cuándo usar cada tipo de JOIN?

#### Usa INNER JOIN cuando:
- ✅ Solo necesitas datos que tienen relación
- ✅ Quieres ver solo coincidencias
- ✅ Ejemplo: Ver inscripciones con nombres (solo las que existen)
- ✅ Ejemplo: Ver pedidos con información del cliente (solo pedidos con cliente)

#### Usa LEFT JOIN cuando:
- ✅ Necesitas TODOS los registros de la tabla izquierda
- ✅ Quieres ver registros incluso sin relación
- ✅ Ejemplo: Listar todos los estudiantes, incluso sin materias
- ✅ Ejemplo: Listar todos los clientes, incluso sin pedidos
- ✅ Útil para encontrar registros "huérfanos" (con WHERE IS NULL)

#### Usa RIGHT JOIN cuando:
- ✅ Necesitas TODOS los registros de la tabla derecha
- ✅ Ejemplo: Listar todas las materias, incluso sin estudiantes
- ✅ Ejemplo: Listar todos los productos, incluso sin pedidos
- ⚠️ **Nota**: Se puede lograr lo mismo con LEFT JOIN cambiando el orden de las tablas

#### Pregunta clave para decidir:
**¿Qué tabla es más importante en tu consulta?**
- Si la tabla izquierda → **LEFT JOIN**
- Si la tabla derecha → **RIGHT JOIN**
- Si solo coincidencias → **INNER JOIN**

### Encontrar Datos Sin Relaciones

#### Estudiantes Sin Inscripciones (LEFT JOIN + WHERE IS NULL)

```sql
SELECT 
    e.nombre, 
    e.apellido, 
    e.email
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
WHERE i.id IS NULL
ORDER BY e.apellido;
```

**Explicación**:
- `LEFT JOIN` trae todos los estudiantes
- `WHERE i.id IS NULL` filtra solo los que no tienen inscripciones (el JOIN devolvió NULL)

**Resultado esperado**: Estudiantes que no están inscritos en ninguna materia

#### Materias Sin Estudiantes (RIGHT JOIN + WHERE IS NULL)

```sql
SELECT 
    m.nombre,
    m.codigo,
    m.creditos
FROM inscripciones i
RIGHT JOIN materias m ON i.id_materia = m.id
WHERE i.id IS NULL
ORDER BY m.nombre;
```

**Explicación**:
- `RIGHT JOIN` trae todas las materias
- `WHERE i.id IS NULL` filtra solo las que no tienen inscripciones

**Resultado esperado**: Materias que no tienen ningún estudiante inscrito

**Alternativa con LEFT JOIN** (mismo resultado):
```sql
SELECT 
    m.nombre,
    m.codigo,
    m.creditos
FROM materias m
LEFT JOIN inscripciones i ON m.id = i.id_materia
WHERE i.id IS NULL
ORDER BY m.nombre;
```

### Errores Comunes con JOINs y Relaciones

#### Error 1: "Cannot delete or update a parent row: a foreign key constraint fails"

**Problema**: Intentas eliminar un registro padre que tiene hijos que lo referencian.

**Ejemplo**:
```sql
-- Intentar eliminar estudiante con inscripciones (RESTRICT por defecto)
DELETE FROM estudiantes WHERE id = 1;
-- ❌ ERROR: Cannot delete or update a parent row
```

**Soluciones**:
1. **Eliminar primero los hijos**:
```sql
-- Primero eliminar las inscripciones
DELETE FROM inscripciones WHERE id_estudiante = 1;
-- Ahora sí puedes eliminar el estudiante
DELETE FROM estudiantes WHERE id = 1;
```

2. **Usar ON DELETE CASCADE** (si tiene sentido):
```sql
-- Recrear tabla con CASCADE
ALTER TABLE inscripciones
DROP FOREIGN KEY inscripciones_ibfk_1;

ALTER TABLE inscripciones
ADD CONSTRAINT fk_estudiante 
FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id) 
ON DELETE CASCADE;
```

#### Error 2: "Cannot add or update a child row: foreign key constraint fails"

**Problema**: Intentas insertar un registro hijo con un ID que no existe en la tabla padre.

**Ejemplo**:
```sql
-- Intentar insertar inscripción con estudiante inexistente
INSERT INTO inscripciones (id_estudiante, id_materia, fecha_inscripcion) 
VALUES (999, 1, '2025-01-15');
-- ❌ ERROR: Cannot add or update a child row
```

**Solución**: Verificar que el ID exista antes de insertar:
```sql
-- Verificar que el estudiante existe
SELECT id FROM estudiantes WHERE id = 999;
-- Si no existe, usar un ID válido o crear el estudiante primero
```

#### Error 3: LEFT JOIN muestra más filas de las esperadas

**Problema**: Un estudiante con múltiples materias aparece varias veces (una por cada materia).

**Ejemplo**:
```sql
SELECT e.nombre, e.apellido, m.nombre as materia
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
LEFT JOIN materias m ON i.id_materia = m.id;
-- Resultado: Juan Pérez aparece 3 veces (una por cada materia)
```

**Explicación**: Es normal. Cada relación genera una fila. Si un estudiante tiene 3 materias, aparecerá 3 veces.

**Si necesitas valores únicos**, usa `DISTINCT`:
```sql
SELECT DISTINCT e.nombre, e.apellido
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante;
```

#### Error 4: Olvidar el `ON` en JOIN

**Problema**: Crear un JOIN sin especificar la condición.

**Ejemplo incorrecto**:
```sql
SELECT e.nombre, m.nombre
FROM estudiantes e
INNER JOIN materias m;  -- ❌ Falta ON
-- ERROR: Syntax error
```

**Solución**: Siempre incluir la condición `ON`:
```sql
SELECT e.nombre, m.nombre
FROM estudiantes e
INNER JOIN materias m ON e.id = m.id;  -- ✅ Correcto
```

#### Error 5: Confundir WHERE y HAVING con JOINs

**Problema**: Usar `HAVING` para filtrar filas individuales en lugar de `WHERE`.

**Ejemplo incorrecto**:
```sql
SELECT e.nombre, COUNT(i.id) as total_materias
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
HAVING total_materias > 2;  -- ⚠️ Funciona pero no es lo ideal
```

**Solución correcta**:
```sql
SELECT e.nombre, COUNT(i.id) as total_materias
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
GROUP BY e.id, e.nombre
HAVING COUNT(i.id) > 2;  -- ✅ HAVING filtra grupos, no filas
```

**Regla general**:
- `WHERE`: Filtra filas individuales (antes de agrupar)
- `HAVING`: Filtra grupos (después de agrupar con GROUP BY)

---

## 10. Funciones de Agregación

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

### Funciones de Formato de Números

#### ROUND - Redondear

Redondea un número a un número específico de decimales.

```sql
-- Redondear a 2 decimales (más común)
SELECT ROUND(AVG(precio), 2) as precio_promedio FROM productos;

-- Redondear a 0 decimales (entero)
SELECT ROUND(123.456, 0);  -- Resultado: 123

-- Redondear a 1 decimal
SELECT ROUND(123.456, 1);  -- Resultado: 123.5

-- Ejemplo práctico: Promedio de notas de estudiantes
SELECT 
    e.nombre,
    e.apellido,
    ROUND(AVG(i.nota), 2) as promedio_notas
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
WHERE i.nota IS NOT NULL
GROUP BY e.id, e.nombre, e.apellido;
```

#### TRUNCATE - Truncar (Cortar)

Trunca un número a un número específico de decimales **sin redondear**.

```sql
-- Truncar a 2 decimales
SELECT TRUNCATE(123.456, 2);  -- Resultado: 123.45

-- Truncar a 0 decimales
SELECT TRUNCATE(123.456, 0);  -- Resultado: 123

-- Ejemplo práctico: Promedio truncado por materia
SELECT 
    m.nombre as materia,
    TRUNCATE(AVG(i.nota), 1) as promedio_truncado
FROM materias m
INNER JOIN inscripciones i ON m.id = i.id_materia
WHERE i.nota IS NOT NULL
GROUP BY m.id, m.nombre;
```

**Diferencia ROUND vs TRUNCATE:**
- `ROUND(123.456, 2)` → `123.46` (redondea hacia arriba)
- `TRUNCATE(123.456, 2)` → `123.45` (solo corta)

#### FORMAT - Formatear con Separadores

Formatea un número con separadores de miles y decimales.

```sql
-- Formatear con separadores de miles
SELECT FORMAT(1234567.89, 2);  -- Resultado: '1,234,567.89'

-- Formatear sin decimales
SELECT FORMAT(1234567, 0);  -- Resultado: '1,234,567'

-- Ejemplo práctico: Total de créditos formateado
SELECT 
    e.nombre,
    e.apellido,
    FORMAT(SUM(m.creditos), 0) as total_creditos_formateado
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id
GROUP BY e.id, e.nombre, e.apellido;
```

**⚠️ Nota:** `FORMAT` devuelve un string, no un número. Útil para mostrar datos pero no para cálculos.

#### CAST / CONVERT - Convertir Tipos

Convierte un valor a otro tipo de dato.

```sql
-- Convertir DECIMAL a INT (trunca decimales)
SELECT CAST(123.456 AS UNSIGNED);  -- Resultado: 123

-- Convertir string a número
SELECT CAST('123' AS UNSIGNED);  -- Resultado: 123

-- Sintaxis alternativa con CONVERT
SELECT CONVERT(123.456, UNSIGNED);  -- Resultado: 123

-- Ejemplo práctico: Promedio como entero
SELECT 
    e.nombre,
    e.apellido,
    CAST(ROUND(AVG(i.nota), 0) AS UNSIGNED) as promedio_entero
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
WHERE i.nota IS NOT NULL
GROUP BY e.id, e.nombre, e.apellido;
```

### Ejemplos Combinados

```sql
-- Estadísticas por categoría con formato
SELECT 
    categoria,
    COUNT(*) as total_productos,
    FORMAT(SUM(precio), 2) as total_ventas,
    ROUND(AVG(precio), 2) as precio_promedio,
    MIN(precio) as precio_minimo,
    MAX(precio) as precio_maximo
FROM productos
GROUP BY categoria
HAVING total_productos > 5
ORDER BY precio_promedio DESC;
```

---

## 11. Triggers, Stored Procedures y Functions

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

## 12. Conexión desde Node.js

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

## 🎯 Ejercicios Prácticos con Base de Datos Escuela

Estos ejercicios te ayudarán a practicar JOINs, relaciones y consultas complejas usando la base de datos `escuela` con las tablas `estudiantes`, `materias` e `inscripciones`.

### Ejercicio 1: Verificar Integridad Referencial

**Objetivo**: Verificar que no haya inscripciones con IDs inválidos (datos huérfanos).

```sql
-- Verificar que todas las inscripciones tienen estudiantes y materias válidos
SELECT 
    i.id as id_inscripcion,
    i.id_estudiante,
    i.id_materia,
    CASE WHEN e.id IS NULL THEN 'ERROR: Estudiante no existe' ELSE 'OK' END as estudiante_valido,
    CASE WHEN m.id IS NULL THEN 'ERROR: Materia no existe' ELSE 'OK' END as materia_valida
FROM inscripciones i
LEFT JOIN estudiantes e ON i.id_estudiante = e.id
LEFT JOIN materias m ON i.id_materia = m.id
WHERE e.id IS NULL OR m.id IS NULL;
```

**Resultado esperado**: 
- Si la integridad referencial funciona correctamente, esta consulta **NO debe devolver resultados**
- Si devuelve filas, significa que hay datos corruptos (inscripciones con IDs inválidos)

**Explicación**:
- `LEFT JOIN` trae todas las inscripciones
- Si el estudiante o materia no existe, el JOIN devuelve NULL
- `WHERE e.id IS NULL OR m.id IS NULL` filtra solo los casos problemáticos

### Ejercicio 2: Estadísticas de Notas por Materia

**Objetivo**: Calcular el promedio de notas, cantidad de estudiantes y nota máxima por materia.

```sql
SELECT 
    m.nombre as materia,
    m.codigo,
    COUNT(i.id) as total_estudiantes,
    COUNT(i.nota) as estudiantes_con_nota,
    AVG(i.nota) as promedio_notas,
    MAX(i.nota) as nota_maxima,
    MIN(i.nota) as nota_minima
FROM materias m
LEFT JOIN inscripciones i ON m.id = i.id_materia
WHERE i.nota IS NOT NULL  -- Solo materias con notas
GROUP BY m.id, m.nombre, m.codigo
HAVING COUNT(i.nota) > 0  -- Solo materias con al menos una nota
ORDER BY promedio_notas DESC;
```

**Resultado esperado**: 
- Lista de materias con estadísticas de notas
- Ordenadas de mayor a menor promedio

**Explicación**:
- `LEFT JOIN` incluye todas las materias
- `WHERE i.nota IS NOT NULL` filtra solo inscripciones con notas
- `GROUP BY` agrupa por materia
- `HAVING` filtra grupos (materias con al menos una nota)
- Funciones de agregación calculan estadísticas

### Ejercicio 3: Estudiantes con Múltiples Materias

**Objetivo**: Encontrar estudiantes que están inscritos en más de 2 materias.

```sql
SELECT 
    e.nombre,
    e.apellido,
    e.email,
    COUNT(i.id) as total_materias,
    GROUP_CONCAT(m.nombre SEPARATOR ', ') as materias
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id
GROUP BY e.id, e.nombre, e.apellido, e.email
HAVING COUNT(i.id) > 2
ORDER BY total_materias DESC, e.apellido;
```

**Resultado esperado**: 
- Estudiantes con más de 2 materias
- Lista de materias separadas por comas

**Explicación**:
- `INNER JOIN` solo incluye estudiantes con inscripciones
- `GROUP BY` agrupa por estudiante
- `GROUP_CONCAT` concatena los nombres de las materias
- `HAVING COUNT(i.id) > 2` filtra solo estudiantes con más de 2 materias

### Ejercicio 4: Materias Más Populares

**Objetivo**: Encontrar las materias con más estudiantes inscritos.

```sql
SELECT 
    m.nombre as materia,
    m.codigo,
    COUNT(i.id_estudiante) as total_inscritos,
    COUNT(DISTINCT i.id_estudiante) as estudiantes_unicos
FROM materias m
LEFT JOIN inscripciones i ON m.id = i.id_materia
GROUP BY m.id, m.nombre, m.codigo
ORDER BY total_inscritos DESC
LIMIT 10;
```

**Resultado esperado**: 
- Top 10 materias con más inscripciones
- Incluye materias sin estudiantes (aparecerán con 0)

**Explicación**:
- `LEFT JOIN` incluye todas las materias
- `COUNT(i.id_estudiante)` cuenta inscripciones (puede haber duplicados si un estudiante se inscribe dos veces)
- `COUNT(DISTINCT i.id_estudiante)` cuenta estudiantes únicos
- `LIMIT 10` muestra solo las 10 primeras

### Ejercicio 5: Estudiantes Sin Inscripciones

**Objetivo**: Encontrar estudiantes que no están inscritos en ninguna materia.

```sql
SELECT 
    e.id,
    e.nombre,
    e.apellido,
    e.email,
    e.edad
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
WHERE i.id IS NULL
ORDER BY e.apellido, e.nombre;
```

**Resultado esperado**: 
- Lista de estudiantes sin inscripciones
- Útil para identificar estudiantes que necesitan inscribirse

**Explicación**:
- `LEFT JOIN` trae todos los estudiantes
- `WHERE i.id IS NULL` filtra solo los que no tienen inscripciones

### Ejercicio 6: Materias Sin Estudiantes

**Objetivo**: Encontrar materias que no tienen ningún estudiante inscrito.

```sql
SELECT 
    m.id,
    m.nombre,
    m.codigo,
    m.creditos
FROM materias m
LEFT JOIN inscripciones i ON m.id = i.id_materia
WHERE i.id IS NULL
ORDER BY m.nombre;
```

**Resultado esperado**: 
- Lista de materias sin estudiantes
- Útil para identificar materias que no se están ofreciendo

**Explicación**:
- `LEFT JOIN` trae todas las materias
- `WHERE i.id IS NULL` filtra solo las que no tienen inscripciones

### Ejercicio 7: Promedio de Notas por Estudiante

**Objetivo**: Calcular el promedio de notas de cada estudiante (solo los que tienen notas).

```sql
SELECT 
    e.nombre,
    e.apellido,
    COUNT(i.nota) as materias_con_nota,
    AVG(i.nota) as promedio_notas,
    MAX(i.nota) as mejor_nota,
    MIN(i.nota) as peor_nota
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
WHERE i.nota IS NOT NULL
GROUP BY e.id, e.nombre, e.apellido
HAVING COUNT(i.nota) > 0
ORDER BY promedio_notas DESC;
```

**Resultado esperado**: 
- Lista de estudiantes con sus promedios
- Ordenados de mayor a menor promedio

**Explicación**:
- `INNER JOIN` solo incluye estudiantes con inscripciones
- `WHERE i.nota IS NOT NULL` filtra solo inscripciones con notas
- Funciones de agregación calculan estadísticas por estudiante

### Ejercicio 8: Verificar ON DELETE CASCADE

**Objetivo**: Probar que `ON DELETE CASCADE` funciona correctamente.

```sql
-- Paso 1: Ver inscripciones de un estudiante antes de eliminarlo
SELECT 
    e.nombre,
    e.apellido,
    COUNT(i.id) as total_inscripciones
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
WHERE e.id = 1
GROUP BY e.id, e.nombre, e.apellido;

-- Paso 2: Ver las inscripciones específicas
SELECT * FROM inscripciones WHERE id_estudiante = 1;

-- Paso 3: Eliminar el estudiante (si tiene ON DELETE CASCADE)
-- ⚠️ CUIDADO: Esto eliminará el estudiante y sus inscripciones
-- DELETE FROM estudiantes WHERE id = 1;

-- Paso 4: Verificar que las inscripciones se eliminaron automáticamente
-- SELECT * FROM inscripciones WHERE id_estudiante = 1;
-- Debe estar vacío si CASCADE funciona
```

**Resultado esperado**: 
- Antes: El estudiante tiene inscripciones
- Después: El estudiante y sus inscripciones se eliminan

**Explicación**:
- `ON DELETE CASCADE` elimina automáticamente los registros hijos cuando se elimina el padre
- Útil para mantener la integridad referencial sin dejar datos huérfanos

### Ejercicio 9: Comparar INNER, LEFT y RIGHT JOIN

**Objetivo**: Ver las diferencias entre los tres tipos de JOIN con la misma consulta.

```sql
-- 1. INNER JOIN - Solo coincidencias
SELECT 
    'INNER JOIN' as tipo_join,
    COUNT(*) as total_filas
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id;

-- 2. LEFT JOIN - Todos los estudiantes
SELECT 
    'LEFT JOIN' as tipo_join,
    COUNT(*) as total_filas,
    COUNT(CASE WHEN m.id IS NULL THEN 1 END) as estudiantes_sin_materias
FROM estudiantes e
LEFT JOIN inscripciones i ON e.id = i.id_estudiante
LEFT JOIN materias m ON i.id_materia = m.id;

-- 3. RIGHT JOIN - Todas las materias
SELECT 
    'RIGHT JOIN' as tipo_join,
    COUNT(*) as total_filas,
    COUNT(CASE WHEN e.id IS NULL THEN 1 END) as materias_sin_estudiantes
FROM estudiantes e
RIGHT JOIN inscripciones i ON e.id = i.id_estudiante
RIGHT JOIN materias m ON i.id_materia = m.id;
```

**Resultado esperado**: 
- INNER JOIN: Menos filas (solo coincidencias)
- LEFT JOIN: Más filas (incluye estudiantes sin materias)
- RIGHT JOIN: Más filas (incluye materias sin estudiantes)

**Explicación**:
- Compara los tres tipos de JOIN con la misma estructura
- Muestra cómo cada uno incluye o excluye datos diferentes

### Ejercicio 10: Vista Completa de Inscripciones con Información Completa

**Objetivo**: Crear una consulta que muestre toda la información relevante de las inscripciones.

```sql
SELECT 
    e.id as id_estudiante,
    e.nombre as nombre_estudiante,
    e.apellido as apellido_estudiante,
    e.email,
    m.id as id_materia,
    m.nombre as nombre_materia,
    m.codigo as codigo_materia,
    m.creditos,
    i.fecha_inscripcion,
    i.nota,
    CASE 
        WHEN i.nota IS NULL THEN 'Sin calificar'
        WHEN i.nota >= 7 THEN 'Aprobado'
        ELSE 'Desaprobado'
    END as estado
FROM estudiantes e
INNER JOIN inscripciones i ON e.id = i.id_estudiante
INNER JOIN materias m ON i.id_materia = m.id
ORDER BY e.apellido, e.nombre, m.nombre;
```

**Resultado esperado**: 
- Lista completa de inscripciones con toda la información
- Incluye estado calculado basado en la nota

**Explicación**:
- `INNER JOIN` solo muestra inscripciones válidas
- `CASE` calcula el estado basado en la nota
- Ordena por estudiante y luego por materia

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
