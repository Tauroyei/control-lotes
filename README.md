# Control de Lotes

Aplicación web para llevar el control de los lotes de cartones de raspadita que salen de la imprenta: qué lotes se registraron, cuáles se despacharon y cuáles ya se pagaron.

**Enlace:** https://tauroyei.github.io/control-lotes/

## Usuarios y acceso

Para usar la app hay que iniciar sesión con usuario y contraseña. Los usuarios se manejan desde la misma app:

- **Configuración inicial**: la primera vez que se abre la app pide crear el usuario administrador. Este paso se hace una sola vez.
- **Usuarios** (solo administradores): crear usuarios nuevos con su contraseña y rol, cambiar el rol y desactivar o volver a activar a alguien. Un usuario desactivado sale de la app al instante.
- **Contraseña**: cada persona puede cambiar la suya con el botón *Contraseña* de arriba. Si alguien olvida su contraseña, el administrador lo desactiva y le crea un usuario nuevo.

Hay dos roles:

| Rol | Puede |
|---|---|
| Administrador | Todo: agregar, editar y eliminar lotes, despachos, pagos, plazas y usuarios. |
| Operador | Agregar lotes, marcar o deshacer despachos y ver los enviados. |

## Vistas

- **Agregar**: registra un lote eligiendo la plaza de la lista, con el código del lote y la cantidad de boletería (cartones).
- **Despachos**: un menú desplegable muestra los lotes agregados que faltan por despachar; se marcan uno o varios con casillas y se despachan con su fecha.
- **Enviados**: muestra todos los lotes despachados desde que se empezó a usar la app, ordenados por código de lote. Se puede elegir un rango de fechas, y hay un indicador de cuántos lotes y cartones se enviaron por plaza.
- **Pagos** (administrador): un menú desplegable muestra los lotes despachados pendientes por pagar; al elegir uno se escribe cuánto pagaron (en quetzales) y la fecha. Abajo está el registro de lotes pagados: se pueden seleccionar varios y la app suma el total pagado. Desde el registro también se pone o cambia el monto de un lote, o se deshace el pago.
- **Plazas** (administrador): crea, renombra y elimina plazas. Al renombrar una plaza también se actualizan sus lotes.
- **Usuarios** (administrador): ver arriba.

En todas las vistas se puede filtrar por plaza y buscar por código. El botón **Descargar Excel** baja los lotes del filtro actual, con una hoja de resumen por plaza. Los cambios se ven al instante en todos los dispositivos que tengan la app abierta.

## Cómo funciona

- [`index.html`](index.html): la app completa (HTML, CSS y JavaScript en un solo archivo), publicada con GitHub Pages.
- [`firebase-config.js`](firebase-config.js): datos públicos del proyecto de Firebase.
- [`firestore.rules`](firestore.rules): reglas de seguridad de la base de datos.

El inicio de sesión usa **Firebase Authentication** (correo y contraseña; el usuario `ana` se guarda como `ana@lotes.app`). Los datos se guardan en **Cloud Firestore**:

- `usuarios/{uid}`: `usuario`, `rol` (`admin` u `operador`), `activo`, `creadoEn`.
- `config/setup`: marca que ya se creó el primer administrador.
- `plazas`: `nombre`, `creadaEn`.
- `lotes`: un documento por lote:

| Campo | Tipo | Ejemplo |
|---|---|---|
| `plaza` | texto | `LA SUERTE` |
| `codigo` | texto | `09` |
| `cartones` | número entero | `12000` |
| `fechaRegistro` | texto ISO | `2026-10-28T21:48:29.842Z` |
| `despachado` | sí/no | `true` |
| `fechaDespacho` | texto `aaaa-mm-dd` o vacío | `2026-10-28` |
| `pagado` | sí/no | `false` |
| `fechaPago` | texto `aaaa-mm-dd` o vacío | `null` |
| `monto` | número en quetzales (opcional) | `1500.5` |
| `creadoPor` | texto (opcional) | `ana` |

## Seguridad

La configuración de `firebase-config.js` no es secreta: Firebase la necesita en la página para conectarse. La protección la dan las reglas de [`firestore.rules`](firestore.rules):

- Sin sesión iniciada no se puede ver ni cambiar nada.
- Solo entra quien tiene un perfil activo en `usuarios`.
- Solo un administrador puede crear o cambiar usuarios, plazas y pagos, y eliminar lotes. Nadie puede cambiar su propio rol ni desactivarse a sí mismo.
- Un operador solo puede agregar lotes nuevos y marcar o deshacer el despacho de lotes sin pagar.
- Solo se aceptan datos con los campos de arriba y valores válidos. Cualquier otra colección está cerrada.

Para que funcione, en la consola de Firebase debe estar activado **Authentication → Método de acceso → Correo electrónico/contraseña**. Si cambias las reglas en este archivo, cópialas también en **Firestore Database → Reglas → Publicar**.

## Versión anterior

[`control-lotes.html`](control-lotes.html) es la primera versión, hecha como Artifact de claude.ai. Ya no se usa.
