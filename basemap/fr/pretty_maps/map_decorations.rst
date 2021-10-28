.. _geoserver.decorating_maps:



Décoration de la Carte
----------------------


WMS Décoration donne un cadre pour annoter visuellement des images du WMS avec placement absolue, plutôt que spatial. Cet example de décoration comprend ligne d'échelle, legendes, et images.


#. allez en $GEOSERVER_DATA_DIR et créez un nouveau répertoire nommé :guilabel:`layouts` puis créez un nouveau fichier nommé :guilabel:`boulder_ly.xml` à l'intérieur de celui là.


#. Dans le fichier :guilabel:`boulder_ly.xml` introduisez le suivant XML:

   .. code-block:: xml

	<layout>
			<decoration type="image" affinity="top,left" offset="45,8"
					size="174,60">
				<option name="url"
					value="${TRAINING_ROOT}/geosolutions-logo-tx.png" />
			</decoration>

			<decoration type="text" affinity="bottom,right" offset="3,3">
				<option name="message" value="Boulder City" />
				<option name="font-size" value="14" />
				<option name="font-color" value="#FFFFFF" />
				<option name="halo-radius" value="1" />
				<option name="halo-color" value="#000000" />
			</decoration>

			<decoration type="scaleline" affinity="bottom,left" offset="3,3" />

			<decoration type="legend" affinity="top,right"
					offset="6,6" size="auto" />
	</layout>

#. Sauvez et fermez le ficihier. 

#. allez à **Layer Preview** pour voire l'avant-première de la nouvelle décoration de la carte sur :guilabel:`geosolutions:Mainrd` couche. Quand le layout :guilabel:`boulder_ly.xml` est défini, faites une enquete en ajoutant :guilabel:`&format_options=layout:boulder_ly` aux paramètres.


   .. figure:: img/decoration2.png
         
      Décoration de la carte
      
   La requete:
   
      http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:Mainrd&styles=&bbox=3048474.661,1226045.092,3095249.0,1279080.5&width=451&height=512&srs=EPSG:2876&format=application/openlayers&format_options=layout:boulder_ly

.. note:: Rapprochez-vous jusq'à que la couche et la légende apparaîssent puisque pour cette couche nous avons des règles fondées sur scale_denominator. Vous pouvez appliquer ce format_layout à n'importe quelle couche, mais soyez prudent avec les overalys puisque vous allez imprimer toutes les légendes sur la droite en haut de la carte.