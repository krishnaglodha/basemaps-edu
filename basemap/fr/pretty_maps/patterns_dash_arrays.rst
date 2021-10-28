.. _geoserver.patterns_dash_arrays:


Patterns et Hatches
--------------------

#. A partir de la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez jusq'à :menuselection:`Styles`.

   .. note:: Vous devez être connecté comme Administrator pour activer cette function.

#. Selectionnez "cemetery_graphics" de la liste

   .. figure:: img/sld_create1.jpg
      :width: 600
         
      Patterns filling SLD

#. Dans le :guilabel:`SLD Editor` vous allez voire la suivante XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor
	xmlns="http://www.opengis.net/sld"
	xmlns:sld="http://www.opengis.net/sld"
	xmlns:ogc="http://www.opengis.net/ogc"
	xmlns:gml="http://www.opengis.net/gml"
	xmlns:xlink="http://www.w3.org/1999/xlink" version="1.0.0">
	  <sld:UserLayer>
		<sld:UserStyle>
		  <sld:Name>tl 2010 08013 arealm</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:Name>cemeteries</sld:Name>
			  <ogc:Filter>
				<ogc:PropertyIsEqualTo>
				  <ogc:PropertyName>MTFCC</ogc:PropertyName>
				  <ogc:Literal>K2582</ogc:Literal>
				</ogc:PropertyIsEqualTo>
			  </ogc:Filter>
			  <sld:MaxScaleDenominator>500000.0</sld:MaxScaleDenominator>
			  <sld:PolygonSymbolizer>
				<sld:Fill>
				  <sld:GraphicFill>
					<sld:Graphic>
					  <sld:ExternalGraphic>
						<sld:OnlineResource
						xlink:type="simple"
						xlink:href="./img/landmarks/area/grave_yard.png" />
						<sld:Format>image/png</sld:Format>
					  </sld:ExternalGraphic>
					</sld:Graphic>
				  </sld:GraphicFill>
				</sld:Fill>
			  </sld:PolygonSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>



   .. figure:: img/sld_create2.jpg
      :width: 600
 		  
      Patterns Filling

   .. note:: Le SLD en dessus definit un ``<PolygonSymbolizer>`` avec un ``<GraphicFill>`` pointant à un png *./img/landmarks/area/grave_yard.png* dans le  répertoire de données Geoserver, qui serà utilisé par GeoServer comme pattern pour remplir le polygon.

#. Comme en avant, maintenant séléctionnez "cemetery_mark" de la liste

   .. figure:: img/sld_create1b.jpg
      :width: 600
         
      True Type Font filling SLD

#. Dans le :guilabel:`SLD Editor` vous allez voire le suivant XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor
	xmlns="http://www.opengis.net/sld"
	xmlns:sld="http://www.opengis.net/sld"
	xmlns:ogc="http://www.opengis.net/ogc"
	xmlns:gml="http://www.opengis.net/gml"
	xmlns:xlink="http://www.w3.org/1999/xlink" version="1.0.0">
	  <sld:UserLayer>
		<sld:Name>cemeteries</sld:Name>
		<sld:UserStyle>
		  <sld:Name>tl 2010 08013 arealm</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:Name>cemeteries</sld:Name>
			  <ogc:Filter>
				<ogc:PropertyIsEqualTo>
				  <ogc:PropertyName>MTFCC</ogc:PropertyName>
				  <ogc:Literal>K2582</ogc:Literal>
				</ogc:PropertyIsEqualTo>
			  </ogc:Filter>
			  <sld:MaxScaleDenominator>500000.0</sld:MaxScaleDenominator>
			  <sld:PolygonSymbolizer>
				<sld:Fill>
				  <sld:CssParameter name="fill">#D3FFD3</sld:CssParameter>
				  <sld:CssParameter name="fill-opacity">0.5</sld:CssParameter>              
				</sld:Fill>
				<sld:Stroke>
				  <sld:CssParameter name="stroke">#6DB26D</sld:CssParameter>
				</sld:Stroke>
			  </sld:PolygonSymbolizer>
			  <sld:PolygonSymbolizer>
				<sld:Fill>
				  <sld:GraphicFill>
					<sld:Graphic>
					  <sld:Mark>
						<sld:WellKnownName>ttf://Wingdings#0x0055</sld:WellKnownName>
						<sld:Stroke>
						<sld:CssParameter name="stroke">#6DB26D</sld:CssParameter>
						</sld:Stroke>
					  </sld:Mark>
					  <sld:Size>16</sld:Size>
					</sld:Graphic>
				  </sld:GraphicFill>
				</sld:Fill>
			  </sld:PolygonSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>



   .. figure:: img/sld_create2b.jpg
      :width: 600
 		  
      Filling avec TTF fonts

   .. note:: Le SLD en dessus définit un ``<PolygonSymbolizer>`` avec un ``<GraphicFill>`` à la recherche d'un particulier charactère *Windings* qui serà utilisé par GeoServer comme pattern pour remplir le polygon.

#. Pour voire comme les styles marchent, ajoutez le `cemetery_mark` comme un autre style de la couche :guilabel:`bplandmarks`, et puis allez à l'avant-première comme vous avez fait dans la séction précédente:
   
   .. figure:: img/sld_create4.jpg
      :width: 650

      *bplandmarks* layer avec the :guilabel:`cemetery_graphics` et :guilabel:`cemetery_mark` appliqué

#. Maintenant on va jeter un coup d'oeil à une autre façon de remplir les polygones en utilisant les patterns, the *Hatches*. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Styles` et séléctionnez "wetlands" de la liste.

   .. note:: Vous pouvez basculer sur la deuxième page pour trouver le style.

   .. figure:: img/sld_create5.jpg
      :width: 600
         
      Wetlands style

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
	  <sld:UserLayer>
		<sld:LayerFeatureConstraints>
		  <sld:FeatureTypeConstraint/>
		</sld:LayerFeatureConstraints>
		<sld:UserStyle>
		  <sld:Name>Wetlands regulatory area</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:Name>default rule</sld:Name>
			  <sld:MaxScaleDenominator>10000.0</sld:MaxScaleDenominator>
			  <sld:PolygonSymbolizer>
				<sld:Fill>
				  <sld:GraphicFill>
					<sld:Graphic>
					  <sld:Mark>
						<sld:WellKnownName>shape://times</sld:WellKnownName>
						<sld:Fill/>
						<sld:Stroke>
						  <sld:CssParameter name="stroke">#ADD8E6</sld:CssParameter>
						  <sld:CssParameter name="stroke-width">1.0</sld:CssParameter>
						</sld:Stroke>
					  </sld:Mark>
					  <sld:Size>
						<ogc:Literal>8.0</ogc:Literal>
					  </sld:Size>
					</sld:Graphic>
				  </sld:GraphicFill>
				  <!--
				  <sld:CssParameter name="fill">#7CE3F8</sld:CssParameter>
				  <sld:CssParameter name="fill-opacity">0.5</sld:CssParameter>
				  -->
				</sld:Fill>
			  </sld:PolygonSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>


#. Commentez la ligne suivante pour voire les polygones à des niveaux de zoom plus faibles aussi:

   .. code-block:: xml

	<!-- sld:MaxScaleDenominator>10000.0</sld:MaxScaleDenominator -->

#. cliquez :guilabel:`Submit` pour ajouter le nouveau SLD.

#. Pour voire comment les styles marchent, assurez vous que le style de défaut du :guilabel:`Wetlands_regulatory_area` feature type est disposé sur :guilabel:`wetlands`.

   .. figure:: img/sld_create6.jpg
      :width: 600
 		  
      Changer le default style du :guilabel:`Wetlands_regulatory_area` feature type à *wetlands*

#. Utilisez le `Map Preview <http://localhost:8083/geoserver/web/?wicket:bookmarkablePage=:org.geoserver.web.demo.MapPreviewPage>`_ pour voire l'avant-première du style.
   
   .. figure:: img/sld_create7.jpg

      :guilabel:`bplandmarks` layer avec le hatches appliqué

#. Dans l'exemple précédent nous avons utilisé *times* comme hatches mark. GeoServer rend disponibles des différents types de hatches marks:

   .. figure:: img/sld_create7a.jpg
      :width: 600
 		  
      Différents types de hatches marques.

#. Restorez le style de défaut  du :guilabel:`bplandmarks` feature type en :guilabel:`arealandmarks`.

   .. figure:: img/sld_create7b.jpg
      :width: 600
 		  
      Changer le default style du :guilabel:`bplandmarks` feature type à *arealandmarks*

Dashes
^^^^^^

#. Essayons maintenant de nous familiariser un peu avec *Dashes*. Nous verrons comment il est possible de dessinner plusieurs types de dashes pour représenter des différents types de sentiers ou de routes. 

#. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Styles`.

   .. note:: Vous devez être connecté comme Administrator pour activer cette function.

#. Selectionnez "trails" de la liste

   .. figure:: img/sld_create8.jpg
      :width: 600
         
      Dashes SLD

#. Dans le :guilabel:`SLD Editor` vous allez voire le suivant XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
	  <sld:UserLayer>
		<sld:LayerFeatureConstraints>
		  <sld:FeatureTypeConstraint/>
		</sld:LayerFeatureConstraints>
		<sld:UserStyle>
		  <sld:Name>Trails</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:MaxScaleDenominator>75000</sld:MaxScaleDenominator>
			  <sld:LineSymbolizer>
				<sld:Stroke>
				  <sld:CssParameter name="stroke">#6B4900</sld:CssParameter>
				  <sld:CssParameter name="stroke-width">0.1</sld:CssParameter>
				  <sld:CssParameter name="stroke-dasharray">2.0 </sld:CssParameter>
				</sld:Stroke>
			  </sld:LineSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>



   .. figure:: img/sld_create8a.jpg
      :width: 600
 		  
      Simple dash-array

   .. note:: La SLD en dessus définit un ``<LineSymbolizer>`` avec un ``<Stroke>`` utilisant la proprieté CSS *stroke-dasharray* pour représenter les sentiers comme un symple trait gris.
   
   .. note:: Organise un motif en traits comme une série de numéros séparés par des espaces. Nombres impairs (1er, 3ème, etc) determinent la longueur en pixels de la ligne, tandis-que les les nombres pairs (2nd, 4ème, etc) determinent la longueur en pixels des espaces blancs. Par défaut nous avons une ligne sens interruptions. à partir de la version 2.1 dash arrays peuvent etre combinés avec strokes pour générer des styles de lignes complexes avec symboles alternés ou un mélange de lignes et de symboles.

#. Le Style en dessus est celui de défaut pour la couche :guilabel:`geosolutions:Trails`. Jetons un coup d'oeil à un example un peu plus complexe. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Styles` et selectionnez "trails2" de la liste

   .. figure:: img/sld_create8b.jpg
      :width: 600
         
      Trails2 Style

#. Dans le :guilabel:`SLD Editor` Vous allez voire le suivant XML:

   .. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
	<sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
	  <sld:UserLayer>
		<sld:LayerFeatureConstraints>
		  <sld:FeatureTypeConstraint/>
		</sld:LayerFeatureConstraints>
		<sld:UserStyle>
		  <sld:Name>Trails</sld:Name>
		  <sld:Title/>
		  <sld:FeatureTypeStyle>
			<sld:Rule>
			  <sld:MaxScaleDenominator>75000</sld:MaxScaleDenominator>
			  <sld:LineSymbolizer>
				<sld:Stroke>
				  <sld:GraphicStroke>
					<sld:Graphic>
					  <sld:Mark>
						<sld:WellKnownName>circle</sld:WellKnownName>
						<sld:Fill>
						  <sld:CssParameter name="fill">#AA0000</sld:CssParameter>
						</sld:Fill>
					  </sld:Mark>
					  <sld:Size>
						<ogc:Literal>6</ogc:Literal>
					  </sld:Size>
					</sld:Graphic>
				  </sld:GraphicStroke>
				  <sld:CssParameter name="stroke-dasharray">6 18</sld:CssParameter>
				</sld:Stroke>
			  </sld:LineSymbolizer>
			  <sld:LineSymbolizer>
				<sld:Stroke>
				  <sld:CssParameter name="stroke">#AA0000</sld:CssParameter>
				  <sld:CssParameter name="stroke-dasharray">10 14</sld:CssParameter>
				  <sld:CssParameter name="stroke-dashoffset">14</sld:CssParameter>
				</sld:Stroke>
			  </sld:LineSymbolizer>
			</sld:Rule>
		  </sld:FeatureTypeStyle>
		</sld:UserStyle>
	  </sld:UserLayer>
	</sld:StyledLayerDescriptor>



   .. figure:: img/sld_create8c.jpg
      :width: 600
 		  
      Railroad dash-array

   .. note:: Nous pouvons remarquer deux choses interessantes dans ce style, deux ``<LineSymbolizer>`` le premier définit un *circle* Mark avec un symple dasharray et le deuxième un simple stroke définant *dashoffset* aussi. Le dernier specifie la distance en pixels dans le dasharray pattern duquel commencer à déssinner. Par défault il est 0.

#. Ouvrez la couche :guilabel:`geosolutions:Trails` et ajoutez *trails2* comme un style supplémentaire, et puis allez à :guilabel:`Layer Preview` pour le voir en action

   .. figure:: img/sld_create8e.jpg
      :width: 500

   .. Attention:: Il faut zoomer in de l'avant-première pour pouvoir voir les lignes à cause du *MaxScaleDenominator*
