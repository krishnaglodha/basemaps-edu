.. module:: geoserver.structure
   :synopsis: Learn the structure of the GeoServer Data Directory.

.. _geoserver.structure:

Structure of the GeoServer Data Directory
=========================================

The following is the GEOSERVER_DATA structure::

	data_directory/
		app-schema-cache/
		cluster/
		coverages/
   		data/
   		demo/
		gwc/
		gwc-layers/
   		layergroups/
		layouts/
		legendsamples/
   		logs/
		monitoring/
		palettes/
   		security/
   		styles/
   		temp/
		tmp/
   		user_projections/
   		validation/
      wfs/
   		workspaces/
   		www/
   		global.xml
   		gwc-gs.xml
   		logging.xml
   		wcs.xml
   		wfs.xml
   		wms.xml
		wmts.xml
   		wps.xml

.. list-table::
   :widths: 20 80

   * - **File**
     - **Description**
   * - ``app-schema-cache``
     - This is the application schema files cache for the App Schema extension.
   * - ``cluster``
     - Contains configuration files for the geoserver-jms-cluster community module.
   * - **coverages**
     - Contains some demo raster layers for this training.
   * - **data**
     - Not to be confused with the ``GeoServer data directory`` itself. The data directory is a location where data can be stored. This directory is commonly used to store shapefiles and raster files but it can be used for any file based data. The main benefit of storing data files inside of the data directory is the portability.
   * - ``demo``
     - The demo directory contains files which define the sample requests available in the Sample Request Tool.
   * - **gwc**
     - This directory holds the cache created by the embedded GeoWebCache service.
   * - **gwc-layers**
     - This directory holds the configuration files created by the embedded GeoWebCache service for each layer.
   * - **layergroups**
     - Contains information on the layer groups configurations.
   * - **layouts**
     - Contains configuration files for WMS decorations.
   * - ``legendsamples``
     - Contains the cached versions of WMS legends.
   * - **logs**
     - This directory contains the GeoServer logging files (log file and logging properties files).
   * - ``monitoring``
     - Contains configuration files for the Monitor extension.
   * - **palettes**
     - The palettes directory is used to store pre-computed Image Palettes. Image palettes are used by the GeoServer WMS as way to reduce the size of produced images while maintaining image quality.
   * - **security**
     - The security directory contains all the files used to configure the GeoServer security subsystem. This includes a set of property files which define access roles, along with the services and data each role is authorized to access.
   * - **styles**
     - The styles directory contains a number of Styled Layer Descriptor (SLD), files that contain styling information used by the GeoServer WMS. 
   * - ``temp`` and ``tmp``
     - Temporary directories, used by the WPS service and other modules. 
   * - **user_projections**
     - The user_projections  directory contains extra spatial reference system definitions. The epsg.properties can be used to add new spatial reference systems, whilst the epsg_override.properties file can be used to override (overwrite) an official definition with a custom one.
   * - ``validation``
     - This directory contains the validation rules.
   * - **wfs**
     - This directory holds temporary files and structures used by the WFS service.
   * - **workspaces**
     - The various workspaces directories contain metadata about ``stores`` and ``layers`` which are published by GeoServer. Each layer will have a layer.xml file associated with it, as well as either a coverage.xml or a featuretype.xml file depending on whether it's a raster or vector.
   * - ``www``
     - The www directory is used to allow GeoServer to act like a regular web server and serve regular files. Although it is not a substitute for a full-blown web server, the www  directory can be useful to easily serve OpenLayers map applications (this avoids the need to setup a proxy in order to respect the `same origin policy <http://en.wikipedia.org/wiki/Same_origin_policy>`_).
   * - **global.xml**
     - Contains settings that go across services, including contact information, JAI settings, character sets and verbosity.
   * - ``gwc-gs.xml`` 
     - Contains various settings for the GeoWebCache service.
   * - **logging.xml**
     - Specifies the logging level, location, and whether it should log to std out.  
   * - ``wcs.xml`` 
     - Contains the service metadata and various settings for the WCS service.
   * - ``wfs.xml`` 
     - Contains the service metadata and various settings for the WFS service.
   * - ``wms.xml`` 
     - Contains the service metadata and various settings for the WMS service.
   * - ``wmts.xml`` 
     - Contains the service metadata and various settings for the WMTS service.
   * - ``wps.xml`` 
     - Contains the service metadata and various settings for the WPS service. 

