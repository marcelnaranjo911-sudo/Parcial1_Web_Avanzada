# Sistema de Gestión de Turnos - Examen Parcial de Programación Web Avanzada

## Descripción del Proyecto

Este repositorio contiene la implementación del Primer Examen Parcial de la asignatura Programación Web Avanzada. Consiste en una aplicación de página única (SPA) desarrollada con React para la gestión de turnos y reservas de clientes. 

El sistema opera enteramente en el lado del cliente (frontend), prescindiendo de un servidor externo y garantizando la persistencia de los datos a través del almacenamiento local del navegador, cumpliendo estrictamente con los requerimientos establecidos en el documento de evaluación.

### Tecnologías y Herramientas Utilizadas
Para el desarrollo de este proyecto se instalaron y configuraron las siguientes tecnologías:
* **React (mediante Vite):** Seleccionado por su rapidez en la compilación y entorno de desarrollo optimizado.
* **React Router Dom:** Para la gestión de rutas anidadas y protegidas dentro de la aplicación.
* **Dexie.js (y dexie-react-hooks):** Librería principal para interactuar con IndexedDB de manera declarativa y obtener datos reactivos (`useLiveQuery`).

## Instrucciones de Instalación y Ejecución

Para evaluar el proyecto de manera local, siga los siguientes pasos:

1.  Descomprimir el archivo `.rar` que contiene el repositorio del proyecto.
2.  Abrir una terminal de comandos apuntando al directorio raíz del proyecto extraído.
3.  Ejecutar el siguiente comando para instalar todas las dependencias declaradas en el `package.json`:
    ```bash
    npm install
    ```
4.  Una vez finalizada la instalación, iniciar el servidor de desarrollo mediante el comando:
    ```bash
    npm run dev
    ```
5.  Abrir el navegador web e ingresar a la dirección local que indique la terminal (por defecto, suele ser `http://localhost:5173`).

**Credenciales de Acceso Administrador:**
* **Correo Electrónico:** admin@admin.com
* **Contraseña:** admin123

## Estructura del Proyecto

El código fuente ha sido organizado modularmente para separar la lógica de negocio de la interfaz de usuario. A continuación, se detalla la función de cada directorio dentro de `src/`:

* **`/components`**: Contiene componentes de interfaz de usuario reutilizables. Aquí se encuentra `Modal.jsx` (encargado de renderizar superposiciones para los formularios), `ReservaForm.jsx`, `TurnoForm.jsx` y los estilos compartidos de la aplicación.
* **`/context`**: Aloja la configuración del estado global. En esta carpeta reside `AuthContext.jsx`, que provee el estado de la sesión activa a todo el árbol de componentes.
* **`/db`**: Contiene la configuración inicial de la base de datos local. El archivo `db.js` define el esquema de las tablas (`usuarios`, `turnos`, `reservas`) para IndexedDB.
* **`/hooks`**: Centraliza la lógica de negocio mediante custom hooks. Esto incluye `useTurnos.js`, `useReservas.js`, `useAuth.js` y `useStorage.js`, asegurando que los componentes de la vista se mantengan limpios y enfocados solo en la presentación.
* **`/layouts`**: Contiene los envoltorios estructurales de la aplicación. `PublicLayout.jsx` define la barra de navegación para usuarios normales, mientras que `AdminLayout.jsx` estructura el panel de control.
* **`/pages`**: Define las vistas principales enrutadas, tales como `TurnosDisponibles.jsx` (pantalla pública), `Login.jsx` y las pantallas de administración (`CRUDTurnos.jsx` y `ListadoReservas.jsx`).

## Cumplimiento de Requerimientos y Decisiones de Arquitectura

Para facilitar la revisión, a continuación se detalla dónde se encuentran las soluciones a los requerimientos clave exigidos:

### 1. Justificación de Persistencia (IndexedDB vs LocalStorage)
Se optó por utilizar **IndexedDB** (a través de Dexie.js) en lugar de `localStorage`. Esta decisión técnica, implementada en la carpeta `/db` y consumida en `/hooks/useStorage.js`, se basa en:
* **Capacidad y Rendimiento:** IndexedDB permite almacenar grandes volúmenes de datos de forma asíncrona, evitando bloquear el hilo principal de la aplicación.
* **Consultas Estructuradas:** Permite crear índices, lo que facilita filtrar información eficientemente (por ejemplo, buscar todas las reservas activas que pertenecen a un ID de turno específico).
* **Reactividad:** Integrado con `useLiveQuery`, cualquier cambio en la base de datos actualiza el estado de React automáticamente, sin necesidad de recargar manualmente el árbol de componentes.

### 2. Implementación de Rutas Protegidas
La protección del panel de administración se encuentra implementada en **`/layouts/AdminLayout.jsx`**. Este componente verifica el estado de autenticación consumiendo `useAuth()`. Si un usuario no está autenticado e intenta acceder a una ruta hija de `/admin`, el componente retorna un `<Navigate to="/login" />`, bloqueando el acceso de forma segura en el lado del cliente.

### 3. Separación de Lógica en Custom Hooks
Toda interacción con la base de datos y validación de reglas se delegó a los hooks personalizados:
* En **`useTurnos.js`**, la función `comprobarSolapamiento` evalúa la lógica matemática para impedir que un administrador cree o edite un turno si este choca con los horarios de otro turno en la misma fecha.
* En **`useReservas.js`**, la función `crearReserva` intercepta la creación para validar que los cupos actuales no superen la capacidad máxima del turno.

### 4. Validaciones de Negocio Destacadas
* **Formato de Carnet:** En **`/components/ReservaForm.jsx`**, se utiliza una expresión regular (`/^\d{11}$/`) en el evento `handleSubmit` para asegurar que el usuario ingrese exactamente 11 dígitos numéricos, emitiendo un error en pantalla si el formato es incorrecto.
* **Gestión de Cupos y Fechas:** En **`TurnosDisponibles.jsx`**, el cálculo del cupo restante se realiza en tiempo real filtrando las reservas con estado "activa". La interfaz bloquea el botón de reserva automáticamente cuando se alcanza la capacidad máxima. Además, el formulario de creación de turnos impide registrar un turno cuya hora de inicio sea mayor o igual a la hora de fin.
