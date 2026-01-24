# NPM: Gestor de Paquetes 📦

## ¿Qué es NPM?

NPM (Node Package Manager) es el gestor de paquetes de Node.js. Permite instalar y gestionar dependencias de proyectos.

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

