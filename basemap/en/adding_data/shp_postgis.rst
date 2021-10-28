.. module:: geoserver.shp_postgis
   :synopsis: Learn how to loading a Shapefile into Postgis.

.. _geoserver.shp_postgis:

Loading a Shapefile into PostGIS
--------------------------------

This task shows how to load a ShapeFile into PostGIS database:

#. Ensure that postgis server is running. If not, open the terminal window and start it:

      .. code-block:: console

            (Windows)

                  postgis_start.bat

#. Once postgis is running, enter the following command and press enter to creating a new database named 'shape':

      .. code-block:: console

            (Linux)

            createdb -U geosolutions -T postgis20 shape

            or 

            (Windows)

            setenv.bat
            createdb -U geosolutions -T postgis20 shape

#. Then enter the following command and press enter to load the ShapeFile into 'shape' database:

      .. code-block:: console

            (Linux)    
            
            shp2pgsql -I ${TRAINING_ROOT}/data/user_data/Mainrd.shp public.main_roads | psql -d shape
     
            or 

            (Windows)
  
            shp2pgsql -I "%TRAINING_ROOT%\data\user_data\Mainrd.shp" public.main_roads | psql -U postgres -d shape
    
   The ShapeFile will be loaded within the 'main_roads' table of the 'shape' database. 
   
   The following screenshot shows some of the table contents in ``pgAdmin``.

   .. note:: In Windows, you can run pgAdmin, executing **pgAdmin.bat** batch file at terminal window.

   .. figure:: img/shp_postgis1.png

      A PostGIS table by ShapeFile

In the :ref:`next <geoserver.postgis_lay>` section we will see how to add a PostGIS layer into GeoServer.
