.. module:: geoserver.analyze

.. _geoserver.analyze:

Analyser Votre Carte
--------------------

Comme démontré dans la section précédente, une carte est généré réunissant markup, declarations de style, et codes d'initialization. Nous allons examiner chacune de ces parties de façon un peu plus détaillée.

Markup de la carte
^^^^^^^^^^^^^^^^^^

Le markup pour la carte dans le :ref:`previous example <geoserver.ol_map>` génère un élément de document unique:

.. code-block:: html

	<div id="map-id"></div>

.. note:: 

   Cet élément ``<div>``   servira comme récipient pour notre carte viewport. Ici, nous utilisons un élément <div>, mais le récipient pour le viewport peut etre n'importe quel élément block-level. Dans ce cas, nous donnons au récipient un attribute ``id``  afin que nous puissions y faire référence facilement ailleurs.

Style de la Carte
^^^^^^^^^^^^^^^^^

OpenLayers est livré avec un défaut stylesheet qui spécifie dans quel style doivent etre les éléments liés à la carte. Nous avons explicitement inclus le stylesheet dans le map.html page (``<link rel="stylesheet" href="lib/openlayers/theme/default/style.css" type="text/css">``).

OpenLayers ne  fait pas de suppositions sur la taille de votre carte. En raison de ça, en suivant le stylesheet de défaut, nous avons bésoin d'inclure au moins une déclaration de style personnalisée pour donner à la carte de la place sur la page.

.. code-block:: html

     <link rel="stylesheet" href="lib/openlayers/theme/default/style.css" type="text/css">
     <style>
         #map-id {
             width: 512px;
             height: 300px;
         }
     </style>

Dans ce cas, nous utilisons la valeur récipient de la carte **id** comme sélecteur, et nous indiquons la largeur(**512px**) et la hauteur (**300px**) du récipient de la carte.

Les déclarations de style sont directement inclues dans le **<head>** du document. Dans la plupart des cas, votres déclarations de style de la carte sera une partie d'un thème d'un site Web plus grand lié en feuilles de style externes.

.. note::

   OpenLayers applique zero marge et de remplissage dans l'élément que vous utilisez comme récipient viewport. Si vous voulez que votre carte soit entourée par un certain marge, envelopper le viewport récipient dans un autre élément ayant des marges et de remplissage.

Initialization de la carte
^^^^^^^^^^^^^^^^^^^^^^^^^^

La prochaine étape pour générer votre carte est d'inclure quelque code d'initialization. Dans notre cas, nous avons inclus un élément **<script>**  au fond de notre document **<body>** pour le faire:

.. code-block:: html

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

.. note:: 

   L'ordre de ces étapes est important. Avant que notre script personnalisé puisse être exécuté, il faut charger la bibliothèque de OpenLayers. Dans notre example, la bibliothèque OpenLayers est chargée dans le ``<head>`` de notre document avec ``<script src="lib/openlayers/OpenLayers.js"></script>``. De même, le code d'initialisation de notre carte personnalisée (au-dessus) ne peut pas fonctionner jusqu'à ce que l'élément document qui sert de récipient viewport, dans ce cas, ``<div id="map-id"></div>``, est prêt. En incluant le code d'initialisation à la fin du document ``<body>``, nous nous assurons que la bibliothèque soit chargée et que le récipient viewport sout pret avant de produire notre carte.

Voyons plus en détail à ce que le script d'initialisation de la carte est en train de faire. La première partie du script crée un nouveau object OpenLayers.Map:

.. code-block:: html

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

	 var map = new OpenLayers.Map('map-id', options);

- **bounds**: représente les boîtes englobantes la carte.
- **options**: représente les options de la carte de definir le réglage de la carte.
- **map**: Nous utilisons la valeur de l'attribut id du récipient viewport pour dire au constructeur de la carte où rendre la carte. Dans ce cas, nous passons la string value "map-id" au constructeur de la carte. Cette syntaxe est un raccourci pour notre commodité. Nous pourrions être plus explicite et fournir une référence directe à l'élément (e.g. document.getElementById("map-id")).

Les prochaines lignes créent une couche qui sera affiché dans notre carte:

.. code-block:: html

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

La partie importante à comprendre est que notre carte est une collection de couches. Pour voire une carte, il faut inclure au moins une couche. Pour OpenLayers une couche WMS, comme dans cet exemple, est définie par:

	- Le nom: ``Counties of Colorado - Untiled``.
	- Le WMS serveur URL: ``http://localhost:8083/geoserver/wms``.
	- Les params: ``{width: '426', srs: 'EPSG:4269', layers: 'geosolutions:ccounties', height: '512', styles: '', format:'image/png'}``.
	- Les options: ``{singleTile: true, ratio: 1}``.

La partie optionnelle est de définir les contrôles de la Carte. Dans ce cas, on définit le contrôle de Navigation pour  déplacer et zoomer la Carte.

.. code-block:: html

	 map.addControl(new OpenLayers.Control.Navigation());

La dernière étape est de fixer les limites géographiques (xmin,ymin,xmax,ymax)de l'affichage de la carte. Cette mesure specifie le rectangle de délimitation minimale d'une région de la carte. Il ya un certain nombre de façons pour spécifier la mesure initiale. Dans notre exemple, nous utilisons une simple demande pour zoomer au maximum. Par défaut, le maximum est le monde en coordonnées géographiques:

.. code-block:: html

         map.zoomToExtent(bounds);

