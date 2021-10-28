.. module:: geoserver.template

.. _geoserver.template:


Personnalisation KML Placemark avec des modèles
-----------------------------------------------

En KML, chaque entrée dans un ensemble de données est représentée avec un repère. Un repère a un titre et une description qui est montré dans une bulle pop-up lorsque le repère est cliquée. Avec les modèles, nous pouvons personnaliser titre and descriptions 

#. Cliquez sur le lien suivant pour ouvrir la couche ``storm_obs`` en modallité superoverlay:

   http://localhost:8083/geoserver/wms/kml?layers=geosolutions:storm_obs&mode=superoverlay
   
#. Dans **Google Earth** cliquez sur un repère.

   .. figure:: img/template1.jpg

      Regarder la description repère par défaut

#. Sur le système de fichiers accédez au répertoire de données de GeoServer situé à :file:`$GEOSERVER_DATA_DIR`.

#. Dans le repertoire :file:`workspaces/geosolutions/storms/storm_obs` directory créer un nouveau domaine nommé :file:`title.ftl`.

   .. figure:: img/template2.jpg

      Création d'un modèle de titre

   .. note:: le :file:`title.ftl` est un modèle qui servira à afficher le titre d'un repère

#. Ouvrez :file:`title.ftl` dans l'éditeur de texte de votre choix et entrer dans le contenu ci-dessous::

    Hurricane ${storm_name.value}

   .. note:: les contenus modèle ci-dessus insère la valeur de l'attribut **storm_name**. Lorsque le modèle est rendue, il va créer dynamiquement un titre qui est le nom de l'ouragan. 

   .. figure:: img/template3.jpg

      Création titre contenu du modèle

#. Dans Google Earth actualisez la vue en élargissant ``stom_obs.kmz`` sous "Temporary places", cliquez droit sur ``gesolutions:storm_obs``, et cliquez sur le menu de rafraîchissement :


   .. figure:: img/refresh.png

      Forcer le rafraîchissement d'un arbre KML


#. Cliquez sur un repère pour voir le nouveau modèle en action:

   .. figure:: img/template4.jpg

      Viewing a custom placemark title


#. Dans le même répertoire que :file:`title.ftl` créer un fichier nommé :file:`description.ftl`. Ouvrez le fichier et entrez le contenu suivant:: 

     <img src="http://localhost:8083/geoserver/www/hurricane_warning.png"></img>
     <br>
     <br>
     On <b>${obs_datetime.value}</b> hurricane ${storm_name.value} was recorded to have a wind speed of <b>${wind.value}</b> mph.
     <br>
     <br>

   .. note:: Le modèle ci-dessus rend certains HTML qui contient une image statique d'un avertissement d'ouragan, ainsi que crée un paragraphe de texte décrivant sous forme de phrase quelques informations sur l'observation de la tempête spécifique.

   .. figure:: img/template5.jpg

      Création de description de contenu de modèle

#. 	Sauver :file:`description.ftl` et rafraîchir l'affichage dans Google Earth.

   .. figure:: img/template6.jpg

      Regarder une description personnalisé d'un repère personnalisé

Dans cette section, les modèles ont été utilisés pour personnaliser la visualisation du repère. Dans les prochaines sections l'utilisation de modèles à d'autres fins de visualisation sera explorée.
