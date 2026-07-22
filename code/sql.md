# Estándares de SQL — Synthera Labs

## Motor de Base de Datos

- **PostgreSQL 16+** para proyectos en producción.
- **SQLite** solo para proyectos pequeños, prototipos o uso embebido.

---

## Nomenclatura

| Elemento | Convención | Ejemplo |
|---|---|---|
| Tablas | snake_case, plural | `usuarios`, `pedidos_detalle` |
| Columnas | snake_case | `nombre_completo`, `fecha_creacion` |
| Primary keys | `id` | `id` |
| Foreign keys | `{tabla_singular}_id` | `usuario_id` |
| Índices | `idx_{tabla}_{columna}` | `idx_usuarios_email` |
| Constraints | `{tipo}_{tabla}_{columna}` | `uq_usuarios_email` |

---

## Estructura de Tablas

Toda tabla debe incluir estas columnas base:

```sql
CREATE TABLE usuarios (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

- `id` siempre UUID, nunca entero autoincremental en producción.
- `created_at` y `updated_at` obligatorios en toda tabla.
- `deleted_at` para soft delete cuando aplique.

---

## Migraciones

- Usar migraciones versionadas. Nunca modificar la base de datos manualmente en producción.
- Cada migración debe tener `up` y `down`.
- Nombrar migraciones con timestamp: `20260101_crear_tabla_usuarios.sql`.

---

## Consultas

- Nunca usar `SELECT *` en producción. Especificar columnas explícitamente.
- Usar parámetros preparados. Nunca concatenar strings en consultas.
- Agregar índices en columnas usadas frecuentemente en `WHERE`, `JOIN` y `ORDER BY`.

```sql
-- ✅ Correcto
SELECT id, nombre, email
FROM usuarios
WHERE email = $1
  AND deleted_at IS NULL;

-- ❌ Incorrecto
SELECT * FROM usuarios WHERE email = '" + email + "'
```

---

## Seguridad

- Nunca almacenar contraseñas en texto plano. Usar bcrypt o argon2.
- Nunca exponer el `id` interno si puede inferirse información sensible.
- Restringir permisos del usuario de base de datos al mínimo necesario.

---

## Índices

```sql
-- Índice en columna de búsqueda frecuente
CREATE INDEX idx_usuarios_email ON usuarios(email);

-- Índice compuesto cuando se filtra por dos columnas juntas
CREATE INDEX idx_pedidos_usuario_estado ON pedidos(usuario_id, estado);
```

---

<sub>Synthera Labs · Standards v1.0.0</sub>
