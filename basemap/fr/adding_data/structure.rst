.. module:: geoserver.structure
   :synopsis: Apprenez la structure du répertoire de données de GeoServer.

.. _geoserver.structure:

Structure du répertoire de données de GeoServer
===============================================

Ce qui suit est la structure de GEOSERVER_DATA_DIR::

	data_directory/
   		global.xml
   		logging.xml
   		wms.xml
   		wfs.xml
   		wcs.xml
   		data/
   		demo/
		gwc/   		
   		layergroups/
   		logs/
		palettes/
   		security/
   		styles/
   		user_projections/
   		workspaces/
   		www/

.. list-table::
   :widths: 20 80

   * - **File**
     - **Description**
   * - ``global.xml``
     - Contient des paramètres qui traversent services, y compris les informations de contact, paramètres JAI, jeux de caractères et verbosité.
   * - ``logging.xml``
     - Indique le niveau de logging, la location, et s'il doit se connecter à std.  
   * - ``wcs.xml`` 
     - Contient des métadonnées de service et divers paramètres pour le service WCS.
   * - ``wfs.xml`` 
     - Contient des métadonnées de service et divers paramètres pour le service WFS.
   * - ``wms.xml`` 
     - Contient des métadonnées de service et divers paramètres pour le service WMS.
   * - ``data``
     - A ne pas confondre avec le répertoire de données ``GeoServer data directory``, le répertoire de données est un endroit où les données réelles peuvent être stockés. Ce répertoire est utilisé pour stocker shapefile et raster  mais peut être utilisé pour toutes les données qui est à base fichier. Le principal avantage de stocker des fichiers de données à l'intérieur du répertoire des données est la portabilité.
   * - ``demo``
     - Le répertoire demo contient des fichiers qui définissent les exemples de requêtes disponibles dans le Sample Request Tool.
   * - ``gwc``
     - Ce répertoire contient le cache créé par le service de GeoWebCache intégré.
   * - ``layergroups``
     - Contient des informations sur les configurations des groupes de Layer.
   * - ``logs``
     - Ce répertoire contient les fichiers de log de GeoServer et le fichiers de propriétés de log.
   * - ``palettes``
     - Le répertoire des palettes est utilisé pour stocker pré-calculées Palettes d'image. Palettes d'image sont utilisées par le GeoServer WMS comme moyen de réduire la taille des images produites, tout en maintenant la qualité de l'image.
   * - ``security``
     - Le répertoire de sécurité contient tous les fichiers utilisés pour configurer le sous-système de sécurité de GeoServer. Ce qui inclut un ensemble de fichiers de propriétés qui définissent les rôles d'accès, ainsi que les services et les données chaque rôle est autorisé à accéder.
   * - ``styles``
     - Le répertoire de styles contient un certain nombre de fichiers Styled Layer Descriptor (SLD) qui contiennent des informations style utilisé par le GeoServer WMS. 
   * - ``user_projections``
     - Le répertoire user_projections contient des définitions de système de références spatiales supplémentaires. Les epsg.properties peuvent être utilisés pour ajouter de nouveaux systèmes de références spatiales, tandis que le fichier epsg_override.properties peut être utilisé pour remplacer une définition officielle avec une coutume.
   * - ``workspaces``
     - les différents répertoires workspaces contiennent des métadonnées sur ``stores`` and ``layers`` qui sont publiées par GeoServer. Chaque layer aura un fichier layer.xml qui lui est associée, ainsi que ce soit un coverage.xml ou un fichier featuretype.xml selon qu'il s'agisse d'un raster ou vecteur.
   * - ``www``
     - Le répertoire www est utilisé pour permettre GeoServer d'agir comme un serveur web régulièrement et servir des fichiers réguliers. 

