.. module:: geoserver.add_shp
   :synopsis: Learn how to adding a Shapefile.

.. _geoserver.add_shp:

Ajout d'un Shapefile
--------------------

L'ajout de shapefile est celle qui est au cœur de tout instruments SIG. Cette section traite de l'ajout et de la publication de Shapefile avec GeoServer.

#. Accédez au workshop directory :file:`$[TRAINING_ROOT}/data/user_data/` (on windows :file:`%TRAINING_ROOT%\\data\\user_data`) situé sur le desktop et trouvez les suivantes shapefiles::

     Mainrd.shp
     Mainrd.shx
     Mainrd.dbf
     Mainrd.prj

   Copiez les fichiers dans la directory suivante::

     $GEOSERVER_DATA_DIR/data/boulder

   .. note:: Assurez-vous que tous les quatre parties de shapefile sont copiés.  Cela inclut les extensions ``shp``, ``shx``, ``dbf``, and ``prj``.

#. Allez à GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Sur la page d'accueil localiser le :guilabel:`Login` form situé dans le coin supérieur droit, puis entrez le nom d'utilisateur "admin" et le mot de passe "Geos".

   .. figure:: img/vector1.jpg
      :width: 600
		
      GeoServer Login

#. cliquez le lien :guilabel:`Add stores`.

   .. figure:: img/vector2.png
     
   
      Ajouter store lien

#. Sélectionnez le lien :guilabel:`Shapefile` et cliquez-le.

   .. figure:: img/vector3.png
     

      ajouter en nouvelle shapefile

   .. note:: Le nouveau menu de data source contient une liste de tous les formats spatiales  soutenue par GeoServer. Lors de la création d'une nouvelle banque de données l'un de ces formats doivent être choisis. Formats comme Shapefile et PostGIS sont soutenue par défaut, et de nombreux autres formats sont disponibles sous forme d'extensions.

#. Sur la page :guilabel:`Edit Vector Data Source` entrer "Mainrd" dans le :guilabel:`Data Source Name` et :guilabel:`Description` fields. Enfin, cliquez sur le lien de navigation afin de définir l'emplacement du Shapefile dans le domaine :guilabel:`URL` et cliquez :guilabel:`Save`.

   .. note:: Le Mainrd.shp est au :file:`$[TRAINING_ROOT}/data/boulder/Mainrd.shp` (sur windows :file:`%TRAINING_ROOT%\\data\\boulder\\Mainrd.shp`)
   
   .. figure:: img/vector4.png
      :width: 600
	  
      Spécification des paramètres de Shapefile

#. Après l'enregistrement, vous serez redirigé vers une page que répertorie toutes les layers dans le shapefile  et vous donne la possibilité de publier un d'eux. Cliquez :guilabel:`Publish`.

   .. figure:: img/vector5.png
      :width: 600
	  
      Publiquer un layer d'un shapefile

#. Le :guilabel:`Coordinate Reference Systems` doit être peuplé manuellement. Les domaines :guilabel:`Name` et :guilabel:`Title`  est rempli automatiquement.

   .. figure:: img/vector6.png
      :width: 600
	  
      Remplissez les champs.

   
   Faites défiler la page et générer les limites du layer  en cliquant sur le bouton :guilabel:`Compute from data` dans la section :guilabel:`Bounding Boxes`.

   .. figure:: img/vector7.png
      :width: 600
	  
      Génération du bounding box du layer

#. Allez au bas de la page, remarquer le read only :guilabel:`Feature Type Detail` table et cliquez :guilabel:`Save`.

   .. figure:: img/vector8.png
      :width: 600
	  
      Présentation de la configuration des layers

#. Si tout va bien, vous devriez voir quelque chose comme ceci:

   .. figure:: img/vector9.png
      :width: 600
	  
      Après un sauvetage réussi

   A ce stade le shapefile a été ajouté et est prêt à être servi par GeoServer.
	  
   .. figure:: img/vector10.png
	  :width: 600

#. Choisissez le lien ``preview`` dans le menu principal et filtrer la liste des layers avec ``mainrd``:

   .. figure:: img/preview_shapefile1.png
	  :width: 600
	  
	  sélectionner le ``mainrd`` shapefile dans la layer preview.

#. Cliquez sur le lien ``OpenLayers`` pour voir le layer dans un interactive viewer:

   .. figure:: img/preview_shapefile2.png
	  
      Le ``mainrd`` shapefile preview

Dans la section :ref:`next <geoserver.shp_postgis>` nous allons voir comment charger un shapeFile dans PostGIS.
