.. module:: geoserver.pretty_maps
   :synopsis: Styling en unités du monde réel


Styling en unités du monde réel
===============================

Par défaut SLD interprète toutes tailles exprimées dans la feuille de style (e.g., largeurs des lignes, taille des symboles) comme s'ils étaient des pixels sur la carte.

Il est toutefois possible faire utiliser à la feuille de style les unités du monde réel, e.g., metres ou pieds, en spécifiant l'unité de mesure souhaitée comme un attribut du symboliseur. Les unités de mesure supportées sont:

* metre: http://www.opengeospatial.org/se/units/metre
* pied: http://www.opengeospatial.org/se/units/foot
* pixel: http://www.opengeospatial.org/se/units/pixel

Le style de la ligne suivante a une largeur de 40 metres:

.. code-block:: xml


          <LineSymbolizer uom="http://www.opengeospatial.org/se/units/metre">
            <Stroke>
              <CssParameter name="stroke">#000033</CssParameter>
              <CssParameter name="stroke-width">40</CssParameter>
            </Stroke>
          </LineSymbolizer>

Mise en place d'un style "uom based" en GeoServer
-------------------------------------------------

#. Créer un nouveau style nommé ``line40m`` en utilisant le SLD suivant:

    .. code-block:: xml

	<?xml version="1.0" encoding="ISO-8859-1"?>
	<StyledLayerDescriptor version="1.0.0"
	 xsi:schemaLocation="http://www.opengis.net/sld StyledLayerDescriptor.xsd"
	 xmlns="http://www.opengis.net/sld"
	 xmlns:ogc="http://www.opengis.net/ogc"
	 xmlns:xlink="http://www.w3.org/1999/xlink"
	 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	  <NamedLayer>
		<Name>line40m</Name>
		<UserStyle>
		  <Title>40 meter wide line</Title>
		  <FeatureTypeStyle>
			<Rule>
			  <LineSymbolizer uom="http://www.opengeospatial.org/se/units/metre">
				<Stroke>
				  <CssParameter name="stroke">#000033</CssParameter>
				  <CssParameter name="stroke-width">40</CssParameter>
				</Stroke>
			  </LineSymbolizer>
			</Rule>
		  </FeatureTypeStyle>
		</UserStyle>
	  </NamedLayer>
	</StyledLayerDescriptor>

#. Associez le ``line40m`` à ``MainRd`` comme style secondaire:

   .. figure:: img/secondary-line-uom.png

      Ajouter le style line40m comme style secondaire pour Mainrd

#. Visualisez l'avant-première de la couche ``MainRd`` and commutez le style en ``line40m``:

   .. figure:: img/uom-zoom1.png
     
      Une ligne basée uom, zoomée en dehors

#. Faites un Zoom en avant et en arrière et observez comment la largeur de la ligne dans lécran varie en changeant le niveau du zoom

  .. figure:: img/uom-zoom2.png
   
     Zoom en avant sur la meme ligne