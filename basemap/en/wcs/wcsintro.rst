.. _geoserver.wfs:


Introduction to Web Coverage Service
====================================

A Web Coverage Service provides:

  * A set of **coverages** (one per raster layer)
  * The means to **describe** the structure of the coverage (resolution, bands and the like)
  * The means to extract data, eventually selecting by area, picking a specific band combination, rescaling
    and reprojecting the data

This allows a client to get to the source of the raster data, download it for later visualization
and analysis. 

This introduction showcases the WCS 2.0 protocol version, as it's easier to explain compared to
the older 1.0 and 1.1 versions.

GetCapabilities
---------------

Let's have a look at a WCS capabilities document using `this url <http://localhost:8083/geoserver/ows?service=WCS&version=2.0.0&request=GetCapabilities>`_:
    
    ``http://localhost:8083/geoserver/ows?service=WCS&version=2.0.0&request=GetCapabilities``
      
The response as usual is divided into sections:

* A **service identification** section with information of the service and profiles implemented (the WCS 2.0 specification
  is highly modular, a server can decide to only implement extraction by bounding box and leave
  other elements out)

  .. code-block:: xml

     <ows:ServiceIdentification>
        <ows:Title>FOSS4G 2011 styling examples</ows:Title>
        <ows:Abstract />
        <ows:Keywords>
          <ows:Keyword>WCS</ows:Keyword>
          <ows:Keyword>WMS</ows:Keyword>
          <ows:Keyword>GEOSERVER</ows:Keyword>
        </ows:Keywords>
        <ows:ServiceType>urn:ogc:service:wcs</ows:ServiceType>
        <ows:ServiceTypeVersion>2.0.1</ows:ServiceTypeVersion>
        <ows:ServiceTypeVersion>1.1.1</ows:ServiceTypeVersion>
        <ows:ServiceTypeVersion>1.1.0</ows:ServiceTypeVersion>
        <ows:Profile>http://www.opengis.net/spec/WCS/2.0/conf/core</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_protocol-binding_get-kvp/1.0.1</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_protocol-binding_post-xml/1.0</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_crs/1.0/conf/crs-gridded-coverage</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_geotiff-coverages/1.0/conf/geotiff-coverage</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/GMLCOV/1.0/conf/gml-coverage</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/GMLCOV/1.0/conf/special-format</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/GMLCOV/1.0/conf/multipart</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_scaling/1.0/conf/scaling</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_crs/1.0/conf/crs</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_interpolation/1.0/conf/interpolation</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_interpolation/1.0/conf/interpolation-per-axis</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_interpolation/1.0/conf/nearest-neighbor</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_interpolation/1.0/conf/linear</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_interpolation/1.0/conf/cubic</ows:Profile>
        <ows:Profile>http://www.opengis.net/spec/WCS_service-extension_range-subsetting/1.0/conf/record-subsetting</ows:Profile>
        <ows:Fees>NONE</ows:Fees>
        <ows:AccessConstraints>NONE</ows:AccessConstraints>
      </ows:ServiceIdentification>

* A **service provider** section with contact information

  .. code-block:: xml
      <ows:ServiceProvider>
        <ows:ProviderName>GeoSolutions</ows:ProviderName>
        <ows:ProviderSite xlink:href="http://geoserver.org" />
        <ows:ServiceContact>
          <ows:IndividualName>GeoSolutions</ows:IndividualName>
          <ows:ContactInfo>
            <ows:Phone>
              <ows:Voice>+39 0584 962313</ows:Voice>
              <ows:Facsimile>+39 0584 962313</ows:Facsimile>
            </ows:Phone>
            <ows:Address>
              <ows:DeliveryPoint>Via Poggio alle Viti 1187</ows:DeliveryPoint>
              <ows:City>Massarosa</ows:City>
              <ows:PostalCode>55054</ows:PostalCode>
              <ows:Country>Italy</ows:Country>
              <ows:ElectronicMailAddress>info@geo-solutions.it</ows:ElectronicMailAddress>
            </ows:Address>
            <ows:OnlineResource xlink:href="http://geoserver.org" />
          </ows:ContactInfo>
        </ows:ServiceContact>
      </ows:ServiceProvider>

* A **operations metadata** section reporting the available requests and their output formats

  .. code-block:: xml
  
      <ows:OperationsMetadata>
          <ows:Operation name="GetCapabilities">
             <ows:DCP>
                <ows:HTTP>
                   <ows:Get xlink:href="http://localhost:8083/geoserver/wcs?" />
                </ows:HTTP>
             </ows:DCP>
             <ows:DCP>
                <ows:HTTP>
                   <ows:Post xlink:href="http://localhost:8083/geoserver/wcs?" />
                </ows:HTTP>
             </ows:DCP>
          </ows:Operation>
          <ows:Operation name="DescribeCoverage">
             <ows:DCP>
                <ows:HTTP>
                ....

* A **service metadata** section reporting requestable formats and the list of target coordinate
  reference systems for reprojection. This section contains by default a long list of coordinate reference systems,
  that can be reduced at will using the "Limited SRS list" in the `WCS admin page <http://localhost:8083/geoserver/web/wicket/bookmarkable/org.geoserver.wcs.web.WCSAdminPage>`_

  .. code-block:: xml
  
      <wcs:ServiceMetadata>
          <wcs:formatSupported>application/gml+xml</wcs:formatSupported>
          <wcs:formatSupported>application/gtopo30</wcs:formatSupported>
          <wcs:formatSupported>application/x-gzip</wcs:formatSupported>
          <wcs:formatSupported>application/x-netcdf</wcs:formatSupported>
          <wcs:formatSupported>image/jpeg</wcs:formatSupported>
          <wcs:formatSupported>image/png</wcs:formatSupported>
          <wcs:formatSupported>image/tiff</wcs:formatSupported>
          <wcs:formatSupported>text/plain</wcs:formatSupported>
          <wcs:Extension>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2000</wcscrs:crsSupported>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2001</wcscrs:crsSupported>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2002</wcscrs:crsSupported>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2003</wcscrs:crsSupported>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2004</wcscrs:crsSupported>
             <wcscrs:crsSupported>http://www.opengis.net/def/crs/EPSG/0/2005</wcscrs:crsSupported>
         ...
  
* A **contents** section showing all available coverages and their location

  .. code-block:: xml
  
       <wcs:Contents>
          <wcs:CoverageSummary>
             <wcs:CoverageId>geosolutions__drone_rgb</wcs:CoverageId>
             <wcs:CoverageSubtype>RectifiedGridCoverage</wcs:CoverageSubtype>
             <ows:WGS84BoundingBox>
                <ows:LowerCorner>-50.56497637687449 -17.152181263039463</ows:LowerCorner>
                <ows:UpperCorner>-50.53540395165792 -17.13600029327641</ows:UpperCorner>
             </ows:WGS84BoundingBox>
             <ows:BoundingBox crs="http://www.opengis.net/def/crs/EPSG/0/EPSG:32722">
                <ows:LowerCorner>546272.1483 8103557.97778</ows:LowerCorner>
                <ows:UpperCorner>549413.48922 8105340.80852</ows:UpperCorner>
             </ows:BoundingBox>
          </wcs:CoverageSummary>
          <wcs:CoverageSummary>
             <wcs:CoverageId>geosolutions__SONY_A6000_15JB_03112016_jpeg</wcs:CoverageId>
      
DescribeCoverage
````````````````
      
Now let's request more information for the ``geosolutions__usa`` layer using the ``DescribeCoverage``
`request <http://localhost:8083/geoserver/ows?service=WCS&version=2.0.0&request=DescribeCoverage&coverageId=geosolutions__usa>`_::
    
    http://localhost:8083/geoserver/ows?service=WCS&version=2.0.0&request=DescribeCoverage&coverageId=geosolutions__usa

.. list-table::
   :header-rows: 1
   
   * - Element
     - Description
   * - http://localhost:8083/geoserver/ows?service=WCS
     - The base URL
   * - service=WCS
     - The service
   * - version=2.0.0
     - The service version
   * - request=DescribeCoverage
     - The request
   * - coverageId=geosolutions__usa
     - The list of coverages that need to be described (comma separated, can be one)
    
The output provides more information about the coverage, including:

* The area occupied by the coverage,

  .. code-block:: xml
 
    <?xml version="1.0" encoding="UTF-8"?><wcs:CoverageDescriptions xmlns:wcs="http://www.opengis.net/wcs/2.0" xmlns:ows="http://www.opengis.net/ows/2.0" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:gmlcov="http://www.opengis.net/gmlcov/1.0" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:swe="http://www.opengis.net/swe/2.0" xmlns:wcsgs="http://www.geoserver.org/wcsgs/2.0" xsi:schemaLocation=" http://www.opengis.net/wcs/2.0 http://schemas.opengis.net/wcs/2.0/wcsDescribeCoverage.xsd">
      <wcs:CoverageDescription gml:id="geosolutions__usa">
        <gml:boundedBy>
          <gml:Envelope srsName="http://www.opengis.net/def/crs/EPSG/0/4326" axisLabels="Lat Long" uomLabels="Deg Deg" srsDimension="2">
            <gml:lowerCorner>20.7052 -130.85168</gml:lowerCorner>
            <gml:upperCorner>54.1141 -62.0054</gml:upperCorner>
          </gml:Envelope>
        </gml:boundedBy>
        <wcs:CoverageId>geosolutions__usa</wcs:CoverageId>
        <gml:coverageFunction>
          <gml:GridFunction>
            <gml:sequenceRule axisOrder="+2 +1">Linear</gml:sequenceRule>
            <gml:startPoint>0 0</gml:startPoint>
          </gml:GridFunction>
        </gml:coverageFunction>
        <gmlcov:metadata>
          <gmlcov:Extension/>
        </gmlcov:metadata>

* Information about its resolution, and how to map pixel coordinate to real world ones:

  .. code-block:: xml

        <gml:domainSet>
          <gml:RectifiedGrid gml:id="grid00__geosolutions__usa" dimension="2">
            <gml:limits>
              <gml:GridEnvelope>
                <gml:low>0 0</gml:low>
                <gml:high>982 597</gml:high>
              </gml:GridEnvelope>
            </gml:limits>
            <gml:axisLabels>i j</gml:axisLabels>
            <gml:origin>
              <gml:Point gml:id="p00_geosolutions__usa" srsName="http://www.opengis.net/def/crs/EPSG/0/4326">
                <gml:pos>54.08616613712375 -130.81666154628687</gml:pos>
              </gml:Point>
            </gml:origin>
            <gml:offsetVector srsName="http://www.opengis.net/def/crs/EPSG/0/4326">0.0 0.07003690742624616</gml:offsetVector>
            <gml:offsetVector srsName="http://www.opengis.net/def/crs/EPSG/0/4326">-0.05586772575250837 0.0</gml:offsetVector>
          </gml:RectifiedGrid>
        </gml:domainSet>

* The available image bands and their allowed values:

  .. code-block:: xml

        <gmlcov:rangeType>
          <swe:DataRecord>
            <swe:field name="RED_BAND">
              <swe:Quantity>
                <swe:description>RED_BAND</swe:description>
                <swe:uom code="W.m-2.Sr-1"/>
                <swe:constraint>
                  <swe:AllowedValues>
                    <swe:interval>-1.7976931348623157E308 1.7976931348623157E308</swe:interval>
                  </swe:AllowedValues>
                </swe:constraint>
              </swe:Quantity>
            </swe:field>
            <swe:field name="GREEN_BAND">
              <swe:Quantity>
                <swe:description>GREEN_BAND</swe:description>
                <swe:uom code="W.m-2.Sr-1"/>
                <swe:constraint>
                  <swe:AllowedValues>
                    <swe:interval>-1.7976931348623157E308 1.7976931348623157E308</swe:interval>
                  </swe:AllowedValues>
                </swe:constraint>
              </swe:Quantity>
            </swe:field>
            <swe:field name="BLUE_BAND">
              <swe:Quantity>
                <swe:description>BLUE_BAND</swe:description>
                <swe:uom code="W.m-2.Sr-1"/>
                <swe:constraint>
                  <swe:AllowedValues>
                    <swe:interval>-1.7976931348623157E308 1.7976931348623157E308</swe:interval>
                  </swe:AllowedValues>
                </swe:constraint>
              </swe:Quantity>
            </swe:field>
          </swe:DataRecord>
        </gmlcov:rangeType>
        <wcs:ServiceParameters>
          <wcs:CoverageSubtype>RectifiedGridCoverage</wcs:CoverageSubtype>
          <wcs:nativeFormat>image/tiff</wcs:nativeFormat>
        </wcs:ServiceParameters>
      </wcs:CoverageDescription>

Armed with this information, the client can now perform data extraction requests.


