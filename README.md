# Repositorio de Hojas de Seguridad (SGA) – Kenzo Jeans

Aplicación de consulta rápida de las fichas de datos de seguridad de los productos químicos usados en la Planta de Tratamiento de Agua Residual y en la planta de producción de Kenzo Jeans.

> **SGA** = Sistema Globalmente Armonizado de clasificación y etiquetado de productos químicos.

## Propósito

- Tener en un solo lugar todas las hojas de seguridad, en lugar de carpetas físicas o archivos dispersos.
- Permitir que cualquier persona encuentre una ficha en segundos, desde el celular o el computador.
- Saber de un vistazo qué fichas están vigentes y cuáles requieren actualización.

## Cómo funciona

```
Hoja de cálculo (Google Sheets)  →  Aplicación (Streamlit)  →  Consulta por sustancia
```

1. El listado de sustancias se mantiene en una hoja de cálculo de Google Sheets.
2. La aplicación descarga la hoja como CSV y la muestra como tarjetas desplegables.
3. Cada tarjeta muestra familia, fecha de última revisión, vigencia, pictograma y enlace a la ficha de seguridad.

**Para agregar o actualizar una ficha no hay que tocar el código:** se edita la hoja de cálculo y la aplicación toma los cambios sola (se actualiza cada 10 minutos o al pulsar **"Recargar datos"**).

## Estructura de la hoja de cálculo

La aplicación busca la fila de encabezados dentro de las primeras 30 filas, así que la hoja puede tener un título o filas previas. Columnas esperadas:

| Columna | Contenido |
|---|---|
| `SUSTANCIA/QUÍMICO` | Nombre de la sustancia |
| `FAMILIA` | Familia o categoría (si está vacía se muestra como `SIN FAMILIA`) |
| `FECHA` | Fecha de última revisión de la ficha |
| `VIGENCIA` | `VIGENTE` o `NO VIGENTE` |
| `URL a Ficha de seguridad` | Enlace al documento (por ejemplo, en Drive) |
| `PICTOGRAMA` | Enlace a la imagen del pictograma SGA (opcional) |

Los encabezados se reconocen por palabras clave (`sustancia`, `famil`, `fecha`, `vigencia`, `url`/`ficha`, `picto`). Si se renombran, conviene que conserven esas palabras.

### Cómo se interpreta cada dato

- **Vigencia:** se acepta `VIGENTE`, `NO VIGENTE` y variantes (`NO-VIGENTE`, `VIGENT`, `SI`, etc.). Un valor vacío, `N/A` o `SIN DATO` queda como "sin dato".
- **Una ficha "sin dato" se cuenta como no vigente** en los contadores y en el filtro de "No vigentes".
- **Vigencia no se calcula por fecha:** es lo que está escrito en la hoja. La fecha solo se muestra como referencia.
- **Fecha:** se muestra como `dd/mm/aaaa`. También convierte las fechas que Excel guarda como número.
- **Familia:** se convierte a mayúsculas para evitar duplicados por escritura.
- **Enlaces:** solo se muestran si comienzan por `http`. Si no hay enlace, la tarjeta muestra "Enlace no disponible"; si no hay pictograma, muestra un ícono genérico.

## Qué ofrece la aplicación

- **Búsqueda** por nombre de sustancia.
- **Filtro por familia** (selección múltiple).
- **Filtro por vigencia:** todos, vigentes o no vigentes.
- **Contadores** en la barra lateral: total de fichas, vigentes y no vigentes.
- **Tarjeta por sustancia** con detalle, pictograma, enlace a la ficha y aviso cuando el documento no está vigente.
- **Fuentes alternativas:** otra URL o ID de Google Sheets, o subir un archivo XLSX/CSV si la descarga falla.

## Fuente de datos

La URL de la hoja oficial viene precargada en el campo de la barra lateral (en el código, dentro de `st.text_input`). Para que la aplicación pueda leerla, la hoja debe estar compartida como **"Cualquier persona con el enlace → Lector"**. La descarga prueba tres formas de exportar el CSV antes de fallar.

Si la hoja oficial cambia de lugar, se actualiza esa URL en el código.

## Tecnologías y dependencias

Python con Streamlit, Pandas, Requests y openpyxl (lectura de Excel).

> Completar: confirmar que `requirements.txt` incluya `streamlit`, `pandas`, `requests` y `openpyxl`.

## Ejecución y despliegue

> Completar:

- **Archivo principal:** `app_sga.py`
- **Instalar dependencias:** `pip install -r requirements.txt`
- **Ejecutar en local:** `streamlit run app_sga.py`
- **Despliegue:** Streamlit Cloud, cuenta `[completar]`, URL `[completar]`
- **Dónde están las fichas (carpeta de Drive):** `[completar]`

## Mantenimiento del repositorio de fichas

> Completar según el procedimiento de la empresa:

- **Quién agrega o actualiza las fichas:** `[área / cargo]`
- **Cada cuánto se revisan:** `[periodicidad]`
- **Cómo se marca una ficha como no vigente:** cambiar el valor de `VIGENCIA` en la hoja.

## Responsables

- **Dueño del proceso (gestión de químicos):** `[área / cargo]`
- **Mantenimiento técnico de la aplicación:** `[nombre / cargo]`
- **Cuentas (GitHub, Streamlit, Google):** `[a nombre de quién]`
