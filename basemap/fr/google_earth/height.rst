.. module:: geoserver.height

.. _geoserver.height:

Génération d'une élévation KML
-----------------------------------

Cette section explique comment utiliser les modèles de GeoServer en conjonction avec le soutien Google Earth pour 'height' pour créer une height visualization

#. Créez le fichier ``height.ftl`` dans le repertoire ``$GEOSERVER_DATA_DIR/workspaces/geosolutions/states/states/`` 

#. Ouvrez l' ``height.ftl``  avec un éditeur de texte et saisissez le contenu suivant::

   ${PERSONS.value?number / 100.0}

#. sauvez ``height.ftl`` et accédez à `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_.

#. Cliquez le lien :guilabel:`KML` à côté du layer :guilabel:`geosolutions:states`.

   .. figure:: img/height2.png

     générer KML avec la carte preview

#. Dans la boîte de dialogue résultante choisir  :guilabel:`Open with Google Earth` et :guilabel:`OK`

#. Zoomer jusqu'à états apparaissent sous forme vectorielle, puis inclinez la vue pour voir les extrudé.

   .. figure:: img/height3.png

      KML Ouverture de GeoServer avec Google Earth

   .. figure:: img/height1.png
      :width: 650

      Regarder le layer de la populations dans Google Earth
