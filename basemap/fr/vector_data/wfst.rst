.. _geoserver.vector_data.wfst:

Modification des types d'entités
--------------------------------

GeoServer propose un service de Web Feature entièrement transactionnel (WFS-T) qui permet aux utilisateurs d'insérer / supprimer / modifier les types d'entités disponible.
Cette section présente quelques-uns des WFS-T capacités et des interactions avec les clients SIG de GeoServer.

#. Ouvrez le client SIG `uDig <http://udig.refractions.net>`_ .

   .. figure:: img/wfs-t1.jpg
      :width: 600

      uDig SIG Desktop Client

#. Ajouter GeoServer WFS au catalogue.

   .. figure:: img/wfs-t2.jpg
      :width: 800
      
      Sélection des données de services Web Feature

#. Insérez dans la zone de texte URL l'adresse suivante::
   
     http://localhost:8083/geoserver/wfs?request=GetCapabilities


   .. figure:: img/wfs-t3.jpg
      :width: 800
      
      l'URL WFS
      
   Sélectionnez le `Mainrd` de la liste
   
   .. figure:: img/wfs-t4.jpg
   
      Données WFS présentés dans le catalogue uDig

#. Chargez le `Mainrd` Feature Type en utilisant *drag-n-drop*.

   .. figure:: img/wfs-t5.jpg
      :width: 800
      
      Importation de `Mainrd` dans la carte

#. Effectuer un zoom sur la partie supérieure droite de la couche.

   .. figure:: img/wfs-t6.jpg

      zoom avant ...
   
   .. figure:: img/wfs-t7.jpg

      zoom avant ...

#. En utilisant l'outil :guilabel:`Select and Edit Geometry` essayez de déplacer / ajouter / supprimer des vertex à la petite ligne au centre de l'écran.

   .. figure:: img/wfs-t8.jpg
   
      Jouer avec la géométrie

#. Une fois terminé utilisez l'outil :guilabel:`Commit` pour conserver les modifications sur GeoServer.

   .. figure:: img/wfs-t9.jpg

      commettre changements à travers le protocole WFS-T

#. Utilisez GeoServer **Layer Preview** pour afficher les modifications sur la couche `Mainrd`.
   
   .. Attention:: Afin de voir les lignes de rues, vous devez spécifier le style de la `line` à la demande de GetMap.
   
   .. figure:: img/wfs-t10.jpg

     Voici les modifications apportées au type de fonction `Mainrd` 

#. Sur uDig regarder les valeurs des attributs de fonction à l'aide de l'outil :guilabel:`Info`.

   .. figure:: img/wfs-t11.jpg
      :width: 800
	  
      Récupération Type d'entité informations de l'interface uDig

#. Utilisez "Poster" à partir de Firefox afin de délivrer une **Update** une requête de type d'entité à la WFS-T. Envoyer via HTTP POST le code XML suivant, qui met à jour toutes les routes étiquetés comme ``Monarch Rd`` ào ``Monarch Road``

   .. code-block:: xml

			<wfs:Transaction xmlns:topp="http://www.openplans.org/topp" xmlns:ogc="http://www.opengis.net/ogc" xmlns:wfs="http://www.opengis.net/wfs" service="WFS" version="1.0.0">
        <wfs:Update typeName="geosolutions:Mainrd">
              <wfs:Property>
                <wfs:Name>LABEL_NAME</wfs:Name>
                <wfs:Value>Monarch Road</wfs:Value>
              </wfs:Property>
              <ogc:Filter>
                <ogc:PropertyIsEqualTo>
                    <ogc:PropertyName>LABEL_NAME</ogc:PropertyName>
                    <ogc:Literal>Monarch Rd</ogc:Literal>
                </ogc:PropertyIsEqualTo>
              </ogc:Filter>
        </wfs:Update>
      </wfs:Transaction>

   .. figure:: img/wfs-t12.png

   .. note:: Le `Firefox Poster Addon <https://addons.mozilla.org/en-US/firefox/addon/2691>`_ est une très bonne alternative à cURL pour ceux qui préfèrent les outils graphiques à la place de ceux de la ligne de commande.

      Émettre  Update Feature Type request à the WFS-T

#. Demandez l'information à nouveau en utilisant l'outil uDig :guilabel:`Info` ...

   .. note:: Pour émettre une demande GetFeatureInfo de l'outil MapPreview OpenLayers, juste à gauche-cliquez sur la ligne.

   .. figure:: img/wfs-t13.jpg
      :width: 800

      L'obtention de la fonction de mise à jour d'informations de type de l'interface uDig

#. Enfin, obtenir les informations de type d'entité en utilisant l'opération GetFeatureInfo émis directement par `Map Preview <http://localhost:8083/geoserver/mapPreview.do>`_ .

   .. figure:: img/wfs-t14.jpg

     L'obtention de la fonction de mise à jour d'informations de type d'OpenLayers MapPreview GetFeatureInfo

