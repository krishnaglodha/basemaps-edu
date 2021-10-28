.. module:: geoserver.shp_postgis
   :synopsis: Apprenez à charger un Shapefile dans PostGIS.

.. _geoserver.shp_postgis:

Charger un Shapefile dans PostGIS
-----------------------------------

Cette tâche montre comment charger un ShapeFile dans la base de donnèes PostGIS:

#. Ouvrir la fenêtre de terminal et entrez la commande suivante et appuyez sur enter pour créer une nouvelle base de données nommée'shape':

  * Linux::
   
      createdb -U postgres -T postgis20 shape

  * Windows::

      setenv.bat  
      cd %TRAINING_ROOT%\postgres\bin
      createdb -U postgres -T postgis20 shape

#. Entrez la commande suivante et appuyez sur enter pour charger le ShapeFile dans la base des donnèe 'shape':

  * linux::
    
     shp2pgsql -I ${TRAINIG_ROOT}/data/user_data/Mainrd.shp public.main_roads | psql -d shape
     
  * windows::
  
     cd %TRAINING_ROOT%
     "postgres\bin\shp2pgsql" -I "data\user_data\Mainrd.shp" public.main_roads > maindrd.sql
     "postgres\bin\psql" -f  maindrd.sql -U postgres -d shape

   Le ShapeFile sera chargé dans la table 'main_roads' de la base des donnèe 'shape' database. La capture d'écran ci-dessous montre une partie du contenu de la table dans ``pgAdmin III``

   .. figure:: img/shp_postgis1.png

      Une table PostGIS par ShapeFile

Dans la section :ref:`next <geoserver.postgis_lay>`  nous allons voir comment ajouter une couche PostGIS dans GeoServer.
