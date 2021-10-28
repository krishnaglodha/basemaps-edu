.. module:: geoserver.overlay_vector

.. _geoserver.overlay_vector:

Superposition des Données Vectorielles
--------------------------------------

Dans cette section nous allons apprendre à travailler avec les données vectorielles dans la Carte OpenLayers.

Ajouter une Superposition
^^^^^^^^^^^^^^^^^

#. Ouvrez le ``index.html``, créé dans la séction :ref:`geoserver.ol_map`, et remplacez le script d'initialization de la carte avec le suivant:

   .. note:: Pour mieux voire la carte utilisée dans cet exemple, vous devez changer le style de la couche *geosolutions:bbuildings* à l'aide du style ``polygon``.

   .. code-block:: html

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
						 layers: 'geosolutions:Counties',
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

			map.addLayers([ccounties, bbuildings]);
			
			// build up all controls
			map.addControl(new OpenLayers.Control.PanZoomBar({
				position: new OpenLayers.Pixel(2, 15)
			}));
			map.addControl(new OpenLayers.Control.Navigation());
			map.addControl(new OpenLayers.Control.Scale());
			map.addControl(new OpenLayers.Control.MousePosition());
			
			map.zoomToExtent(bounds);
		</script>

   .. note:: Fairtes attention lors lors-que vous introduisez plusieurs couches dans la carte. Charger autant de données peut rendre le navigateur instable s'il n'est pas contrôlé.

#. Puis remplacez l'étiquette du style en utilisant ce qui suit:
   
   .. code-block:: html
   
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

#. Sauvez le fichier et ouvrez la carte and dans votre navigateur Web à `Welcome Page <http://localhost:8083/Map/>`_.  

   .. figure:: img/ol2.png

      Une carte de Counties of Colorado avec une superposition.

   .. figure:: img/ol3.png

      Une carte zoomée Counties of Colorado avec une superposition.  

   .. note:: 

      Le code ajouté::

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

      définit une superposition à travers le parametre ``transparent``  mis en ``true``.

Ajouter les caractéristiques vectorielles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Ouvrez le ``index.html`` et remplacez le scipt d'initialization de la Carte avec ce qui suit:

   .. code-block:: html

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
		
		var bptlandmarks = new OpenLayers.Layer.Vector("bptlandmarks", {
			 strategies: [new OpenLayers.Strategy.Fixed()],
			 protocol: new OpenLayers.Protocol.HTTP({
				 url: "data/bptlandmarks.json",
				 format: new OpenLayers.Format.GeoJSON()
			 })
		});

		map.addLayers([ccounties, bbuildings, bptlandmarks]);
		
		// build up all controls
		map.addControl(new OpenLayers.Control.PanZoomBar({
			position: new OpenLayers.Pixel(2, 15)
		}));
		
		map.addControl(new OpenLayers.Control.Navigation());
		map.addControl(new OpenLayers.Control.Scale());
		map.addControl(new OpenLayers.Control.MousePosition());
		
		map.zoomToExtent(bounds);
	</script>

#. Sauvez le fichier et ouvrez la carte dans votre navigateur Web à `Welcome Page <http://localhost:8083/Map/>`_. 

   .. figure:: img/ol5.png

      Une carte Counties of Colorado avec points de repère caractéristiques vectorielles.

   .. figure:: img/ol4.png

      Une carte zoomée Counties of Colorado avec points de repère caractéristiques vectorielles.  

   .. note:: 

      Le code ajoint::

		var bptlandmarks = new OpenLayers.Layer.Vector("Point landmarks", {
			 strategies: [new OpenLayers.Strategy.Fixed()],
			 protocol: new OpenLayers.Protocol.HTTP({
				 url: "data/bptlandmarks.json",
				 format: new OpenLayers.Format.GeoJSON()
			 })
		});

      définit une couche vectorielle chargée du fichier JSON. Le fichier bptlandmarks.json est produit par GeoServer en utilisant using la requete suivante::

      	  http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=geosolutions:bptlandmarks&outputFormat=json&CQL_FILTER=MTFCC=%27C3061%27
