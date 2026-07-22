# Requisitos de Documentación — Synthera Labs

Todo proyecto entregado por Synthera Labs debe incluir la documentación mínima definida en este documento. Un proyecto sin documentación no está terminado.

---

## Documentación Obligatoria

### README.md

El README es el punto de entrada de cualquier proyecto. Debe incluir:

```markdown
# Nombre del Proyecto

Descripción breve de qué hace el proyecto y para quién.

## Requisitos
Lista de lo necesario para correr el proyecto.

## Instalación
Pasos para instalar el proyecto localmente.

## Configuración
Variables de entorno necesarias y su descripción.

## Uso
Cómo usar el proyecto con ejemplos concretos.

## Arquitectura
Descripción de la estructura del proyecto y decisiones técnicas clave.

## API
Enlace a documentación de API o descripción de endpoints principales.

## Despliegue
Cómo hacer deploy a producción.

## Changelog
Enlace al CHANGELOG.md.
```

---

### CHANGELOG.md

Registro de todos los cambios por versión. Seguir el formato [Keep a Changelog](https://keepachangelog.com).

```markdown
# Changelog

## [1.2.0] - 2026-07-01
### Agregado
- Autenticación con JWT.
- Endpoint de recuperación de contraseña.

### Corregido
- Error en validación de email duplicado.

## [1.1.0] - 2026-06-01
### Agregado
- CRUD completo de usuarios.
```

---

### .env.example

Plantilla de todas las variables de entorno necesarias, con descripción y sin valores reales.

```env
# Base de datos
DATABASE_URL=postgresql://usuario:contraseña@localhost:5432/nombre_db

# Autenticación
JWT_SECRET=tu_secreto_aqui
JWT_EXPIRATION=7d

# API Keys externas
OPENAI_API_KEY=tu_api_key_aqui
```

---

## Documentación de API

Para proyectos con API REST, la documentación debe incluir:

- Descripción de cada endpoint.
- Método HTTP y ruta.
- Parámetros de entrada con tipos y validaciones.
- Estructura de respuesta exitosa.
- Estructura de respuesta de error.
- Ejemplos de request y response.

Herramientas aceptadas: **Swagger/OpenAPI** (preferido), Postman Collection, o Markdown estructurado.

---

## Documentación de Arquitectura

Para proyectos medianos y grandes, incluir en `docs/architecture.md`:

- Diagrama de componentes.
- Decisiones de diseño y justificaciones.
- Flujos principales del sistema.
- Dependencias externas y por qué se eligieron.

---

## Checklist de Entrega

Antes de cerrar cualquier proyecto, verificar:

- [ ] `README.md` completo con todas las secciones.
- [ ] `CHANGELOG.md` con los cambios de esta versión.
- [ ] `.env.example` actualizado con todas las variables.
- [ ] Documentación de API actualizada.
- [ ] Tests documentados y pasando.
- [ ] Instrucciones de deploy verificadas.

---

<sub>Synthera Labs · Standards v1.0.0</sub>
