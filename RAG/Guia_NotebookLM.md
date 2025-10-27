# **Construyendo un sistema RAG con NotebookLM**

Tutorial para la creación y uso de un sistema RAG usando NotebookLM.

## **Fase 1: Creación del cuaderno y definición de la base de conocimiento**

El primer paso es establecer el espacio de trabajo y definirlo con las fuentes de información relevantes.

* **Acceder a NotebookLM y crear un nuevo cuaderno.**  
  * Enlace: [NotebookLM](https://notebooklm.google.com). Accesible con una cuenta de google.  
  * Cree un nuevo **cuaderno**. Un cuaderno funciona como un espacio de trabajo o proyecto autónomo, donde se agruparán todas las fuentes y notas relacionadas con un tema específico.  

  <div align="center"><img src="/RAG/notebooklm/crear_cuaderno.png"></div>

* **Añadir fuentes.**  
  * Una vez creado lo primero que nos propone es añadir fuentes:

  <div align="center"><img src="/RAG/notebooklm/añadir_fuentes.png"></div>


  * Opciones disponibles:  
    * **Subir un archivo:** Arrastrando a la ventana es posible añadir archivos en formato pdf, txt, markdown o audio.  
    * **Desde Google Drive:** Permite navegar por los archivos almacenados.
    * **Añadir un sitio Web** 
    * **Añadir un enlace a YouTube**
    * **Pegar texto**  
    
* **Procesamiento inicial y resumen del cuaderno.**  
  * Una vez cargadas las fuentes, NotebookLM tardará unos momentos en procesarlas e indexarlas.  
  * Al finalizar, aparecerá un resumen generado automáticamente, los temas clave identificados y una lista de preguntas sugeridas.

  <div align="center"><img src="/RAG/notebooklm/resumen.png"></div>

## **Fase 2: Interactuando con sus fuentes**

Con la base de conocimiento creada, el siguiente paso es comenzar a consultarla.

* **Preguntar.**  
  * El cuadro de chat se localiza en la parte inferior de la pantalla.
  * Comenzar con una pregunta simple y directa cuyo contenido se encuentre en los documentos cargados.
* **Refinar consultas.**  
  * Intentar construir desde la respuesta inicial. Se pueden pedir aclaraciones ("Explica ese último punto con palabras más sencillas") o solicitar una expansión del análisis ("Compara los argumentos de la Fuente A con los de la Fuente B").  
* **Utilizar las Preguntas Sugeridas.**  
  * Después de cada respuesta, NotebookLM proporciona preguntas de seguimiento generadas automáticamente. Estas sugerencias son una buena manera de guiar la exploración y descubrir nuevas facetas del contenido.

## **Fase 3: Sintetizando y Verificando la Información**

La respuesta que aparece en el chat es el resultado de sintetizar la consulta del usuario con el contexto definido por las fuentes aportadas.

* **Analizar la respuesta.**  
  * Observar la estructura de la respuesta, comprobar que el texto generado contesta nuestra pregunta.
* **Verificar las notas.**  
  * Este es un paso fundamental. NotebookLM indexa las fuentes con un sistema similar a las notas al pie de página.  
  * Al pasar el ratón por una anotación, se resaltará instantáneamente el pasaje exacto de donde se extrajo esa información.
  <div align="center"><img src="/RAG/notebooklm/citas.png"></div>

## **Fase 4: Organización y exportación del conocimiento**
NotebookLM ofrece herramientas para organizar y exportar los conocimientos generados.

* **Guardar respuestas como notas.**  
  * El historial de preguntas es efímero.  
  * Utilice el botón *Guardar como nota* que aparece debajo de cada respuesta para conservarla.
  <div align="center"><img src="/RAG/notebooklm/guardar_nota.png"></div>

* **Utilizar herramientas de generación predefinidas.**  
  * Se pueden generar documentos predefinidos. Por ejemplo, mapas mentales interactivos o resúmenes de audio.
  <div align="center"><img src="/RAG/notebooklm/panel_notas.png"></div>

# **Estrategias para maximizar la eficacia de NotebookLM**

## **Crear cuadernos especializados**

La estrategia más efectiva para obtener buenos resultados es evitar la creación de un único cuaderno masivo con documentos no relacionados. Mezclar documentos poco relacionados solo confundirá a la IA y producirá resultados genéricos o poco relevantes. 

## **Ingeniería de prompts**

Para desbloquear un análisis más profundo, es necesario ir más allá de las preguntas simples de *qué* y empezar a formular preguntas de *cómo* y *por qué*. Por ejemplo:

* **Análisis comparativo:** "Compara y contrasta la metodología descrita en la Fuente A con las conclusiones de la Fuente B".
* **Juego de roles:** "Actúa como un periodista escéptico. Basándote en estos documentos, ¿cuáles son las tres preguntas más difíciles que le harías al autor?".
* **Síntesis creativa:** "Escribe un breve esquema para una entrada de blog que explique el concepto central de este documento técnico a una audiencia no especializada".  
* **Debate dialéctico:** "Construye un debate entre dos académicos imaginarios que interpretan este argumento de manera opuesta. ¿Qué evidencia del texto usaría cada uno para apoyar su punto de vista?".
* **Escenarios "Qué pasaría si":** "Si la idea central de este documento se aplicara a \[un problema del mundo real\], traza los resultados previstos y las consecuencias no deseadas".

Una técnica útil es la de **sobresolicitar**: pedir más respuestas de lo que se necesita para luego seleccionar las mejores.

## **Verificación y evaluación crítica**

Es crucial recordar que NotebookLM es una herramienta para aumentar el intelecto humano, no para reemplazarlo. El pensamiento crítico del usuario sigue siendo indispensable.

* **Verificación sistemática:** La práctica más importante es **revisar siempre las notas**. Antes de confiar en una afirmación generada por la IA, el usuario debe hacer clic en el número de la nota y leer el texto original para asegurarse de que la IA no lo ha malinterpretado o sacado de contexto. Este es el paso final para garantizar la fiabilidad.
* **Fuentes de calidad:** La precisión del sistema RAG depende enteramente de la calidad, exactitud y falta de sesgo de los documentos fuente proporcionados.