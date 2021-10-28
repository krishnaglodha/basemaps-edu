.. module:: geoserver.height

.. _geoserver.height:

Generating a KML Elevation
--------------------------

This section covers how to use GeoServer templates in conjunction with Google Earth support for 'height' to create a height visualizations.

#. Create the :file:`height.ftl` file in :file:`%TRAINING_ROOT%\\geoserver_data\\workspaces\\geosolutions\\states\\states\\` directory

#. Open the :file:`height.ftl` with a text editor and enter the following content::

   ${PERSONS.value?number / 100.0}

#. Save the :file:`height.ftl` and  navigate to the `GeoServer Layers Preview <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.web.demo.MapPreviewPage>`_.

#. Click the :guilabel:`KML` link next to the :guilabel:`geosolutions:states` layer.

   .. figure:: img/height2.png

     Generating KML with the map preview
#. In the resulting dialog choose to :guilabel:`Open with Google Earth` and :guilabel:`OK` (or double click on downloaded KML file)

   .. figure:: img/height3.png

      Opening KML from GeoServer with Google Earth


#. Zoom in until states show up in vector form, and then tilt the view to see them extruded.

   .. figure:: img/height1.png

      Viewing the States population layer in Google Earth
