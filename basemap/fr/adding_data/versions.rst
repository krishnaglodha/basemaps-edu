.. module:: geoserver.versions
   :synopsis: Apprenez comment migrer un répertoire de données de GeoServer entre les différentes versions.

.. _geoserver.versions:

Migration d'un répertoire de données entre les différentes versions
====================================================================


En général il ne devrait être un problème la mise à jour de répertoires de données vers une nouvelle version de GeoServer (i.e. from 2.0.0 to 2.0.2 and vice versa, or from 1.6.x to 1.7.x and vice versa).


Migration à partir de 1.7.x to 2.2.x
-------------------------------------


Lors de l'utilisation 2.2.x GeoServer avec un répertoire de données de la branche 1.7.x, ce repértoire sera changé automatiquement et le rèpertorie ne sera plus compatible avec 1.7.x! Voici une liste des modifications apportées au répertoire de données.


Fichiers et répertoires ajoutés.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

	wfs.xml
	wcs.xml
	wms.xml
	logging.xml
	global.xml
	workspaces/*
	layergroups/*
	styles/*.xml

fichiers renommés
^^^^^^^^^^^^^^^^^^^

	- :guilabel:`catalog.xml` renamed to :guilabel:`catalog.xml.old`
	- :guilabel:`services.xml` renamed to :guilabel:`services.xml.old`


Restauration à partir d' 2.2.x à 1.7.x
----------------------------------------


Afin de rétablir le répertoire pour être compatible avec 1.7.x:

#. Stop GeoServer.

#. Supprimer les fichiers et répertoires suivants::

	wfs.xml
	wcs.xml
	wms.xml
	logging.xml
	global.xml
	workspaces/*
	layergroups/*
	styles/*.xml

#. renommez :guilabel:`catalog.xml.old` en :guilabel:`catalog.xml`.

#. renommez :guilabel:`services.xml.old` en :guilabel:`services.xml`.