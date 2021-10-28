.. module:: geoserver.time

.. _geoserver.time:


Génération d'une série chronologique KML
-----------------------------------------

Lorsque les données spatiales sont combinées avec des informations temporelles des effets de visualisation peuvent être très intéressant. L'exposition de comment les objets changent au fil du temps dans l'espace est une animation puissante communément appelé une série chronologique. Cette section explique comment utiliser les modèles de GeoServer avec le soutien de Google Earth pur creér une série chronologique.

#. Dans le même répertoire que le :file:`title.ftl` et :file:`description.ftl` modèles ont été créés dans la section précédente, a créé un nouveau modèle appelé :file:`time.ftl`.

   .. figure:: img/time1.jpg

      Création d'un nouveau modèle de temps

#. Ouvrez :file:`time.ftl` avec un éditeur de texte et saisissez le contenu suivant::

     ${obs_datetime.value} 

   .. note:: Le modele de temps functionne par l'obtention d'une date à partir d'un attribut**temporal**. 

   .. figure:: img/time2.jpg

      Création de contenu pour un modèle de temps.      

#. Sauvez :file:`time.ftl`, fermer Google Earth et suivre à nouveau ce lien:

   http://localhost:8083/geoserver/wms/kml?layers=geosolutions:storm_obs&mode=download

   .. note:: Cette fois, nous utilisons``mode=download`` pour avoir GE qui obtient l'ensemble de données complet, puisque nous allons voir des parties en fonction du temps, par opposition à la zone géographique et de l'importance

#. Un curseur de temps doit apparaître dans la partie supérieure de l'orifice de vue. 

   .. figure:: img/time3.jpg

      Affichage des données temporelles dans Google Earth

#. Modifier l'intervalle de temps dans le curseur temporel à **1 month**, et régler le **animation speed** pour d'être lente.

   .. figure:: img/time4.jpg
    
      Réglage des paramètres d'animation du temps.

#. Cliquez :guilabel:`OK` et démarrer l'animation.

   .. figure:: img/time5.jpg

      Regarde la série temporelle des ouragans
A ce stade, une animation temporelle de l'ensemble de données a été créée avec la simple utilisation de modèles. Dans la section :ref:`next <geoserver.height>`  modèles seront encore utilisés de nouveau pour ajouter un autre aspect de la visualisation.
