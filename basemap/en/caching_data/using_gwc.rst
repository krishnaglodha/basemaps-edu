.. module:: geoserver.using_gwc

.. _geoserver.using_gwc:

Using GeoWebCache with OpenLayers, Google Maps and Virtual Earth
----------------------------------------------------------------

In this section will learn how to use GeoWebCache with OpenLayers, GoogleMaps and Virtual Earth through the simple examples.

GeoWebCache with OpenLayers
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``OpenLayers`` tiling the WMS request splitting it into several sub-requests sent to the geospatial server. To ensure that ``GeoWebCache`` returns tiles from the cache consistent with OpenLayers tiles, we should respect some constraints including:

	- The tiles of the OpenLayers and GeoWebCache should have the same size in terms of pixels, both in terms of coverage area.
	- The tiles of the OpenLayers and GeoWebCache should be aligned in the same grid.

.. note::
   
   This ensures not only an optimal caching of tiles but also an optimal reuse of the GeoWebCache tiles with other clients as well as OpenLayers.

#. Create an wmts.html file and enter the following code:

   .. code-block:: html

    <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="utf-8"/>
        <title>Counties of Colorado:EPSG:4326 image/png</title>
        <link rel="stylesheet" href="lib/ol3/css/ol.css" type="text/css">	
        <!-- Use ol.js for production, and ol-debug.js for development
        	script src="lib/ol3/ol.js" type="text/javascript"></script-->
        <script src="lib/ol3/ol-debug.js" type="text/javascript"></script>
	
			<style>

        /* Map settings */
        #map {
           width: 600px;
           height: 400px;
        }
      </style>
    </head>
    <body>
      <div id="map" class="map"></div>

      <script>
          var projection = ol.proj.get('EPSG:4326');
          var projectionExtent = projection.getExtent();
          var mapExtent =  [-105.70160457542876, 39.799058451017345, -104.99644707392322, 40.301219114868005];
          var matrixIds = new Array(22);
        
          for (var z = 0; z < 22; ++z) {        
            matrixIds[z] = "EPSG:4326:" + z;;
          }
        
          resolutions = [
          	0.703125, 0.3515625, 0.17578125, 0.087890625,        
          	0.0439453125, 0.02197265625, 0.010986328125,
          	0.0054931640625, 0.00274658203125, 0.001373291015625,
          	6.866455078125E-4, 3.4332275390625E-4, 1.71661376953125E-4,
          	8.58306884765625E-5, 4.291534423828125E-5, 2.1457672119140625E-5,
        	  1.0728836059570312E-5, 5.364418029785156E-6, 2.682209014892578E-6,
          	1.341104507446289E-6, 6.705522537231445E-7, 3.3527612686157227E-7
          ];
        
          bouldersLayerWMTS = new ol.layer.Tile({
          	source: new ol.source.WMTS({
          	url: 'http://localhost:8083/geoserver/gwc/service/wmts',
        	  layer: 'boulder',
          	matrixSet: 'EPSG:4326',
          	format: 'image/png',
          	projection: projection,
        	  tileGrid: new ol.tilegrid.WMTS({
        	    origin: ol.extent.getTopLeft(projectionExtent),
           	  resolutions: resolutions,
        	    matrixIds: matrixIds
        	  })
            })
        });
		
         var map = new ol.Map({
          layers: [bouldersLayerWMTS],
          target: 'map',
          view: new ol.View({
          	extent: mapExtent,
           	projection: projection,
          	center: [-105.4, 40.03],		
          	zoom: 10
          })
        });

        </script>
      </body>
     </html>

#. Save the file in the **Map** folder located in:

  * Linux::

      /opt/tomcat_geoserver/webapps/Map

  * Windows::

      %TRAINING_ROOT%/tomcat/instances/instance1/webapps/Map

#. Open the working map in your web browser at `WMTS Page <http://localhost:8083/Map/wmts.html>`_.

   .. figure:: img/caching1.png

      GeoWebCache with OpenLayers 

   .. note::

      The default values are the following:
	
	- **Tile width**: 256px
	- **Tile height**: 256px
	- **Tile origin**: EPSG:4326 -180,-90 ; EPSG:900913: -20037508.34,-20037508.34
	- **Resolution**: EPSG:4326 180 / (2^k) ; EPSG:900913: (2*20037508.34)/(2^k), k represents the zoomlevel.

GeoWebCache with Google Maps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GeoWebCache also supports Google Maps client. An example of Google Maps client, which can be used as a base (template) for more complex client, is as follows:

#. Create a new index.html file and enter the following code:

  .. code-block:: html

	<html xmlns="http://www.w3.org/1999/xhtml">
		<head>
			<meta http-equiv="content-type" content="text/html; charset=utf-8"/>
			<title>Google Maps with GeoWebCache</title>
			<script src="https://maps.googleapis.com/maps/api/js?sensor=false" type="text/javascript"></script>
			<script type="text/javascript">
				function initialize() {
					var myOptions = {
						  zoom: 9,
						  center: new google.maps.LatLng(40, -105.5),
						  zoomControl: true,
						  mapTypeId: google.maps.MapTypeId.ROADMAP
						}
					var map = new google.maps.Map(document.getElementById("map_canvas"), myOptions);

					var countiesTiles = {
						 getTileUrl: function(coord, zoom) {
						   return "http://localhost:8083/geoserver/gwc/service/gmaps?layers=boulder&" +
							"zoom=" + zoom + "&x=" + coord.x + "&y=" + coord.y + "&format=image/png";
						 },
						 tileSize: new google.maps.Size(256, 256),
						 isPng: true,
						 opacity: 0.5
					   };

				   var customMapType = new google.maps.ImageMapType(countiesTiles);
				   map.overlayMapTypes.insertAt(0, customMapType);

				}
			</script>
				</head>
		<body onload="initialize()" onunload="GUnload()">
		   <div id="map_canvas" style="width: 596px; height: 367px"></div>
		</body>
	</html>

  .. note:: 

       The ``/service/gmaps`` path tells the dispatcher to use the **gwcGmaps**  service for indexing and generating tiles compatible with this type of requests.

#. Save the file in the **Map** folder (same as previous section).

#. Open the working map in your web browser at `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/caching2.png
	  
      GeoWebCache with Google Maps


GeoWebCache with Virtual Earth
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GeoWebCache can also support requests from MS Virtual Earth clients. An example of a client, which can be used as a base (template) for clients more complex; is the following:

#. Create a new index.html file and enter the following code:


   .. code-block:: html

        <html xmlns="http://www.w3.org/1999/xhtml">
            <head>
                    <title>Virtual Earth with GeoWebCache</title>
                    <script type='text/javascript' src='http://www.bing.com/api/maps/mapcontrol'></script>
                    <script type="text/javascript">
                             var map, tileLayer;
                             var tileLayerURL = 'http://localhost:8083/geoserver/gwc/service/ve?quadkey={quadkey}&format=image/png&layers=boulder';
                     function GetMap(){
				     var map = new Microsoft.Maps.Map('#myMap', { 
						credentials:'Your Bing Maps Key',
						center:new Microsoft.Maps.Location(40, -105.5),
						mapTypeId: Microsoft.Maps.MapTypeId.road,
						zoom: 9
				     });
			             var tileSourceSpec = new Microsoft.Maps.TileSource({
			                uriConstructor: tileLayerURL
			             });
				     var tileSourceLayer = new Microsoft.Maps.TileLayer({
			                title:'TITLE_OF_LAYER',
			                mercator: tileSourceSpec,
			                opacity: 0.5
			             });
				     map.layers.insert(tileSourceLayer);
                             }
                    </script>
            </head>
            <body onload="GetMap();">
                          <div id='myMap' style="position:relative; width:596px; height:367px;"></div>
            </body>
        </html>

   .. note::

      The ``/service/ve`` path tells you to use the service **gwcVEConverter** for indexing and generating tiles compatible with this type of requests.


#. Save the file in the **Map** folder replacing the created one in previous section.

#. Open the working map in your web browser at `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/caching3.png

      GeoWebCache with Virtual Earth
   
