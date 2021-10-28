.. module:: geoserver.adding_data
   :synopsis: Learn how to adding data to GeoServer.

.. _geoserver.adding_data:

Install required plugins
========================

In this section you will install the GPKG plugin for GeoServer.


#. The required plugin is **geoserver-2.18-SNAPSHOT-geopkg-plugin.zip**.
   
   It is available from the **community modules** section of the GeoServer download area

   You can find it in the data/plugin directory of the training package.
   
#. Extract the JAR files from the downloaded archive:

   .. figure:: img/gpkg-plugin-extract.png
         :width: 600

#. Copy the JAR files into the GeoServer WEB-INF/lib

   .. figure:: img/gpkg-plugin-copy.png
      :width: 600

#. Restart GeoServer

#. Check if the GeoPackage stores (vector/raster) are available:

   .. figure:: img/gpkg-stores.png
      :width: 400

