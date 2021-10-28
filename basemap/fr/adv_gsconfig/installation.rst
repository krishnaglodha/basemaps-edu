.. geoserver.installation:

Installer l'Extension Monitoring 
===================================

Monitoring est une extension de la communauté, et donc on ne la trouve pas sur les pages de délivrance standard de GeoServer. Les extension de la communauté ne sont disponibles que via `Nightly builds <http://geoserver.org/display/GEOS/Nightly>`_ ou dans la compilation de la source.

#. Téléchargez l'extension "monitoring" liée à `GeoServer nightly builds page <http://geoserver.org/display/GEOS/Nightly>`_.

   .. Attention:: Assurez-vous que l'extension correspond à la version de installation de GeoServer installée.

#. Extrayez les contenus de l'archive dans le répértoire ``WEB-INF/lib`` 
de l'installation de GeoServer.

Verifier l'Installation
---------------------------

Il ya deux façons de vérifier que l'extension de monitoring extension soit bien installée.

* Démarrez GeoServer et ouvrez le :ref:`geoserver.wai`.  Enregistrez vous en utilisant l'administration account. S'il est bien installé, il y aura une section :guilabel:`Monitor`  sur la colonne de gauche de la home page.

  .. figure:: img/monitorwebadmin.png
     :align: center

     *Monitoring section in the web admin interface*

* Démarrez GeoServer et naviguez au courant :ref:`geoserver.gs_data_dir`. S'il est bien installé, un nouveau répertoire  nommé ``monitoring`` serà créé dans le répértoire des données.
