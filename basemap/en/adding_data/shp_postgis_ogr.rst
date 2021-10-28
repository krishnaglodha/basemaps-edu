.. module:: geoserver.shp_postgis_ogr
   :synopsis: Learn how to loading a Shapefile into Postgis using ogr2ogr.

.. _geoserver.shp_postgis:

Loading a Shapefile into PostGIS - ogr2ogr
--------------------------------

This task shows how to load a ShapeFile into PostGIS database:

#. Ensure that postgis server is running. If not, open the terminal window and start it:

      .. code-block:: console

            (Windows)

                  postgis_start.bat

#. Then enter the following command and press enter to load the ShapeFile into 'shape' database:

      .. code-block:: console
      
            (Linux)
            ogr2ogr -f PostgreSQL PG:"dbname='shape' host='127.0.0.1' port='5434' user='geosolutions' password='Geos'" ../data/user_data/Mainrd.shp -lco GEOMETRY_NAME=geom -lco FID=gid -lco SPATIAL_INDEX=GIST -nlt PROMOTE_TO_MULTI -nln main_roads_2 -overwrite

            (Windows)
            ogr2ogr -f PostgreSQL PG:"dbname='shape' host='127.0.0.1' port='5434' user='geosolutions' password='Geos'" ..\data\user_data\Mainrd.shp -lco GEOMETRY_NAME=geom -lco FID=gid -lco SPATIAL_INDEX=GIST -nlt PROMOTE_TO_MULTI -nln main_roads_2 -overwrite

Where:

      * **-f PostgreSQL**: specifies the output format as PostgreSql table
      * **-lco GEOMETRY_NAME**: (layer creation option) specifies the name of the geometry field
      * **-lco FID**: (layer creation option) specifies the name of the feature identifier column to create
      * **-lco SPATIAL_INDEX**: (layer creation option) specifies the type of spatial index (available values are: NONE/GIST/SPGIST/BRIN)
            
The ShapeFile will be loaded within the 'main_roads_2' table of the 'shape' database. 
   
The following screenshot shows some of the table contents in ``pgAdmin``.

      .. note:: In Windows, you can run pgAdmin, executing pgAdmin.bat batch file at terminal window.

      .. figure:: img/shp_postgis2.png

      A PostGIS table by ShapeFile

In the :ref:`next <geoserver.postgis_lay>` section we will see how to add a PostGIS layer into GeoServer.
