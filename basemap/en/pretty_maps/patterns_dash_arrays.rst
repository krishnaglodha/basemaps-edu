.. _geoserver.patterns_dash_arrays:


Patterns and Hatches
--------------------

#. Go and edit the configuration of the ``bplandmarks`` layer, enter the "Publishing" tab and associate the ``cemetery_mark`` and ``cemetery_graphics`` styles as "Additional styles" for the layer, then press "Save"

   .. figure:: img/sld_create0.png

#. Navigate to :menuselection:`Styles` list.

#. Select "cemetery_graphics" from the list

   .. figure:: img/sld_create1.png
         
      Patterns filling SLD

#. In the :guilabel:`SLD Editor` you will see the following XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor
    xmlns="http://www.opengis.net/sld"
    xmlns:sld="http://www.opengis.net/sld"
    xmlns:ogc="http://www.opengis.net/ogc"
    xmlns:gml="http://www.opengis.net/gml"
    xmlns:xlink="http://www.w3.org/1999/xlink" version="1.0.0">
      <sld:UserLayer>
        <sld:UserStyle>
          <sld:Name>tl 2010 08013 arealm</sld:Name>
          <sld:Title/>
          <sld:FeatureTypeStyle>
            <sld:Rule>
              <sld:Name>cemeteries</sld:Name>
              <ogc:Filter>
                <ogc:PropertyIsEqualTo>
                  <ogc:PropertyName>MTFCC</ogc:PropertyName>
                  <ogc:Literal>K2582</ogc:Literal>
                </ogc:PropertyIsEqualTo>
              </ogc:Filter>
              <sld:MaxScaleDenominator>500000.0</sld:MaxScaleDenominator>
              <sld:PolygonSymbolizer>
                <sld:Fill>
                  <sld:GraphicFill>
                    <sld:Graphic>
                      <sld:ExternalGraphic>
                        <sld:OnlineResource
                        xlink:type="simple"
                        xlink:href="./img/landmarks/area/grave_yard.png" />
                        <sld:Format>image/png</sld:Format>
                      </sld:ExternalGraphic>
                    </sld:Graphic>
                  </sld:GraphicFill>
                </sld:Fill>
              </sld:PolygonSymbolizer>
            </sld:Rule>
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>

   .. note:: The above SLD defines a ``<PolygonSymbolizer>`` with a ``<GraphicFill>`` pointing to a png *./img/landmarks/area/grave_yard.png* in the GeoServer data directory, which will be used by GeoServer as pattern to fill the polygon.


   .. note:: The CSS equivalent of this style is the following
        
        .. code-block:: css
            
            [MTFCC='K2582'] [@scale < 500000] {
                fill: url(./img/landmarks/area/grave_yard.png);
                fill-mime: 'image/png';
            }
            


#. If you click the "Preview Legend" button, a preview of the legend for this style will be displayed.


   .. figure:: img/previewlegend.png
 		  
      The legend preview of the style

#. In the `Layer Preview <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.web.demo.MapPreviewPage>`__ , select the bplandmarks layer, and open it. The style will appear as shown in the following image:

   .. figure:: img/sld_create2.png
 		  
      Filling with patterns


#. Like before, select now "cemetery_mark" from the list of Styles

   .. figure:: img/sld_create1b.png
         
      True Type Font filling SLD

#. In the :guilabel:`SLD Editor` you will see the following XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor
    xmlns="http://www.opengis.net/sld"
    xmlns:sld="http://www.opengis.net/sld"
    xmlns:ogc="http://www.opengis.net/ogc"
    xmlns:gml="http://www.opengis.net/gml"
    xmlns:xlink="http://www.w3.org/1999/xlink" version="1.0.0">
      
      <sld:UserLayer>
        <sld:Name>cemeteries</sld:Name>
        <sld:UserStyle>
          <sld:Name>tl 2010 08013 arealm</sld:Name>
          <sld:Title/>
          <sld:FeatureTypeStyle>

          
            <sld:Rule>
              <sld:Name>cemeteries</sld:Name>
              <ogc:Filter>
                <ogc:PropertyIsEqualTo>
                  <ogc:PropertyName>MTFCC</ogc:PropertyName>
                  <ogc:Literal>K2582</ogc:Literal>
                </ogc:PropertyIsEqualTo>
              </ogc:Filter>
              <sld:MaxScaleDenominator>500000.0</sld:MaxScaleDenominator>
              <sld:PolygonSymbolizer>
                <sld:Fill>
                  <sld:CssParameter name="fill">#D3FFD3</sld:CssParameter>
                  <sld:CssParameter name="fill-opacity">0.5</sld:CssParameter>              
                </sld:Fill>
                <sld:Stroke>
                  <sld:CssParameter name="stroke">#6DB26D</sld:CssParameter>
                </sld:Stroke>
              </sld:PolygonSymbolizer>
              <sld:PolygonSymbolizer>
                <sld:Fill>
                  <sld:GraphicFill>
                    <sld:Graphic>
                      <sld:Mark>
                        <sld:WellKnownName>ttf://Wingdings#0x0055</sld:WellKnownName>
                        <sld:Stroke>
                        <sld:CssParameter name="stroke">#6DB26D</sld:CssParameter>
                        </sld:Stroke>
                      </sld:Mark>
                      <sld:Size>16</sld:Size>
                    </sld:Graphic>
                  </sld:GraphicFill>
                </sld:Fill>
                <sld:VendorOption name="graphic-margin">8</sld:VendorOption>
              </sld:PolygonSymbolizer>
              
            </sld:Rule>
      
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>


   .. note:: The CSS equivalent of this style is the following
        
        .. code-block:: css
            
            [MTFCC='K2582'] [@scale < 500000] {
              fill: #D3FFD3, symbol('ttf://Wingdings#0x0055');
              fill-size: 16;
              fill-opacity: 0.5, 1.0;
              stroke: #6DB26D;
              -gt-graphic-margin: 8;
              :nth-symbol(2) {
                stroke: #6DB26D;
              }
            }
            


   .. note:: The above SLD defines a ``<PolygonSymbolizer>`` with a ``<GraphicFill>`` looking for a specific *Windings* character which will be used by GeoServer as pattern to fill the polygon. The ``graphic-margin`` ``VendorOption`` is used to add some space around symbols.
    
#. In this case, if you open the layer in the `Layer Preview <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.web.demo.MapPreviewPage>`__ the style will appear as the following image:

   .. figure:: img/sld_create2b.png
 		  
      Filling with TTF fonts


#. Lets now take a look at another way to fill polygons using patterns, the *Hatches*. From the `Welcome Page <http://localhost:8083/geoserver>`_ navigate to :menuselection:`Styles` and select "wetlands" from the list.

   .. note:: You may switch to the second page in order to find the style.

   .. figure:: img/sld_create5.png
         
      Wetlands style with some hatches

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
      <sld:UserLayer>
        <sld:LayerFeatureConstraints>
          <sld:FeatureTypeConstraint/>
        </sld:LayerFeatureConstraints>
        <sld:UserStyle>
          <sld:Name>Wetlands regulatory area</sld:Name>
          <sld:Title/>
          <sld:FeatureTypeStyle>
            <sld:Rule>
              <sld:Name>default rule</sld:Name>
              <sld:MaxScaleDenominator>10000.0</sld:MaxScaleDenominator>
              <sld:PolygonSymbolizer>
                <sld:Fill>
                  <sld:GraphicFill>
                    <sld:Graphic>
                      <sld:Mark>
                        <sld:WellKnownName>shape://times</sld:WellKnownName>
                        <sld:Fill/>
                        <sld:Stroke>
                          <sld:CssParameter name="stroke">#ADD8E6</sld:CssParameter>
                          <sld:CssParameter name="stroke-width">1.0</sld:CssParameter>
                        </sld:Stroke>
                      </sld:Mark>
                      <sld:Size>
                        <ogc:Literal>8.0</ogc:Literal>
                      </sld:Size>
                    </sld:Graphic>
                  </sld:GraphicFill>
                  <!--
                  <sld:CssParameter name="fill">#7CE3F8</sld:CssParameter>
                  <sld:CssParameter name="fill-opacity">0.5</sld:CssParameter>
                  -->
                </sld:Fill>
              </sld:PolygonSymbolizer>
            </sld:Rule>
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>


   .. note:: The CSS equivalent of this style is the following
        
        .. code-block:: css
            
            [@scale < 10000] {
              fill: symbol('shape://times');
              fill-size: 8;
              :nth-symbol(1) {
                stroke: #ADD8E6;
                stroke-width: 1.0;
              };
            }
            


   
#. Comment out the following line in order to see the polygons at lower zoom levels too:

   .. code-block:: xml

    <!-- sld:MaxScaleDenominator>10000.0</sld:MaxScaleDenominator -->

#. Click :guilabel:`Save` to save the updated SLD.

#. To see how the styles work, make sure the default style of the :guilabel:`Wetlands_regulatory_area` feature type is set to :guilabel:`wetlands`.

   .. figure:: img/sld_create6.png
 		  
      Changing the default style of the :guilabel:`Wetlands_regulatory_area` feature type to *wetlands*

#. Use the `Layer Preview <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.web.demo.MapPreviewPage>`__ to preview the new style.
   
   .. figure:: img/sld_create7.png

      Previewing the :guilabel:`Wetlands_regulatory_area` layer with the hatches applied

#. On the previous example we used *times* as hatches mark. GeoServer makes available different kinds of hatches marks:

   .. figure:: img/sld_create7a.png
 		  
      Different types of hatches marks.

Dashes
^^^^^^

#. Lets now familiarize a bit with *Dashes*. We are going to see how it's possible to draw several kind of dashes to represent different types of trails or roads. 

#. From the `Welcome Page <http://localhost:8083/geoserver>`_ navigate to :menuselection:`Styles`.

   .. note:: You have to be logged in as Administrator in order to activate this function.

#. Select "trails" from the list

   .. figure:: img/sld_create8.png
         
      Dashes SLD

#. In the :guilabel:`SLD Editor` you will see the following XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
      <sld:UserLayer>
        <sld:LayerFeatureConstraints>
          <sld:FeatureTypeConstraint/>
        </sld:LayerFeatureConstraints>
        <sld:UserStyle>
          <sld:Name>Trails</sld:Name>
          <sld:Title/>
          <sld:FeatureTypeStyle>
            <sld:Rule>
              <sld:MaxScaleDenominator>75000</sld:MaxScaleDenominator>
              <sld:LineSymbolizer>
                <sld:Stroke>
                  <sld:CssParameter name="stroke">#6B4900</sld:CssParameter>
                  <sld:CssParameter name="stroke-width">0.1</sld:CssParameter>
                  <sld:CssParameter name="stroke-dasharray">2.0 </sld:CssParameter>
                </sld:Stroke>
              </sld:LineSymbolizer>
            </sld:Rule>
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>


   .. note:: The CSS equivalent of this style is the following
        
        .. code-block:: css
            
            [@scale < 75000] {
              stroke: #6B4900;
              stroke-width: 0.1;
              stroke-dasharray: 2.0;
            }
            


   .. figure:: img/sld_create8a.png
 		  
      Simple dash-array

   .. note:: The above SLD defines a ``<LineSymbolizer>`` with a ``<Stroke>`` using the CSS property *stroke-dasharray* to represent the trails like a simple gray dash.
   
   .. note:: Encodes a dash pattern as a series of numbers separated by spaces. Odd-indexed numbers (first, third, etc) determine the length in pxiels to draw the line, and even-indexed numbers (second, fourth, etc) determine the length in pixels to blank out the line. Default is an unbroken line. Starting from version 2.1 dash arrays can be combined with graphic strokes to generate complex line styles with alternating symbols or a mix of lines and symbols.

#. The Style above is the default one for the layer :guilabel:`geosolutions:Trails`. Lets have a look at a bit more complex example. From the `Welcome Page <http://localhost:8083/geoserver>`_ navigate to :menuselection:`Styles` and select "trails2" from the list

   .. figure:: img/sld_create8b.png
         
      Trails2 Style

#. In the :guilabel:`SLD Editor` you will see the following XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
      <sld:UserLayer>
        <sld:LayerFeatureConstraints>
          <sld:FeatureTypeConstraint/>
        </sld:LayerFeatureConstraints>
        <sld:UserStyle>
          <sld:Name>Trails</sld:Name>
          <sld:Title/>
          <sld:FeatureTypeStyle>
            <sld:Rule>
              <sld:MaxScaleDenominator>75000</sld:MaxScaleDenominator>
              <sld:LineSymbolizer>
                <sld:Stroke>
                  <sld:GraphicStroke>
                    <sld:Graphic>
                      <sld:Mark>
                        <sld:WellKnownName>circle</sld:WellKnownName>
                        <sld:Fill>
                          <sld:CssParameter name="fill">#AA0000</sld:CssParameter>
                        </sld:Fill>
                      </sld:Mark>
                      <sld:Size>
                        <ogc:Literal>6</ogc:Literal>
                      </sld:Size>
                    </sld:Graphic>
                  </sld:GraphicStroke>
                  <sld:CssParameter name="stroke-dasharray">6 18</sld:CssParameter>
                </sld:Stroke>
              </sld:LineSymbolizer>
              <sld:LineSymbolizer>
                <sld:Stroke>
                  <sld:CssParameter name="stroke">#AA0000</sld:CssParameter>
                  <sld:CssParameter name="stroke-dasharray">10 14</sld:CssParameter>
                  <sld:CssParameter name="stroke-dashoffset">14</sld:CssParameter>
                </sld:Stroke>
              </sld:LineSymbolizer>
            </sld:Rule>
          </sld:FeatureTypeStyle>
        </sld:UserStyle>
      </sld:UserLayer>
    </sld:StyledLayerDescriptor>


   .. note:: The CSS equivalent of this style is the following
        
        .. code-block:: css
            
            [@scale < 75000] {
              stroke: #AA0000, symbol(circle);
              stroke-dasharray: 10 14, 6 18;
              stroke-dashoffset: 14, 0;
              :nth-symbol(2) {
                size: 6;
                fill: #AA0000;
              }
            }

   .. note:: We may notice two interesting things in this style, two ``<LineSymbolizer>`` the first one defining a *circle* Mark with a simple dasharray and the second one a simple stroke defining also a *dashoffset*. The latter specifies the distance in pixels into the dasharray pattern at which to start drawing. Default is 0.

#. Open the :guilabel:`geosolutions:Trails` layers and add *trails2* as an additional style, then go to the :guilabel:`Layer Preview` to see it in action

   .. figure:: img/sld_create8e.png

   .. warning:: You have to zoom in from the layer preview in order to see the lines due to the *MaxScaleDenominator*
