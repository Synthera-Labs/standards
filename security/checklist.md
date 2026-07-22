# Checklist de Seguridad — Synthera Labs

Basado en OWASP Top 10. Este checklist debe completarse antes de cualquier deploy a producción.

---

## Checklist General (Todo Proyecto)

### Configuración y Secretos
- [ ] No hay credenciales, API keys ni contraseñas en el código fuente.
- [ ] El archivo `.env` está en `.gitignore`.
- [ ] Existe `.env.example` con todas las variables necesarias sin valores reales.
- [ ] Las variables de entorno están documentadas.

### Autenticación
- [ ] Las contraseñas se almacenan con hash (bcrypt o argon2). Nunca en texto plano.
- [ ] Los tokens JWT tienen tiempo de expiración definido.
- [ ] Existe mecanismo de revocación de tokens.
- [ ] Los endpoints sensibles requieren autenticación.

### Autorización
- [ ] Cada endpoint verifica que el usuario tiene permiso para el recurso.
- [ ] No se expone información de otros usuarios en respuestas.
- [ ] Los roles y permisos están claramente definidos y aplicados.

### Validación de Entradas
- [ ] Todo input del usuario es validado antes de procesarse.
- [ ] Los parámetros de base de datos usan queries preparadas.
- [ ] No se construyen queries SQL con concatenación de strings.
- [ ] Los archivos subidos son validados en tipo y tamaño.

### Exposición de Datos
- [ ] Las respuestas de la API no exponen campos innecesarios.
- [ ] Los errores no revelan información sensible del sistema (stack traces, versiones).
- [ ] Los logs no contienen contraseñas ni tokens.

### Headers HTTP
- [ ] `Content-Security-Policy` configurado.
- [ ] `X-Content-Type-Options: nosniff` configurado.
- [ ] `X-Frame-Options: DENY` configurado.
- [ ] HTTPS obligatorio en producción.
- [ ] CORS configurado explícitamente con dominios permitidos.

### Dependencias
- [ ] Las dependencias están actualizadas.
- [ ] Se ejecutó auditoría de vulnerabilidades (`npm audit` / `pip audit`).
- [ ] No se usan versiones con vulnerabilidades conocidas.

---

## Checklist por Tipo de Proyecto

### APIs REST
- [ ] Rate limiting configurado en endpoints públicos.
- [ ] Paginación implementada en endpoints que retornan listas.
- [ ] Los IDs expuestos no son secuenciales (usar UUID).
- [ ] Autenticación con JWT o API Key según el caso.

### Aplicaciones Web
- [ ] Protección contra CSRF en formularios con mutaciones.
- [ ] Sanitización de HTML si se renderiza contenido del usuario.
- [ ] Sin datos sensibles almacenados en `localStorage`.

### Bases de Datos
- [ ] El usuario de base de datos tiene solo los permisos necesarios.
- [ ] Backups automáticos configurados en producción.
- [ ] Datos sensibles cifrados en reposo si aplica.

---

## Firma de Revisión

| Campo | Valor |
|---|---|
| Proyecto | |
| Revisado por | |
| Fecha | |
| Versión | |
| Aprobado | ☐ Sí / ☐ No |

---

<sub>Synthera Labs · Standards v1.0.0 · Basado en OWASP Top 10</sub>
