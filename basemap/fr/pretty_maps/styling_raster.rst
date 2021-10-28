  .. _geoserver.styling_raster:


Styling Raster data
-------------------

Dans la section précédente nous avons créé et optimisé quelques styles des vecteurs. dans cette séction nous traiterons un styled SRTM raster et nour verrons comment obténir une meilleure visualisation de cette couche en ajoutant de l'ombrage.

#. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Layer Preview` et selectionnez le lien OpenLayers  pour la couche ``geosolutions:srtm``.

   .. figure:: img/raster_srtm.png

      SRTM rendering with DEM style

   Il ya un DEM style associé à cette couche de données SRTM, résultant en un tel rendu de couleurs.

#. Returnez à la GeoServer `Welcome Page`, selectionnez le :menuselection:`Styles` et cliquez le style ``dem`` pour voire quelle couleure est appliqué à la carte.

   .. note:: Vous devez être connecté comme Administrator pour pour modifier/maitriser les styles.

   .. figure:: img/raster_dem_style.png

       Rédaction du Style

   Remarquez les entrées avec ``opacity = 0.0`` qui ne permettent d'avoire aucune valeur de données transparente.

Le style courant DEM  nous permet d'obtenir un rendu agréable du SRTM dataset mais on peut avoire des meilleures résultats en le combinant avec une couche d'ombrage créé à travers une autre GDAL utilité (gdaldem).

Ajouter l'Ombrage
^^^^^^^^^^^^^^^^^^

#. Ouvrez une shell (CTRL+ALT+T) et déclenchez::

     * Linux
     
     gdaldem hillshade -z 5 -s 111120 ${TRAINING_ROOT}/geoserver_data/data/boulder/srtm_boulder.tiff ${TRAINING_ROOT}/geoserver_data/data/boulder/srtm_boulder_hs.tiff -co tiled=yes
     
     * windows

     gdaldem hillshade -z 5 -s 111120 %TRAINING_ROOT%\\geoserver_data\\data\\boulder\\srtm_boulder.tiff %TRAINING_ROOT%\\geoserver_data\\data\\boulder\\srtm_boulder_hs.tiff -co tiled=yes

   .. note:: Le parametre ``z``  exagère l'élévation, le parametre ``s``  fournit le rapport entre les unités d'élévation  et les unités terrestres (degrés en ce cas), ``-co tiled=yes`` qui fait générer gdaldem  un TIFF avec un carrelege à l'intérieur. Nous étudierons cette dernière option mieux dans les pages suivantes.

#. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Styles` et selectionnez `Add a new style` comme on a précédemment vu dans la séction :ref:`Adding a style <geoserver.add_style>`.

#. Dans le :guilabel:`SLD Editor` introduisez le suivant XML:

   .. code-block:: xml
   
    <?xml version="1.0" encoding="UTF-8"?>
    <sld:StyledLayerDescriptor xmlns="http://www.opengis.net/sld" xmlns:sld="http://www.opengis.net/sld" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" version="1.0.0">
        <sld:UserLayer>
            <sld:LayerFeatureConstraints>
                <sld:FeatureTypeConstraint/>
            </sld:LayerFeatureConstraints>
            <sld:UserStyle>
                <sld:Title/>
                <sld:FeatureTypeStyle>
                    <sld:Name>name</sld:Name>
                    <sld:FeatureTypeName>Feature</sld:FeatureTypeName>
                    <sld:Rule>
                        <sld:MinScaleDenominator>75000</sld:MinScaleDenominator>
                        <sld:RasterSymbolizer>
                            <sld:Geometry>
                                <ogc:PropertyName>grid</ogc:PropertyName>
                            </sld:Geometry>
                            <sld:ColorMap>
                                <sld:ColorMapEntry color="#000000" opacity="0.0" quantity="0.0"/>
                                <sld:ColorMapEntry color="#999999" opacity="0.7" quantity="1.0"/>
                                <sld:ColorMapEntry color="#FFFFFF" opacity="0.7" quantity="256.0"/>
                            </sld:ColorMap>
                        </sld:RasterSymbolizer>
                    </sld:Rule>
                </sld:FeatureTypeStyle>
            </sld:UserStyle>
        </sld:UserLayer>
    </sld:StyledLayerDescriptor>

   .. note:: Les valeurs de l'opacité inférieur à 1, pour les rendre partiellement transparents  afin de pouvoir les superposer sur les autres couches

#. Fixer :file:`hillshade` comme nom et puis cliquez le bouton :guilabel:`Submit`.

#. Selectionner :guilabel:`Add stores` de la GeoServer `Welcome Page` pour ajouter le raster ``hillshade`` précédemment créé.

#. Selectionner :guilabel:`GeoTIFF - Tagged Image File Format with Geographic information` du set des Raster Data Sources disponibles. 

#. Specifiez :file:`hillshade` comme nom dans le camp de l'interface :guilabel:`Data Source Name`.

#. cliquez sur le lien  :guilabel:`browse` pour fixer la location GeoTIFF  dans le camp :guilabel:`URL`.

   .. note:: Assurez vous de spécifier le :file:`srtm_boulder_hs.tiff` précédemment crée avec gdaldem, qui doit être situé en :file:`${TRAINING_ROOT}/geoserver_data/data/boulder`

#. cliquez :guilabel:`Save`. 

#. Publier la couche en cliquant sur le lien :guilabel:`publish`. 

   .. figure:: img/raster_hillshade.png
         
      Publier Raster couche

#. Fixez comme title :file:`SRTM Hillshade`

#. Transférez-vous à l'onglet `Publishing`

   .. figure:: img/raster_hillshade_publishing.png

#. Assurez vous de fixer le  style de défaut pour ``hillshade`` dans la séction `Publishing --> Default Style`.

   .. figure:: img/raster_hillshade_defaultstyle.png
         
      Rédaction des info de Raster Publishing

#. cliquez :guilabel:`Save` pour créer une nouvelle couche.

#. Utilisez le **Layer Preview** pour voire l'avant-première de la nouvelle couche avec le style ombrage.
   
   .. figure:: img/raster_hillshade_preview.png

      Visualiser la nouvelle couche raster layer avec le style ombrage appliqué

#. Modifier la Layer Preview URL dans votre navigateur en localisant les parametres `layers` 

    .. figure:: img/raster_overlay_url.png

#. Inserez le `geosolutions:srtm,` couche supplémentaire (remarquez la virgule finale) avant celle de `geosolutions:hillshade`

    .. figure:: img/raster_overlay_2layers.png

#. Appuyez sur Enter pour envoyer la demande mise à jour. L'avant-première des couches devrait changer comme ça et on devrait voire soit le srtm soit la couche ombrage.

    .. figure:: img/raster_overlay.png

       L'avant-première de la couche avec srtm et de l'ombrage est superposée.

