# Git Workflow — Synthera Labs

## Modelo de Branching

Synthera Labs usa **Git Flow** como modelo de branching estándar.

```
main          → Código en producción. Siempre estable.
develop       → Rama de integración. Base para nuevas features.
feature/*     → Desarrollo de nuevas funcionalidades.
hotfix/*      → Correcciones urgentes sobre producción.
release/*     → Preparación de nuevas versiones.
```

---

## Reglas de Ramas

| Rama | Quién escribe | Merge hacia | Requiere PR |
|---|---|---|---|
| `main` | Nadie directamente | — | Sí, desde `release/*` o `hotfix/*` |
| `develop` | Nadie directamente | `main` | Sí, desde `feature/*` |
| `feature/*` | Desarrolladores | `develop` | Sí |
| `hotfix/*` | Desarrolladores | `main` y `develop` | Sí |
| `release/*` | Responsable de release | `main` y `develop` | Sí |

**Regla absoluta:** Nadie hace push directo a `main`. Sin excepción.

---

## Convención de Nombres de Ramas

```
feature/descripcion-corta
feature/autenticacion-jwt

hotfix/descripcion-corta
hotfix/error-login-produccion

release/version
release/1.2.0
```

Usar kebab-case. Sin mayúsculas. Sin caracteres especiales.

---

## Convención de Commits

Synthera Labs sigue **Conventional Commits**.

```
<tipo>(<alcance opcional>): <descripción en imperativo>
```

### Tipos permitidos

| Tipo | Cuándo usarlo |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `docs` | Cambios en documentación |
| `style` | Formato, espacios, puntos y comas (sin cambio de lógica) |
| `refactor` | Refactorización sin cambio de funcionalidad |
| `test` | Agregar o modificar tests |
| `chore` | Tareas de mantenimiento, dependencias |
| `perf` | Mejoras de rendimiento |
| `ci` | Cambios en configuración de CI/CD |

### Ejemplos

```
feat(auth): agregar autenticación con JWT
fix(usuarios): corregir validación de email duplicado
docs(readme): actualizar instrucciones de instalación
refactor(pedidos): extraer lógica de cálculo a servicio separado
chore(deps): actualizar fastapi a 0.110.0
```

---

## Proceso de Pull Request

1. Crear rama desde `develop` con nombre descriptivo.
2. Hacer commits con la convención definida.
3. Abrir Pull Request hacia `develop`.
4. El PR debe incluir:
   - Descripción de los cambios.
   - Capturas o evidencia si hay cambios visuales.
   - Tests que cubran los cambios.
5. Al menos una revisión aprobada antes de merge.
6. Resolver todos los comentarios antes de mergear.
7. Usar **Squash and Merge** para mantener el historial limpio.

---

## Tags y Versiones

Synthera Labs usa **Semantic Versioning** (`MAJOR.MINOR.PATCH`).

| Tipo | Cuándo incrementar |
|---|---|
| `MAJOR` | Cambios incompatibles con versión anterior |
| `MINOR` | Nueva funcionalidad compatible |
| `PATCH` | Corrección de bugs compatible |

```bash
git tag -a v1.2.0 -m "release: versión 1.2.0"
git push origin v1.2.0
```

---

<sub>Synthera Labs · Standards v1.0.0</sub>
