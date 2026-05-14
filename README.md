# Tarea

Este proyecto es una aplicación Full-Stack desarrollada para la gestión de turnos y reservas. Cuenta con una interfaz de usuario interactiva creada en React y un servidor backend construido con Node.js y Express.

## Características Principales

* **Frontend (React/Vite):** * Interfaz pública para visualización de horarios y creación de reservas.
    * Panel administrativo protegido (Login) para la gestión (CRUD) de la oferta de turnos.
    * Validaciones de solapamiento de horarios y control de capacidad máxima (cupos).
* **Backend (Node.js/Express):**
    * API RESTful para el CRUD de turnos.
    * Base de datos basada en archivos (`turnos.json`) para persistencia local.
    * Middlewares personalizados para la validación de integridad y coherencia de datos.
    * Endpoints de monitoreo de salud del servidor (RAM, CPU, OS).

## Endpoints de Salud (Health Checks)
* `GET /health`: Retorna un JSON con el estado del servidor, uso de memoria, CPU y detalles del sistema operativo. (http://localhost:3000/health)
* `GET /health/report`: Genera y descarga automáticamente un archivo `report.txt` con las métricas del servidor. (http://localhost:3000/health/report)

---

## Instrucciones de Instalación y Ejecución

Para correr este proyecto en un entorno local, sigue estos pasos:

### 1. Instalación de Dependencias
Debido a que el proyecto consta de dos partes (Frontend y Backend), es necesario instalar las dependencias en ambos directorios.

Abre una terminal en la carpeta raíz del proyecto y ejecuta:
\`\`\`bash
npm install
\`\`\`

Luego, navega a la carpeta del backend y repite el proceso:
\`\`\`bash
cd backend
npm install
\`\`\`

### 2. Levantar el Servidor Backend
En la misma terminal, dentro de la carpeta `backend`, ejecuta el siguiente comando para iniciar el servidor (se ejecutará en el puerto 3000):
\`\`\`bash
npx nodemon server.js
\`\`\`

### 3. Levantar la Aplicación Frontend
Abre una **nueva terminal** en la raíz del proyecto (fuera de la carpeta backend) y ejecuta:
\`\`\`bash
npm run dev
\`\`\`

La terminal te indicará la ruta local (generalmente `http://localhost:5173`) donde podrás visualizar la aplicación en tu navegador.
