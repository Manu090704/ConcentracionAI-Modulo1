# Evidencia de estadística: análisis de ventas de la categoría de leche

Este repositorio contiene un análisis estadístico de las ventas de distintos
fabricantes de leche. El estudio se desarrolla en un notebook de Jupyter y
abarca la limpieza y agregación mensual de los datos, la composición del
mercado, la relación entre precios y volúmenes, y la estimación de la
elasticidad-precio del producto principal de **FAB3**.

El análisis también compara un modelo de elasticidad simple con modelos que
incorporan tendencia, estacionalidad y el número de semanas observadas en cada
mes. Finalmente, propone una estrategia para pronosticar precios y ventas a 24
meses. Los resultados son descriptivos y no deben interpretarse como efectos
causales.

## Contenido del análisis

El notebook realiza las siguientes actividades:

1. Importa y revisa la base de ventas.
2. Identifica valores nulos, devoluciones, cantidades en cero y la consistencia
   entre los identificadores y los nombres de producto.
3. Agrupa la información a nivel producto-mes.
4. Calcula ventas, volumen, precios ponderados y participación de mercado por
   fabricante.
5. Identifica los fabricantes que acumulan al menos 80 % del mercado durante
   los últimos 12 meses.
6. Analiza las correlaciones de log-precios y log-volúmenes entre fabricantes.
7. Estima la elasticidad-precio directa del producto con mayores ventas de
   FAB3 mediante una regresión log-log.
8. Compara el modelo simple con modelos que controlan tendencia,
   estacionalidad y exposición semanal, incluyendo errores estándar HAC de
   Newey-West.
9. Propone alternativas para pronosticar precios y demanda a 24 meses.

Entre los resultados documentados en el notebook, **FAB1, FAB3 y FAB5** son los
fabricantes necesarios para superar 80 % de participación en la ventana móvil
analizada. El modelo simple estima una elasticidad de -1.7656 para el producto
principal de FAB3. Al agregar controles temporales y de calendario, la
estimación cambia y pierde significancia estadística, lo que evidencia la
sensibilidad del resultado a la especificación del modelo.

## Archivos del repositorio

| Archivo | Descripción |
| --- | --- |
| `retoEstadisticaParte1&2ManuelBajos.ipynb` | Notebook principal con el código, las gráficas, los modelos y la interpretación de resultados. |
| `DATA_VENTAS.xlsx` | Fuente de datos utilizada por el notebook. Contiene 3,296 registros y 15 variables en la hoja `datos`. |
| `Bajos_Rivera_Manuel.xlsx` | Archivo de evaluación con las hojas `Mi calificación` y `Rúbrica`. No es necesario para ejecutar el análisis. |
| `README.md` | Instrucciones de instalación, ejecución y descripción del proyecto. |

El notebook necesita que `DATA_VENTAS.xlsx` conserve ese nombre y permanezca
en la misma carpeta cuando se ejecuta localmente.

## Requisitos

- Git, si se descargará el proyecto mediante `git clone`.
- Python 3.11 o posterior. El notebook fue creado con Python 3.13.15.
- Una terminal y un navegador web.

Las dependencias de Python son:

- JupyterLab
- pandas
- NumPy
- Matplotlib
- seaborn
- statsmodels
- openpyxl

No se requieren credenciales, servicios externos, una base de datos ni
variables de entorno.

## Ejecución local

### 1. Descargar el repositorio

Clona el repositorio y entra en su carpeta:

```bash
git clone https://github.com/Manu090704/ConcentracionAI-Modulo1.git
cd ConcentracionAI-Modulo1
```

También puedes usar **Code > Download ZIP** en GitHub y descomprimir el archivo.
En ese caso, abre una terminal dentro de la carpeta descomprimida.

### 2. Crear un entorno virtual

En macOS o Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

En Windows PowerShell:

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Instalar las dependencias

Con el entorno virtual activo, ejecuta:

```bash
python -m pip install --upgrade pip
python -m pip install jupyterlab pandas numpy matplotlib seaborn statsmodels openpyxl
```

### 4. Abrir el notebook

```bash
jupyter lab "retoEstadisticaParte1&2ManuelBajos.ipynb"
```

Dentro de JupyterLab:

1. Confirma que el kernel seleccionado corresponda al entorno `.venv`.
2. Selecciona **Kernel > Restart Kernel and Run All Cells**.
3. Espera a que terminen las tablas, gráficas y regresiones.

Los resultados se muestran dentro del mismo notebook. El proceso no modifica
el archivo fuente `DATA_VENTAS.xlsx`.

## Ejecución en Google Colab

1. Abre [Google Colab](https://colab.research.google.com/).
2. Selecciona **Archivo > Subir notebook** y carga
   `retoEstadisticaParte1&2ManuelBajos.ipynb`.
3. En el panel de archivos de Colab, sube `DATA_VENTAS.xlsx` a `/content`.
4. Selecciona **Entorno de ejecución > Ejecutar todas**.

Si prefieres leer los datos desde Google Drive, coloca el archivo en esta ruta,
que es la configurada actualmente en el notebook:

```text
/content/drive/MyDrive/septimoSemestre/DATA_VENTAS.xlsx
```

El notebook monta Google Drive automáticamente cuando no encuentra
`DATA_VENTAS.xlsx` en su directorio de trabajo.

## Datos y metodología

La fuente contiene información de fabricante, producto, fechas semanales y
mensuales, valor de venta, litros, paquetes y precios unitarios. El flujo
principal es:

```text
DATA_VENTAS.xlsx
        |
        v
revisión y limpieza de registros
        |
        v
agregación mensual por fabricante y producto
        |
        +--> participación de mercado y descriptivos
        +--> correlaciones de precios y volúmenes
        +--> regresiones de elasticidad-precio
        +--> propuesta de pronóstico a 24 meses
```

Las tres filas con precios unitarios nulos se excluyen antes de aplicar
logaritmos. Las cantidades negativas se conservan como devoluciones para que
los totales netos coincidan con la fuente. Los precios agregados se calculan
como precios ponderados, dividiendo el valor total de venta entre el volumen
total correspondiente.

## Solución de problemas

### `FileNotFoundError: DATA_VENTAS.xlsx`

Ejecuta Jupyter desde la raíz del repositorio y verifica que el archivo conserve
exactamente el nombre `DATA_VENTAS.xlsx`.

### `ModuleNotFoundError`

Activa el entorno virtual e instala de nuevo las dependencias indicadas. Si el
notebook usa otro kernel, selecciónalo desde **Kernel > Change Kernel**.

### PowerShell impide activar el entorno

Puedes omitir la activación y ejecutar los comandos con el intérprete del
entorno:

```powershell
.venv\Scripts\python.exe -m pip install jupyterlab pandas numpy matplotlib seaborn statsmodels openpyxl
.venv\Scripts\python.exe -m jupyter lab "retoEstadisticaParte1&2ManuelBajos.ipynb"
```

### Las gráficas o resultados antiguos siguen visibles

Usa **Kernel > Restart Kernel and Run All Cells** para reconstruir todos los
resultados desde el inicio y en el orden correcto.
