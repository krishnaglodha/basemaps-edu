.. module:: geoserver.add_shp
   :synopsis: Learn how to adding a Shapefile.

.. _geoserver.add_shp:

Adding a Shapefile
------------------

Adding a Shapefile is the core of any GIS tool. This section covers the task of adding and publishing a Shapefile with GeoServer.

#. Navigate to the workshop directory :file:`${TRAINING_ROOT}/data/user_data/` (on Windows :file:`%TRAINING_ROOT%\\data\\user_data`) and find the following shapefiles::

     Mainrd.shp
     Mainrd.shx
     Mainrd.dbf
     Mainrd.prj

   Copy the files in the following directory::

     $geoserver_data/data/boulder
   
   for Windows::
     
      %geoserver_data_dir%\data\boulder
        
        
   .. note:: Ensure that all the four parts of the shapefile are copied.  This includes the ``shp``, ``shx``, ``dbf``, and ``prj`` extensions.

#. Navigate to the GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Use the following credentials to :guilabel:`Login`:  

	- username: admin 
	- password: Geos

   .. figure:: img/vector1.png
      :width: 600
		
      GeoServer Login

#. Click the :guilabel:`Add stores` button.

   .. figure:: img/vector2.png
      :width: 500
   
      Add stores link

#. Click the :guilabel:`Shapefile`.

   .. figure:: img/vector3.png
      :width: 600

      Add a new shapefile

   .. note:: The new data source menu contains a list of all the spatial formats supported by GeoServer. When creating a new data store one of these formats must be chosen. Formats like Shapefile and PostGIS are supported by default, and many other formats are available as extensions.

#. On the :guilabel:`New Vector Data Source` page, enter "Mainrd" in the :guilabel:`Data Source Name` and :guilabel:`Description` fields. Finally click on Browse... in order to set the Shapefile location in the :guilabel:`URL` field and click :guilabel:`Save`.

   .. note:: The Mainrd.shp was copied in the data directory, inside the "data/boulder" folder.
   
   .. figure:: img/vector4.png
      :width: 600
	  
      Specifying Shapefile parameters

#. After saving, Click :guilabel:`Publish`.

   .. figure:: img/vector5.png
      :width: 600
	  
      Publishing a layer from the shapefile

#. Set the :guilabel:`Coordinate Reference Systems` EPSG, in this case EPSG: 2876. The :guilabel:`Name` and :guilabel:`Title` fields should be automatically filled.

   .. figure:: img/vector6.png
      :width: 600
	  
      Populate fields.

   Scroll down the page and generate the bounds for the layer by clicking the :guilabel:`Compute from data` button in the :guilabel:`Bounding Boxes` section.

   .. figure:: img/vector7.png
      :width: 600
	  
      Generating the layer bounding box

#. Scroll to the bottom of the page, notice the read only :guilabel:`Feature Type Detail` table and then click :guilabel:`Save`.

   .. figure:: img/vector8.png
      :width: 600
	  
      Submitting the layer configuration

#. If all went well, you should see something like this:

   .. figure:: img/vector9.png
      :width: 600
	  
      After a successful save

   At this point a shapefile has been added and is ready to be served by GeoServer.
	  
   .. figure:: img/vector10.png
	  :width: 600

#. Choose the ``preview`` link in the main menu and filter the layer list with ``mainrd``:

   .. figure:: img/preview_shapefile1.png
	  :width: 600
	  
	  Selecting the ``mainrd`` shapefile in the layer preview.

#. Click on the ``OpenLayers`` link to preview the layer in an interactive viewer:

   .. figure:: img/preview_shapefile2.png
	  
      The ``mainrd`` shapefile preview

In the :ref:`next <geoserver.shp_postgis>` section we will see how to load a ShapeFile into PostGIS.
