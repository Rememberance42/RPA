# README.md

# Laboratorio de Extracción y Automatización RPA

## Integrantes

- Isaac Torres Vargas
- [Agregar integrantes si aplica]

---

# Descripción del proyecto

Este proyecto implementa un flujo de automatización RPA para procesar solicitudes ficticias. 
El sistema recibe documentos PDF, extrae información mediante OCR, valida los datos obtenidos, 
genera un CSV aprobado y posteriormente utiliza Power Automate Desktop para registrar las 
solicitudes en un formulario.

El objetivo principal es evitar registrar información incorrecta mediante una separación clara
entre extracción, validación y automatización.

---

# Herramientas utilizadas

## Extracción y validación

- Python 3.12
- Entorno virtual `.venv`
- PyMuPDF
- Tesseract OCR
- pytesseract
- Pillow
- python-dotenv
- OpenAI SDK (preparado para modo real y utilizado en modo simulado)

## Automatización

- Microsoft Power Automate Desktop Version: 11.2610.155.0

## Sistema operativo

- Windows

---

# Estructura del proyecto


Proyecto integral de RPA-parcial 2/

│

├── scripts/

│   ├── E02_extraer_texto_ocr.py

│   ├── E03_extraer_con_reglas.py

│   ├── E04_extraer_con_llm.py

│   ├── E05_validar_comparar_exportar.py

│   └── comun.py

│

├── entradas/

│   ├── C01.pdf

│   ├── C02.pdf

│   └── C03.pdf

│

├── PAD/

|   ├── bitacora_solicitudes.txt

|   ├── Carga exitiosa.mp4

|   ├── Datos Invalidos.mp4

|   ├── Duplicado.mp4

|   ├── Flujo PAD y elementos UI.mp4

|   ├── Lento correcto.mp4

|   ├── NO se encontro la pantalla.png

|   ├── Consola de locura.png

├── salidas/

│   ├── ocr/

│   ├── reglas/

│   ├── llm_simulado/

│   ├── revision_humana.json

│   ├── reporte_validacion.json

│   ├── pendientes_revision.json

│   └── solicitudes_para_pad.csv

│

README.md

---

# Preparación del entorno

Crear y activar el entorno virtual:

```powershell
python -m venv .venv

.\.venv\Scripts\Activate.ps1


Instalar dependencias:

```powershell
pip install -r requirements.txt


Configurar Tesseract OCR:

Ruta utilizada:

```
C:\Program Files\Tesseract-OCR\tesseract.exe



# Ejecución del flujo de extracción

## 1. OCR

Convierte los documentos PDF en texto:

```powershell
.\.venv\Scripts\python.exe scripts/E02_extraer_texto_ocr.py
```

Resultado:

```
salidas/ocr/
```

---

## 2. Extracción mediante reglas

Ejecutar:

```powershell
.\.venv\Scripts\python.exe scripts/E03_extraer_con_reglas.py
```

Obtiene los campos:

- id_solicitud
- solicitante
- área
- tipo
- descripción

---

## 3. Extracción LLM simulada

Ejecutar:

```powershell
.\.venv\Scripts\python.exe scripts/E04_extraer_con_llm.py
```

El modo simulado permite probar el flujo sin realizar llamadas externas.

Para modo real:

```powershell
.\.venv\Scripts\python.exe scripts/E04_extraer_con_llm.py --modo real --autorizar-costo
```

requiere una clave API configurada localmente.

---

## 4. Validación y exportación

Ejecutar:

```powershell
.\.venv\Scripts\python.exe scripts/E05_validar_comparar_exportar.py --metodo reglas
```

Genera:

```
solicitudes_para_pad.csv
```

El CSV aprobado contiene únicamente solicitudes validadas.

Resultado final esperado:

```
CSV: 2 filas; método: reglas.
```

---

# Flujo de Power Automate Desktop

El flujo realiza:

1. Lectura del CSV aprobado.
2. Localización de la aplicación.
3. Captura de elementos UI.
4. Escritura de datos en el formulario.
5. Confirmación del registro.
6. Lectura del folio generado.
7. Escritura de bitácora.

La automatización no procesa solicitudes rechazadas por la etapa de validación.

---

# Pruebas realizadas

Se documentaron los siguientes escenarios:

- Documento completo aprobado.
- Documento con información faltante (C03).
- Datos inválidos en formulario.
- Identificador duplicado.
- Respuesta lenta y espera.
- Aplicación cerrada durante la búsqueda.

Cada prueba incluye:

- preparación realizada,
- resultado esperado,
- resultado obtenido,
- evidencia asociada.

---

# Limitaciones conocidas

- El modo LLM utilizado para pruebas es simulado y no representa una evaluación real del modelo.
- Los documentos utilizados contienen información ficticia.
- Las salidas generadas deben revisarse antes de enviarse a producción.
- Los tiempos de espera dependen del comportamiento de la aplicación objetivo.
- No todos los errores permiten reintentos automáticos, debido al riesgo de duplicar registros.

---

# Protección de archivos

No se incluyen:

- claves API,
- archivos `.env`,
- entornos virtuales,
- cachés,
- información personal.

Las evidencias contienen únicamente datos ficticios proporcionados por el laboratorio.


---

# Reflexión (150–200 palabras)


## Reflexión

En este proyecto se evitó registrar información incorrecta separando las etapas de extracción, validación y automatización. El OCR únicamente convierte los documentos en texto; posteriormente las reglas de extracción organizan los campos, pero sus resultados no se consideran confiables hasta pasar por una revisión y validación. Esta separación permite detectar datos faltantes como ocurrió con el caso C03, el cual no fue enviado al formulario debido a que no contaba con toda la información requerida.

La comprobación del guardado es importante porque una automatización no debe asumir que una acción fue exitosa solamente porque se ejecutó un clic. Por ello, el flujo verifica mensajes de confirmación, obtiene el folio generado y registra el resultado en una bitácora.

No todas las acciones pueden reintentarse automáticamente porque algunas operaciones tienen efectos permanentes. Si un registro pudo haberse creado pero la confirmación no fue recibida, repetir la acción podría generar duplicados. Por esta razón, el proceso utiliza estados pendientes y verificaciones posteriores antes de intentar nuevamente una operación crítica.
