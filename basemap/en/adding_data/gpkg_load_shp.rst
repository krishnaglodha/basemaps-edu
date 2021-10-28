.. module:: geoserver.adding_data
   :synopsis: Learn how to adding data to GeoServer.

.. _geoserver.adding_data:

Loading Shapefile into GeoPackage
========================

In this section you will use the OGR2OGR utility to create a GeoPackage layer starting from a shapefile.

.. code-block:: console

      ogr2ogr \
      -s_srs EPSG:4326 \
      -t_srs EPSG:3857 \
      -f GPKG <output-file>.gpkg \
      <input-file>.shp


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

      ogr2ogr -s_srs EPSG:4326 -t_srs EPSG:3857 -nlt MULTIPOLYGON -f GPKG countries.gpkg ne_50m_admin_0_countries.shp

   Where:

   * **-s_srs**: SRS of the source (shapefile)
   * **-t_srs**: SRS of the target (geopackage)
   * **-nlt**: define the geometry type for the layer. In this case we used MULTIPOLYGON because the shapefile contains bot POLYGONs and MULTIPOLYGONs geometry types.
   * **-f**: define the output format (GPKG)
   * **<output>.gpkg**: output path for the geopakage 
   * **<input>.shp**: input path for the shapefile 


#. Check the GeoPackage with a GIS client (eg. QGIS):

      #. From the Browser panel right click the GeoPackage section and click "New connection..."

            .. figure:: img/gpkg-qgis-new-connection.png
                  :width: 200
      
      #. Browse the filesystem and select the created countries.gpkg database
      
      #. Add the layer named ne_50m_admin_0_countries into the Map canvas:
      
            .. figure:: img/gpkg-qgis-add-layer.png
                  :width: 200            

            .. figure:: img/gpkg-countries-qgis.png
                  :width: 600

