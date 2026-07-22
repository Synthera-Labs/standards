# Estándares de TypeScript — Synthera Labs

## Versión

TypeScript 5.0 o superior. Obligatorio en todos los proyectos frontend y backend con Node.js.

---

## Configuración Base

Usar `strict: true` en `tsconfig.json` sin excepción.

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022"],
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## Formato y Estilo

- Usar **ESLint** con reglas estrictas.
- Usar **Prettier** como formateador.
- Longitud máxima de línea: **100 caracteres**.
- Punto y coma: **obligatorio**.
- Comillas: **simples**.

---

## Tipos

- Nunca usar `any`. Si es inevitable, documentar el motivo con un comentario.
- Preferir `interface` sobre `type` para objetos.
- Usar `type` para uniones y tipos utilitarios.

```typescript
// ✅ Correcto
interface Usuario {
  id: string;
  nombre: string;
  email: string;
}

type EstadoPedido = 'pendiente' | 'procesando' | 'completado' | 'cancelado';

// ❌ Incorrecto
const usuario: any = obtenerUsuario();
```

---

## Nombres

| Elemento | Convención | Ejemplo |
|---|---|---|
| Variables | camelCase | `nombreUsuario` |
| Funciones | camelCase | `obtenerUsuario()` |
| Clases | PascalCase | `UsuarioService` |
| Interfaces | PascalCase | `UsuarioRepository` |
| Tipos | PascalCase | `EstadoPedido` |
| Constantes | UPPER_SNAKE_CASE | `MAX_REINTENTOS` |
| Archivos | kebab-case | `usuario-service.ts` |
| Componentes React | PascalCase | `UsuarioCard.tsx` |

---

## Funciones

- Preferir funciones flecha para callbacks.
- Tipar siempre los parámetros y el retorno.
- Funciones de máximo 20 líneas. Si es más larga, dividir.

```typescript
// ✅ Correcto
const calcularTotal = (precio: number, cantidad: number): number => {
  return precio * cantidad;
};

// ❌ Incorrecto
function calcularTotal(precio, cantidad) {
  return precio * cantidad;
}
```

---

## Manejo de Errores

- Nunca ignorar errores silenciosamente.
- Crear clases de error personalizadas para errores de dominio.
- Usar Result pattern o lanzar errores tipados.

```typescript
class UsuarioNoEncontradoError extends Error {
  constructor(id: string) {
    super(`Usuario con id ${id} no encontrado`);
    this.name = 'UsuarioNoEncontradoError';
  }
}
```

---

## Imports

- Usar imports absolutos configurados en `tsconfig.json`.
- Agrupar imports: externos → internos → tipos.
- Sin imports no utilizados.

```typescript
// ✅ Correcto
import { NextResponse } from 'next/server';

import { UsuarioService } from '@/services/usuario-service';
import { validarEmail } from '@/utils/validaciones';

import type { Usuario } from '@/types/usuario';
```

---

## Variables de Entorno

- Validar variables de entorno al inicio de la aplicación.
- Nunca acceder a `process.env` directamente en componentes o servicios.
- Centralizar en un módulo `env.ts`.

```typescript
// env.ts
const env = {
  databaseUrl: process.env.DATABASE_URL!,
  apiKey: process.env.API_KEY!,
} as const;

export default env;
```

---

<sub>Synthera Labs · Standards v1.0.0</sub>
