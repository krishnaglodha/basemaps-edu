.. module:: geoserver.add_geotiff
   :synopsis: Learn how to adding a GeoTiff.

.. _geoserver.add_geotiff:

Ajout d'un GeoTiff
-------------------

Le GeoTIFF est un format de données géospatiales raster Largement utilisé: il est composé d'un fichier unique contenant à la fois la date et les informations de géoréférencement (à ne pas confondre avec .tiff/.tfw/.prj file triplet, qui est considéré comme "world image" file in GeoServer).
Cette section fournit des instructions pour ajouter et publier un fichier GeoTIFF.

#. Ouvrez le navigateur Web et accédez à la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_.

#. sélectionner :guilabel:`Add stores` à partir de l'interface. 

   .. figure:: img/geotiff_addstores.png

#. sélectionner :guilabel:`GeoTIFF - Tagged Image File Format with Geographic information` à partir de l'ensemble des sources disponibles de données raster. 

   .. figure:: img/geotiff_sources.png
   

#. Spécifiez un nom propre (pour exemple, :file:`13tde815295_200803_0x6000m_cl`) dans :guilabel:`Data Source Name` domaine de l'interface. 

#. Cliquez sur le lien pour définir l'emplacement GeoTIFF dans le domain :guilabel:`URL`.

   .. note:: le 13tde815295_200803_0x6000m_cl.tif est à :file:`${TRAINING_ROOT}/data/user_data/aerial/13tde815295_200803_0x6000m_cl.tif` (on windows :file:`%TRAINIG_ROOT%\\data\\user_data\\aerial\\13tde815295_200803_0x6000m_cl.tif`)

   .. figure:: img/addgeotiff1.png
      :width: 800

#. cliquez :guilabel:`Save`. 

#. Publier le layer en cliquant sur ​​le link :guilabel:`publish`. 

   .. figure:: img/addgeotiff2.png
      :width: 800

#. Vérifiez les systèmes de référence de coordonnées et les domaines Bounding Boxes fields sont correctement définis et cliquez sur :guilabel:`Save`. 

   .. figure:: img/addgeotiff3.png

#. A ce stade, le GeoTIFF est publié avec GeoServer. Vous pouvez utiliser le layer preview pour voir les données.

   .. figure:: img/addgeotiff4.png
