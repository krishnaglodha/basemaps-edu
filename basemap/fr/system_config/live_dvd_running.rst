.. module:: geoserver.live_dvd_running

.. _geoserver.live_dvd_running:


Fair marcher le DVD.
--------------------

Le groupe de datas et des matières sont fournies pendant la vision du DVD envoyé avec cette documentation. Le DVD donne un exemple de XUbuntu 9.10 personalisée pour ce groupe de travail par `Geosolutions <http://www.geo-solutions.it>`_ qui contient le logiciel suivant:

#. Le `SUN Java <http://www.java.com>`_ JDK 1.6 et le `Tomcat <http://tomcat.apache.org/index.html>`_ 6.0.30 Servlet Conteneur

#. GDAL libraries et `FWTools 2.0.6 <http://fwtools.maptools.org/>`_

   .. note::  FWTools est un ensemble de GIS utilitaires pour gouverner et manipuler les datas GIS. FWTools comprend OpenEV, GDAL, MapServer, PROJ.4 et OGDI et en plus des components de support.

#. Un example de `GeoServer 2.1.x <http://geoserver.org/display/GEOS/Nightly>`_ configuré avec des datasets et des extensions, comme, par exemple, RESTConfig, ImageMosaic, ImagePyramid, Chart Dynamic Symbolizer

   .. note::  pour marcher GeoServer il faut disposer d'un Java Development Kit (JDK), et non simplement d'un Java Runtime Environment (JRE).

#. Le plus populairs GIS clients comme `GoogleEarth <http://earth.google.com>`_ 6.2, `uDig <http://udig.refractions.net/>`_ 1.3.1 and `QGis <http://www.qgis.org/>`_ 1.8.0

Lancer le DVD
^^^^^^^^^^^^^^^^^^

Le DVD contient une version complètement fonctionnelle de Xbuntu 9.10. Elle peut commencer à marcher automatiquement du DVD ou elle peut aussi etre installée sur le système, PC ou VM (voire la séction suivante). 

Voilà les instructions pour faire marcher le DVD:

#. Vèrifiez que la séquence d'initialisation du système sur votre ordinateur vous permet de controler CD/DVD ou USB avant le HD.

	* Avant tout mettez en marche votre ordinateur. Pendant la sèquence d'initialisation, généralement dans la première ou la deuxième image visualisée sur votre écran, vous pourrez voire un message, "press f8 for set up" ou "press ctrl-alt-del to enter setup". Des différentes machines peuvent vous demander d'introduire CMOS setup dans des différents façons mais un ordinateur individuel devrait vous donner un indice sur comment le faire pendant la sèquence d'initialisation. 

	* Une fois que vous etes dans le BIOS/CMOS setup vous pourrez explorer l'environnement. Normalement la fleche vous permettra de bouger dans les programmes utilitaires mais il est possible que votre ordinateur vous demande d'utiliser des clés supplémentaires ou totalement différentes. Il est important donc de vous familiariser avec votre version de CMOS avant d'effectuer n'importe quel changement.

	* Maintenant que vous avez gagné un peu de confiance avec CMOS vous pouvez continuer à naviguer dans les données jusq'à que vous allez voire la séquence d'initialisation. Selectionnez ce programme utilitaire et changez-le selon la séquence que vous voulez obtenir (assurez-vous d'avoir déplacé la clé du DVD/USB au sommet de la liste pour permettre à la machine d'effectuer la sèquence d'initialisation par eux au lieu d'initialiser le disque dur local).  

	* Finalement controlez ce que vous avez fait pour etre sur que vous avez effectué les choix correctes. Sauvez la nouvelle séquence et appuyez sur Esc quand vous avez terminé. Cela devrait vous conduire en arrière jusq'à l'initialization du système et dépendant des changements que vous avez effectué il pourrait vous démander de redémarrer votre ordinateur pour abiliter les changements faits. 

#. Assurez-vous que le DVD soit correctement inséré dans le DVD-ROM lecteur avant la séquence d'initialisation de l'ordinateur. 

#. Quand l'ordinateur se met en marche vous devrez voire un écran de choix comme le suivant:

   .. figure:: img/dvd_run1.jpg
      :width: 600
	  
	  
      Première initialisation du DVD
      
#. Appuyez sur ENTER ou tapez 'live' et appuyez sur ENTER pour faire commencer le système du DVD.

	 .. note:: Ignorez les autres options pour le moment, on va les reprendre plus en avant dans cette guide.
	 
#. Si le DVD marche correctement sans aucun erreur vous allez voire le Xbuntu GNOME GUI comme montré en dessous.

   .. figure:: img/dvd_running.jpg
      :width: 600
	  
      Lubuntu LiveDVD GUI
