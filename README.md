# Tablero gerencial de alertas de deforestación – INFONA (DEMO)

Demostración de un tablero gerencial para la Presidencia del INFONA, construido sobre la capa de alertas de deforestación de la Dirección General de Oficinas Regionales (capa `DGOR_REPORTES`).

**Ver la demo:** abrir `index.html` desde la página publicada de este repositorio.

## Qué muestra

- Filtros por año, oficina regional, departamento, tipo y condición de atención, más búsqueda por reporte o distrito.
- Indicadores: alertas, % atendidas, pendientes, superficie registrada y oficinas regionales con alertas.
- Mapa con selector de mapas base (claro, gris, oscuro, satélite, topográfico, OpenStreetMap).
- Gráficos interactivos: al hacer clic en una barra se filtra todo el tablero.
- Tarjeta de detalle de cada alerta.
- Descargas **Excel** y **PDF** que incluyen solo los registros filtrados.

## Datos de la demo

`alertas.csv` contiene datos reales exportados de la capa, **sin información personal**:

- Se excluyeron: denunciado, sumariado, observaciones, descripción, número de acta y de oficio, e identificadores internos.
- Las coordenadas están redondeadas a 2 decimales (~1 km).

## Contenido del repositorio

| Archivo | Uso |
|---|---|
| `index.html` | Tablero de demostración (lee `alertas.csv`) |
| `alertas.csv` | Datos de la demo, ya depurados |
| `herramientas/limpiar_csv.html` | Prepara un CSV exportado de la capa para publicarlo (quita columnas sensibles y redondea coordenadas). Funciona en el navegador, sin enviar datos |
| `version_arcgis/index.html` | Versión institucional: consulta la capa **en vivo** en ArcGIS Enterprise, con inicio de sesión del portal |

## Cómo implementarlo en los servidores del INFONA

### Opción A – Versión en vivo (recomendada)

Muestra los datos actualizados por Field Maps, con detalle completo protegido por inicio de sesión.

1. Copiar `version_arcgis/index.html` a un servidor web del INFONA, idealmente en el mismo dominio del portal (`visor.infona.gov.py`). Así se evitan problemas de CORS y de autenticación.
2. Editar el bloque `CONFIG` al inicio del archivo:
   ```javascript
   layerUrl: "https://visor.infona.gov.py/server/rest/services/Hosted/DGOR_REPORTES/FeatureServer/0",
   portalUrl: "https://visor.infona.gov.py/portal",
   appId: "ID_DE_APLICACION_REGISTRADA",
   ```
3. En el portal: registrar el tablero como aplicación web y copiar el ID de aplicación en `appId`. Tipo **Explorador**; la URI de redireccionamiento es la URL donde se aloja el tablero.
4. Compartir la capa con un grupo que incluya a los usuarios autorizados. La capa **no** debe ser pública.
5. Si el tablero se aloja en otro dominio, agregar ese dominio en *Organización → Configuración → Seguridad → Orígenes permitidos*.

### Opción B – Versión con CSV

Funciona sin ArcGIS, en cualquier servidor web.

1. Copiar `index.html` y `alertas.csv` a la misma carpeta del servidor.
2. Para actualizar: exportar la capa a CSV, procesarla con `herramientas/limpiar_csv.html` si se va a publicar abiertamente, y reemplazar `alertas.csv`.
3. En un servidor interno (intranet) se puede usar el CSV completo, sin depurar.

## Tecnologías

HTML y JavaScript sin instalación ni compilación. Librerías desde cdnjs: Leaflet (mapa), PapaParse (CSV), SheetJS (Excel), jsPDF y jsPDF-AutoTable (PDF). La versión institucional usa además ArcGIS Maps SDK for JavaScript 4.30.

