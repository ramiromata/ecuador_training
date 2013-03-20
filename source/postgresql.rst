.. |PG|  replace:: *PostgreSQL*

PostgreSQL
============

Nota Legal
-----------

Nota: La presente documentación ha sido extraída literalmente de la documentación para el proyecto de
portal de turismo y modernización del geoportal CREDIA (Honduras) realizado por geomati.co (`http://geomati.co
<http://geomati.co/>`_).

A su vez, parte de los contenidos de este punto son traducciones de la documentación
oficial de PostgreSQL.

PostgreSQL is Copyright © 1996-2006 by the PostgreSQL Global Development Group and is distributed under the terms of the license of the University of California below.

Postgres95 is Copyright © 1994-5 by the Regents of the University of California.

Permission to use, copy, modify, and distribute this software and its documentation for any purpose, without fee, and without a written agreement
is hereby granted, provided that the above copyright notice and this paragraph and the following two paragraphs appear in all copies.

IN NO EVENT SHALL THE UNIVERSITY OF CALIFORNIA BE LIABLE TO ANY PARTY FOR DIRECT, INDIRECT, SPECIAL, INCI-
DENTAL, OR CONSEQUENTIAL DAMAGES, INCLUDING LOST PROFITS, ARISING OUT OF THE USE OF THIS SOFTWARE AND ITS
DOCUMENTATION, EVEN IF THE UNIVERSITY OF CALIFORNIA HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

THE UNIVERSITY OF CALIFORNIA SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IM-
PLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE SOFTWARE PROVIDED HERE-
UNDER IS ON AN “AS-IS” BASIS, AND THE UNIVERSITY OF CALIFORNIA HAS NO OBLIGATIONS TO PROVIDE MAINTENANCE,
SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS.

Introducción
-------------

El objetivo de este tutorial sobre |PG| es que el usuario sea capaz
de crear y eliminar bases de datos y acceder a ellas para la manipulación de los
datos.

Por esto, los puntos siguientes están pensados para dar una introducción
simple a |PG|, a los conceptos básicos sobre bases de datos relacionales
y al lenguaje SQL. No se requiere experiencia en
sistemas UNIX ni en programación. 

Tras el tutorial, es posible continuar el aprendizaje leyendo la
documentación oficial del proyecto, en inglés, en la que se puede encontrar
abundante información sobre el lenguaje SQL, el desarrollo de
aplicaciones para PostgreSQL y la configuración y administración de servidores.

Arquitectura cliente/servidor
-------------------------------

Al igual que el resto de componentes instalados, |PG| utiliza un modelo
cliente/servidor, ya explicado en la introducción.

Las aplicaciones cliente pueden ser de naturaleza muy diversa: una herramienta 
orientada a texto (psql), una aplicación gráfica (pgAdmin3), un servidor web que
accede a la base de datos para mostrar las páginas web, o una herramienta de
mantenimiento de bases de datos especializadas. Algunas aplicaciones de cliente
se suministran con la distribución |PG| mientras que otras son desarrolladas por los usuarios. 

Creación de una base de datos
--------------------------------

El primer paso para trabajar con |PG| es crear una base de datos. Para ello es necesario ejecutar 
como usuario *postgres* el comando *createdb*::

	$ sudo su postgres
	$ createdb mibd

Si no se tiene acceso físico al servidor o se prefiere acceder de forma remota
es necesario utilizar un cliente SSH. La siguiente instrución::

	$ ssh geo@190.109.197.226

conecta al servidor 190.109.197.226 con el usuario *geo*.

Generalmente el mejor modo de mantener la información en la base de datos es utilizando
un usuario distinto a *postgres*, que sólo debería usarse para tareas administrativas. Es
posible incluso crear más de un usuario con diferentes derechos (SELECT, INSERT, UPDATE,
DELETE) para tener un entorno más seguro. Sin embargo, esto queda fuera del ámbito
de este tutorial y se conectará siempre con el usuario *postgres*.

Acceso a una base de datos
-----------------------------

Una vez la base de datos ha sido creada es posible utilizar un cliente para conectar
a ella. Para ello podemos instalar el programa pgAdmin y arrancarlo. pgAdmin servirá como proceso cliente para
conectar al servidor. 

Para ello se deberá seleccionar el menu File > Add Server y registrar el nuevo servidor
con su dirección IP y el puerto en el que está escuchando (5432 por defecto). También
habrá que indicar el nombre de usuario con el que se desea hacer la conexión. 

Una vez se tiene configurada una entrada para la base de datos en pgAdmin, es posible 
conectar a dicho servidor haciendo doble click en dicha entrada. 

.. image :: _static/training_postgresql_connect_pgadmin.png

Una vez creada, es posible selecionar la nueva base de datos y mostrar el árbol de
objetos que contiene. Se puede ver el esquema "public" que no contiene ningún elemento.

Una vez que haya creado una base de datos, se puede acceder a él a través de:

- psql: el programa de terminal interactivo de PostgreSQL que permite introducir
  de forma interactiva, editar y ejecutar comandos SQL. Veremos más adelante qué
  es SQL. Es el que utilizaremos.

- una herramienta existente con interfaz gráfica, como pgAdmin. 

- una aplicación personalizada desarrollada con algún lenguaje para el que haya un 
  driver de acceso. Esta posibilidad no se trata en esta formación. 

Para seguir interactuando con la base de datos abriremos una ventana SQL clicando sobre
el siguiente icono:

.. image :: _static/training_postgresql_open_sql.png

Que abrirá una ventana que permite enviar comandos SQL al servidor de base de datos. Probemos
con los siguientes comandos::

	SELECT version ();
	SELECT current_date;
	SELECT 2 + 2;

psql
-----

También podemos conectar a la base de datos con psql. Podemos conectar con psql desde cualquier
máquina que tenga el programa instalado. El propio servidor tiene dicho programa instalado por lo
que para usarlo tendremos que acceder al servidor::
	
	$ ssh geo@190.109.197.226

Una vez en el servidor hay que tomar la identidad del usuario *postgres*, que se utiliza
para cualquier tarea administrativa de la base de datos::

	$ sudo su postgres
	
Una vez seamos *postgres* hay que conectar a la base de datos. Para ello podemos 
usar la opción -d para entrar a *psql* ya conectados a la base de datos especificada::

	$ psql -d mibd
	
o conectar sin especificar la base de datos y usar el comando \\c dentro de *psql*::

	$ psql
	=# \c mibd
	You are now connected to database "mibd" as user "postgres".
	
Para obtener el listado de las bases de datos existentes en el sistema, usar el comando
\\l::

	=# \l
	
Y para listar tablas del esquema por defecto de la base de datos actual (*public*)::

	=# \dt

Si queremos listar las tablas que hay en otro esquema es posible utilizar la siguiente sintaxis::
  
	=# \dt gis.*  

Por último, para obtener información sobre cualquier objeto de la base de datos es posible
utilizar el comando \\d::

	=# \d gis.categorias
	
Se puede añadir un + para obtener información más detallada::

	=# \d+ gis.categorias
	 
Hay que notar la diferencia entre el símbolo de sistema, representado por el carácter
del dólar, y el símbolo de psql, representado por los carácteres =#. Esta nomenclatura
se sigue en toda la documentación.

Ayuda de psql
..............
	
Para una completa referencia de los comandos disponibles es posible usar el comando \\?::

	=# \?

que nos abrirá la ayuda. El formato de la ayuda es el mismo que el del comando *less*.

SQL en psql
............	

Hay que resaltar que además de los comandos, que comienzan por barra invertida (\\) es
posible introducir directamente sentencias SQL::

	=# SELECT version ();
	                                                  version                                                   
	------------------------------------------------------------------------------------------------------------
	 PostgreSQL 9.1.5 on x86_64-unknown-linux-gnu, compiled by gcc (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3, 64-bit
	(1 row)
	
	=# SELECT current_date;
	    date    
	------------
	 2012-09-11
	(1 row)
	
	=# SELECT 2 + 2;
	 ?column? 
	----------
	        4
	(1 row)
	
	=# 

Los comandos SQL se pueden introducir en varias líneas y *psql* sólo los da por finalizados cuando
el usuario introduce el carácter de finalización de la instrución: el punto y coma::

	=# SELECT 
	-# current_date;
	    date    
	------------
	 2012-09-11
	(1 row)

Así, si nos hemos olvidado teclear el punto y coma, no es necesario teclear de nuevo la instrucción.
Basta con añadir dicho carácter::

	=# select * from gis.categorias
	-# ;
	 id |         descripcion         |    abreviatura     | orden 
	----+-----------------------------+--------------------+-------
	  1 | Alojamiento                 | to_sleep           |     3
	  2 | Alimentación                | where_to_eat       |     2
	  3 | Esparcimiento               | for_fun            |     4
	  4 | Otros Servicios turísticos  | organize_your_trip |     5
	  6 | Qué quieres hacer           | what_do_you_do     |     1
	  9 | Acontecimientos programados | what_happening     |     6
	(6 rows)
	
Más información
----------------

La página web de |PG| se puede consultar aquí [1]_. En ella hay abundante información en inglés [2]_,
así como listas de correo en español [3]_.

Referencias
------------

.. [1] http://www.postgresql.org
.. [2] http://www.postgresql.org/docs/9.2/static/index.html
.. [3] http://archives.postgresql.org/pgsql-es-ayuda/