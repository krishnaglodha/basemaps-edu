.. module:: geoserver.wind_barbs

.. _geoserver.wind_barbs:

Rendering Transformations: Wind Barbs
=====================================
In this section we will use a Coverage View of two wind layers from the store `windGrib` and we will apply a Rendering Transformation which will dynamically draw wind barbs (some notes about wind barbs meaning are provided in the next section).

The final CoverageView will have two bands, each of them representing the wind value as the *u* and *v* components; the wind barbs
will be calculated from a combination of them. 

Coverage View configuration
---------------------------

#. Go to `Layers > Add a new layer`, select *geosolutions:windGrib* and click on :guilabel:`Configure a new Coverage view`.

	.. figure:: img/rtx_barbs_001.png

#. Add the two bands and save them with the name *windView*

	.. figure:: img/rtx_barbs_002.png

#. In the Layer configuration page, inside the :guilabel:`Coverage Band Details` section,  change the Band names to *u* and *v*.

	.. figure:: img/rtx_barbs_003.png

#. Click on Save button.

#. Execute the following WMS request::

	http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:windView&styles=gray&bbox=-180,-90,180,90&width=658&height=330&srs=EPSG:4326&format=image/png

#. The result should be similar to this

	.. figure:: img/rtx_barbs_004.png
		
However this kind of map doesn't allow to have an understandable representation of wind entity. Wind barbs are more appropriate ways to represent winds.

Wind Barbs definition
`````````````````````
Wind Barbs are used for representing wind direction and speed in meteorology. A wind barb always points to the direction on where
the wind comes from. Its shape can change in relation to the speed magnitude:

	* a circle represents 0 or a value lower than 3 knots
	* an half barb 5 knots
	* a full barb 10 knots
	* a flag 50 knots

Also a wind barb have a different versus based on the hemisphere where is located.

Wind barbs may be drawn through a Rendering Transformation.

.. note:: A **knot** is a unit of speed equal to one nautical mile (1.852 km) per hour, approximately 1.151 mph. Although
         not fitting the SI system, its retention for nautical and aviation use is important because of practical matters
         when preparing/reading nautical charts. For reference, 1 knot = 0.514444444 m/s, 3 knots = 1.543333332 m/s

Rendering Transformation
------------------------

Style description
`````````````````

Create a style following this example for the wind barbs and call it *windbarbs*:

.. code-block:: xml

   <StyledLayerDescriptor version="1.0.0"
     xmlns="http://www.opengis.net/sld" xmlns:gml="http://www.opengis.net/gml"
     xmlns:ogc="http://www.opengis.net/ogc" xmlns:xlink="http://www.w3.org/1999/xlink"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.opengis.net/sld ./StyledLayerDescriptor.xsd">
      <NamedLayer>
         <Name>windbarbs</Name>
         <UserStyle>
            <Title>windbarbs</Title>
            <FeatureTypeStyle>
               <Transformation>
                  <ogc:Function name="ras:RasterAsPointCollection">
                     <ogc:Function name="parameter">
                        <ogc:Literal>data</ogc:Literal>
                     </ogc:Function>
                     <!-- Activate the logic to recognize the emisphere -->
                     <ogc:Function name="parameter">
                       <ogc:Literal>emisphere</ogc:Literal>
                       <ogc:Literal>True</ogc:Literal>
                     </ogc:Function>                          
                     <ogc:Function name="parameter">
                       <ogc:Literal>interpolation</ogc:Literal>
                       <ogc:Literal>InterpolationBilinear</ogc:Literal>
                     </ogc:Function>                         
                     <ogc:Function name="parameter">
                       <ogc:Literal>scale</ogc:Literal>
                       <ogc:Function name="Categorize">
                        <ogc:Function name="env">
                          <ogc:Literal>wms_scale_denominator</ogc:Literal>
                        </ogc:Function>
                          <ogc:Literal>16</ogc:Literal>
                          <ogc:Literal>100000</ogc:Literal>
                          <ogc:Literal>8</ogc:Literal>
                          <ogc:Literal>500000</ogc:Literal>    
                          <ogc:Literal>2</ogc:Literal>
                          <ogc:Literal>1000000</ogc:Literal>     
                          <ogc:Literal>0.5</ogc:Literal>
                          <ogc:Literal>5000000</ogc:Literal>     
                          <ogc:Literal>0.2</ogc:Literal>
                          <ogc:Literal>10000000</ogc:Literal>     
                          <ogc:Literal>0.1</ogc:Literal>
                          <ogc:Literal>20000000</ogc:Literal>     
                          <ogc:Literal>0.05</ogc:Literal>   
                          <ogc:Literal>60000000</ogc:Literal>     
                          <ogc:Literal>0.02</ogc:Literal>   
                       </ogc:Function>      
                     </ogc:Function>                        
                  </ogc:Function>
               </Transformation>                 
               <Rule>
                  <PointSymbolizer>
                     <Graphic>
                        <Mark>
                          <WellKnownName>windbarbs://default(
                           <ogc:Function name="sqrt">
                             <ogc:Add>
                                 <ogc:Mul>
                                   <ogc:PropertyName>u</ogc:PropertyName>
                                   <ogc:PropertyName>u</ogc:PropertyName>
                                 </ogc:Mul>
                                 <ogc:Mul>
                                   <ogc:PropertyName>v</ogc:PropertyName>
                                   <ogc:PropertyName>v</ogc:PropertyName>
                                 </ogc:Mul>
                             </ogc:Add>
                            </ogc:Function>)[m/s]?emisphere= 
                           <ogc:PropertyName>emisphere</ogc:PropertyName>
                          </WellKnownName>
                             <Stroke>
                                <CssParameter name="stroke">000000</CssParameter>
                                <CssParameter name="stroke-width">1</CssParameter>
                             </Stroke>   
                          </Mark>
                          <Size>
                           <ogc:Function name="Categorize">
                                 <!-- Value to transform -->
                                <ogc:Function name="sqrt">
                                 <ogc:Add>
                                   <ogc:Mul>
                                    <ogc:PropertyName>u</ogc:PropertyName>
                                    <ogc:PropertyName>u</ogc:PropertyName>
                                   </ogc:Mul>
                                   <ogc:Mul>
                                    <ogc:PropertyName>v</ogc:PropertyName>
                                    <ogc:PropertyName>v</ogc:PropertyName>
                                   </ogc:Mul>
                                 </ogc:Add>
                                </ogc:Function>
                                 <ogc:Literal>8</ogc:Literal>
                                 <ogc:Literal>1.543333332</ogc:Literal>
                                 <ogc:Literal>32</ogc:Literal>          
                              </ogc:Function>                            
                          </Size>
                          <Rotation>
                             <ogc:Function name="Categorize">
                                  <!-- Value to transform -->
                                  <ogc:Function name="sqrt">
                                   <ogc:Add>
                                       <ogc:Mul>
                                         <ogc:PropertyName>u</ogc:PropertyName>
                                         <ogc:PropertyName>u</ogc:PropertyName>
                                       </ogc:Mul>
                                       <ogc:Mul>
                                         <ogc:PropertyName>v</ogc:PropertyName>
                                         <ogc:PropertyName>v</ogc:PropertyName>
                                       </ogc:Mul>
                                   </ogc:Add>
                                 </ogc:Function>
                           
                                  <!-- Output values and thresholds -->
                                  <ogc:Literal>0</ogc:Literal>
                                  <ogc:Literal>1.543333332</ogc:Literal>
                                   <ogc:Function name="toDegrees">
                               <ogc:Function name="atan2">
                               <ogc:PropertyName>u</ogc:PropertyName>
                               <ogc:PropertyName>v</ogc:PropertyName>
                            </ogc:Function>
                             </ogc:Function>                    
                           </ogc:Function>                           
                        </Rotation>
                     </Graphic>
                  </PointSymbolizer>
                  <PointSymbolizer>
                     <Graphic>
                        <Mark>
                          <WellKnownName>circle</WellKnownName>
                           <Fill>
                              <CssParameter name="fill">
                                 <ogc:Literal>#ff0000</ogc:Literal>
                              </CssParameter>
                           </Fill>
                        </Mark>
                        <Size>3</Size>
                     </Graphic>
                  </PointSymbolizer>                    
               </Rule>
            </FeatureTypeStyle>
         </UserStyle>
      </NamedLayer>
   </StyledLayerDescriptor>
	
Investigating more deeply into the XML, we can found a few interesting blocks. 

.. code-block:: xml

   <Transformation>
      <ogc:Function name="ras:RasterAsPointCollection">
         <ogc:Function name="parameter">
            <ogc:Literal>data</ogc:Literal>
         </ogc:Function>
         <!-- Activate the logic to recognize the emisphere -->
         <ogc:Function name="parameter">
           <ogc:Literal>emisphere</ogc:Literal>
           <ogc:Literal>True</ogc:Literal>
         </ogc:Function>                          
         <ogc:Function name="parameter">
           <ogc:Literal>interpolation</ogc:Literal>
           <ogc:Literal>InterpolationBilinear</ogc:Literal>
         </ogc:Function>                         
         <ogc:Function name="parameter">
           <ogc:Literal>scale</ogc:Literal>
           <ogc:Function name="Categorize">
            <ogc:Function name="env">
              <ogc:Literal>wms_scale_denominator</ogc:Literal>
            </ogc:Function>
              <ogc:Literal>16</ogc:Literal>
              <ogc:Literal>100000</ogc:Literal>
              <ogc:Literal>8</ogc:Literal>
              <ogc:Literal>500000</ogc:Literal>    
              <ogc:Literal>2</ogc:Literal>
              <ogc:Literal>1000000</ogc:Literal>     
              <ogc:Literal>0.5</ogc:Literal>
              <ogc:Literal>5000000</ogc:Literal>     
              <ogc:Literal>0.2</ogc:Literal>
              <ogc:Literal>10000000</ogc:Literal>     
              <ogc:Literal>0.1</ogc:Literal>
              <ogc:Literal>20000000</ogc:Literal>     
              <ogc:Literal>0.05</ogc:Literal>   
              <ogc:Literal>60000000</ogc:Literal>     
              <ogc:Literal>0.02</ogc:Literal>   
           </ogc:Function>      
         </ogc:Function>                        
      </ogc:Function>
   </Transformation> 
	
This block contains the operation which converts the input raster into a vector of points, scaling them if necessary.
From this code we can see the following parameters:

	* *emisphere*: set to true, which indicates that the hemisphere must be taken into account when writing barbs.
	* *interpolation*: is set to bilinear.
	* *scale*: factor changes depending on the input `wms_scale_denominator` factor. This will allow performing subsampling/oversampling in order to avoid a windbarbs for each raster pixel which may result in having too many barbs partially overlapping, making the map "unreadable".
	
	
.. code-block:: xml

   <WellKnownName>windbarbs://default(
      <ogc:Function name="sqrt">
        <ogc:Add>
            <ogc:Mul>
              <ogc:PropertyName>u</ogc:PropertyName>
              <ogc:PropertyName>u</ogc:PropertyName>
            </ogc:Mul>
            <ogc:Mul>
              <ogc:PropertyName>v</ogc:PropertyName>
              <ogc:PropertyName>v</ogc:PropertyName>
            </ogc:Mul>
        </ogc:Add>
       </ogc:Function>)[m/s]?emisphere= 
      <ogc:PropertyName>emisphere</ogc:PropertyName>
     </WellKnownName>
	  
The above builds the full name of a specific wind barb, using the following syntax::

	windbarbs://$(value)[m/s]?emisphere=(n/s)
	
Where *$(value)* indicates the wind speed and *$(n/s)* indicates the point emisphere. It should be noted that the wind speed is calculated
as the square root of the *u* and *v* squared values.
	
	
.. code-block:: xml

   <Size>
      <ogc:Function name="Categorize">
         <!-- Value to transform -->
        <ogc:Function name="sqrt">
         <ogc:Add>
           <ogc:Mul>
            <ogc:PropertyName>u</ogc:PropertyName>
            <ogc:PropertyName>u</ogc:PropertyName>
           </ogc:Mul>
           <ogc:Mul>
            <ogc:PropertyName>v</ogc:PropertyName>
            <ogc:PropertyName>v</ogc:PropertyName>
           </ogc:Mul>
         </ogc:Add>
        </ogc:Function>
         <ogc:Literal>8</ogc:Literal>
         <ogc:Literal>1.543333332</ogc:Literal>
         <ogc:Literal>32</ogc:Literal>          
      </ogc:Function>                            
   </Size>

The following XML code is used for calculating the dimension of each point. The Categorize function is used for defining the dimensions of the point
depending on the wind speed: if the speed is less than 1.543333332, then the point dimension is 8; else the dimension in 32.

.. code-block:: xml

   <Rotation>
      <ogc:Function name="Categorize">
             <!-- Value to transform -->
             <ogc:Function name="sqrt">
              <ogc:Add>
                  <ogc:Mul>
                    <ogc:PropertyName>u</ogc:PropertyName>
                    <ogc:PropertyName>u</ogc:PropertyName>
                  </ogc:Mul>
                  <ogc:Mul>
                    <ogc:PropertyName>v</ogc:PropertyName>
                    <ogc:PropertyName>v</ogc:PropertyName>
                  </ogc:Mul>
              </ogc:Add>
            </ogc:Function>
             <!-- Output values and thresholds -->
             <ogc:Literal>0</ogc:Literal>
             <ogc:Literal>1.543333332</ogc:Literal>
              <ogc:Function name="toDegrees">
          <ogc:Function name="atan2">
          <ogc:PropertyName>u</ogc:PropertyName>
          <ogc:PropertyName>v</ogc:PropertyName>
         </ogc:Function>
        </ogc:Function>                    
      </ogc:Function>                           
   </Rotation>

This block indicates the rotation to set for each point vector, depending on the wind speed. If the wind speed is less than 1.543333332,
then the rotation is 0, else it is calculated with the *atan2* function.
	
.. code-block:: xml	

   <PointSymbolizer>
      <Graphic>
         <Mark>
           <WellKnownName>circle</WellKnownName>
            <Fill>
               <CssParameter name="fill">
                  <ogc:Literal>#ff0000</ogc:Literal>
               </CssParameter>
            </Fill>
         </Mark>
         <Size>3</Size>
      </Graphic>
   </PointSymbolizer>    
	
This last block is used for representing the anchor point for each wind barb.

WMS Request
```````````

Execute the following WMS request in order to see the result of the application of such style::

	http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:windView&styles=windbarbs&bbox=-180,-90,180,90&width=658&height=330&srs=EPSG:4326&format=image/png

.. note:: We have saved the wind barbs style with the name *windbarbs*, if you set another name for it, you must change the `&styles=windbarbs` label with `&styles=NAME_OF_THE_STYLE`
	
And the result should be this:

	.. figure:: img/rtx_barbs_005.png
	
As you may see, the wind barbs direction and magnitude seems to be random; this is due to the high subsampling of the input raster data while getting a preview of the winds across the whole world. Selecting a smaller region, provides a nicer view. Execute this zoomed-in request and see the results:

	.. code-block:: xml

		http://localhost:8083/geoserver/geosolutions/wms?LAYERS=geosolutions:windView&STYLES=windbarbs&FORMAT=image/png&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&SRS=EPSG:4326&BBOX=-64.57763671875,35.518798828125,-35.66162109375,50.020751953125&WIDTH=658&HEIGHT=330

	.. figure:: img/rtx_barbs_006.png

