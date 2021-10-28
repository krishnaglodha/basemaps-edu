.. module:: hale.gsml_query
.. _hale.gsml_query:

.. include:: <isonum.txt>

.. include:: ../common/query_intro.txt

Retrieve gsml:GeologicEvent by age
++++++++++++++++++++++++++++++++++

This query retrieves all **gsml:GeologicEvent** features whose **gsml:eventAge** attribute is expressed as an interval of time with lower bound equal to the string **'Oligocene'**.

The body of the **GetFeature** request is:

.. code-block:: xml

   <wfs:GetFeature service="WFS" version="1.1.0"
     xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0"
     xmlns:wfs="http://www.opengis.net/wfs"
     xmlns:ogc="http://www.opengis.net/ogc"
     xmlns:gml="http://www.opengis.net/gml">
     <wfs:Query typeName="gsml:GeologicEvent">
	   <ogc:Filter>
		   <ogc:PropertyIsEqualTo>
			   <ogc:PropertyName>gsml:eventAge/gsml:CGI_TermRange/gsml:lower/gsml:CGI_TermValue/gsml:value</ogc:PropertyName>
			   <ogc:Literal>Oligocene</ogc:Literal>
		   </ogc:PropertyIsEqualTo>
	   </ogc:Filter>
     </wfs:Query>
   </wfs:GetFeature>

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::
   
  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/ge_getfeature_age.xml http://localhost:8083/geoserver/ows > output.xml

The request should return just 1 feature. Here is the response body:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:wfs="http://www.opengis.net/wfs" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:ows="http://www.opengis.net/ows" xmlns:base="http://inspire.ec.europa.eu/schemas/base/3.3" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:geosolutions="http://www.geo-solutions.it/workshop" xmlns:sf="http://www.geo-solutions.it/sf" xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0" xmlns:gml="http://www.opengis.net/gml" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" numberOfFeatures="1" timeStamp="2015-09-07T11:05:55.907Z" xsi:schemaLocation="urn:cgi:xmlns:CGI:GeoSciML:2.0 http://www.geosciml.org/geosciml/2.0/xsd/geosciml.xsd http://www.opengis.net/wfs http://localhost:8083/geoserver/schemas/wfs/1.1.0/wfs.xsd">
     <gml:featureMembers>
       <gsml:GeologicEvent gml:id="ge.26931120">
	 <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitId">gu.93
	 </gml:name>
	 <gsml:eventAge>
	   <gsml:CGI_TermRange>
	     <gsml:lower>
	       <gsml:CGI_TermValue>
		 <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Oligocene
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:lower>
	     <gsml:upper>
	       <gsml:CGI_TermValue>
		 <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Paleocene
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:upper>
	   </gsml:CGI_TermRange>
	 </gsml:eventAge>
	 <gsml:eventEnvironment>
	   <gsml:CGI_TermValue>
	     <gsml:value>fluvial
	     </gsml:value>
	   </gsml:CGI_TermValue>
	 </gsml:eventEnvironment>
	 <gsml:eventProcess>
	   <gsml:CGI_TermValue>
	     <gsml:value>channelled stream flow
	     </gsml:value>
	   </gsml:CGI_TermValue>
	 </gsml:eventProcess>
       </gsml:GeologicEvent>
     </gml:featureMembers>
   </wfs:FeatureCollection>

Notice the **numberReturned="1"** attribute of the root **wfs:FeatureCollection** tag.

Filtering gsml:GeologicUnit by BBOX
+++++++++++++++++++++++++++++++++++

This query retrieves all **gsml:GeologicUnit** features intersecting the specified bounding box. Notice that *gsml:GeologicUnit* has itself no geometric attribute and the filter is applied to an attribute (**gsml:shape**) of a joined type (**gsml:MappedFeature**), nested in the queried feature type via feature chaining.

.. code-block:: xml

   <wfs:GetFeature service="WFS" version="1.1.0"
     xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0"
     xmlns:wfs="http://www.opengis.net/wfs"
     xmlns:ogc="http://www.opengis.net/ogc"
     xmlns:gml="http://www.opengis.net/gml">
     <wfs:Query typeName="gsml:GeologicUnit">
	   <ogc:Filter>
		   <ogc:Intersects>
			   <ogc:PropertyName>gsml:occurrence/gsml:MappedFeature/gsml:shape</ogc:PropertyName>
			   <gml:Envelope>
				   <gml:lowerCorner>143.49 -39</gml:lowerCorner>
				   <gml:upperCorner>143.50 -38</gml:upperCorner>
			   </gml:Envelope>
		   </ogc:Intersects>
	   </ogc:Filter>
     </wfs:Query>
   </wfs:GetFeature>

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/gu_getfeature_bbox.xml http://localhost:8083/geoserver/ows > output.xml
  
Again, the request should return just 1 feature. Here is the response body:
   
.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wfs:FeatureCollection xmlns:wfs="http://www.opengis.net/wfs" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gsml="urn:cgi:xmlns:CGI:GeoSciML:2.0" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gmd="http://www.isotc211.org/2005/gmd" xmlns:ows="http://www.opengis.net/ows" xmlns:base="http://inspire.ec.europa.eu/schemas/base/3.3" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:geosolutions="http://www.geo-solutions.it/workshop" xmlns:sf="http://www.geo-solutions.it/sf" xmlns:lcv="http://inspire.ec.europa.eu/schemas/lcv/3.0" xmlns:gml="http://www.opengis.net/gml" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" numberOfFeatures="1" timeStamp="2015-09-07T11:15:40.119Z" xsi:schemaLocation="urn:cgi:xmlns:CGI:GeoSciML:2.0 http://www.geosciml.org/geosciml/2.0/xsd/geosciml.xsd http://www.opengis.net/wfs http://localhost:8083/geoserver/schemas/wfs/1.1.0/wfs.xsd">
     <gml:featureMembers>
       <gsml:GeologicUnit gml:id="gu.77">
	 <gml:description>Calcareous mudstone, minor thin calcarenite beds: locally carbonaceous and burrowed, locally abundant glauconite pellets and polished quartz sand, foraminifers, bryozoans, brachiopods and molluscs; open marine (below storm wave base) deposits
	 </gml:description>
	 <gml:name codeSpace="urn:x-test:classifierScheme:TestAuthority:GeologicUnitName">Narrawaturk Marl
	 </gml:name>
	 <gml:name codeSpace="urn:x-test:classifierScheme:TestAuthority:GeologicUnitName">-Pnn
	 </gml:name>
	 <gml:name codeSpace="urn:x-test:classifierScheme:TestAuthority:GeologicUnitName">urn:x-test:GeologicUnit:16777549126931077
	 </gml:name>
	 <gsml:occurrence>
	   <gsml:MappedFeature gml:id="mf.26106">
	     <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitLabel">Some mudstone
	     </gml:name>
	     <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitLabel">urn:x-test:GeologicUnit:16777549126931077
	     </gml:name>
	     <gsml:observationMethod>
	       <gsml:CGI_TermValue>
		 <gsml:value codeSpace="urn:ietf:rfc:2141">urn:ogc:def:nil:OGC:missing
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:observationMethod>
	     <gsml:positionalAccuracy>
	       <gsml:CGI_TermValue>
		 <gsml:value codeSpace="urn:ietf:rfc:2141">urn:ogc:def:nil:OGC:missing
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:positionalAccuracy>
	     <gsml:specification xlink:href="urn:x-test:GeologicUnit:16777549126931077"/>
	     <gsml:shape>
	       <gml:Polygon srsName="http://www.opengis.net/gml/srs/epsg.xml#4283">
		 <gml:exterior>
		   <gml:LinearRing>
		     <gml:posList>143.496091 -38.800309 143.496241 -38.799286 143.496136 -38.797775 143.497646 -38.800192 143.496091 -38.800309
		     </gml:posList>
		   </gml:LinearRing>
		 </gml:exterior>
	       </gml:Polygon>
	     </gsml:shape>
	   </gsml:MappedFeature>
	 </gsml:occurrence>
	 <gsml:geologicHistory>
	   <gsml:GeologicEvent gml:id="ge.26930473">
	     <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitId">gu.77
	     </gml:name>
	     <gsml:eventAge>
	       <gsml:CGI_TermRange>
		 <gsml:lower>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Holocene
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:lower>
		 <gsml:upper>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Pleistocene
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:upper>
	       </gsml:CGI_TermRange>
	     </gsml:eventAge>
	     <gsml:eventEnvironment>
	       <gsml:CGI_TermValue>
		 <gsml:value>swamp/marsh/bog
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventEnvironment>
	     <gsml:eventProcess>
	       <gsml:CGI_TermValue>
		 <gsml:value>detrital deposition still water
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventProcess>
	   </gsml:GeologicEvent>
	 </gsml:geologicHistory>
	 <gsml:geologicHistory>
	   <gsml:GeologicEvent gml:id="ge.26930960">
	     <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitId">gu.77
	     </gml:name>
	     <gsml:eventAge>
	       <gsml:CGI_TermRange>
		 <gsml:lower>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Pliocene
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:lower>
		 <gsml:upper>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:StratChart:2008">Miocene
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:upper>
	       </gsml:CGI_TermRange>
	     </gsml:eventAge>
	     <gsml:eventEnvironment>
	       <gsml:CGI_TermValue>
		 <gsml:value>marine
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventEnvironment>
	   </gsml:GeologicEvent>
	 </gsml:geologicHistory>
	 <gsml:geologicHistory>
	   <gsml:GeologicEvent gml:id="ge.26932959">
	     <gml:name codeSpace="urn:cgi:classifierScheme:GSV:GeologicalUnitId">gu.77
	     </gml:name>
	     <gsml:eventAge>
	       <gsml:CGI_TermRange>
		 <gsml:lower>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:Strat">LowerOrdovician
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:lower>
		 <gsml:upper>
		   <gsml:CGI_TermValue>
		     <gsml:value codeSpace="urn:cgi:classifierScheme:ICS:Strat">LowerOrdovician
		     </gsml:value>
		   </gsml:CGI_TermValue>
		 </gsml:upper>
	       </gsml:CGI_TermRange>
	     </gsml:eventAge>
	     <gsml:eventEnvironment>
	       <gsml:CGI_TermValue>
		 <gsml:value>submarine fan
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventEnvironment>
	     <gsml:eventEnvironment>
	       <gsml:CGI_TermValue>
		 <gsml:value>hemipelagic
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventEnvironment>
	     <gsml:eventProcess>
	       <gsml:CGI_TermValue>
		 <gsml:value>water [process]
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventProcess>
	     <gsml:eventProcess>
	       <gsml:CGI_TermValue>
		 <gsml:value>turbidity current
		 </gsml:value>
	       </gsml:CGI_TermValue>
	     </gsml:eventProcess>
	   </gsml:GeologicEvent>
	 </gsml:geologicHistory>
	 <gsml:composition>
	   <gsml:CompositionPart>
	     <gsml:role codeSpace="urn:cgi:classifierScheme:GSV:CompositionpartRole">interbedded component
	     </gsml:role>
	   </gsml:CompositionPart>
	 </gsml:composition>
	 <gsml:composition>
	   <gsml:CompositionPart>
	     <gsml:role codeSpace="urn:cgi:classifierScheme:GSV:CompositionpartRole">sole component
	     </gsml:role>
	   </gsml:CompositionPart>
	 </gsml:composition>
       </gsml:GeologicUnit>
     </gml:featureMembers>
   </wfs:FeatureCollection>
