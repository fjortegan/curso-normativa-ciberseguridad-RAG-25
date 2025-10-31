Para completar el curso es necesario realizar **una** de las siguientes actividades:  

Opción 1. RAG con herramientas propietarias:  
* Seleccionar uno o varios de los documentos incluídos en la normativa de referencia perteneciente al BOE.  
* Convertir el documento pdf a markdown usando marker-pdf.  
* Crear tres cuadernos en NotebookLM, incluyendo respectivamente: el documento en formato pdf, el enlace a la URL del BOE y el documento en formato markdown.
* Realizar la misma consulta en los tres cuadernos comparando los resultados.  
* Usar la búsqueda profunda de perplexity aportando el documento procesado previamente, pero sin usar la web.
* Realizar la misma operación pero dejando libre la búsqueda web. Comparar los resultados.

Opción 2. RAG en local:
* Seleccionar uno o varios de los documentos incluídos en la normativa de referencia perteneciente al BOE.  
* Convertir el documento pdf a markdown usando marker-pdf.
* Instalar el soporte para ejecutar modelos en local: ollama y openwebUI.
* Descargar dos modelos para ollama (deben adaptarse al hardware disponible, para sistemas con hardware limitado se recomiendan: gemma3 y llama3.2).  
* En openwebUI crear dos bases de conocimiento: una con el documento procesado previamente en formato pdf y otra en formato markdown.
* Realizar la misma consulta usando las 4 combinaciones posibles de modelos y bases de conocimiento. Comparar los resultados.
* Habilitar la búsqueda en internet de openwebui y complementar la consulta del apartado anterior que mejores resultados haya proporcionado, con la búsqueda en internet (sería interesante limitar el dominio de búsqueda, por ejemplo: boe.es). 

Para comprobar la realización de las actividades, basta con aportar capturas de pantalla que muestren los resultados de las diferentes consultas.