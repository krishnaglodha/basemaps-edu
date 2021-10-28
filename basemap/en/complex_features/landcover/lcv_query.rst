.. module:: hale.lcv_query
.. _hale.lcv_query:

.. include:: <isonum.txt>

.. include:: ../common/query_intro.txt

Retrieve lcv:LandConverUnit by description
++++++++++++++++++++++++++++++++++++++++++

This query retrieves all **lcv:LandCoverUnit** features whose **gml:description** attribute is equal to the string **'VIAREGGIO'**.

The body of the **GetFeature** request is:

.. code-block:: xml

   <wfs:GetFeature service="WFS" version="2.0.0"
     xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0"
     xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:gml="http://www.opengis.net/gml/3.2">
     <wfs:Query typeNames="lcv:LandCoverUnit">
       <fes:Filter>
         <fes:PropertyIsEqualTo>
	   <fes:ValueReference>gml:description</fes:ValueReference>
	   <fes:Literal>VIAREGGIO</fes:Literal>
	 </fes:PropertyIsEqualTo>
       </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/lcu_getfeature_descr.xml http://localhost:8083/geoserver/ows > output.xml

The request should return 73 features. Here is an excerpt from the response body:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:wfs="http://www.opengis.net/wfs/2.0"
			  xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0"
			  xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:base="http://inspire.ec.europa.eu/schemas/base/3.3"
			  xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:geosolutions="http://www.geo-solutions.it/workshop"
			  xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:sf="http://www.geo-solutions.it/sf"
			  xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			  numberMatched="unknown" numberReturned="73" timeStamp="2015-09-07T08:58:43.433Z"
			  xsi:schemaLocation="http://inspire.ec.europa.eu/schemas/lcv/3.0 http://inspire.ec.europa.eu/schemas/lcv/3.0/LandCoverVector.xsd http://www.opengis.net/gml/3.2 http://localhost:8083/geoserver/schemas/gml/3.2.1/gml.xsd http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd">
     <wfs:member>
       <lcv:LandCoverUnit gml:id="lcu.90">
	 <gml:description>VIAREGGIO</gml:description>
	 <lcv:inspireId>
	   <base:Identifier>
	     <base:localId>lcu.90</base:localId>
	     <base:namespace>http://it.geosolutions.hale-training</base:namespace>
	   </base:Identifier>
	 </lcv:inspireId>
	 <lcv:geometry>
	   <gml:MultiSurface srsName="urn:ogc:def:crs:EPSG::3044">
	     <gml:surfaceMember>
	       <gml:Polygon>
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>4857886.29111215 601905.3493974642
		     4857887.810884466 601856.8903100973 4857887.819975
		     601854.6246530213 4857889.308042958
		     601853.7024644027 4857887.972016796
		     601816.7710701665 4857884.737440859
		     601783.2948182033 4857886.747501158
		     601782.9210170452 4857885.800511726
		     601756.7422173256 4857885.790496408
		     601752.9622890407 4857881.890570495
		     601752.9723048495 4857883.040301406
		     601741.4538185433 4857883.850484745
		     601741.1425211226 4857883.221089987
		     601739.5073547103 4857885.140368759 601718.84293867
		     4857885.059952996 601714.629018903 4857902.020039483
		     601716.7029100031 4857972.298742857
		     601725.9024472586 4857991.80836919 601725.1123822049
		     4857991.525159633 601721.4728523782
		     4857994.8568963725 601721.4728387103
		     4857997.5575023955 601808.5537764309
		     4857952.12955588 601830.7205424695 4857941.069792542
		     601837.22046459 4857934.099949125 601843.1303811199
		     4857924.4801777685 601854.3402080253
		     4857920.260345488 601875.6898205113 4857915.45047825
		     601885.7896487329 4857910.730595849
		     601892.6095387777 4857903.670748277
		     601897.0894827913 4857886.29111215 601905.3493974642</gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gml:surfaceMember>
	   </gml:MultiSurface>
	 </lcv:geometry>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>141</lcv:class>
	     <lcv:observationDate>2007-01-01T00:00:00Z</lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>141</lcv:class>
	     <lcv:observationDate>2010-01-01T00:00:00Z</lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>133</lcv:class>
	     <lcv:observationDate>2013-01-01T00:00:00Z</lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
       </lcv:LandCoverUnit>
     </wfs:member>
     ...
     </wfs:member>
   </wfs:FeatureCollection>

Notice the **numberReturned="73"** attribute of the root **wfs:FeatureCollection** tag.

Filter lcv:LandConverUnit by observation class
++++++++++++++++++++++++++++++++++++++++++++++

This query retrieves all **lcv:LandCoverUnit** features with an observation class value of **112**  (discontinuous urban fabric). Notice that the filter is applied to an attribute (**lcv:class**) of a joined type (**lcv:LandCoverObservation**), nested in the queried feature type via feature chaining.

The body of the **GetFeature** request is:

.. code-block:: xml

   <wfs:GetFeature service="WFS" version="2.0.0"
     xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0"
     xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:gml="http://www.opengis.net/gml/3.2">
     <wfs:Query typeNames="lcv:LandCoverUnit">
	   <fes:Filter>
		   <fes:PropertyIsEqualTo>
			   <fes:ValueReference>lcv:landCoverObservation/lcv:LandCoverObservation/lcv:class</fes:ValueReference>
			   <fes:Literal>112</fes:Literal>
		   </fes:PropertyIsEqualTo>
	   </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/lcu_getfeature_obsclass.xml http://localhost:8083/geoserver/ows > output.xml

The request should return 6 features. Here is an excerpt of the response body:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:base="http://inspire.ec.europa.eu/schemas/base/3.3" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:geosolutions="http://www.geo-solutions.it/workshop" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:sf="http://www.geo-solutions.it/sf" xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" numberMatched="unknown" numberReturned="6" timeStamp="2015-09-07T09:55:08.742Z" xsi:schemaLocation="http://inspire.ec.europa.eu/schemas/lcv/3.0 http://inspire.ec.europa.eu/schemas/lcv/3.0/LandCoverVector.xsd http://www.opengis.net/gml/3.2 http://localhost:8083/geoserver/schemas/gml/3.2.1/gml.xsd http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd">
     <wfs:member>
       <lcv:LandCoverUnit gml:id="lcu.104">
	 <gml:description>VIAREGGIO
	 </gml:description>
	 <lcv:inspireId>
	   <base:Identifier>
	     <base:localId>lcu.104
	     </base:localId>
	     <base:namespace>http://it.geosolutions.hale-training
	     </base:namespace>
	   </base:Identifier>
	 </lcv:inspireId>
	 <lcv:geometry>
	   <gml:MultiSurface srsName="urn:ogc:def:crs:EPSG::3044">
	     <gml:surfaceMember>
	       <gml:Polygon>
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>4857850.012682666 602120.2454714874 4857877.211838755 602040.4268734097 4857907.741242151 602036.2968264862 4857912.531152764 602036.6767996326 4857916.901077465 602038.5467462488 4857922.731001072 602046.9065638175 4857945.86077571 602099.0154808646 4857957.4530526735 602122.6727847301 4857933.161126897 602125.8410243031 4857934.412080127 602144.6033634065 4857899.691867419 602151.4446761096 4857898.451867045 602145.6147917402 4857892.481985973 602146.9747904416 4857884.681545318 602149.7136705059 4857877.57215353 602118.8153855527 4857850.012682666 602120.2454714874
		     </gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gml:surfaceMember>
	   </gml:MultiSurface>
	 </lcv:geometry>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>141
	     </lcv:class>
	     <lcv:observationDate>2007-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>112
	     </lcv:class>
	     <lcv:observationDate>2010-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>112
	     </lcv:class>
	     <lcv:observationDate>2013-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
       </lcv:LandCoverUnit>
     </wfs:member>
     <wfs:member>
       <lcv:LandCoverUnit gml:id="lcu.324">
	 <gml:description>VIAREGGIO
	 </gml:description>
	 <lcv:inspireId>
	   <base:Identifier>
	     <base:localId>lcu.324
	     </base:localId>
	     <base:namespace>http://it.geosolutions.hale-training
	     </base:namespace>
	   </base:Identifier>
	 </lcv:inspireId>
	 <lcv:geometry>
	   <gml:MultiSurface srsName="urn:ogc:def:crs:EPSG::3044">
	     <gml:surfaceMember>
	       <gml:Polygon>
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>4856990.789343509 602009.7560911616 4856985.045612242 601999.9273010979 4857006.590157048 601988.6864258727 4857020.078005672 602014.2108865468 4857000.43442217 602024.8177659957 4856991.528534664 602011.0210641434 4856990.789343509 602009.7560911616
		     </gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gml:surfaceMember>
	   </gml:MultiSurface>
	 </lcv:geometry>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>112
	     </lcv:class>
	     <lcv:observationDate>2007-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>121
	     </lcv:class>
	     <lcv:observationDate>2010-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>121
	     </lcv:class>
	     <lcv:observationDate>2013-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
       </lcv:LandCoverUnit>
     </wfs:member>
     <wfs:member>
       <lcv:LandCoverUnit gml:id="lcu.597">
	 <gml:description>VIAREGGIO
	 </gml:description>
	 <lcv:inspireId>
	   <base:Identifier>
	     <base:localId>lcu.597
	     </base:localId>
	     <base:namespace>http://it.geosolutions.hale-training
	     </base:namespace>
	   </base:Identifier>
	 </lcv:inspireId>
	 <lcv:geometry>
	   <gml:MultiSurface srsName="urn:ogc:def:crs:EPSG::3044">
	     <gml:surfaceMember>
	       <gml:Polygon>
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>4855605.408744699 602935.079327123 4855628.698157359 602924.0793401959 4855639.257956396 602923.9692989733 4855643.047890096 602925.349257262 4855646.067840668 602927.2792082791 4855648.037809539 602928.8091711868 4855648.3084057355 602929.1343639108 4855610.41050307 602948.0835600132 4855605.408744699 602935.079327123
		     </gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gml:surfaceMember>
	   </gml:MultiSurface>
	 </lcv:geometry>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>210
	     </lcv:class>
	     <lcv:observationDate>2007-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>210
	     </lcv:class>
	     <lcv:observationDate>2010-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>112
	     </lcv:class>
	     <lcv:observationDate>2013-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
       </lcv:LandCoverUnit>
     </wfs:member>
     ...
   </wfs:FeatureCollection>

The **numberReturned="6"** attribute of the root **wfs:FeatureCollection** tag confirms that only 6 features matched the filter. Notice that for different Land Cover Units, the matching Land Cover Observation may be associated to different years: for example, **lcu.104** has *<obs:class>112</obs:class>* in years 2010 and 2013, while **lcu.324** has it only in year 2007. This happens because *lcv:landCoverObservation* is a multi-valued property and we did not specify which instance of it should match (i.e. specifying an index), so any will do.

If you published Land Cover data using the mapping definition from the :ref:`normalized tables example <mapping_normalized>`, you can't rely on a correspondence between property index and observation year, so all possible indices must be tested (assuming we know the number of *LandCoverObservation* instances for each *LandCoverUnit* instance, i.e. for how many years we have observation data):

.. code-block:: xml

   <wfs:GetFeature service="WFS" version="2.0.0"
     xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0"
     xmlns:wfs="http://www.opengis.net/wfs/2.0"
     xmlns:fes="http://www.opengis.net/fes/2.0"
     xmlns:gml="http://www.opengis.net/gml/3.2">
     <wfs:Query typeNames="lcv:LandCoverUnit">
	   <fes:Filter>
		   <fes:Or>
			   <fes:And>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[1]/lcv:LandCoverObservation/lcv:class</fes:ValueReference>
					   <fes:Literal>112</fes:Literal>
				   </fes:PropertyIsEqualTo>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[1]/lcv:LandCoverObservation/lcv:observationDate</fes:ValueReference>
					   <fes:Literal>2007-01-01T00:00:00Z</fes:Literal>
				   </fes:PropertyIsEqualTo>
			   </fes:And>
			   <fes:And>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[2]/lcv:LandCoverObservation/lcv:class</fes:ValueReference>
					   <fes:Literal>112</fes:Literal>
				   </fes:PropertyIsEqualTo>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[2]/lcv:LandCoverObservation/lcv:observationDate</fes:ValueReference>
					   <fes:Literal>2007-01-01T00:00:00Z</fes:Literal>
				   </fes:PropertyIsEqualTo>
			   </fes:And>
			   <fes:And>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[3]/lcv:LandCoverObservation/lcv:class</fes:ValueReference>
					   <fes:Literal>112</fes:Literal>
				   </fes:PropertyIsEqualTo>
				   <fes:PropertyIsEqualTo>
					   <fes:ValueReference>lcv:landCoverObservation[3]/lcv:LandCoverObservation/lcv:observationDate</fes:ValueReference>
					   <fes:Literal>2007-01-01T00:00:00Z</fes:Literal>
				   </fes:PropertyIsEqualTo>
			   </fes:And>
		   </fes:Or>
	   </fes:Filter>
     </wfs:Query>
   </wfs:GetFeature>

The above request selects only the units with observation class **112** in year **2007**. You can try it by executing the following command::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/lcu_getfeature_obsclass_obsdate_1.xml http://localhost:8083/geoserver/ows > output.xml

The response will contain just one feature, as expected:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:base="http://inspire.ec.europa.eu/schemas/base/3.3" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:geosolutions="http://www.geo-solutions.it/workshop" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:sf="http://www.geo-solutions.it/sf" xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" numberMatched="unknown" numberReturned="1" timeStamp="2015-09-07T10:22:10.485Z" xsi:schemaLocation="http://inspire.ec.europa.eu/schemas/lcv/3.0 http://inspire.ec.europa.eu/schemas/lcv/3.0/LandCoverVector.xsd http://www.opengis.net/gml/3.2 http://localhost:8083/geoserver/schemas/gml/3.2.1/gml.xsd http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd">
     <wfs:member>
       <lcv:LandCoverUnit gml:id="lcu.324">
	 <gml:description>VIAREGGIO
	 </gml:description>
	 <lcv:inspireId>
	   <base:Identifier>
	     <base:localId>lcu.324
	     </base:localId>
	     <base:namespace>http://it.geosolutions.hale-training
	     </base:namespace>
	   </base:Identifier>
	 </lcv:inspireId>
	 <lcv:geometry>
	   <gml:MultiSurface srsName="urn:ogc:def:crs:EPSG::3044">
	     <gml:surfaceMember>
	       <gml:Polygon>
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>4856990.789343509 602009.7560911616 4856985.045612242 601999.9273010979 4857006.590157048 601988.6864258727 4857020.078005672 602014.2108865468 4857000.43442217 602024.8177659957 4856991.528534664 602011.0210641434 4856990.789343509 602009.7560911616
		     </gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gml:surfaceMember>
	   </gml:MultiSurface>
	 </lcv:geometry>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>112
	     </lcv:class>
	     <lcv:observationDate>2007-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>121
	     </lcv:class>
	     <lcv:observationDate>2010-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
	 <lcv:landCoverObservation>
	   <lcv:LandCoverObservation>
	     <lcv:class>121
	     </lcv:class>
	     <lcv:observationDate>2013-01-01T00:00:00Z
	     </lcv:observationDate>
	   </lcv:LandCoverObservation>
	 </lcv:landCoverObservation>
       </lcv:LandCoverUnit>
     </wfs:member>
   </wfs:FeatureCollection>

.. Had we used the mapping from the :ref:`example based on denormalized tables <mapping_denormalized>`, the request would have been simpler, as the mapping explicitly defines a correspondence between *LandCoverObservation* instance and observation year.

.. For example, observations collected in year 2007 are always stored in the first *landCoverObservation* value, so the previous request becomes:
   .. code-block:: xml

      <wfs:GetFeature service="WFS" version="2.0.0"
	xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0"
	xmlns:wfs="http://www.opengis.net/wfs/2.0"
	xmlns:fes="http://www.opengis.net/fes/2.0"
	xmlns:gml="http://www.opengis.net/gml/3.2">
	<wfs:Query typeNames="lcv:LandCoverUnit">
	      <fes:Filter>
		      <fes:PropertyIsEqualTo>
			      <fes:ValueReference>lcv:landCoverObservation[1]/lcv:LandCoverObservation/lcv:class</fes:ValueReference>
			      <fes:Literal>112</fes:Literal>
		      </fes:PropertyIsEqualTo>
	      </fes:Filter>
	</wfs:Query>
      </wfs:GetFeature>

   Try the request with the command::

     curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/lcu_getfeature_obsclass_obsdate_2.xml http://localhost:8083/geoserver/ows > output.xml

   The response body should be the same as before.
