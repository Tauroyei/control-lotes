# Control de Lotes

Aplicación web para llevar el control de los lotes de cartones de raspadita que salen de la imprenta: qué lotes se registraron, cuáles se despacharon y cuáles ya se pagaron.

## Vistas

- **Agregar**: registra un lote con el nombre de la plaza, el código del lote y la cantidad de cartones. Permite editar y eliminar lotes.
- **Despachos**: marca uno o varios lotes como despachados o enviados, con su fecha.
- **Pagos**: marca como pagados los lotes ya despachados, con la fecha de pago.

En todas las vistas se puede filtrar por plaza y buscar por código. Arriba hay un resumen de lotes y cartones en cada estado: sin despachar → despachado → pagado.

## Cómo funciona

La app es un solo archivo, [`control-lotes.html`](control-lotes.html), publicado como Artifact en claude.ai. Usa dos capacidades de esa plataforma:

- **`db`**: base de datos del artifact. Los lotes se guardan en la colección `lotes`.
- **`mcp`** con el conector de Google Sheets: después de cada cambio, la lista completa se copia a una hoja de cálculo.

Por eso el archivo solo funciona abierto desde claude.ai. Abierto directamente en un navegador o en GitHub Pages se ve la interfaz, pero no guarda datos.

## Configuración de Google Sheets

El ID de la hoja de cálculo **no está en el código**. La app lo lee de la base de datos del artifact, en el documento `config/sheets`:

```json
{ "spreadsheetId": "ID_DE_TU_HOJA" }
```

La hoja debe tener una pestaña llamada `Lotes` con estos encabezados en la fila 1:

| Plaza | Código del lote | Cartones | Fecha de registro | Estado | Fecha de despacho | Fecha de pago |
|---|---|---|---|---|---|---|

La base de datos es la fuente principal: cada sincronización reescribe la hoja con todos los lotes, así que los cambios hechos a mano en la hoja se pierden.
