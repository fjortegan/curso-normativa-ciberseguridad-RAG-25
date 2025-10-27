

# **Instalación y optimización de ollama**

## **Introducción a ollama**

La capacidad de ejecutar Modelos de Lenguaje (LLMs) directamente en hardware personal marca un punto de inflexión.

ollama emerge como una herramienta fundamental en esta revolución, actuando como un marco integral diseñado para simplificar la instalación, gestión y ejecución de LLMs de código abierto en una computadora personal.

La arquitectura de ollama empaqueta modelos en una herramienta de línea de comandos y un servidor de API REST local.

Esta abstracción permite que un proceso que antes requería profundos conocimientos en gestión de dependencias de Python, configuración de entornos CUDA y manejo de grandes archivos de modelos, se reduzca a un único comando.

Esta democratización del acceso es, quizás, el valor más significativo de ollama, ya que abre la puerta a desarrolladores, investigadores y entusiastas para que experimenten con IA sin necesidad de ser expertos en aprendizaje automático.

## **Ventajas: Privacidad, coste y personalización**

La ejecución de LLMs en un entorno local ofrece un conjunto de ventajas:

* **Privacidad y soberanía:** En un modelo local, todas las consultas y los datos procesados permanecen en la máquina del usuario. Nunca se transmiten a servidores de terceros. Esto es de vital importancia para manejar información sensible, propietaria o personal, garantizando una confidencialidad total.  
* **Control de coste:** Los modelos de IA basados en la nube operan con un modelo de pago por uso, donde cada solicitud (token) incurre en un coste. La ejecución local elimina este gasto recurrente. Tras la inversión inicial en hardware, la experimentación y el uso son ilimitados, lo que fomenta la innovación sin la preocupación de acumular facturas elevadas.  
* **Personalización y flexibilidad:** ollama permite un control granular sobre los modelos. Los usuarios pueden crear variantes personalizadas, ajustar parámetros de inferencia como la "creatividad" (temperatura) y operar completamente sin conexión a internet.

## **El ecosistema ollama**

El ecosistema de ollama está compuesto por tres componentes interconectados que trabajan en conjunto para proporcionar una experiencia fluida:

* **El servidor (ollama serve):** Es el corazón de ollama. Gestiona todas las operaciones: carga de modelos en memoria (RAM o VRAM), procesamiento de solicitudes de inferencia y exposición de una API REST. La existencia de esta API estandarizada es clave, ya que transforma a ollama en una plataforma. Permite que herramientas de terceros —desde interfaces gráficas de usuario como Open WebUI hasta extensiones de editores de código como VS Code y plataformas de automatización como n8n— se integren perfectamente con los modelos locales. Instalar ollama es, en esencia, establecer un centro de IA personal en la propia máquina.  
* **La interfaz de línea de comandos:** La herramienta ollama es la interfaz principal para la interacción con el usuario. A través de comandos simples, permite descargar modelos de la biblioteca oficial, ejecutarlos en modo chat interactivo, listar los modelos instalados y monitorear los procesos activos.
* **La librería de modelos:** ollama mantiene una biblioteca de modelos de lenguaje de código abierto populares, pre-empaquetados y optimizados para una fácil descarga y ejecución. Esta biblioteca, accesible en [ollama.com](https://ollama.com/library), es el punto de partida para explorar las capacidades de diferentes arquitecturas.

## **Requisitos del sistema y estrategia de hardware**

La experiencia de usar ollama está intrínsecamente ligada al hardware sobre el que se ejecuta. Una configuración adecuada no solo determina si un modelo se puede ejecutar, sino también la velocidad y la fluidez de la interacción. Esta sección desglosa los requisitos de hardware y software, explicando la relación entre cada componente y el rendimiento para guiar las decisiones de configuración o actualización.

### **Requisitos de hardware: CPU, RAM y almacenamiento**

* **CPU:** Aunque la GPU es preferible para la inferencia, la CPU sigue siendo un componente crucial. Se recomienda un mínimo de 4 núcleos para tareas básicas, pero para ejecutar modelos más grandes (de 13 mil millones de parámetros o más) de manera efectiva, es aconsejable contar con al menos 8 núcleos.8 Sin embargo, el número de núcleos no es el único factor. Las microarquitecturas de CPU modernas (como Intel de 11ª generación o AMD Zen 4 en adelante) incluyen conjuntos de instrucciones avanzadas como AVX512. Estas instrucciones aceleran masivamente las operaciones de multiplicación de matrices, que son el núcleo computacional de los LLMs. Por lo tanto, una CPU moderna con 6 núcleos y soporte AVX512 puede superar en rendimiento a una CPU más antigua de 10 núcleos que carece de estas instrucciones, creando una brecha de rendimiento oculta que es vital considerar para los usuarios que dependen de la CPU.8  
* **RAM:** La cantidad de RAM del sistema es un factor limitante directo. Se utiliza para cargar el sistema operativo, las aplicaciones y, fundamentalmente, el propio modelo de lenguaje si este no cabe por completo en la memoria de la GPU. La correspondencia es:  
  * **8 GB:** Mínimo absoluto, adecuado solo para los modelos más pequeños (parámetros de \~3B).
  * **16 GB:** Recomendado para una experiencia fluida con modelos populares de 7B.
  * **32 GB:** Necesario para ejecutar modelos de 13B y algunos de 30B de manera cómoda.
  * **64 GB o más:** Requerido para modelos grandes de 33B o más, o para ejecutar múltiples modelos simultáneamente.
* **Almacenamiento:** Se necesita un mínimo de 12 GB de espacio libre en disco para la instalación base de ollama y los modelos iniciales. No obstante, esta cifra es engañosa. Los archivos de los modelos son grandes, variando desde 2 GB para un modelo pequeño hasta más de 200 GB para los más grandes. Por ello, se recomienda disponer de al menos 50 GB de espacio libre en una unidad de estado sólido (SSD) o NVMe. La velocidad del disco también es importante, ya que afecta al tiempo de carga inicial de los modelos.

### **GPU: La VRAM determina el rendimiento**

Para un rendimiento serio, una tarjeta gráfica (GPU) dedicada es esencial. La naturaleza paralela de los cálculos de los LLMs se adapta perfectamente a la arquitectura de las GPUs, que contienen miles de núcleos diseñados para este tipo de tareas.  

El factor más crítico de una GPU para ollama es su **VRAM (Video RAM)**. Esta es la memoria dedicada de la GPU, y la cantidad disponible determina directamente el tamaño máximo del modelo que se puede cargar y ejecutar. Si un modelo no cabe por completo en la VRAM, ollama puede dividirlo, cargando una parte en la VRAM y el resto en la RAM del sistema (un proceso conocido como *offloading*). Sin embargo, esto introduce una latencia significativa, ya que la comunicación entre la RAM y la VRAM es mucho más lenta que la operación dentro de la VRAM, degradando drásticamente el rendimiento.  

La estrategia de hardware para ollama es, por tanto, un equilibrio entre el tamaño del modelo deseado, la VRAM disponible y el uso de técnicas como la cuantización.

La cuantización reduce la precisión de los pesos del modelo (por ejemplo, de 16 bits a 4 bits), disminuyendo su tamaño en VRAM con una pérdida de calidad a menudo imperceptible para muchas tareas. Esto permite a los usuarios con hardware modesto ejecutar modelos mucho más grandes de lo que sería posible de otro modo.

| Característica | Nivel Básico / Entusiasta | Nivel Desarrollador / Profesional | Nivel Investigador / Avanzado |
| :---- | :---- | :---- | :---- |
| **CPU** | Intel Core i5 / AMD Ryzen 5 (6+ núcleos) | Intel Core i7 / AMD Ryzen 7 (8+ núcleos, con AVX512) | Intel Core i9 / AMD Ryzen 9 (12+ núcleos, con AVX512) |
| **RAM** | 16 GB DDR4 | 32 GB DDR5 | 64 GB+ DDR5 |
| **Almacenamiento** | 512 GB SSD | 1 TB NVMe SSD | 2 TB+ NVMe SSD |
| **GPU (VRAM)** | 8 GB (ej. NVIDIA RTX 3060 Ti) | 16-24 GB (ej. NVIDIA RTX 3080 / 4090\) | 24-48 GB (ej. NVIDIA RTX 4090 / A6000) |

| Tamaño del Modelo (Parámetros) | VRAM Requerida (FP16 \- Precisión Completa) | VRAM Requerida (Q4 \- Cuantizado 4-bit) |
| :---- | :---- | :---- |
| **3B** | \~6 GB | \~2 GB |
| **7B** | \~14 GB | \~4 GB |
| **13B** | \~26 GB | \~8 GB |
| **30B-34B** | \~60-68 GB | \~16-20 GB |
| **70B** | \~140 GB | \~40 GB |

## **Requisitos de Software**

Ollama es compatible con los principales sistemas operativos de escritorio. Los requisitos específicos son:

* **Windows:** Windows 10 o posterior.  
* **macOS:** macOS 11 Big Sur o posterior.8 La aplicación de escritorio oficial requiere macOS 14 Sonoma o posterior.
* **Linux:** Cualquier distribución moderna, con soporte oficial para Ubuntu 18.04 y posteriores.

## **Instalación de ollama**

El proceso de instalación de ollama está diseñado para ser lo más sencillo posible, aunque varía ligeramente entre sistemas operativos: [Instalación de ollama](https://ollama.readthedocs.io/en/quickstart/)

## **Comandos esenciales**

Una vez que ollama está instalado, la interfaz de línea de comandos (CLI) se convierte en la herramienta principal para interactuar con el sistema. La CLI está diseñada para ser intuitiva, y para aquellos familiarizados con Docker, muchos de los comandos resultarán instantáneamente reconocibles, ya que ollama adopta un paradigma similar para la gestión de modelos como Docker lo hace para las imágenes de contenedores.

### **Verificación de la Instalación y Primer Contacto**

El primer paso después de la instalación es verificar que todo funciona correctamente. Abra una nueva ventana de terminal y ejecute:

`ollama --version`

Este comando debería devolver la versión instalada de ollama.

### **Descarga de modelos**

Hay dos comandos para obtener modelos de la biblioteca de ollama:

* `ollama pull <nombre_del_modelo>`: Este comando descarga un modelo a la máquina local sin iniciar una sesión interactiva. Es útil para pre-cargar modelos que se planean usar más tarde.
* `ollama run <nombre_del_modelo>`: Este es el comando más común. Comprueba si el modelo especificado existe localmente. Si no, lo descarga primero (pull) y luego inicia una sesión de chat interactiva en la terminal, permitiéndo conversar con el modelo inmediatamente.

Ejemplo:  
`ollama run llama3.2`

Una vez dentro de la sesión de chat, puede escribir sus preguntas. Para introducir un texto de varias líneas, puede encerrarlo entre comillas triples (""").

### **Gestión de la librería local**

A medida que se descargan más modelos, son necesarias herramientas para gestionarlos:

* `ollama ls`: Muestra los modelos descargados.
* `ollama rm <nombre_del_modelo>`: Elimina un modelo del sistema, liberando el espacio en disco que ocupaba.  

### **Monitorización de modelos activos**

Para entender lo que sucede en segundo plano, ollama proporciona comandos de inspección:

* `ollama show <nombre_del_modelo>`: Proporciona información detallada sobre un modelo específico
* `ollama ps`: Similar a docker ps, muestra los modelos que están actualmente cargados en la memoria.

## **Consideraciones de seguridad para exponer el servidor ollama**

Por defecto, el servidor de ollama solo escucha en la interfaz loopback (127.0.0.1), lo que significa que solo las aplicaciones que se ejecutan en la misma máquina pueden acceder a él.

Es posible configurarlo para permitir el acceso desde otros dispositivos en su red local, pero teniendo en cuenta que la API de ollama no tiene un mecanismo de autenticación incorporado. Cualquier dispositivo en la misma red podrá interactuar con sus modelos. Por lo tanto, se recomienda exponer el servidor solo en redes en las que se confíe plenamente.

## **Mantener ollama actualizado**

Ollama es un proyecto de código abierto en activo. Mantener tanto la herramienta como los modelos actualizados es crucial para beneficiarse de las últimas mejoras de rendimiento, correcciones de errores y nuevas características.

* **Actualizar la aplicación:** Reinstalar ollama.
* **Actualizar los modelos:** Para actualizar un modelo descargado, ejecutar: `ollama pull <nombre_del_modelo>`