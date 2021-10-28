.. module:: geoserver.postgis_lay
   :synopsis: Apprenez à ajouter un layer PostGIS.

.. _geoserver.postgis_lay:

Ajouter Postgis layer
----------------------

Cette tâche montre comment ajouter un layer PostGIS dans GeoServer:


#. Accedez à la page de GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Si vous n'êtes pas déjà connecté, sur la page d'accueil localiser le formulaire situé dans le coin supérieur droit :guilabel:`Login`, ae Entrez le Nom d'utilisateur "admin" et mot de passe "Geos".

   .. figure:: img/vector1.jpg
      :width: 600

      GeoServer Login

#. Cliquez le lien :guilabel:`Add stores`.

   .. figure:: img/vector2.jpg

      Ajouter store lien

#. sélectionner le lien :guilabel:`PostGIS` et cliquez le.


   .. figure:: img/postgis_lay1.png

      Ajouter un nouveau PostGIS Store

#. Sur la page :guilabel:`New Vector Data Source` remplir le paramètre suivant:

   - :guilabel:`Data source name`, 'shape'
   - :guilabel:`database`, 'shape' le nom de la base de données créée dans l'étape précédente.
   - :guilabel:`user`, 'geosolutions' le nom du propriétaire de la base de données.
   - :guilabel:`password`, 'Geos' le mot de passe de l'utilisateur.
   
   et cliquez :guilabel:`Save`.

   .. figure:: img/postgis_lay2.jpg

      Réglage des paramètres de connexion de base de données

#. Après l'enregistrement, vous serez redirigé vers une page qui répertorie toutes les couches de la base de données PostGIS et vous donne la possibilité de publier un d'entre eux. Cliquez :guilabel:`Publish`.

   .. figure:: img/postgis_lay4.png

      La publication d'un layer de la table PostGIS

#. Les domaines :guilabel:`Name` et :guilabel:`Title` doit être rempli automatiquement. Remplissez le domaine :guilabel:`Declared SRS` pour définir les systèmes de référence de coordonnées et générer les limites du layer en cliquant sur les boutons :guilabel:`Compute from data` et :guilabel:`Compute from native bounds` dans la section :guilabel:`Bounding Boxes`


   .. figure:: img/postgis_lay5.png
   .. figure:: img/postgis_lay6.png

     Remplir les champs et generer le layer bounding box

#. Allez au bas de la page, remarquez que la table de seule lecture :guilabel:`Feature Type Detail` et cliquez :guilabel:`Save`.

   .. figure:: img/postgis_lay7.png

      Présentation de la configuration de layer

#. Si tout va bien, vous devriez voir quelque chose comme ceci:

   .. figure:: img/postgis_lay8.png

      Après un sauvetage réussi

#. A ce stade, le layer PostGIS a été ajouté et est prêt à être servi par GeoServer. Utilisez l'aperçu de la couche pour afficher son contenu, filtrage sur le nom 'main_road'.
