# Trabajo de diseño de sistemas de internet

**Estudiantes**:
-Francelia Michell Lopez Alonzo
-Anthony Alexander Aguilar Parrales
-Yessbelin Valezca Alvarado 

## Descripción
Proyecto de práctica que expone un API REST simple para gestionar usuarios (CRUD). El servicio usa almacenamiento en memoria para facilitar pruebas rápidas y documentación con Swagger.

## Endpoints principales
# Trabajo de diseño de sistemas de internet

**Estudiantes**
- Francelia Michell Lopez Alonzo
- Anthony Alexander Aguilar Parrales
- Yessbelin Valezka Alvarado

Descripción
-----------
Proyecto docente / de práctica que expone APIs REST construidos con NestJS. El repositorio contiene dos carpetas de interés:

- `[Trabajo-de-diseno-](.)`: aplicación principal entregada para la práctica (usuarios, Swagger, Prisma).
- `[nest-app](../nest-app)`: proyecto Nest adicional generado durante la sesión (esqueleto creado por `nest new`).

Estructura principal (resumen)
-------------------------------

- [src/](src) — código fuente de la aplicación principal.
  - [src/user](src/user) — controlador/servicio/DTOs de usuarios.
  - [src/auth](src/auth) — módulo `auth` generado (controlador, servicio, módulo). See [src/auth/auth.module.ts](src/auth/auth.module.ts#L1-L80).
  - [src/prisma](src/prisma) — módulo/servicio para usar Prisma (si existe).
- [prisma](prisma) — esquemas de Prisma (si están presentes).

Qué se añadió durante esta sesión
---------------------------------

- Se generó el módulo `auth`, servicio y controlador usando el CLI de Nest:

  - `npx -y @nestjs/cli new nest-app --package-manager npm --skip-install` (scaffold opcional en `nest-app`)
  - `npx -y nest generate module auth`
  - `npx -y nest generate service auth`
  - `npx -y nest generate controller auth`

- Archivos importantes generados en este repo:
  - [src/auth/auth.module.ts](src/auth/auth.module.ts#L1-L200)
  - [src/auth/auth.service.ts](src/auth/auth.service.ts#L1-L200)
  - [src/auth/auth.controller.ts](src/auth/auth.controller.ts#L1-L200)

Descripción del módulo `auth`
----------------------------

- `AuthModule` registra `JwtModule` con una clave de ejemplo (`your-secret-key`) y exporta `AuthService`.
- `AuthService` usa `PrismaService` y `JwtService` para validar credenciales y firmar tokens JWT.
- `AuthController` expone dos rutas principales:
  - `GET /auth/ping` — punto de comprobación (retorna `{ ok: true }`).
  - `POST /auth/login` — recibe `LoginDto` y devuelve `{ access_token: string }` si las credenciales son válidas.

Requisitos y variables de entorno
---------------------------------

- Node.js (recomendado >= 18). npm (>= 9).
- Variables útiles (crear un archivo `.env` en la raíz del proyecto):

```env
JWT_SECRET=your-secret-key
JWT_EXPIRES_IN=1h
DATABASE_URL=file:./dev.db
```

Nota: Actualmente el secret está en el código como ejemplo; muévelo a `process.env.JWT_SECRET` en `JwtModule.register()` para producción.

Instalación y ejecución (aplicación principal)
--------------------------------------------

1. Abrir la carpeta del proyecto principal:

```bash
cd Trabajo-de-diseno-
```

2. Instalar dependencias:

```bash
npm install
```

3. Ejecutar en modo desarrollo:

```bash
npm run start:dev
```

4. Swagger UI disponible en `http://localhost:3000/api` (si está configurado en `src/main.ts`).

Instalación y ejecución (carpeta `nest-app`)
-----------------------------------------

Si quieres usar el proyecto scaffold creado en `nest-app`:

```bash
cd nest-app
npm install
npm run start:dev
```

Endpoints útiles
----------------

- Usuarios (descrito en la entrega original):
  - `POST /users` — crear usuario
  - `GET /users` — listar usuarios
  - `GET /users/:id` — obtener usuario
  - `PUT /users/:id` — actualizar usuario
  - `DELETE /users/:id` — eliminar usuario

- Autenticación (módulo `auth` generado):
  - `GET /auth/ping` — comprobación
  - `POST /auth/login` — login (cuerpo: `LoginDto` — usualmente `email` y `password`)

Prisma y persistencia
---------------------

- El proyecto incluye dependencias de Prisma y un `PrismaService` que actúa como wrapper del cliente generado. Si quieres usar la BD local con SQLite:

```bash
npx prisma migrate dev --name init
npm run start:dev
```

Notas de Git y remote
---------------------

- Se añadió el repo remoto `https://github.com/AnthonyAguilarP/JWT.git` y se hizo push.
- Advertencia detectada al añadir archivos: existe un repositorio Git embebido en `Trabajo-de-diseno-`.
  - Si ese comportamiento fue intencional y quieres que sea un submódulo, usa:

```bash
git submodule add <url> Trabajo-de-diseno-
```

  - Si prefieres eliminar la carpeta embebida del índice del repo principal (dejándola como carpeta normal), ejecuta desde la raíz del repo principal:

```bash
git rm --cached -r Trabajo-de-diseno-
echo "Trabajo-de-diseno-/" >> .gitignore
git commit -m "Remove embedded repo from index and ignore it"
```

Buenas prácticas y próximos pasos sugeridos
-----------------------------------------

- Mover secretos fuera del código y usar `.env` + `@nestjs/config`.
- Añadir validación y `@ApiProperty()` a DTOs para mejorar Swagger.
- Añadir tests unitarios para `AuthService` y e2e para el flujo de login.
- Si prefieres consolidar en un solo proyecto, decide si `nest-app` o `Trabajo-de-diseno-` será la base y elimina la otra carpeta o fusiónala.

Contacto
-------

Si quieres, puedo:

- Ajustar `JwtModule` para leer `process.env.JWT_SECRET` y añadir `@nestjs/config`.
- Eliminar el sub-repositorio embebido y añadir un `.gitignore` correcto.
- Añadir ejemplos de uso con `httpie` o Postman collection.

---
Archivo actualizado con documentación completa del proyecto.

# JWT NestJS Project

Este repositorio contiene una API back-end desarrollada con NestJS que implementa gestión básica de usuarios, autenticación por JWT, y una capa de acceso a datos con Prisma (SQLite por defecto). El proyecto incluye documentación Swagger y utilidades para pruebas y formateo.

**Estado:** Código fuente presente; ver `src/` para lógica principal.

**Contenido de este README**
- Descripción
- Características
- Estructura del proyecto
- Endpoints principales
- Esquema de la base de datos (Prisma)
- Dependencias (librerías usadas)
- Scripts útiles
- Configuración y variables de entorno
- Instalación y ejecución
- Migraciones y Prisma
- Testing
- Notas de seguridad


**Descripción**

La aplicación expone endpoints para la gestión de usuarios (CRUD en memoria) y un endpoint de autenticación que valida usuario y contraseña con almacenamiento en Prisma/SQLite y entrega un token JWT firmado.

**Características**

- Endpoints REST para `users` (crear, listar, obtener por id, actualizar, eliminar).
- Autenticación con JWT: `POST /auth/login` devuelve `access_token` si las credenciales son válidas.
- Hashing de contraseñas usando `bcrypt` (comparación en `AuthService`).
- Documentación de la API vía Swagger en `/api`.
- ORM Prisma configurado para SQLite (archivo `prisma/Schema.prisma`).


**Estructura del proyecto**

- `src/main.ts`: arranca la app y registra Swagger.
- `src/app.module.ts`: módulo raíz que importa `UserModule` y `AuthModule`.
- `src/user/`: controladores, DTOs y servicio de usuario (implementación en memoria en `UserService`).
- `src/auth/`: controlador y servicio de autenticación que usan `JwtService`, `PrismaService` y `bcrypt`.
- `src/prisma/`: `PrismaModule`/`PrismaService` que extiende el cliente generado por Prisma.
- `prisma/Schema.prisma`: esquema de la base de datos y generador del cliente.


**Endpoints principales**

- `GET /auth/ping` — health check simple que devuelve { ok: true }.
- `POST /auth/login` — body: `{ "email": string, "password": string }` (ver DTO `src/user/dto/login.dto.ts`). Devuelve `{ access_token: string }` si credenciales válidas.
- `POST /users` — crea usuario (usa `CreateUserDto`).
- `GET /users` — lista todos los usuarios (implementación en memoria en `UserService`).
- `GET /users/:id` — obtiene usuario por id.
- `PUT /users/:id` — actualiza usuario.
- `DELETE /users/:id` — elimina usuario.


**Esquema de la base de datos (resumen)**

Archivo: `prisma/Schema.prisma` — resumen del modelo `User`:

- `id` (Int, autoincremental) — PK
- `email` (String, unique)
- `name` (String?)
- `password` (String)
- `telephone` (String?)
- `createdAt` (DateTime)
- `updatedAt` (DateTime)
- `role` (enum Role: USER | ADMIN)


**Librerías / Dependencias usadas**

Dependencias (runtime):

- `@nestjs/common`, `@nestjs/core`, `@nestjs/platform-express` — framework NestJS.
- `@nestjs/jwt` — utilidades JWT para Nest.
- `@nestjs/swagger` + `swagger-ui-express` — generación y UI de documentación OpenAPI/Swagger.
- `bcrypt` — comparación y hashing de contraseñas.
- `reflect-metadata`, `rxjs` — requeridas por Nest/TypeScript runtime.

Dependencias de desarrollo (resumen):

- `typescript`, `ts-node`, `ts-jest`, `jest`, `supertest` — compilación y testing.
- `prisma`, `@prisma/client` — ORM/cliente para acceder a la base de datos.
- `eslint`, `prettier` y plugins — lint y formateo.

Para ver las versiones exactas, consulte `package.json`.


**Scripts disponibles** (ver `package.json`)

- `npm run start` — arranca la app (producción si está en `dist`).
- `npm run start:dev` — arranque en modo watch (desarrollo).
- `npm run build` — compila con Nest.
- `npm run format` — formatea con Prettier.
- `npm run lint` — ejecuta ESLint y aplica fixes.
- `npm run test` — ejecuta tests con Jest.
- `npm run test:e2e` — ejecuta tests E2E.


**Configuración y variables de entorno**

Variables esperadas:

- `DATABASE_URL` — URL de conexión para Prisma. Por defecto el `datasource` en `prisma/Schema.prisma` usa provider `sqlite` y la URL suele apuntar a un archivo sqlite: `file:./dev.db` o similar.
- `PORT` — puerto en el que corre la app (por defecto 3000 si no está definida).
- `JWT_SECRET` — (no implementado explícitamente en env en el módulo actual) el `JwtModule` en `src/auth/auth.module.ts` usa actualmente el string `'your-secret-key'`. Se recomienda extraerlo a una variable de entorno `JWT_SECRET` y no dejarlo hardcodeado.


**Instalación y ejecución (local)**

1. Clonar el repositorio

  git clone https://github.com/AnthonyAguilarP/JWT.git
  cd JWT

2. Instalar dependencias

  npm install

3. Configurar variables de entorno

  - Crear un fichero `.env` con `DATABASE_URL="file:./dev.db"` y opcional `PORT` y `JWT_SECRET`.

4. Generar cliente de Prisma (si cambian los modelos):

  npx prisma generate

5. Ejecutar migraciones (si aplica) o crear la base de datos:

  npx prisma migrate dev --name init

6. Ejecutar la aplicación en desarrollo

  npm run start:dev

7. Abrir Swagger UI

  http://localhost:3000/api


**Migraciones y Prisma**

- El esquema se encuentra en `prisma/Schema.prisma`.
- Para crear/actualizar migraciones use `npx prisma migrate dev --name <nombre>` y luego `npx prisma generate`.
- Para inspeccionar la base de datos puede usar `npx prisma studio`.


**Testing**

- Unit tests con Jest: `npm run test`.
- E2E tests: `npm run test:e2e`.


**Notas de seguridad y recomendaciones**

- Nunca dejar secret keys hardcodeadas (`your-secret-key` en `AuthModule`). Usar variables de entorno (`JWT_SECRET`) y `ConfigModule` de Nest para centralizar.
- Asegurar que las contraseñas estén hasheadas al persistir (en el código actual el `UserService` de ejemplo almacena en memoria; al usar Prisma y persistencia, siempre hashear con `bcrypt.hash` antes de guardar).
- Validar y sanitizar entradas (usar `class-validator` y `ValidationPipe` si se desea).


**Guard JWT y protección del endpoint `GET /users`**

Se agregó un guard simple para exigir autorización en el endpoint `GET /users`.

- Archivo del guard: `src/auth/guards/jwt-auth-guards.ts`.
- Comportamiento: el guard comprueba la cabecera `Authorization` en formato `Bearer <token>` y verifica el token JWT usando la misma clave hardcodeada (`your-secret-key`) que está en `src/auth/auth.module.ts`. Si falta la cabecera o el token es inválido/expirado, responde con `401 Unauthorized`.

Propósito: facilitar una comprobación rápida de autorización en endpoints. En producción se recomienda extraer el secreto a `JWT_SECRET`, usar `Passport`/`@nestjs/passport` y `JwtStrategy`, y manejar usuarios desde la base de datos.

Prueba rápida (desde una terminal con `curl`):

- Sin cabecera Authorization (respuesta esperada):

  curl -i http://localhost:3000/users

  Respuesta: `401 Unauthorized` con cuerpo `{ "message": "Missing Authorization header", ... }`

- Con token inválido:

  curl -i -H "Authorization: Bearer invalid.token.here" http://localhost:3000/users

  Respuesta: `401 Unauthorized` con cuerpo `{ "message": "Invalid or expired token", ... }`

Cómo funciona en el código:

- `UserController.findAll()` está decorado con `@UseGuards(JwtAuthGuard)` y por tanto requiere autorización para poder ejecutarse.

