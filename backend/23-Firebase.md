# Master Guide: Firebase y Firestore Database 🔥

## 📑 Índice
1. [Introducción a Firebase](#1-introducción-a-firebase)
2. [Conceptos Básicos de Firestore](#2-conceptos-básicos-de-firestore)
3. [Configuración en la Consola de Firebase](#3-configuración-en-la-consola-de-firebase)
4. [Configuración en el Proyecto](#4-configuración-en-el-proyecto)
5. [Operaciones CRUD Completas](#5-operaciones-crud-completas)
6. [Consultas Avanzadas](#6-consultas-avanzadas)
7. [Reglas de Seguridad](#7-reglas-de-seguridad)
8. [Troubleshooting](#8-troubleshooting)
9. [Buenas Prácticas](#9-buenas-prácticas)

---

## 1. Introducción a Firebase

**Firebase** es una plataforma de Google que proporciona servicios backend en tiempo real. Uno de sus servicios estrella es **Cloud Firestore**.

### ¿Qué es Firebase?

Firebase es una plataforma **Backend-as-a-Service (BaaS)** que ofrece:
- ✅ **Firestore**: Base de datos NoSQL en tiempo real
- ✅ **Authentication**: Autenticación de usuarios
- ✅ **Storage**: Almacenamiento de archivos
- ✅ **Hosting**: Hosting de aplicaciones web
- ✅ **Cloud Functions**: Funciones serverless
- ✅ **Analytics**: Análisis de uso

### ¿Por qué usar Firebase?

- ✅ **Rápido de configurar**: Menos código backend necesario
- ✅ **Tiempo real**: Sincronización automática de datos
- ✅ **Escalable**: Maneja grandes cargas automáticamente
- ✅ **Gratis**: Plan gratuito generoso
- ✅ **Integración fácil**: SDKs para múltiples plataformas

---

## 2. Conceptos Básicos de Firestore

### ¿Qué es Firestore?

Es una base de datos **NoSQL** flexible y escalable para el desarrollo para móviles y la web.

**Características**:
- ✅ **NoSQL**: Utiliza colecciones y documentos (similar a MongoDB)
- ✅ **Tiempo real**: Mantiene los datos actualizados en todos los clientes conectados
- ✅ **Escalable**: Maneja grandes cargas de datos automáticamente
- ✅ **Offline**: Soporte para modo offline
- ✅ **Seguro**: Reglas de seguridad granulares

### Estructura de Datos

Firestore organiza los datos en una jerarquía:

```
Base de Datos (Database)
  └── Colección (Collection)
        └── Documento (Document)
              └── Campo (Field)
```

#### Colecciones

**Colecciones** son contenedores de documentos (ej: `usuarios`, `productos`). Equivalente a una tabla en SQL.

**Características**:
- No tienen esquema fijo
- Pueden contener documentos con diferentes estructuras
- Se crean automáticamente al agregar el primer documento

#### Documentos

**Documentos** son unidades de almacenamiento que contienen campos con valores. Equivalente a una fila en SQL.

**Características**:
- Tienen un ID único (automático o personalizado)
- Contienen campos (pares clave-valor)
- Pueden tener subcolecciones

#### Campos

**Campos** son pares de clave-valor (ej: `nombre: "Juan"`). Equivalente a una columna en SQL.

**Tipos de datos soportados**:
- String, Number, Boolean
- Timestamp, Date
- Array, Object
- Null
- Reference (referencia a otro documento)

### Ejemplo de Estructura

```
ecommerce_db (Database)
├── usuarios (Collection)
│   ├── user_123 (Document)
│   │   ├── nombre: "Juan"
│   │   ├── email: "juan@example.com"
│   │   └── compras (Subcollection)
│   │       └── compra_1 (Document)
│   └── user_456 (Document)
└── productos (Collection)
    └── prod_1 (Document)
```

---

## 3. Configuración en la Consola de Firebase

### Paso 1: Crear el Proyecto

1. Ve a [Firebase Console](https://console.firebase.google.com)
2. Click en **"Agregar proyecto"** o **"Create a project"**
3. **Nombre del proyecto**: Asigna un nombre único (ej: "mi-ecommerce")
4. **Google Analytics** (opcional): Puedes desactivarlo para pruebas rápidas
5. Click en **"Crear proyecto"**
6. Espera a que se cree (unos segundos)

### Paso 2: Registrar la Aplicación y Obtener Credenciales

1. En la pantalla principal, haz click en el ícono de **Web `</>`**
2. **App nickname**: Registra un apodo para la app (ej: "mi-app-web")
3. **Firebase Hosting** (opcional): Puedes activarlo después
4. Click en **"Registrar app"**
5. **Copia el objeto `firebaseConfig`**. Contiene las claves necesarias:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyC...",
  authDomain: "tu-proyecto.firebaseapp.com",
  projectId: "tu-proyecto",
  storageBucket: "tu-proyecto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc"
};
```

**⚠️ IMPORTANTE**: Guarda estas credenciales, las necesitarás para configurar tu proyecto.

### Paso 3: Habilitar Firestore

1. En el menú lateral, ve a **"Compilación"** > **"Firestore Database"**
2. Click en **"Crear base de datos"**
3. **Modo de seguridad**:
   - **"Iniciar en modo producción"**: Requiere reglas de seguridad (recomendado)
   - **"Iniciar en modo de prueba"**: Permite lectura/escritura por 30 días (solo desarrollo)
4. **Ubicación**: Selecciona la ubicación del servidor más cercana
   - **us-central** (Iowa, USA)
   - **southamerica-east1** (São Paulo, Brasil) - Recomendado para Argentina
5. Click en **"Habilitar"**

### Paso 4: Reglas de Seguridad

Para desarrollo, debes permitir el acceso de lectura/escritura. Ve a la pestaña **"Reglas"** y publica lo siguiente:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

**⚠️ IMPORTANTE**: Estas reglas permiten acceso completo. En producción, debes restringir el acceso según tus necesidades.

Click en **"Publicar"** para guardar las reglas.

---

## 4. Configuración en el Proyecto

### 4.1. Instalar SDK

```bash
npm install firebase
```

### 4.2. Variables de Entorno (`.env`)

Crea un archivo `.env` en la raíz del proyecto:

```env
FIREBASE_API_KEY=AIzaSyC...
FIREBASE_AUTH_DOMAIN=tu-proyecto.firebaseapp.com
FIREBASE_PROJECT_ID=tu-proyecto
FIREBASE_STORAGE_BUCKET=tu-proyecto.appspot.com
FIREBASE_MESSAGING_SENDER_ID=123456789
FIREBASE_APP_ID=1:123456789:web:abc
```

**⚠️ IMPORTANTE**: 
- No subas `.env` a Git
- Agrega `.env` a `.gitignore`

### 4.3. Archivo de Inicialización (`firebase.js`)

Crea un archivo para inicializar Firebase:

```javascript
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import dotenv from 'dotenv';

dotenv.config();

const firebaseConfig = {
    apiKey: process.env.FIREBASE_API_KEY,
    authDomain: process.env.FIREBASE_AUTH_DOMAIN,
    projectId: process.env.FIREBASE_PROJECT_ID,
    storageBucket: process.env.FIREBASE_STORAGE_BUCKET,
    messagingSenderId: process.env.FIREBASE_MESSAGING_SENDER_ID,
    appId: process.env.FIREBASE_APP_ID
};

// Inicializar Firebase
const app = initializeApp(firebaseConfig);

// Obtener instancia de Firestore
export const dbFirebase = getFirestore(app);

// Exportar app si la necesitas
export default app;
```

**⚠️ IMPORTANTE**: Inicializa Firebase una sola vez en tu aplicación.

---

## 5. Operaciones CRUD Completas

### Importaciones Necesarias

```javascript
import { 
    collection,
    addDoc,
    getDoc,
    getDocs,
    doc,
    updateDoc,
    deleteDoc,
    query,
    where,
    orderBy,
    limit,
    startAfter,
    Timestamp
} from 'firebase/firestore';
import { dbFirebase } from './firebase.js';
```

### Create (Crear)

#### Crear Documento con ID Automático

```javascript
// Crear documento en colección "usuarios"
const docRef = await addDoc(collection(dbFirebase, "usuarios"), {
    nombre: "Juan",
    email: "juan@example.com",
    edad: 30,
    fechaCreacion: Timestamp.now()
});

console.log("Documento creado con ID:", docRef.id);
```

#### Crear Documento con ID Personalizado

```javascript
import { setDoc } from 'firebase/firestore';

// Crear documento con ID específico
await setDoc(doc(dbFirebase, "usuarios", "user_123"), {
    nombre: "Juan",
    email: "juan@example.com",
    edad: 30
});
```

### Read (Leer)

#### Leer un Documento por ID

```javascript
// Obtener documento por ID
const docRef = doc(dbFirebase, "usuarios", "user_123");
const docSnap = await getDoc(docRef);

if (docSnap.exists()) {
    console.log("Datos:", docSnap.data());
    console.log("ID:", docSnap.id);
} else {
    console.log("Documento no existe");
}
```

#### Leer Todos los Documentos de una Colección

```javascript
// Obtener todos los documentos
const querySnapshot = await getDocs(collection(dbFirebase, "usuarios"));

querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

#### Leer con Filtros

```javascript
// Consulta con filtro
const q = query(
    collection(dbFirebase, "usuarios"),
    where("edad", ">", 18),
    where("activo", "==", true)
);

const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

#### Leer Ordenado

```javascript
// Consulta ordenada
const q = query(
    collection(dbFirebase, "usuarios"),
    orderBy("fechaCreacion", "desc"),
    limit(10)
);

const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

### Update (Actualizar)

#### Actualizar Documento

```javascript
// Actualizar campos específicos
const docRef = doc(dbFirebase, "usuarios", "user_123");

await updateDoc(docRef, {
    nombre: "Juan Pérez",
    edad: 31
});

console.log("Documento actualizado");
```

#### Actualizar con Merge

```javascript
import { setDoc } from 'firebase/firestore';

// Actualizar con merge (no sobrescribe campos existentes)
await setDoc(
    doc(dbFirebase, "usuarios", "user_123"),
    { nombre: "Juan Pérez" },
    { merge: true }
);
```

### Delete (Eliminar)

```javascript
// Eliminar documento
const docRef = doc(dbFirebase, "usuarios", "user_123");
await deleteDoc(docRef);

console.log("Documento eliminado");
```

### Ejemplo Completo: CRUD de Usuarios

```javascript
import { 
    collection, addDoc, getDocs, doc, getDoc, updateDoc, deleteDoc,
    query, where, orderBy
} from 'firebase/firestore';
import { dbFirebase } from './firebase.js';

// Crear usuario
export const crearUsuario = async (datosUsuario) => {
    try {
        const docRef = await addDoc(collection(dbFirebase, "usuarios"), {
            ...datosUsuario,
            fechaCreacion: Timestamp.now()
        });
        return { id: docRef.id, ...datosUsuario };
    } catch (error) {
        console.error("Error al crear usuario:", error);
        throw error;
    }
};

// Obtener usuario por ID
export const obtenerUsuario = async (userId) => {
    try {
        const docRef = doc(dbFirebase, "usuarios", userId);
        const docSnap = await getDoc(docRef);
        
        if (docSnap.exists()) {
            return { id: docSnap.id, ...docSnap.data() };
        } else {
            return null;
        }
    } catch (error) {
        console.error("Error al obtener usuario:", error);
        throw error;
    }
};

// Obtener todos los usuarios
export const obtenerUsuarios = async () => {
    try {
        const querySnapshot = await getDocs(collection(dbFirebase, "usuarios"));
        return querySnapshot.docs.map(doc => ({
            id: doc.id,
            ...doc.data()
        }));
    } catch (error) {
        console.error("Error al obtener usuarios:", error);
        throw error;
    }
};

// Actualizar usuario
export const actualizarUsuario = async (userId, datosActualizados) => {
    try {
        const docRef = doc(dbFirebase, "usuarios", userId);
        await updateDoc(docRef, datosActualizados);
        return { id: userId, ...datosActualizados };
    } catch (error) {
        console.error("Error al actualizar usuario:", error);
        throw error;
    }
};

// Eliminar usuario
export const eliminarUsuario = async (userId) => {
    try {
        const docRef = doc(dbFirebase, "usuarios", userId);
        await deleteDoc(docRef);
        return true;
    } catch (error) {
        console.error("Error al eliminar usuario:", error);
        throw error;
    }
};
```

---

## 6. Consultas Avanzadas

### Múltiples Filtros

```javascript
// Consulta con múltiples condiciones
const q = query(
    collection(dbFirebase, "productos"),
    where("precio", ">", 100),
    where("categoria", "==", "electronica"),
    where("disponible", "==", true),
    orderBy("precio", "asc"),
    limit(20)
);

const querySnapshot = await getDocs(q);
```

### Paginación

```javascript
// Primera página
const first = query(
    collection(dbFirebase, "productos"),
    orderBy("precio"),
    limit(10)
);

const documentSnapshots = await getDocs(first);
const lastVisible = documentSnapshots.docs[documentSnapshots.docs.length - 1];

// Siguiente página
const next = query(
    collection(dbFirebase, "productos"),
    orderBy("precio"),
    startAfter(lastVisible),
    limit(10)
);

const nextSnapshot = await getDocs(next);
```

### Consultas en Tiempo Real

```javascript
import { onSnapshot } from 'firebase/firestore';

// Escuchar cambios en tiempo real
const q = query(collection(dbFirebase, "usuarios"));

const unsubscribe = onSnapshot(q, (querySnapshot) => {
    querySnapshot.forEach((doc) => {
        console.log("Cambio detectado:", doc.id, doc.data());
    });
});

// Para dejar de escuchar
// unsubscribe();
```

---

## 7. Reglas de Seguridad

Las reglas de seguridad controlan quién puede leer y escribir en Firestore.

### Reglas Básicas

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Permitir lectura/escritura a todos (solo desarrollo)
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

### Reglas por Colección

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Usuarios: solo lectura pública, escritura autenticada
    match /usuarios/{userId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    // Productos: lectura pública, escritura solo admin
    match /productos/{productId} {
      allow read: if true;
      allow write: if request.auth != null && 
                      get(/databases/$(database)/documents/usuarios/$(request.auth.uid)).data.rol == 'admin';
    }
  }
}
```

### Reglas con Validación

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /usuarios/{userId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && 
                       request.resource.data.keys().hasAll(['nombre', 'email']) &&
                       request.resource.data.nombre is string &&
                       request.resource.data.email is string;
      allow update: if request.auth != null && 
                       request.auth.uid == userId;
      allow delete: if request.auth != null && 
                       request.auth.uid == userId;
    }
  }
}
```

---

## 8. Troubleshooting

### Error: "Missing or insufficient permissions"

**Causa**: Las reglas de seguridad están bloqueando la petición.

**Solución**:
1. Verifica las reglas en Firebase Console
2. Asegúrate de que las reglas permitan la operación que intentas hacer
3. Para desarrollo, puedes usar reglas permisivas temporalmente

### Error: "The query requires an index"

**Causa**: Firestore requiere índices para consultas complejas (unir `where` y `orderBy`).

**Solución**:
1. Firestore te dará un link en el error para crear el índice automáticamente
2. Click en el link y crea el índice
3. Espera a que el índice se cree (unos minutos)

### Error: "Firebase App already exists"

**Causa**: Intentas inicializar Firebase múltiples veces.

**Solución**:
```javascript
// Verificar si ya está inicializado
import { getApps, initializeApp } from 'firebase/app';

if (getApps().length === 0) {
    const app = initializeApp(firebaseConfig);
}
```

### Error: "Permission denied"

**Causa**: Las reglas de seguridad no permiten la operación.

**Solución**:
1. Revisa las reglas en Firebase Console
2. Verifica que el usuario tenga los permisos necesarios
3. Si usas autenticación, asegúrate de que el usuario esté autenticado

---

## 9. Buenas Prácticas

### Estructura de Datos

- ✅ **Organizar por colecciones**: Usa colecciones para diferentes tipos de datos
- ✅ **IDs descriptivos**: Usa IDs que tengan significado cuando sea posible
- ✅ **Evitar anidamiento profundo**: Máximo 2-3 niveles de subcolecciones
- ✅ **Normalizar cuando sea necesario**: Evita duplicación de datos

### Performance

- ✅ **Índices**: Crea índices para consultas frecuentes
- ✅ **Límites**: Usa `limit()` para limitar resultados
- ✅ **Paginación**: Implementa paginación para grandes colecciones
- ✅ **Cache**: Usa cache local cuando sea apropiado

### Seguridad

- ✅ **Reglas estrictas**: En producción, usa reglas restrictivas
- ✅ **Validación**: Valida datos antes de escribir
- ✅ **Autenticación**: Usa autenticación para operaciones sensibles
- ✅ **No exponer credenciales**: Nunca subas credenciales a Git

### Código

- ✅ **Manejo de errores**: Siempre maneja errores con try/catch
- ✅ **Validación**: Valida datos antes de escribir
- ✅ **Tipos**: Usa TypeScript cuando sea posible
- ✅ **Modularización**: Separa lógica de Firestore en módulos

---

