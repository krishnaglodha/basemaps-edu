.. _geoserver.base_map:

Créer une Carte de Base avec un Groupe de Couches
--------------------------------------------------

La meilleure voie pour configurer une carte avec plus d'une couche par consommation est de créer un `Layer Group`, un groupe de couches, ce que on va faire dans cette séction

#. Localisez le lien :guilabel:`Layer Group` et cliquez le.

   .. figure:: img/group1.png
      :height: 500

      Lien Groupe de Couches

#. Cliquez le lien :guilabel:`Add new layer group` .

   .. figure:: img/group2.png

      Add new layer group link

#. Nommez-le ``test``, et cliquez sur le lien :guilabel:`Add Layer`.

   .. figure:: img/group3.png
      :height: 500
	  
      Ajoutez une nouvelle couche

#. Selectionnez la couche "Mainrd" dans la fenetre popup.

   .. figure:: img/group4.png

      Selectionnez une couche

#. Ajoutez les autres couches jusq'à ce que vous voyez ça:

   .. figure:: img/group5.png

      Liste des couches pour le groupe

   .. note:: Vous pouvez utiliser ler fleches vertes pour organiser les couches jusq'à qu'elles ressemblent la figure au dessus.

#. Cliquez le bouton  produisez des frontières pour que GeoServer calcule les bords du groupe des couches à son intérieur:

#. Faite défiler jusq'au bout de la page puis cliquez :guilabel:`Save`.

#. Si tout s'est bien passé, vous dévriez voire une chose pareille:

   .. figure:: img/group7.png

      Après avoir sauvé avec succès.

	 .. note:: Les limites crées automatiquement peuvent etre trop grandes et l'avant-première pourrait résulter désagreable à voire. Vous pouvez alors réduire les bords du groupe de couches introduisant manuellement les valeurs des boites, comme celles suivantes:
	           minx = 3.057.566,8646; maxx = 3.079.500,65246; miny = 1.241.929,35617; maxy = 1.257.467,5777

Le groupe de couches est pret à etre utilisé:

#. Naviguez jusq'à GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Allez jusq'au lien à la fin de la page au menu à votre gauche :guilabel:`Layer Preview`.
 
   .. figure:: img/preview1.png

      Avant-première des couches

#. Trouvez le `test` du groupe de couches et cliquez sur le lien :guilabel:`OpenLayers`. Vous allez voire une carte glissante avec toutes les couches configurées de la région des Rochers. Vous pouvez controler le zoom utilisant le rouleau du souris, faire une panoramique en trainant et grandir l'image par la fenetre en appuyant sur ``SHIFT`` pendant q'on traine.

   .. figure:: img/preview3.png

      Voir les Couches Ouvertes

   .. note:: Controlez la barre d'addresse du navigateur pour avoir un example interessant d'une requete WMS pour la couche.
      
#. Comme vous avez pu remarquer avant on a déja configuré pour vous un groupe plus grand et réalistique. Son nom est ``boulder``. Jetez un oeil à sa définition et ajoutez-la à la couche `Mainrd`. Puis en utilisant les fleches vertes, faites bouger la couche jusq'à la position suivante (regardez l'immage sur l'écran).

   .. figure:: img/preview3b.png

      Une nouvelle couche dans un groupe de couches dèja existant.
	  
#. Puis utilisez l'avantpremière pour la montrer.



#. Essayez de cliquer dans le Try clicking dans le milieu de la carte. Vous devrez voir au fond une couple de tables avec plus d'informations sur les caractéristiques véctorielles clicquèes. 
   
   .. figure:: img/preview4.png

      Infos sur les caractéristiques

#. Essayez de vous rapprocher de plus en plus. Des nouvelles couches commencent à apparaitre. Celui est le style en fonction de l'échelle.