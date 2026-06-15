# Indicaciones para el desarrollo en Frontend

Para desarrollar cualquier proyecto en frontend, es necesario considerar una serie de indicaciones que permitan estandarizar los proyectos en la medida de lo posible, especialmente si se contempla la comunicación con servicios backend.

## Prácticas básicas

### 1. Ejecución mediante contenedores Docker

El proyecto debe tener la capacidad de ejecutarse en contenedores Docker. Esto con la finalidad de poder orquestar contenedores en diferentes ambientes y ejecutarlos independientemente de la plataforma destino.

Debes crear un directorio `docker/` en la carpeta raiz del proyecto y guardar ahí todos los archivo relacionados a Docker.

### 2. Archivo `README.md`

Incluir el archivo `README.md` en todo proyecto (en español por ahora). Este archivo cumple con el objetivo de presentar el proyecto, así como el stack tecnológico utilizado, incluyendo las versiones correspondientes.

**Contenido mínimo del README:**

- Prerrequisitos que se deben cumplir (stack + versiones) para hacer uso del proyecto.
- Instrucciones para ejecutar en modo desarrollo, realizar el build y ejecutar en modo test (aunque por ahora esta última puede omitirse).
- Instrucciones para la ejecución en Docker. El proyecto debe poder ejecutarse con o sin Docker.
- Instrucciones de despliegue (deploy).
- Incluirte como autora y al contribuidor.

> El README generalmente sigue un formato estándar para proyectos de este tipo; en eso puede ayudarte tu IA favorita.

### 3. Archivos de configuración global

Incluir archivos de configuración global, por ejemplo archivos `.env` que almacenen variables de entorno para el proyecto. Es importante parametrizar configuraciones relevantes como la URL de la API, API keys, puertos, etc. Sin embargo, **no se deben subir al repositorio los valores de las variables**. Debería quedar algo como:

```
.env
---------------------------------------------
URL_API=
PORT=
API_KEY=
...
---------------------------------------------
```

Además de este archivo, se debe crear otro con nombre `.env.dev`, para que el proyecto pueda ejecutarse en ambientes de desarrollo (`.env.dev`) y productivo (`.env`) por separado. También sirve como archivo de plantilla. Su contenido puede ser similar a:

```
.env.dev
---------------------------------------------
URL_API=<URL de la API>
PORT=<Puerto de la API>
API_KEY=<API key>
...
---------------------------------------------
```

### 4. Licencia del proyecto

Agregar el archivo de licencia en la carpeta raíz del proyecto. Para proyectos públicos, la licencia contemplada es **MIT License** (en inglés). Debes figurar como autora y agregar al contribuidor.

### 5. Archivo `NOTICE.md`

Agregar el archivo `NOTICE.md` (en inglés) en la carpeta raíz del proyecto. Puedes basarte en el que ya existe, aunque posteriormente se le harán ajustes. Debes figurar como autora y agregar al contribuidor.


### 6. Documentación

Agregar la carpeta `docs/` en la raiz del proyecto, aquí se pondrán archivos de documentación para el proyecto.

### 7. IA

Agregar la carpeta `ia/` en la raiz del proyecto, aquí se pondrán archivos ya sea de contexto, de skills o cualquier otro artifact que sea empleada por herramientas IA

### 8. Stack tecnológico

El framework, librería, lenguaje o cualquier otra herramienta de desarrollo es de libre elección. Mi recomendación es que no escojas la última versión más reciente, sino **escojer la versión estable más reciente (que es muy diferente, preferentemente un LTS o de soporte extendido)**
También tomar en cuenta que la versión seleccionada no sea tan reciante ( menor a 5 meses )

### 9. Versionado y repositorios

En todo proyecto, estamos usando repositorios Git, por lo tanto, van a existir 3 branches estándar para el proyecto:

- main   ->  Branch para modo productivo, siempre funcional
- dev    ->  Branch para modo desarrollo, debe compilar siempre
- test   ->  Branch para modo test, siempre funcional


Además de esos 3 branches, cada quíen debe tener 1 branch remoto propio, ahi vamos a ir subiendo nuestros cambios, funcionen o no, por ejemplo:

- dev-aot  -> mi branch dev
- dev-magui  -> tu branch dev  

para que se haga el merge/PR a alguna rama estándar, el branch propio, debe siempre compilar.

Hasta que nuestras versiones de desarrollo ya tengan un avanze significativo, ya aplicamos esa regla.

Desde ahora:

Cada commit debe seguir un formato:

```plaintext
tipo(scope): descripción
```

Ejemplos:

```plaintext
feat(permission): add permission entity

feat(task): implement task repository

fix(auth): resolve jwt validation issue

refactor(domain): simplify task aggregate

docs(readme): update installation guide

test(task): add task service tests

chore(docker): update postgres image
```

## Tipos más comunes

| Tipo     | Uso                     |
| -------- | ----------------------- |
| feat     | Nueva funcionalidad     |
| fix      | Corrección de bug       |
| refactor | Refactorización         |
| docs     | Documentación           |
| test     | Pruebas                 |
| chore    | Tareas de mantenimiento |
| build    | Build/Docker            |
| ci       | CI/CD                   |
| perf     | Optimización            |
| style    | Formato                 |

---

## Tags del versionado

Vamos a usar el formato SemVer

Consiste en un número de tres dígitos separados por puntos, siguiendo estrictamente la estructura: **`X.Y.Z`** ---

### ¿En qué consiste cada número?

Cada uno de estos dígitos tiene un significado muy específico basado en el tipo de cambios que se introducen en el código:

| Posición | Nombre | ¿Cuándo se incrementa? | Ejemplo |
| --- | --- | --- | --- |
| **`X`** | **MAJOR** (Mayor) | Cuando haces cambios incompatibles en la App. Si alguien actualiza a esta versión, su código podría romperse y requerir modificaciones. | `1.4.2` $\rightarrow$ `2.0.0` |
| **`Y`** | **MINOR** (Menor) | Cuando añades funcionalidad de manera compatible hacia atrás (p. ej., una nueva función o característica, pero lo que ya existía sigue funcionando igual). | `1.4.2` $\rightarrow$ `1.5.0` |
| **`Z`** | **PATCH** (Parche) | Cuando corriges errores (bugs) de manera compatible hacia atrás. No hay funciones nuevas, solo arreglos. | `1.4.2` $\rightarrow$ `1.4.3` |

---

Para que el sistema funcione, seguir estas reglas clave:

1. **Empezar en 0.y.z:** La versión `0.1.0` se usa para el desarrollo inicial. En esta etapa, la App no es estable y cualquier cosa puede cambiar en cualquier momento. 
2. **El lanzamiento inicial es 1.0.0:** Cuando el software ya está listo para producción y es estable, se define como la versión `1.0.0`.
3. **Reinicios a cero:** Cuando se incrementa el número **MAJOR**, los números MINOR y PATCH se reinician a `0` (ej: `1.5.3` $\rightarrow$ `2.0.0`). Cuando se incrementa el número **MINOR**, el PATCH se reinicia a `0` (ej: `1.5.3` $\rightarrow$ `1.6.0`).
4. **No se modifica el código publicado:** Una vez que una versión se libera, ese código específico no se puede modificar. Cualquier cambio requiere un número de versión nuevo.


Para cuando tengamos un MVP funcional, empezamos a usar SemVer





### 10. Otras consideraciones

Conforme avance el proyecto, iremos ajustando algunos archivos, agregando o quitando elementos.