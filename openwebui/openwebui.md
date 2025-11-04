

# **Puesta en marcha y gestión de OpenWebUI con docker**

# **Instalación de requisitos software**

## **Instalación de docker**

Docker es la tecnología de contenedores que aislará y gestionará OpenWebUI y sus dependencias. El proceso de instalación varía según el sistema operativo: [documentación de instalación.](https://docs.docker.com/engine/install/)


## **Instalación de NVIDIA container toolkit (esencial para GPU en sistemas Linux)**

En sistemas Linux, docker por sí solo no puede interactuar con las GPUs de NVIDIA. El NVIDIA Container Toolkit actúa como un puente, permitiendo que los contenedores accedan al hardware de la GPU del host: [documentación de instalación.](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.18.0/)

## **Verificación de la instalación**

Antes de proceder, es vital verificar que todos los componentes están correctamente instalados:

* Verificar la versión de Docker: `docker --version`
* Ejecutar un contenedor de prueba: `docker run hello-world`  
* Verificar el acceso a la GPU desde docker **(solo Linux con NVIDIA)**:  
 `docker run --rm --gpus all nvidia/cuda:12.1.0-base-ubuntu22.04 nvidia-smi`  
 Este comando debe mostrar la tabla de estado de la GPU, confirmando que el container toolkit funciona correctamente.

# **Despliegue con docker run**

El comando `docker run` es la herramienta fundamental para iniciar contenedores. Si disponemos de [docker desktop](https://www.docker.com/products/docker-desktop/), al ejecutar el comando una sola vez podemos gestionar posteriormente el contenedor desde la interfaz gráfica.

## **Comando docker run**

`docker run -d --network host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui ghcr.io/open-webui/open-webui:main`

Este comando pone en marcha el contenedor, conectando con una instancia del servidor ollama funcionando en nuestro sistema.

Si queremos que el **contenedor siga funcionando entre reinicios** de nuestro sistema, habría que añadir al comando la opción: `--restart always`. Quedaría así:

`docker run -d --network host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main`

Este comando está pensado para ejecutarse una vez, creando el contenedor `open-webui`. Si el contenedor se para, basta ejecutar `docker start open-webui` para ponerlo de nuevo en marcha.

Para pararlo, el comando sería `docker stop open-webui`

# **Configuración inicial y gestión de la aplicación**

Una vez que los contenedores están en funcionamiento, el siguiente paso es configurar la aplicación OpenWebUI, crear la cuenta de administrador y empezar a gestionar modelos y funcionalidades como Retrieval-Augmented Generation (RAG).

## **Creación de la cuenta de administrador**

La primera vez que se accede al sistema, se visualiza un carrusel de imágenes. Hay que pulsar en la flecha para entrar:

<div align="center"><img src="/openwebui/capturas/inicio.png"></div>

Lo primero que nos ofrece el sistema es el registro del usuario administrador del sistema:

<div align="center"><img src="/openwebui/capturas/registro_admin.png"></div>

Una vez completamos el registro entramos en la pantalla principal del sistema automáticamente.

<div align="center"><img src="/openwebui/capturas/pantalla_principal.png"></div>

## **Panel de administración**

El panel de administración permite solucionar algunos problemas que pueden surgir en la comunicación con ollama.

**Sin conexión a una API de ollama, openwebui se convierte en un cascarón vacío.**

Para acceder al panel de administración se puede usar el menú que se activa pulsando sobre el usuario en la esquina inferior izquierda de la pantalla principal.

<div align="center"><img src="/openwebui/capturas/menu_admin.png"></div>

Pulsando sobre *Admin Panel* se muestra el panel de administración.

<div align="center"><img src="/openwebui/capturas/panel_admin.png"></div>

Si tenemos problemas para conectar con ollama, debemos seleccionar *Settings* \> *Connections*. Se muestra el siguiente diálogo:

<div align="center"><img src="/openwebui/capturas/conexiones.png"></div>

Pulsando en el icono del engranaje de las opciones de ollama, se muestra el siguiente diálogo, donde se puede configurar la dirección donde se encuentra accesible la API de ollama. Pulsando sobre el icono de refrescar, comprueba la conexión.

<div align="center"><img src="/openwebui/capturas/ollama_config.png"></div>

<div align="center"><img src="/openwebui/capturas/ollama_config_diag.png"></div>



## **Gestión de modelos y configuración de RAG**

El panel de administración y la pantalla principal ofrecen herramientas para gestionar los LLMs.

* **Descarga de modelos de ollama:** La forma más sencilla de añadir nuevos modelos es directamente desde la interfaz. Se puede ir a *Settings* \> *Connections*, seleccionar la conexión de Ollama y usar el campo de texto para buscar y descargar modelos de la biblioteca oficial de Ollama. Alternativamente, se puede escribir el nombre de un modelo (por ejemplo, llama3) en el selector de modelos de la ventana de chat principal. Si el modelo no está disponible localmente, aparecerá un botón para descargarlo.
* **Importación de modelos GGUF:** OpenWebUI permite el uso de modelos en el popular formato GGUF de Hugging Face. Esto amplía el número de modelos disponibles más allá de la biblioteca oficial de Ollama. El proceso implica descargar el modelo GGUF y luego usar la interfaz de OpenWebUI para crear un nuevo Modelfile de Ollama que apunte a ese archivo local.  
* **Configuración de Retrieval-Augmented Generation (RAG):** La funcionalidad RAG integrada transforma OpenWebUI de un simple chatbot a una potente plataforma de consulta de conocimiento personalizado. Permite a los LLMs acceder y utilizar información de documentos privados para responder preguntas. El proceso, que abstrae una gran complejidad técnica, es el siguiente:  
  1. **Crear una "Knowledge Base":** En la interfaz, navegue a *Workspace* \> *Knowledge*. Aquí, cree una nueva base de conocimiento, dándole un nombre y un propósito.  
  2. **Cargar documentos:** Dentro de la base de conocimiento recién creada, cargue sus documentos. Se pueden subir archivos individuales (como .pdf, .txt, .md), directorios completos o incluso añadir contenido desde una URL. OpenWebUI procesará estos documentos, los dividirá en fragmentos (chunks), generará representaciones vectoriales (embeddings) y los almacenará en una base de datos vectorial interna.  
  3. **Crear un modelo personalizado:** Navegue a *Workspace* \> *Models*. Cree un nuevo modelo, asígnele un nombre, seleccione un modelo base potente (por ejemplo, qwen2:72b), y en la sección de configuración, asocie este nuevo modelo con la "Knowledge Base" creada en el paso anterior.  
  4. **Uso en el chat:** Inicie un nuevo chat y seleccione el modelo personalizado que acaba de crear. Ahora, cuando haga una pregunta, el sistema primero buscará en su base de conocimiento los fragmentos de texto más relevantes para su consulta, los inyectará en el contexto del LLM y luego generará una respuesta basada tanto en su propio conocimiento como en la información específica de sus documentos. También es posible invocar documentos o URLs directamente en un chat usando el prefijo \#.  

Esta capacidad de RAG democratiza una técnica de IA avanzada, permitiendo a usuarios sin experiencia en programación construir sistemas de preguntas y respuestas sobre sus propios datos, convirtiendo a OpenWebUI en una herramienta mucho más versátil y poderosa.
