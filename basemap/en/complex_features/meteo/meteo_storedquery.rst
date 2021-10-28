.. module:: hale.meteo_storedquery
.. _hale.meteo_storedquery:

.. include:: <isonum.txt>

Stored Query support
--------------------

GeoServer provides Stored Query support for Complex features. On this trainning session we will create a store query for the Station feature type and use it for retrieving future queries to GeoServer.

Create a Stored Query
+++++++++++++++++++++

We will create a WFS Stored Query for *st:Station* layer, using the *CreateStoredQuery* request and the XML definition

The body of the **GetFeature** request is:

.. code-block:: xml

    <wfs:CreateStoredQuery service='WFS' version='2.0.0'
                         xmlns:wfs='http://www.opengis.net/wfs/2.0'
                         xmlns:fes='http://www.opengis.net/fes/2.0'
                         xmlns:gml='http://www.opengis.net/gml/3.2'
                         xmlns:st='http://www.stations.org/1.0'>
    <wfs:StoredQueryDefinition id='stationsStoredQuery'>
      <wfs:Parameter name='mail' type='string'/>
      <wfs:QueryExpressionText
          returnFeatureTypes='st:Station'
          language='urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression'
          isPrivate='false'>
        <wfs:Query typeNames='st:Station'>
          <fes:Filter>
            <fes:PropertyIsEqualTo>
              <fes:ValueReference>st:contactMail</fes:ValueReference>
              <fes:Literal>${mail}</fes:Literal>
            </fes:PropertyIsEqualTo>
          </fes:Filter>
        </wfs:Query>
      </wfs:QueryExpressionText>
    </wfs:StoredQueryDefinition>
  </wfs:CreateStoredQuery>

Copy-paste the following command in the cURL shell to execute a **POST** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos -XPOST -H "Content-type: text/xml" -d @data/sample_requests/st_createstoredquery.xml http://localhost:8083/geoserver/wfs > output.xml
 
List and describe existing Stored Queries
+++++++++++++++++++++++++++++++++++++++++

We will list current Stored Queries on GeoServer using the *ListStoredQueries* WFS request.
  
Copy-paste the following command in the cURL shell to execute a **GET** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos http://localhost:8083/geoserver/wfs?request=ListStoredQueries&service=wfs&version=2.0.0 > output.xml
  
The XML output for this request should include the *stationsStoredQuery* stored query definition, as following:

.. code-block:: xml
  
  <?xml version="1.0" encoding="UTF-8"?>
  <wfs:ListStoredQueriesResponse
      xmlns:geosolutions="http://www.geo-solutions.it/workshop"
      xmlns:sf="http://www.geo-solutions.it/sf"
      xmlns:xs="http://www.w3.org/2001/XMLSchema"
      xmlns:fes="http://www.opengis.net/fes/2.0"
      xmlns:wfs="http://www.opengis.net/wfs/2.0"
      xmlns:gml="http://www.opengis.net/gml/3.2"
      xmlns:ows="http://www.opengis.net/ows/1.1"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd">
    <wfs:StoredQuery id="urn:ogc:def:query:OGC-WFS::GetFeatureById">
      <wfs:Title xml:lang="en">Get feature by identifier</wfs:Title>
      <wfs:ReturnFeatureType>sf:restricted</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>sf:roads</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>sf:streams</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:BoulderCityLimits</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:Counties</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:Parcels</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:Trails</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:Wetlands_regulatory_area</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:WorldCountries</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:bbuildings</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:blakes</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:bplandmarks</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:bptlandmarks</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:bptlandmarks_2876</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:brivers</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:bstreets</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:poi</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:restricted</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:states</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:storm_obs</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>geosolutions:streams</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>st:Observation</wfs:ReturnFeatureType>
      <wfs:ReturnFeatureType>st:Station</wfs:ReturnFeatureType>
    </wfs:StoredQuery>
    <wfs:StoredQuery id="stationsStoredQuery">
      <wfs:Title xml:lang="en"/>
      <wfs:ReturnFeatureType>st:Station</wfs:ReturnFeatureType>
    </wfs:StoredQuery>
  </wfs:ListStoredQueriesResponse>
  
Now we can get a description using the *DescribeStoredQueries* WFS request.
  
Copy-paste the following command in the cURL shell to execute a **GET** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos   http://localhost:8083/geoserver/wfs?service=wfs&version=2.0.0&request=DescribeStoredQueries&storedQuery_Id=stationsStoredQuery > output.xml
  
The XML output for this request should include the *stationsStoredQuery* stored query definition, as following:

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <wfs:DescribeStoredQueriesResponse
      xmlns:xs="http://www.w3.org/2001/XMLSchema"
      xmlns:fes="http://www.opengis.net/fes/2.0"
      xmlns:wfs="http://www.opengis.net/wfs/2.0"
      xmlns:gml="http://www.opengis.net/gml/3.2"
      xmlns:ows="http://www.opengis.net/ows/1.1"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd">
    <wfs:StoredQueryDescription id="stationsStoredQuery">
      <wfs:Parameter name="mail" type="geosolutions:string"/>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="st:Station">
        <wfs:Query wfs:typeNames="st:Station">
          <fes:Filter>
            <fes:PropertyIsEqualTo>
              <fes:ValueReference>st:contactMail</fes:ValueReference>
              <fes:Literal>${mail}</fes:Literal>
            </fes:PropertyIsEqualTo>
          </fes:Filter>
        </wfs:Query>
      </wfs:QueryExpressionText>
    </wfs:StoredQueryDescription>
  </wfs:DescribeStoredQueriesResponse>
  
Using the Stored Query
++++++++++++++++++++++

We will use the stored query and fetch the result features using the *GetFeature* WFS request.  The defined *mail* parameter is used in the same request URL.
  
Copy-paste the following command in the cURL shell to execute a **GET** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos http://localhost:8083/geoserver/wfs?service=wfs&version=2.0.0&request=GetFeature&typeNames=st%3AStation&StoredQuery_ID=stationsStoredQuery&mail=other%40other.com > output.xml
  
The XML output for this request should include the Station a feature that contains the *other@other.com* email, as following:

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <wfs:FeatureCollection
      xmlns:wfs="http://www.opengis.net/wfs/2.0"
      xmlns:xs="http://www.w3.org/2001/XMLSchema"
      xmlns:st="http://www.stations.org/1.0"
      xmlns:gml="http://www.opengis.net/gml/3.2"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" numberMatched="unknown" numberReturned="1" timeStamp="2020-05-27T20:44:45.360Z" xsi:schemaLocation="http://www.opengis.net/wfs/2.0 http://localhost:8083/geoserver/schemas/wfs/2.0/wfs.xsd http://www.stations.org/1.0 http://localhost:8083/geoserver/www/meteo/meteo.xsd http://www.opengis.net/gml/3.2 http://localhost:8083/geoserver/schemas/gml/3.2.1/gml.xsd">
    <wfs:member>
      <st:Station gml:id="station.21">
        <st:stationCode>ROV</st:stationCode>
        <st:stationName>Rovereto</st:stationName>
        <st:contactMail>other@other.com</st:contactMail>
        <st:position>
          <gml:Point srsName="http://www.opengis.net/gml/srs/epsg.xml#404000">
            <gml:pos>11.05 45.89</gml:pos>
          </gml:Point>
        </st:position>
      </st:Station>
    </wfs:member>
  </wfs:FeatureCollection>
  
Drop the Stored Query
+++++++++++++++++++++

Finally we will drop the stored query using the *DropStoredQuery* WFS request.
  
Copy-paste the following command in the cURL shell to execute a **GET** request (reponse is saved in a file called **output.xml**)::

  curl -u admin:Geos http://localhost:8083/geoserver/wfs?service=wfs&version=2.0.0&request=DropStoredQuery&storedQuery_Id=stationsStoredQuery > output.xml
