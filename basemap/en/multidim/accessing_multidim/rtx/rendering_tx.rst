.. module:: geoserver.multidim.rendering_tx

.. _geoserver.multidim.rendering_tx:

Rendering Transformations
-------------------------

**Rendering Transformations** allow processing to be performed out on datasets within the GeoServer rendering pipeline. A typical transformation computes a derived or aggregated result from the input data, allowing various useful visualization effects to be obtained. Transformations may transform data from one format into another (i.e vector to raster or vice-versa), to provide an appropriate format for display.

The following table lists examples of various kinds of rendering transformations available in GeoServer:

+------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Type                   | Examples                                                                                                                               |
+========================+========================================================================================================================================+
| Raster-to-Vector       | Contour extracts contours from a DEM raster. RasterAsPointCollections extracts a vector field from a multi-band raster.                |
+------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Vector-to-Raster       | BarnesSurfaceInterpolation computes a surface from scattered data points. Heatmap computes a heatmap surface from weighted data points.|
+------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Vector-to-Vector       | PointStacker aggregates dense point data into clusters.                                                                                |
+------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Raster-to-Raster       | Dynamic Colormap which applies a color map to a raster.                                                                                |
+------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

Rendering transformations are invoked within SLD styles. Parameters may be supplied to control the appearance of the output. The rendered output for the layer is produced by applying the styling rules and symbolizers in the SLD to the result of transformation.

Rendering transformations are implemented using the same mechanism as WPS Processes. They can thus also be executed via the WPS protocol, if required. Conversely, any WPS process can be executed as a transformation, as long as the input and output are appropriate for use within an SLD.

Installation
^^^^^^^^^^^^

Using Rendering Transformations requires the WPS extension to be installed. 

.. Important:: The WPS service does not need to be enabled to use Rendering Transformations. To avoid unwanted consumption of server resources it may be desirable to disable the WPS service if it is not being used directly.

Usage
^^^^^

Rendering Transformations are invoked by adding the :file:`<Transformation>` element to a :file:`<FeatureTypeStyle>` element in an SLD document. 
This element specifies the name of the transformation process and usually includes parameter values controlling the operation of the transformation.

The :file:`<Transformation>` element syntax leverages on the OGC Filter function syntax. The content of the element is a :file:`<ogc:Function>` with the name of the rendering transformation process. Transformation processes may accept some number of parameters, which may be either required (in which case they must be specified), or optional (in which case they may be omitted if the default value is acceptable). Parameters are supplied as name/value pairs. Each parameter's name and value are supplied via another function :file:`<ogc:Function name="parameter">`. The first argument to this function is an :file:`<ogc:Literal>` containing the name of the parameter. The optional following arguments provide the value for the parameter (if any). Some parameters accept only a single value, while others may accept a list of values. As with any filter function argument, values may be supplied in several ways:

* As a literal value.
* As a computed expression.
* As an SLD environment variable, whose actual value is supplied in the WMS request (variable substitution in SLD).
* As a predefined SLD environment variable (which allows obtaining values for the current request such as output image width and height).
* The order of the supplied parameters is not significant.

Specifying the input dataset
#############################

Most rendering transformations take as input a dataset to be transformed. This is supplied via a *specially named parameter** which does not have a value specified (hence it is **implicitly** the data being read/accessed as part the WMS GetMap request). The name of the parameter is determined by the particular rendering transformation being used but it is usually *data*. When the transformation is executed, the input dataset is passed to it via this parameter. An snippet is provided here below:

.. code-block:: xml

    <StyledLayerDescriptor version="1.0.0"
      xmlns="http://www.opengis.net/sld" xmlns:gml="http://www.opengis.net/gml"
      xmlns:ogc="http://www.opengis.net/ogc" xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.opengis.net/sld ./StyledLayerDescriptor.xsd">
        <NamedLayer>
            <Name>Wind</Name>
            <UserStyle>
                <Title>Wind</Title>
                <FeatureTypeStyle>
                    ...
                    <Transformation>
                        <ogc:Function name="gs:renderingTransformation">
                            <ogc:Function name="parameter">
                                <ogc:Literal>data</ogc:Literal>
                            </ogc:Function>
                            </ogc:Function>
                            <!-- Param 1 -->
                            <ogc:Function name="parameter">
                              <ogc:Literal>Param1</ogc:Literal>
                              <ogc:Literal>True</ogc:Literal>
                            </ogc:Function>      
                            ...
                            <!-- Param N -->
                            <ogc:Function name="parameter">
                              <ogc:Literal>ParamN</ogc:Literal>
                              <ogc:Literal>True</ogc:Literal>
                            </ogc:Function>
                        </ogc:Function>
                    </Transformation>                 
                    ...
                </FeatureTypeStyle>
            </UserStyle>
        </NamedLayer>
    </StyledLayerDescriptor>ata</ogc:Literal>
	</ogc:Function>         

This would take the *implicit* parameter that GeoServer has read as part of the GetMap request (a vector layer or a raster layer) and will associate it to the *data* parameter of the rendering transformation being called. 

The input dataset to be processed by a rendering transformation is determined by the same query mechanism as used for all WMS requests, and can thus be filtered in the request if required.

In rendering transformations which take as input a vector dataset (featuretype) and convert it to a raster dataset, in order to pass validation, the SLD needs to mention the geometry attribute of the input dataset (even though it is not used). This is done by specifying the attribute name in the symbolizer :file:`<Geometry>` element.

The output of the rendering transformation is styled using symbolizers appropriate to its format: *PointSymbolizer*, *LineSymbolizer*, *PolygonSymbolizer*, and *TextSymbolizer* for vector data, and *RasterSymbolizer* for raster coverage data.

Important Remarks
^^^^^^^^^^^^^^^^^^^^

* A Rendering Transformation performs a (pre)processing of some sort on the data being accessed during a GetMap request allowing the creation of custom (and complex) logic to be executed on the fly. It is worth to emphasize this aspect once more since given the fact that data is processed on the fly as it is being rendered, such processing cannot be too heavy otherwise the performance of the WMS will suffer a lot, eventually resulting in requests timing out.

* If it is desired to display the input dataset in its original form, or transformed in another way, there are two options:

	* Another :file:`<FeatureTypeStyle>` can be used in the same SLD.
	* Another SLD can be created, and the layer displayed twice using the different SLDs.
	
* Rendering transformations may not work correctly in tiled mode, unless they have been specifically written to accomodate it.

Additional Examples
^^^^^^^^^^^^^^^^^^^^^^^^
In the following pages a few additional working examples are introduced.

.. toctree::
   :maxdepth: 1

   Wind arrows <wind_arrows.rst>
   Wind barbs <wind_barbs.rst>
   Current arrows <currents.rst>
   Dynamic Colormap <colormap.rst>


