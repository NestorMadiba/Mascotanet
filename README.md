# MascotaNet - Red Social de Mascotas

¡Bienvenido a MascotaNet! Esta es una plataforma web completa que permite a los dueños de mascotas publicar y buscar avisos de venta, compra, cruza, adopción y servicios para sus animales.

## Tabla de Contenidos
1. [Stack Tecnológico](#stack-tecnológico)
2. [Prerrequisitos](#prerrequisitos)
3. [Configuración Local](#configuración-local)
   - [Backend](#-backend)
   - [Frontend](#-frontend)
4. [Ejecución del Proyecto](#ejecución-del-proyecto)
5. [Instrucciones de Despliegue (Producción)](#instrucciones-de-despliegue-producción)
   - [Paso 1: Base de Datos (MongoDB Atlas)](#paso-1-base-de-datos-mongodb-atlas)
   - [Paso 2: Almacenamiento de Imágenes (Cloudinary)](#paso-2-almacenamiento-de-imágenes-cloudinary)
   - [Paso 3: Despliegue del Backend (Render)](#paso-3-despliegue-del-backend-render)
   - [Paso 4: Despliegue del Frontend (Vercel)](#paso-4-despliegue-del-frontend-vercel)

## Stack Tecnológico
- **MongoDB**: Base de datos NoSQL.
- **Express.js**: Framework para el backend en Node.js.
- **React**: Librería para el frontend.
- **Node.js**: Entorno de ejecución para el backend.
- **Adicionales**:
    - **Mongoose**: ODM para MongoDB.
    - **JSON Web Tokens (JWT)**: Para autenticación.
    - **Cloudinary**: Para almacenamiento de imágenes en la nube.
    - **Multer**: Middleware para la subida de archivos.
    - **Tailwind CSS**: Framework de CSS para el diseño.
    - **React Router**: Para el enrutamiento en el frontend.
    - **Axios**: Para peticiones HTTP.

## Prerrequisitos
- **Node.js**: v18 o superior.
- **npm**: Gestor de paquetes de Node.js.
- **Git**: Para clonar el repositorio.

## Configuración Local

### 1. Clonar el Repositorio
```bash
git clone <URL_DEL_REPOSITORIO>
cd mascotanet
```

### 2. Backend
Navega a la carpeta del backend e instala las dependencias.
```bash
cd backend
npm install
```
Crea un archivo `.env` en la raíz de la carpeta `/backend` a partir del ejemplo.
```bash
cp .env.example .env
```
Abre el archivo `.env` y añade tus propias credenciales:
```env
# Puerto en el que correrá el servidor
PORT=5000

# URI de conexión de tu base de datos de MongoDB Atlas
MONGO_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/mascotanet?retryWrites=true&w=majority

# Clave secreta para firmar los JWT (usa una cadena larga y aleatoria)
JWT_SECRET=tu_secreto_muy_largo_y_seguro_para_jwt

# URL de Cloudinary para subir imágenes. Formato: cloudinary://API_KEY:API_SECRET@CLOUD_NAME
CLOUDINARY_URL=cloudinary://...
```

### 3. Frontend
Abre una nueva terminal, navega a la carpeta del frontend e instala las dependencias.
```bash
cd frontend
npm install
```
Crea un archivo `.env` en la raíz de la carpeta `/frontend` a partir del ejemplo.
```bash
cp .env.example .env
```
Abre el archivo `.env` y configura la URL de tu API del backend.
```env
# URL base de tu API del backend. Para desarrollo local, es el puerto del servidor backend.
REACT_APP_API_URL=http://localhost:5000/api
```

## Ejecución del Proyecto
1.  **Iniciar el Backend**: En la terminal dentro de la carpeta `/backend`, ejecuta:
    ```bash
    npm run dev
    ```
    El servidor se iniciará en `http://localhost:5000`.

2.  **Iniciar el Frontend**: En la otra terminal dentro de la carpeta `/frontend`, ejecuta:
    ```bash
    npm start
    ```
    La aplicación React se abrirá en `http://localhost:3000`.

## Instrucciones de Despliegue (Producción)

### Paso 1: Base de Datos (MongoDB Atlas)
1.  Ve a [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) y crea una cuenta gratuita.
2.  Crea un nuevo Cluster (elige la opción gratuita "M0").
3.  En la sección "Database Access", crea un nuevo usuario de base de datos con contraseña. Anota el usuario y la contraseña.
4.  En la sección "Network Access", añade tu dirección IP actual (o `0.0.0.0/0` para permitir el acceso desde cualquier lugar, aunque es menos seguro).
5.  Ve a "Databases", haz clic en "Connect" en tu cluster, selecciona "Connect your application" y copia la cadena de conexión (Connection String).
6.  Reemplaza `<password>` con la contraseña del usuario que creaste y `myFirstDatabase` con el nombre que desees para tu base de datos (ej. `mascotanet`). Esta es tu `MONGO_URI`.

### Paso 2: Almacenamiento de Imágenes (Cloudinary)
1.  Ve a [Cloudinary](https://cloudinary.com/) y crea una cuenta gratuita.
2.  En el Dashboard, encontrarás tu "Cloud Name", "API Key" y "API Secret".
3.  Construye tu `CLOUDINARY_URL` con el formato: `cloudinary://<API_Key>:<API_Secret>@<Cloud_Name>`.

### Paso 3: Despliegue del Backend (Render)
1.  Crea una cuenta en [Render](https://render.com/).
2.  En el Dashboard, haz clic en "New +" y selecciona "Web Service".
3.  Conecta tu repositorio de GitHub/GitLab.
4.  Configura el servicio:
    - **Name**: `mascotanet-backend` (o el que prefieras).
    - **Root Directory**: `backend` (¡Importante!).
    - **Runtime**: `Node`.
    - **Build Command**: `npm install`.
    - **Start Command**: `npm start`.
5.  Añade las siguientes variables de entorno en la sección "Environment":
    - `MONGO_URI`: La cadena de conexión de MongoDB Atlas.
    - `JWT_SECRET`: Tu clave secreta para JWT.
    - `CLOUDINARY_URL`: Tu URL de Cloudinary.
    - `NODE_ENV`: `production`
6.  Haz clic en "Create Web Service". Render desplegará tu backend y te dará una URL pública (ej: `https://mascotanet-backend.onrender.com`).

### Paso 4: Despliegue del Frontend (Vercel)
1.  Crea una cuenta en [Vercel](https://vercel.com/).
2.  Haz clic en "Add New..." y selecciona "Project".
3.  Importa el mismo repositorio de Git.
4.  Vercel detectará que es una aplicación de React (Create React App).
5.  Configura el proyecto:
    - **Root Directory**: `frontend` (¡Importante!).
    - Mantén los "Build & Output Settings" por defecto.
6.  Añade la variable de entorno:
    - **Name**: `REACT_APP_API_URL`
    - **Value**: La URL de tu backend desplegado en Render, seguida de `/api` (ej: `https://mascotanet-backend.onrender.com/api`).
7.  Haz clic en "Deploy". Vercel desplegará tu frontend y te dará la URL final de tu proyecto.

¡Y listo! Tu aplicación MascotaNet estará completamente online.