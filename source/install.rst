Installation
=============

Install the 10.18.2 version of chef-solo
-------------------------------------------

Ejecutar los siguientes comandos. Las líneas que comienzan por almuadilla son comentarios, las que
comienzan por $ se deben de poner en la línea de comandos de línux. El $ simboliza la línea de comandos
por lo que no hay que incluirlo::

	# Añadir "deb http://apt.opscode.com/ precise-0.10 main" 
	# (sin las comillas) al final del fichero /etc/apt/sources.list
	$ sudo gedit /etc/apt/sources.list
	$ gpg --keyserver keys.gnupg.net --recv-keys 83EF826A
	$ gpg --export packages@opscode.com|sudo tee /etc/apt/trusted.gpg.d/opscode-keyring.gpg > /dev/null
	$ sudo apt-get update
	$ sudo apt-get install opscode-keyring
	
	# Durante el siguiente paso se pide un dato que no es necesario, se puede dejar en blanco
	
	$ sudo apt-get install chef
	$ chef-solo -version

Run chef
---------

Ejecutar los siguientes comandos::

	$ sudo mkdir /var/chef-solo
	
Copiar los cookbooks y los ficheros solo.rb y dna.json al directorio recién creado. El resultado final debe ser este::

	/
	\- var
	    \- chef-solo
	        |- dna.json
	        |- solo.rb
	        \- cookbooks
	            |- apache2
	            |- apt
	            |- ark
	            |- build-essential
	            |- database
	            |- gis
	            |- java
	            |- logrotate
	            |- omnibus_updater
	            |- openssl
	            |- postgresql
	            |- tomcat
	            \- unredd-nfms-portal

Ejecutar desde el directorio /var/chef-solo::

	sudo chef-solo -c solo.rb -j dna.json

