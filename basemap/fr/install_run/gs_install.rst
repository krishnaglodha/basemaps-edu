.. module:: geoserver.gs_install

.. _geoserver.gs_install:


Installation de GeoServer
--------------------------

GeoServer est emballé comme un servlet autonome pour une utilisation avec des applications de conteneur de servlet existants tels que `Apache Tomcat <http://tomcat.apache.org/>`_.

.. note:: GeoServer a surtout été testé en utilisant Tomcat, et par conséquent, ces instructions peut ne pas fonctionner avec d'autres container d'applications


Installation
^^^^^^^^^^^^^

#. Accédez la page de `téléchargement de GeoServer <http://geoserver.org/display/GEOS/Download>`_ et choisissez le version 2.2.5 from "All Releases" à télécharger.

#. sélectionner :guilabel:`Web archive` sur la page de téléchargement.

#. Téléchargez et décompressez l'archive.  Copiez le fichier :file:`geoserver.war` dans le répertoire qui contient votre container de webapps.

#. votre container application devrait décompresser l'archive web et automatiquement configurer et exécuter GeoServer.

   .. note:: Un redémarrage de votre container application peuvent être nécessaires.


	
exploitation
^^^^^^^^^^^^^

Utilisez la méthode de démarrage et d'arrêt de votre container application webapps pour exécuter GeoServer.

#. Accès Tomcat ouvrir un navigateur et accédez à :file:`http://{host}:{}/geoserver`.  Par exemple, avec Tomcat fonctionnant localement sur ​​le port 8083 comme dans cet workshop, l'URL serait ``http://localhost:8083/geoserver``.


désinstallation
^^^^^^^^^^^^^^^

Dans le cas où vous devez retirer GeoServer (mais ne pas le faire maintenant) vous pouvez utiliser les étapes suivantes:

#. Arrêter le container application.

#. Retirez la webapp de GeoServer à partir du répertoire webapps de application's webapps directory.