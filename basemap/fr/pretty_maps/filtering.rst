.. _geoserver.filtering:


Filtrer les Cartes
------------------


Cette séction éxplique les capacités de GeoServer WMS fitering.

#. Naviguez jusq'à GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Allez au lien :guilabel:`Layer Preview` qui se trouve en bas du menu à votre gauche et montrez la couche :guilabel:`geosolutions:WorldCountries` avec OpenLayers 'Common Format'.

   .. figure:: img/filtering1.png
      
	  qui montre l'avant-première des couches GeoServer

   .. figure:: img/filtering2.png

      Montre la couche avec OpenLayers

#. De la :guilabel:`Filter` combo box séléctionnez 'CQL' et inserez la commande suivante dans le champ de texte::

	POP_EST <= 5000000 AND POP_EST >100000

#. cliquez 'Apply Filter' bouton à droite.

   .. figure:: img/filtering3.png

       Résultat du filtre CQL 
 
   .. note:: La requete WMS correspondente est: 
   
	     http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:WorldCountries&styles=&bbox=-180.0,-89.99889902136009,180.00000000000003,83.59960032829278&width=684&height=330&srs=EPSG:4326&format=image/png&CQL_FILTER=POP_EST%20%3C=%205000000%20AND%20POP_EST%20%3E100000

#. Maintennt introduisez la commande suivante dans le champ de texte::

	DISJOINT(the_geom, POLYGON((-90 40, -90 45, -60 45, -60 40, -90 40))) AND strToLowerCase(NAME) LIKE '%on%'
	
#. cliquez 'Apply Filter' bouton à droite.

   .. figure:: img/filtering6.png

      Resultat du filtre CQL 
 
   .. note:: La requete WMS correspondente est: 
   
         http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:WorldCountries&styles=&bbox=-180.0,-89.99889902136009,180.00000000000003,83.59960032829278&width=684&height=330&srs=EPSG:4326&format=image/png&CQL_FILTER=DISJOINT%28the_geom%2C%20POLYGON%28%28-90%2040%2C%20-90%2045%2C%20-60%2045%2C%20-60%2040%2C%20-90%2040%29%29%29%20AND%20strToLowerCase%28NAME%29%20LIKE%20%27%25on%25%27		 

#. De la :guilabel:`Filter` combo box séléctionnez 'OGC' et inserez le filtre suivant dans le champ de texte:

   .. code-block:: xml

	<Filter><PropertyIsEqualTo><PropertyName>TYPE</PropertyName><Literal>Sovereign country</Literal></PropertyIsEqualTo></Filter>

#. cliquez 'Apply Filter' bouton à droite.

   .. figure:: img/filtering4.png

      Resultat du filtre OGC

   .. note:: La requete WMS correspondente est: 
	
	     http://localhost:8083/geoserver/wms?LAYERS=geosolutions%3AComuni&STYLES=&HEIGHT=600&WIDTH=950&SRS=EPSG%3A3003&FORMAT=image%2Fpng&SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&EXCEPTIONS=application%2Fvnd.ogc.se_inimage&FILTER=%3CFilter%3E%3CPropertyIsEqualTo%3E%3CPropertyName%3EPROVINCIA%3C%2FPropertyName%3E%3CLiteral%3Eprato%3C%2FLiteral%3E%3C%2FPropertyIsEqualTo%3E%3C%2FFilter%3E&BBOX=1617269.5547063,4832131.9509527,1715607.0958195,4894239.8716559
	
#. De la :guilabel:`Filter` combo box séléctionnez 'FeatureID' et inserez les  caractéristique suivantes ids dans le champ de texte séparées par la virgule::

	WorldCountries.227,WorldCountries.184,WorldCountries.33

#. Clicuez 'Apply Filter' bouton à droite.

   .. figure:: img/filtering5.png

      Resultat du filtre FeatureID

   .. note:: La requete WMS correspondente est: 

         http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:WorldCountries&styles=&bbox=-180.0,-89.99889902136009,180.00000000000003,83.59960032829278&width=684&height=330&srs=EPSG:4326&format=image/png&FEATUREID=WorldCountries.227,WorldCountries.184,WorldCountries.33