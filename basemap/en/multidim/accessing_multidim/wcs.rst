.. module:: geoserver.wcs
   :synopsis: WCS requests for multidimensional coverages.

.. _geoserver.wcs:

WCS requests for multidimensional coverages
-------------------------------------------
WCS 2.0.1 provides capabilities to access multidimensional data. Requests like the `GetCapabilities` seen in the previous section or the `DescribeCoverage` provided below, allow to get information about the extent of the whole coverage.

**WCS 2.0 Sample Requests**

*DescribeCoverage*

KVP Encoding

Single Coverage

.. code-block:: xml

  http://localhost:8083/geoserver/ows?service=WCS&version=2.0.1&request=DescribeCoverage&coverageId=geosolutions__NO2

The request will download an XML containing the details of the multidimensional coverage.

* The `BoundedBy` is represented by an `EnvelopeWithTimePeriod`

  .. code-block:: xml

    <gml:boundedBy>
        <gml:EnvelopeWithTimePeriod srsName="http://www.opengis.net/def/crs/EPSG/0/4326" axisLabels="Lat Long elevation time" uomLabels="Deg Deg m s" srsDimension="3">
            <gml:lowerCorner>44.96875 4.9375 10.0</gml:lowerCorner>
            <gml:upperCorner>50.96875 14.9375 2500.0</gml:upperCorner>
            <gml:beginPosition>2013-03-01T00:00:00.000Z</gml:beginPosition>
            <gml:endPosition>2013-03-02T23:00:00.000Z</gml:endPosition>
        </gml:EnvelopeWithTimePeriod>
    </gml:boundedBy>
   
* The `TimeDomain` reports the list of the available TimePosition's

  .. code-block:: xml

    <wcsgs:TimeDomain default="2013-03-02T23:00:00.000Z">
       <gml:TimeInstant gml:id="geosolutions__NO2_td_0">
          <gml:timePosition>2013-03-01T00:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       <gml:TimeInstant gml:id="geosolutions__NO2_td_1">
          <gml:timePosition>2013-03-01T01:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       <gml:TimeInstant gml:id="geosolutions__NO2_td_2">
          <gml:timePosition>2013-03-01T02:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       <gml:TimeInstant gml:id="geosolutions__NO2_td_3">
          <gml:timePosition>2013-03-01T03:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       <gml:TimeInstant gml:id="geosolutions__NO2_td_4">
          <gml:timePosition>2013-03-01T04:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       <gml:TimeInstant gml:id="geosolutions__NO2_td_5">
          <gml:timePosition>2013-03-01T05:00:00.000Z</gml:timePosition>
       </gml:TimeInstant>
       ....
    </wcsgs:TimeDomain>

* The `ElevationDomain` reports the list values for the available elevations

  .. code-block:: xml

    <wcsgs:ElevationDomain uom="m" default="10.0">
       <wcsgs:SingleValue>10.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>35.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>75.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>125.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>175.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>250.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>350.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>450.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>550.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>700.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>900.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>1250.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>1750.0</wcsgs:SingleValue>
       <wcsgs:SingleValue>2500.0</wcsgs:SingleValue>
    </wcsgs:ElevationDomain>

* The `DimensionDomain` reports the list of the available dimensions for this coverage

  .. code-block:: xml

    <wcsgs:DimensionDomain name="UPDATED" default="2018-07-02T07:52:06.000Z">
      <gml:TimeInstant gml:id="geosolutions__NO2_dd_0_0">
        <gml:timePosition>2018-07-02T07:52:06.000Z</gml:timePosition>
      </gml:TimeInstant>
      <gml:TimeInstant gml:id="geosolutions__NO2_dd_0_1">
        <gml:timePosition>2018-07-02T07:52:08.000Z</gml:timePosition>
      </gml:TimeInstant>
    </wcsgs:DimensionDomain>
    <wcsgs:DimensionDomain name="FILEDATE" default="2013-03-01T00:00:00.000Z">
      <gml:TimeInstant gml:id="geosolutions__NO2_dd_1_0">
        <gml:timePosition>2013-03-01T00:00:00.000Z</gml:timePosition>
      </gml:TimeInstant>
      <gml:TimeInstant gml:id="geosolutions__NO2_dd_1_1">
        <gml:timePosition>2013-03-02T00:00:00.000Z</gml:timePosition>
      </gml:TimeInstant>
      <gml:TimeInstant gml:id="geosolutions__NO2_dd_1_2">
        <gml:timePosition>2013-03-03T00:00:00.000Z</gml:timePosition>
      </gml:TimeInstant>
    </wcsgs:DimensionDomain>
    
Those dimension values may be used to perform a getCoverage request in order to get raw data back.
WCS 2.0.1 also allows getting raw data from a multidimensional coverage through `slicing` and `trimming`:

* Slicing: allows to specify a single value for a specific dimension.
* Trimming: allows to specify a min-max values range for a specific dimension, as an instance, time, in order to get back all the data available within that range.


*GetCoverage*

KVP Encoding

The following request will get back a GeoTiff produced on top of raw data defined by this selection:

* Trimming on latitude [40 -> 50]
* Trimming on longitude [5 -> 20]
* Slicing on elevation [350]
* Slicing on time [2013-03-01T10:00:00.000Z]

GetCoverage request using KVP encoding

   .. code-block:: xml
   
    http://localhost:8083/geoserver/wcs?request=GetCoverage&service=WCS&version=2.0.1&coverageId=geosolutions__NO2&Format=geotiff

The browser should download a `geosolutions__NO2.tif` containing the requested data.

Trimming on dimensions (except the 2D dimensions such as latitude and logitude) is only supported when the output format properly supports the creation of a dataset containing multiple results, each of them being related to a specific combination of values for the requested dimensions.

Therefore, a NetCDF output format has been developed to achieve this, as explained in the next section.

Default values management
^^^^^^^^^^^^^^^^^^^^^^^^^
A Default values management logic is applied under these conditions:

#. The output format doesn't support multiple results so a single element will be served
#. The granules are provided by a Structured GridCoverageReader
#. Some dimensions haven't been specified in the request

In that case a query will be performed to get a granule matching the specified dimension values. Then the unspecified dimensions values will be set to the values obtained from the returned granule.

 