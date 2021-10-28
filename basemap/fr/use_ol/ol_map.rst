.. module:: geoserver.ol_map

.. _geoserver.ol_map:


Créer un simple OpenLayers
----------------------------

En OpenLayers, une Carte est une ensemble de couches à traiter avec les interactions de l'utilisateur. Une carte est généré avec trois éléments de base: markup, declarations du style, et code d'initialization de la carte.


Example de travail
^^^^^^^^^^^^^^^^^^

Jetons un coup d'oeil à un vrai example de travail d'une Carte OpenLayers.

.. code-block:: html

		<html>
		   <head>
			 <title>Counties of Colorado</title>
			 <link rel="stylesheet" href="lib/openlayers/theme/default/style.css" type="text/css">
			 <style>
				 #map-id {
					 width: 512px;
					 height: 300px;
				 }
			 </style>
			 <script src="lib/openlayers/OpenLayers.js"></script>
			</head>
			<body>
			 <h1>Counties of Colorado</h1>
			 <div id="map-id"></div>
			 <script>
				 var bounds = new OpenLayers.Bounds(
					-109.06, 36.992,
					-102.041, 41.003
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
							 layers: 'geosolutions:Counties',
							 height: '512',
							 styles: '',
							 format:'image/png'
						 },
						 {singleTile: true, ratio: 1}
				 );

				 map.addLayer(ccounties);

				 // build up all controls
				 map.addControl(new OpenLayers.Control.PanZoomBar({
					 position: new OpenLayers.Pixel(2, 15)
				 }));
				 map.addControl(new OpenLayers.Control.Navigation());
				 map.addControl(new OpenLayers.Control.Scale());
				 map.addControl(new OpenLayers.Control.MousePosition());
				 map.zoomToExtent(bounds);

			 </script>
			</body>
		</html>

#. Copiez le texte ci-dessus dans un nouveau fichier nommé index.html, et sauvez-le dans le répertoire:guilabel:`/opt/tomcat-geoserver/webapps/Map`.

#. Ouvrez la carte dans votre navigateur Web à `Welcome Page <http://localhost:8083/Map/>`_.

   .. figure:: img/ol1.png

      Une carte de visualisation d'images Counties of Colorado
