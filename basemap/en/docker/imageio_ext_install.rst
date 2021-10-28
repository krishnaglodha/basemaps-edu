.. module:: geoserver.imageio_ext_install

.. _geoserver.imageio_ext_install:


Installing ImageIO-Ext
----------------------

`ImageIO-Ext <https://github.com/geosolutions-it/imageio-ext>`_ is a bit different than other GeoServer extension, as the pure ImageIO-Ext library files are built into GeoServer by default.  However, in order for GeoServer to leverage these libraries, the ImageIO-Ext native libraries must be installed through your host system's OS. 
Once those libraries are installed, GeoServer will be able to recognize GDAL data types (this will require a Tomcat restart).

#. Navigate to the `imageio-ext download page <http://demo.geo-solutions.it/share/github/imageio-ext/releases/`_.
#. Select the most recent stable binary release and download it.
#. Then look for "native" folder and select it. 
#. Select "gdal".
   
   .. note:: Depending on your Geoserver release, you will need to consider using a different version of GDAL. Check this `link <https://docs.geoserver.org/maintain/en/user/data/raster/gdal.html#installing-gdal-native-libraries>`_.

#. Download and extract the GDAL CRS definitions:

	* Click on the "gdal-data.zip" to download the CRS definitions archive.
	* Extract this archive on disk and place it in a proper directory on your system.

   .. note:: Make sure to set a GDAL_DATA environment variable to the folder where you have extracted this file.
   
#. Select the folder related to your OS from the download page.

#. Download and extract/install the correct version for your OS:

   * Assuming you are on a 64 bits Ubuntu Operating System, click on the "gdal192-Ubuntu12-gcc4.6.3-x86_64.tar.gz" to download the native libraries archive (If the OS is Windows check the end of the page).
   * Extract the archive on disk and place it in a proper directory on your system.

   .. warning:: If you are on Windows, make sure that the GDAL DLL files are on your PATH. If you are on Linux, be sure to set the LD_LIBRARY_PATH environment variable to refer to the folder where the SOs are extracted.

   .. note:: The native libraries contains the GDAL gdalinfo utility which can be used to test whether or not the libs are corrupted. This can be done by browsing to the directory where the libs have been extracted and performing a *gdalinfo* command with the *formats* options that shows all the formats supported. Moreover the package contains also a Java versions of the gdalinfo utility to check also the Java bindings correct functioning (you can see a .bat script for Windows and .sh for Linux).

Extra Steps for Windows Platforms   
+++++++++++++++++++++++++++++++++++++
There are a few things to be careful with as well as some extra steps if you are deploying on Windows.

First of all, you'll notice that we have multiple versions like MSVC2005, MSVC2008 and so on matching the Microsoft Visual C++ Redistributables. Depending on the version of the underlying operating system you'll have to pick up the right one. You can google around for the one you need.

That said, we have DLLs for both 32 bits as well as 64 bits Operating Systems. Again, pick the one that matches your infrastructure.
   
Adding support for ECW and MrSID on Windows
+++++++++++++++++++++++++++++++++++++++++++
If you are on Windows and you want to add support for ECW and MrSID there is an extra step to perform.

In the Windows packaging ECW and MrSID are built as plugins hence they are not loaded by default but we need to place their DLLs in a location that is pointed by the *GDAL_DRIVER_PATH* environmental variable.
GDAL uses internally this env variable to look up additional drivers (notice that there are a few default places where GDAL will look anyway). For additional information, please, check this `link <http://trac.osgeo.org/gdal/wiki/ConfigOptions#GDAL_DRIVER_PATH>`_.

   
Final Result
++++++++++++++  
   
Once the previous steps have been completed, restart GeoServer.  If all the steps have been performed  correctly, new data formats will be in the :guilabel:`Raster Data Sources` list when creating a new data store as shown here below.

.. figure:: img/gdalcreate.png
   :width: 310
	  

   *GDAL image formats in the list of raster data stores*

If you use a GDAL (Base drivers + MrSID and ECW support) native libraries, all the data formats will be in the :guilabel:`Raster Data Sources` list are the following:

.. figure:: img/gdalcreate1.png
   :width: 440
	  

   *GDAL image formats in the list of raster data stores with ECW support*

.. warning::

   Native libraries: GDAL (Base drivers + MrSID and ECW support). Only download it if you have read _README_FIRST_ and you have agreed with the ECW EULA. Note that these binaries are not meant to be used freely in server side apps, consult the ECW license for details. 
   
   
   
