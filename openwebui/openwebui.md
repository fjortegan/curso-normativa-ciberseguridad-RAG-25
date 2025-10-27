

# **Puesta en marcha y gestión de OpenWebUI con docker**

## **Instalación de requisitos software**

### **Instalación de docker**

Docker es la tecnología de contenedores que aislará y gestionará OpenWebUI y sus dependencias. El proceso de instalación varía según el sistema operativo: [documentación de instalación.](https://docs.docker.com/engine/install/)


### **Instalación de NVIDIA container toolkit (esencial para GPU)**

Docker por sí solo no puede interactuar con las GPUs de NVIDIA. El NVIDIA Container Toolkit actúa como un puente, permitiendo que los contenedores accedan al hardware de la GPU del host: [documentación de instalación.](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.18.0/)

### **Verificación de la instalación**

Antes de proceder, es vital verificar que todos los componentes están correctamente instalados:

* Verificar la versión de Docker: `docker --version`
* Ejecutar un contenedor de prueba: `docker run hello-world`  
* Verificar el acceso a la GPU desde docker **(solo Linux con NVIDIA)**:  
 `docker run --rm --gpus all nvidia/cuda:12.1.0-base-ubuntu22.04 nvidia-smi`  
 Este comando debe mostrar la tabla de estado de la GPU, confirmando que el container toolkit funciona correctamente.

## **Despliegue con docker run**

El comando docker run es la herramienta fundamental para iniciar contenedores. Aunque existen métodos más avanzados como docker compose, si disponemos de [docker desktop](https://www.docker.com/products/docker-desktop/), al ejecutar el comando una sola vez podemos gestionar posteriormente el contenedor desde la interfaz gráfica.

#### **Comando docker run**

`docker run -d --network host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui ghcr.io/open-webui/open-webui:main`

Este comando pone en marcha el contenedor, conectando con una instancia del servidor ollama funcionando en nuestro sistema.

Si queremos que el **contenedor siga funcionando entre reinicios** de nuestro sistema, habría que añadir al comando la opción: `--restart always`. Quedaría así:

`docker run -d --network host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main`

Este comando está pensado para ejecutarse una vez, creando el contenedor `open-webui`. Si el contenedor se para, basta ejecutar `docker start open-webui` para ponerlo de nuevo en marcha.

Para pararlo, el comando sería `docker stop open-webui`

## **Configuración inicial y gestión de la aplicación**

Una vez que los contenedores están en funcionamiento, el siguiente paso es configurar la aplicación OpenWebUI, crear la cuenta de administrador y empezar a gestionar modelos y funcionalidades como Retrieval-Augmented Generation (RAG).

### **4.1 Primer Acceso y Creación de la Cuenta de Administrador**

Este es un punto crítico que a menudo genera confusión entre los nuevos usuarios. El proceso correcto es el siguiente:

1. **Acceso a la Interfaz:** Abra un navegador web y navegue a la dirección http://localhost:3000 (o el puerto que haya configurado en el mapeo de puertos).17  
2. **Proceso de Registro ("Sign Up"):** En la primera visita, la pantalla mostrará un formulario de inicio de sesión. Es fundamental ignorar el inicio de sesión y, en su lugar, hacer clic en la opción para **crear una cuenta nueva ("Sign Up")**.27  
3. **Creación de la Cuenta de Administrador:** Rellene el formulario de registro con un nombre, correo electrónico y contraseña. La **primera cuenta que se crea en una instalación nueva de OpenWebUI se designa automáticamente como la cuenta de administrador**, otorgándole privilegios completos sobre el sistema.19

Esta particularidad en el flujo de configuración es una fuente común de errores. Los usuarios que intentan iniciar sesión con credenciales por defecto inexistentes se encuentran bloqueados. La documentación debe dejar claro que el primer paso es siempre el registro.  
Una vez creada la cuenta de administrador, cualquier cuenta posterior que se registre tendrá, por defecto, el rol de "pendiente" (pending), lo que significa que requerirá la aprobación manual del administrador para poder acceder a la aplicación.31

### **4.2 Panel de Administración y Gestión de Usuarios**

La cuenta de administrador tiene acceso a un panel de control completo desde el cual se puede gestionar toda la instancia de OpenWebUI.28

* **Navegación:** Se puede acceder al panel de administración haciendo clic en el avatar del usuario y seleccionando "Admin Panel".  
* **Control de Acceso Basado en Roles (RBAC):** Una de las características más potentes de OpenWebUI para entornos colaborativos es su sistema de RBAC. Los administradores pueden crear roles de usuario personalizados y grupos, asignando permisos granulares para controlar qué usuarios pueden acceder a qué modelos.11 Por defecto, todos los modelos son privados para el usuario que los descarga y deben ser compartidos explícitamente con otros usuarios o grupos, o hacerse públicos para toda la instancia.18 Este nivel de control es esencial para la gobernanza de datos y la seguridad en despliegues multiusuario o empresariales.

### **4.3 Gestión de Modelos y Configuración de RAG**

El panel de administración y la interfaz principal ofrecen herramientas intuitivas para gestionar los LLMs.

* **Descarga de Modelos de Ollama:** La forma más sencilla de añadir nuevos modelos es directamente desde la interfaz. Se puede ir a Settings \> Connections, seleccionar la conexión de Ollama y usar el campo de texto para buscar y descargar modelos de la biblioteca oficial de Ollama. Alternativamente, se puede escribir el nombre de un modelo (por ejemplo, llama3) en el selector de modelos de la ventana de chat principal. Si el modelo no está disponible localmente, aparecerá un botón para descargarlo.26  
* **Importación de Modelos GGUF:** OpenWebUI permite el uso de modelos en el popular formato GGUF de Hugging Face. Esto amplía enormemente el ecosistema de modelos disponibles más allá de la biblioteca oficial de Ollama. El proceso implica descargar el modelo GGUF y luego usar la interfaz de OpenWebUI para crear un nuevo Modelfile de Ollama que apunte a ese archivo local.32  
* **Configuración de Retrieval-Augmented Generation (RAG):** La funcionalidad RAG integrada transforma OpenWebUI de un simple chatbot a una potente plataforma de consulta de conocimiento personalizado. Permite a los LLMs acceder y utilizar información de documentos privados para responder preguntas. El proceso, que abstrae una gran complejidad técnica, es el siguiente 38:  
  1. **Crear una "Knowledge Base":** En la interfaz, navegue a Workspace \> Knowledge. Aquí, cree una nueva base de conocimiento, dándole un nombre y un propósito.  
  2. **Cargar Documentos:** Dentro de la base de conocimiento recién creada, cargue sus documentos. Se pueden subir archivos individuales (como .pdf, .txt, .md), directorios completos o incluso añadir contenido desde una URL. OpenWebUI procesará estos documentos, los dividirá en fragmentos (chunks), generará representaciones vectoriales (embeddings) y los almacenará en una base de datos vectorial interna.  
  3. **Crear un Modelo Personalizado:** Navegue a Workspace \> Models. Cree un nuevo modelo, asígnele un nombre, seleccione un modelo base potente (por ejemplo, qwen2:72b), y en la sección de configuración, asocie este nuevo modelo con la "Knowledge Base" creada en el paso anterior.  
  4. **Uso en el Chat:** Inicie un nuevo chat y seleccione el modelo personalizado que acaba de crear. Ahora, cuando haga una pregunta, el sistema primero buscará en su base de conocimiento los fragmentos de texto más relevantes para su consulta, los inyectará en el contexto del LLM y luego generará una respuesta basada tanto en su conocimiento preentrenado como en la información específica de sus documentos. También es posible invocar documentos o URLs directamente en un chat usando el prefijo \#.39

Esta capacidad de RAG democratiza una técnica de IA avanzada, permitiendo a usuarios sin experiencia en programación construir sistemas de preguntas y respuestas sobre sus propios datos, convirtiendo a OpenWebUI en una herramienta mucho más versátil y poderosa.