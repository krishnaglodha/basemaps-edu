.. module:: geoserver.adding_data
   :synopsis: Learn how to adding data to GeoServer.

.. _geoserver.adding_data:

Adding a (gpkg) Raster Layer
========================

In this section you will publish a GeoPackage raster layer in GeoServer.


#. Open the GeoServer UI:
   
   .. code-block:: console
   
      http://localhost:8083/geoserver/web


#. From the **Data > Stores** page click the **Add new Store** button

   .. figure:: img/add-new-store.png
         :width: 400

#. Select the GeoPackage format from the Raster section:

   .. figure:: img/gpkg-ras-store.png
         :width: 400         

#. Define the store properties:

   **Data Source Name**: gpkg-bmreduced

   **Connection parameters**: browse the filesystem to identify the GPKG database:
   
   .. code-block:: console

      (Linux)
      cd ${TRAINING_ROOT}/data/user_data/countries.gpkg

      or

      (Windows)
      cd %TRAINING_ROOT%\data\user_data directory\countries.gpkg

   .. figure:: img/gpkg-new-ras-store-1.png
         :width: 600  

#. Click "Save".

#. Publish the layer:

   .. figure:: img/gpkg-ras-layer-pub.png
      :width: 500 
   
   .. figure:: img/gpkg-ras-layer-edit-0.png
      :width: 400 
   
   .. figure:: img/gpkg-ras-layer-edit-1.png
      :width: 400 

#. Click "Save".

#. Preview the layer:

   .. figure:: img/gpkg-ras-layer-preview-0.png
      :width: 400 

   .. figure:: img/gpkg-ras-layer-preview-1.png
      :width: 400 