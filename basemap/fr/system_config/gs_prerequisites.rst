.. module:: geoserver.gs_prerequisites

.. _geoserver.gs_prerequisites:


Installer les conditions préalables à GeoServer 
------------------------------------------------

Cette séction est dédiée aux conditions préalables pour bien travailler avec GeoServer.

FWTools
^^^^^^^

FWTools est une série de GIS Open Sources binaires. Les kits veulent etre très symples à installer et à utiliser pour l'utilisateur final. ... FWtools incluent OpenEV, GDAL, MapServer, PROJ.4 and OGDI et aussi des éléments de  support. pour plus d'informations consultez la séction `FWTools site <http://fwtools.maptools.org/>`_. 

A fin d'installer FWTools dans votre système Ubuntu il faut suivir les étapes suivantes:

#. Naviguez jusq'au site `FWTools <http://fwtools.maptools.org/>`_ et téléchargez la version courante ``FWTools 2.0.6 (Linux x86 32bit)``.

#. Décompressez le ``tar.gz file`` et faites marcher le install.sh écriture dans le nouveau repertoire, et puis ajoutez le repertoire bin_safe à votre chemin. 


	% tar xzvf FWTools.tar.gz
	% cd FWTools
	% ./install.sh

#. Si vous utilisez Bash comme votre coquille ajoutez le suivant à votre écriture de mise en marche.(ie. ~/.bash_profile):

	PATH=$PATH:$HOME/FWTools/bin_safe

   Ou si vous utilisez csh ou tcsh pour coquille ajoutez les suivants à votre .cshrc:    

	setenv PATH $PATH:$HOME/FWTools/bin_safe

Java
^^^^

Pour faire marcher GeoServer il faut un Java Development Kit (JDK), et non seulement un Java Runtime Environment (JRE). A fin d'installer Java JDK suivez ces étapes:

#. Mettez à jour l'index package:: 


            sudo apt-get update

#. Installez le JDK::

            sudo apt-get install sun-java6-jdk

#. Si vous voulez vous pouvez configurer le ``JAVA_HOME`` environnement variable in ``.baschrc``::

            sudo nano .bashrc    

            Ajoutez les commands suivants à la fin du fichier:
            
            export JAVA_HOME=/usr/lib/jvm/java-6-sun-1.6.0.32
            export PATH=$PATH:/usr/lib/jvm/java-6-sun-1.6.0.32

Apache Tomcat
^^^^^^^^^^^^^

Apache Tomcat est un open source software implementation du ``Java Servlet`` et ``JavaServer Pages`` technologies. Les specifications de Java Servlet et de JavaServer Pages  sont developées sous le `Java Community Process <http://jcp.org/en/introduction/overview>`_  .

#. Naviguez jusq'à l'`Apache Tomcat site <http://tomcat.apache.org/download-60.cgi>`_ et téléchargez ``Core - tar.gz`` distribution binaire .

#. Déplacez le ``tar.gz file`` dans le repertoire ``/opt`` et décompressez-la avec les commandes suivantes:: 

	sudo mv -f apache-tomcat.tar.gz /opt

	sudo tar -xzf apache-tomcat.tar.gz

#. Pour faire marcher Tomcat automatiquement sur la mise en marche du système, créez un file ``tomcat`` dans  le repertoire ``/etc/init.d/`` et tapez le texte suivant::

      #!/bin/bash
      # Startup script for the Tomcat server
      #
      # chkconfig: - 83 53
      # description: Starts and stops the Tomcat daemon.
      # processname: tomcat
      # pidfile: /var/run/tomcat.pid
      # See how we were called.
      case $1 in
      start)
      export CLASSPATH=/opt/apache-tomcat-6.0.30/lib/servlet-api.jar
      export CLASSPATH=/opt/apache-tomcat-6.0.30/lib/jsp-api.jar
      echo "Tomcat is started"
      su - geosolutions -c "/opt/apache-tomcat-6.0.30/bin/startup.sh"
      ;;
      stop)
      su - geosolutions -c "/opt/apache-tomcat-6.0.30/bin/shutdown.sh -force"
      echo "Tomcat is stopped"
      ;;
      restart)
      su - geosolutions -c "/opt/apache-tomcat-6.0.30/bin/shutdown.sh -force"
      echo "Tomcat is stopped"
      su - geosolutions -c "/opt/apache-tomcat-6.0.30/bin/startup.sh"
      echo "Tomcat is started"
      ;;
      *)
      echo "Usage: /etc/init.d/tomcat start|stop|restart"
      ;;
      esac
      exit 0


  .. Attention:: Ce texte ne marche bien q'avec une seule version de Tomcat. Actuellement le groupe de travail contient un Tomcat mais deux différents contextes, l'un pour Geoserver utilisant HTTP port 8083 et l'autre pour GeowebCache utilisant port 8084. Regardez ci dessous pour les détails sur les deux différents contextes. 

#. Donnez-les les justes permissions, et faisez-le executable et enregistré::

	sudo chmod 755 /etc/init.d/tomcat
	sudo update-rc.d tomcat defaults 
	
#. Placez l'environnement pour Tomcat en créant le ``setenv.sh`` file dans le repertoire ``/opt/apache-tomcat-6.0.30/bin`` et tapez le texte suivant:: 

	JAVA_HOME="/usr/lib/jvm/java-6-sun-1.6.0.32"
	JRE_HOME="$JAVA_HOME"

	CATALINA_HOME="/opt/apache-tomcat-6.0.30"
	CATALINA_PID=$CATALINA_HOME/catalina.pid

	JAVA_OPTS="$JAVA_OPTS -server -Xms256m -Xmx256m 
	            -XX:SoftRefLRUPolicyMSPerMB=36000
				-XX:MaxPermSize=128m"

	LD_LIBRARY_PATH="${TRAINING_ROOT}/geoserver_src/nativelibs"
	GDAL_DATA="${TRAINING_ROOT}/geoserver_src/gdal_data"

   .. Attention:: Dans ce groupe d'étude le JAVA_HOME est défini à la mise en marche de Tomcat. Si vous définissez le JAVA_HOME variable dans le .bashrc file vous ne devrez pas le definir ici. 
				
#. Pour faire marcher Apache Tomcat::

	/etc/init.d/tomcat start

#. Pour arreter Apache Tomcat::

	/etc/init.d/tomcat stop

  .. note::
    
	Comme indiqué ci-dessus le workshop OS contient différent scripts pour Tomcat puisque on est en train de faire marcher deux différent contextes sur deux HTTP ports, 8083 and 8084.
	
	Dessous ``/etc/init.d`` il est possible de trouver trois textes:
   
   #. ``/etc/init.d/tomcatRunner`` : Executes a Tomcat startup command and notes the PID; takes 4 parameters: 1. the pidfile - 2. the tomcat logfile - 3. the command to execute - 4. other opts to attach to the command
   #. ``/etc/init.d/geoserver`` : Starts/Stop GeoServer tomcat context on HTTP 8083. **service geoserver {start/stop/restart}**
   #. ``/etc/init.d/geowebcache`` : Starts/Stop GeowebCache tomcat context on HTTP 8084. **service geowebcache {start/stop/restart}**



 
