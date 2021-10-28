.. module:: geoserver.geopackage_output

.. _geoserver.geopackage_output:

GeoPackage Output
----------------

A custom GeoPackage can be created with any number of tiles and features layers using the GeoPackage WPS Process.

The WPS process takes in one parameter: **contents** which is an XML schema that represents the desired output.


Content schema
`````````````

Each **GeoPackage** has a mandatory **name**, which will be the name of the file (with the extension .gpkg added). 

Each **layer** (features or tiles) has the following properties:

* **name** (mandatory): the name of the layer in the GeoPackage
* **identifier** (optional): an identifier for the layer
* **description** (optional): a description for the layer
* **srs** (mandatory for tiles, optional for features): coordinate reference system; for features the default is the SRS of the feature type
* **bbox** (mandatory for tiles, optional for features): the bounding box; for features the default is the bounding box of the feature type

Features Layer schema
`````````````
Each **features layer** has the following properties:

* **featuretype** (mandatory): the feature type
* **propertynames** (optional): list of comma-separated names of properties in feature type to be included (default is all properties)
* **filter** (optional): any OGC filter that will be applied on features before output
* **indexed** (optional): include spatial indexes in the output (true/false)
* **styles** (optional): include styles in the output using the portrayal and semantic annotation extensions (true/false)

Tiles Layer schema
`````````````
Each **tiles layer** has the following properties:

* **layers** (mandatory): comma-separated list of layers that will be included
* **styles**, **sld**, and **sldbody** are mutually exclusive, having one is mandatory
   * **styles**: list of comma-separated styles to be used
   * **sld**: path to SLD style file
   * **sldbody**: inline SLD style file

* **format** (optional): mime-type of image format of tiles (image/png or image/jpeg)
* **bgcolor** (optional): background colour as a six-digit hexadecimal RGB value
* **transparent** (optional): transparency (true or false)
* **coverage** (optional)
* **minzoom**, **maxzoom**, **minColumn**, **maxColumn**, **minRow**, **maxRow** (all optional): set the minimum and maximum zoom level, column, and rows
* **gridset** (optional): for the definition of the gridset we can use 2 forms:
    * using the name of a known gridset:
	.. code-block:: xml
   
		<gridset>
			<name>mygridset</name>
		</gridset>
    * using a custom definition of the gridset:
	.. code-block:: xml

		<gridset>
			<grids>
				<grid>
					<zoomlevel>1</zoomlevel>
					<tileWidth>256</tileWidth>
					<tileHeight>256</tileHeight>
					<matrixWidth>4</matrixWidth>
					<matrixHeight>4</matrixHeight>
					<pixelXSize>0.17</pixelXSize>
					<pixelYSize>0.17</pixelYSize>
				</grid>
				<grid>...</grid>
				...
			</grids>
		</gridset>

Export example
`````````````

#. Create a file named **request-gpkg.xml** with the following content:

	.. code-block:: xml
		:linenos:

		<?xml version="1.0" encoding="UTF-8"?><wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
		<ows:Identifier>gs:GeoPackage</ows:Identifier>
		<wps:DataInputs>
			<wps:Input>
				<ows:Identifier>contents</ows:Identifier>
				<wps:Data>
					<wps:ComplexData mimeType="text/xml; subtype=geoserver/geopackage">
						<![CDATA[
							<geopackage xmlns="http://www.opengis.net/gpkg" name="mygeopackage">
								<features identifier="L01" name="WorldCountries">
									<featuretype>geosolutions:WorldCountries</featuretype>
								</features>
							</geopackage>
						]]>
					</wps:ComplexData>
				</wps:Data>
			</wps:Input>
		</wps:DataInputs>
		<wps:ResponseForm>
			<wps:RawDataOutput>
				<ows:Identifier>geopackage</ows:Identifier>
			</wps:RawDataOutput>
		</wps:ResponseForm>
		</wps:Execute>

#. Open the GDAL shel and move to the path where you created the xml file of the request and run:

   .. code-block:: console
	
       curl -u <user>:<password> -H "Content-Type: application/xml" -XPOST -d @request-gpkg.xml http://localhost:8083/geoserver/wps

   The response of the request contains the url endpoint for the download of the gpkg file:

   .. code-block:: console

       http://localhost:8083/geoserver/ows?service=WPS&version=1.0.0&request=GetExecutionResult&executionId=e4311010-3c8c-4776-8a70-158cd946e934&outputId=mygeopackage.gpkg&mimetype=application%2Fx-gpkg
