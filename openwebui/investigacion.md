# **Investigación en profundidad usando openwebui**

## **Requisitos**

Para implementar una *especie* de investigación profunda en openwebUI debemos combinar una serie de elementos:
- **Búsqueda web:** es una opción incluida en openwebui a través de buscadores externos. En nuestro caso usamos un servidor [SearXNG](https://docs.searxng.org/).
- **Ingeniería de prompts:** se puede crear un indicador (prompt) personalizado para describir la estructura del informe final.
- **Búsquedas reiteradas:** a través de una herramienta (tool) publicada en la librería de openwebui.

## **Puesta en marcha**

Para poner en marcha la infrestructura necesaria usaremos la herramienta docker compose.

En la carpeta [rag_local](/rag_local) de este repositorio se encuentran los archivos necesarios para hacerlo.

Basta con clonar el repositorio y acceder a la carpeta rag_local, para posteriormente ejecutar:  
    ``docker compose up -d``

## **Herramientas**

Permiten personalizar el comportamiento de openwebui.

Las **herramientas** son scripts en lenguaje python que complementan las consultas en el chat. 

Se instalan desde espacio de trabajo, usando una página externa para la búsqueda:

<div align="center"><img src="/openwebui/capturas/descubrir.png"></div>

<div align="center"><img src="/openwebui/capturas/busqueda.png"></div>

<div align="center"><img src="/openwebui/capturas/herramienta_research.png"></div>

<div align="center"><img src="/openwebui/capturas/importar.png"></div>

<div align="center"><img src="/openwebui/capturas/guardar_herramienta.png"></div>

Una vez instaladas, se activan en la pantalla principal de chat.

<div align="center"><img src="/openwebui/capturas/herramientas1.png"></div>

<div align="center"><img src="/openwebui/capturas/herramientas2.png"></div>

## **Indicadores**

Los **indicadores** (prompts) son texto predetermionado para las consultas. 

Se crean en el espacio de trabajo:

<div align="center"><img src="/openwebui/capturas/nuevo_indicador.png"></div>

Se puede acceder a ellos en la pantalla principal de chat usando el comando asignado en su creación:

<div align="center"><img src="/openwebui/capturas/comando_indicador.png"></div>

<div align="center"><img src="/openwebui/capturas/indicador_var.png"></div>



