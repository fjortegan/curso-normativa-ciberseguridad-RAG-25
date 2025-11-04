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

## **Herramientas e indicadores en openwebui**

Permiten personalizar el comportamiento de openwebui.

Las **herramientas** son scripts en lenguaje python que complementan las consultas en el chat. Se activan en la pantalla principal de chat.

<div align="center"><img src="/openwebui/capturas/pantalla_principal.png"></div>

Los **indicadores** (prompts) son texto predetermionado para las consultas. Se puede acceder a ellos en el char

## **Usando la investigación en profundidad**



