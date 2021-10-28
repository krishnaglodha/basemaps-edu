.. module:: geoserver.grib_mosaic
   :synopsis: Mosaicking Grib files with additional dimension.

.. _geoserver.grib_mosaic:

Mosaicking Grib files with additional dimension
===========================================================================

In this page will be presented how to mosaic Grib data. These data represents the wind amplitude for the x and y directions on the sea surface of all the world.
As you can see, the steps for configuring the mosaic are very similar to those of the NetCDF mosaic.

Check the indexer
^^^^^^^^^^^^^^^^^^^^^^

#. Navigate to the :file:`%TRAINING_ROOT%/geoserver_data/coverages/REGIONAL/wind/indexer.xml` file and open it. 
    
#. At the end of the file you should have a **parameters** section like this:

   .. code-block:: xml
   
    <parameters>
        <parameter name="AuxiliaryFile" value="_wind.xml" />
        <parameter name="AbsolutePath" value="true" />
    </parameters>
   
Configuring the Grib mosaic in GeoServer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Look at the :file:`%TRAINING_ROOT%/geoserver_data/coverages/REGIONAL/wind`. The folder contains several files:

   #. **indexer.xml**: An XML file containing the Grib ImageMosaic index definition previously seen.
   
   #. **_wind.xml**: An XML file representing the Grib ancillary file to be shared by all the granules.
   
   #. **datastore.properties**: A text file containing the connection parameters to an external *Postgres* database.


#. From the GeoServer Administration GUI, go to the `Stores > Add new Store` page and configure a new ImageMosaic datastore.

#. Insert `windGrib` as `Data Source Name` and `file:coverages/REGIONAL/wind` as URL

   .. figure:: img/imagemosaic_grib_ds_001.png

   .. note:: Notice that the `ImageMosaic` plugin requires the folder name. Notice also that since the dataset is physically located under the `${GEOSERVER_DATA_DIR}` you can use a relative path.

   .. note:: For Postgres version lower than 9.2.3 the following parameter must be added to the **datastore.properties** file:
   
		.. code-block:: xml
		
			create\ database\ params=WITH\ TEMPLATE\=template_postgis
	
		(Specifying the proper postgis template. In this example: template_postgis).
   
#. Select the first layer *u-component_of_wind_height_above_ground*  

   .. figure:: img/imagemosaic_grib_ds_002.png

#. On the `Dimensions` tab, configure the time and elevation dimensions

   .. figure:: img/imagemosaic_grib_ds_003.png

#. Click on the `save` button when done

#. Repeat the Layer configuration steps to add the other layer contained within the `windGrib` store: `v-component_of_wind_height_above_ground`

Customizing GRIB cache directory location
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When accessing a Grib file on GeoServer for the first time, by default two additional files are created. These files have the same name of the grib 
file and also the following extensions:

	* .ncx
	* .gbx9
	
Those files are generated automatically by the internal Java low level libraries at the first access to the input data or if they have 
been removed. By default on GeoServer, those files are created in the same directory of the Grib file, but this could lead to an exception if
the directory is not writable. This behaviour can be avoided by setting a Java parameter called `GRIB_CACHE_DIR` with the path to a 
writable directory where the .ncx and .gbx9 files will be stored. 

In case you need to specify a different Grib cache location:

  * Check if your ``setenv.bat`` has the following property set in the JAVA_OPTS section and update/add it with a new location::
  
    -DGRIB_CACHE_DIR=PATH/TO/GRIB_CACHE_DIR
