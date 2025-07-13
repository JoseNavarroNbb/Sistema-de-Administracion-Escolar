# Sistema de Administración Escolar

## Descripción General
Este proyecto es un sistema de administración escolar basado en web, diseñado para que los maestros gestionen a sus alumnos asignados, tomen lista, asignen tareas, registren participaciones, gestionen proyectos y califiquen exámenes. Los maestros pueden iniciar sesión, ver a los alumnos por salón y ciclo escolar, y calcular calificaciones basadas en ponderaciones predefinidas (30% tareas, 10% participaciones, 30% proyectos, 30% exámenes, donde 100% equivale a una calificación de 10). Los maestros también pueden cargar su fotografía de perfil, registrar su nombre completo y asociarse a una escuela y nivel educativo.

## Tecnologías
- **Backend**: Laravel 12 (framework PHP)
- **Frontend**: Vue 3 (framework JavaScript)
- **Base de Datos**: PostgreSQL
- **Contenerización**: Docker
- **Autenticación**: Laravel Sanctum
- **Almacenamiento**: Laravel Filesystem para manejar fotografías de perfil de maestros
- **Documentación de APIs**: OpenAPI/Swagger (opcional, puede integrarse)
- **Herramientas de Desarrollo**: Composer, Node.js, npm, Docker Compose

## Funcionalidades
- **Autenticación de Maestros**: Inicio de sesión seguro para maestros.
- **Perfil de Maestro**: Gestión de datos del maestro (nombre completo, fotografía, escuela, nivel educativo).
- **Gestión de Alumnos**: Visualización de alumnos asignados a un maestro por salón y ciclo escolar.
- **Toma de Lista**: Registro de asistencia diaria de los alumnos.
- **Sistema de Calificaciones**: Gestión de tareas, participaciones, proyectos y exámenes con ponderaciones configurables (30% tareas, 10% participaciones, 30% proyectos, 30% exámenes).
- **Cálculo de Calificaciones**: Cálculo automático de calificaciones finales basado en ponderaciones (100% = 10/10).
- **Catálogo de Escuelas**: Gestión de escuelas y sus niveles educativos (primaria, secundaria, bachillerato, universidad).

## Estructura del Proyecto
```
sistema-admin-escolar/
├── app/                    # Código de la aplicación Laravel
├── bootstrap/              # Archivos de arranque de Laravel
├── config/                 # Archivos de configuración
├── database/               # Migraciones, seeders y scripts de base de datos
├── public/                 # Activos públicos y punto de entrada
├── resources/              # Componentes Vue y activos frontend
├── routes/                 # Rutas API y web
├── storage/                # Almacenamiento de archivos (e.g., fotos de perfil)
├── tests/                  # Pruebas automatizadas
├── docker/                 # Archivos de configuración de Docker
├── .env.example            # Plantilla de configuración de entorno
├── composer.json           # Dependencias PHP
├── package.json            # Dependencias Node.js
├── vite.config.js          # Configuración de Vite para Vue
├── Dockerfile              # Configuración de Docker para Laravel
├── docker-compose.yml      # Configuración de Docker Compose
└── README.md               # Documentación del proyecto
```

## Estructura de la Base de Datos
La base de datos está diseñada en PostgreSQL con las siguientes tablas principales:
- `users`: Almacena datos de autenticación de maestros.
- `teachers`: Almacena información de perfiles de maestros (nombre, foto, escuela, nivel).
- `schools`: Catálogo de escuelas con sus niveles educativos.
- `classrooms`: Salones asignados a maestros para ciclos escolares específicos.
- `students`: Alumnos asignados a salones.
- `attendance_records`: Registros diarios de asistencia de alumnos.
- `assignments`: Tareas, participaciones, proyectos y exámenes con calificaciones.
- `grading_criteria`: Almacena ponderaciones de calificaciones para cada salón.
- `academic_cycles`: Define ciclos escolares (e.g., 2025-2026).

Consulta `database/scripts/create_tables.sql` para el esquema completo.

## APIs
El sistema expone APIs RESTful para las funcionalidades principales, construidas con Laravel 12 y aseguradas con Sanctum.

### Autenticación
- `POST /api/login`: Autentica a un maestro y devuelve un token.
- `POST /api/logout`: Cierra la sesión de un maestro.
- `POST /api/register`: Registra un nuevo maestro (opcional, solo para administradores).

### Gestión de Maestros
- `GET /api/teachers`: Lista todos los maestros (solo administradores).
- `GET /api/teachers/{id}`: Obtiene detalles del perfil de un maestro.
- `PUT /api/teachers/{id}`: Actualiza el perfil de un maestro (nombre, foto, escuela, nivel).
- `POST /api/teachers/{id}/picture`: Carga la foto de perfil de un maestro.

### Gestión de Salones
- `GET /api/classrooms`: Lista los salones asignados al maestro autenticado.
- `GET /api/classrooms/{id}/students`: Lista los alumnos de un salón específico.
- `GET /api/classrooms/{id}/academic-cycle`: Obtiene detalles del ciclo escolar de un salón.

### Asistencia
- `POST /api/classrooms/{id}/attendance`: Registra la asistencia de alumnos en un salón.
- `GET /api/classrooms/{id}/attendance`: Visualiza los registros de asistencia de un salón.

### Calificaciones
- `POST /api/classrooms/{id}/assignments`: Crea una nueva tarea, participación, proyecto o examen.
- `PUT /api/assignments/{id}`: Actualiza detalles o calificaciones de una asignación.
- `GET /api/classrooms/{id}/grades`: Visualiza las calificaciones de los alumnos en un salón.
- `POST /api/classrooms/{id}/grading-criteria`: Establece ponderaciones de calificación (por defecto: 30% tareas, 10% participaciones, 30% proyectos, 30% exámenes).
- `GET /api/classrooms/{id}/final-grades`: Calcula y visualiza las calificaciones finales de los alumnos.

## Instrucciones de Configuración
### Prerrequisitos
- Docker y Docker Compose
- Node.js y npm
- Composer
- Git

### Pasos para Levantar el Proyecto
1. **Clonar el Repositorio**
   ```bash
   git clone https://github.com/tu-repositorio/sistema-admin-escolar.git
   cd sistema-admin-escolar
   ```

2. **Copiar el Archivo de Entorno**
   ```bash
   cp .env.example .env
   ```
   Actualiza `.env` con tus credenciales de PostgreSQL y otras configuraciones:
   ```env
   DB_CONNECTION=pgsql
   DB_HOST=postgres
   DB_PORT=5432
   DB_DATABASE=school_admin
   DB_USERNAME=admin
   DB_PASSWORD=secret
   ```

3. **Instalar Dependencias**
   ```bash
   composer install
   npm install
   ```

4. **Configurar Docker**
   Inicia los contenedores con Docker Compose:
   ```bash
   docker-compose up -d
   ```

5. **Ejecutar Migraciones de la Base de Datos**
   Aplica el esquema de la base de datos:
   ```bash
   docker-compose exec app php artisan migrate
   ```

6. **Poblar la Base de Datos** (opcional)
   Inserta datos de prueba:
   ```bash
   docker-compose exec app php artisan db:seed
   ```

7. **Compilar Activos Frontend**
   Compila los activos de Vue 3 con Vite:
   ```bash
   npm run dev
   ```

8. **Acceder a la Aplicación**
   - API Backend: `http://localhost:8000/api`
   - Frontend: `http://localhost:3000` (servidor de desarrollo de Vite)
   - PostgreSQL: `localhost:5432`

## Ejecución de Pruebas
Ejecuta el conjunto de pruebas para verificar el funcionamiento:
```bash
docker-compose exec app php artisan test
```

## Configuración de Docker
El archivo `docker-compose.yml` define los servicios para:
- **Aplicación Laravel**: PHP 8.2 con Laravel 12
- **PostgreSQL**: Servidor de base de datos
- **Nginx**: Servidor web
- **Node**: Para desarrollo frontend (Vite)

## Guías de Desarrollo
- Sigue los principios de **Código Limpio** (SOLID, DRY, KISS).
- Usa las convenciones de nomenclatura de Laravel para modelos, controladores y migraciones.
- Escribe APIs RESTful con puntos finales claros y versionados.
- Utiliza la API de Composición de Vue 3 para componentes frontend.
- Asegúrate de que las migraciones de la base de datos sean idempotentes y reversibles.
- Escribe pruebas unitarias y de funcionalidad para características críticas.

## Mejoras Futuras
- Agregar control de acceso basado en roles (e.g., administrador, maestro).
- Implementar documentación de API con Swagger.
- Añadir soporte para exportar calificaciones a PDF usando LaTeX.
- Integrar notificaciones en tiempo real para actualizaciones de asignaciones.