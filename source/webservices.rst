Servicios web
===============

Existen múltiples definiciones del concepto de servicio web, ya que es un concepto muy general. En el caso que nos ocupa nos
referiremos a servicios disponibles en la red que pueden ser accedido usando el protocolo HTTP, especificando unos parámetros
y obteniendo una salida como resultado. Dichos servicios son componentes expuestos que realizan una tarea muy concreta y que
pueden ser combinados para construir servicios más complejos. 

Al contrario que las aplicaciones monolíticas en las que los componentes están fuertemente acoplados, los sistemas basados en
servicios web fomentan la independencia de los distintos elementos que forman la aplicación. Así, los componentes son
servicios que exponen una API al resto para la colaboración en el contexto de la aplicación y que pueden ser intercambiados
fácilmente por otros servicios que ofrezcan la misma API.

HTTP
-----

Los servicios que se van a consumir se construyen sobre el protocolo HyperText Transfer Protocol (HTTP), por lo que se van a ilustrar algunos conceptos del
mismo.

Las interacciones HTTP se dan cuando un equipo cliente envía peticiones a un servidor. Estas peticiones incluyen un encabezado con
información descriptiva sobre la petición y un cuerpo de mensaje opcional. Entre los datos del encabezado se encuentra el método
requerido: GET, PUT, PUSH, etc. Está fuera del ámbito de esta documentación explicar las semánticas de los distintos métodos. Baste
el ejemplo de la barra de direcciones de los navegadores web, que realiza peticiones HTTP GET para descargarse el recurso especificado
en la dirección.

Siguiendo con el mismo ejemplo, mediante la barra de direcciones podemos descargar distintos tipos de contenidos: páginas HTML, ficheros
de texto plano, ficheros XML, imágenes, vídeos, etc. Algunos de estos contenidos son directamente interpretados por el navegador mientras que
para otros se ofrece la descarga del recurso. ¿En qué se basa el navegador para tomar estas decisiones?

Cada respuesta desde el servidor tiene también una cabecera en la que especifica el *Content-Type* de los datos que vienen en el cuerpo del
mensaje de respuesta. El *Content-Type* puede ser por ejemplo:

* text/html

* text/plain

* text/xml

* image/gif

* video/mpeg

* application/zip

Por último, la respuesta incorpora un código con información adicional sobre lo que sucedió en el servidor al recibir la petición. El código más habitual
usando el navegador es el 200, que informa que el contenido de la respuesta es aquello que se pidió. Otro código también muy frecuente es el 404, que
indica que el recurso que se ha pedido no existe en el servidor.

Servicios del Open Geospatial Consortium (OGC)
-----------------------------------------------

El Open Geospatial Consortium es un consorcio industrial internacional que reúne centenares de compañias, agencias gubernamentales y universidades para
la participación en la creación de estándares disponibles de forma pública.

En el contexto de los servicios web el OGC define los OGC Web Services (OWS), que son estándares construidos sobre el protocolo HTTP y que definen
el contenido de las distintas peticiones y sus respuestas, así como la relación entre ellas. Los OWS proporcionan una infraestructura interoperable, independiente
de la implementación y de los proveedores para la creación de aplicaciones basadas en web y relacionadas con la información geográfica.

Entre las ventajas del uso de estándares podemos destacar:

* Es posible consumir servicios de otros proveedores, independientemente de la implementación concreta.

* Existen desarrollos OpenSource que implementan estos estándares y permiten por tanto la publicación con interfaces estándar de forma sencilla. 

* Multitud de herramientas OpenSource que permiten trabajar con los datos consumidos a través de estas interfaces.

Dos de los OWS más representativos son Web Map Service (WMS) y Web Feature Service (WFS), que vemos con algo más de detalle a continuación. 

El estándar WMS define básicamente tres tipos de peticiones: *GetCapabilities*, *GetMap* y *GetFeatureInfo*. La primera de ellas es común para todos
los OWS y devuelve un documento XML con información sobre las capas existentes en el servidor, los sistemas de coordenadas (CRS, Coordinate Reference System)
soportados, etc.

Ejemplo::

	http://www.cartociudad.es/wms/CARTOCIUDAD/CARTOCIUDAD?REQUEST=GetCapabilities

La petición GetMap obtiene imágenes con representaciones gráficas de la información geográfica existente en el servidor::

	http://www.cartociudad.es/wms/CARTOCIUDAD/CARTOCIUDAD?REQUEST=GetMap&SERVICE=WMS&VERSION=1.3.0&LAYERS=DivisionTerritorial&
		CRS=EPSG:25830&BBOX=718563.200906236,4363954.866694199,735300.5071689372,4377201.249079251&WIDTH=701&
		HEIGHT=555&FORMAT=image/png&STYLES=municipio&TRANSPARENT=TRUE
	
Y por último, la petición GetFeatureInfo es capaz de proporcionar información sobre un punto::

	http://www.cartociudad.es/wms/CARTOCIUDAD/CARTOCIUDAD?REQUEST=GetFeatureInfo&SERVICE=WMS&QUERY_LAYERS=CodigoPostal&
		VERSION=1.3.0&INFO_FORMAT=application/vnd.ogc.gml&LAYERS=CodigoPostal&CRS=EPSG:25830&
		BBOX=665656.9561496238,4410190.54853407,690496.231896245,4427113.624503085&WIDTH=706&HEIGHT=481&
		FORMAT=image/png&STYLES=codigo_postal&TRANSPARENT=TRUE&I=475&J=204&FEATURE_COUNT=10000&EXCEPTIONS=XML

Por su parte el estándar WFS no trabaja con representaciones de los datos sino que lo hace con los datos mismos. Para ello define las siguientes llamadas::

* GetCapabilities: Al igual que todos los OWS, WFS admite la llamada GetCapabilities para obtener una lista de las capas y posibilidades que ofrece
  el servicio.

* DescribeFeatureType: Permite obtener un documento con la estructura de los datos.

* GetFeature: Permite realizar una consulta al sistema y obtener las entidades que cumplen los criterios de búsqueda.

Así, podemos ver qué capas hay en un servicio WFS::

	http://www.cartociudad.es/wfs-comunidad/services?request=GetCapabilities&service=WFS
	
Consultar la estructura de una de ellas::

	http://www.cartociudad.es/wfs-comunidad/services?request=DescribeFeatureType&service=WFS&VERSION=1.0.0&
		TypeName=app:entidadLocal_&outputformat=text/xml;%20subtype=gml/3.1.1

Y efectivamente descargar algunas de ellas::

	http://www.cartociudad.es/wfs-comunidad/services?REQUEST=GetFeature&SERVICE=WFS&TYPENAME=app:entidadLocal_&
		NAMESPACE=xmlns%28app=http://www.deegree.org/app%29&VERSION=1.1.0&EXCEPTIONS=XML&MAXFEATURES=10
