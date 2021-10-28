.. module:: geoserver.processing

.. _geoserver.processing:

Traitement avec GDAL
--------------------

Dans la section :ref:`Adding a GeoTiff <geoserver.add_geotiff>`, un fichier GeoTIFF a été ajouté à geoserver telles quelles. Cependant, il est de pratique courante de faire une analyse de preliminar sur les données disponibles et, si nécessaire, d'optimiser puisque la configuration de grands ensembles de données sans pré-traitement approprié, peut entraîner une faible performance lors de leur accès.
Dans cette section, des instructions sur la façon de faire l'optimisation des données seront fournies par l'introduction de certains FWTools Utilities.

gdalinfo
````````
Ce utility permet d'obtenir plusieurs informations à partir de la bibliothèque GDAL, par exemple, specifique Driver capabilities et datasets en entrée / fichiers de propriétés.  

*gdalinfo - obtention de Drivers Capabilities*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 En etant GeoTIFF un format géospatiale largement adoptée, il est utile d'obtenir des informations sur les capacités de GDAL GeoTIFF's Driver capabilities en utilisant la commande::

     gdalinfo --format GTIFF
     
Il ne s'agit que d'une version revu à la baisse d'une sortie typique::

     Format Details:
       Short Name: GTiff
       Long Name: GeoTIFF
       Extension: tif
       Mime Type: image/tiff
       Help Topic: frmt_gtiff.html
       Supports: Create() - Create writeable dataset.
       Supports: CreateCopy() - Create dataset by copying another.
       Supports: Virtual IO - eg. /vsimem/
       Creation Datatypes: Byte UInt16 Int16 UInt32 Int32 Float32 Float64 CInt16 CInt32 CFloat32 CFloat64
       <CreationOptionList>
       <Option name="COMPRESS" type="string-select">
              <Value>NONE</Value>
              <Value>LZW</Value>
              <Value>PACKBITS</Value>
              <Value>JPEG</Value>
              <Value>CCITTRLE</Value>
              <Value>CCITTFAX3</Value>
              <Value>CCITTFAX4</Value>
              <Value>DEFLATE</Value>
       </Option>
       <Option name="PREDICTOR" type="int" description="Predictor Type" />
       <Option name="JPEG_QUALITY" type="int" description="JPEG quality 1-100" default="75"/>
       <Option name="ZLEVEL" type="int" description="DEFLATE compression level 1-9" default="6" />
       <Option name="LZMA_PRESET" type="int" description="LZMA compression level 0(fast)-9(slow)" default="6" />
       <Option name="NBITS" type="int" description="BITS for sub-byte files (1-7), sub-uint16 (9-15), sub-uint32 (17-31)" />
       <Option name="INTERLEAVE" type="string-select" default="PIXEL">
              <Value>BAND</Value>
              <Value>PIXEL</Value>
       </Option>
       <Option name="TILED" type="boolean" description="Switch to tiled format"/>
       <Option name="TFW" type="boolean" description="Write out world file"/>
       <Option name="RPB" type="boolean" description="Write out .RPB (RPC) file" />
       <Option name="BLOCKXSIZE" type="int" description="Tile Width"/>
       <Option name="BLOCKYSIZE" type="int" description="Tile/Strip Height"/>
       <Option name="PHOTOMETRIC" type="string-select">
              <Value>MINISBLACK</Value>
              <Value>MINISWHITE</Value>
              <Value>PALETTE</Value>
              <Value>RGB</Value>
              <Value>CMYK</Value>
              <Value>YCBCR</Value>
              <Value>CIELAB</Value>
              <Value>ICCLAB</Value>
              <Value>ITULAB</Value>
       </Option>
       <Option name="SPARSE_OK" type="boolean" description="Can newly created files have missing blocks?" default="FALSE" />
       <Option name="ALPHA" type="boolean" description="Mark first extrasample as being alpha" />
       <Option name="PROFILE" type="string-select" default="GDALGeoTIFF">
              <Value>GDALGeoTIFF</Value>
              <Value>GeoTIFF</Value>
              <Value>BASELINE</Value>
       </Option>
       <Option name="PIXELTYPE" type="string-select">
              <Value>DEFAULT</Value>
              <Value>SIGNEDBYTE</Value>
       </Option>
       <Option name="BIGTIFF" type="string-select" description="Force creation of BigTIFF file">
              <Value>YES</Value>
              <Value>NO</Value>
              <Value>IF_NEEDED</Value>
              <Value>IF_SAFER</Value>
       </Option>
       <Option name="ENDIANNESS" type="string-select" default="NATIVE" description="Force endianness of created file. For DEBUG purpose mostly">
              <Value>NATIVE</Value>
              <Value>INVERTED</Value>
              <Value>LITTLE</Value>
              <Value>BIG</Value>
       </Option>
       <Option name="COPY_SRC_OVERVIEWS" type="boolean" default="NO" description="Force copy of overviews of source dataset (CreateCopy())" />
       </CreationOptionList>

Dans la liste ci-dessus de créer des options, il est possible de déterminer les capacités d'écriture du GeoTIFF Driver's writing capabilities principal:
  * COMPRESS: personnaliser la compression à utiliser lors de l'écriture des données de sortie
  * JPEG_QUALITY: spécifier un facteur de qualité pour être utilisé par la compression JPEG
  * TILED: WSi vous choisissez Oui, il permet de sortir des données de tuiles
  * BLOCKXSIZE, BLOCKYZISE: Spécifiez la largeur de la dimension des carreaux de tuiles et de hauteur de dimension
  * PHOTOMETRIC: Préciser l'interprétation photométrique des données
  * PROFILE: Spécifiez le profil GeoTIFF à utiliser (certains profils ne prennent en charge un ensemble minimal de  tag TIFF tandis que d'autres offrent un large éventail de tag)
  * BIGTIFF: Spécifiez le moment d'écrire les données que BigTIFF (format TIFF qui permet de briser la frontière Offset 4 Go)



*gdalinfo - Obtenir Dataset / Propriétés du fichier*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Les instructions suivantes vous permettent d'obtenir des informations sur l'ensemble des données de l'échantillon préalablement configuré dans GeoServer.

#. Run::

    * Linux::
      
      cd ${TRAINING_ROOT}/data/user_data/aerial
      
      gdalinfo 13tde815295_200803_0x6000m_cl.tif
     
    * Windows::
     
      cd %TRAINING_ROOT%\\data\\user_data\\aerial\\
      
      gdalinfo 13tde815295_200803_0x6000m_cl.tif

   .. figure:: img/fw_basegdalinfo.png

      Part of the *gdalinfo* output on a sample dataset 

#. Vérifiez le **Block** info ainsi que le **Overviews** info si present. 
  
  * **Block**: Il représente le carrelage intérieur. Notez que les echantillons de donnez a tuiles  en 16 rangées ayant une largeur égale à la largeur de l'image complète.  
  * **Overviews**: Il fournit des informations sur les aperçus sous-jacents. Notez que les echantillons de donnez n'a pas aperçus depuis le * Aperçus * propriété est totalement absent de gdalinfo output.

gdal_translate
``````````````
Cet utilitaire permet de convertir un ensemble de données dans un format différent en permettant à un large éventail de paramètres pour personnaliser la conversion.

Exécution de la commande::

     gdal_translate

permet d'obtenir la liste des paramètres pris en charge ainsi que les formats de sortie pris en charge::

     Usage: gdal_translate [--help-general]
            [-ot {Byte/Int16/UInt16/UInt32/Int32/Float32/Float64/
                  CInt16/CInt32/CFloat32/CFloat64}] [-strict]
            [-of format] [-b band] [-mask band] [-expand {gray|rgb|rgba}]
            [-outsize xsize[%] ysize[%]]
            [- unscale] [-scale [src_min src_max [dst_min dst_max]]]
            [-srcwin xoff yoff xsize ysize] [-projwin ulx uly lrx lry]
            [-a_srs srs_def] [-a_ullr ulx uly lrx lry] [-a_nodata value]
            [-gcp pixel line easting northing [elevation]]*
            [-mo "META-TAG=VALUE"]* [-q] [-sds]
            [-co "NAME=VALUE"]* [-stats]
            src_dataset dst_dataset

Lorsque le sens des principaux paramètres est résumée ci-dessous:
  * *-ot*: permet de spécifier le type de données de sortie (Assurez-vous que le type de données spécifié est contenu dans le fichier liste de writing driver *Creation Datatypes*)
  * *-of*: spécifier le format de sortie souhaité (GTIFF est la valeur par défaut)
  * *-b*: permet de définir une bande d'entrée à écrire dans le fichier de sortie. (Utilisez multiple option *-b* pour spécifier plusieurs bandes)
  * *-mask*: permet de spécifier une bande d'entrée à écrire une bande de masque de données de sortie.
  * *-expand*: permet d'exposer un ensemble de données avec 1 bande avec une table de couleurs comme un ensemble de données 3 (RVB) ou 4 (RGBA) bandes. La valeur (gris) permet de développer un ensemble de données avec une table de couleur ne contenant que des niveaux de gris pour un ensemble de données indexée gris.
  * *-outsize*: permet de régler la taille du fichier de sortie en termes de pixels et de lignes à moins que le *% * enseigne est apposée dans ce cas, c'est comme une fraction de la taille de l'image d'entrée.
  * *-unscale*: permet d'appliquer le barème / offset métadonnées pour les bandes  pour convertir de des valeurs mises à l'échelle dans valeurs non mises à l'échelle.
  * *-scale*: permet de remettre à l'échelle les valeurs de pixels d'entrée à partir de la gamme de src_min src_max à la gamme dst_min à dst_max. (En cas d'omission la plage de sortie est de 0 à 255. En cas d'omission la plage de saisie est automatiquement calculée à partir de la source de données).
  * *-srcwin*: permet de sélectionner une sous-fenêtre à partir de l'image source sur le plan de xoffset, yoffset, largeur et hauteur
  * *-projwin*: permet de sélectionner une sous-fenêtre de l'image source, en précisant les angles donnés en coordonnées géoréférencées.
  * *-a_srs*: permet de passer outre la projection de l'image de sortie. Le srs_def peut être n'importe laquelle des GDAL / OGR formes habituelles, WKT complet, PROJ.4, EPSG: n ou un fichier contenant le WKT.
  * *-a_ullr*: permet d'assigner / outrepasser les limites géoréférencées du fichier de sortie. 
  * *-a_nodata*: permet d'affecter une valeur de nodata spécifié à des bandes de sortie.
  * *-co*: permet de définir une option de création sous la forme "NOM = VALEUR" pour le pilote de format de sortie. (Multiple option *-co* peuvent être énumérés.)
  * *-stats*: permet d'obtenir des statistiques (min, max, mean, stdDev)pour chaque bande
  * *src_dataset*: est le nom de données source. Il peut être soit le nom du fichier, l'URL de la source de données ou le nom du sous-jeu pour les fichiers multi-*-ensemble de données.
  * *dst_dataset*: est le nom de fichier de destination.
  
*gdal_translate - Tiling the sample dataset*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Les étapes suivantes fournissent des instructions pour tuile les échantillons de données  précédemment configuré dans GeoServer, en utilisant le driver GeoTiff.

#. Créez un répertoire pour stocker les données converties:

  * Linux::
  
     cd ${TRAINING_ROOT}/data/user_data
     
     mkdir retiled 
     
  * Windows::
  
     cd %TRAINING_ROOT%\\data\\user_data
     
     mkdir retiled 

#. Convertir les données d'échantillonnage d'entrée vers un fichier de sortie ayant carrelage réglé à 512x512. exécuter:

  * Linux::
  
     gdal_translate -co "TILED=YES" -co "BLOCKXSIZE=512" -co "BLOCKYSIZE=512" aerial/13tde815295_200803_0x6000m_cl.tif retiled/13tde815295_200803_0x6000m_cl.tif 
     
  * Windows::
     
     gdal_translate -co "TILED=YES" -co "BLOCKXSIZE=512" -co "BLOCKYSIZE=512" aerial\\13tde815295_200803_0x6000m_cl.tif retiled\\13tde815295_200803_0x6000m_cl.tif 

#. Optionally, check that the output dataset have been successfully tiled, by running the command:

  * Linux::
  
     gdalinfo retiled/13tde815295_200803_0x6000m_cl.tif 
     
  * Windows::
     
     gdalinfo retiled\\13tde815295_200803_0x6000m_cl.tif 
     

   .. figure:: img/fw_tiledgdalinfo.png

      Une partie de *gdalinfo* output sur l'ensemble de données en mosaïque. Notez que la valeur **Block** maintenant est 512x512

gdaladdo
````````
Ce utility permet d'ajouter des aperçus d'un ensemble de données. Les étapes suivantes fournissent des instructions pour ajouter des aperçus à l'échantillons de données en mosaïque.

Exécution de la commande::

     gdaladdo

permet d'obtenir la liste des paramètres pris en charge::

     Usage: gdaladdo [-r {nearest,average,gauss,average_mp,average_magphase,mode}]
                     [-ro] [--help-general] filename levels

Lorsque le sens des principaux paramètres est résumée ci-dessous:
  * *-r*: permet de spécifier l'algorithme de rééchantillonnage(Est le plus proche de la valeur par défaut)
  * *-ro*: permet d'ouvrir l'ensemble de données en mode lecture seule, afin de générer aperçu externe(pour GeoTIFF surtout)
  * *filename*: représente le fichier pour construire des aperçus pour.
  * *levels*: permet de spécifier une liste de niveau d'ensemble à construire.

*gdaladdo - Ajout aperçus à l'échantillon de données*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. exécuter:

  * Linux::
  
     cd ${TRAINING_ROOT}/data/user_data/retiled

     gdaladdo -r average 13tde815295_200803_0x6000m_cl.tif 2 4 8 16 32
     
  * Windows::
  
     cd %TRAINING_ROOT%\\data\\user_data\\retiled

     gdaladdo -r average 13tde815295_200803_0x6000m_cl.tif 2 4 8 16 32

  pour ajouter 5 niveaux de synthèses ayant 2,4,8,16,32 facteurs sous-échantillonnage appliqués à la résolution de l'image originale, respectivement.

#. Eventuellement, vérifiez que les synthèses ont été ajoutés à l'ensemble de données, en exécutant la commande::

     gdalinfo 13tde815295_200803_0x6000m_cl.tif

   .. figure:: img/fw_tiledovgdalinfo.png

      partie de *gdalinfo* output sur l'ensemble de données en mosaïque avec des aperçus. remarquerez les propriétés **Overviews** 

Procédé en nombre
``````````````````

Au lieu de répéter manuellement ces 2 étapes (rutile + aperçus ADD) pour chaque fichier, nous pouvons invoquer quelques commandes pour obtenir automatisé.

#. exécuter:

  * Linux::
  
     cd ${TRAINING_ROOT}/data/user_data

     mkdir optimized

     cd aerial

     for i in `find *.tif`; do gdal_translate -CO "TILED=YES" -CO "BLOCKXSIZE=512" -CO "BLOCKYSIZE=512" $i ../optimized/$i; gdaladdo -r average ../optimized/$i 2 4 8 16 32; done
     
  * Windows::
      
      cd %TRAINING_ROOT%\\data\\user_data\\
      
      mkdir optimized
      
      cd aerial
      
      for %%F in (*.tif) do  (
        echo Processing file %%F

        REM translate
        echo Performing gdal_translate on file %%F to file %%~nF.tiff
        gdal_translate -co "TILED=YES" -co "BLOCKXSIZE=512" -co "BLOCKYSIZE=512" -co "COMPRESS=DEFLATE" %%F ..\optimized\\%%~nF.tiff

        REM add overviews
        echo Adding overviews on file %%~nF.tiff
        gdaladdo -r average --config COMPRESS_OVERVIEW DEFLATE ..\\optimized\\%%~nF.tiff 2 4 8 16 32

      )      

#. You should see a list of run like thisVous devriez voir une liste de course comme ça::

     ...
     Input file size is 2500, 2500
     0...10...20...30...40...50...60...70...80...90...100 - done.
     0...10...20...30...40...50...60...70...80...90...100 - done.
     Input file size is 2500, 2500
     0...10...20...30...40...50...60...70...80...90...100 - done.
     0...10...20...30...40...50...60...70...80...90...100 - done.
     Input file size is 2500, 2500
     0...10...20...30...40...50...60...70...80...90...100 - done.
     0...10...20...30...40...50...60...70...80...90...100 - done.
     ...

.. Attention:: Ce processus peut prendre quelques secondes.

A ce stade, les ensembles de données optimisés ont été préparés et ils sont prêts à être servis par GeoServer comme ImageMosaic. 

gdalwarp
````````
Cet utilitaire permet de se déformer et reprojeter un ensemble de données. Les étapes suivantes fournissent des instructions à l'ensemble de données Reproject aérienne (qui a "EPSG:26913" système de coordonnées de référence) à WGS84 ("EPSG:4326").

Exécution de la commande::

     gdalwarp

permet d'obtenir la liste des paramètres pris en charge::

     Usage: gdalwarp [--help-general] [--formats]
            [-s_srs srs_def] [-t_srs srs_def] [-to "NAME=VALUE"]
            [-order n | -tps | -rpc | -geoloc] [-et err_threshold]
            [-refine_gcps tolerance [minimum_gcps]]
            [-te xmin ymin xmax ymax] [-tr xres yres] [-tap] [-ts width height]
            [-wo "NAME=VALUE"] [-ot Byte/Int16/...] [-wt Byte/Int16]
            [-srcnodata "value [value...]"] [-dstnodata "value [value...]"] -dstalpha
            [-r resampling_method] [-wm memory_in_mb] [-multi] [-q]
            [-cutline datasource] [-cl layer] [-cwhere expression]
            [-csql statement] [-cblend dist_in_pixels] [-crop_to_cutline]
            [-of format] [-co "NAME=VALUE"]* [-overwrite]
            srcfile* dstfile

Lorsque le sens des principaux paramètres est résumée ci-dessous:
  * *-s_srs*: permet de spécifier le système de référence de coordonnées de source
  * *-t_srs*: permet de spécifier le cible du système de référence de coordonnées 
  * *-te*: permet de définir les extensions géoréférencées (exprimé en cible CRS) de l'output
  * *-tr*: permet de spécifier la résolution de sortie(exprimée en unités cibles géoréférencées)
  * *-ts*: permet de spécifier le format de sortie en pixels et les lignes.
  * *-r*: permet de spécifier la méthode de rééchantillonnage(l'un des près, bilinéaire, cubique, cubicspline et lanczos)
  * *-srcnodata*: permet de spécifier les valeurs de la bande à être exclus de l'interpolation.
  * *-dstnodata*: permet de spécifier les valeurs nodata le fichier de sortie.
  * *-wm*: permet de spécifier la quantité de mémoire (exprimée en méga-octets) utilisée par l'API de déformation pour la mise en cache.


*gdalwarp - Reprojetez échantillon de données à WGS84*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. exécuter::

     cd ${TRAINING_ROOT}/data/user_data/retiled

     gdalwarp -t_srs "EPSG:4326" -co "TILED=YES" 13tde815295_200803_0x6000m_cl.tif 13tde815295_200803_0x6000m_cl_warped.tif

   pour reprojeter à l'ensemble de données aérienne spécifié pour le système de référence de coordonnées WGS84.

#. Eventuellement, vérifiez que reprojection a été réussie, en exécutant la commande::

     gdalinfo 13tde815295_200803_0x6000m_cl_warped.tif

   .. figure:: img/fw_warpedgdalinfo.png

      une partie de *gdalinfo* output sur données déformé. remarquerez la propriété mise à jour **Coordinate System** 


Dans la section :ref:`next <geoserver.mosaic_pyramid>`, des instructions pour configurer un imagemosaic seront fournis.
