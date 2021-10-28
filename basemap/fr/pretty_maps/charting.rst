.. _geoserver.dynamic_symb:

Cartographie    
------------


Cette séction montre les caractéristiques avancées de style de Geoserver.

   .. Attention:: Si vous utilisez l'installation de Windows, avant d'installer le 'geoserver-2.2-SNAPSHOT-charts-plugin' du 
                :file:`data\\plugins\\`. Décompressez le fichier zip dans 
		:file:`%CATALINA_BASE%\\webapps\\geoserver\\WEB-INF\\lib\\` et redémarrez GeoServer.

#.  Pour imprimer les graphiques dynamiques sur la carte ajoutez un nouveau style nommé :guilabel:`statespiepss`.

   .. figure:: img/dyn_symb1.jpg
      :width: 600
 		  
      Créer un nouveau Style Dynamique

#. Dans le :guilabel:`SLD Editor` tapez la XML suivante:

  .. code-block:: xml
   
    <?xml version="1.0" encoding="ISO-8859-1"?>
    <StyledLayerDescriptor version="1.0.0"
      xsi:schemaLocation="http://www.opengis.net/sld http://schemas.opengis.net/sld/1.0.0/StyledLayerDescriptor.xsd"
      xmlns="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc"
      xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <NamedLayer>
	<Name></Name>
	<UserStyle>
	  <Name>Pie charts</Name>
	  <FeatureTypeStyle>
	    <Rule>
	      <PolygonSymbolizer>
		<Fill>
		  <CssParameter name="fill">#AAAAAA</CssParameter>
		</Fill>
		<Stroke />
	      </PolygonSymbolizer>
	    </Rule>
	  </FeatureTypeStyle>
	  <FeatureTypeStyle>
	    <Rule>
	      <PointSymbolizer>
		<Graphic>
		  <ExternalGraphic>
		    <OnlineResource
		      xlink:href="http://chart?cht=p&amp;chd=t:${100 * MALE / PERSONS},${100 * FEMALE / PERSONS}&amp;chf=bg,s,FFFFFF00" />
		    <Format>application/chart</Format>
		  </ExternalGraphic>
		  <Size>
		    <ogc:Add>
		      <ogc:Literal>20</ogc:Literal>
		      <ogc:Mul>
			<ogc:Div>
			  <ogc:PropertyName>PERSONS</ogc:PropertyName>
			  <ogc:Literal>20000000.0</ogc:Literal>
			</ogc:Div>
			<ogc:Literal>60</ogc:Literal>
		      </ogc:Mul>
		    </ogc:Add>
		  </Size>
		</Graphic>
	      </PointSymbolizer>
	    </Rule>
	  </FeatureTypeStyle>
	</UserStyle>
      </NamedLayer>
    </StyledLayerDescriptor>

   .. note:: L'élement ``<ExternalGraphic>``. Nous avons une expression qui utilise le type de fonction attribuées pour déssinner dynamiquement le graphique à tarte. Le URL suit la syntaxe de Google Chart API , mais le graphique est généré à l'intérieur de GeoServer.

#. Modifier le style de défaut des couches :guilabel:`states` au nouvellement créé :guilabel:`statespiepss`.

   .. figure:: img/dyn_symb2.jpg
      :width: 800
 		  
      Changer le style de défaut des couches d'état

#. Utiliser le **Layer Preview** pour voire l'avant-première du nouveau style.
   
   .. figure:: img/dyn_symb3.jpg

      Avant-première des couches d'état avec le statespiepss appliqué


#. Finalement restorez le style précédent `states`.