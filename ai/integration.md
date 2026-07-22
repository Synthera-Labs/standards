# Estándares de Integración de IA — Synthera Labs

## Principio General

La inteligencia artificial en un proyecto de Synthera Labs es una herramienta al servicio del producto, no el producto en sí. Toda integración de IA debe estar justificada, documentada, evaluada y controlable.

---

## Modelos Aprobados

| Proveedor | Modelo | Uso recomendado |
|---|---|---|
| Anthropic | Claude Sonnet / Opus | Razonamiento complejo, generación de contenido, análisis |
| OpenAI | GPT-4o | Casos donde se requiera ecosistema OpenAI |
| Google | Gemini Pro | Multimodal, integración con Google Workspace |

**Regla:** No integrar modelos no evaluados sin aprobación del CTO.

---

## Gestión de API Keys

- Las API keys de modelos de IA se tratan como secretos críticos.
- Nunca se hardcodean. Siempre en variables de entorno.
- Rotar API keys si existe sospecha de exposición.
- Monitorear uso y costos mensualmente.

```env
ANTHROPIC_API_KEY=tu_key_aqui
OPENAI_API_KEY=tu_key_aqui
```

---

## Diseño de Prompts

- Los prompts de sistema se versionan igual que el código.
- Separar el prompt del código que lo llama.
- Documentar el propósito de cada prompt.
- Incluir instrucciones de formato de salida cuando aplique.

```python
# ✅ Correcto: prompt separado y documentado
SYSTEM_PROMPT = """
Eres un asistente especializado en análisis de contratos legales.
Tu tarea es identificar cláusulas de riesgo y resumirlas en términos claros.
Responde siempre en español.
Formato de respuesta: JSON con campos 'riesgos' y 'resumen'.
"""

# ❌ Incorrecto: prompt embebido en la lógica
response = cliente.messages.create(
    model="claude-opus-4-6",
    messages=[{"role": "user", "content": f"analiza este contrato: {contrato}"}]
)
```

---

## Manejo de Errores

Las APIs de IA pueden fallar. Siempre implementar:

- Timeout definido en cada llamada.
- Reintentos con backoff exponencial (máximo 3 intentos).
- Fallback documentado si el modelo no responde.
- Registro del error con suficiente contexto para debugging.

```python
import anthropic
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(
    stop=stop_after_attempt(3),
    wait=wait_exponential(multiplier=1, min=2, max=10)
)
def llamar_modelo(prompt: str) -> str:
    cliente = anthropic.Anthropic()
    respuesta = cliente.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=1024,
        messages=[{"role": "user", "content": prompt}]
    )
    return respuesta.content[0].text
```

---

## Control de Costos

- Estimar el costo mensual antes de integrar un modelo en producción.
- Configurar alertas de gasto en el proveedor.
- Implementar caché de respuestas cuando la misma consulta se repite.
- Documentar el costo estimado por operación en el README del proyecto.

---

## Evaluación de Outputs

Antes de poner en producción cualquier feature de IA:

- [ ] Definir métricas de calidad del output.
- [ ] Probar con al menos 20 casos representativos.
- [ ] Documentar los casos donde el modelo falla.
- [ ] Implementar mecanismo de feedback del usuario si aplica.
- [ ] Definir umbrales de confianza si el modelo produce scores.

---

## Consideraciones Éticas

- No usar IA para tomar decisiones que afecten a personas sin revisión humana.
- No procesar datos personales sensibles sin consentimiento explícito.
- Informar al usuario cuando está interactuando con un sistema de IA.
- No usar outputs de IA como fuente de verdad sin validación.

---

## Documentación Obligatoria

Todo proyecto con IA debe documentar en su README:

- Qué modelos usa y para qué.
- Costo estimado mensual.
- Cómo se gestionan los prompts.
- Limitaciones conocidas del sistema.
- Qué pasa si el modelo no está disponible.

---

<sub>Synthera Labs · Standards v1.0.0</sub>
