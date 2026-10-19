# Arquitectura (Etapa 1)

## Componentes y responsabilidades

| Componente | Tecnología | Responsabilidad |
|---|---|---|
| frontend | React + Vite + Tailwind, servido por nginx | Interfaz, validación de formularios, rutas privadas; nginx reenvía `/api/` al backend |
| backend | FastAPI (Python 3.12) | API REST, autenticación, permisos, validación, lógica de negocio, cliente de IA |
| db | PostgreSQL 16 | Datos persistentes en el volumen `pgdata` |

Módulos del backend: `auth`, `users`, `categories`, `services`, `requests`, `admin`, `ai`.
Capas: routers → servicios (lógica) → modelos SQLAlchemy. Migraciones con Alembic.

## Funcionalidades por módulo

- **Usuarios:** registro, login/logout, editar perfil, roles `user` y `admin`.
- **Servicios:** CRUD de servicios propios, categoría, precio o gratuito, ubicación general, contacto, estado y fecha, búsqueda y filtros.
- **Solicitudes:** un interesado envía una solicitud; el prestador la ve y cambia su estado; el solicitante consulta el suyo.
- **Admin:** listar/gestionar usuarios y servicios, desactivar publicaciones. Los permisos se validan en el backend.
- **IA:** generar descripción, sugerir categoría, mejorar redacción, búsqueda en lenguaje natural.

## Decisiones técnicas

1. **JWT en cabecera `Authorization`** con contraseñas hasheadas con argon2id. Simple y compatible con Swagger.
2. **Roles como columna** `role` en `users` (no tablas de permisos): solo hay dos roles.
3. **Migraciones como única fuente del esquema:** no habrá SQL de inicialización duplicado.
4. **Admin inicial** desde `ADMIN_EMAIL`/`ADMIN_PASSWORD` del `.env`; datos demo solo con `LOAD_DEMO_DATA=true`, marcados como demo.
5. **IA:** la clave viene de `ANTHROPIC_API_KEY`. Sin clave, la API responde 503 con mensaje claro y el resto sigue funcionando. Solo se envían título, descripción, categoría y ubicación general: nunca contraseñas, correos ni teléfonos. Límite por usuario por hora (`AI_RATE_LIMIT_PER_HOUR`).
6. **Búsqueda en lenguaje natural:** el modelo solo devuelve filtros (categoría, ubicación, gratuito, precio máx., palabras clave); el backend los valida contra una lista permitida y ejecuta consultas SQLAlchemy normales. El modelo nunca escribe SQL.
7. **Puertos:** frontend 8080 y backend 8000 (configurables). PostgreSQL no se publica al host, para evitar conflictos.
8. **Pruebas:** unitarias en `backend/tests/`; de integración entre contenedores en `tests/`.

## Contrato entre etapas

- El backend debe exponer `GET /api/health` (lo usa el healthcheck), Swagger en `/api/docs`, y los módulos `alembic/` y `app.seed`.
- El frontend debe tener `package.json` y `package-lock.json` (el Dockerfile usa `npm ci`).
