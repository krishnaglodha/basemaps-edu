.. module:: geoserver.using_gwc

.. _geoserver.using_gwc:

Using GeoWebCache with OpenLayers, Google Maps and Virtual Earth
----------------------------------------------------------------

In this section will learn how to use GeoWebCache with OpenLayers and GoogleMaps through the simples examples.

GeoWebCache with OpenLayers
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``OpenLayers`` tiling the WMS request splitting it into several sub-requests sent to the geospatial server. To ensure that ``GeoWebCache`` returns tiles from the cache consistent with OpenLayers tiles, we should respect some constraints including:

	- The tiles of the OpenLayers and GeoWebCache should have the same size in terms of pixels, both in terms of coverage area.
	- The tiles of the OpenLayers and GeoWebCache should be aligned in the same grid.

.. note::
   
   This ensures not only an optimal caching of tiles, but also an optimal reuse of the GeoWebCache tiles with other clients as well as OpenLayers.

#. Crate an index.html file and enter the following code:

   .. code-block:: html
   
		<html>
			<head>
				<meta http-equiv="imagetoolbar" content="no">

				<title>Counties of Colorado:EPSG:4269 image/png</title>

				<style type="text/css">
					body { font-family: sans-serif; font-weight: bold; font-size: .8em; }
					body { border: 0px; margin: 0px; padding: 0px; }
					#map { width: 85%; height: 85%; border: 0px; padding: 0px; }
				</style>

				<script src="lib/openlayers/OpenLayers.js"></script>
				<script type="text/javascript">
						var map, layer;

						function init(){
							var mapOptions = {
								resolutions: [
									0.703125, 0.3515625, 0.17578125, 0.087890625, 
									0.0439453125, 0.02197265625, 0.010986328125, 
									0.0054931640625, 0.00274658203125, 0.001373291015625, 
									6.866455078125E-4, 3.4332275390625E-4, 1.71661376953125E-4, 
									8.58306884765625E-5, 4.291534423828125E-5, 2.1457672119140625E-5, 
									1.0728836059570312E-5, 5.364418029785156E-6, 2.682209014892578E-6, 
									1.341104507446289E-6, 6.705522537231445E-7, 3.3527612686157227E-7
								],
								projection: new OpenLayers.Projection('EPSG:4326'),
								maxExtent: new OpenLayers.Bounds(-180.0,-90.0,180.0,90.0),
								units: "degrees",
								controls: []
							};

							map = new OpenLayers.Map('map', mapOptions );

							map.addControl(new OpenLayers.Control.PanZoomBar({
									position: new OpenLayers.Pixel(2, 15)
							}));
							
							map.addControl(new OpenLayers.Control.Navigation());
							var demolayer = new OpenLayers.Layer.WMS(
											"City of Boulder","http://localhost:8083/geoserver/gwc/service/wms",
											{layers: 'boulder', format: 'image/png' },
											{tileSize: new OpenLayers.Size(256,256)}
							);

							map.addLayer(demolayer);
							map.zoomToExtent(new OpenLayers.Bounds(-105.70160457542876,39.799058451017345,-104.99644707392322,40.301219114868005));
						}
				</script>
			</head>
			<body onload="init()">
					<div id="map"></div>
			</body>
		</html>

#. Save the file in the **Map** folder located in :guilabel:`<TRAINING_ROOT>/tomcat-7.0.72/instances/instance1/webapps` folder.

#. Open the working map in your web browser at `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/caching1.png
      :width: 600
	  
      GeoWebCache with OpenLayers 

   .. note::

      The default values are the following:
	
	- **Tile width**: 256px
	- **Tile height**: 256px
	- **Tile origin**: EPSG:4326 -180,-90 ; EPSG:900913: -20037508.34,-20037508.34
	- **Resolution**: EPSG:4326 180 / (2^k) ; EPSG:900913: (2*20037508.34)/(2^k), k represents the zoomlevel.

GeoWebCache with Google Maps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GeoWebCache also supports Google Maps client. An example of Google Maps client, which can be used as a base (template) for more complex client is as follows.

#. Crate a new index.html file and enter the following code:

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
		   <div id="map_canvas" style="width: 800px; height: 500px"></div>
		</body>
	</html>

  .. note:: 

       The ``/service/gmaps`` path tells the dispatcher to use the **gwcGmaps**  service for indexing and generating tiles compatible with this type of requests.

#. Save the file in the **Map** folder replacing the previous created.

#. Open the working map in your web browser at `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/caching2.png
      :width: 600
	  
      GeoWebCache with Google Maps


GeoWebCache with Virtual Earth
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GeoWebCache can also support requests from MS Virtual Earth clients. An example of a client, which can be used as a base (template) for clients more complex is the following.

#. Crate a new index.html file and enter the following code:


   .. code-block:: html

      <html xmlns="http://www.w3.org/1999/xhtml">
		  <head>
			  <title>Virtual Earth with GeoWebCache</title>
			  <script type="text/javascript" src="http://dev.virtualearth.net/mapcontrol/mapcontrol.ashx?	v=6.1"></script>
			  <script type="text/javascript">
				   var map, tileLayer;
				   var tileLayerURL = 'http://localhost:8083/geoserver/gwc/service/ve?quadkey=%4&format=image/png&layers=boulder';
			   function GetMap(){
					   var map = new VEMap('myMap');
					   map.LoadMap(new VELatLong(40, -105.5) , 9, 'r' , false);
					   var tileSourceSpec = new VETileSourceSpecification('TITLE_OF_LAYER', tileLayerURL);
					   tileSourceSpec.Opacity = 0.5;
					   map.AddTileLayer(tileSourceSpec, true);
				   } 
			  </script>
		  </head>
		  <body onload="GetMap();">
				<div id='myMap' style="position:relative; width:800px; height:500px;"></div>
		  </body>
	  </html>

   .. note::

      The ``/service/ve`` path tells you to use the service **gwcVEConverter** for indexing and generating tiles compatible with this type of requests.


#. Save the file in the **Map** folder replacing the previous created.

#. Open the working map in your web browser at `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/caching3.png
      :width: 600
      
	  GeoWebCache with Virtual Earth
   
