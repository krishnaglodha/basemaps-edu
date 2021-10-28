.. module:: geoserver.creating_setting
   :synopsis: Learn how to creating and setting a new GeoServer data directory.

.. _geoserver.creating_setting:


Creating and setting a new GeoServer Data Directory
---------------------------------------------------


#. Generally if GeoServer is running in Web Archive mode inside of a servlet container, as in this Workshop, the data directory is located at ``<web application root>/data`` (the data directory contains the GeoServer configuration data).

#. The first thing to do is to configure correctly the geoserver_data directory. For facilities reasons,  the ``geoserver_data`` is configured by default under the directory::
	
	
	${TRAINING_ROOT}/geoserver_data or %TRAINING_ROOT%\geoserver_data on Windows
	
	
   Generally this is not an issue. But if you run the system from the Linux ISO this folder resides in the memory. The first thing to do is to move this folder into a local persistent storage.
	
	
	* Move the ``geoserver_data`` somewhere in the persistent storage using the command::
	
		sudo mv -f ${TRAINING_ROOT}/geoserver_data <TARGET_DIR>
	
	
	* Make a symbolic link to the ``geoserver_data`` by issuing the command::
	
		ln -s <TARGET_DIR> ${TRAINING_ROOT}/geoserver_data
	
	
   .. warning:: Check that the user ``geosolutions`` has permissions to read/write all the files/folder inside the ``geoserver_data``.

   .. note:: Instead of creating a symbolic link you can configure GeoServer in order to allow it to point to the new ``geoserver_data``. To do that edit the file :file:`/opt/tomcat-geoserver/webapps/geoserver/WEB-INF/web.xml` and modify the context param ``geoserver_data``.

