.. module:: geoserver.georss

.. _geoserver.georss:

Gestion de GeoRss
-----------------


**RSS** est l'un des plus populaires formats basés XML pour distribuer des contenu Web et définit une structure pour contenir un mélange de nouvelles, chaqune composée par des divers domaines.
**GeoRSS** a été conçu comme un moyen léger, entraîné par la community pour étendre les feeds existants avec des informations géographiques. 

cette section vous enseignera comment gérer GeoRSS avec OpenLayers l'aide d'un exemple simple.

.. code-block:: html

	<html>
	   <head>
		 <title>Point landmarks GeoRSS</title>
		 <link rel="stylesheet" href="lib/openlayers/theme/default/style.css" type="text/css">
			<style type="text/css">
				/* General settings */
				body {
					font-family: Verdana, Geneva, Arial, Helvetica, sans-serif;
					font-size: small;
				}
				
				/* Map settings */
				#map {
					clear: both;
					position: relative;
					width: 407px;
					height: 512px;
					border: 1px solid black;
				}
			</style>
		 <script src="lib/openlayers/OpenLayers.js"></script>
		</head>
		<body>
		 <h1>Point landmarks GeoRSS</h1>
		 <div id="map-id"></div>
			<script>
				 var bounds = new OpenLayers.Bounds(
					-105.84, 39.54,
					-104.46, 40.16
				 );
				 
				 var options = {
						 controls: [],
						 maxExtent: bounds,
						 maxResolution: 0.02741796875,
						 projection: "EPSG:4269",
						 units: 'm'
				 };

				 map = new OpenLayers.Map('map-id', options);

				 var ccounties = new OpenLayers.Layer.WMS(
						 "Counties of Colorado - Untiled", "http://localhost:8083/geoserver/wms",
						 {
							 width: '426',
							 srs: 'EPSG:4269',
							 layers: 'geosolutions:ccounties',
							 height: '512',
							 styles: '',
							 format:'image/png'
						 },
						 {singleTile: true, ratio: 1}
				 );

				var bbuildings = new OpenLayers.Layer.WMS(
						"Boulder buildings - Untiled", "http://localhost:8083/geoserver/wms",
						{
							layers: 'geosolutions:bbuildings',
							format:'image/png',
							transparent: true,
							styles: 'polygon'
						},
						{singleTile: true, buffer:0, ratio: 1} 
				);
				
				var value = "data/bptlandmarks_georss.xml";
				var parts = value.split("/");
				var bptlandmarks = new OpenLayers.Layer.GeoRSS( parts[parts.length-1], value);

				map.addLayers([ccounties, bbuildings, bptlandmarks]);
				
				// build up all controls
				map.addControl(new OpenLayers.Control.PanZoomBar({
					position: new OpenLayers.Pixel(2, 15)
				}));			
				map.addControl(new OpenLayers.Control.Navigation());
				
				map.zoomToExtent(bounds);
			</script>
		</body>
	</html>

#. Créez un nouveau fichier ``index.html`` et copiez le texte au-dessus.

#. Sauvez le nouveau fichier et remplacez celui créé précédemment  :ref:`geoserver.ol_map`.

#. Ouvrez la carte dans votre web browser à `Welcome Page <http://localhost:8083/Map/>`_.  

   .. figure:: img/ol6.png

      Une carte avec couche GeoRSS.

   .. figure:: img/ol7.png

      Une carte zoomée avec couche GeoRSS.  

   .. note:: 

      Le code à noter est::

		var value = "data/bptlandmarks_georss.xml";
		var parts = value.split("/");
		var bptlandmarks = new OpenLayers.Layer.GeoRSS( parts[parts.length-1], value);

      défini pour ajouter la couche GeoRSS à la carte. Le fichier georss.xml est produit par GeoServer en utilisant la requête suivante::

      	  http://localhost:8083/geoserver/wms/reflect?layers=geosolutions:bptlandmarks&format=rss&CQL_FILTER=MTFCC=%27C3061%27



