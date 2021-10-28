.. _geoserver.vector_data.wfst:

Modifying Feature Types
-----------------------

GeoServer provides a fully **Transactional Web Feature Service (WFS-T)** which enables users to **insert** / **delete** / **modify** the available FeatureTypes.
This section shows a few of the GeoServer WFS-T capabilities and interactions with desktop GIS clients.

.. warning:: The layer ``Mainrd`` is required for this exercise. Be sure to have completed the :ref:`geoserver.add_shp` section.

.. warning:: 
    
    This section assumes all the layers are open for anyone to modify.
    
    Open the **Security** -> `Data <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.security.web.data.DataSecurityPage>`_ page
    
    Ensure the Rule path **\*.\*.w** is set to be available to any role
    
    Refer to the :ref:`geoserver.layer_level` section for further information

.. warning:: For this exercise, we will use `uDig <http://udig.refractions.net>`_ GIS desktop client. In general, all GIS desktop clients provides same kind of features allowing to reproduce the exercise.
    

#. Open your preferred GIS desktop client. In our case QGIS.

#. Add GeoServer WFS to the catalog. Use the import button in the catalog tab **(1)**, and select "data" **(2)** in the first page of the wizard    

   .. figure:: img/wfs-t1.png

   Select the **Web Feature Service** Data Source
   
   .. figure:: img/wfs-t2.png

   Insert the following address into the URL text box and press ``finish``::
   
     http://localhost:8083/geoserver/wfs?request=GetCapabilities&service=WFS

   .. figure:: img/wfs-t3.png
      
   Right click on the ``Mainrd`` layer from the list and select ``Add to current map``
   
   .. figure:: img/wfs-t4.png

#. The ``Mainrd`` layer is now visible in the uDig map

   .. figure:: img/wfs-t5.png

#. Perform a zoom operation on the upper-right part of the layer.

   .. figure:: img/wfs-t6.png

      Zooming in ...
   
   .. figure:: img/wfs-t7.png

      Zooming in ...

#. By using the ``Select and Edit Geometry`` tool try to move/add/remove some vertex to the small line at the center of the screen.

   .. figure:: img/wfs-t8.png
   
      Playing with the Geometry

#. Once finished use the :guilabel:`Commit` tool to persist the changes on GeoServer.

   .. figure:: img/wfs-t9.png

      Committing changes throught the WFS-T protocol

#. Use GeoServer **Layer Preview** to view the changes on the `Mainrd` layer.
   
   .. figure:: img/wfs-t10.png

      Showing the changes to the `Mainrd` Feature Type

#. On uDig look the Feature attribute values using the ``Info`` tool.

   .. figure:: img/wfs-t11.png
	  
      Retrieving Feature Type info from uDig interface

#. Now open / create the ``request.xml`` file in the training root dir and set in the following request, which will be used to issue an **update** Feature type request to the WFS-T updating all roads labelled as ``Monarch Rd`` to ``Monarch Road``

   .. code-block:: xml

      <wfs:Transaction xmlns:topp="http://www.openplans.org/topp" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wfs="http://www.opengis.net/wfs" service="WFS" version="1.0.0">
        <wfs:Update typeName="geosolutions:Mainrd">
              <wfs:Property>
                <wfs:Name>LABEL_NAME</wfs:Name>
                <wfs:Value>Monarch Road</wfs:Value>
              </wfs:Property>
              <ogc:Filter>
                <ogc:PropertyIsEqualTo>
                    <ogc:PropertyName>LABEL_NAME</ogc:PropertyName>
                    <ogc:Literal>Monarch Rd</ogc:Literal>
                </ogc:PropertyIsEqualTo>
              </ogc:Filter>
        </wfs:Update>
      </wfs:Transaction>

#. Issue the WFS-T request towards GeoServer using curl on the command line::

      curl -XPOST -d @request.xml -H "Content-type: application/xml" "http://localhost:8083/geoserver/ows"

#. The response should be a TransactionResponse XML document containing a ``wfs:SUCCESS`` element

#. Ask the info again using the uDig ``Info`` tool. In some cases you have also to refresh the catalog to see the data changes in uDig.

   .. figure:: img/wfs-t13.png

      Obtaining the updated Feature Type info from uDig interface

#. Finally, obtain the Feature type info using the GetFeatureInfo operation issued directly by the ``Map Preview``.

   .. figure:: img/wfs-t14.png

      Obtaining the updated Feature Type info from OpenLayers MapPreview GetFeatureInfo

.. note:: In order to issue a GetFeatureInfo request from the OpenLayers MapPreview tool, just left-click over the line.

