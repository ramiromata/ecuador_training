Formación Ecuador
=================

Introducción a Linux
----------------------

Se parte de una máquina virtual con el sistema operativo recién instalado.

Se comenta la estructura de ficheros, navegación y algunos comandos, conexiones entre comandos.

Instalación del sistema
-------------------------

Actividad guiada: Instalación de chef-solo y ejecución de los cookbooks.

Introducción a los servicios web
----------------------------------

Entender el protocolo HTTP sobre el que se construyen los servicios OGC

Servicios web del Open Geospatial Consortium (OGC)
...................................................

Tener una visión general del OGC.

Beneficios de los estándares: interoperabilidad.

Estándares WMS y WFS. Explicar también WMTS por encima.

Descarga de la especificación.

Arquitectura del sistema
--------------------------

Mientras el cookbook chef se ejecuta se pueden ir comentando cosas: 

El sistema está sufriendo un proceso de simplificación para facilitar la adopción.
En dicho proceso se pretende cambiar la arquitectura por otra más sencilla pero
algo menos potente. Aunque en esta primera fase vamos a instalar el sistema no simplificado,
nos vamos a centrar en un subconjunto del mismo que permite realizar la mayor parte de los
casos de uso que ofrece el portal, dejando para una siguiente misión el resto de los casos de
uso y los componentes del sistema necesarios para la realización de los cálculos.
 
Lo importante en este punto es que conozcan los roles que son necesarios para que el portal funcione:

- Aplicación cliente servidora

- Servicios estándar: publicación de datos vía interfaces estándar

- Servicios específicos

- Aplicación javascript que consume estos recursos

Mostrar un diagrama de arquitectura global (architecture0)

Mostrar el uso de los servicios sobre HTTP para ilustrar el concepto de la arquitectura, de las llamadas desde el cliente.

Servicios web Open Geospatial

Introducción a GeoServer
-------------------------

Enlace: ¿Cuales son las ventajas de usar servicios estándar? Entre otras, que existen implementaciones ya listas, en software
libre, que se comportan de una manera que conocemos y que se adapta perfectamente a nuestras necesidades. Una de estas
implementaciones es GeoServer, que vamos a estudiar con detalle.

¿Qué es geoserver?

	Explicación (implementación de estándares)

	Así pues, va a ser parte determinante de nuestra arquitectura (architecture1). Revisar la arquitectura y mostrar los estándares que implementa.

Presentar los objetivos de la siguiente parte del curso

	Ser capaz de publicar capas vectoriales y raster en geoserver

	Consumirlas a través de algún cliente estándar. Destacar la importancia de publicar los datos. Es algo menos visible que el portal pero de igual importancia.
	
	Personalización del portal

	Consumición de nuestras capas a través del portal
	
	Visualización de rasters temporales.

Estado del servidor

Logs de GeoServer
	
	Ejercicio: Visualización de logs con la consola de linux (por qué? porque se puede documentar! Porque las herramientas de consola son muy potentes: tail)

Información de contacto (Recordar una llamada a GetCapabilities)

Acerca de GeoServer

GeoServer: Publicación de datos vectoriales
---------------------------------------------

Espacios de trabajo

Almacenes de datos

Publicación de capas vectoriales (en shape y en postgis)

Previsualización de capas

Visualización desde QGIS

Simbolización de capas vectoriales -> Edición del documento XML (Mención al estándar con posible descarga. Ejemplo de simbolización de carreteras)

Personalización del portal
----------------------------

Volvemos a mostrar el diagrama de arquitectura. Explicamos el valor que hemos creado publicando nuestros datos mediante interfaces estándar. A continuación explicar el valor que aporta el portal y explicar qué partes podemos personalizar.

Mostrar el directorio de configuración

Enumerar los ficheros que son relevantes describiendo la información que contienen.

Adaptación del aspecto gráfico.

Soporte multiidioma.

	Explicar el tema de la codificación de los ficheros de propiedades

Configuración de una nueva capa vectorial

Configuración de un nuevo grupo de capas

prerequisites
.............

* css, json syntax, usage of the wms protocol

GeoServer: Publicación de datos raster
----------------------------------------

Almacén de datos GeoTIFF

Publicación de una capa GeoTIFF

Simbolización raster

Optimización de GeoTIFF para su publicación

(Ejemplo de publicación)

Publicación de un mosaico raster temporal
	Configuración de los ficheros de propiedades
	Error permisos. Go through it -> Añadir un mosaico en un directorio para el que tomcat6 no tenga permisos de escritura
	Ver la dimensión temporal en el GetCapabilities
	Consumirlo a través del navegador web
	
(Publicación de un mosaico con varias capas raster)

Personalización del portal (datos raster)
-------------------------------------------

(Configuración de las capas raster añadidas en el punto anterior)

Mostrar el uso de la barra temporal -> Peligro de utilizar GeowebCaché!!!

Mostrar las estadísticas en una demo existente. 

Introducción a PostgreSQL/PostGIS
----------------------------------



Modelo de datos del servicio de estadísticas
---------------------------------------------

¿Cómo configuramos las estadísticas? GeoServer no proporciona esta funcionalidad por lo
que deberemos de montarnos una serie de servicios adicionales a GeoServer. 

Para ello, estos servicios necesitan conocer qué cálculos necesitamos aplicar a qué instantáneas de capa y cómo presentar esto al
usuario. Esta información se va modelizar en una base de datos postgresql llamada geostore.

Así pues vamos a tener las siguientes entidades "capa", "actualización de capa", "definición de estadística", 
"resultados", "definición gráfica", "resultados gráfica".

Mostrar un diagrama entidad relación.

Explicar que se almacena en PostgreSQL, sin entrar en detalles porque Geostore es críptico y no se puede
mostrar el contenido desde la base de datos de una manera inteligible. Sólo que será necesario hacer backups para
guardar toda la información de las estadísticas.

Mostrar la arquitectura con la base de datos PostgreSQL fuera de la nube (ver libreta)

Tras ejecutar el script chef tenemos el sistema preparado para ir añadiendo capas, instantáneas, estadísticas y gráficas. Así pues, 
tenemos que:

* Crear una capa
* Añadir instantáneas a la capa
* Crear estadísticas
* Crear gráficas

Creación de una capa (Define a new layer in GeoBatch administration)
---------------------------------------------------------------------

Inclusión de nuevas instantáneas de capa
------------------------------------------

Definición de la capa (define a new layer)
	
Ingestión de una instantánea de la capa (proceso automático -> el fichero debe contener toda la información necesaria. Sin 
enseñar GeoBatch, mostrando los logs en su lugar)

	Formato del fichero ZIP de ingestión (con preguntas: cómo sabemos a qué capa corresponde este fichero? cómo sabemos a qué año?)

Creación de estadísticas
---------------------------

Explicar el proceso de cálculo de estadísticas

Proporcionar un XML base

 y los cambios que tienen que hacer a un 








