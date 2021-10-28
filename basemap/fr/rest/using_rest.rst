.. module:: geoserver.using_rest
   :synopsis: Learn how to use the GeoServer REST module.

Using REST module
-----------------

This section contains a number of examples which illustrate various uses of the REST data configuration api.

The GeoServer REST configuration module uses the REST principles to expose services allowing to edit the catalog, in particular to manage workspaces, stores, layers, styles and groups.

.. note::
  
  The REST configuration extension has normally to be installed separately, it is not come out of the box.

The examples in this section use the `cURL <http://curl.haxx.se/>`_ utility, which is a handy command line tool for executing HTTP requests and transferring files.

#. Open the Terminal and enter the following command::

   	curl -u admin:Geos -v -XPOST -H 'Content-type: text/xml' -d '<workspace><name>myworkspace</name></workspace>' http://localhost:8083/geoserver/rest/workspaces

  the response should contains the following:

   .. figure:: img/workspace1.png

      Create a new workspace via REST

#. Go to the ``Workspaces`` section via Web interface to show the new workspace created

   .. figure:: img/workspace2.png

      GET request to abtain new workspace details

#. Get the new created workspace details entering the following::

	curl -u geosolutions:Geos -XGET -H 'Accept: text/xml' http://localhost:8083/geoserver/rest/workspaces/myworkspace

   .. figure:: img/workspace3.png

      GET request to obtain new workspace details

#. Publish a shapefile using the ``myworkspace`` workspace entering the following:: 

	curl -u geosolutions:Geos -H "Content-type: application/zip" -T ${TRAINING_ROOT}/data/user_data/pointlands.zip http://localhost:8083/geoserver/rest/workspaces/myworkspace/datastores/pointlands/file.shp

#. Go to the **Layer Preview** to show the layers in a OpenLayers Map.

   .. figure:: img/shape1.png

      Showing the new layer created

   .. figure:: img/shape2.png

      The new layers created

   .. note:: If you previously followed the security portion of the workshop the layer won't be accessible because the administrator does not have the required roles. Go back in the service security section and remove the rule limiting the GetMap requests.

#. Retrieves the created data store as XML entering the following::

		curl -u geosolutions:Geos -XGET http://localhost:8083/geoserver/rest/workspaces/myworkspace/datastores/pointlands.xml

   .. code-block:: xml
	
		<dataStore>
		  <name>pointlands</name>
		  <type>Shapefile</type>
		  <enabled>true</enabled>
		  <workspace>
			<name>myworkspace</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/myworkspace.xml" type="application/xml"/>
		  </workspace>
		  <connectionParameters>
			<entry key="url">file:${TRAINING_ROOT}/geoserver_data/data/myworkspace/pointlands/</entry>
			<entry key="namespace">http://myworkspace</entry>
		  </connectionParameters>
		  <__default>false</__default>
		  <featureTypes>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/myworkspace/datastores/pointlands/featuretypes.xml" type="application/xml"/>
		  </featureTypes>
		</dataStore>

   .. note:: 

      By default when a shapefile is uploaded a feature type resource and the associated layer are automatically created.

#. Retrieve the layer as XML entering the following::

		curl -u geosolutions:Geos -XGET http://localhost:8083/geoserver/rest/layers/myworkspace:pointlands.xml

   .. code-block:: xml
	
		<layer>
		  <name>pointlands</name>
		  <type>VECTOR</type>
		  <defaultStyle>
			<name>point</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/styles/point.xml" type="application/xml"/>
		  </defaultStyle>
		  <resource class="featureType">
			<name>pointlands</name>
			<atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/myworkspace/datastores/pointlands/featuretypes/pointlands.xml" type="application/xml"/>
		  </resource>
		  <enabled>true</enabled>
		  <metadata>
			<entry key="GWC.metaTilingX">4</entry>
			<entry key="GWC.autoCacheStyles">true</entry>
			<entry key="GWC.metaTilingY">4</entry>
			<entry key="GWC.gutter">0</entry>
			<entry key="GWC.enabled">true</entry>
			<entry key="GWC.gridSets">EPSG:4326,EPSG:900913</entry>
			<entry key="GWC.cacheFormats">image/png,image/jpeg</entry>
		  </metadata>
		  <attribution>
			<logoWidth>0</logoWidth>
			<logoHeight>0</logoHeight>
		  </attribution>
		</layer>

   .. note:: 
   
      When the layer is created a default style named ``point`` is assigned to it.

#. Apply the existing ``landmarks`` style to the layer created ``myworkspace:pointlands`` (this operation does not overwrite the entire layer definition, updates it instead)::

    curl -u geosolutions:Geos -XPUT -H 'Content-type: text/xml' -d '<layer><defaultStyle><name>landmarks</name></defaultStyle><enabled>true</enabled></layer>' http://localhost:8083/geoserver/rest/layers/myworkspace:pointlands

#. Go to the **Layer Preview** to show the layers with the new ``landmarks`` style.

   .. figure:: img/shpchanging2.png

      Viewing the layers with the new created style ``landmarks``
