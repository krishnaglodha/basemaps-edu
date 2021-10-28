.. module:: geoserver.get_started
   :synopsis: Presentation of the tools and plugins of multidim GeoServer extensions.

.. _geoserver.get_started:

Preparing GeoServer for serving Multidimensional data
=========================================================

The GeoServer provided with this workshop has been set-up to contain all the libraries you need to proceed with the training  (e.g.NetCDF and GRIB dependencies). The next section provides instructions which may be used to set-up the required set of libraries by yourself.

.. important:: Everytime you see the :file:`%TRAINING_ROOT%` placemark in a path that has to be given to GeoServer, you **must** substitute this with the absolute path of the training installation folder, or alternatively set an environment variable accordingly.

If instead, you execute the provided `setenv.bat` command as the first step when opening the command line, then the `%TRAINING_ROOT%` variable will be automatically substituted for you.

.. note:: **Linux users have to notice** that in this section only the Windows notation :file:`%TRAINING_ROOT%` is used and never :file:`$TRAINING_ROOT`. Just remember to use the latter everytime the first is reported.



Gathering required extension
---------------------------------------------------------
The GeoServer version used for this training is on the 2.18.x branch. It can be downloaded from `this link <https://build.geoserver.org/geoserver/2.18.x/geoserver-2.18.x-latest-war.zip>`_

A set of external modules and extensions is required to deal with the additional capabilities introduced in the following sections:

#. **Web Processing Service (WPS) Extension** for running geoprocesses and using *rendering transformations*. Download the WPS extension from `this link <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_ and extract it on disk.

#. **Web Coverage Service Profile for Earth Observation Data** which implements the EO Profile for WCS 2.0.1. Download the WCS2.0_eo extension from `this link <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_ and extract it on disk.

#. The **NetCDF Reader**, download  it from  `this link <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_ and extract it on disk.

#. The **NetCDF WCS output format**, download it from `this link <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_ and extract it on disk.

#. The **GriB Reader**, download it from `this link <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_ and extract it on disk.

#. **Additional Extensions/Community Modules**. Download the following community modules: 

   #. `WMS-EO protocol support <https://build.geoserver.org/geoserver/2.18.x/community-latest/>`_
   #. `Dynamic ColorMap Extension <https://build.geoserver.org/geoserver/2.18.x/community-latest/>`_

.. note:: All the mentioned zip files for each module/extension can be found in `%TRAINING_ROOT%/data/plugins` folder.
   
Installing the extensions
---------------------------
Once you have downloaded all the extensions you need to unzip them and then copy all the provided JAR files into the *WEB-INF/lib* folder of your GeoServer distribution. For the training the latter can be found at `%TRAINING_ROOT%/tomcat/instances/instance1/webapps/geoserver/WEB-INF/lib`.

   .. note:: Remember that all the needed JARs can be found, for your convenience, under the `%TRAINING_ROOT%/data/plugins` folder.

Once the JARs have been copied in the right location we are supposed to update the :file:`setenv.bat` file in order to include the required **JAVA_OPTS** for configuring the extensions as follows:

.. warning:: This step is **very** important. Without those properties correctly set the next exercises won't work as expected.

* When ingesting and serving time-series data, GeoServer needs to be run in a web container that has the **timezone properly configured** in order to avoid problems during requests due to wrong timezone applied when parsing and/or ingesting timestamp values. To set the time zone to be Coordinated Universal Time (UTC) (**which we strongly recommend**), you should add the following switch when launching the Java process for GeoServer. This will ensure that every time a String representing a time is parse or encoded it is done using the GMT timezone; the same applies to when Date objects area created internally in Java.

    .. code-block:: xml
	
	  -Duser.timezone=GMT

* If you are using a shapefile as the mosaic index store, another java process option is needed to enable support for timestamps in shapefile stores:

    .. code-block:: xml
	
	-Dorg.geotools.shapefile.datetime=true
   
  Support for timestamp is not part of the DBF standard (used in shapefile for attributes). The DBF standard only supports Date and only few applications understand it. As long as shapefiles are only used for GeoServer input that is not a problem, but the above setting will cause issues if you have WFS enabled and users also download shapefiles as GetFeature output: if the feature type extracted has timestamps, then the generated shapefile will have as well, making it difficult to use the generated shapefile in desktop applications. As a rule of thumb, if you also need WFS support it is advisable to use an external store (PostGIS, Oracle) instead of shapefile. Of course, if all that's needed is a date, using shapefile as an index without the above property is fine as well.

* When accessing NetCDF datasets, some auxiliary files are automatically created. By default they will be created beside the original data. However, in order to avoid write permission issues with the directories containing the NetCDF datasets, it is possible to configure an external directory to contain the auxiliary files. Add this switch to the Java process.

    .. code-block:: xml
	
	  -DNETCDF_DATA_DIR=%TRAINING_ROOT%/netcdf_data_dir

* When accessing GRIB datasets, some auxiliary files are automatically created and cached to speedup the access. By default they will be created beside the original data. However, in order to avoid write permission issues with the directories containing the GRIB datasets, is it possible to configure an external directory to contain the cache files. Add this switch to the java process

    .. code-block:: xml
	
	  -DGRIB_CACHE_DIR=%TRAINING_ROOT%/grib_cache_dir
	  
Checking the installation
---------------------------

In order to make sure the various extensions were properly installed, you should start the GeoServer instance provided with the training and verify that the plug-in and extensions are available through the user interface:

   .. note:: You have to be logged as an Admin in order to see the following pages.

* Look on the left-side menu for the EO Layers Group voice

  .. figure:: img/wcs_wms_eo_001.png
	 :align: center
	 
	 Checking EO Extensions proper installation.

* Click on the WCS menu item 

  .. figure:: img/wcs_wms_eo_003.png
	 :align: center
	 
	 The WCS menu item under settings.

* Look for the WCS EO options on the bottom
  
  .. figure:: img/wcs_wms_eo_004.png
	 :align: center
	 
	 Checking EO Extensions proper installation.

		 
.. Warning:: The ones above are quick checks to be sure that GeoServer still works correctly and has loaded the extensions. In the next sections there are exercises that will give you the instruments to test and check that the multidimensional extensions work as expected.


