# Arquitectura General — Synthera Labs

## Principios Base

Todo sistema construido en Synthera Labs debe cumplir estos principios antes de escribir la primera línea de código.

| Principio | Descripción |
|---|---|
| **Separación de responsabilidades** | Cada módulo, clase y función tiene una única responsabilidad clara. |
| **Dependencias hacia adentro** | Las capas externas dependen de las internas, nunca al revés. |
| **Abstracciones sobre implementaciones** | El dominio no conoce la base de datos ni el framework. |
| **Fácil de probar** | Si algo es difícil de testear, la arquitectura está mal. |
| **Fácil de cambiar** | Cambiar el motor de base de datos o el framework no debe romper el negocio. |

---

## Capas de la Arquitectura

```
┌─────────────────────────────────┐
│         Presentación            │  HTTP, CLI, WebSocket
├─────────────────────────────────┤
│         Aplicación              │  Casos de uso, DTOs
├─────────────────────────────────┤
│           Dominio               │  Entidades, reglas de negocio
├─────────────────────────────────┤
│        Infraestructura          │  DB, APIs externas, email
└─────────────────────────────────┘
```

**Regla de dependencia:** Las flechas de dependencia apuntan siempre hacia el dominio. El dominio no importa nada de infraestructura.

---

## Estructura de Carpetas Estándar

```
src/
├── domain/
│   ├── entities/        → Entidades del negocio
│   ├── repositories/    → Interfaces (contratos)
│   └── services/        → Lógica de dominio pura
├── application/
│   ├── use-cases/       → Casos de uso específicos
│   └── dtos/            → Objetos de transferencia de datos
├── infrastructure/
│   ├── database/        → Implementaciones de repositorios
│   ├── http/            → Clientes HTTP externos
│   └── email/           → Servicios de email
└── presentation/
    ├── controllers/     → Controladores HTTP
    ├── middlewares/     → Middleware de autenticación, validación
    └── routes/          → Definición de rutas
```

---

## Decisiones de Diseño

Antes de iniciar cualquier proyecto, documentar en el README:

1. **¿Por qué este stack?** — Justificación de las tecnologías elegidas.
2. **¿Cuáles son los límites del sistema?** — Qué hace y qué no hace.
3. **¿Cómo escala?** — Estrategia de escalado horizontal o vertical.
4. **¿Cuáles son los puntos críticos de falla?** — Y cómo se mitigan.

---

## API Design

- Seguir principios REST con recursos en plural y sustantivos.
- Versionar todas las APIs desde el inicio: `/api/v1/`.
- Respuestas consistentes en toda la API.

```json
{
  "success": true,
  "data": {},
  "error": null,
  "meta": {
    "page": 1,
    "total": 100
  }
}
```

---

## Environments

Todo proyecto debe tener tres entornos definidos:

| Entorno | Propósito | Base de datos |
|---|---|---|
| `development` | Desarrollo local | Local o Docker |
| `staging` | Pruebas antes de producción | Separada de producción |
| `production` | Usuarios reales | Protegida con backups |

Nunca usar datos reales en `development` ni en `staging`.

---

<sub>Synthera Labs · Standards v1.0.0</sub>
