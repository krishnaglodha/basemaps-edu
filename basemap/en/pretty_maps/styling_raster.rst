  .. _geoserver.styling_raster:


Styling Raster data
-------------------

In the previous section we have created and optimized some vector styles. In this section we will deal with a styled SRTM raster and we will see how to get a better visualization of that layer by adding hillshade.

#. From the `Welcome Page <http://localhost:8083/geoserver>`_ navigate to :menuselection:`Layer Preview` and select the OpenLayers link for the ``geosolutions:srtm`` layer.

   .. figure:: img/raster_srtm.png

      SRTM rendering with DEM style

   There is a DEM style associated to that SRTM dataset layer resulting in such a colored rendering.

#. Return to the GeoServer `Welcome Page`, select the :menuselection:`Styles` and click the ``dem`` style to see which color map is applied.

   .. note:: You have to be logged in as Administrator in order to edit/check styles.

   .. figure:: img/raster_dem_style.png

      Style editing

   Note the entries with ``opacity = 0.0`` which allow to make no data values as transparent.

The current DEM style allows to get a pleasant rendering of the SRTM dataset but we can get better results by combining it with a hillshade layer which will be created through another GDAL utility (gdaldem).

.. note:: The CSS equivalent of this style is the following

  .. code-block:: css
  
    [@scale > 75000] {
      raster-channels: auto;
      raster-color-map:
        color-map-entry(#00BFBF, -100.0, 0)
        color-map-entry(#00FF00, 920.0, 0)
        color-map-entry(#00FF00, 920.0, 1.0)
        color-map-entry(#FFFF00, 1940.0, 1.0)
        color-map-entry(#FFFF00, 1940.0, 1.0)
        color-map-entry(#FF7F00, 2960.0, 1.0)
        color-map-entry(#FF7F00, 2960.0, 1.0)
        color-map-entry(#BF7F3F, 3980.0, 1.0)
        color-map-entry(#BF7F3F, 3980.0, 1.0)
        color-map-entry(#141514, 5000.0, 1.0);
    }

Adding hillshade
^^^^^^^^^^^^^^^^

#. If on Linux, open a shell and run::

    gdaldem hillshade -z 5 -s 111120 ${TRAINING_ROOT}/geoserver_data/data/boulder/srtm_boulder.tiff ${TRAINING_ROOT}/geoserver_data/data/boulder/srtm_boulder_hs.tiff -co tiled=yes
     
#. If on Windows instead, open the ``TRAINING_ROOT/gdal`` folder, launch the ``SDKShell.bat`` file and run::

    gdaldem hillshade -z 5 -s 111120 %TRAINING_ROOT%\geoserver_data\data\boulder\srtm_boulder.tiff %TRAINING_ROOT%\geoserver_data\data\boulder\srtm_boulder_hs.tiff -co tiled=yes

   .. note:: The ``z`` parameter exaggerates the elevation, the ``s`` parameter provides the ratio between the elevation units and the ground units (degrees in this case), ``-co tiled=yes`` makes gdaldem generate a TIFF with inner tiling. We'll investigate this last option better in the following pages.

#. From the `Welcome Page <http://localhost:8083/geoserver>`_ navigate to :menuselection:`Styles` and select `Add a new style` as previously seen in the :ref:`Adding a style <geoserver.add_style>` section.

#. In the :guilabel:`SLD Editor` enter the following XML:

   .. code-block:: xml
   
    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
      <sld:UserLayer>
        <sld:LayerFeatureConstraints>
          <sld:FeatureTypeConstraint/>
        </sld:LayerFeatureConstraints>
        <sld:UserStyle>
          <sld:Title/>
          <sld:FeatureTypeStyle>
            <sld:Name>name</sld:Name>
            <sld:FeatureTypeName>Feature</sld:FeatureTypeName>
            <sld:Rule>
              <sld:MinScaleDenominator>75000</sld:MinScaleDenominator>
              <sld:RasterSymbolizer>
                <sld:Geometry>
                  <ogc:PropertyName>grid</ogc:PropertyName>
                </sld:Geometry>
                <sld:ColorMap>
                  <sld:ColorMapEntry color="#000000" opacity="0.0" quantity="0.0"/>
                  <sld:ColorMapEntry color="#999999" quantity="1.0"/>
                  <sld:ColorMapEntry color="#FFFFFF" quantity="256.0"/>
                </sld:ColorMap>
              </sld:RasterSymbolizer>
            </sld:Rule>
            <sld:VendorOption name="composite">multiply</sld:VendorOption>
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>

   .. note:: The CSS equivalent of this style is the following

      .. code-block:: css

        [@scale > 75000] {
          raster-channels: auto;
          raster-color-map:
            color-map-entry(#000000, 0, 0)
            color-map-entry(#999999, 1)
            color-map-entry(#FFFFFF, 256);
          composite: 'multiply';
        }

   .. note:: Note the "composite" vendor option used to have the hillshading use a special color blending mode to get a better visual result compared to simple translucency

#. Set :file:`hillshade` as name and then click the :guilabel:`Submit` button.

#. Select :guilabel:`Add stores` from the GeoServer `Welcome Page` to add the previously created ``hillshade`` raster.

#. Select :guilabel:`GeoTIFF - Tagged Image File Format with Geographic information` from the set of available Raster Data Sources. 

#. Specify :file:`hillshade` as name in the :guilabel:`Data Source Name` field of the interface.

#. Click on  :guilabel:`browse` link in order to set the GeoTIFF location in the :guilabel:`URL` field.

   .. note:: make sure to specify the :file:`srtm_boulder_hs.tiff` previously created with gdaldem, which should be located at :file:`${TRAINING_ROOT}/geoserver_data/data/boulder` ( :file:`%TRAINING_ROOT%\\geoserver_data\\data\\boulder` on Windows )

#. Click :guilabel:`Save`. 

#. Publish the layer by clicking on the :guilabel:`publish` link. 

   .. figure:: img/raster_hillshade.png
         
      Publishing Raster Layer

#. Set :file:`hillshade` as the name

#. Switch to `Publishing` tab

   .. figure:: img/raster_hillshade_publishing.png

      Raster Layer - Publishing tab

#. Make sure to set the default style to ``hillshade`` on the `Publishing --> Default Style` section.

   .. figure:: img/raster_hillshade_defaultstyle.png
         
      Editing Raster Publishing info

#. Click :guilabel:`Save` to create the new layer.

#. Use the **Layer Preview** to preview the new layer with the hillshade style.
   
   .. figure:: img/raster_hillshade_preview.png

      Previewing the new raster layer with the hillshade style applied

#. Edit the Layer Preview URL in your browser by locating the `layers` parameter

    .. figure:: img/raster_overlay_url.png

       Edition of URL

#. Insert the `geosolutions:srtm,` additional layer (note the final comma) before the `geosolutions:hillshade` one:

    .. figure:: img/raster_overlay_2layers.png
       
       Edition of URL - Additional layer on list

#. Press Enter to send the updated request. The Layer Preview should change like this where you can see both the srtm and hillshade layers.

    .. figure:: img/raster_overlay.png

       Layer preview with srtm and hillshade being overlaid
