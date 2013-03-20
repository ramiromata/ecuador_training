Linux
======

Nota: La presente documentación ha sido extraída literalmente de la documentación para el proyecto de
portal de turismo y modernización del geoportal CREDIA (Honduras) realizado por geomati.co (`http://geomati.co
<http://geomati.co/>`_) 

En el símbolo de sistema presentado es posible hacer uso de una serie de comandos que la mayor
parte de sistemas linux tienen. Pero antes de ver los comandos es importante tener claro
cómo se organiza el sistema de ficheros y cómo se referencian estos mediante rutas relativas
y absolutas.

El sistema de ficheros linux se organiza jerárquicamente a partir de un directorio llamado
"raíz" y que se denota por la barra inclinada hacia adelante (/). En linux los ficheros
se referencian mediante rutas. Estas rutas pueden ser relativas o absolutas.
Las absolutas comienzan por /, mientras que las relativas empiezan por el nombre de un subdirectorio,
por . (directorio actual) o por .. (directorio padre).

Así pues, podemos tener rutas absolutas como::

	/tmp
	/home/geo
	/home/geo/Escritorio
	etc.

Las rutas absolutas se pueden utilizar desde cualquier directorio. Podemos listar los directorios
anteriores con los siguientes comandos, independientemente del directorio en el que se esté::

	$ ls /tmp
	$ ls /home/geo
	$ ls /home/geo/Escritorio
	
Las rutas relativas en cambio, parten del directorio actual. Si por ejemplo estamos en */home/geo*, podemos
listar los directorios anteriores con los siguientes comandos::

	$ ls ../../tmp
	$ ls .
	$ ls Escritorio	

o "navegando" de forma más caprichosa::

	$ ls Escritorio/../../../tmp
	$ ls ./././././././././././../geo
	$ ls ../geo/Escritorio

A continuación mostramos algunos comandos útiles en linux:

- less: Visualiza un fichero de texto. La interacción es la misma que la descrita en el apartado
  "Ayuda de psql" anterior::

	$ less ruta_fichero_a_visualizar
	
  El fichero a visualizar se presenta de una manera muy común en los sistemas
  UNIX y que podemos identificar porque en la esquina inferior izquierda tenemos el
  signo de los dos puntos (:) seguido del cursor.
  
  .. image :: _static/linux/training_postgresql_help.png
  
  Podemos navegar por el contenido pulsando los cursores arriba y abajo, así como las
  teclas de página anterior y posterior.
  
  También es posible hacer búsquedas utilizando el comando /texto. Una vez pulsamos intro,
  se resaltarán las coincidencias encontradas, como se puede ver en la siguiente imagen. Para navegar a la
  siguiente coincidencia es posible pulsar la tecla 'n' y para ir a la anterior Mayúsculas + 'n' 
  
  .. image :: _static/linux/training_postgresql_psql_help_search.png
  	
  Para salir pulsar 'q'.	
  
- nano: Permite editar ficheros. En la parte de abajo se muestran los comandos para salir, guardar
  el fichero, etc.::

	$ nano ruta_fichero_a_editar

- locate: Localiza ficheros en el sistema operativo::

	$ locate parte_del_nombre_del_fichero

  Un aspecto a tener en cuenta en el uso de locate es que
  el sistema programa escaneos regulares del disco para construir un índice con los ficheros existentes y 
  permitir así a *locate* responder al usuario sin tener que realizar la búsqueda en el sistema de
  ficheros, que toma mucho tiempo. Es por esto que *locate* funciona muy rápido pero puede que no 
  encuentre los ficheros creados recientemente. Para estos, habrá que esperar a que se produzca un
  escaneo programado.
	
- id: Muestra la identidad actual del usuario::

	$ id

- su: Permite autenticarse con un usuario distinto. El siguiente comando probablemente no funcionará
  porque es necesario tener permisos de superusuario para realizar *su*, ver el siguiente caso::

	$ su postgres 

- sudo: No es un comando en sí, sino que permite ejecutar el comando que le sigue con permisos
  de superusuario. Por ejemplo, para ejecutar el comando anterior con permisos de superusuario::

	$ sudo su postgres

- passwd: Cambia el password de un usuario. Por ejemplo para cambiar el password de root::

	$ sudo passwd root

- ssh: Acceso remoto en línea de comandos. Con SSH es posible entrar a un servidor remoto que tenga
  activado dicho acceso. Para ello es necesario especificar la dirección del servidor::
  
	$ ssh 168.202.48.151
	The authenticity of host '168.202.48.151 (168.202.48.151)' can't be established.
	ECDSA key fingerprint is 9f:7c:a8:9c:8b:66:37:68:8b:7f:95:a4:1b:24:06:39.
	Are you sure you want to continue connecting (yes/no)? yes
	
  En la salida anterior podemos observar como primeramente el sistema pregunta por la autenticidad de
  la máquina a la que queremos conectar. Tras responder afirmativamente el sistema nos comunica que
  el servidor al que vamos a conectarnos se añade a la lista de hosts conocidos, de manera que el
  mensaje anterior no volverá a aparecer la siguiente vez que se intente una conexión. A continuación
  el sistema pregunta el password del usuario "usuario"::
  
	Warning: Permanently added '168.202.48.151' (ECDSA) to the list of known hosts.
	usuario@168.202.48.151's password:
  
  En caso de querer conectar con otro usuario es necesario prefijar el nombre de dicho usuario, seguido
  del carácter "@" antes de la dirección del servidor::
  
	$ ssh otro_usuario@168.202.48.151

- scp: Copia ficheros al servidor::

	$ scp fichero_origen directorio_destino
	
  El directorio puede ser una ruta normal o la cadena de conexión por SSH a un servidor remoto. Veamos
  varios ejemplos. El siguiente copia ficheros locales en el directorio */tmp* de un servidor remoto::
  
  	$ scp mi_fichero_local geo@geoportalcredia.org:/tmp
  	
  El siguiente comando copia el fichero de vuelta::
  
  	$ scp geo@geoportalcredia.org:/tmp/mi_fichero_local .
  	
  Se puede observar que el format de la URL remota es parecido al que se usa para conectar por cliente
  SSH. La única diferencia es que al final, separado por (:), encontramos una ruta en la máquina remota

- zip: Comprime ficheros::

	$ zip -r ruta_fichero.zip lista_de_ficheros_a_comprimir
	
  La opción -r hace que zip incluya los contenidos de los directorios que se encuentre en la 
  lista de ficheros a compartir.
	
- unzip: Descomprime ficheros::

	$ unzip ruta_fichero.zip

- chgrp, chown y chmod