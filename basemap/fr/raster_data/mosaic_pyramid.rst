.. module:: geoserver.mosaic_pyramid

.. _geoserver.mosaic_pyramid:

Gestion des mosaïques et pyramides
-----------------------------------
Dans cette section, vous apprendrez à gérer des mosaïques d'images et de pyramides d'images dans GeoServer.

Configuration d'une mosaïque d'images
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Une mosaïque d'images est composé d'un ensemble d'ensembles de données qui sont exposées comme une seule couverture. Le format de imagemosaic permet de construire automatiquement et de configurer une mosaïque à partir d'un ensemble de bases de données géoréférencées.
Cette section fournit de meilleures instructions pour configurer une mosaïque d'images

.. note:: Avant de commencer, s'assurer que la section :ref:`Maps - Raster <geoserver.mosaic>` a été achevée.

Nous allons configurer un imagemosaic en utilisant le jeu de données optimisé. Comme expliqué dans la section :ref:`Maps - Raster <geoserver.mosaic>`, suivez les étapes 1 à 6. Puis:

#. Spécifiez un nom (par exemple, :file:`boulder_bg_optimized`) dans le domaine :guilabel:`Data Source Name` ou l'interface. 
#. Spécifiez :file:`file:<TRAINING_ROOT>/data/user_data/optimized` comme URL des données d'échantillon dans le domaine :guilabel:`Connections Parameter's - URL`. 

   .. figure:: img/mosaic_addraster.jpg

#. Cliquez :guilabel:`Save`. 

#. Publier le calque en cliquant sur ​​le lien :guilabel:`publish`. 

   .. figure:: img/mosaic_publish.jpg
   
#. Set :file:`boulder_bg_optimized` que le nom et le titre de la couche. 

   .. figure:: img/mosaic_setname.jpg

#. Vérifiez les systèmes de référence de coordonnées et les champs de boîtes englobantes sont correctement réglés.

#. Personnalisez les propriétés de imagemosaic si nécessaire. Pour la mosaïque de l'échantillon, réglez le OutputTransparentColor à la valeur **000000** (Quelle est la couleur Noir). cliquez sur :guilabel:`Save` lorsque vous avez terminé. 

   .. figure:: img/mosaic_parameters.png

* *AllowMultithreading*: Si c'est vrai, permettre à carreaux multithreading chargement. Cela permet d'effectuer le chargement parallélisé des granules qui composent la mosaïque.
* *BackgroundValues*: Réglez la valeur du fond de mosaïque. Selon la nature de la mosaïque, il est sage de définir une valeur pour le pas de zone de données (usually -9999). Cette valeur est répété sur toutes les bandes de mosaïque.
* *InputTransparentColor*:Régler la couleur transparente pour les granulés avant de les mosaïquer afin de contrôler le processus de superposition entre eux. Quand GeoServer compose les granules pour satisfaire la demande des utilisateurs, certains d'entre eux peuvent se chevaucher d'autres, donc, de régler ce paramètre avec la couleur opportun évite le chevauchement des pas de zones de données entre les granules.
* *MaxAllowedTiles*: Définissez le nombre maximum des tuiles qui peuvent être charger simulatenously pour une demande. Dans le cas d'une grande mosaïque ce paramètre doit être opportunément configuré pour ne pas saturer le serveur avec trop de granules chargés en même temps.
* *OutputTransparentColor*: Régler la couleur transparente pour la mosaïque créée.
* *SUGGESTED_TILE_SIZE*: Contrôle la taille des carreaux des granules d'entrée ainsi que la taille de la mosaïque de la mosaïque de sortie. Il se compose de deux entiers positifs séparés par une virgule, like 512,512.
* *USE_JAI_IMAGEREAD*: Si c'est vrai, GeoServer fera usage de fonctionnement JAI ImageRead et son mécanisme de chargement différé pour charger granules; si elle est fausse, GeoServer effectuera ImageIO direct en lecture appels qui se traduira par charge immédiate.

A ce stade, le imagemosaic est publié avec GeoServer. La prochaine étape est de vérifier comment les performances d'accès aux ensembles de données ont été améliorées.

#. Cliquez le lien :guilabel:`Layer Preview` dans le menu de gauche GeoServer. 

#. recherchez la couche *geosolutions:boulder_bg* (l'ensemble de données sans optimisation) et cliquez le lien :guilabel:`OpenLayers` à côté de celui-ci. 

   .. figure:: img/mosaic_pratopreview.jpg

#. Jouez avec la carte prévisualisation en zoom et de panoramique. Lors d'un zoom, le temps de réponse n'est pas immédiate en raison de l'accès aux grandes bases de données sous-jacentes qui n'ont pas été optimisés.

#. Retour à la page :guilabel:`Layer Preview`. 

#. Recherchez la couche *geosolutions:boulder_bg_optimized* (l'ensemble de données optimisé avec carrelage et un ensemble de vues d'ensemble)cliquez le lien :guilabel:`OpenLayers` à côté de celui-ci. 

   .. figure:: img/mosaic_retiledpreview.jpg

#. Jouez avec la carte prévisualisation en zoom et de panoramique. Vérifiez la façon dont les performances ont été améliorées (s'appuyant sur ​​les aperçus et le carrelage). A noter également la qualité d'image des points de vue les plus bas de résolution, après avoir utilisé un algorithme d'interpolation moyenne lors de la création des aperçus.

Configuration d'une pyramide d'images
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GeoServer peut traiter efficacement avec le BigTIFF avec des aperçus, tant que le TIFF est en dessous de la limite de taille de 2 Go. Une fois que la taille de l'image va au-delà de cette limite, il est temps de commencer à envisager une pyramide d'images à la place. Une pyramide d'images construit plusieurs mosaïques d'images, chacun à un niveau de zoom différent, faisant en sorte que chaque carreau est stocké dans un fichier séparé. Cela vient avec une surcharge de composition pour ramener les tuiles en une seule image, mais peut accélérer le traitement de l'image que chaque ensemble est carrelé, et donc un des sous-ensemble on peut y accéder de manière efficace (par opposition à un seul GeoTIFF, où le niveau de base peut être carrelée, mais les aperçus ne les sont jamais).
.. note::

   Afin de construire la pyramide, nous allons utiliser l'utilitaire `gdal_retile.py <http://www.gdal.org/gdal_retile.html>`_ , partie des utilitaires de ligne de commande GDAL et disponibles pour différents systèmes d'exploitation.

  
#. Accédez au répertoire de l'atelier et copier le répertoire `bmpyramid` dans le répertoire `<TRAINING_ROOT>\\data\\user_data`

#. De la ligne de commande exécuter::

  * Linux::

      cd $GEOSERVER_DATA_DIR/data/user_data
      
      gdal_retile.py -v -r bilinear -levels 4 -ps 2048 2048 -co "TILED=YES" -co "COMPRESS=JPEG" -targetDir bmpyramid bmreduced.tiff
      
  * Windows::
  
      cd %TRAINING_ROOT%\\data\\user_data\\
      
      gdal_retile -v -r bilinear -levels 4 -ps 2048 2048 -co "TILED=YES" -co "COMPRESS=JPEG" -targetDir bmpyramid bmreduced.tiff


   Le guide utilisateur `gdal_retile.py  <http://www.gdal.org/gdal_retile.html>`_ fournit une explication détaillée de tous les paramètres possibles, voici une description de ceux utilisés dans la ligne de commande ci-dessus:
   
     * `-v`: verbose output,permet à l'utilisateur de voir chaque création de fichier passer donc des progrès ont été réalisés (construction d'une grande pyramide peut prendre des heures)
     * `-r bilinear`: utiliser une interpolation bilinéaire lors de la construction des niveaux de résolution inférieurs. Cela est essentiel pour obtenir une bonne qualité d'image sans demander GeoServer pour effectuer des interpolations coûteuses en mémoire
     * `-levels 4`: le nombre de niveaux de la pyramide
     * `-ps 2048 2048`: chaque carreau dans la pyramide sera 2048x2048 GeoTIFF
     * `-co "TILED=YES"`: chaque tuile GeoTIFF dans la pyramide sera intérieure carrelée
     * `-co "COMPRESS=JPEG"`: chaque tuile GeoTIFF dans la pyramide sera JPEG compressé (métiers de petite taille pour des performances supérieures, essayer sans ce paramètre trop)
     * `-targetDir bmpyramid`: construire la pyramide dans le répertoire bmpyramid. Le répertoire cible doit exister et être vide
     * `bmreduced.tiff`: le fichier source
  
   Ceci va produire un certain nombre de fichiers TIFF dans bmpyramid avec les sous-répertoires `1`, `2,` `3`, and `4`.

#. Allez à la section **Stores** et ajouter un nouveau ``Raster Data Source`` cliquez **ImagePyramid**:

   .. figure:: 
      img/pyramid1.png

      *Ajout d'une source de données ImagePyramid*

   .. Attention::
    
      Cela suppose l'image pyramide GeoServer plug-in est déjà installé. La pyramide est normalement une extension.
	  
      Si vous utilisez l'installation de Windows, avant de faire l'exercice installer le 'geoserver-2.2-SNAPSHOT-pyramid-plugin' from :file:`%TRAINING_ROOT%\\data\\plugins\\`. Juste décompresser le fichier zip dans  :file:`%TRAINING_ROOT%\\webapps\\geoserver\\WEB-INF\\lib\\` et redémarrez GeoServer.

#. Spécifiez un nom (``bm_pyramid``) dans le champ Nom de la source de données de l'interface et spécifier une URL correcte avec le répertoire de données de la pyramide: 

   .. figure:: 
      img/pyramid2.png

      *Configuration d'un magasin de pyramide d'images*

#. Cliquez le buton **Save**.

   .. note:: 
    
      En cliquant save le magasin va chercher dans le répertoire, reconnaître un structure genèré ``gdal_retile`` et effectue certaines opérations en arrière-plan::
      	
      		- déplacer tous les fichiers TIFF dans la racine d'un répertoire nouvellement créer 0
                - créer une mosaïque d'images dans toutes les sous-répertoires (shapefile index plus property file)
                - créer le fichier de propriétés de la racine décrivant l'ensemble de la structure de la pyramide

#. Publier la nouvelle pyramide créée:

   .. figure:: 
      img/pyramid3.png

      *Le choix de la couverture de l'édition*

#. Configurez le paramètre de couche USE_JAI_IMAGEREAD false pour obtenir une meilleure évolutivité:

   .. figure:: 
      img/pyramid4.png

      *Réglage des paramètres de la pyramide*

#. Cliquez le buton **Submit** et allez à GeoServer **Map Preview** pour voir la pyramide:

   .. figure:: 
      img/pyramid5.png

      *Prévisualisation de la pyramide*