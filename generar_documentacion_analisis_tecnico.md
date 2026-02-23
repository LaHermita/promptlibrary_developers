Ponte en modo desarrollador full-stack y analiza técnicamente mi aplicación. El objetivo es generar documentación técnica completa en formato Markdown. Analiza todos los archivos incluidos en el contexto del proyecto.

Sigue esta estructura:

---

## 1. Diagrama de arquitectura y componentes

- Introduce brevemente la arquitectura utilizada (por ejemplo, Hexagonal/Clean Architecture) y explica sus componentes principales.
- Incluye un pequeño diagrama de componentes en Markdown que muestre cómo se interrelacionan las capas o módulos del sistema (por ejemplo: Controller, Service, Repository, Entity, DTO, etc.).

---

## 2. Diagrama de estructura y relación de clases

- Haz un listado completo de todas las clases implementadas en el proyecto.
- Enfócate especialmente en las entidades del dominio: nómbralas, explica brevemente su propósito y relaciones si existen.
- Si es posible, incluye un diagrama básico en Markdown o pseudocódigo que represente las relaciones entre clases.

---

## 3. Procedimiento para desarrollar componentes

- Redacta un párrafo resumen sobre cómo está estructurada la API REST: cómo se implementan las entidades, qué patrones de diseño se siguen, y qué flujo general siguen las peticiones.
- Enumera todos los endpoints REST definidos en el proyecto, usando el siguiente formato:

### {CLASE O CONTROLADOR}:
- **Método:** `{nombreDelMetodo}`
- **Endpoint:** `{verboHTTP} /{ruta}`
- **Descripción:** `{Breve descripción del propósito del método}`
- **Parámetros:**
  - `{parametro}`: `{explicación de su función}`
  - Ejemplo:
    - `request`: Objeto `RequestPostPutAppointmentDto` que contiene la información de la cita a crear.
- **Excepciones controladas:**
  - `{NombreExcepción}`: `{Motivo o condición bajo la cual se lanza}`
  - Ejemplo:
    - `BusinessException`: Si ocurre un error durante la creación de la cita.
- **Respuestas:**
  - `{Descripción general de la respuesta esperada (código HTTP, contenido JSON, DTO usado, etc.)}`

---

## 4. Estructura del Frontend

- Analiza la estructura del frontend y genera un árbol de carpetas en Markdown.
- Si se utiliza un framework (Angular, React, Vue...), explícalo brevemente.
- Describe brevemente los principales módulos, páginas o componentes frontend detectados.

---

Rellena los valores entre llaves `{}` en función de lo que encuentres en el análisis del código.

