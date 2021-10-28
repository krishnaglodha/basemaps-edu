.. module:: geoserver.REST
   :synopsis: Learn how to interact with ImageMosaic content through REST API.
   
.. _geoserver.REST:

Manage ImageMosaic content through REST API
=============================================================
REST is an acronym for *Representational State Transfer*. REST adopts a fixed set of operations on named resources, where the representation of each resource is the same for retrieving and setting information. In other words, you can retrieve (read) data in an XML format and also send data back to the server in similar XML format in order to set (write) changes to the system.


GeoServer provides a RESTful interface through which clients (*with administrator privileges*) can retrieve information about an instance and make configuration changes. 
Using the REST interface's simple HTTP calls, clients can configure GeoServer without needing to use the Web Administration Interface.

Extensive capabilities are available for configuring raster multidimensional data through REST calls to GeoServer. Specifically, a user with administrative privileges would be able to:

* **Access the structure of an ImageMosaic index** which is extremely useful for building complex queries to the index itself
* Create/update/drop ImageMosaic and the related coverages

In the following chapter, a few examples are presented that show how to use the various REST endpoints and functionalities. 
In the first subsections we are going to use the *polyphemus* mosaic created before, by performing calls to:

* get information about the granules index structure
* get the granules description
* query the index
* add more granules to the index
* remove granules from the index

Getting the ImageMosaic granules index
--------------------------------------

This sub-section will show you how to know the internal structure of the imageMosaic, by exposing the schema of the index.

* Enter the following command in the terminal::

    curl -v -u admin:Geos -XGET "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index.xml"

The resulting output should be like this:

   .. code-block:: xml
  
      <Schema>
        <attributes>
          <Attribute>
            <name>the_geom</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>com.vividsolutions.jts.geom.Polygon</binding>
          </Attribute>
          <Attribute>
            <name>location</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.lang.String</binding>
          </Attribute>
          <Attribute>
            <name>imageindex</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.lang.Integer</binding>
          </Attribute>
          <Attribute>
            <name>time</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.sql.Timestamp</binding>
          </Attribute>
          <Attribute>
            <name>elevation</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.lang.Double</binding>
          </Attribute>
          <Attribute>
            <name>fileDate</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.sql.Timestamp</binding>
          </Attribute>
          <Attribute>
            <name>updated</name>
            <minOccurs>0</minOccurs>
            <maxOccurs>1</maxOccurs>
            <nillable>true</nillable>
            <binding>java.sql.Timestamp</binding>
          </Attribute>
        </attributes>
        <atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules.xml" type="application/xml"/>
      </Schema>

Listing the granules
--------------------

The following sub-section will show you how to get the granules (description), by also limiting the number of the result to a maximum of 2 records.

* Enter the following command in the terminal::

    curl -v -u admin:Geos -XGET "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules.xml?limit=2"

This will result in a GML description of the granules, as follows:

   .. code-block:: xml
    
       <?xml version="1.0" encoding="UTF-8"?><wfs:FeatureCollection xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gf="http://www.geoserver.org/rest/granules" xmlns:wfs="http://www.opengis.net/wfs" xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc">
         <gml:boundedBy>
           <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
             <gml:coord>
               <gml:X>4.9375</gml:X>
               <gml:Y>44.96875</gml:Y>
             </gml:coord>
             <gml:coord>
               <gml:X>14.9375</gml:X>
               <gml:Y>50.96875</gml:Y>
             </gml:coord>
           </gml:Box>
         </gml:boundedBy>
         <gml:featureMember>
           <gf:NO2 fid="NO2.1">
             <gml:boundedBy>
               <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
                 <gml:coordinates>4.9375,44.96875 14.9375,50.96875</gml:coordinates>
               </gml:Box>
             </gml:boundedBy>
             <gf:the_geom>
               <gml:Polygon srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
                 <gml:outerBoundaryIs>
                   <gml:LinearRing>
                     <gml:coordinates>4.9375,44.96875 4.9375,50.96875 14.9375,50.96875 14.9375,44.96875 4.9375,44.96875</gml:coordinates>
                   </gml:LinearRing>
                 </gml:outerBoundaryIs>
               </gml:Polygon>
             </gf:the_geom>
             <gf:location>$TRAINING_ROOT\data\user_data\multidim\polyphemus\polyphemus_20130301.nc</gf:location>
             <gf:imageindex>336</gf:imageindex>
             <gf:time>2013-03-01T00:00:00Z</gf:time>
             <gf:elevation>35.0</gf:elevation>
             <gf:fileDate>2013-03-01T00:00:00Z</gf:fileDate>
             <gf:updated>2018-07-02T07:52:08Z</gf:updated>
           </gf:NO2>
         </gml:featureMember>
         <gml:featureMember>
           <gf:NO2 fid="NO2.2">
             <gml:boundedBy>
               <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
                 <gml:coordinates>4.9375,44.96875 14.9375,50.96875</gml:coordinates>
               </gml:Box>
             </gml:boundedBy>
             <gf:the_geom>
               <gml:Polygon srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
                 <gml:outerBoundaryIs>
                   <gml:LinearRing>
                     <gml:coordinates>4.9375,44.96875 4.9375,50.96875 14.9375,50.96875 14.9375,44.96875 4.9375,44.96875</gml:coordinates>
                   </gml:LinearRing>
                 </gml:outerBoundaryIs>
               </gml:Polygon>
             </gf:the_geom>
             <gf:location>%TRAINING_ROOT%\data\user_data\multidim\polyphemus\polyphemus_20130301.nc</gf:location>
             <gf:imageindex>337</gf:imageindex>
             <gf:time>2013-03-01T00:00:00Z</gf:time>
             <gf:elevation>15.0</gf:elevation>
             <gf:fileDate>2013-03-01T00:00:00Z</gf:fileDate>
             <gf:updated>2018-07-02T07:52:08Z</gf:updated>
           </gf:NO2>
         </gml:featureMember>
       </wfs:FeatureCollection>
	
	
Filtering the granules
----------------------

The following sub-section will show you how to filter granules, by also limiting the number of the result to a maximum of 2 records and starting from the 3rd value of the list.


* Enter the following command in the terminal::

	curl -v -u admin:Geos -XGET "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules.json?limit=2&offset=2&filter=elevation=35"	

.. note:: Note that the result will be returned in JSON format.
	
The response should look like this::
   
      {  
      "type":"FeatureCollection",
      "bbox":[  
        4.9375,
        44.96875,
        14.9375,
        50.96875
      ],
      "crs":{  
        "type":"name",
        "properties":{  
          "name":"EPSG:4326"
        }
      },
      "features":[  
        {  
          "type":"Feature",
          "geometry":{  
            "type":"Polygon",
            "coordinates":[  
               [  
                 [  
                   4.9375,
                   44.9688
                 ],
                 [  
                   4.9375,
                   50.9688
                 ],
                 [  
                   14.9375,
                   50.9688
                 ],
                 [  
                   14.9375,
                   44.9688
                 ],
                 [  
                   4.9375,
                   44.9688
                 ]
               ]
            ]
          },
          "properties":{  
            "location":"$TRAINING_ROOT\\data\\user_data\\multidim\\polyphemus\\polyphemus_20130301.nc",
            "imageindex":365,
            "time":"2013-03-01T02:00:00.000+0000",
            "elevation":35.0,
            "fileDate":"2018-07-02T07:52:08.000+0000",
            "updated":"2018-07-02T07:52:08.000+0000"
          },
          "id":"NO2.30"
        },
        {  
          "type":"Feature",
          "geometry":{  
            "type":"Polygon",
            "coordinates":[  
               [  
                 [  
                   4.9375,
                   44.9688
                 ],
                 [  
                   4.9375,
                   50.9688
                 ],
                 [  
                   14.9375,
                   50.9688
                 ],
                 [  
                   14.9375,
                   44.9688
                 ],
                 [  
                   4.9375,
                   44.9688
                 ]
               ]
            ]
          },
          "properties":{  
            "location":"%TRAINING_ROOT%\\data\\user_data\\multidim\\polyphemus\\polyphemus_20130301.nc",
            "imageindex":379,
            "time":"2013-03-01T03:00:00.000+0000",
            "elevation":35.0,
            "fileDate":"2018-07-02T07:52:08.000+0000",
            "updated":"2018-07-02T07:52:08.000+0000"
          },
          "id":"NO2.44"
        }
      ]
   }      
		

Harvesting files
----------------

This sub-section will show you how to add more granules to an existing ImageMosaic, through the harvesting process. We will add a new polyphemus sample to the previously configured ImageMosaic.

#. Enter the following command in the terminal::

    curl -v -u admin:Geos -XPOST -H "Content-type: text/plain" -d "%TRAINING_ROOT%/data/user_data/multidim/polyphemus_20130303.nc" "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/external.imagemosaic"

.. note:: Change %TRAINING_ROOT% with you training root folder path.

#. Check that new granules having time = 2013-03-03T00:00:00Z are available, by using a time filter in the request (we will also limit the number of results to 1). Enter the following command in the terminal::

    curl -v -u admin:Geos -XGET "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules.xml?limit=1&filter=time='2013-03-03T00:00:00Z'"

This will result in a GML description of the requested granules, as follows:

   .. code-block:: xml
  
    <?xml version="1.0" encoding="UTF-8"?>
    <wfs:FeatureCollection xmlns:gf="http://www.geoserver.org/rest/granules" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wfs="http://www.opengis.net/wfs" xmlns:gml="http://www.opengis.net/gml">
      <gml:boundedBy>
        <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
          <gml:coord>
            <gml:X>4.9375</gml:X>
            <gml:Y>44.96875</gml:Y>
          </gml:coord>
          <gml:coord>
            <gml:X>14.9375</gml:X>
            <gml:Y>50.96875</gml:Y>
          </gml:coord>
        </gml:Box>
      </gml:boundedBy>
      <gml:featureMember>
        <gf:NO2 fid="NO2.673">
          <gml:boundedBy>
            <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
              <gml:coord>
                <gml:X>4.9375</gml:X>
                <gml:Y>44.96875</gml:Y>
              </gml:coord>
              <gml:coord>
                <gml:X>14.9375</gml:X>
                <gml:Y>50.96875</gml:Y>
              </gml:coord>
            </gml:Box>
          </gml:boundedBy>
          <gf:the_geom>
            <gml:Polygon>
              <gml:outerBoundaryIs>
                <gml:LinearRing>
                  <gml:coordinates>4.9375,44.96875 4.9375,50.96875 14.9375,50.96875 14.9375,44.96875 4.9375,44.96875</gml:coordinates>
                </gml:LinearRing>
              </gml:outerBoundaryIs>
            </gml:Polygon>
          </gf:the_geom>
          <gf:location>%TRAINING_ROOT%\data\user_data\multidim\polyphemus_20130303.nc</gf:location>
          <gf:imageindex>336</gf:imageindex>
          <gf:time>2013-03-03T00:00:00Z</gf:time>
          <gf:z>10.0</gf:elevation>
          <gf:runtime>2013-03-03T00:00:00Z</gf:runtime>
        </gf:NO2>
      </gml:featureMember>
    </wfs:FeatureCollection>


Removing a single granule
-------------------------			

This sub-section will show you how to find a single granule and how to remove it.

#. Enter the following command in the terminal:

	.. code-block:: xml

		curl -v -u admin:Geos -XGET http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules/NO2.671.xml

	This will result in a GML description of the requested granules, as follows:

	.. code-block:: xml
   
		  <?xml version="1.0" encoding="UTF-8"?><wfs:FeatureCollection xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:gf="http://www.geoserver.org/rest/granules" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wfs="http://www.opengis.net/wfs" xmlns:gml="http://www.opengis.net/gml">
			<gml:boundedBy>
			 <gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
			   <gml:coord>
				<gml:X>4.9375</gml:X>
				<gml:Y>44.96875</gml:Y>
			   </gml:coord>
			   <gml:coord>
				<gml:X>14.9375</gml:X>
				<gml:Y>50.96875</gml:Y>
			   </gml:coord>
			 </gml:Box>
			</gml:boundedBy>
			<gml:featureMember>
			 <gf:NO2 fid="NO2.671">
			   <gml:boundedBy>
				<gml:Box srsName="http://www.opengis.net/gml/srs/epsg.xml#4326">
				  <gml:coord>
				   <gml:X>4.9375</gml:X>
				   <gml:Y>44.96875</gml:Y>
				  </gml:coord>
				  <gml:coord>
				   <gml:X>14.9375</gml:X>
				   <gml:Y>50.96875</gml:Y>
				  </gml:coord>
				</gml:Box>
			   </gml:boundedBy>
			   <gf:the_geom>
				<gml:Polygon>
				  <gml:outerBoundaryIs>
				   <gml:LinearRing>
					 <gml:coordinates>4.9375,44.96875 4.9375,50.96875 14.9375,50.96875 14.9375,44.96875 4.9375,44.96875</gml:coordinates>
				   </gml:LinearRing>
				  </gml:outerBoundaryIs>
				</gml:Polygon>
			   </gf:the_geom>
			   <gf:location>%TRAINING_ROOT%\data\user_data\multidim\polyphemus\polyphemus_20130302.nc</gf:location>
			   <gf:imageindex>670</gf:imageindex>
			   <gf:time>2013-03-02T23:00:00Z</gf:time>
			   <gf:elevation>0.0</gf:elevation>
         <gf:fileDate>2013-03-02T00:00:00Z</gf:fileDate>
         <gf:updated>2018-07-02T07:52:08Z</gf:updated>
			 </gf:NO2>
			</gml:featureMember>
		  </wfs:FeatureCollection>

#. Then enter the following command for removing the selected granule::
	
	curl -v -u admin:Geos -XDELETE http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules/NO2.671.xml
   
#. If you try to select the same granule again::

	curl -v -u admin:Geos -XGET http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/polyphemus/coverages/NO2/index/granules/NO2.671.xml

You can see the result will be an **HTTP 404 Not Found** error.

Let's now illustrate how to create a new ImageMosaic coverage stores containing temperature datasets (having both time and elevation dimensions).

Creating a coverage store
-------------------------

This sub-section will show you how to add a new coverage store.

* Enter the following command in the terminal::

    curl -u admin:Geos -XPUT -H "Content-type:application/zip" --data-binary @%TRAINING_ROOT%/data/user_data/multidim/temperature.zip http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/temperature/file.imagemosaic

  The response to this command should look like this:

   .. code-block:: xml
  
    <coverageStore>
      <name>temperature</name>
      <type>ImageMosaic</type>
      <enabled>true</enabled>
      <workspace>
        <name>geosolutions</name>
        <href>http://localhost:8083/geoserver/rest/workspaces/geosolutions.xml</href>
      </workspace>
      <__default>false</__default>
      <url>file:data/geosolutions/temperature</url>
      <coverages>
        <atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/temperature/file/coverages.xml" type="application/xml"/>
      </coverages>
    </coverageStore>

* Enter the following command in the terminal to check the granule's index structure has time and elevation attributes with range (time,endtime and lowz,highz respectively) ::

    curl -v -u admin:Geos -XGET "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/temperature/coverages/temperature/index.xml"

  This will result in an output like this:

   .. code-block:: xml
  
    <Schema>
      <attributes>
        <Attribute>
          <name>the_geom</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>com.vividsolutions.jts.geom.MultiPolygon</binding>
        </Attribute>
        <Attribute>
          <name>location</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>java.lang.String</binding>
          <length>254</length>
        </Attribute>
        <Attribute>
          <name>time</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>java.sql.Timestamp</binding>
          <length>8</length>
        </Attribute>
        <Attribute>
          <name>endtime</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>java.sql.Timestamp</binding>
          <length>8</length>
        </Attribute>
        <Attribute>
          <name>lowz</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>java.lang.Integer</binding>
          <length>9</length>
        </Attribute>
        <Attribute>
          <name>highz</name>
          <minOccurs>0</minOccurs>
          <maxOccurs>1</maxOccurs>
          <nillable>true</nillable>
          <binding>java.lang.Integer</binding>
          <length>9</length>
        </Attribute>
      </attributes>
      <atom:link xmlns:atom="http://www.w3.org/2005/Atom" rel="alternate" href="http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/temperature/coverages/temperature/index/granules.xml" type="application/xml"/>
    </Schema>

As you have seen, we have created a new coverage store mosaic, starting from an ImageMosaic containing TIFF granules.
The next section will illustrate how to create an empty mosaic (that is, containing no datasets, just configuration files) for future data add through harvesting.

Configuring an empty mosaic
---------------------------
Summarizing the steps to configure an empty mosaic:

.. note:: This commands must be executed on the terminal.

#.    Enter the following command in the terminal to configure an empty GOME2 coverage store, using *gome2.zip* available inside `%TRAINING_ROOT%/data/user_data/multidim/` (a zip file only containing indexer, datastore.properties and optionally, auxiliary files). Note the "configure=none" param which avoid automatic configuration of the ImageMosaic coverages.

        .. code-block:: xml

            curl -v -u admin:Geos -XPUT -H "Content-type:application/zip" --data-binary @%TRAINING_ROOT%/data/user_data/multidim/gome2.zip http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/file.imagemosaic?configure=none

#.    Once the ImageMosaic store has been created we can start harvesting a new granule. 

        .. code-block:: xml

            curl -v -u admin:Geos -XPOST -H "Content-type: text/plain" -d "file://%TRAINING_ROOT%/data/user_data/multidim/20130101.METOPA.GOME2.COT.PGL.nc" "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/external.imagemosaic"

#.    Next step is getting the list of the coverages available to be configured on the mosaic, using the following command:

        .. code-block:: xml

            curl -v -u admin:Geos -XGET http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/coverages.xml?list=all

#.    Configuring a coverage (COT of gome2) - once for coverage

        .. code-block:: xml

            curl -v -u admin:Geos -XPOST -H "Content-type: text/xml" -d @"%TRAINING_ROOT%/data/user_data/multidim/cot_coverage.xml" "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/coverages"

    Where `cot_coverage.xml` simply looks like this (note that only the name has been specified to allow for automatic configuration. The name has been get from the previous query where we have asked for listing the available coverages):

        .. code-block:: xml

            <coverage>
              <name>COT</name>
            </coverage>

#.    Updating dimensions for COT coverage - Once for coverage

    .. code-block:: xml
    
        curl -v -u admin:Geos -XPUT -H "Content-type: text/xml" -d @"%TRAINING_ROOT%/data/user_data/multidim/cot_dims.xml" "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/coverages/COT"

    Where `cot_dims.xml` looks like this:

        .. code-block:: xml

            <coverage>
              <name>COT</name>
              <enabled>true</enabled>
              <metadata>
               <entry key="time">
                <dimensionInfo>
                    <enabled>true</enabled>
                    <presentation>LIST</presentation>
                    <units>ISO8601</units>
                </dimensionInfo>
               </entry>
              </metadata>
            </coverage>

            
.. note :: Note that:

            * Step 1 should be performed only once per mosaic.
            * Step 2 is the only step you may need to perform to add more granules.
            * Step 3 should be performed only once per mosaic in case all the granules share the same coverages/variables.
            * Step 4 and 5 should be performed only once for different coverage, to define the coverage configuration. Adding more granules related to the same coverage, as an instance by adding more forecasts of the same coverage, doesn't require a reconfiguration of the coverage. 

Removing an existing mosaic
---------------------------

After creating the empty mosaic with the coverages, you can remove it with the following code from the Terminal

	.. code-block:: xml
	
		curl -v -u admin:Geos -XDELETE "http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2?recurse=true&purge=metadata"
		
The HTTP status code must be 200. 

Try to execute the same GET request as before:

	    .. code-block:: xml

             curl -v -u admin:Geos -XGET http://localhost:8083/geoserver/rest/workspaces/geosolutions/coveragestores/gome2/coverages.xml?list=all

You can see the result will be an **HTTP 404 Not Found** error.
