# Prompt — Generador de Documentación Técnica con Mermaid JS

Usa este prompt en GitHub Copilot Chat, ChatGPT, Claude, etc.  
Sustituye `[NOMBRE DEL PROYECTO]` y pega el código fuente al final.

---

## Prompt base

```
Analiza el siguiente código fuente del proyecto "[NOMBRE DEL PROYECTO]" y genera un archivo
DIAGRAMAS.md completo en español con los siguientes diagramas en formato Mermaid JS:

1. **Diagrama de Clases** (classDiagram)
   - Lista todas las clases con sus atributos y métodos públicos (+) y privados (-).
   - Indica las relaciones entre clases (usa, hereda, compone, retorna).
   - Incluye las clases de librerías externas relevantes con las que interactúa el proyecto.

2. **Diagrama de Flujo de Ejecución** (flowchart TD)
   - Representa el flujo completo desde el punto de entrada (__main__ o equivalente).
   - Incluye todos los nodos de decisión (validaciones, comprobaciones de archivos, etc.).
   - Muestra los errores/excepciones posibles como nodos de salida con forma de trapecio (/texto/).
   - El flujo debe ser vertical (TD = top-down).

3. **Diagrama de Secuencia** (sequenceDiagram)
   - Representa la interacción ordenada en el tiempo entre: el usuario, los módulos propios,
     las librerías externas y el sistema de archivos.
   - Usa actor para el usuario final y participant para el resto.
   - Las llamadas síncronas con ->> y las respuestas con -->>.

4. **Mapa de Dependencias entre Módulos** (graph LR)
   - Agrupa con subgraph los módulos propios, librerías externas, stdlib de Python y archivos/recursos.
   - Usa etiquetas descriptivas en las flechas (importa, carga, retorna, etc.).

5. **Diagrama de Relaciones entre Funciones** (graph TD o flowchart TD)
   - Cada nodo/forma debe representar **exactamente una función o método** del proyecto.
   - Las flechas deben representar llamadas directas entre funciones (`A --> B` = A llama a B).
   - Distingue visualmente funciones de entrada, funciones de dominio y funciones externas usando subgraph.
   - Incluye funciones/métodos externos solo si son invocados directamente por el código del proyecto.
   - Si una función no llama a ninguna otra, debe aparecer igualmente como nodo hoja.
   - Debe aparecer siempre como **sección 5** del documento y antes de la tabla final.
   - Para máxima compatibilidad, usa nodos rectangulares `[..]` para todos los tipos y diferencia por `subgraph`.

Requisitos de formato del archivo generado:
- Encabezado con nombre del proyecto y descripción breve.
- Cada diagrama en su propia sección numerada con título (##).
- Bloques de código Mermaid con triple backtick y la palabra "mermaid".
- Incluir al final una tabla resumen de responsabilidades de cada módulo/clase.
- El archivo debe poder renderizarse directamente como HTML con MermaidJS sin modificaciones.
- En el diagrama de funciones, usar etiquetas de nodo con formato simple `modulo.funcion` o `Clase.metodo`.
- En el diagrama de funciones, incluir al menos estos subgraph: `Entrada`, `Dominio`, `Externas`.

Compatibilidad Mermaid (obligatorio, versión 11+):
- Evita caracteres conflictivos en nodos de `flowchart`: no usar `__main__`, comillas, llaves `{}` ni paréntesis `()` en etiquetas de nodos.
- En `flowchart`, evita tildes y signos invertidos (`¿`, `¡`) dentro de etiquetas de nodo; usa texto ASCII simple.
- Evita etiquetas complejas en aristas (por ejemplo `-->|texto|`); usa `-->` simple cuando sea posible.
- No uses formas avanzadas (`{{..}}`, `[/..../]`) salvo que el render esté validado; prioriza `[..]`, `{..}` y `([..])`.
- No repitas el mismo ID de nodo con distinta etiqueta.
- Si hay riesgo de parseo, preferir `graph TD` en lugar de `flowchart TD` para el diagrama de funciones.
```

---

## Variables a rellenar

| Variable | Descripción | Ejemplo |
|---|---|---|
| `[NOMBRE DEL PROYECTO]` | Nombre descriptivo del proyecto | `Samjoko Lector Básico` |
| *(código fuente)* | Pega todos los archivos `.py` relevantes al final del prompt | `samjoko.py`, `samjoko_lector.py` |

---

## Prompt extendido (con control de calidad)

Si quieres más control sobre el resultado, añade estas instrucciones adicionales al prompt base:

```
Reglas adicionales:
- Si una función solo tiene 2-3 líneas y es trivial, no le dediques nodo propio en el flujo;
  agrúpala con su llamador.
- No incluyas detalles de implementación interna de librerías de terceros salvo los métodos
  que el código del proyecto llama directamente.
- Los nombres de nodos en los diagramas deben estar en español cuando sean descripciones,
  y en código (sin traducir) cuando sean nombres de funciones/clases/métodos.
- Usa IDs de nodo cortos y sin espacios en los diagramas (A, B, LP, PV, etc.).
- El diagrama de secuencia no debe superar 20 líneas de interacciones.
- En el diagrama de funciones, evitar nodos duplicados: una función debe mapear a un único nodo.
- Si hay recursión o ciclos, representarlos explícitamente con flechas de retorno.
- Priorizar el diagrama de funciones reales sobre helpers internos de librerías externas.
- Si hay demasiadas funciones, dividir por módulo con subgraph adicionales sin perder las llamadas cruzadas.
- Antes de finalizar, validar que cada bloque Mermaid compile sin errores de sintaxis.
```

---

## Notas de uso

- **Modelos recomendados:** GPT-4o, Claude 3.5 Sonnet o superior, GitHub Copilot (modo agente).
- **Contexto mínimo necesario:** todos los archivos `.py` del proyecto + el `README.md` si existe.
- **Resultado esperado:** un único archivo `DOCUMENTACION.md` listo para abrirse con el viewer HTML.
- Para proyectos grandes (+10 archivos), genera la documentación módulo a módulo y luego
  pide un diagrama de dependencias global que los una todos.
