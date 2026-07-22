# Estándares de Python — Synthera Labs

## Versión

Python 3.11 o superior. Obligatorio en todos los proyectos.

---

## Formato y Estilo

- Seguir [PEP 8](https://pep8.org/) sin excepción.
- Longitud máxima de línea: **88 caracteres** (estándar Black).
- Usar **Black** como formateador automático.
- Usar **Ruff** como linter.
- Usar **mypy** para verificación de tipos estáticos.

---

## Tipado

Todo el código debe usar type hints. No se acepta código sin tipos en funciones públicas.

```python
# ✅ Correcto
def calcular_total(precio: float, cantidad: int) -> float:
    return precio * cantidad

# ❌ Incorrecto
def calcular_total(precio, cantidad):
    return precio * cantidad
```

---

## Nombres

| Elemento | Convención | Ejemplo |
|---|---|---|
| Variables | snake_case | `nombre_usuario` |
| Funciones | snake_case | `obtener_usuario()` |
| Clases | PascalCase | `ClienteService` |
| Constantes | UPPER_SNAKE_CASE | `MAX_REINTENTOS` |
| Módulos | snake_case | `user_service.py` |

---

## Estructura de Proyecto

```
proyecto/
├── src/
│   └── nombre_modulo/
│       ├── __init__.py
│       ├── main.py
│       ├── models/
│       ├── services/
│       ├── repositories/
│       └── utils/
├── tests/
├── .env.example
├── pyproject.toml
├── README.md
└── Dockerfile
```

---

## Dependencias

- Usar `pyproject.toml` con `poetry` o `pip` + `requirements.txt`.
- Separar dependencias de desarrollo de las de producción.
- Fijar versiones exactas en producción.

```toml
[tool.poetry.dependencies]
python = "^3.11"
fastapi = "^0.110.0"

[tool.poetry.group.dev.dependencies]
black = "^24.0.0"
ruff = "^0.3.0"
mypy = "^1.9.0"
pytest = "^8.0.0"
```

---

## Manejo de Errores

- Nunca usar `except Exception` sin registrar el error.
- Crear excepciones personalizadas para errores de dominio.
- No silenciar errores con `pass`.

```python
# ✅ Correcto
try:
    resultado = servicio.procesar(datos)
except ValueError as e:
    logger.error("Error de validación: %s", e)
    raise

# ❌ Incorrecto
try:
    resultado = servicio.procesar(datos)
except:
    pass
```

---

## Variables de Entorno

- Nunca hardcodear credenciales, URLs de base de datos ni API keys.
- Usar `pydantic-settings` para gestionar configuración.
- Siempre incluir `.env.example` en el repositorio.

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    api_key: str
    debug: bool = False

    class Config:
        env_file = ".env"

settings = Settings()
```

---

## Tests

- Cobertura mínima: **80%** en código de producción.
- Usar `pytest` como framework de testing.
- Nombrar tests de forma descriptiva.

```python
# ✅ Correcto
def test_calcular_total_retorna_producto_precio_por_cantidad():
    assert calcular_total(10.0, 3) == 30.0

# ❌ Incorrecto
def test_1():
    assert calcular_total(10.0, 3) == 30.0
```

---

## Comentarios

- Comentar el **por qué**, no el **qué**.
- Usar docstrings en funciones públicas y clases.

```python
def aplicar_descuento(precio: float, porcentaje: float) -> float:
    """
    Aplica un descuento porcentual al precio dado.

    Args:
        precio: Precio original en MXN.
        porcentaje: Porcentaje de descuento entre 0 y 100.

    Returns:
        Precio con descuento aplicado.
    """
    return precio * (1 - porcentaje / 100)
```

---

<sub>Synthera Labs · Standards v1.0.0</sub>
