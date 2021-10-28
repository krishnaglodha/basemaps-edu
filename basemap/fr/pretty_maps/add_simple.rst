.. _geoserver.add_simple:

Examiner un style existant
--------------------------

#. De la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_ naviguez jusq'à :guilabel:`Style`.

   .. figure:: img/style1.png
      :width: 600
   
      Naviguez jusq'à configuration du Style

#. De la liste des styles selectionnez le `citylimits` style

   .. figure:: img/styling_vector1.png

     La liste des styles

#. A l'intérieur du *Style Editor* nous trouvons le style suivant:

   .. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" 
		xmlns:sld="http://www.opengis.net/sld" 
		xmlns:ogc="http://www.opengis.net/ogc" 
		xmlns:gml="http://www.opengis.net/gml" 
		version="1.0.0">
	  <sld:UserLayer>
		<sld:LayerFeatureConstraints>
		  <sld:FeatureTypeConstraint/>
		</sld:LayerFeatureConstraints>
		<sld:UserStyle>
		  <sld:Name>BoulderCityLimits</sld:Name>
		  <sld:Title/>
		  <sld:IsDefault>1</sld:IsDefault>
		  <sld:FeatureTypeStyle>
			<sld:Name>group 0</sld:Name>
			<sld:FeatureTypeName>Feature</sld:FeatureTypeName>
			<sld:SemanticTypeIdentifier>generic:geometry</sld:SemanticTypeIdentifier>
			<sld:SemanticTypeIdentifier>simple</sld:SemanticTypeIdentifier>
			<sld:Rule>
			  <sld:Name>Filled</sld:Name>
			  <sld:MinScaleDenominator>75000</sld:MinScaleDenominator>
			  <sld:PolygonSymbolizer>
				<sld:Fill>
				  <sld:CssParameter name="fill">#7F7F7F</sld:CssParameter>
				  <sld:CssParameter name="fill-opacity">0.5</sld:CssParameter>
				</sld:Fill>
				<sld:Stroke>
				  <sld:CssParameter name="stroke">#7F7F7F</sld:CssParameter>
				  <sld:CssParameter name="stroke-opacity">0.5</sld:CssParameter>
				  <sld:CssParameter name="stroke-width">2.0</sld:CssParameter>
				</sld:Stroke>
			  </sld:PolygonSymbolizer>
			  <sld:TextSymbolizer>
				<sld:Label>
				  <ogc:Literal>Boulder</ogc:Literal>
				</sld:Label>
				<sld:Font>
				  <sld:CssParameter name="font-family">Arial</sld:CssParameter>
				  <sld:CssParameter name="font-size">14.0</sld:CssParameter>
				  <sld:CssParameter name="font-style">normal</sld:CssParameter>
				  <sld:CssParameter name="font-weight">normal</sld:CssParameter>
				</sld:Font>
				<sld:LabelPlacement>
				  <sld:PointPlacement>
					<sld:AnchorPoint>
					  <sld:AnchorPointX>
						<ogc:Literal>0.0</ogc:Literal>
					  </sld:AnchorPointX>
					  <sld:AnchorPointY>
						<ogc:Literal>0.5</ogc:Literal>
					  </sld:AnchorPointY>
					</sld:AnchorPoint>
					<sld:Rotation>
					  <ogc:Literal>0</ogc:Literal>
					</sld:Rotation>
				  </sld:PointPlacement>
				</sld:LabelPlacement>
				<sld:Fill>
				  <sld:CssParameter name="fill">#000000</sld:CssParameter>
				</sld:Fill>
				<sld:VendorOption name="maxDisplacement">200</sld:VendorOption>
				<sld:VendorOption name="Group">true</sld:VendorOption>
			  </sld:TextSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>
	
   .. note:: Les séctions les plus importants sont:

	  - Le ``<Rule>`` tag combine un numéro de symboles (nous avons aussi la possibilité de définir le OGC filtre) pour définir le portrait d'une fonctionnalité.
	  - Le ``<PolygonSymbolizer>`` polygons styles contiennent les informations sur leur bordure (stroke) and leur remplissage.
	  - Le ``<TextSymbolizer >`` specifie le texte des etiquettes et leur style:
	  
			* ``<Label>`` Specifie le contenu du texte de l'etiquette
			* ``<Font>`` Specifie les infos sur le caractère des etiquettes.
			* ``<LabelPlacement>`` Dispose la position de l'etiquette rélativement à sa fonction associée.
			* ``<Fill>`` Determine la couleur du remplissement de l'etiquette de texte.
			* VendorOption ``maxDisplacement`` Controle le déplacement de létiquette selon une ligne. Normalement GeoServer étiqueterait un polygone dans son centre, à moins que cette place ne soit déjà occupée par une autre étiquette et que l'étiquette ne soit trop grande comparée au polygone; outrement il ne l'étiquetterait pas du tout. Quand le maxDisplacement est fixé, l'étiquetteur chercherà une place différente dans les pixels du maxDisplacement du point précalculé pour l'étiquette.
			* VendorOption ``Group`` Quelques fois vous aurez un ensemble de caractéristiques pour lesquelles vous voules une seule étiquette. L'option de regroupement classifie toutes les caractéristiques avec le meme texte dans l'étiquette, et trouve une géométrie representative pour le groupe.
			
	  - Le ``<MaxScaleDenominator>`` et ``<MinScaleDenominator>`` sont utilisés pour appliquer une particulière règle SLD à une échelle spécifique. Le SLD en dessus s'assure que les bords des rochers s'égarent quand on fait un zoom pour voir led détails d'une ville. 
	   Une façon alternative de s'approcher pourrait etre de tenir la couche en montre, mais en lui changeant de style, par ex une fine ligne rouge, de façon que les détails de la ville ne soyent pas dérangés par le remplissage du polygone.

#. Maintenant choisissez de la liste des styles `rivers`.

#. A l'intérieur du *Style Editor* on peut trouver le style suivant:

   .. code-block:: xml
   
	<?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" 
		xmlns:sld="http://www.opengis.net/sld" 
		xmlns:ogc="http://www.opengis.net/ogc"
		xmlns:gml="http://www.opengis.net/gml" 
		version="1.0.0">
	  <sld:UserLayer>
		<sld:LayerFeatureConstraints>
		  <sld:FeatureTypeConstraint/>
		</sld:LayerFeatureConstraints>
		<sld:UserStyle>
		  <sld:Name>Hydrology Line</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:Name>default rule</sld:Name>
			  <sld:MaxScaleDenominator>75000</sld:MaxScaleDenominator>
			  <sld:LineSymbolizer>
				<sld:Stroke>
				  <sld:CssParameter name="stroke-width">0.5</sld:CssParameter>
				  <sld:CssParameter name="stroke">#06607F</sld:CssParameter>
				</sld:Stroke>
			  </sld:LineSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>

   .. note:: 
		Ce ci est une ligne de style très simple. Prenez en considération le LineSymbolizer pour le style des lignes. Les lignes sont des éléments géometriques à une dimension qui contiennent une position et une longueur.
		Le slignes peuvent comprendre multiples segments de ligne.
		
		La marque la plus reculèe est le <Stroke>. Cette marque est nécessaire et détermine la visualisation de la ligne:

		* ``stroke`` Specifie la couleur de la ligne, dans la forme #RRGGBB.  Par défault elle est noire (``#000000``).
		* ``stroke-width`` Specifie la largeur de la ligne en pixels.  Par défault elle est ``1``.

    Dans ce cas là ``MaxScaleDenominator`` est utilisé pour s'assurer que les fleuves soient visibles quand on se rapproche en particulier quand les bords de la ville disparassent.

Créer un style simple pour les points
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. De la page de GeoServer `Welcome Page <http://localhost:8083/geoserver>`_ naviguez jusq'à :guilabel:`Style`.

   .. figure:: img/style1.png

     Naviguez jusq'à configuration Style
     
#. Cliquez :guilabel:`New`

   .. figure:: img/style2.png

     Ajouter un nouveau style

#. Tapez "landmarks" dans le domain :guilabel:`Name`.

   .. figure:: img/styling_vector2.png
         
      Créez un nouveau style
	  
#. Dans le :guilabel:`SLD Editor` tapez le suivant XML:

   .. code-block:: xml

	<StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="1.0.0" xsi:schemaLocation="http://www.opengis.net/sld http://schemas.opengis.net/sld/1.0.0/StyledLayerDescriptor.xsd">
	  <NamedLayer>
		<Name>landmarks</Name>
		<UserStyle>
		  <Name>landmarks</Name>
		  <Title>Point Landmarks</Title>
		  <FeatureTypeStyle>
			<Rule>
			  <Name>default</Name>
			  <Title>Landmarks</Title>
			   <PointSymbolizer>
				 <Graphic>
				   <Mark>
					 <WellKnownName>triangle</WellKnownName>
					 <Fill>
					   <CssParameter name="fill">#009900</CssParameter>
					   <CssParameter name="fill-opacity">0.2</CssParameter>
					 </Fill>
					 <Stroke>
					   <CssParameter name="stroke">#000000</CssParameter>
					   <CssParameter name="stroke-width">2</CssParameter>
					 </Stroke>
				   </Mark>
				   <Size>12</Size>
				 </Graphic>
			   </PointSymbolizer>
			</Rule>
		  </FeatureTypeStyle>
		</UserStyle>
	  </NamedLayer>
	</StyledLayerDescriptor>

   .. note:: 
   
		Prenez en considération:

    * ``WellKnownName`` Le nom des formes les plus communes. Les options sont circle, carré, triangle, étoile, croix, ou x. Par défaut on a le carré.
		* ``fill`` Specifie comment remplir le simboliseur.  Les options sont un ``<CssParameter name="fill">`` specifiant une couleure en forme de ``#RRGGBB``, ou ``<GraphicFill>`` pour un remplissement fait d'une graphique repetée.
		* ``fill-opacity`` Determine l'opacité (transparence) des simboliseurs.  La plage de valeurs de ``0`` (completement transparent) jusq'à ``1`` (completement opaque).  Par défaut on a  ``1``.	

#. Puis cliquez :guilabel:`Save` bouton.

#. Ouvrez la couche vectorielle (vector layer) ``geosolutions:bptlandmarks`` , mais cette fois associez le style comme "Additional Style":

   .. figure:: img/styling_vector_add_style.png
         
      Ouvrez l'avant-première des couches

#. Pour prévisualiser la couche ``geosolutions:bptlandmarks``, qui devrait etre vide à cause des scale dependencies.
   En fin cliquez le bouton des options en haut à la gauche de la carte et séléctionnez le style ``landmarks`` dans le menu déroulant:

   .. figure:: img/styling_vector4.png
         
      Ouvrez la prévisualisation des couches
