# Registro de prompts

Solo se registran prompts realmente enviados. Los pendientes se marcan como tal.

## P-001 — Prompt maestro (EJECUTADO)

- **Fecha:** 2026-10-18
- **Herramienta de IA:** Claude (Anthropic), app móvil de chat
- **Objetivo:** pedir el proyecto completo de registro de servicios sociales y profesionales, trabajado por 8 etapas.
- **Prompt completo:** archivo `prompts/P-001-prompt-maestro.txt`. **PENDIENTE DE COLOCAR:** el archivo subido a la conversación no quedó disponible en el entorno de Claude (carpeta de subidas vacía), así que el texto original debe copiarse ahí tal cual, sin modificarlo.
- **Resultado obtenido:** Etapa 1 (arquitectura y decisiones) y Etapa 2 (estructura de carpetas y configuración de Docker).
- **Archivos afectados:** `docker-compose.yml`, `.env.example`, `.gitignore`, `backend/Dockerfile`, `backend/.dockerignore`, `frontend/Dockerfile`, `frontend/nginx.conf`, `frontend/.dockerignore`, `docs/arquitectura.md`, `database/.gitkeep`, `tests/.gitkeep`.
- **Verificaciones realizadas:**
  - `docker-compose.yml` se leyó correctamente como YAML y define 3 servicios (db, backend, frontend).
  - Todas las variables `${...}` del compose existen en `.env.example`.
  - **NO realizado:** `docker compose config`, `docker compose build` y `docker compose up`, porque Docker no está disponible en el entorno de Claude. Deben ejecutarse en la computadora con Docker Desktop.
  - Los Dockerfiles aún no se pueden construir: faltan `backend/requirements.txt`, `alembic/`, `app/` y `frontend/package*.json` (Etapas 3 a 5).

## Pendientes (aún no ejecutados)

- Etapa 3: base de datos y migraciones.
- Etapa 4: backend.
- Etapa 5: frontend.
- Etapa 6: integración de IA.
- Etapa 7: pruebas y comunicación entre contenedores.
- Etapa 8: documentación final y README.
