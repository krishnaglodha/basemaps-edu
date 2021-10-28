.. module:: geoserver.adding_data
   :synopsis: Learn how to adding data to GeoServer.

.. _geoserver.adding_data:

Loading GeoTiff into GeoPackage
========================

In this section you will use the GDAL_TRANSLATE utility to create a GeoPackage layer starting from a GeoTiff image.

.. code-block:: console

      gdal_translate -of GPKG <input-file>.tif <output-file>.gpkg


**Example**      

#. Open the gdal shell

#. Move to the user_data directory:

   .. code-block:: console

      (Linux)
      cd ${TRAINING_ROOT}/data/user_data directory

      or

      (Windows)
      cd %TRAINING_ROOT%\data\user_data directory

#. Run:      

   .. code-block:: console

      gdal_translate -of GPKG bmreduced.tiff countries.gpkg -co APPEND_SUBDATASET=YES -co RASTER_TABLE=bmreduced -co TILE_FORMAT=JPEG

   .. code-block:: console
      
      gdaladdo -r cubic -oo TILE_FORMAT=JPEG countries.gpkg 2 4 8 16 32 64

#. Check the GeoPackage with a GIS client (eg. QGIS):

      #. From the Browser panel right click the GeoPackage section and click "New connection..."

            .. figure:: img/gpkg-qgis-new-connection.png
                  :width: 200
      
      #. Browse the filesystem and select the created countries.gpkg database
      
      #. Add the layer named bmreduced into the Map canvas:
      
            .. figure:: img/gpkg-qgis-add-ras-layer.png
                  :width: 200            

            .. figure:: img/gpkg-bmreduced-qgis.png
                  :width: 600

