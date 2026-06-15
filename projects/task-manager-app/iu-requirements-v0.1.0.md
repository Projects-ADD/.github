# Release 0.1.0 — Requisitos de Interfaz de Usuario

## Objetivo

Tener la aplicación con usuario _root_ lista para ser operada con las siguientes especificaciones. La interfaz de usuario debe manejar el diseño responsivo para visualizar en dispositivos diferentes como Pantallas de Laptop/escritorio, smartphones, tablets, pantallas de TV.

---

## 1. Usuario Root

- El usuario _root_ puede realizar cualquier acción dentro de la aplicación.
- Credenciales por defecto:
  - **Usuario:** `root`
  - **Contraseña:** `admin12345`
- **Importante:** La primera vez que se autentique, se debe forzar el cambio de contraseña antes de continuar con el flujo de la aplicación. Se debe agregar el campo `last_session` como nullable.
- Se puede usar el usuario _root_ en modo desarrollo, pero la mejor práctica es crear otro usuario con rol _Root_.

---

## 2. Roles y Permisos Iniciales

Al desplegar la aplicación **por primera vez**, la base de datos debe crear los siguientes roles y permisos:

| Rol | Permisos |
|-----|----------|
| **Root** | `write_task`, `read_task`, <br/>`write_permission`, `read_permission`, <br/>`write_rol`, `read_rol` |
| **User** | `write_task`, `read_task` |
| **Guest** | `only_view` |

Los permisos del rol _Root_ son básicos pero suficientes para realizar cambios en tareas, roles y permisos.

### Capacidades de Root

- Crear nuevos permisos.
- Crear nuevos roles y asignarles permisos.
- Quitar o agregar permisos a una cuenta de usuario, incluso si ya tiene un rol definido.

**Ejemplo:**

| Cuenta | Rol | Permisos |
|--------|-----|----------|
| `jperez` | User | `write_task`, `read_task` |
| `ahernandez` | User | `read_task` _(Root le quitó `write_task`)_ |

---

## 3. Tema y Paleta de Colores

- Tema y paleta de libre elección, considerando que debe poder ser dinámica en el futuro.
- Soportar cambio de tema **Dark / Claro**.

---

## 4. Inicio de Sesión

Asumiendo que ya existe al menos un usuario para iniciar sesión (o mecanismo de caché):

- Credenciales requeridas:
  - `username`
  - `contraseña`
- Incluir un verificador de humano (por ejemplo, captcha).
- Manejar errores de inicio de sesión adecuadamente.

---

## 5. Interfaz Web — General

- **Layout:** Holy Grail Layout (clásico).
- **Navbar** (lado superior derecho, colocar elementos de izquierda a derecha):
  1. Botón o switch para cambiar entre **Dark mode** / **Normal mode**.
  2. Menú de idiomas. El idioma por defecto es el configurado en el navegador. Si el navegador tiene un idioma no contemplado, se usará inglés.
     - Inglés
     - Español
     - Alemán
     - ...
  3. Campanita de notificaciones. Al hacer clic, muestra un menú con máximo 5 notificaciones y opción **"Ver más"**.
  4. Avatar del usuario logueado. Al hacer clic, despliega un menú con:
     - Configuración
     - Cerrar sesión
- **Lado superior izquierdo:** Botón de menú hamburguesa que despliega el _side menu_.
- **Side menu:** Se despliega al oprimir el botón de hamburguesa.
- **Footer:** En la parte inferior (contenido a criterio del desarrollador).

---

## 6. Side Menu

| Opción | Descripción |
|--------|-------------|
| **Home** | Área de trabajo de tareas |
| **Dashboard** | Estadísticas (solo admin, pendiente por definir) |
| **Settings** | Despliega submenú: |
| | — Users: pantalla de usuarios (CRUD) |
| | — Permissions: pantalla de permisos (CRUD) |
| | — Roles: pantalla de roles (CRUD) |
| | — Application: (por definir) |

---

## 7. CRUD de Tareas (Home)

Asumiendo un usuario (ej. `d_michi0`) que ya inició sesión y está en el área de trabajo donde se despliegan las tareas en forma de _cards_.

- **Agregar tarea: (ver Area de trabajo)** Botón para agregar tarea (una por una). Al hacer clic, se abre un modal para ingresar los datos. El modal debe poder guardar la tarea.
- Al guardar, la tarea se refleja en el espacio de trabajo.
- Cada tarea nueva se agrega al inicio del espacio de trabajo (cola inversa).
- La _card_ de la tarea tiene ancho (`width`) y alto (`height`) fijos  **(Evaluar viabilidad)**.
- Se establecen espacios predefinidos para las tareas (grid invisible).

### Editar / Actualizar

- Al hacer clic sobre la _card_, se abre el modal con los datos de la tarea para actualizarla.
  - **Éxito:** Se cambian los datos, se hace clic en **Save**, se cierra el modal y aparece un toast verde con mensaje de éxito.
  - **Error:** Se despliega un toast rojo.

### Eliminar

- La _card_ tiene un botón de bote de basura. Al hacer clic:
  - **OK:** Se borra la tarea (con confirmación previa).
  - **Cancel:** Se cierra el modal de confirmación.

---

## 8. Área de Trabajo

En el área superior del área de trabajo (no en el navbar, sino dedicar un pequeño espacio para una barra de herramientas), del lado derecho, se muestran:

- Botón para **agregar tarea** del usuario.
- Botón para **borrar todas las tareas** del usuario.
- Botón para **exportar a Excel** todas las tareas del usuario.
- **Barra de búsqueda** de tareas por título o descripción.
- **Botón de ordenar:**
  - Por título (asc — desc)
  - Por fecha (asc — desc)
- **Componente de filtrado** (pueden aplicarse uno o más filtros):
  - Por hechas / no hechas
  - Por usuario
  - Por rango de fechas

El área de trabajo es un grid invisible que permite mover las _cards_ de las tareas.

---

## 9. Cards de Tareas

- Las tareas se muestran como _cards_.
- En la parte superior derecha de cada _card_ hay una pequeña barra con:
  - **Botón de editar** (lápiz)
  - **Botón de borrar** (bote de basura)
- Debajo de la barra, se muestra el **título** de la tarea.
- Abajo del título, una **barra de color** con indicadores de status de tarea (por definir).
- Debajo de la barra de color, los **avatares** de los usuarios asociados a la tarea.

---

## 10. Pantalla de Usuarios (solo Root)

- Título: **Usuarios**
- Tabla con los datos de los usuarios.
- Última columna (**Actions**) permite:
  - Borrar usuario (con confirmación)
  - Editar datos
  - Deshabilitar / habilitar usuario

---

## 11. Pantalla de Roles (solo Root)

*(Pendiente por definir)*

---

## 12. Pantalla de Permisos (solo Root)

*(Pendiente por definir)*

---

## 13. Pendientes por Definir

- Agregar campo `username` a los usuarios (backend).
