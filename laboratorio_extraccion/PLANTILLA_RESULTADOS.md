# Evidencia del equipo

Integrantes: 
Sistema / Python / Tesseract / spa disponible: 
Ruta del ensayo: 
LLM real o simulado / modelo (si real): 

## Inspección

PDF digital, caracteres extraídos:
- Un PDF digital ya contiene una capa de texto. El texto puede extraerse directamente sin necesidad de OCR.
- Ejemplo: C01_digital.pdf → 179 caracteres extraídos.
PDF escaneado, caracteres extraídos directamente:
- Un PDF escaneado normalmente solo contiene imágenes, por lo que la extracción directa devuelve 0 caracteres. Primero debe aplicarse OCR para convertir la imagen en texto.
- Ejemplos:
  - C01.pdf → 0 caracteres
  - C02.pdf → 0 caracteres
  - C03.pdf → 0 caracteres
Diferencia entre OCR, extracción, validación y revisión:
- OCR: Convierte los píxeles de una imagen en texto reconocible.
  - Ejemplo de error: confundir "0" con "O" o "1" con "I".
- Extracción: Organiza el texto obtenido para identificar campos específicos (por ejemplo, nombre, folio o fecha).
  - Ejemplo de error: asignar una fecha al campo de folio o no detectar un campo existente.
- Validación: Comprueba que los datos cumplen reglas definidas (formato, longitud, rangos o campos obligatorios).
  - Ejemplo de error: detectar que una fecha tiene un formato inválido o que un folio no cumple el patrón esperado.
- Revisión: Verificación realizada por una persona para confirmar que la información coincide con el documento original y corregir posibles errores.
  - Ejemplo de error: el revisor identifica que el OCR leyó un apellido de forma incorrecta y lo corrige.

## Observaciones por campo

Completa una fila por cada campo de cada documento (15 filas).

| Caso | Campo | Visible en original | Texto OCR | Reglas | LLM y modo | Coincide | Formato válido | Diagnóstico confirmado |
|---|---|---|---|---|---|---|---|---|
| C01 | id_solicitud | SOL-801 | SOL-801 | Debe cumplir formato SOL-### | No usado | Sí | Sí | Sin error |
| C01 | solicitante | Persona ficticia Alba | Persona ficticia Alba | Debe existir y contener un nombre legible | No usado | Sí | Sí | Sin error |
| C01 | área | Administración | Administración | Debe pertenecer a un área válida | No usado | Sí | Sí | Sin error |
| C01 | tipo | Equipo | Equipo | Debe pertenecer a un tipo permitido | No usado | Sí | Sí | Sin error |
| C01 | descripción | Revisar la pantalla del equipo de práctica. | Revisar la pantalla del equipo de práctica. | Debe existir y contener una descripción legible | No usado | Sí | Sí | Sin error |
| C02 | id_solicitud | SOL-802 | SOL-802 | Debe cumplir formato SOL-### | No usado | Sí | Sí | Sin error |
| C02 | solicitante | Persona ficticia Bruno | Persona ficticia Bruno | Debe existir y contener un nombre legible | No usado | Sí | Sí | Sin error |
| C02 | área | Docencia | Docencia | Debe pertenecer a un área válida | No usado | Sí | Sí | Sin error |
| C02 | tipo | Acceso | Acceso | Debe pertenecer a un tipo permitido | No usado | Sí | Sí | Sin error |
| C02 | descripción | Habilitar acceso al aula de práctica. | Habilitar acceso al aula de práctica. | Debe existir y contener una descripción legible | No usado | Sí | Sí | Sin error |
| C03 | id_solicitud | SOL-803 | SOL-803 | Debe cumplir formato SOL-### | No usado | Sí | Sí | Sin error |
| C03 | solicitante | Persona ficticia Celia | Persona ficticia Celia | Debe existir y contener un nombre legible | No usado | Sí | Sí | Sin error |
| C03 | área | Biblioteca | Biblioteca | Debe pertenecer a un área válida | No usado | Sí | Sí | Sin error |
| C03 | tipo | No aparece en original | No aparece en OCR | Debe pertenecer a un tipo permitido y no estar vacío | No usado | Sí | No | Campo ausente/incierto |
| C03 | descripción | Revisar la lámpara del espacio de lectura. | Revisar la lámpara del espacio de lectura. | Debe existir y contener una descripción legible | No usado | Sí | Sí | Sin error |
Diagnósticos: OCR, extracción, incumplimiento de reglas, ausente/incierto, fallo técnico. Pueden coexistir. No confundir una hipótesis automática de OCR con una comprobación visual.

## Ejercicio de modificación

**Cambio general de etiqueta Area / Área:  **
Se modificó el extractor para aceptar las etiquetas "Área:" y "Area:" como equivalentes. Se actualizó el patrón de reconocimiento y el mapa de equivalencias para almacenar ambos casos en el campo interno area.

**Prueba agregada y resultado:**  
Se agregó una prueba con la entrada "Area: Docencia" para comprobar que la etiqueta sin acento se extrae correctamente. Después del ajuste, las pruebas E24 finalizan correctamente con resultado OK.

**Por qué no memoriza documentos:  **
El extractor utiliza reglas generales basadas en etiquetas y patrones, no información específica de documentos anteriores. No contiene nombres, IDs ni datos de solicitudes, por lo que puede procesar nuevos documentos sin almacenar casos previos.

## Comparación y revisión
**Huellas OCR iguales por caso:**  
Sí. Las huellas (SHA-256) del OCR coincidieron con las registradas para C01, C02 y C03, indicando que los archivos OCR revisados no fueron modificados.

**Aciertos observados de reglas (por documento):**  
- C01: Se extrajeron correctamente los cinco campos mediante reglas.  
- C02: Se extrajeron correctamente el identificador, solicitante, área, tipo y descripción a partir del texto en formato de párrafo.  
- C03: Se extrajeron correctamente los campos presentes; el campo **Tipo** fue identificado como ausente y la validación lo marcó como dato faltante.

**Aciertos observados del LLM real, o NO MEDIDO si simulado:**  
NO MEDIDO (no se evaluó el método LLM real).

**Limitaciones de la comparación literal de descripciones:**  
La comparación literal puede considerar diferentes textos que solo cambian en espacios, saltos de línea o formato, aunque el contenido sea el mismo. Por ello, la revisión humana es necesaria para confirmar la fidelidad respecto al documento original.

**Método elegido para CSV:**  
Reglas.

**Filas aprobadas, revisor y justificación:**  
- C01: Aprobado. Revisor: Isaac Torres Vargas. Los cinco campos coinciden con el documento original y cumplen las reglas de validación.  
- C02: Aprobado. Revisor: Isaac Torres Vargas. Los cinco campos fueron verificados y coinciden con el documento original.  
- C03: No aprobado. El campo **Tipo** está ausente en el documento original y no debe inventarse.

**Casos pendientes y causa:**  
- C03: Pendiente porque el documento original no contiene el campo **Tipo**. De acuerdo con las instrucciones del laboratorio, el resultado no debe aprobarse ni completarse manualmente.

## Evidencias

Rutas de OCR, JSON de ambos métodos, reporte, pendientes, revisión y CSV: C:\RPA\laboratorio_extraccion\salidas
Resultado de unittest: .\.venv\Scripts\python.exe -m unittest discover -s pruebas -v

## Reflexión de 150-200 palabras

Durante este laboratorio comprendí que la extracción automática de información requiere varias etapas claramente diferenciadas. Primero, el OCR convierte la imagen en texto, pero eso no garantiza que el contenido sea correcto o completo. Después, la extracción organiza ese texto en campos específicos y, finalmente, la validación verifica que cada dato cumpla las reglas definidas. También aprendí que un documento puede verse legible y aun así contener errores silenciosos que solo se detectan al compararlo con el original. La revisión humana sigue siendo necesaria para confirmar que los resultados son fieles al documento fuente y para evitar aprobar información incorrecta o inventada. Otro aspecto importante fue comprender que las reglas deben ser generales y no memorizar datos de casos específicos, lo que permite procesar documentos nuevos de forma consistente. Además, observé la importancia de conservar las huellas digitales y las firmas generadas por el sistema para mantener la trazabilidad de los resultados. En conjunto, esta práctica me permitió entender cómo combinar OCR, extracción por reglas, validación y revisión humana para obtener datos confiables antes de exportarlos a un sistema de automatización como Power Automate Desktop.
