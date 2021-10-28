.. module:: geoserver.adding_data
   :synopsis: Learn how to adding data to GeoServer.

.. _geoserver.adding_data:

Adding a (gpkg) Vector Layer
========================

In this section you will publish a GeoPackage vector layer in GeoServer.


#. Open the GeoServer UI:
   
   .. code-block:: console
   
      http://localhost:8083/geoserver/web


#. From the **Data > Stores** page click the **Add new Store** button

   .. figure:: img/add-new-store.png
         :width: 400

#. Select the GeoPackage format from the Vector section:

   .. figure:: img/gpkg-vec-store.png
         :width: 400         

#. Define the store properties:

   **Data Source Name**: gpkg-countries

   **Connection parameters**: browse the filesystem to identify the GPKG database:
   
   .. code-block:: console

      (Linux)
      cd ${TRAINING_ROOT}/data/user_data/countries.gpkg

      or

      (Windows)
      cd %TRAINING_ROOT%\data\user_data directory\countries.gpkg

   .. figure:: img/gpkg-new-store-1.png
         :width: 400  

#. Click "Save".

#. Publish the layer:

   .. figure:: img/gpkg-vec-layer-pub.png
      :width: 400 
   
   .. figure:: img/gpkg-vec-layer-edit-0.png
      :width: 400 
   
   Click "compute from native bounds":

   .. figure:: img/gpkg-vec-layer-edit-1.png
      :width: 400 

#. Click "Save".

#. Preview the layer:

   .. figure:: img/gpkg-vec-layer-preview-0.png
      :width: 400 

   .. figure:: img/gpkg-vec-layer-preview-1.png
      :width: 400 