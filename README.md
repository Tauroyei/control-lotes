# Control de Lotes

Aplicación web para llevar el control de los lotes de cartones de raspadita que salen de la imprenta: qué lotes se registraron, cuáles se despacharon y cuáles ya se pagaron.

**Enlace:** https://tauroyei.github.io/control-lotes/

## Vistas

- **Agregar**: registra un lote con el nombre de la plaza, el código del lote y la cantidad de cartones. Permite editar y eliminar lotes.
- **Despachos**: marca uno o varios lotes como despachados o enviados, con su fecha.
- **Pagos**: marca como pagados los lotes ya despachados, con la fecha de pago.

En todas las vistas se puede filtrar por plaza y buscar por código. Arriba hay un resumen de lotes y cartones en cada estado: sin despachar → despachado → pagado. Los cambios se ven al instante en todos los dispositivos que tengan la app abierta.

## Cómo funciona

- [`index.html`](index.html): la app completa (HTML, CSS y JavaScript en un solo archivo), publicada con GitHub Pages.
- [`firebase-config.js`](firebase-config.js): datos públicos del proyecto de Firebase.
- [`firestore.rules`](firestore.rules): reglas de seguridad de la base de datos.

Los lotes se guardan en **Cloud Firestore** (Firebase), en la colección `lotes`. Cada documento tiene:

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

## Seguridad

La configuración de `firebase-config.js` no es secreta: Firebase la necesita en la página para conectarse. La protección la dan las reglas de [`firestore.rules`](firestore.rules):

- Cualquier persona con el enlace puede ver, agregar, cambiar y eliminar lotes.
- Solo se aceptan lotes con los campos de arriba y valores válidos.
- Cualquier otra colección está cerrada.

Si cambias las reglas en este archivo, cópialas también en la consola de Firebase: **Firestore Database → Reglas → Publicar**.

## Versión anterior

[`control-lotes.html`](control-lotes.html) es la primera versión, hecha como Artifact de claude.ai. Ya no se usa.
