.. _geoserver.mosaic:

Ajout d'une mosaïque d'images à GeoServer
-----------------------------------------

Cette section traite de la tâche de l'ajout et de la publication d'un fichier imagemosaic avec GeoServer.

#. Accédez au GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Sur la page d'accueil localiser le form :guilabel:`Login` situé dans le coin supérieur droit, et entrez le nom d'utilisateur "admin" et mot de passe "Geos".

   .. figure:: img/vector1.png

      GeoServer Login

#. Cliquez le lien :guilabel:`Add stores`.

   .. figure:: img/vector2.png

      Ajouter le lien de store

#. sélectionner le lien :guilabel:`ImageMosaic` et cliquez.

   .. figure:: img/raster1.png

      Ajouter une nouvelle mosaïque d'images

#. sur la page :guilabel:`Add Raster Data Source` entrer :file:`${TRAINING_ROOT}/data/user_data/aerial` (on windows :file:`%TRAINING_ROOT%\\data\\user_data\\aerial\\`) dans le domaine :guilabel:`URL`, "boulder_bg" dans :guilabel:`Data Source Name` et les domaines :guilabel:`Description`, et cliquez :guilabel:`Save`.

   .. figure:: img/raster2.png

      Spécification des paramètres de magasin

#. Après l'enregistrement, vous serez redirigé vers une page qui répertorie toutes les couches dans le magasin et vous donne la possibilité de publier un d'entre eux. Cliquez :guilabel:`Publish`.

   .. figure:: img/raster3.png

      La publication d'une couche du magasin

#. Le :guilabel:`Coordinate Reference Systems` devraient être automatiquement rempli, ainsi que the :guilabel:`Name`, :guilabel:`Title` et les domaines :guilabel:`Bounding Boxes`.

   .. note:: Changez :guilabel:`Name` et :guilabel:`Title` dans **boulder_bg** comme indiqué sur la figure.

   .. figure:: img/raster4.png

      Champs Auto-peuplées.

#. Allez au bas de la page, puis cliquez sur :guilabel:`Save`.

   .. figure:: img/raster5.png

     Présentation de la configuration de la couche

#. Si tout se passe bien, vous devriez voir quelque chose comme ceci:

   .. figure:: img/raster6.png

      Après un sauvetage réussi.

#. Cliquez sur le lien OpenLayers pour prévisualiser la couche dans un visualiseur interactif, filtering by `boulder_bg` name:

   .. figure:: img/raster7.png

      Aperçu de la mosaïque.


