.. module:: geoserver.overlays

.. _geoserver.overlays:


GeoServer automatique mode de rafraîchissement
----------------------------------------------

Lorsque vous utilisez le KML réflecteur GeoServer sert la commutation entre raster et le mode vecteur de données pour éviter Google Earth être submergés par un trop grand nombre de données vectorielles: il commence avec une trame WMS overlay il commence avec une trame WMS superposition et quand assez près du sol, il passe en mode vectoriel. Raster sage, il s'assure que les données sont actualisées avec les versions supérieures de résolution que l'utilisateur zoome.
   
Vector actualisation automatique
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Ouvrez votre navigateur et saisissez cette demande, après le téléchargement, ouvrez le fichier KML avec Google Earth::

    http://localhost:8083/geoserver/geosolutions/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:bbuildings&styles=polygon&bbox=3056998.71136,1255540.05688,3066102.02166,1260326.72900&srs=EPSG:2876&format=application/vnd.google-earth.kml+XML&width=407&height=512

   .. Attention:: depuis le ``geosolutions:bbuildings`` est une grande base de données nous avons filtré sur une petite surface à travers le BBOX; être conscient que l'information de fonction peut prendre un temps très long à télécharger et utiliser beaucoup de ressources client side (le fichier KML résultant peut avoir une taille de hundread de Mo). Cela signifie également que la partie des données est toujours disponible dans Google Earth.
#. Entrez dès maintenant dans votre navigateur cette demande, après le téléchargement, ouvrez le fichier KML avec Goolge Terre::

	http://localhost:8083/geoserver/wms/kml?layers=geosolutions:bbuildings&styles=polygon

   Dans ce cas GeoServer prend peu de temps pour produire un petit fichier KML contenant un lien dynamique qui obtient automatiquement rafraîchie par Google Earth lorsque les changements de zoom: GoeServer utilise le niveau de zoom afin de déterminer s'il ya lieu de renvoyer une superposition WMS ou des données vectorielles plaine, et s'assure que la zone d'affichage actuelle est toujours couvert.

Actualisation automatique de raster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Ouvrez votre navigateur et saisissez cette demande::

	http://localhost:8083/geoserver/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:boulder_bg&styles=&bbox=474000.0,4425000.0,483000.0,4435500.0&width=438&height=512&srs=EPSG:26913&format=application/vnd.google-earth.kml+XML

   .. figure:: img/overlay1.png
      :width: 750

      Demande KML standard

#. zoom avant:

   .. figure:: img/overlay2.png
     

     Google earth zoom avec demande KML norme

#. Ouvrez votre navigateur et saisissez cette demande::

	http://localhost:8083/geoserver/wms/kml?layers=geosolutions:boulder_bg

   .. figure:: img/overlay3.png

     KML demande Reflector

#. zoom avant:

   .. figure:: img/overlay4.png
      :width: 750

      Google earth zoom avec KML Reflector demande et l'actualisation automatique
