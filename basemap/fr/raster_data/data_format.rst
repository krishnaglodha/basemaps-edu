.. module:: geoserver.data_format

.. _geoserver.data_format:

Ajout d'un format de données GDAL
---------------------------------
Dans le cas où les bibliothèques GDAL appropriés sont disponibles, il est possible d'accéder à des formats de données pris en charge par plusieurs GDAL.
En fait, les plugins GDAL disponibles permettent de soutenir le format de données DTED, EHdr, ERDASImg, MrSID, JP2K (via MrSID Driver) et NITF.
En outre, dans le cas d'une licence valide ont été achetés et la bibliothèque native adéquate est disponible, aussi ECW, JP2K (via ECW) et JP2K (via Kakadu) sont prises en charge.
Cette section fournit des instructions pour ajouter et publier un ensembles de données MrSID, ECW and JP2K.

   .. Attention::
    
      Cela suppose que  la GeoServer image GDAL plug-in est déjà installé. Le plugin GDAL est normalement une extension.
	  
	  
      Si vous utilisez l'installation de Windows, avant de faire l'exercice installer le 'geoserver-2.2-SNAPSHOT-gdal-plugin' de :file:`%TRAINING_ROOT%\\data\\plugins\\`. Il suffit de décompresser le fichier zip dans  :file:`%TRAINING_ROOT%\\webapps\\geoserver\\WEB-INF\\lib\\` et redémarrez GeoServer.

Données MrSID Set
^^^^^^^^^^^^^^^^^

#. Ouvrez le navigateur Web et accédez à la page de GeoServer `Welcome Page <http://localhost:8083/geoserver>`_.

#. sélectionner :guilabel:`Add stores` à partir de l'interface. 

   .. figure:: img/geotiff_addstores.png

#. sélectionner :guilabel:`MrSID - MrSID Coverage Format` de l'ensemble des sources disponibles de données raster. 

   .. figure:: img/gdal_sources.png

#. Spécifiez un nom (par exemple, :file:`c3008957_nes_20`) dans le domaine :guilabel:`Data Source Name` de l'interface. 

#. Spécifiez :file:`file:data/user_data/c3008957_nes_20/c3008957_nes_20.sid` comme URL des données d'échantillon dans le domanie :guilabel:`Connections Parameter's - URL`. 

   .. figure:: img/gdal_addraster.jpg

#. Cliquez :guilabel:`Save`. 

#. Publier le calque en cliquant sur ​​le lien :guilabel:`publish`. 

   .. figure:: img/gdal_publish.jpg

#. Cliquez :guilabel:`Save` lorsque vous avez terminé. 

A ce stade, les données MrSID est publié avec GeoServer. 

#. Cliquez le lien :guilabel:`Layer Preview` link dans le menu de gauche de GeoServer. 

#. Cherchez une couche *geosolutions:c3008957_nes_20* et cliquez le lien :guilabel:`OpenLayers` à côté de celui-ci. 

   .. figure:: img/gdal_preview.jpg

   .. figure:: img/gdal_openlayer.jpg

ECW Data Set
^^^^^^^^^^^^^^

.. Attention:: Attention, vous avez besoin d'une licence pour utiliser les données ECW fixe. Ici, nous utilisons un fichier ECW distribué gratuitement uniquement pour la démonstration.

ECW (Enhanced Compression Wavelet) est une compression par ondelettes format d'image propriétaire optimisé pour l'imagerie aérienne et satellite.

#. Ouvrez le navigateur Web et accédez à la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_.

#. sélectionner :guilabel:`Add stores` à partir de l'interface. 

   .. figure:: img/geotiff_addstores.png

#. sélectionner :guilabel:`ECW - ECW Coverage Format` de l'ensemble des sources disponibles de données raster.

   .. figure:: img/ecw.png

#. Spécifiez un nom (par exemple, :file:`TerraColor_Sydney_AU_15m.ecw`) dans le domaine :guilabel:`Data Source Name` de l'interface. 

#. Spécifiez :file:`file:data/user_data/tc_sydney_au_ecw/TerraColor_Sydney_AU_15m.ecw` comme URL des données d'échantillon dans le domaine :guilabel:`Connections Parameter's - URL`. 

   .. figure:: img/ecw0.png

#. Cliquez :guilabel:`Save`. 

#. Publier le calque en cliquant sur ​​le lien :guilabel:`publish`. 

   .. figure:: img/ecw1.png

A ce stade, les données ECW est publié avec GeoServer. 

#. Cliequez sur le lien :guilabel:`Layer Preview` dans le menu de gauche GeoServer. 

#. Cherchez une couche *geosolutions:TerraColor_Sydney_AU_15m* et cliquez le lien :guilabel:`OpenLayers` à côté de celui-ci. 

   .. figure:: img/ecw3.png

   .. figure:: img/ecw4.png


JP2K Data Set
^^^^^^^^^^^^^^

JPEG 2000 est un système de codage d'image qui utilise des techniques de compression state-of-the-art basé sur la technologie des ondelettes.

#. Ouvrez le navigateur Web et accédez à la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_.

#. sélectionner :guilabel:`Add stores` à partir de l'interface. 

   .. figure:: img/geotiff_addstores.png

#. sélectionner :guilabel:`JP2ECW - JP2 (ECW) Coverage Format` de l'ensemble des sources disponibles de données raster. 

   .. note:: nous avons utilisé :guilabel:`JP2ECW - JP2 (ECW) Coverage Format` parce que :guilabel:`JP2MrSID - JP2 (MrSID) Coverage Format` n'est pas totalement stable, et peut ne pas fonctionner correctement en particulier avec plusieurs distributions Linux.

   .. figure:: img/jpeg2k0.png

#. Spécifiez un nom (par exemple, :file:`bogota`) dans le domaine :guilabel:`Data Source Name` sur l'interface. 

#. Spécifiez :file:`file:data/user_data/tc_sydney_au_jp2/TerraColor_Sydney_AU_15m.jp2` comme URL des données d'échantillon dans le domaine the :guilabel:`Connections Parameter's - URL`. 

   .. figure:: img/jpeg2k1.png

#. CLiquez :guilabel:`Save`. 

#. Publier le calque en cliquant sur ​​le lien :guilabel:`publish`. 

   .. figure:: img/jpeg2k2.png

   .. figure:: img/jpeg2k3.png

A ce stade, les données ECW est publié avec GeoServer. 

#. Cliquez le lien :guilabel:`Layer Preview` dans le menu de gauche de GeoServer. 

#. Cherchez la couche a *geosolutions:TerraColor_Sydney_AU_15m* et cliquez le lien :guilabel:`OpenLayers` à côté de celui-ci. 
