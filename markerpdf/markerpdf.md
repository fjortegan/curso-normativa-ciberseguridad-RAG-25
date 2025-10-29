

# **Marker-pdf: Guía de instalación y uso**

## **Requisitos del sistema**

Antes de la instalación, es fundamental asegurar que el entorno cumple con los requisitos necesarios para el funcionamiento de Marker.

* **Software:** Se requiere una versión de Python 3.10 o superior. La dependencia más importante es PyTorch, que debe instalarse de acuerdo con el hardware del sistema. Los usuarios deben consultar la documentación oficial de PyTorch para obtener la versión correcta compatible con su configuración específica, ya sea CPU, una GPU con CUDA o un sistema Apple Silicon con MPS.  
* **Hardware:** Marker es flexible y puede ejecutarse en CPU, GPU o MPS. Sin embargo, es crucial gestionar las expectativas de rendimiento. Una CPU es viable para tareas ligeras, pero resultará significativamente más lenta para documentos que requieran un uso intensivo de OCR. Una GPU acelera drásticamente el proceso, especialmente la detección de la estructura y el OCR, convirtiéndose en la opción recomendada para cargas de trabajo a gran escala o con plazos ajustados.

## **Instalación de marker-pdf**

El proceso de instalación puede adaptarse según las necesidades.

* **Instalación Estándar:** Para procesar archivos PDF, la instalación principal se realiza con un único comando a través de pip:  
  `pip install marker-pdf`  
* **Instalación con Funcionalidad Completa:** Para aprovechar la capacidad de Marker de procesar otros tipos de archivo además de PDF (como DOCX, PPTX, EPUB, etc.), es necesario instalar las dependencias adicionales. Esto se logra con la siguiente variante del comando:  
  `pip install marker-pdf[full]`

  2 Este es un detalle crucial que a menudo se pasa por alto y es esencial para desbloquear todo el potencial de la herramienta.  

## **La interfaz de línea de comandos (CLI)**
La interfaz de línea de comandos (CLI) es el método principal y más directo para interactuar con Marker. La herramienta proporciona comandos distintos diseñados para diferentes escalas de operación, desde la depuración de un único archivo hasta el procesamiento a gran escala de miles de documentos.

### **Procesamiento de documentos individuales (marker\_single)**
Para convertir un único archivo, ya sea para pruebas, depuración o tareas puntuales, se utiliza el comando marker\_single. Su sintaxis básica es:

`marker_single /ruta/al/archivo.pdf --output_dir /ruta/a/la/carpeta/de/salida`

Una de las fortalezas de Marker es su flexibilidad de entrada. Este comando no solo acepta archivos PDF, sino también imágenes y otros formatos de documento como DOCX o EPUB, siempre que se hayan instalado las dependencias completas (`pip install marker-pdf[full]`).

**Ejemplos prácticos:**
* Convertir las primeras 10 páginas de un libro:  
  `marker_single libro.pdf --output_dir ./output --page_range "0-9"`

* Convertir una imagen escaneada forzando el OCR:  
  `marker_single pagina_escaneada.png --output_dir ./output --force_ocr`

### **Formatos de salida: Markdown, HTML y JSON**
Marker puede generar la salida en varios formatos, seleccionables con el parámetro \--output\_format, cada uno adecuado para diferentes casos de uso.2

* **Markdown (predeterminado):** Es el formato más común y legible por humanos. La salida incluye enlaces a las imágenes extraídas (guardadas en la misma carpeta), tablas formateadas con la sintaxis de Markdown y otros elementos. 
* **HTML:** La salida HTML es estructuralmente similar a la de Markdown, pero utiliza etiquetas HTML estándar para los elementos.  
* **JSON:** Este es el formato más potente para el uso automatizado. Proporciona una representación estructurada y jerárquica del documento, que puede ser fácilmente analizada por otras aplicaciones.

## **Pre-procesamiento para RAG**
La aplicación más relevante de marker-pdf es su papel como herramienta de pre-procesamiento para RAGs. La calidad de la fase de análisis inicial de documentos tiene un efecto en cascada que determina el rendimiento de todo el procesamiento del RAG.

### **Mejores prácticas usando marker-pdf**
Para optimizar la salida de marker-pdf específicamente para aplicaciones RAG, se recomiendan las siguientes prácticas:

* **Utilizar el modo híbrido (--use\_llm):** Para documentos complejos, especialmente aquellos con tablas o formularios, la precisión superior del análisis mejorado por LLM producirá fragmentos mejor estructurados y, por lo tanto, una recuperación más precisa.  
* **Gestionar imágenes:** El parámetro `--disable_image_extraction` tiene un comportamiento especial cuando se usa junto con `--use_llm`, en lugar de simplemente omitir las imágenes, las reemplaza con descripciones textuales generadas por el LLM. Estas descripciones pueden ser indexadas, añadiendo una dimensión multimodal de búsqueda al sistema RAG.  

