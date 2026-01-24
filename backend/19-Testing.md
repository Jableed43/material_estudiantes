# Master Guide: Testing Full-Stack y Swagger 🛠️⚖️

## 📑 Índice
1. [Introducción al Testing](#1-introducción-al-testing)
2. [La Pirámide de Pruebas](#2-la-pirámide-de-pruebas)
3. [TDD: Desarrollo Guiado por Pruebas](#3-tdd-desarrollo-guiado-por-pruebas)
4. [Tipos de Pruebas en el Stack Full-Stack](#4-tipos-de-pruebas-en-el-stack-full-stack)
5. [Jest: Framework de Testing](#5-jest-framework-de-testing)
6. [Supertest: Testing de APIs](#6-supertest-testing-de-apis)
7. [React Testing Library](#7-react-testing-library)
8. [Swagger y OpenAPI](#8-swagger-y-openapi)
9. [Prompt Engineering para Testing](#9-prompt-engineering-para-testing)
10. [Buenas Prácticas](#10-buenas-prácticas)

---

## 1. Introducción al Testing

### ¿Qué es el Testing? (Analogía del Mundo Real)

### ✅ Analogía: La Prueba de Calidad

Imagina que fabricas productos:
- **Código**: Es el producto que fabricas
- **Testing**: Es la prueba de calidad antes de vender
- **Tests**: Son las verificaciones que haces (¿funciona? ¿es seguro? ¿cumple los requisitos?)

**El testing es como la prueba de calidad** - verificas que tu código funciona correctamente antes de usarlo.

### 🏥 Analogía: El Examen Médico

Piensa en un examen médico:
- **Código**: Es tu cuerpo
- **Testing**: Es el examen médico
- **Tests**: Son las pruebas que te hacen (análisis, radiografías, etc.)

**El testing verifica que tu código está "sano"** y funciona como debe.

### 🧪 Analogía: El Experimento Científico

Un experimento científico:
- **Código**: Es la hipótesis
- **Testing**: Es el experimento que prueba la hipótesis
- **Tests**: Son las pruebas que haces para verificar

**El testing verifica que tu código hace lo que esperas** que haga.

### ¿Qué es el Testing?

Garantizar la calidad y robustez del código es fundamental. El testing asegura que tu código haga lo que debe hacer y no rompa cosas viejas al añadir nuevas.

**En términos simples**: El testing es como hacer pruebas de calidad a tu código - verificas que funciona correctamente y que no rompe cosas existentes cuando agregas nuevas funcionalidades.

### ¿Por qué Testear?

- ✅ **Confianza**: Saber que tu código funciona correctamente
- ✅ **Prevenir regresiones**: Detectar cuando cambios nuevos rompen funcionalidad existente
- ✅ **Documentación viva**: Los tests documentan cómo debe usarse el código
- ✅ **Refactorización segura**: Poder cambiar código sabiendo que los tests detectarán errores
- ✅ **Calidad**: Código más robusto y mantenible

### Conceptos Clave

- **Test Case**: Un caso de prueba específico
- **Test Suite**: Conjunto de tests relacionados
- **Assertion**: Verificación de que algo es cierto
- **Mock**: Simulación de dependencias
- **Coverage**: Porcentaje de código cubierto por tests

---

## 2. La Pirámide de Pruebas

La pirámide de pruebas es una guía visual que indica cómo se deben distribuir los esfuerzos de testing para lograr un equilibrio entre rapidez, aislamiento y cobertura.

### Estructura de la Pirámide

```
        /\
       /  \      E2E (End-to-End)
      /____\     Pocos, lentos, costosos
     /      \    
    /________\   Integración
   /          \  Algunos, medios
  /____________\ Unitarios
  Muchos, rápidos, baratos
```

### Tabla Comparativa

| Nivel | Rapidez | Aislamiento | Objetivo | Herramientas |
|-------|---------|-------------|----------|--------------|
| **E2E (End-to-End)** | Lento | Bajo | Flujo completo del usuario | Cypress, Playwright |
| **Integración** | Medio | Medio | Interacción entre módulos | Supertest + Rutas |
| **Unitaria (Unit)** | Rápido | Alto | Lógica individual y funciones | Jest |

### Distribución Recomendada

- **70% Unit Tests**: Pruebas rápidas y aisladas
- **20% Integration Tests**: Pruebas de interacción entre componentes
- **10% E2E Tests**: Pruebas de flujos completos críticos

### ¿Por qué esta Distribución?

- ✅ **Unit Tests**: Rápidos, fáciles de escribir, detectan errores temprano
- ✅ **Integration Tests**: Validan que las piezas funcionen juntas
- ✅ **E2E Tests**: Validan flujos críticos pero son lentos y costosos

---

## 3. TDD: Desarrollo Guiado por Pruebas

**TDD (Test-Driven Development)** es una metodología donde **primero escribes el test** (que falla) y luego desarrollas el código necesario para que pase.

### Ciclo TDD (Red-Green-Refactor)

1. **🔴 Red**: Escribir un test que falle
2. **🟢 Green**: Escribir el código mínimo para que pase
3. **🔵 Refactor**: Mejorar el código manteniendo los tests pasando

### Ventajas de TDD

- ✅ **Código más simple**: Solo escribes lo necesario
- ✅ **Mejor diseño**: El código se diseña pensando en cómo se usa
- ✅ **Confianza**: Sabes que el código funciona porque los tests pasan
- ✅ **Documentación**: Los tests documentan el comportamiento esperado

### Ejemplo de TDD

**Paso 1: Red (Test que falla)**
```javascript
// test.js
test('suma 1 + 2 es igual a 3', () => {
  expect(sumar(1, 2)).toBe(3);
});

// sumar.js (aún no existe)
// ❌ Test falla: sumar is not defined
```

**Paso 2: Green (Código mínimo)**
```javascript
// sumar.js
function sumar(a, b) {
  return a + b;
}

// ✅ Test pasa
```

**Paso 3: Refactor (Mejorar)**
```javascript
// sumar.js (mejorado)
function sumar(a, b) {
  if (typeof a !== 'number' || typeof b !== 'number') {
    throw new Error('Los argumentos deben ser números');
  }
  return a + b;
}

// ✅ Tests siguen pasando
```

---

## 4. Tipos de Pruebas en el Stack Full-Stack

### 1. Pruebas Unitarias (Unit Tests) 🧪

Prueban la unidad mínima de código de forma completamente **aislada**.

#### Características

- ✅ **Aislamiento**: Se usa **Mocking** para simular dependencias (DB, APIs externas)
- ✅ **Rápidas**: Ejecutan en milisegundos
- ✅ **Muchas**: Deberías tener muchas pruebas unitarias

#### Aplicación Full-Stack

**Backend (Express)**:
- Funciones de utilidades
- Lógica de negocio pura del **Service**
- Validaciones
- Helpers

**Frontend (React)**:
- **Custom Hooks** (sin UI)
- Funciones de **helpers**
- Utilidades
- Lógica de negocio pura

#### Ejemplo Backend

```javascript
// utils/validators.js
export const validarEmail = (email) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

// utils/validators.test.js
import { validarEmail } from './validators';

test('validarEmail retorna true para email válido', () => {
  expect(validarEmail('juan@example.com')).toBe(true);
});

test('validarEmail retorna false para email inválido', () => {
  expect(validarEmail('email-invalido')).toBe(false);
});
```

#### Ejemplo Frontend

```javascript
// hooks/useCounter.js
export const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  
  return { count, increment, decrement };
};

// hooks/useCounter.test.js
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

test('useCounter inicia en 0', () => {
  const { result } = renderHook(() => useCounter());
  expect(result.current.count).toBe(0);
});

test('useCounter incrementa correctamente', () => {
  const { result } = renderHook(() => useCounter());
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```

### 2. Pruebas de Componente (React) ⚛️

Verifican que un componente se renderice y reaccione correctamente según sus `props`.

#### Características

- ✅ **Renderizado**: Verifica que el componente se renderice
- ✅ **Props**: Verifica que reaccione a diferentes props
- ✅ **Interacción**: Verifica que responda a eventos del usuario

#### Herramienta

**React Testing Library (RTL)**: Enfocada en probar el comportamiento del usuario, no la implementación.

#### Ejemplo

```javascript
// components/Button.jsx
export const Button = ({ onClick, children, disabled }) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
};

// components/Button.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

test('Button se renderiza con texto', () => {
  render(<Button>Click me</Button>);
  expect(screen.getByText('Click me')).toBeInTheDocument();
});

test('Button llama onClick al hacer click', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click me</Button>);
  
  fireEvent.click(screen.getByText('Click me'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});

test('Button está deshabilitado cuando disabled es true', () => {
  render(<Button disabled>Click me</Button>);
  expect(screen.getByText('Click me')).toBeDisabled();
});
```

### 3. Pruebas de Integración 🔗

Validan que las piezas del sistema se comuniquen bien.

#### Características

- ✅ **Interacción**: Prueba cómo interactúan múltiples componentes
- ✅ **Realismo**: Más cercano a cómo se usa en producción
- ✅ **Velocidad media**: Más lentas que unitarias pero más rápidas que E2E

#### Aplicación Full-Stack

**Backend (Express)**:
- Probar rutas de la API simulando peticiones HTTP con **Supertest**
- Validar la cadena: **Middleware → Controller → Service → Model**
- Probar autenticación y autorización

**Frontend (React)**:
- Probar la interacción de un componente con una fuente de datos **mockeada**
- Probar comunicación entre componente padre e hijo
- Probar hooks con contexto

#### Ejemplo Backend (Supertest)

```javascript
// routes/userRoutes.js
router.post('/create', createUserController);

// tests/userRoutes.test.js
import request from 'supertest';
import app from '../app';

describe('POST /api/user/create', () => {
  test('crea usuario exitosamente', async () => {
    const response = await request(app)
      .post('/api/user/create')
      .send({
        nombre: 'Juan',
        email: 'juan@example.com',
        password: '123456'
      })
      .expect(201);
    
    expect(response.body).toHaveProperty('id');
    expect(response.body.nombre).toBe('Juan');
  });
  
  test('retorna 400 si faltan campos', async () => {
    const response = await request(app)
      .post('/api/user/create')
      .send({
        nombre: 'Juan'
        // Falta email y password
      })
      .expect(400);
  });
});
```

#### Ejemplo Frontend

```javascript
// components/UserList.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import { UserList } from './UserList';

// Mock de la API
jest.mock('../hooks/useUsers', () => ({
  useUsers: () => ({
    users: [
      { id: 1, nombre: 'Juan' },
      { id: 2, nombre: 'María' }
    ],
    loading: false
  })
}));

test('UserList muestra usuarios', async () => {
  render(<UserList />);
  
  await waitFor(() => {
    expect(screen.getByText('Juan')).toBeInTheDocument();
    expect(screen.getByText('María')).toBeInTheDocument();
  });
});
```

### 4. Pruebas E2E (Fin a Fin) 🏁

Simulan a un usuario real navegando por la web. Validan flujos críticos como el Checkout o el Login.

#### Características

- ✅ **Realismo**: Simula un usuario real
- ✅ **Completas**: Prueban todo el stack (Frontend + Backend + DB)
- ✅ **Lentas**: Toman tiempo en ejecutarse
- ✅ **Costosas**: Requieren más recursos

#### Herramientas Comunes

- **Cypress**: Popular, fácil de usar, buena documentación
- **Playwright**: Rápido, soporta múltiples navegadores
- **Selenium**: Clásico, muy establecido

#### Ejemplo con Cypress

```javascript
// cypress/e2e/login.cy.js
describe('Login Flow', () => {
  it('usuario puede hacer login', () => {
    cy.visit('/login');
    
    cy.get('[data-testid="email-input"]').type('juan@example.com');
    cy.get('[data-testid="password-input"]').type('123456');
    cy.get('[data-testid="login-button"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.get('[data-testid="user-name"]').should('contain', 'Juan');
  });
});
```

#### Cuándo Usar E2E

- ✅ Flujos críticos (login, checkout, pago)
- ✅ Validar integración completa
- ✅ Antes de releases importantes
- ❌ No para toda la aplicación (muy lento)

---

## 5. Jest: Framework de Testing

**Jest** es el framework de testing más popular para JavaScript. Viene preconfigurado en Create React App y es fácil de usar.

### Instalación

```bash
npm install --save-dev jest
```

### Configuración Básica

**`package.json`**:
```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

### Sintaxis Básica

```javascript
// test.js o archivo.test.js
test('descripción del test', () => {
  expect(valor).toBe(valorEsperado);
});

// O con describe para agrupar
describe('Grupo de tests', () => {
  test('test 1', () => {
    // ...
  });
  
  test('test 2', () => {
    // ...
  });
});
```

### Matchers (Comparadores)

```javascript
// Igualdad
expect(valor).toBe(3);              // ===
expect(valor).toEqual({ a: 1 });     // Comparación profunda

// Verdadero/Falso
expect(valor).toBeTruthy();
expect(valor).toBeFalsy();

// Números
expect(valor).toBeGreaterThan(3);
expect(valor).toBeLessThan(5);

// Strings
expect(valor).toMatch(/pattern/);
expect(valor).toContain('texto');

// Arrays
expect(array).toContain(item);
expect(array).toHaveLength(3);

// Objetos
expect(obj).toHaveProperty('key');
expect(obj).toEqual({ key: 'value' });

// Excepciones
expect(() => funcion()).toThrow();
expect(() => funcion()).toThrow('mensaje');
```

### Mocking (Simulación)

#### Mock de Funciones

```javascript
// Mock de función
const mockFn = jest.fn();
mockFn('arg1', 'arg2');

expect(mockFn).toHaveBeenCalled();
expect(mockFn).toHaveBeenCalledWith('arg1', 'arg2');
expect(mockFn).toHaveBeenCalledTimes(1);
```

#### Mock de Módulos

```javascript
// Mock de módulo completo
jest.mock('../services/userService', () => ({
  createUser: jest.fn(() => Promise.resolve({ id: 1, nombre: 'Juan' }))
}));
```

#### Mock de Implementación

```javascript
const mockFn = jest.fn();
mockFn.mockReturnValue(42);
mockFn.mockResolvedValue({ data: 'result' });
mockFn.mockRejectedValue(new Error('Error'));
```

### Setup y Teardown

```javascript
// Antes de cada test
beforeEach(() => {
  // Configuración
});

// Después de cada test
afterEach(() => {
  // Limpieza
});

// Antes de todos los tests
beforeAll(() => {
  // Configuración global
});

// Después de todos los tests
afterAll(() => {
  // Limpieza global
});
```

### Ejemplo Completo

```javascript
// utils/calculadora.js
export const sumar = (a, b) => a + b;
export const restar = (a, b) => a - b;

// utils/calculadora.test.js
import { sumar, restar } from './calculadora';

describe('Calculadora', () => {
  describe('sumar', () => {
    test('suma 1 + 2 es igual a 3', () => {
      expect(sumar(1, 2)).toBe(3);
    });
    
    test('suma números negativos', () => {
      expect(sumar(-1, -2)).toBe(-3);
    });
  });
  
  describe('restar', () => {
    test('resta 5 - 3 es igual a 2', () => {
      expect(restar(5, 3)).toBe(2);
    });
  });
});
```

---

## 6. Supertest: Testing de APIs

**Supertest** es una librería para probar rutas de Express simulando peticiones HTTP.

### Instalación

```bash
npm install --save-dev supertest
```

### Configuración

```javascript
// app.js (exportar app para testing)
const express = require('express');
const app = express();
// ... configuración ...
module.exports = app;

// O si usas ES modules
export default app;
```

### Sintaxis Básica

```javascript
const request = require('supertest');
const app = require('../app');

describe('GET /api/usuarios', () => {
  test('retorna lista de usuarios', async () => {
    const response = await request(app)
      .get('/api/usuarios')
      .expect(200);
    
    expect(response.body).toBeInstanceOf(Array);
  });
});
```

### Métodos HTTP

```javascript
// GET
await request(app).get('/api/usuarios');

// POST
await request(app)
  .post('/api/usuarios')
  .send({ nombre: 'Juan' });

// PUT
await request(app)
  .put('/api/usuarios/1')
  .send({ nombre: 'Pedro' });

// DELETE
await request(app).delete('/api/usuarios/1');
```

### Headers y Autenticación

```javascript
// Con headers
await request(app)
  .get('/api/usuarios')
  .set('Authorization', 'Bearer token123')
  .expect(200);

// Con Content-Type
await request(app)
  .post('/api/usuarios')
  .set('Content-Type', 'application/json')
  .send({ nombre: 'Juan' });
```

### Validaciones

```javascript
// Validar status code
.expect(200)
.expect(201)
.expect(400)

// Validar headers
.expect('Content-Type', /json/)

// Validar body
.expect((res) => {
  expect(res.body).toHaveProperty('id');
})
```

### Ejemplo Completo

```javascript
const request = require('supertest');
const app = require('../app');

describe('API Usuarios', () => {
  let token;
  
  // Setup: Login antes de los tests
  beforeAll(async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({
        username: 'test',
        password: '123456'
      });
    token = response.body.accessToken;
  });
  
  describe('GET /api/usuarios', () => {
    test('retorna 200 y lista de usuarios', async () => {
      const response = await request(app)
        .get('/api/usuarios')
        .set('Authorization', `Bearer ${token}`)
        .expect(200);
      
      expect(response.body).toBeInstanceOf(Array);
      expect(response.body.length).toBeGreaterThan(0);
    });
    
    test('retorna 401 sin token', async () => {
      await request(app)
        .get('/api/usuarios')
        .expect(401);
    });
  });
  
  describe('POST /api/usuarios', () => {
    test('crea usuario exitosamente', async () => {
      const nuevoUsuario = {
        nombre: 'Test User',
        email: 'test@example.com',
        password: '123456'
      };
      
      const response = await request(app)
        .post('/api/usuarios')
        .send(nuevoUsuario)
        .expect(201);
      
      expect(response.body).toHaveProperty('id');
      expect(response.body.nombre).toBe('Test User');
    });
    
    test('retorna 400 si faltan campos', async () => {
      await request(app)
        .post('/api/usuarios')
        .send({ nombre: 'Test' })
        .expect(400);
    });
  });
});
```

### Mocking de Base de Datos

```javascript
// Mock de Mongoose
jest.mock('../models/userModel', () => ({
  find: jest.fn(),
  findById: jest.fn(),
  create: jest.fn()
}));

const User = require('../models/userModel');

test('GET /api/usuarios retorna usuarios', async () => {
  User.find.mockResolvedValue([
    { id: 1, nombre: 'Juan' },
    { id: 2, nombre: 'María' }
  ]);
  
  const response = await request(app)
    .get('/api/usuarios')
    .expect(200);
  
  expect(response.body).toHaveLength(2);
});
```

---

## 7. React Testing Library

**React Testing Library (RTL)** es la herramienta estándar para testear componentes de React. Se enfoca en probar el comportamiento del usuario, no la implementación.

### Instalación

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

### Configuración

**`setupTests.js`**:
```javascript
import '@testing-library/jest-dom';
```

### Queries (Consultas)

```javascript
// Por texto
screen.getByText('Click me');
screen.getByText(/click/i);  // Case insensitive

// Por rol
screen.getByRole('button');
screen.getByRole('textbox');

// Por test id (recomendado)
screen.getByTestId('submit-button');

// Por label
screen.getByLabelText('Email');

// Múltiples
screen.getAllByText('Item');  // Retorna array
```

### Interacciones

```javascript
import { fireEvent, userEvent } from '@testing-library/react';

// fireEvent (básico)
fireEvent.click(button);
fireEvent.change(input, { target: { value: 'texto' } });

// userEvent (recomendado, más realista)
await userEvent.click(button);
await userEvent.type(input, 'texto');
```

### Ejemplo Completo

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { LoginForm } from './LoginForm';

describe('LoginForm', () => {
  test('renderiza formulario de login', () => {
    render(<LoginForm />);
    
    expect(screen.getByLabelText('Email')).toBeInTheDocument();
    expect(screen.getByLabelText('Password')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /login/i })).toBeInTheDocument();
  });
  
  test('envía formulario con datos correctos', async () => {
    const handleSubmit = jest.fn();
    render(<LoginForm onSubmit={handleSubmit} />);
    
    fireEvent.change(screen.getByLabelText('Email'), {
      target: { value: 'juan@example.com' }
    });
    fireEvent.change(screen.getByLabelText('Password'), {
      target: { value: '123456' }
    });
    fireEvent.click(screen.getByRole('button', { name: /login/i }));
    
    await waitFor(() => {
      expect(handleSubmit).toHaveBeenCalledWith({
        email: 'juan@example.com',
        password: '123456'
      });
    });
  });
});
```

---

## 8. Swagger y OpenAPI

**Swagger** es un conjunto de herramientas para documentar y probar APIs REST basadas en la **Especificación OpenAPI (OAS)**.

### ¿Qué es OpenAPI?

**OpenAPI (OAS)** es un estándar para describir APIs REST. Define:
- Rutas y endpoints
- Parámetros de entrada
- Esquemas de datos
- Códigos de respuesta
- Autenticación

### Componentes de Swagger

#### 1. Archivo de Contrato (YAML/JSON)

Define formalmente toda la API. Es la "verdad única" entre Frontend y Backend.

**Ejemplo `swagger.yaml`**:
```yaml
openapi: 3.0.0
info:
  title: API Usuarios
  version: 1.0.0
paths:
  /api/usuarios:
    get:
      summary: Obtener todos los usuarios
      responses:
        '200':
          description: Lista de usuarios
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Usuario'
    post:
      summary: Crear usuario
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UsuarioInput'
      responses:
        '201':
          description: Usuario creado
components:
  schemas:
    Usuario:
      type: object
      properties:
        id:
          type: string
        nombre:
          type: string
        email:
          type: string
    UsuarioInput:
      type: object
      required:
        - nombre
        - email
      properties:
        nombre:
          type: string
        email:
          type: string
```

#### 2. Swagger UI

Interfaz gráfica interactiva generada a partir del archivo OAS. Permite:
- ✅ Ver todos los endpoints
- ✅ Probar endpoints en vivo
- ✅ Ver esquemas de datos
- ✅ Ver ejemplos de requests/responses

### Instalación y Configuración

```bash
npm install swagger-jsdoc swagger-ui-express
```

**Configuración en Express**:
```javascript
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const swaggerOptions = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'API Usuarios',
      version: '1.0.0',
    },
  },
  apis: ['./src/routes/*.js'], // Archivos con documentación
};

const swaggerSpec = swaggerJsdoc(swaggerOptions);

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

### Documentar Endpoints

```javascript
/**
 * @swagger
 * /api/usuarios:
 *   get:
 *     summary: Obtener todos los usuarios
 *     tags: [Usuarios]
 *     responses:
 *       200:
 *         description: Lista de usuarios
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 $ref: '#/components/schemas/Usuario'
 */
router.get('/usuarios', getAllUsersController);
```

### Importancia para el Testing

- ✅ **Contrato definido**: Las Pruebas de Integración deben validar el contrato
- ✅ **Frontend**: El frontend debe cumplir con el contrato
- ✅ **Documentación viva**: Siempre actualizada con el código
- ✅ **Testing manual**: Probar endpoints sin Postman

---

## 9. Prompt Engineering para Testing

Para generar mejores casos de prueba con IA, sigue estas pautas:

### 1. Rol Específico

Define a la IA como un especialista:

```
"Actúa como un Ingeniero de Automatización QA experto en TDD y en el framework Jest."
```

### 2. Inyección de Contexto y Framework

Proporciona el código y pide el framework específico:

**Backend (Integración)**:
```
"Usando Supertest, genera una prueba de integración para la siguiente ruta de Express. 
[CÓDIGO DE RUTA]. La prueba debe simular la función service.createUser (mockeada) 
y verificar el código de respuesta HTTP."
```

**Frontend (Componente)**:
```
"Genera pruebas de interacción para el siguiente componente de React. 
Usa React Testing Library (RTL) y userEvent para simular que el usuario 
rellena el formulario y hace clic en 'Submit'."
```

### 3. Solicitud de Cobertura Específica

Utiliza la técnica **Chain of Thought (CoT)**:

```
"Antes de darme el código, lista los 4 casos de prueba (éxito, error, 
dato faltante, dato inválido) que cubrirás en la prueba de integración. 
Luego, genera el código completo para cada caso."
```

### 4. Casos de Prueba Completos

Pide específicamente:
- ✅ Casos de éxito
- ✅ Casos de error
- ✅ Casos límite (valores extremos)
- ✅ Validaciones (campos requeridos, formatos)

### Ejemplo de Prompt Completo

```
Actúa como un QA Automation Engineer experto en Jest y Supertest.

Contexto:
- Framework: Jest + Supertest
- Ruta: POST /api/usuarios
- Controller: createUserController
- Service: createUser (mockear)

Antes de escribir el código, lista los casos de prueba que cubrirás:
1. Éxito: Usuario creado correctamente (201)
2. Error: Campos faltantes (400)
3. Error: Email inválido (400)
4. Error: Email duplicado (409)
5. Error: Error del servidor (500)

Luego, genera el código completo de tests usando Supertest, 
incluyendo mocks del service y validaciones de respuesta.
```

---

## 10. Buenas Prácticas

### Estructura de Tests

- ✅ **Organizar por funcionalidad**: Agrupa tests relacionados
- ✅ **Nombres descriptivos**: "debe retornar 400 cuando faltan campos"
- ✅ **AAA Pattern**: Arrange (preparar), Act (ejecutar), Assert (verificar)

### Cobertura

- ✅ **Aim for 80%+**: Pero calidad > cantidad
- ✅ **Cubrir casos críticos**: Prioriza funcionalidad importante
- ✅ **No obsesionarse**: 100% de cobertura no garantiza calidad

### Mantenibilidad

- ✅ **Tests independientes**: Cada test debe poder ejecutarse solo
- ✅ **No depender de orden**: Los tests no deben depender unos de otros
- ✅ **Limpiar después**: Usa `afterEach` para limpiar estado

### Performance

- ✅ **Tests rápidos**: Unit tests deben ser muy rápidos
- ✅ **Mock pesado**: Mockea llamadas a DB, APIs externas
- ✅ **Paralelización**: Ejecuta tests en paralelo cuando sea posible

### Documentación

- ✅ **Swagger actualizado**: Mantén la documentación actualizada
- ✅ **Tests como documentación**: Los tests documentan el comportamiento
- ✅ **README con ejemplos**: Incluye ejemplos de uso

---

