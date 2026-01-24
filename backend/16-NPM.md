# NPM: Gestor de Paquetes 📦

## 📑 Índice

1. [¿Qué es NPM? (Analogía del Mundo Real)](#qué-es-npm-analogía-del-mundo-real)
2. [Inicializar Proyecto](#inicializar-proyecto)
3. [Instalar Paquetes](#instalar-paquetes)
4. [package.json](#packagejson)
5. [Scripts](#scripts)
6. [Comandos Útiles](#comandos-útiles)
7. [Referencias Relacionadas](#referencias-relacionadas)

---

## ¿Qué es NPM? (Analogía del Mundo Real)

### 📦 Analogía: La Tienda de Aplicaciones

Imagina una tienda de aplicaciones (como App Store o Google Play):
- **NPM**: Es la tienda donde encuentras herramientas (paquetes)
- **Paquetes**: Son las herramientas que puedes instalar (Express, React, etc.)
- **Instalación**: Como descargar una app en tu teléfono
- **Dependencias**: Como cuando una app necesita otra app para funcionar

**NPM es como la tienda de herramientas** para desarrolladores.

### 🏪 Analogía: El Supermercado

Piensa en un supermercado:
- **NPM**: El supermercado completo
- **Paquetes**: Los productos que puedes comprar
- **Carrito (package.json)**: Tu lista de compras
- **Instalación**: Como llevar los productos a casa

**NPM te permite "comprar" herramientas** para tu proyecto.

### 🧰 Analogía: La Caja de Herramientas

Una caja de herramientas:
- **NPM**: La ferretería donde compras herramientas
- **Paquetes**: Las herramientas individuales
- **package.json**: Tu lista de herramientas que necesitas
- **Instalación**: Como agregar herramientas a tu caja

**NPM te permite obtener herramientas** que otros desarrolladores ya crearon.

### ¿Qué es NPM?

NPM (Node Package Manager) es el gestor de paquetes de Node.js. Permite instalar y gestionar dependencias de proyectos.

**En términos simples**: NPM es como una tienda donde puedes descargar herramientas (paquetes) que otros desarrolladores crearon, para usarlas en tu proyecto sin tener que crearlas desde cero.

## Inicializar Proyecto

```bash
# Crear package.json
npm init

# Crear package.json con valores por defecto
npm init -y
```

## Instalar Paquetes

```bash
# Instalar paquete localmente
npm install express

# Instalar paquete globalmente
npm install -g nodemon

# Instalar como dependencia de desarrollo
npm install --save-dev nodemon

# Instalar versión específica
npm install express@4.18.0
```

## package.json

```json
{
  "name": "mi-proyecto",
  "version": "1.0.0",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.0"
  }
}
```

## Scripts

```bash
# Ejecutar script
npm start
npm run dev
npm run build
```

## Comandos Útiles

```bash
# Instalar todas las dependencias
npm install

# Actualizar paquetes
npm update

# Desinstalar paquete
npm uninstall express

# Ver paquetes instalados
npm list

# Ver información de un paquete
npm info express
```

## node_modules y package-lock.json

- **node_modules/**: Carpeta con todas las dependencias instaladas
- **package-lock.json**: Archivo que bloquea versiones exactas de dependencias

## Conceptos Clave

1. **NPM**: Gestor de paquetes de Node.js
2. **package.json**: Archivo de configuración del proyecto
3. **dependencies**: Paquetes necesarios en producción
4. **devDependencies**: Paquetes solo para desarrollo
5. **scripts**: Comandos personalizados
6. **node_modules**: Carpeta con dependencias
7. **package-lock.json**: Bloquea versiones exactas

