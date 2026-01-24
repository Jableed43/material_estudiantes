# Master Guide: AWS S3 para Almacenamiento de Imágenes ☁️📸

## 📑 Índice
1. [Introducción a AWS S3](#1-introducción-a-aws-s3)
2. [Configuración del Bucket S3](#2-configuración-del-bucket-s3)
3. [Configuración de Permisos y Seguridad](#3-configuración-de-permisos-y-seguridad)
4. [Configuración de Usuario IAM](#4-configuración-de-usuario-iam)
5. [Instalación y Configuración en el Proyecto](#5-instalación-y-configuración-en-el-proyecto)
6. [Subir Imágenes a S3](#6-subir-imágenes-a-s3)
7. [Obtener y Eliminar Imágenes](#7-obtener-y-eliminar-imágenes)
8. [URLs Firmadas (Signed URLs)](#8-urls-firmadas-signed-urls)
9. [Integración con Express.js](#9-integración-con-expressjs)
10. [Troubleshooting](#10-troubleshooting)
11. [Buenas Prácticas](#11-buenas-prácticas)

---

## 1. Introducción a AWS S3

### ¿Qué es AWS S3? (Analogía del Mundo Real)

### ☁️ Analogía: El Almacén en la Nube

Imagina que necesitas guardar cosas:
- **S3**: Es como un almacén gigante en la nube
- **Bucket**: Es como una sección del almacén (puedes tener múltiples secciones)
- **Objetos**: Son las cosas que guardas (imágenes, videos, archivos)

**S3 es como un almacén ilimitado** donde puedes guardar cualquier cosa (imágenes, videos, archivos).

### 📦 Analogía: El Depósito de Archivos

Piensa en un depósito de archivos:
- **S3**: Es el depósito completo
- **Bucket**: Es como una carpeta en el depósito
- **Objetos**: Son los archivos que guardas

**S3 es como un depósito gigante** donde puedes guardar todos tus archivos de forma segura.

### 🗄️ Analogía: El Archivo Digital

Un archivo digital:
- **S3**: Es el archivo completo
- **Bucket**: Es como una gaveta del archivo
- **Objetos**: Son los documentos que guardas

**S3 es como un archivo digital ilimitado** en la nube donde puedes guardar cualquier archivo.

**Amazon S3 (Simple Storage Service)** es un servicio de almacenamiento de objetos escalable que permite almacenar y recuperar cualquier cantidad de datos desde cualquier lugar.

**En términos simples**: S3 es como un "almacén en la nube" donde puedes guardar imágenes, videos y archivos de forma segura y accesible desde cualquier lugar.

### ¿Qué es S3?

S3 es un servicio de almacenamiento en la nube que ofrece:
- ✅ **Almacenamiento ilimitado**: Almacena cualquier cantidad de datos
- ✅ **Alta disponibilidad**: 99.999999999% (11 nueves) de durabilidad
- ✅ **Escalable**: Crece automáticamente con tus necesidades
- ✅ **Seguro**: Control de acceso granular
- ✅ **Económico**: Pago solo por lo que usas
- ✅ **CDN integrado**: CloudFront para distribución global

### ¿Por qué usar S3 para Imágenes?

- ✅ **No sobrecarga el servidor**: Las imágenes no están en tu servidor
- ✅ **Escalable**: Maneja millones de imágenes sin problemas
- ✅ **Rápido**: CDN global para entrega rápida
- ✅ **Económico**: Muy barato para almacenar archivos estáticos
- ✅ **Seguro**: Control de acceso granular

### Conceptos Clave

- **Bucket**: Contenedor para almacenar objetos (similar a una carpeta)
- **Object (Objeto)**: Archivo almacenado en S3 (imagen, video, etc.)
- **Key**: Nombre/identificador único del objeto dentro del bucket
- **Region**: Ubicación geográfica del bucket
- **IAM**: Servicio de gestión de identidades y accesos

---

## 2. Configuración del Bucket S3

### Paso 1: Crear el Bucket en AWS S3

1. **Acceder a AWS Console**:
   - Ve a [aws.amazon.com](https://aws.amazon.com)
   - Inicia sesión o crea una cuenta
   - Ve a **AWS Console**

2. **Navegar a S3**:
   - En la barra de búsqueda, escribe "S3"
   - Click en **"S3"** en los resultados

3. **Crear Bucket**:
   - Click en **"Create bucket"** o **"Crear bucket"**

4. **Configuración Básica**:
   - **Bucket name**: `tu-nombre-bucket-productos` (debe ser único globalmente)
     - Solo minúsculas, números y guiones
     - Entre 3 y 63 caracteres
     - Ejemplo: `mi-ecommerce-imagenes-2024`
   - **AWS Region**: Selecciona la región más cercana
     - `us-east-1` (N. Virginia) - Recomendado para USA
     - `sa-east-1` (São Paulo) - Recomendado para Argentina/Brasil
     - `eu-west-1` (Irlanda) - Recomendado para Europa

5. **Configuración de Objetos**:
   - **Object Ownership**: "ACLs disabled" (recomendado)
   - **Block Public Access settings**: Por ahora, desactiva temporalmente para pruebas:
     - Desmarca todas las opciones (solo para desarrollo)
     - ⚠️ En producción, activa estas opciones y usa URLs firmadas

6. **Versionamiento** (opcional):
   - Puedes activar versionamiento para mantener historial de archivos

7. **Tags** (opcional):
   - Agrega tags para organización (ej: `Environment: Development`)

8. **Encriptación** (recomendado):
   - **Server-side encryption**: "Enable"
   - **Encryption type**: "Amazon S3 managed keys (SSE-S3)"

9. **Click en "Create bucket"**

### Paso 2: Configuración Adicional del Bucket

#### Configurar Propiedades

1. Click en el nombre del bucket creado
2. Ve a la pestaña **"Properties"**
3. **Static website hosting** (opcional): Si quieres servir archivos estáticos
4. **Logging** (opcional): Activar logs de acceso

---

## 3. Configuración de Permisos y Seguridad

### A. Política de Bucket (Bucket Policy)

La **Bucket Policy** controla quién puede acceder a los objetos del bucket.

1. Ve a tu bucket → **"Permissions"** → **"Bucket policy"**
2. Click en **"Edit"**
3. Agrega la siguiente política:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::tu-bucket-name/*"
    }
  ]
}
```

**⚠️ IMPORTANTE**: 
- Reemplaza `tu-bucket-name` con el nombre real de tu bucket
- Esta política hace el bucket público (solo lectura)
- En producción, usa URLs firmadas en lugar de hacer el bucket público

**Explicación**:
- `Principal: "*"`: Permite acceso a todos (público)
- `Action: "s3:GetObject"`: Permite leer objetos
- `Resource`: Especifica qué objetos (el `/*` significa todos)

4. Click en **"Save changes"**

### B. CORS Configuration

**CORS (Cross-Origin Resource Sharing)** permite que tu frontend acceda a las imágenes desde S3.

1. Ve a **"Permissions"** → **"Cross-origin resource sharing (CORS)"**
2. Click en **"Edit"**
3. Agrega la siguiente configuración:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

**Para producción**, restringe los orígenes:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
    "AllowedOrigins": [
      "https://tu-dominio.com",
      "https://www.tu-dominio.com",
      "http://localhost:3000"
    ],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

**Explicación**:
- `AllowedHeaders`: Headers permitidos en las peticiones
- `AllowedMethods`: Métodos HTTP permitidos
- `AllowedOrigins`: Dominios que pueden hacer peticiones
- `ExposeHeaders`: Headers que el navegador puede leer
- `MaxAgeSeconds`: Tiempo que el navegador cachea la configuración CORS

4. Click en **"Save changes"**

### C. Block Public Access (Opcional pero Recomendado)

Para mayor seguridad, puedes bloquear el acceso público y usar URLs firmadas:

1. Ve a **"Permissions"** → **"Block public access"**
2. Activa todas las opciones
3. Esto requiere usar URLs firmadas para acceder a las imágenes

---

## 4. Configuración de Usuario IAM

**IAM (Identity and Access Management)** gestiona quién puede acceder a qué recursos de AWS.

### A. Crear Usuario IAM

1. **Navegar a IAM**:
   - En AWS Console, busca "IAM"
   - Click en **"IAM"**

2. **Crear Usuario**:
   - Click en **"Users"** en el menú lateral
   - Click en **"Create user"**

3. **Configuración del Usuario**:
   - **User name**: `s3-product-images-user` (o el nombre que prefieras)
   - **Access type**: Selecciona **"Programmatic access"**
     - Esto permite acceso mediante API/CLI
   - Click en **"Next: Permissions"**

### B. Crear Política Personalizada

1. **Crear Política**:
   - Click en **"Policies"** en el menú lateral
   - Click en **"Create policy"**
   - Click en la pestaña **"JSON"**

2. **Agregar Política**:
   - Reemplaza el contenido con:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:PutObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::tu-bucket-name/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::tu-bucket-name"
      ]
    }
  ]
}
```

**⚠️ IMPORTANTE**: Reemplaza `tu-bucket-name` con el nombre real de tu bucket.

**Explicación**:
- **Primer Statement**: Permite operaciones en objetos (Get, Put, Delete)
- **Segundo Statement**: Permite listar objetos en el bucket
- `Resource`: Especifica el bucket y sus objetos

3. **Revisar y Crear**:
   - Click en **"Next: Tags"** (opcional)
   - Click en **"Next: Review"**
   - **Policy name**: `S3ProductImagesPolicy` (o el nombre que prefieras)
   - **Description**: "Permite acceso a bucket de imágenes de productos"
   - Click en **"Create policy"**

### C. Asignar Política al Usuario

1. **Volver a Usuarios**:
   - Ve a **"Users"** → Selecciona el usuario creado

2. **Agregar Permisos**:
   - Click en **"Add permissions"**
   - Selecciona **"Attach policies directly"**
   - Busca la política que creaste (`S3ProductImagesPolicy`)
   - Márcala y click en **"Next"**
   - Click en **"Add permissions"**

### D. Obtener Credenciales

1. **Crear Access Key**:
   - Con el usuario seleccionado, ve a la pestaña **"Security credentials"**
   - Click en **"Create access key"**

2. **Seleccionar Use Case**:
   - Selecciona **"Application running outside AWS"**
   - Click en **"Next"**

3. **Descripción** (opcional):
   - Agrega una descripción: "Para aplicación Node.js"

4. **Crear y Guardar**:
   - Click en **"Create access key"**
   - **⚠️ IMPORTANTE**: Guarda las credenciales ahora (solo se muestran una vez):
     - **Access Key ID**: `AKIAIOSFODNN7EXAMPLE`
     - **Secret Access Key**: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

5. **Descargar** (recomendado):
   - Click en **"Download .csv file"** para guardar las credenciales de forma segura

**⚠️ SEGURIDAD**: 
- Nunca subas estas credenciales a Git
- No las compartas públicamente
- Si se comprometen, elimínalas y crea nuevas

---

## 5. Instalación y Configuración en el Proyecto

### Paso 1: Instalar SDK de AWS

```bash
npm install @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
```

**Paquetes**:
- `@aws-sdk/client-s3`: Cliente para interactuar con S3
- `@aws-sdk/s3-request-presigner`: Para generar URLs firmadas

### Paso 2: Configurar Variables de Entorno

Actualiza tu archivo `.env`:

```env
# AWS Configuration
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
AWS_S3_BUCKET_NAME=tu-bucket-name
```

**⚠️ IMPORTANTE**:
- Reemplaza con tus credenciales reales
- Agrega `.env` a `.gitignore`
- Nunca subas `.env` a Git

### Paso 3: Crear Cliente S3

Crea un archivo `config/s3.js`:

```javascript
import { S3Client } from '@aws-sdk/client-s3';
import dotenv from 'dotenv';

dotenv.config();

const s3Client = new S3Client({
  region: process.env.AWS_REGION,
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  },
});

export default s3Client;
```

### Paso 4: Verificar Configuración

Crea un archivo de prueba `test-s3.js`:

```javascript
import { ListBucketsCommand } from '@aws-sdk/client-s3';
import s3Client from './config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

async function testS3Connection() {
  try {
    const command = new ListBucketsCommand({});
    const response = await s3Client.send(command);
    
    console.log('✅ S3 Connection successful!');
    console.log('Available buckets:', response.Buckets.map(b => b.Name));
    
    // Verificar que nuestro bucket existe
    const bucketExists = response.Buckets.some(
      b => b.Name === process.env.AWS_S3_BUCKET_NAME
    );
    
    if (bucketExists) {
      console.log(`✅ Bucket "${process.env.AWS_S3_BUCKET_NAME}" found!`);
    } else {
      console.log(`⚠️ Bucket "${process.env.AWS_S3_BUCKET_NAME}" not found`);
    }
  } catch (error) {
    console.error('❌ S3 Connection failed:', error.message);
    console.error('Error details:', error);
  }
}

testS3Connection();
```

**Ejecutar**:
```bash
node test-s3.js
```

**Salida esperada**:
```
✅ S3 Connection successful!
Available buckets: [ 'tu-bucket-name', ... ]
✅ Bucket "tu-bucket-name" found!
```

---

## 6. Subir Imágenes a S3

### Función Básica para Subir

Crea un archivo `services/s3Service.js`:

```javascript
import { PutObjectCommand } from '@aws-sdk/client-s3';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Sube un archivo a S3
 * @param {Buffer} fileBuffer - Buffer del archivo
 * @param {string} fileName - Nombre del archivo
 * @param {string} mimetype - Tipo MIME del archivo (ej: 'image/jpeg')
 * @returns {Promise<string>} URL del archivo subido
 */
export const uploadFileToS3 = async (fileBuffer, fileName, mimetype) => {
  try {
    const command = new PutObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: fileName,
      Body: fileBuffer,
      ContentType: mimetype,
      // ACL: 'public-read' // Solo si el bucket es público
    });

    await s3Client.send(command);

    // Construir URL del archivo
    const fileUrl = `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_REGION}.amazonaws.com/${fileName}`;
    
    return fileUrl;
  } catch (error) {
    console.error('Error uploading file to S3:', error);
    throw error;
  }
};
```

### Subir con Multer (Express.js)

Si usas `multer` para manejar uploads:

```javascript
import multer from 'multer';
import { uploadFileToS3 } from '../services/s3Service.js';

// Configurar multer para memoria (no guarda en disco)
const storage = multer.memoryStorage();
const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB
  },
  fileFilter: (req, file, cb) => {
    // Solo permitir imágenes
    if (file.mimetype.startsWith('image/')) {
      cb(null, true);
    } else {
      cb(new Error('Solo se permiten imágenes'), false);
    }
  },
});

// Middleware para una sola imagen
export const uploadSingle = upload.single('image');

// Controlador
export const uploadImageController = async (req, res) => {
  try {
    if (!req.file) {
      return res.status(400).json({ error: 'No se proporcionó imagen' });
    }

    // Generar nombre único para el archivo
    const fileName = `productos/${Date.now()}-${req.file.originalname}`;

    // Subir a S3
    const fileUrl = await uploadFileToS3(
      req.file.buffer,
      fileName,
      req.file.mimetype
    );

    res.json({
      message: 'Imagen subida exitosamente',
      url: fileUrl,
    });
  } catch (error) {
    console.error('Error uploading image:', error);
    res.status(500).json({ error: 'Error al subir imagen' });
  }
};
```

### Ruta en Express

```javascript
import express from 'express';
import { uploadSingle, uploadImageController } from '../controllers/imageController.js';

const router = express.Router();

router.post('/upload', uploadSingle, uploadImageController);

export default router;
```

### Ejemplo Completo: Subir Imagen de Producto

```javascript
// services/productImageService.js
import { PutObjectCommand } from '@aws-sdk/client-s3';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';
import crypto from 'crypto';

dotenv.config();

/**
 * Sube imagen de producto a S3
 * @param {Buffer} fileBuffer - Buffer de la imagen
 * @param {string} originalName - Nombre original del archivo
 * @param {string} mimetype - Tipo MIME
 * @param {string} productId - ID del producto
 * @returns {Promise<{url: string, key: string}>}
 */
export const uploadProductImage = async (
  fileBuffer,
  originalName,
  mimetype,
  productId
) => {
  try {
    // Generar nombre único
    const fileExtension = originalName.split('.').pop();
    const randomName = crypto.randomBytes(16).toString('hex');
    const fileName = `productos/${productId}/${randomName}.${fileExtension}`;

    // Subir a S3
    const command = new PutObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: fileName,
      Body: fileBuffer,
      ContentType: mimetype,
      // Metadata opcional
      Metadata: {
        'product-id': productId,
        'uploaded-at': new Date().toISOString(),
      },
    });

    await s3Client.send(command);

    // Construir URL
    const fileUrl = `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_REGION}.amazonaws.com/${fileName}`;

    return {
      url: fileUrl,
      key: fileName, // Guardar key para poder eliminar después
    };
  } catch (error) {
    console.error('Error uploading product image:', error);
    throw error;
  }
};
```

---

## 7. Obtener y Eliminar Imágenes

### Obtener Imagen

```javascript
import { GetObjectCommand } from '@aws-sdk/client-s3';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Obtener objeto de S3
 * @param {string} key - Key del objeto en S3
 * @returns {Promise<Buffer>} Buffer del archivo
 */
export const getFileFromS3 = async (key) => {
  try {
    const command = new GetObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: key,
    });

    const response = await s3Client.send(command);
    
    // Convertir stream a buffer
    const chunks = [];
    for await (const chunk of response.Body) {
      chunks.push(chunk);
    }
    
    return Buffer.concat(chunks);
  } catch (error) {
    console.error('Error getting file from S3:', error);
    throw error;
  }
};
```

### Eliminar Imagen

```javascript
import { DeleteObjectCommand } from '@aws-sdk/client-s3';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Eliminar archivo de S3
 * @param {string} key - Key del objeto en S3
 * @returns {Promise<boolean>}
 */
export const deleteFileFromS3 = async (key) => {
  try {
    const command = new DeleteObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: key,
    });

    await s3Client.send(command);
    return true;
  } catch (error) {
    console.error('Error deleting file from S3:', error);
    throw error;
  }
};
```

### Listar Objetos

```javascript
import { ListObjectsV2Command } from '@aws-sdk/client-s3';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Listar objetos en un prefijo (carpeta)
 * @param {string} prefix - Prefijo (ej: 'productos/')
 * @returns {Promise<Array>} Lista de objetos
 */
export const listFilesFromS3 = async (prefix = '') => {
  try {
    const command = new ListObjectsV2Command({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Prefix: prefix,
    });

    const response = await s3Client.send(command);
    return response.Contents || [];
  } catch (error) {
    console.error('Error listing files from S3:', error);
    throw error;
  }
};
```

---

## 8. URLs Firmadas (Signed URLs)

Las **URLs firmadas** permiten acceso temporal a objetos privados sin hacer el bucket público.

### Generar URL Firmada para Lectura

```javascript
import { GetObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Generar URL firmada para leer un objeto
 * @param {string} key - Key del objeto
 * @param {number} expiresIn - Segundos hasta que expire (default: 3600 = 1 hora)
 * @returns {Promise<string>} URL firmada
 */
export const generateSignedUrl = async (key, expiresIn = 3600) => {
  try {
    const command = new GetObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: key,
    });

    const signedUrl = await getSignedUrl(s3Client, command, { expiresIn });
    return signedUrl;
  } catch (error) {
    console.error('Error generating signed URL:', error);
    throw error;
  }
};
```

### Generar URL Firmada para Escritura (Upload Directo)

```javascript
import { PutObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';

dotenv.config();

/**
 * Generar URL firmada para subir un objeto
 * @param {string} key - Key del objeto
 * @param {string} contentType - Tipo MIME
 * @param {number} expiresIn - Segundos hasta que expire
 * @returns {Promise<string>} URL firmada
 */
export const generateUploadUrl = async (key, contentType, expiresIn = 3600) => {
  try {
    const command = new PutObjectCommand({
      Bucket: process.env.AWS_S3_BUCKET_NAME,
      Key: key,
      ContentType: contentType,
    });

    const signedUrl = await getSignedUrl(s3Client, command, { expiresIn });
    return signedUrl;
  } catch (error) {
    console.error('Error generating upload URL:', error);
    throw error;
  }
};
```

### Uso en Controlador

```javascript
// controllers/imageController.js
import { generateSignedUrl, generateUploadUrl } from '../services/s3Service.js';

// Obtener URL firmada para leer imagen
export const getImageSignedUrl = async (req, res) => {
  try {
    const { key } = req.params;
    const signedUrl = await generateSignedUrl(key, 3600); // 1 hora
    
    res.json({ url: signedUrl });
  } catch (error) {
    res.status(500).json({ error: 'Error generating signed URL' });
  }
};

// Obtener URL firmada para subir imagen
export const getUploadSignedUrl = async (req, res) => {
  try {
    const { fileName, contentType } = req.body;
    const key = `productos/${Date.now()}-${fileName}`;
    
    const signedUrl = await generateUploadUrl(key, contentType, 3600);
    
    res.json({ 
      uploadUrl: signedUrl,
      key: key,
      // URL final después de subir
      finalUrl: `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_REGION}.amazonaws.com/${key}`
    });
  } catch (error) {
    res.status(500).json({ error: 'Error generating upload URL' });
  }
};
```

---

## 9. Integración con Express.js

### Estructura Completa del Proyecto

```
proyecto/
├── config/
│   └── s3.js              # Cliente S3
├── services/
│   └── s3Service.js       # Funciones S3
├── controllers/
│   └── imageController.js # Controladores
├── routes/
│   └── imageRoutes.js     # Rutas
├── middleware/
│   └── upload.js          # Multer config
└── .env                   # Variables de entorno
```

### Servicio Completo de S3

```javascript
// services/s3Service.js
import {
  PutObjectCommand,
  GetObjectCommand,
  DeleteObjectCommand,
  ListObjectsV2Command,
} from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import s3Client from '../config/s3.js';
import dotenv from 'dotenv';
import crypto from 'crypto';

dotenv.config();

export class S3Service {
  static async uploadFile(fileBuffer, fileName, mimetype, folder = '') {
    try {
      const key = folder ? `${folder}/${fileName}` : fileName;
      
      const command = new PutObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET_NAME,
        Key: key,
        Body: fileBuffer,
        ContentType: mimetype,
      });

      await s3Client.send(command);

      const fileUrl = `https://${process.env.AWS_S3_BUCKET_NAME}.s3.${process.env.AWS_REGION}.amazonaws.com/${key}`;
      return { url: fileUrl, key };
    } catch (error) {
      console.error('Error uploading file:', error);
      throw error;
    }
  }

  static async deleteFile(key) {
    try {
      const command = new DeleteObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET_NAME,
        Key: key,
      });

      await s3Client.send(command);
      return true;
    } catch (error) {
      console.error('Error deleting file:', error);
      throw error;
    }
  }

  static async generateSignedUrl(key, expiresIn = 3600) {
    try {
      const command = new GetObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET_NAME,
        Key: key,
      });

      return await getSignedUrl(s3Client, command, { expiresIn });
    } catch (error) {
      console.error('Error generating signed URL:', error);
      throw error;
    }
  }

  static generateUniqueFileName(originalName) {
    const fileExtension = originalName.split('.').pop();
    const randomName = crypto.randomBytes(16).toString('hex');
    return `${randomName}.${fileExtension}`;
  }
}
```

### Controlador Completo

```javascript
// controllers/imageController.js
import { S3Service } from '../services/s3Service.js';
import multer from 'multer';

const storage = multer.memoryStorage();
export const upload = multer({
  storage,
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
  fileFilter: (req, file, cb) => {
    if (file.mimetype.startsWith('image/')) {
      cb(null, true);
    } else {
      cb(new Error('Solo se permiten imágenes'), false);
    }
  },
});

export const uploadImage = async (req, res) => {
  try {
    if (!req.file) {
      return res.status(400).json({ error: 'No se proporcionó imagen' });
    }

    const fileName = S3Service.generateUniqueFileName(req.file.originalname);
    const { url, key } = await S3Service.uploadFile(
      req.file.buffer,
      fileName,
      req.file.mimetype,
      'productos'
    );

    res.json({ url, key });
  } catch (error) {
    res.status(500).json({ error: 'Error al subir imagen' });
  }
};

export const deleteImage = async (req, res) => {
  try {
    const { key } = req.params;
    await S3Service.deleteFile(key);
    res.json({ message: 'Imagen eliminada exitosamente' });
  } catch (error) {
    res.status(500).json({ error: 'Error al eliminar imagen' });
  }
};

export const getSignedUrl = async (req, res) => {
  try {
    const { key } = req.params;
    const url = await S3Service.generateSignedUrl(key);
    res.json({ url });
  } catch (error) {
    res.status(500).json({ error: 'Error al generar URL' });
  }
};
```

### Rutas

```javascript
// routes/imageRoutes.js
import express from 'express';
import { uploadImage, deleteImage, getSignedUrl, upload } from '../controllers/imageController.js';

const router = express.Router();

router.post('/upload', upload.single('image'), uploadImage);
router.delete('/:key', deleteImage);
router.get('/signed-url/:key', getSignedUrl);

export default router;
```

---

## 10. Troubleshooting

### Error: "Access Denied"

**Causa**: Las credenciales o permisos son incorrectos.

**Soluciones**:
1. Verifica que las credenciales en `.env` sean correctas
2. Verifica que la política IAM tenga los permisos necesarios
3. Verifica que el nombre del bucket sea correcto
4. Verifica que estés en la región correcta

### Error: "Bucket not found"

**Causa**: El nombre del bucket es incorrecto o estás en la región incorrecta.

**Soluciones**:
1. Verifica el nombre del bucket en AWS Console
2. Verifica que `AWS_REGION` en `.env` coincida con la región del bucket
3. Verifica que el bucket exista

### Error: "CORS"

**Causa**: La configuración CORS no permite el origen desde el que haces la petición.

**Soluciones**:
1. Verifica la configuración CORS del bucket
2. Asegúrate de que `AllowedOrigins` incluya tu dominio
3. Para desarrollo, puedes usar `"*"` temporalmente

### Error: "InvalidAccessKeyId"

**Causa**: El Access Key ID es incorrecto o no existe.

**Soluciones**:
1. Verifica que el Access Key ID sea correcto
2. Verifica que el usuario IAM tenga una access key activa
3. Crea una nueva access key si es necesario

### Error: "SignatureDoesNotMatch"

**Causa**: El Secret Access Key es incorrecto.

**Soluciones**:
1. Verifica que el Secret Access Key sea correcto
2. Asegúrate de no tener espacios extra en `.env`
3. Crea una nueva access key si es necesario

### Error: "RequestEntityTooLarge"

**Causa**: El archivo es demasiado grande.

**Soluciones**:
1. Verifica el límite en multer: `limits: { fileSize: 5 * 1024 * 1024 }`
2. Aumenta el límite si es necesario
3. Considera comprimir la imagen antes de subirla

---

## 11. Buenas Prácticas

### Seguridad

- ✅ **Nunca subir credenciales a Git**: Usa variables de entorno
- ✅ **Usar URLs firmadas**: En producción, usa URLs firmadas en lugar de hacer el bucket público
- ✅ **Restringir CORS**: Solo permite orígenes necesarios
- ✅ **Validar archivos**: Verifica tipo y tamaño antes de subir
- ✅ **Nombres únicos**: Usa nombres únicos para evitar sobrescritura

### Performance

- ✅ **Comprimir imágenes**: Comprime imágenes antes de subir
- ✅ **Tamaños múltiples**: Genera thumbnails y diferentes tamaños
- ✅ **CDN**: Usa CloudFront para distribución global
- ✅ **Lazy loading**: Carga imágenes bajo demanda

### Organización

- ✅ **Estructura de carpetas**: Organiza por tipo (productos, usuarios, etc.)
- ✅ **Nombres descriptivos**: Usa nombres que indiquen el contenido
- ✅ **Metadata**: Agrega metadata útil a los objetos
- ✅ **Versionamiento**: Activa versionamiento para backups

### Costos

- ✅ **Lifecycle policies**: Configura políticas para eliminar archivos antiguos
- ✅ **Storage classes**: Usa clases de almacenamiento apropiadas
- ✅ **Monitoreo**: Monitorea el uso y costos en AWS Console

---

## 📚 Checklist de Configuración

- [ ] ✅ Bucket creado en S3
- [ ] ✅ Política de bucket configurada
- [ ] ✅ CORS configurado
- [ ] ✅ Usuario IAM creado
- [ ] ✅ Política IAM creada y asignada
- [ ] ✅ Access Key creada y guardada
- [ ] ✅ Credenciales guardadas en `.env`
- [ ] ✅ `.env` agregado a `.gitignore`
- [ ] ✅ SDK de AWS instalado
- [ ] ✅ Cliente S3 configurado
- [ ] ✅ Conexión probada con `test-s3.js`
- [ ] ✅ Funciones de upload/delete implementadas
- [ ] ✅ Integración con Express.js completa

---

