# Aprender a colaborar y gestionar versiones en productos digitales

> **Nota:** Git y GitHub no son lo mismo. **Git** es la herramienta que registra el historial del proyecto en la computadora. **GitHub** es un servicio en la nube que aloja repositorios Git y agrega herramientas para colaborar. Esa diferencia ordena las dos prácticas del módulo.


## Git para Desarrolladores — Fundamentos del Control de Versiones

Observe estos dos escenarios con el mismo proyecto.

**Antes (sin control de versiones):**

```
proyecto/
├── index.html
├── index_copia.html
├── index_final.html
├── index_final_v2.html
└── index_final_AHORA_SI.html
```

Preguntas sin respuesta clara: ¿cuál es la versión buena? ¿Qué cambió entre `v2` y `AHORA_SI`? ¿Quién lo cambió y por qué? Si dos personas editan a la vez, ¿quién pisa a quién?

**Después (con Git):**

```
proyecto/
├── index.html          ← un solo archivo
└── .git/               ← el historial completo vive acá
```

```
$ git log --oneline
a1b2c3d Agrega formulario de contacto
9f8e7d6 Corrige el color del botón
4c5d6e7 Primera versión de la página
```

Un solo archivo, y el historial responde las tres preguntas: **qué** cambió, **quién** lo cambió y **cuándo**, además del **porqué** en el mensaje de cada *commit*.

## Vocabulario mínimo

| Término | Qué es | Analogía |
|---|---|---|
| **Repositorio** | Carpeta de proyecto con su historial completo | Un álbum con todas las fotos del proyecto |
| **Commit** | Una «foto» guardada del proyecto con un mensaje que explica el cambio | Una foto con título |
| **Rama (branch)** | Línea de trabajo paralela | Una copia de trabajo para experimentar sin romper lo principal |
| **Merge** | Unir el trabajo de una rama con otra | Juntar dos borradores en uno |
| **Remoto** | Copia del repositorio alojada en otro lugar (GitHub) | La copia compartida en la nube |

## Radiografía: las cuatro zonas de Git

Este es el mapa más importante del módulo. Los cambios viajan por cuatro zonas, y cada comando mueve cambios de una zona a la siguiente.

```
  ① CARPETA DE      ② STAGING        ③ REPOSITORIO     ④ REMOTO
     TRABAJO           (área de          LOCAL             (GitHub)
  (archivos)          preparación)    (.git, historial)

  se edita ──────► git add ──────► git commit ──────► git push
        ◄─────────────────────────────── git pull / git fetch
```

| Zona | Qué contiene | Cómo se ve |
|---|---|---|
| ① Carpeta de trabajo | Los archivos que se editan ahora | Lo que se ve en el editor |
| ② Staging | Los cambios que se eligieron incluir en el próximo commit | Una caja que se está armando |
| ③ Repositorio local | Los commits ya guardados | El historial en la máquina |
| ④ Remoto | Los commits publicados | Lo que ve el resto del equipo |

**Por qué existe el staging:** permite elegir *qué* entra en cada commit. Si se tocaron tres archivos pero solo dos forman parte de la misma idea, se agregan esos dos y se deja el tercero para otro commit. Así el historial queda ordenado.

## Configuración inicial (una sola vez)

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global init.defaultBranch main
```

Estos datos quedan escritos en cada commit que se realice. Por eso conviene usar el mismo correo que se va a usar en GitHub.

## El ciclo básico, comando por comando

Cada comando tiene un efecto que puede **verse**. Pruebe este flujo en una carpeta nueva.

**Paso 1: Crear el repositorio**

```bash
mkdir mi-proyecto
cd mi-proyecto
git init
```

Efecto visible: aparece una carpeta oculta `.git`. Desde ahora, esa carpeta es un repositorio.

**Paso 2: Crear un archivo y mirar el estado**

```bash
echo "<h1>Hola mundo</h1>" > index.html
git status
```

Efecto visible: Git indica que `index.html` está *sin seguimiento* (untracked). Existe en la zona ①, pero Git todavía no lo registra.

**Paso 3: Pasarlo a staging**

```bash
git add index.html
git status
```

Efecto visible: ahora figura como *listo para confirmar*. Se pasó de ① a ②.

**Paso 4: Guardar la foto**

```bash
git commit -m "Primera versión de la página"
git log --oneline
```

Efecto visible: el commit aparece en el historial. Se pasó de ② a ③.

**Paso 5: Hacer un cambio y ver la diferencia**

```bash
echo "<p>Bienvenida</p>" >> index.html
git diff
```

Efecto visible: Git muestra la línea agregada con un `+`. Esa es la diferencia entre la carpeta de trabajo y el último commit.

### Estados comparados de un archivo

| Estado | Salida de `git status` | Zona | Siguiente comando |
|---|---|---|---|
| Nuevo, sin registrar | *Untracked files* | ① | `git add` |
| Modificado | *Changes not staged for commit* | ① | `git add` |
| Preparado | *Changes to be committed* | ② | `git commit` |
| Guardado | *nothing to commit, working tree clean* | ③ | Seguir trabajando |

> **Hábito clave:** ejecutar `git status` antes y después de cada comando. Es la forma de saber en qué zona está cada cambio.

## Escribir buenos mensajes de commit

| ❌ Antes | ✅ Después |
|---|---|
| `cambios` | `Agrega validación al campo de correo` |
| `arreglo` | `Corrige el desborde del menú en pantallas chicas` |
| `asdf` | `Actualiza el título de la página de inicio` |

Una regla útil: el mensaje debería completar la frase «Este commit… ». Un buen mensaje ahorra a la persona abrir cada archivo para entender qué pasó.

## Ramas: experimentar sin miedo

Una rama es una línea de trabajo paralela. El proyecto principal queda intacto mientras se prueba una idea.

```
main      ●───●───●─────────────●   ← versión estable
                   \           /
funcion-menu        ●───●───●       ← experimento
```

```bash
git switch -c funcion-menu     # crea la rama y te pasa a ella
git switch main                # vuelve a la principal
git merge funcion-menu         # une el trabajo
```

Efecto visible: al cambiar de rama, **los archivos de la carpeta cambian** para reflejar esa línea de trabajo. Pruebe: cree un archivo en la rama, vuelva a `main` y verá que desapareció; reaparecerá al volver a la rama.

## Conflictos: cuando dos cambios chocan

Un conflicto ocurre cuando dos ramas modificaron **la misma línea**. Git no sabe cuál elegir y lo solicita.

**Antes de resolver** (el archivo con marcas de Git):

```
<<<<<<< HEAD
<h1>Bienvenidos</h1>
=======
<h1>Hola a todos</h1>
>>>>>>> funcion-menu
```

| Marca | Significa |
|---|---|
| `<<<<<<< HEAD` | Empieza la versión actual (la rama donde se está) |
| `=======` | Separador |
| `>>>>>>> funcion-menu` | Termina la versión de la otra rama |

**Después de resolver:** se elige o se combina el contenido y se eliminan las tres marcas.

```
<h1>Bienvenidos a todos</h1>
```

```bash
git add index.html
git commit -m "Resuelve conflicto en el título"
```

Un conflicto no es un error: es Git pidiendo una decisión humana.

## Deshacer sin entrar en pánico

| Situación | Comando | Qué hace |
|---|---|---|
| Quiero ver qué cambié | `git diff` | Muestra diferencias sin modificar nada |
| Saqué algo de staging por error | `git restore --staged archivo` | Vuelve el archivo de ② a ① |
| Quiero descartar mis cambios de un archivo | `git restore archivo` | **Borra** los cambios no guardados de ese archivo |
| Quiero revertir un commit ya hecho | `git revert <id>` | Crea un commit nuevo que deshace al anterior |

⚠️ `git restore archivo` descarta cambios sin guardarlos en ningún lado. Antes de usarlo, revise `git diff`.


## GitHub para la colaboración en equipo

Hasta ahora, todo vive en la computadora. Para colaborar, se necesita un lugar compartido. GitHub es ese lugar: aloja la copia remota (zona ④) y agrega herramientas para discutir y revisar cambios.

| Qué hace | Git | GitHub |
|---|---|---|
| Registra el historial | ✅ | ✅ (lo aloja) |
| Funciona sin internet | ✅ | ❌ |
| Permite revisar cambios de otras personas | ❌ | ✅ (pull requests) |
| Gestiona tareas y errores | ❌ | ✅ (issues) |
| Controla quién puede escribir | ❌ | ✅ (permisos) |

## Conectar el repositorio local con GitHub

1. En GitHub, cree un repositorio vacío (sin README) llamado `mi-portafolio`.
2. En la terminal:

```bash
git remote add origin https://github.com/TU-USUARIO/mi-portafolio.git
git push -u origin main
```

Efecto visible: al refrescar la página del repositorio en GitHub, aparecen los archivos y el historial de commits.

| Comando | Qué hace | Zonas |
|---|---|---|
| `git remote add origin <url>` | Registra la dirección del remoto | Configura ④ |
| `git push` | Envía los commits al remoto | ③ → ④ |
| `git fetch` | Trae novedades del remoto sin mezclarlas | ④ → ③ (sin tocar ①) |
| `git pull` | Trae novedades y las integra en el trabajo | ④ → ③ → ① |
| `git clone <url>` | Descarga un repositorio completo | ④ → la máquina |

> **Diferencia importante:** `fetch` es «mirar qué hay de nuevo» y `pull` es «traerlo y mezclarlo». Si hay dudas, se recomienda empezar con `fetch`.


## El flujo de colaboración: rama → pull request → revisión → merge

Este flujo (conocido como *GitHub flow*) es el corazón de la colaboración moderna.

```
 ① Crear rama     ② Hacer commits    ③ Subir la rama
 git switch -c    git add / commit   git push -u origin
 corregir-menu                       corregir-menu
                                          │
                                          ▼
 ⑥ Merge ◄── ⑤ Aprobación ◄── ④ Abrir Pull Request
                 del equipo         (Compare & pull request)
```

| Paso | Dónde ocurre | Qué pasa |
|---|---|---|
| ① Crear rama | Terminal | Se aísla el trabajo de `main` |
| ② Commits | Terminal | Se registran cambios pequeños y claros |
| ③ Subir rama | Terminal | La rama aparece en GitHub |
| ④ Abrir PR | GitHub | Se proponen cambios y se explica por qué |
| ⑤ Revisión | GitHub | El equipo comenta, pide cambios o aprueba |
| ⑥ Merge | GitHub | Los cambios entran a `main` |

### Anatomía de un buen Pull Request

| Parte | ❌ Antes | ✅ Después |
|---|---|---|
| **Título** | «Cambios» | «Corrige el menú desplegable en móviles» |
| **Descripción** | (vacía) | «Qué cambié, por qué, y cómo probarlo» |
| **Tamaño** | 40 archivos tocados | Un solo objetivo, pocos archivos |
| **Referencia** | Ninguna | «Cierra #12» (vincula al issue) |

Un PR pequeño y bien explicado se revisa en minutos. Uno enorme y sin contexto se posterga o se aprueba sin mirar.


## Revisión de código: cómo dar y recibir feedback

**Al revisar:**

- Describa el efecto, no a la persona: «Esta función repite lógica» en lugar de «Escribiste esto mal».
- Distinga lo obligatorio de lo opcional («Sugerencia: …»).
- Reconozca lo que está bien hecho.

**Al recibir una revisión:**

- Un comentario es sobre el código, no sobre la persona.
- Si algo no se entiende, pregunte.
- Responda cada comentario o haga el cambio pedido y suba un nuevo commit: el PR se actualiza solo.


## Buenas prácticas de equipo

| Práctica | Por qué importa |
|---|---|
| No trabajar directamente en `main` | Mantiene estable la versión principal |
| Hacer `git pull` antes de empezar | Evita partir de una versión vieja |
| Commits pequeños y frecuentes | Facilitan entender y revertir |
| Archivo `.gitignore` | Evita subir `node_modules`, claves y archivos temporales |
| Nunca subir secretos (claves de API, contraseñas) | Una vez publicados, hay que considerarlos comprometidos |
| README claro | Es la puerta de entrada al proyecto |

Ejemplo de `.gitignore` básico:

```
node_modules/
.env
.DS_Store
```

## Estados comparados: ¿qué te dice Git cuando algo no va bien?

| Mensaje | Qué significa | Qué hacer |
|---|---|---|
| `rejected ... fetch first` | El remoto tiene commits que no se tienen localmente | `git pull` y luego `git push` |
| `CONFLICT (content)` | Dos cambios chocan en la misma línea | Resolver marcas, `git add`, `git commit` |
| `Authentication failed` | Falló la credencial | Revisar token, `gh auth login` o clave SSH |
| `fatal: not a git repository` | Se está fuera de un repositorio | Entrar a la carpeta correcta o hacer `git init` |
