# Master Guide: Handlebars y Sistema MVC 🟢

## 📑 Índice
1. [Introducción: API vs Server](#1-introducción-api-vs-server)
2. [¿Qué es Handlebars?](#2-qué-es-handlebars)
3. [Arquitectura MVC](#3-arquitectura-mvc)
4. [Instalación y Configuración](#4-instalación-y-configuración)
5. [Sintaxis de Handlebars](#5-sintaxis-de-handlebars)
6. [Layouts (Plantillas Base)](#6-layouts-plantillas-base)
7. [Partials (Componentes Reutilizables)](#7-partials-componentes-reutilizables)
8. [Helpers (Funciones Personalizadas)](#8-helpers-funciones-personalizadas)
9. [Renderizar Vistas desde Controladores](#9-renderizar-vistas-desde-controladores)
10. [Formularios y Redirecciones](#10-formularios-y-redirecciones)
11. [Sesiones y Autenticación](#11-sesiones-y-autenticación)
12. [Archivos Estáticos](#12-archivos-estáticos)
13. [Sistema CRUD Completo](#13-sistema-crud-completo)
14. [Buenas Prácticas](#14-buenas-prácticas)
15. [Troubleshooting](#15-troubleshooting)

---

## 1. Introducción: API vs Server

### 🔵 API (Tema Anterior)

Una **API REST** es un backend que **solo retorna JSON**, sin vistas HTML.

**Características**:
- ✅ **Solo backend**: Retorna JSON, sin vistas HTML
- ✅ **Stack**: Express.js + Mongoose
- ✅ **Uso**: Consumida por frontend (React, Vue, etc.)
- ✅ **Respuestas**: Siempre JSON
- ✅ **CORS**: Configurado para permitir peticiones del frontend
- ✅ **Sin Handlebars**: No renderiza HTML

**Ejemplo de respuesta**:
```javascript
// GET /api/usuarios
res.json({ usuarios: [...] });
```

### 🟢 Server (Este Tema)

Un **Server con Handlebars** es un backend que **renderiza HTML** usando templates.

**Características**:
- ✅ **Backend con vistas**: Renderiza HTML usando Handlebars
- ✅ **Stack**: Express.js + Mongoose + Handlebars
- ✅ **Uso**: Aplicación web completa con interfaz visual
- ✅ **Respuestas**: HTML renderizado
- ✅ **Sin CORS**: No necesario (mismo origen)
- ✅ **Handlebars**: Template engine para renderizar HTML

**Ejemplo de respuesta**:
```javascript
// GET /usuarios
res.render('user/list', { users: [...] });
```

### Comparación Visual

| Aspecto | API (JSON) | Server (Handlebars) |
|---------|------------|---------------------|
| **Respuesta** | JSON | HTML |
| **Frontend** | React/Vue separado | HTML integrado |
| **CORS** | Necesario | No necesario |
| **Template Engine** | No usa | Handlebars |
| **Formularios** | JavaScript (fetch) | HTML nativo |
| **Redirecciones** | No aplica | `res.redirect()` |

### ¿Cuándo Usar Cada Uno?

**Usa API (JSON)** cuando:
- ✅ Tienes frontend separado (React, Vue, Angular)
- ✅ Quieres separar frontend y backend completamente
- ✅ Necesitas que múltiples clientes consuman la API (web, móvil, etc.)

**Usa Server (Handlebars)** cuando:
- ✅ Quieres una aplicación web tradicional
- ✅ Prefieres renderizar HTML en el servidor
- ✅ No necesitas un frontend separado
- ✅ Quieres SEO mejorado (HTML completo)

---

## 2. ¿Qué es Handlebars? (Analogía del Mundo Real)

### 📝 Analogía: La Plantilla de Carta

Imagina que tienes una plantilla de carta:
- **Plantilla**: "Estimado {{nombre}}, tu pedido {{numero}} está listo..."
- **Datos**: `{ nombre: "Juan", numero: "123" }`
- **Resultado**: "Estimado Juan, tu pedido 123 está listo..."

**Handlebars funciona igual**: Tienes una plantilla HTML con "huecos" ({{variable}}) que se llenan con datos.

### 🏠 Analogía: La Casa con Espacios para Muebles

Piensa en una casa con espacios designados:
- **Plantilla (Handlebars)**: La casa con espacios marcados (aquí va el sofá, aquí la mesa)
- **Datos**: Los muebles reales que vas a colocar
- **Resultado**: La casa completa con los muebles en su lugar

**Handlebars define "espacios"** en tu HTML donde van los datos.

### 📋 Analogía: El Formulario con Campos

Un formulario:
- **Plantilla**: El formulario con campos vacíos (Nombre: _____, Email: _____)
- **Datos**: La información que llenas
- **Resultado**: El formulario completo con la información

**Handlebars es como un formulario HTML** que se llena automáticamente con datos del servidor.

### ¿Qué es Handlebars?

**Handlebars** es un motor de plantillas (template engine) que permite generar HTML dinámico desde el servidor.

**En términos simples**: Es como tener una plantilla HTML con "huecos" que se llenan automáticamente con datos del servidor.

### Conceptos Clave

- **Template (Plantilla)**: Archivo `.handlebars` o `.hbs` con HTML y sintaxis Handlebars
- **Renderizar**: Proceso de convertir template + datos → HTML final
- **Variables**: `{{variable}}` - Muestra el valor de una variable
- **Helpers**: Funciones personalizadas para lógica en templates
- **Partials**: Componentes reutilizables (header, footer, etc.)
- **Layouts**: Plantillas base que envuelven otras vistas

### Ventajas de Handlebars

- ✅ **Sintaxis simple**: Fácil de aprender
- ✅ **Separación de concerns**: HTML separado de lógica
- ✅ **Reutilizable**: Partials y layouts
- ✅ **Seguro**: Escapa HTML automáticamente
- ✅ **Extensible**: Helpers personalizados

### Ejemplo Básico

**Template** (`user.handlebars`):
```handlebars
<h1>{{name}}</h1>
<p>Email: {{email}}</p>
```

**Renderizar**:
```javascript
res.render('user', { name: 'Juan', email: 'juan@example.com' });
```

**HTML Resultante**:
```html
<h1>Juan</h1>
<p>Email: juan@example.com</p>
```

---

## 3. Arquitectura MVC

**MVC (Model-View-Controller)** es un patrón de arquitectura que separa la aplicación en tres componentes:

### Componentes del MVC

#### Model (Modelo)
**Responsabilidad**: Estructura de datos y acceso a la base de datos.

**Ubicación**: `src/models/`

**Ejemplo**:
```javascript
// src/models/userModel.js
import mongoose from 'mongoose';

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, required: true },
  password: { type: String, required: true }
});

export default mongoose.model('User', userSchema);
```

#### View (Vista)
**Responsabilidad**: Lo que ve el usuario (templates Handlebars).

**Ubicación**: `src/views/`

**Ejemplo**:
```handlebars
<!-- src/views/user/list.handlebars -->
<h1>Lista de Usuarios</h1>
{{#each users}}
  <p>{{name}} - {{email}}</p>
{{/each}}
```

#### Controller (Controlador)
**Responsabilidad**: Lógica de la aplicación, conecta Model y View.

**Ubicación**: `src/controllers/`

**Ejemplo**:
```javascript
// src/controllers/userController.js
export const getAllUsers = async (req, res) => {
  const users = await User.find();
  res.render('user/list', { users });
};
```

### Flujo Completo de una Petición

```
1. Usuario hace clic en enlace
   ↓
2. Ruta (Route) recibe la petición
   ↓
3. Controlador (Controller) procesa
   ↓
4. Servicio (Service) ejecuta lógica de negocio
   ↓
5. Modelo (Model) interactúa con base de datos
   ↓
6. Datos vuelven al Controlador
   ↓
7. Controlador renderiza Vista (View) con datos
   ↓
8. Usuario ve HTML renderizado
```

### Estructura de Carpetas MVC

```
proyecto/
├── src/
│   ├── models/          # 📊 MODELOS - Estructura de datos
│   │   ├── userModel.js
│   │   ├── categoryModel.js
│   │   └── productModel.js
│   ├── views/           # 🎨 VISTAS - Templates Handlebars
│   │   ├── layouts/     # Layouts principales
│   │   │   └── main.handlebars
│   │   ├── user/        # Vistas de usuario
│   │   │   ├── list.handlebars
│   │   │   ├── create.handlebars
│   │   │   └── edit.handlebars
│   │   ├── category/    # Vistas de categoría
│   │   └── product/      # Vistas de producto
│   ├── controllers/     # 🎮 CONTROLADORES - Lógica
│   │   ├── userController.js
│   │   ├── categoryController.js
│   │   └── productController.js
│   ├── services/        # 🔧 SERVICIOS - Lógica de negocio
│   │   ├── userService.js
│   │   ├── categoryService.js
│   │   └── productService.js
│   ├── routes/          # 🛣️ RUTAS - URLs
│   │   ├── userRoute.js
│   │   ├── categoryRoute.js
│   │   └── productRoute.js
│   ├── middlewares/     # Middlewares (auth, etc.)
│   ├── helpers/         # Helpers de Handlebars
│   ├── public/          # Archivos estáticos (CSS, JS, imágenes)
│   └── utils/           # Utilidades
└── index.js             # Servidor principal
```

### Ejemplo Completo: Flujo CRUD

**1. Usuario llena formulario** (`/user/create`):
```handlebars
<!-- Vista: user/create.handlebars -->
<form action="/user/create" method="POST">
  <input type="text" name="name" required>
  <input type="email" name="email" required>
  <button type="submit">Crear</button>
</form>
```

**2. Ruta recibe POST**:
```javascript
// routes/userRoute.js
router.post('/create', createUser);
```

**3. Controlador procesa**:
```javascript
// controllers/userController.js
export const createUser = async (req, res) => {
  try {
    const { name, email } = req.body;
    await createUserService({ name, email });
    res.redirect('/user/getAll');
  } catch (error) {
    res.render('user/create', { error: error.message });
  }
};
```

**4. Servicio ejecuta lógica**:
```javascript
// services/userService.js
export const createUserService = async (data) => {
  const user = new User(data);
  return await user.save();
};
```

**5. Modelo guarda en DB**:
```javascript
// models/userModel.js - Ya definido arriba
```

---

## 4. Instalación y Configuración

### Paso 1: Instalar Dependencias

```bash
npm install express-handlebars express body-parser express-session method-override
```

**Paquetes**:
- `express-handlebars`: Motor de plantillas Handlebars
- `express`: Framework web
- `body-parser`: Parsear datos de formularios
- `express-session`: Manejo de sesiones
- `method-override`: Permitir PUT/DELETE desde formularios HTML

### Paso 2: Configurar Express con Handlebars

**`index.js`**:
```javascript
import express from 'express';
import { engine } from 'express-handlebars';
import bodyParser from 'body-parser';
import session from 'express-session';
import methodOverride from 'method-override';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();

// ===== CONFIGURACIÓN DE HANDLEBARS =====

// Configurar motor de plantillas
app.engine('handlebars', engine({
  // Opciones de Handlebars
  runtimeOptions: {
    allowProtoPropertiesByDefault: true,
    allowProtoMethodsByDefault: true
  },
  // Layout por defecto
  defaultLayout: 'main',
  // Directorio de layouts
  layoutsDir: path.join(__dirname, 'src/views/layouts'),
  // Directorio de partials
  partialsDir: path.join(__dirname, 'src/views/partials'),
  // Extensión de archivos
  extname: '.handlebars'
}));

// Establecer Handlebars como motor de vistas
app.set('view engine', 'handlebars');

// Directorio donde están las vistas
app.set('views', path.join(__dirname, 'src/views'));

// ===== MIDDLEWARES =====

// Permitir métodos PUT y DELETE desde formularios HTML
app.use(methodOverride('_method'));

// Parsear JSON
app.use(bodyParser.json());

// Parsear datos de formularios (urlencoded)
app.use(bodyParser.urlencoded({ extended: true }));

// Configurar sesiones
app.use(session({
  secret: process.env.SECRET || 'mi-secreto-super-seguro',
  resave: false,
  saveUninitialized: false,
  cookie: {
    maxAge: 1000 * 60 * 60 * 24 // 24 horas
  }
}));

// Archivos estáticos (CSS, JS, imágenes)
app.use(express.static(path.join(__dirname, 'src/public')));

// ===== RUTAS =====

app.get('/', (req, res) => {
  res.render('home', { title: 'Inicio' });
});

// ===== INICIAR SERVIDOR =====

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Paso 3: Crear Estructura de Carpetas

```bash
mkdir -p src/views/layouts
mkdir -p src/views/partials
mkdir -p src/views/user
mkdir -p src/views/category
mkdir -p src/views/product
mkdir -p src/public/css
mkdir -p src/public/js
mkdir -p src/public/images
```

### Paso 4: Crear Layout Principal

**`src/views/layouts/main.handlebars`**:
```handlebars
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{title}} - Sistema de Gestión</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <nav>
        <a href="/">Inicio</a>
        <a href="/user/getAll">Usuarios</a>
        <a href="/category/getAll">Categorías</a>
        <a href="/product/getAll">Productos</a>
    </nav>

    {{#if message}}
        <div class="message">{{message}}</div>
    {{/if}}

    <!-- Contenido de la vista se inserta aquí -->
    {{{body}}}
</body>
</html>
```

**Explicación**:
- `{{title}}`: Variable que se pasa desde el controlador
- `{{{body}}}`: Triple llave para insertar HTML sin escapar
- `{{#if message}}`: Bloque condicional
- `{{> partial}}`: Incluir partial (ver más adelante)

---

## 5. Sintaxis de Handlebars

### Variables

**Sintaxis**: `{{variable}}`

**Ejemplo**:
```handlebars
<h1>{{name}}</h1>
<p>Email: {{email}}</p>
```

**Renderizar**:
```javascript
res.render('user', { name: 'Juan', email: 'juan@example.com' });
```

**Resultado**:
```html
<h1>Juan</h1>
<p>Email: juan@example.com</p>
```

### Escapado de HTML

- `{{variable}}`: Escapa HTML (seguro)
- `{{{variable}}}`: No escapa HTML (usar con cuidado)

**Ejemplo**:
```handlebars
{{name}}        <!-- Si name = "<script>alert('xss')</script>" -->
                <!-- Resultado: &lt;script&gt;alert('xss')&lt;/script&gt; -->

{{{html}}}      <!-- Si html = "<strong>Texto</strong>" -->
                <!-- Resultado: <strong>Texto</strong> -->
```

### Bloques Condicionales

#### `{{#if}}` / `{{else}}` / `{{/if}}`

```handlebars
{{#if user}}
  <p>Hola, {{user.name}}</p>
{{else}}
  <p>No hay usuario</p>
{{/if}}
```

#### `{{#unless}}` (opuesto de if)

```handlebars
{{#unless user}}
  <p>Por favor inicia sesión</p>
{{/unless}}
```

### Bloques de Iteración

#### `{{#each}}` / `{{/each}}`

```handlebars
<ul>
  {{#each users}}
    <li>{{name}} - {{email}}</li>
  {{/each}}
</ul>
```

**Con índice**:
```handlebars
{{#each users}}
  <p>{{@index}}: {{name}}</p>
{{/each}}
```

**Primero y último**:
```handlebars
{{#each users}}
  {{#if @first}}
    <p>Primero: {{name}}</p>
  {{/if}}
  {{#if @last}}
    <p>Último: {{name}}</p>
  {{/if}}
{{/each}}
```

**Con contexto padre**:
```handlebars
{{#each users}}
  <p>{{name}}</p>
  {{#each products}}
    <p>Producto: {{name}} (Usuario: {{../name}})</p>
  {{/each}}
{{/each}}
```

### Comentarios

```handlebars
{{! Este es un comentario que no aparece en el HTML }}
<!-- Este comentario SÍ aparece en el HTML -->
```

---

## 6. Layouts (Plantillas Base)

Los **Layouts** son plantillas base que envuelven otras vistas. Permiten tener una estructura común (header, footer, navegación) para todas las páginas.

### Crear Layout Principal

**`src/views/layouts/main.handlebars`**:
```handlebars
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{title}} - Sistema de Gestión</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <!-- Navegación -->
    <nav>
        <a href="/">Inicio</a>
        <a href="/user/getAll">Usuarios</a>
        <a href="/category/getAll">Categorías</a>
        <a href="/product/getAll">Productos</a>
        
        {{#if session.userId}}
            <span>Hola, {{session.userName}}</span>
            <a href="/user/logout">Cerrar Sesión</a>
        {{else}}
            <a href="/user/login">Iniciar Sesión</a>
            <a href="/user/create">Registrarse</a>
        {{/if}}
    </nav>

    <!-- Mensajes de sesión -->
    {{#if message}}
        <div class="message {{#if success}}success{{else}}error{{/if}}">
            {{message}}
        </div>
    {{/if}}

    <!-- Contenido principal (se inserta aquí la vista) -->
    <main>
        {{{body}}}
    </main>

    <!-- Footer -->
    <footer>
        <p>&copy; 2024 Sistema de Gestión</p>
    </footer>

    <script src="/js/app.js"></script>
</body>
</html>
```

### Usar Layout en Vista

**Vista** (`src/views/user/list.handlebars`):
```handlebars
<h1>Lista de Usuarios</h1>
{{#each users}}
  <p>{{name}}</p>
{{/each}}
```

**Renderizar**:
```javascript
res.render('user/list', { 
  title: 'Lista de Usuarios',
  users: users 
});
```

**Resultado**: La vista se inserta en `{{{body}}}` del layout.

### Layouts Múltiples

Puedes tener múltiples layouts:

```javascript
// Layout por defecto
app.engine('handlebars', engine({
  defaultLayout: 'main',
  layoutsDir: path.join(__dirname, 'src/views/layouts')
}));
```

**Usar layout diferente**:
```javascript
res.render('admin/dashboard', { 
  layout: 'admin',  // Usa layouts/admin.handlebars
  title: 'Dashboard',
  data: data
});
```

**Sin layout**:
```javascript
res.render('email/template', { 
  layout: false,  // No usa layout
  content: content
});
```

---

## 7. Partials (Componentes Reutilizables)

Los **Partials** son componentes reutilizables que puedes incluir en múltiples vistas.

### Crear Partial

**`src/views/partials/header.handlebars`**:
```handlebars
<header>
    <nav>
        <a href="/">Inicio</a>
        <a href="/user/getAll">Usuarios</a>
        <a href="/category/getAll">Categorías</a>
    </nav>
</header>
```

**`src/views/partials/footer.handlebars`**:
```handlebars
<footer>
    <p>&copy; 2024 Sistema de Gestión</p>
    <p>Contacto: info@example.com</p>
</footer>
```

### Usar Partial

**Sintaxis**: `{{> nombrePartial}}`

**En Layout**:
```handlebars
<!DOCTYPE html>
<html>
<head>
    <title>{{title}}</title>
</head>
<body>
    {{> header}}    <!-- Incluye partials/header.handlebars -->
    
    {{{body}}}
    
    {{> footer}}    <!-- Incluye partials/footer.handlebars -->
</body>
</html>
```

### Pasar Datos a Partials

**Partial** (`partials/userCard.handlebars`):
```handlebars
<div class="user-card">
    <h3>{{name}}</h3>
    <p>{{email}}</p>
</div>
```

**Usar con datos**:
```handlebars
{{#each users}}
    {{> userCard name=name email=email}}
{{/each}}
```

**O pasar objeto completo**:
```handlebars
{{#each users}}
    {{> userCard this}}
{{/each}}
```

### Partials Comunes

**`partials/formField.handlebars`**:
```handlebars
<div class="form-field">
    <label for="{{id}}">{{label}}</label>
    <input 
        type="{{type}}" 
        id="{{id}}" 
        name="{{name}}" 
        value="{{value}}"
        {{#if required}}required{{/if}}
    >
    {{#if error}}
        <span class="error">{{error}}</span>
    {{/if}}
</div>
```

**Usar**:
```handlebars
{{> formField 
    id="name" 
    label="Nombre" 
    name="name" 
    type="text" 
    value=user.name 
    required=true
}}
```

---

## 8. Helpers (Funciones Personalizadas)

Los **Helpers** son funciones personalizadas que extienden las capacidades de Handlebars.

### Registrar Helpers

**`src/utils/helpers.js`**:
```javascript
import Handlebars from 'handlebars';

export const registerHelpers = (Handlebars) => {
    
    // Helper para comparar valores (ESENCIAL)
    Handlebars.registerHelper('eq', function (a, b) {
        return a === b;
    });

    // Helper para convertir a string (ESENCIAL para ObjectId)
    Handlebars.registerHelper('toString', function (value) {
        return value ? value.toString() : '';
    });

    // Helper para lógica AND
    Handlebars.registerHelper('and', function (a, b) {
        return a && b;
    });

    // Helper para lógica OR
    Handlebars.registerHelper('or', function (a, b) {
        return a || b;
    });

    // Helper para formatear fecha
    Handlebars.registerHelper('formatDate', function (date) {
        if (!date) return '';
        return new Date(date).toLocaleDateString('es-ES', {
            year: 'numeric',
            month: 'long',
            day: 'numeric'
        });
    });

    // Helper para formatear moneda
    Handlebars.registerHelper('formatCurrency', function (amount) {
        return new Intl.NumberFormat('es-AR', {
            style: 'currency',
            currency: 'ARS'
        }).format(amount);
    });

    // Helper para comparar mayor que
    Handlebars.registerHelper('gt', function (a, b) {
        return a > b;
    });

    // Helper para comparar menor que
    Handlebars.registerHelper('lt', function (a, b) {
        return a < b;
    });

    // Helper para capitalizar
    Handlebars.registerHelper('capitalize', function (str) {
        if (!str) return '';
        return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
    });

    // Helper para truncar texto
    Handlebars.registerHelper('truncate', function (str, len) {
        if (!str || str.length <= len) return str;
        return str.substring(0, len) + '...';
    });
};
```

### Registrar Helpers en Express

**`index.js`**:
```javascript
import { registerHelpers } from './src/utils/helpers.js';
import Handlebars from 'handlebars';

// Registrar helpers
registerHelpers(Handlebars);

// Configurar Handlebars
app.engine('handlebars', engine({
  // ... otras opciones
}));
```

### Usar Helpers en Vistas

**Comparar valores**:
```handlebars
{{#if (eq user.role "admin")}}
  <p>Eres administrador</p>
{{/if}}

{{#if (eq (toString user._id) (toString currentUserId))}}
  <p>Este es tu perfil</p>
{{/if}}
```

**Formatear datos**:
```handlebars
<p>Fecha: {{formatDate user.createdAt}}</p>
<p>Precio: {{formatCurrency product.price}}</p>
<p>Nombre: {{capitalize user.name}}</p>
<p>Descripción: {{truncate product.description 50}}</p>
```

**Comparaciones**:
```handlebars
{{#if (gt user.age 18)}}
  <p>Es mayor de edad</p>
{{/if}}

{{#if (and user.isActive user.hasPermission)}}
  <p>Acceso permitido</p>
{{/if}}
```

### Helpers con Múltiples Parámetros

```javascript
// Helper con múltiples parámetros
Handlebars.registerHelper('ifEquals', function (arg1, arg2, options) {
    return (arg1 === arg2) ? options.fn(this) : options.inverse(this);
});
```

**Usar**:
```handlebars
{{#ifEquals user.role "admin"}}
  <p>Panel de administrador</p>
{{else}}
  <p>Panel de usuario</p>
{{/ifEquals}}
```

---

## 9. Renderizar Vistas desde Controladores

### Método `res.render()`

**Sintaxis**: `res.render('ruta/vista', { datos })`

**Ejemplo básico**:
```javascript
export const homeView = (req, res) => {
  res.render('home', { 
    title: 'Inicio' 
  });
};
```

### Pasar Datos a la Vista

**Controlador**:
```javascript
export const getAllUsersView = async (req, res) => {
  try {
    const users = await getUsersService();
    
    res.render('user/getAllUsers', {
      title: 'Lista de Usuarios',
      users: users,
      message: req.session.message || null,
      session: req.session
    });
  } catch (error) {
    res.render('user/getAllUsers', {
      title: 'Lista de Usuarios',
      users: [],
      message: 'Error al cargar usuarios'
    });
  }
};
```

**Vista** (`user/getAllUsers.handlebars`):
```handlebars
<h1>{{title}}</h1>

{{#if message}}
  <div class="message">{{message}}</div>
{{/if}}

{{#if users.length}}
  <table>
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Email</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      {{#each users}}
      <tr>
        <td>{{name}}</td>
        <td>{{email}}</td>
        <td>
          <a href="/user/update/{{_id}}">Editar</a>
          <a href="/user/delete/{{_id}}">Eliminar</a>
        </td>
      </tr>
      {{/each}}
    </tbody>
  </table>
{{else}}
  <p>No hay usuarios registrados</p>
{{/if}}
```

### Renderizar con Layout Diferente

```javascript
res.render('admin/dashboard', {
  layout: 'admin',  // Usa layouts/admin.handlebars
  title: 'Dashboard',
  data: data
});
```

### Renderizar sin Layout

```javascript
res.render('email/template', {
  layout: false,  // No usa layout
  content: content
});
```

### Manejo de Errores

```javascript
export const getUserView = async (req, res) => {
  try {
    const user = await getUserService(req.params.id);
    
    if (!user) {
      return res.status(404).render('error', {
        title: 'Error 404',
        message: 'Usuario no encontrado'
      });
    }
    
    res.render('user/detail', {
      title: 'Detalle de Usuario',
      user: user
    });
  } catch (error) {
    res.status(500).render('error', {
      title: 'Error',
      message: error.message
    });
  }
};
```

---

## 10. Formularios y Redirecciones

### Formularios HTML

En Handlebars, los formularios son HTML estándar.

#### Formulario de Creación

**Vista** (`user/createUser.handlebars`):
```handlebars
<h2>Crear Usuario</h2>

<form action="/user/create" method="POST">
    <div>
        <label for="name">Nombre:</label>
        <input type="text" id="name" name="name" required>
    </div>
    
    <div>
        <label for="lastName">Apellido:</label>
        <input type="text" id="lastName" name="lastName" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
    </div>
    
    <div>
        <label for="age">Edad:</label>
        <input type="number" id="age" name="age" required>
    </div>
    
    <div>
        <label for="password">Contraseña:</label>
        <input type="password" id="password" name="password" required>
    </div>
    
    <button type="submit" class="btn btn-success">Crear Usuario</button>
    <a href="/user/getAll" class="btn">Cancelar</a>
</form>
```

#### Formulario de Edición

**Vista** (`user/updateUser.handlebars`):
```handlebars
<h2>Editar Usuario</h2>

<form action="/user/update/{{user._id}}?_method=PUT" method="POST">
    <div>
        <label for="name">Nombre:</label>
        <input type="text" id="name" name="name" value="{{user.name}}" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" value="{{user.email}}" required>
    </div>
    
    <button type="submit" class="btn">Actualizar</button>
    <a href="/user/getAll" class="btn">Cancelar</a>
</form>
```

**⚠️ IMPORTANTE**: Usa `?_method=PUT` para métodos PUT/DELETE (requiere `method-override`).

### Controladores para Formularios

#### Crear (POST)

```javascript
export const createUser = async (req, res) => {
  try {
    // Los datos vienen en req.body
    const { name, lastName, email, age, password } = req.body;
    
    // Llamar al servicio
    await createUserService({ name, lastName, email, age, password });
    
    // Guardar mensaje de éxito en sesión
    req.session.message = 'Usuario creado exitosamente';
    
    // Redirigir a la lista
    res.redirect('/user/getAll');
  } catch (error) {
    // Si hay error, guardar mensaje y redirigir al formulario
    req.session.message = error.message;
    res.redirect('/user/create');
  }
};
```

#### Actualizar (PUT)

```javascript
export const updateUser = async (req, res) => {
  try {
    const userId = req.params.id;
    const { name, email, age } = req.body;
    
    await updateUserService(userId, { name, email, age });
    
    req.session.message = 'Usuario actualizado exitosamente';
    res.redirect('/user/getAll');
  } catch (error) {
    req.session.message = error.message;
    res.redirect(`/user/update/${req.params.id}`);
  }
};
```

#### Eliminar (DELETE)

**Vista con formulario de eliminación**:
```handlebars
<form action="/user/delete/{{_id}}?_method=DELETE" method="POST" style="display: inline;">
    <button type="submit" class="btn btn-danger"
            onclick="return confirm('¿Estás seguro de eliminar a {{name}}?')">
        Eliminar
    </button>
</form>
```

**Controlador**:
```javascript
export const deleteUser = async (req, res) => {
  try {
    const userId = req.params.id;
    await deleteUserService(userId);
    
    req.session.message = 'Usuario eliminado exitosamente';
    res.redirect('/user/getAll');
  } catch (error) {
    req.session.message = error.message;
    res.redirect('/user/getAll');
  }
};
```

### Redirecciones

**Sintaxis**: `res.redirect('/ruta')`

**Ejemplos**:
```javascript
// Redirigir después de crear
res.redirect('/user/getAll');

// Redirigir con mensaje en sesión
req.session.message = 'Operación exitosa';
res.redirect('/user/getAll');

// Redirigir a página anterior
res.redirect('back');

// Redirigir a URL externa
res.redirect('https://example.com');
```

### Mensajes en Sesión

**Guardar mensaje**:
```javascript
req.session.message = 'Usuario creado exitosamente';
```

**Mostrar mensaje en vista**:
```handlebars
{{#if message}}
  <div class="message">{{message}}</div>
{{/if}}
```

**Limpiar mensaje después de mostrarlo**:
```javascript
// En el controlador, después de renderizar
req.session.message = null;
```

---

## 11. Sesiones y Autenticación

### Configurar Sesiones

**`index.js`**:
```javascript
import session from 'express-session';

app.use(session({
  secret: process.env.SECRET || 'mi-secreto-super-seguro',
  resave: false,
  saveUninitialized: false,
  cookie: {
    maxAge: 1000 * 60 * 60 * 24, // 24 horas
    secure: false, // true en producción con HTTPS
    httpOnly: true // Previene acceso desde JavaScript
  }
}));
```

### Login (Autenticación)

**Vista de Login** (`user/login.handlebars`):
```handlebars
<h2>Iniciar Sesión</h2>

{{#if message}}
  <div class="message error">{{message}}</div>
{{/if}}

<form action="/user/validate" method="POST">
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
    </div>
    
    <div>
        <label for="password">Contraseña:</label>
        <input type="password" id="password" name="password" required>
    </div>
    
    <button type="submit" class="btn">Iniciar Sesión</button>
</form>
```

**Controlador de Login**:
```javascript
export const validate = async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // Validar credenciales
    const result = await validateUserService(email, password);
    
    if (result.success) {
      // Guardar en sesión
      req.session.userId = result.userId;
      req.session.userName = result.userName;
      req.session.userRole = result.userRole;
      
      req.session.message = `Bienvenido, ${result.userName}`;
      res.redirect('/');
    } else {
      req.session.message = 'Credenciales incorrectas';
      res.redirect('/user/login');
    }
  } catch (error) {
    req.session.message = error.message;
    res.redirect('/user/login');
  }
};
```

### Logout

```javascript
export const logout = (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      console.error('Error al cerrar sesión:', err);
    }
    res.redirect('/');
  });
};
```

### Middleware de Autenticación

**`src/middlewares/authMiddleware.js`**:
```javascript
export const authMiddleware = (req, res, next) => {
  if (req.session.userId) {
    // Usuario autenticado, continuar
    next();
  } else {
    // Usuario no autenticado, redirigir al login
    req.session.message = 'Debes iniciar sesión para acceder';
    res.redirect('/user/login');
  }
};
```

**Usar middleware**:
```javascript
import { authMiddleware } from './src/middlewares/authMiddleware.js';

// Proteger ruta
router.get('/dashboard', authMiddleware, dashboardView);

// Proteger todas las rutas de un router
router.use(authMiddleware);
```

### Verificar Autenticación en Vistas

**Layout** (`layouts/main.handlebars`):
```handlebars
<nav>
    {{#if session.userId}}
        <span>Hola, {{session.userName}}</span>
        <a href="/user/logout">Cerrar Sesión</a>
    {{else}}
        <a href="/user/login">Iniciar Sesión</a>
        <a href="/user/create">Registrarse</a>
    {{/if}}
</nav>
```

**Pasar sesión a vistas**:
```javascript
// Middleware para pasar sesión a todas las vistas
app.use((req, res, next) => {
  res.locals.session = req.session;
  next();
});
```

---

## 12. Archivos Estáticos

Los **archivos estáticos** (CSS, JS, imágenes) se sirven directamente sin procesamiento.

### Configurar Archivos Estáticos

**`index.js`**:
```javascript
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Servir archivos estáticos
app.use(express.static(path.join(__dirname, 'src/public')));
```

### Estructura de Carpetas

```
src/
└── public/
    ├── css/
    │   └── style.css
    ├── js/
    │   └── app.js
    └── images/
        └── logo.png
```

### Usar en Vistas

**CSS**:
```handlebars
<link rel="stylesheet" href="/css/style.css">
```

**JavaScript**:
```handlebars
<script src="/js/app.js"></script>
```

**Imágenes**:
```handlebars
<img src="/images/logo.png" alt="Logo">
```

### Rutas de Archivos Estáticos

Con la configuración anterior:
- `/css/style.css` → `src/public/css/style.css`
- `/js/app.js` → `src/public/js/app.js`
- `/images/logo.png` → `src/public/images/logo.png`

**⚠️ IMPORTANTE**: La ruta en el HTML **no incluye** `public`, solo `/css/`, `/js/`, etc.

---

## 13. Sistema CRUD Completo

### Estructura de Rutas

**`src/routes/userRoute.js`**:
```javascript
import express from 'express';
import {
  createUserView,
  createUser,
  getAllUsersView,
  updateUserView,
  updateUser,
  deleteUser,
  loginView,
  validate,
  logout
} from '../controllers/userController.js';

const router = express.Router();

// VISTAS (GET) - Mostrar formularios y listas
router.get('/create', createUserView);
router.get('/getAll', getAllUsersView);
router.get('/update/:id', updateUserView);
router.get('/login', loginView);

// ACCIONES (POST, PUT, DELETE) - Procesar formularios
router.post('/create', createUser);
router.post('/update/:id', updateUser);
router.post('/delete/:id', deleteUser);
router.post('/validate', validate);
router.get('/logout', logout);

export default router;
```

### Controlador Completo

**`src/controllers/userController.js`**:
```javascript
import {
  createUserService,
  getUsersService,
  getUserService,
  updateUserService,
  deleteUserService,
  validateUserService
} from '../services/userService.js';

// ===== VISTAS (GET) =====

export const createUserView = (req, res) => {
  res.render('user/createUser', {
    title: 'Registrar Usuario',
    message: req.session.message || null
  });
  req.session.message = null; // Limpiar mensaje
};

export const getAllUsersView = async (req, res) => {
  try {
    const users = await getUsersService();
    res.render('user/getAllUsers', {
      title: 'Lista de Usuarios',
      users: users,
      message: req.session.message || null
    });
    req.session.message = null;
  } catch (error) {
    res.render('user/getAllUsers', {
      title: 'Lista de Usuarios',
      users: [],
      message: 'Error al cargar usuarios'
    });
  }
};

export const updateUserView = async (req, res) => {
  try {
    const userId = req.params.id;
    const user = await getUserService(userId);
    
    if (!user) {
      req.session.message = 'Usuario no encontrado';
      return res.redirect('/user/getAll');
    }
    
    res.render('user/updateUser', {
      title: 'Editar Usuario',
      user: user,
      message: req.session.message || null
    });
    req.session.message = null;
  } catch (error) {
    req.session.message = 'Error al cargar usuario';
    res.redirect('/user/getAll');
  }
};

export const loginView = (req, res) => {
  res.render('user/login', {
    title: 'Iniciar Sesión',
    message: req.session.message || null
  });
  req.session.message = null;
};

// ===== ACCIONES (POST, PUT, DELETE) =====

export const createUser = async (req, res) => {
  try {
    await createUserService(req.body);
    req.session.message = 'Usuario creado exitosamente';
    res.redirect('/user/getAll');
  } catch (error) {
    req.session.message = error.message;
    res.redirect('/user/create');
  }
};

export const updateUser = async (req, res) => {
  try {
    const userId = req.params.id;
    await updateUserService(userId, req.body);
    req.session.message = 'Usuario actualizado exitosamente';
    res.redirect('/user/getAll');
  } catch (error) {
    req.session.message = error.message;
    res.redirect(`/user/update/${userId}`);
  }
};

export const deleteUser = async (req, res) => {
  try {
    const userId = req.params.id;
    await deleteUserService(userId);
    req.session.message = 'Usuario eliminado exitosamente';
    res.redirect('/user/getAll');
  } catch (error) {
    req.session.message = error.message;
    res.redirect('/user/getAll');
  }
};

export const validate = async (req, res) => {
  try {
    const { email, password } = req.body;
    const result = await validateUserService(email, password);
    
    if (result.success) {
      req.session.userId = result.userId;
      req.session.userName = result.userName;
      req.session.message = `Bienvenido, ${result.userName}`;
      res.redirect('/');
    } else {
      req.session.message = 'Credenciales incorrectas';
      res.redirect('/user/login');
    }
  } catch (error) {
    req.session.message = error.message;
    res.redirect('/user/login');
  }
};

export const logout = (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      console.error('Error al cerrar sesión:', err);
    }
    res.redirect('/');
  });
};
```

### Vista de Lista Completa

**`src/views/user/getAllUsers.handlebars`**:
```handlebars
<h2>Lista de Usuarios</h2>

<a href="/user/create" class="btn btn-success">Crear Usuario</a>

{{#if message}}
  <div class="message">{{message}}</div>
{{/if}}

{{#if users.length}}
  <table>
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Apellido</th>
        <th>Email</th>
        <th>Edad</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      {{#each users}}
      <tr>
        <td>{{name}}</td>
        <td>{{lastName}}</td>
        <td>{{email}}</td>
        <td>{{age}}</td>
        <td>
          <a href="/user/update/{{_id}}" class="btn">Editar</a>
          <form action="/user/delete/{{_id}}?_method=DELETE" method="POST" style="display: inline;">
            <button type="submit" class="btn btn-danger"
                    onclick="return confirm('¿Estás seguro de eliminar a {{name}} {{lastName}}?')">
              Eliminar
            </button>
          </form>
        </td>
      </tr>
      {{/each}}
    </tbody>
  </table>
{{else}}
  <p>No hay usuarios registrados.</p>
{{/if}}
```

### Vista de Creación

**`src/views/user/createUser.handlebars`**:
```handlebars
<h2>Crear Usuario</h2>

{{#if message}}
  <div class="message error">{{message}}</div>
{{/if}}

<form action="/user/create" method="POST">
    <div>
        <label for="name">Nombre:</label>
        <input type="text" id="name" name="name" required>
    </div>
    
    <div>
        <label for="lastName">Apellido:</label>
        <input type="text" id="lastName" name="lastName" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
    </div>
    
    <div>
        <label for="age">Edad:</label>
        <input type="number" id="age" name="age" required>
    </div>
    
    <div>
        <label for="password">Contraseña:</label>
        <input type="password" id="password" name="password" required>
    </div>
    
    <button type="submit" class="btn btn-success">Crear Usuario</button>
    <a href="/user/getAll" class="btn">Cancelar</a>
</form>
```

### Vista de Edición

**`src/views/user/updateUser.handlebars`**:
```handlebars
<h2>Editar Usuario</h2>

{{#if message}}
  <div class="message error">{{message}}</div>
{{/if}}

<form action="/user/update/{{user._id}}?_method=PUT" method="POST">
    <div>
        <label for="name">Nombre:</label>
        <input type="text" id="name" name="name" value="{{user.name}}" required>
    </div>
    
    <div>
        <label for="lastName">Apellido:</label>
        <input type="text" id="lastName" name="lastName" value="{{user.lastName}}" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" value="{{user.email}}" required>
    </div>
    
    <div>
        <label for="age">Edad:</label>
        <input type="number" id="age" name="age" value="{{user.age}}" required>
    </div>
    
    <button type="submit" class="btn">Actualizar</button>
    <a href="/user/getAll" class="btn">Cancelar</a>
</form>
```

### Patrón: Vista + Acción

**⚠️ IMPORTANTE**: En cada acción (PUT, DELETE, POST) vamos a tener una vista asociada con el método GET que posea algún formulario.

**Ejemplo**:
- `GET /user/create` → Muestra formulario (vista)
- `POST /user/create` → Procesa formulario (acción)
- `GET /user/update/:id` → Muestra formulario con datos (vista)
- `PUT /user/update/:id` → Procesa actualización (acción)
- `GET /user/getAll` → Muestra lista (vista)
- `DELETE /user/delete/:id` → Procesa eliminación (acción)

---

## 14. Buenas Prácticas

### Organización

- ✅ **Separar vistas por entidad**: `views/user/`, `views/product/`
- ✅ **Usar layouts**: Evitar repetir HTML
- ✅ **Crear partials**: Reutilizar componentes
- ✅ **Helpers personalizados**: Para lógica común

### Seguridad

- ✅ **Escapar HTML**: Usar `{{variable}}` (no `{{{variable}}}`) cuando sea posible
- ✅ **Validar datos**: Validar en el servicio, no solo en el frontend
- ✅ **Sesiones seguras**: Usar `httpOnly` y `secure` en producción
- ✅ **Sanitizar inputs**: Limpiar datos del usuario antes de guardar

### Performance

- ✅ **Cache de vistas**: En producción, cachear vistas compiladas
- ✅ **Minificar CSS/JS**: Reducir tamaño de archivos estáticos
- ✅ **Lazy loading**: Cargar imágenes bajo demanda

### Código

- ✅ **Nombres descriptivos**: Vistas y controladores con nombres claros
- ✅ **Mensajes de sesión**: Usar para feedback al usuario
- ✅ **Manejo de errores**: Siempre manejar errores en controladores
- ✅ **Comentarios**: Documentar código complejo

### Estructura

- ✅ **MVC claro**: Separar Model, View, Controller
- ✅ **Servicios**: Lógica de negocio en servicios, no en controladores
- ✅ **Rutas organizadas**: Agrupar rutas por recurso

---

## 15. Troubleshooting

### Error: "Cannot find module 'express-handlebars'"

**Solución**:
```bash
npm install express-handlebars
```

### Error: "View not found"

**Causa**: La ruta de la vista es incorrecta o el archivo no existe.

**Solución**:
- Verificar que el archivo existe en `src/views/`
- Verificar que la ruta en `res.render()` sea correcta
- Verificar que `app.set('views')` apunte al directorio correcto

### Error: "Layout not found"

**Causa**: El layout no existe o la ruta es incorrecta.

**Solución**:
- Verificar que `layouts/main.handlebars` existe
- Verificar `layoutsDir` en la configuración

### Error: "Helper not defined"

**Causa**: El helper no está registrado.

**Solución**:
- Verificar que `registerHelpers()` se llama antes de configurar el engine
- Verificar que el helper está correctamente registrado

### Error: "req.body is undefined"

**Causa**: Falta el middleware `body-parser`.

**Solución**:
```javascript
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

### Error: "Cannot DELETE /user/delete/:id"

**Causa**: Falta `method-override`.

**Solución**:
```javascript
import methodOverride from 'method-override';
app.use(methodOverride('_method'));
```

Y en el formulario:
```handlebars
<form action="/user/delete/{{_id}}?_method=DELETE" method="POST">
```

### Error: "Session not working"

**Causa**: Falta configuración de sesiones o `SECRET`.

**Solución**:
```javascript
app.use(session({
  secret: process.env.SECRET || 'mi-secreto',
  resave: false,
  saveUninitialized: false
}));
```

### Error: "Static files not loading"

**Causa**: Ruta incorrecta o middleware no configurado.

**Solución**:
- Verificar `app.use(express.static(...))`
- Verificar que las rutas en HTML no incluyan `public`
- Verificar que los archivos existen en `src/public/`

---

## 📚 Resumen de Conceptos Clave

### Handlebars

- **Template Engine**: Genera HTML dinámico
- **Sintaxis**: `{{variable}}`, `{{#if}}`, `{{#each}}`
- **Layouts**: Plantillas base
- **Partials**: Componentes reutilizables
- **Helpers**: Funciones personalizadas

### MVC

- **Model**: Estructura de datos (Mongoose)
- **View**: Templates Handlebars
- **Controller**: Lógica que conecta Model y View

### Flujo

```
Usuario → Ruta → Controlador → Servicio → Modelo → DB
                                              ↓
Usuario ← HTML ← Vista ← Controlador ← Servicio ← DB
```

### Diferencias Clave

| API (JSON) | Server (Handlebars) |
|------------|---------------------|
| `res.json()` | `res.render()` |
| Frontend separado | HTML integrado |
| CORS necesario | Sin CORS |
| Sin templates | Con Handlebars |

---

