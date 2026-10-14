# Milestone — AI-driven Engineering

### Objetivos de aprendizaje

- **Gestión de Contexto Persistente (Memory Bank):** Aprender a estructurar información técnica y de negocio en Markdown para que los asistentes de IA mantengan el hilo conductor entre sesiones de desarrollo.
- **Gobernanza de Agentes (`AGENTS.md` y `.agents/`):** Definir reglas, restricciones y protocolos obligatorios que el agente debe seguir de forma automatizada antes de modificar código o hacer commits.
- **Automatización Reutilizable (Skills):** Diseñar una skill de agente acotada, con entradas claras y criterios de aceptación verificables para tareas recurrentes.
- **Estructuración Limpia de Monorepos:** Comprender la separación de responsabilidades entre la infraestructura de configuración de agentes (`.agents/`) y el código de producto futuro (`/uis`, `/services`).

## Instrucciones

### Paso 1: Configurar el Contexto de Negocio (`CONTEXT.md`)

1. Ve al repositorio oficial del curso en GitHub (`ai-engineering-syllabus`) y localiza la carpeta de contextos: `content/contexts/00-general-contexts`.
2. Elige el archivo de briefing correspondiente a tu empresa asignada (por ejemplo: `CONTEXT-<tu-empresa>-briefing.es.md`).
3. Cópialo por completo y pégalo en un nuevo archivo llamado exactamente `CONTEXT.md` ubicado en la raíz de tu monorepo. Si la plantilla tenía un archivo vacío con ese nombre, reemplázalo.

### Paso 2: Crear el Banco de Memoria (`memory-bank/`)

Crea una carpeta llamada `memory-bank/` en la raíz y construye los tres documentos iniciales basados estrictamente en tu `CONTEXT.md`:

- `projectbrief.md`: Misión, visión, modelo de negocio y problemas que resuelve la empresa.
- `techContext.md`: Stack tecnológico base, decisiones arquitectónicas y restricciones.
- `progress.md`: Registro del estado actual del desarrollo y siguientes pasos.

### Paso 3: Definir el Protocolo de Operación (`AGENTS.md`)

Crea el archivo `AGENTS.md` en la raíz. Este archivo dictará las reglas obligatorias que cualquier agente de programación debe seguir de forma automática al abrir el repositorio.

### Paso 4: Configurar Reglas y Skills (`.agents/`)

1. Crea la carpeta `.agents/rules/` y añade al menos una regla específica (por ejemplo, normas de formato o manejo de variables de entorno).
2. Crea la carpeta `.agents/skills/<nombre-de-skill>/` conteniendo un archivo `SKILL.md` con un objetivo único, inputs claros y criterios de verificación medibles.

## Ejemplo Práctico: Archivo `AGENTS.md`

Este archivo se coloca en la raíz del monorepo (`./AGENTS.md`):

```markdown
# Directrices de Operación para Agentes de IA

Eres un desarrollador senior de IA colaborando en este monorepo. Antes de escribir código o realizar cualquier modificación, debes cumplir estrictamente con los siguientes protocolos.

## 1. Lectura Obligatoria de Contexto al Iniciar Sesión

Al comienzo de cualquier tarea, lee obligatoriamente los siguientes archivos para entender el negocio y la arquitectura:

- `CONTEXT.md` (Briefing de la empresa)
- `memory-bank/projectbrief.md`
- `memory-bank/techContext.md`
- `memory-bank/progress.md`

## 2. Flujo Obligatorio Antes de Cada Commit

Ningún cambio puede ser enviado o commiteado sin seguir estrictamente este flujo de 4 pasos:

1. **Validación Local:** Ejecuta los tests o linters disponibles para asegurar que no hay errores de sintaxis o compilación.
2. **Verificación de Fronteras:** Comprueba que no has modificado archivos en carpetas prohibidas (ver sección 3).
3. **Actualización de Progreso:** Modifica `memory-bank/progress.md` para reflejar qué feature o tarea acabas de completar.
4. **Mensaje Descriptivo:** Redacta un commit claro que vincule el cambio realizado con los objetivos del proyecto.

## 3. Zonas Protegidas (No Modificar sin Autorización Explícita)

El agente **tiene prohibido** modificar los siguientes elementos sin una orden directa y justificada del desarrollador humano:

- La estructura raíz de directorios del monorepo (`/uis/website`, `/uis/backoffice`, `/services`).
- Las dependencias principales declaradas en los archivos de configuración globales de paquetes (`package.json`, `requirements.txt`, etc.).
- Las reglas de gobernanza ubicadas en `.agents/` y este mismo archivo (`AGENTS.md`).
```

## Ejemplos de Prompts para Construir el `memory-bank/` con IA

Puedes abrir tu IDE (como Cursor o Windsurf) con el repositorio clonado y usar estos prompts con tu asistente para generar rápidamente la documentación inicial a partir de tu `CONTEXT.md`:


### Prompt para generar `memory-bank/projectbrief.md`

> "Actúa como arquitecto de software y analista de negocio. Lee el archivo `CONTEXT.md` en la raíz de este repositorio. A partir de él, genera un archivo exhaustivo en `memory-bank/projectbrief.md` que resuma: (1) El modelo de negocio y propuesta de valor de la empresa, (2) El problema principal que resuelve el proyecto, y (3) Las metas principales y entregables esperados para este curso."

### Prompt para generar `memory-bank/techContext.md`

> "Basándote en el archivo `CONTEXT.md` y en la estructura de carpetas actual de este monorepo (`/uis/website`, `/uis/backoffice`, `/services`), genera el archivo `memory-bank/techContext.md` detallando: (1) el stack tecnológico recomendado para el frontend y backend según el perfil de la empresa, (2) decisiones arquitectónicas clave (por ejemplo, separación de monorepo), y (3) restricciones técnicas o de entorno que debemos respetar."

### Prompt para generar `memory-bank/progress.md`

> "Crea el archivo `memory-bank/progress.md` que actúe como tablero de control del estado actual del proyecto. Debe incluir: (1) qué está completado (la infraestructura base de agentes de este hito), (2) qué está en curso, y (3) los próximos pasos lógicos para cuando comencemos a desarrollar las interfaces y servicios de la empresa."
