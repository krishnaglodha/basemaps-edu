.. module:: geoserver.currents

.. _geoserver.currents:

Rendering Transformations: Current Arrows
=========================================
In this section we show a rendering transformation using the Coverage view *currents* created before.

Rendering Transformation
------------------------

Create a new style, copy the XML code below and save it as *currents* :

.. code-block:: xml

   <?xml version="1.0" encoding="ISO-8859-1"?>
   <StyledLayerDescriptor version="1.0.0"
      xsi:schemaLocation="http://www.opengis.net/sld StyledLayerDescriptor.xsd"
      xmlns="http://www.opengis.net/sld"
      xmlns:ogc="http://www.opengis.net/ogc"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
     <NamedLayer>
      <Name>currents</Name>
      <UserStyle>
        <Title>currents</Title>
        <FeatureTypeStyle>
         <Transformation>
            <ogc:Function name="ras:RasterAsPointCollection">
               <ogc:Function name="parameter">
                  <ogc:Literal>data</ogc:Literal>
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
                    <ogc:Literal>8</ogc:Literal>
                    <ogc:Literal>50000</ogc:Literal>
                    <ogc:Literal>4</ogc:Literal>
                    <ogc:Literal>100000</ogc:Literal>
                    <ogc:Literal>2</ogc:Literal>
                    <ogc:Literal>500000</ogc:Literal>    
                    <ogc:Literal>1</ogc:Literal>
                    <ogc:Literal>1000000</ogc:Literal>     
                    <ogc:Literal>0.2</ogc:Literal>
                    <ogc:Literal>5000000</ogc:Literal>     
                    <ogc:Literal>0.1</ogc:Literal>
                    <ogc:Literal>10000000</ogc:Literal>     
                    <ogc:Literal>0.05</ogc:Literal>
                    <ogc:Literal>20000000</ogc:Literal>     
                    <ogc:Literal>0.02</ogc:Literal>   
                 </ogc:Function>      
               </ogc:Function>                        
            </ogc:Function>
         </Transformation>  
         <Rule>
           <Title>Heading</Title>
           <PointSymbolizer>
            <Graphic>
              <Mark>
                <WellKnownName>extshape://narrow</WellKnownName>
                <Fill>
                  <CssParameter name="fill">
                   <ogc:Function name="Categorize">
                     <ogc:Function name="sqrt">
                       <ogc:Add>
                        <ogc:Mul>
                          <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                          <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                        </ogc:Mul>
                        <ogc:Mul>
                          <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                          <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                        </ogc:Mul>
                       </ogc:Add>
                     </ogc:Function>
                     <ogc:Literal>#0000FF</ogc:Literal>
                     <ogc:Literal>0.3</ogc:Literal>
                     <ogc:Literal>#0033FF</ogc:Literal>
                     <ogc:Literal>0.2</ogc:Literal>
                     <ogc:Literal>#0066FF</ogc:Literal>
                     <ogc:Literal>0.1</ogc:Literal>
                     <ogc:Literal>#0099FF</ogc:Literal>
                     <ogc:Literal>0.05</ogc:Literal>                       
                     <ogc:Literal>#00CCFF</ogc:Literal>
                     <ogc:Literal>0.02</ogc:Literal>
                     <ogc:Literal>#00FFFF</ogc:Literal>
                   </ogc:Function>                       
                  </CssParameter>
               </Fill>
              </Mark>
              <Size>
                <ogc:Function name="Categorize">
                   <!-- Value to transform -->
                  <ogc:Function name="sqrt">
                    <ogc:Add>
                     <ogc:Mul>
                       <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                       <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                     </ogc:Mul>
                     <ogc:Mul>
                       <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                       <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                     </ogc:Mul>
                    </ogc:Add>
                  </ogc:Function>
                     <ogc:Literal>4</ogc:Literal>
                     <ogc:Literal>0.1</ogc:Literal>
                     <ogc:Literal>5</ogc:Literal>
                     <ogc:Literal>0.2</ogc:Literal>
                     <ogc:Literal>6</ogc:Literal>
                     <ogc:Literal>0.3</ogc:Literal>
                     <ogc:Literal>8</ogc:Literal>
                     <ogc:Literal>0.4</ogc:Literal>
                     <ogc:Literal>10</ogc:Literal>
                     <ogc:Literal>0.5</ogc:Literal>
                     <ogc:Literal>12</ogc:Literal>
                     <ogc:Literal>0.6</ogc:Literal>
                     <ogc:Literal>14</ogc:Literal>
                     <ogc:Literal>0.7</ogc:Literal>
                     <ogc:Literal>16</ogc:Literal>
                     <ogc:Literal>0.8</ogc:Literal>
                     <ogc:Literal>18</ogc:Literal>
                     <ogc:Literal>0.9</ogc:Literal>
                     <ogc:Literal>20</ogc:Literal>
                     <ogc:Literal>1.0</ogc:Literal>
                     <ogc:Literal>22</ogc:Literal>
                     <ogc:Literal>1.1</ogc:Literal>
                     <ogc:Literal>25</ogc:Literal>
                   </ogc:Function>       
              </Size>
              <Rotation>
                  <ogc:Function name="toDegrees">
                   <ogc:Function name="atan2">
                      <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                      <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                   </ogc:Function>
                  </ogc:Function>                    
              </Rotation>
            </Graphic>
           </PointSymbolizer>
         </Rule>
        </FeatureTypeStyle>
      </UserStyle>
     </NamedLayer>
   </StyledLayerDescriptor>


As you may notice there are several XML blocks, each of them with a precise function.

.. code-block:: xml

   <Transformation>
      <ogc:Function name="ras:RasterAsPointCollection">
         <ogc:Function name="parameter">
            <ogc:Literal>data</ogc:Literal>
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
              <ogc:Literal>8</ogc:Literal>
              <ogc:Literal>50000</ogc:Literal>
              <ogc:Literal>4</ogc:Literal>
              <ogc:Literal>100000</ogc:Literal>
              <ogc:Literal>2</ogc:Literal>
              <ogc:Literal>500000</ogc:Literal>    
              <ogc:Literal>1</ogc:Literal>
              <ogc:Literal>1000000</ogc:Literal>     
              <ogc:Literal>0.2</ogc:Literal>
              <ogc:Literal>5000000</ogc:Literal>     
              <ogc:Literal>0.1</ogc:Literal>
              <ogc:Literal>10000000</ogc:Literal>     
              <ogc:Literal>0.05</ogc:Literal>
              <ogc:Literal>20000000</ogc:Literal>     
              <ogc:Literal>0.02</ogc:Literal>   
           </ogc:Function>      
         </ogc:Function>                        
      </ogc:Function>
   </Transformation> 

This XML block contains the operation which converts the input raster into a vector of points, scaling them if necessary.
From this code we can see the following parameters:

	* *interpolation*: is set to bilinear.
	* *scale*: factor changes depending on the input `wms_scale_denominator` factor.

.. code-block:: xml

   <Mark>
       <WellKnownName>extshape://narrow</WellKnownName>
       <Fill>
         <CssParameter name="fill">
          <ogc:Function name="Categorize">
            <ogc:Function name="sqrt">
              <ogc:Add>
               <ogc:Mul>
                 <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
                 <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
               </ogc:Mul>
               <ogc:Mul>
                 <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
                 <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
               </ogc:Mul>
              </ogc:Add>
            </ogc:Function>
            <ogc:Literal>#0000FF</ogc:Literal>
            <ogc:Literal>0.3</ogc:Literal>
            <ogc:Literal>#0033FF</ogc:Literal>
            <ogc:Literal>0.2</ogc:Literal>
            <ogc:Literal>#0066FF</ogc:Literal>
            <ogc:Literal>0.1</ogc:Literal>
            <ogc:Literal>#0099FF</ogc:Literal>
            <ogc:Literal>0.05</ogc:Literal>                       
            <ogc:Literal>#00CCFF</ogc:Literal>
            <ogc:Literal>0.02</ogc:Literal>
            <ogc:Literal>#00FFFF</ogc:Literal>
          </ogc:Function>                       
         </CssParameter>
      </Fill>
   </Mark>
	
The block above is used for associating an arrow to each current vector point following the WKT definition::

	extshape://narrow

Also for each arrow is defined a colour depending on the current speed. The colours are various shades of blue.
Note that the current speed is calculated as the square root of the squared *u* and *v* components of each current vector point.

.. code-block:: xml

   <Size>
       <ogc:Function name="Categorize">
          <!-- Value to transform -->
         <ogc:Function name="sqrt">
           <ogc:Add>
            <ogc:Mul>
              <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
              <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
            </ogc:Mul>
            <ogc:Mul>
              <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
              <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
            </ogc:Mul>
           </ogc:Add>
         </ogc:Function>
            <ogc:Literal>4</ogc:Literal>
            <ogc:Literal>0.1</ogc:Literal>
            <ogc:Literal>5</ogc:Literal>
            <ogc:Literal>0.2</ogc:Literal>
            <ogc:Literal>6</ogc:Literal>
            <ogc:Literal>0.3</ogc:Literal>
            <ogc:Literal>8</ogc:Literal>
            <ogc:Literal>0.4</ogc:Literal>
            <ogc:Literal>10</ogc:Literal>
            <ogc:Literal>0.5</ogc:Literal>
            <ogc:Literal>12</ogc:Literal>
            <ogc:Literal>0.6</ogc:Literal>
            <ogc:Literal>14</ogc:Literal>
            <ogc:Literal>0.7</ogc:Literal>
            <ogc:Literal>16</ogc:Literal>
            <ogc:Literal>0.8</ogc:Literal>
            <ogc:Literal>18</ogc:Literal>
            <ogc:Literal>0.9</ogc:Literal>
            <ogc:Literal>20</ogc:Literal>
            <ogc:Literal>1.0</ogc:Literal>
            <ogc:Literal>22</ogc:Literal>
            <ogc:Literal>1.1</ogc:Literal>
            <ogc:Literal>25</ogc:Literal>
          </ogc:Function>       
   </Size>

This block of code calculates the current speed and uses this value for setting the arrow size.

.. code-block:: xml

   <Rotation>
      <ogc:Function name="toDegrees">
       <ogc:Function name="atan2">
          <ogc:PropertyName>u-component_of_current_surface</ogc:PropertyName>
          <ogc:PropertyName>v-component_of_current_surface</ogc:PropertyName>
       </ogc:Function>
      </ogc:Function>                    
   </Rotation>

This last block defines the rotation of each current vector point. The rotation value is calculated with the *atan2* function
between the two components of the current vector. 

WMS Request
-----------

Execute the following WMS request for seeing the result of the rendering transformation::

	http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:currents&styles=currents&bbox=-20.1,-30.1,51.1,20.1&width=512&height=360&srs=EPSG:4326&format=image/png
	
And the result should be like this:

	.. figure:: img/rtx_currents_001.png
	
As for the wind barbs, you can see that the arrows are not ordered and this is caused by the high subsampling. In fact if you zoom 
on the image, reducing the subsampling, you can see that the arrows became ordered on the same direction.

Execute this zoomed request and see the result on GeoServer:

	.. code-block:: xml

		http://localhost:8083/geoserver/geosolutions/wms?LAYERS=geosolutions:currents&STYLES=currents&FORMAT=image/png&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&SRS=EPSG:4326&BBOX=-4.051318359375,-8.6597686767577,-2.938818359375,-7.8775421142577&WIDTH=512&HEIGHT=360
	
	.. figure:: img/rtx_currents_002.png