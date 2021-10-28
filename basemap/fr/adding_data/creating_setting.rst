.. module:: geoserver.creating_setting
   :synopsis: Apprenez à créer et à établir un nouveau répertoire de données de GeoServer.

.. _geoserver.creating_setting:


Créer et configurer un nouveau répertoire de données GeoServer
--------------------------------------------------------------


#. En général, si GeoServer fonctionne en mode Archive Web à l'intérieur d'un conteneur de servlet, comme dans ce workshop, le répertoire de données se trouve à ``<web application root>/data`` (le répertoire de données contient les données de configuration de GeoServer).

#. La première chose à faire est de configurer correctement le GEOSERVER_DATA_DIR. Pour augmenter la portabilité de leurs données et de faciliter les mises à jour GeoServer, dans la configuration par défaut du Workshop est configuré dans le répertoire: ``GEOSERVER_DATA_DIR`` 
	
	
	${TRAINING_ROOT}/geoserver_data
	
	
   En général, ce n'est pas un problème, mais si vous exécutez le système à partir du LiveDVD ce dossier réside dans la mémoire. La première chose à faire est de faire avancer ce dossier dans un stockage persistant locale.
	
	
	* Déplacez le ``GEOSERVER_DATA_DIR`` dans quelque part dans le stockage persistant en utilisant la commande::
	
		sudo mv -f ${TRAINING_ROOT}/geoserver_data <TARGET_DIR>
	
	
	* Faire un lien symbolique vers le ``GEOSERVER_DATA_DIR`` en exécutant la commande::
	
		ln -s <TARGET_DIR> ${TRAINING_ROOT}/geoserver_data
	
	
   .. warning:: CVérifiez que l'utilisateur  ``geosolutions`` dispose des autorisations de lecture / écriture de tous les fichiers / dossiers à l'intérieur du ``GEOSERVER_DATA_DIR``.

   .. note:: Au lieu de créer un lien symbolique, vous pouvez configurer GeoServer afin de lui permettre de pointer vers le nouveau ``GEOSERVER_DATA_DIR``. Pour ce faire, éditez le fichier :file:`/opt/tomcat-geoserver/webapps/geoserver/WEB-INF/web.xml` et modifier le paramètre de contexte ``GEOSERVER_DATA_DIR``.

