.. module:: geoserver.reflector

.. _geoserver.reflector:


Utilisation du KML Reflector
----------------------------

La demande d'un fichier KML d'une couche peut comprendre une demande très lourd::

	http://localhost:8083/geoserver/ows?service=WMS&request=GetMap&version=1.1.1&format=application/vnd.google-earth.kml+XML&width=407&height=512&srs=EPSG:2876&layers=geosolutions:bbuildings&styles=polygon&bbox=3045967.25,1206627.75,3108482.75,1285209.75

Avec :guilabel:`KML Reflector`, GeoServer offre la possibilité de demander KML aux demandes simples en utilisant les valeurs par défaut pour la plupart des paramètres d'une requête WMS norme:


#. Ouvrez votre navigateur et saisissez l'ULR suivant pour télécharger le fichier KML en utilisant le KML Reflector::

	http://localhost:8083/geoserver/wms/kml?layers=geosolutions:bbuildings

   .. figure:: img/reflector1.png

      Standard KML demande Reflector

   .. Attention:: Vous devez effectuer ZoomIn sur Google Earth en raison des paramètres de ScaleDenominator à l'intérieur de la couche SLD par défaut. 
   
  Le tableau suivant présente les hypothèses par défaut:

	.. list-table::
   	   :widths: 20 80
   
   	   * - **Key**
     	     - **Value**
   	   * - ``request``
    	     - ``GetMap``
           * - ``service``
   	     - ``wms``
   	   * - ``version``
   	     - ``1.1.1``
  	   * - ``srs``
   	     - ``EPSG:4326``
   	   * - ``format``
     	     - ``applcation/vnd.google-earth.kmz+xml``
   	   * - ``width``
   	     - ``256`` for vector layers, ``1024`` for raster layers
   	   * - ``height``
   	     - ``256`` for vector layers, ``1024`` for raster layers
   	   * - ``bbox``
             - ``<layer bounds>``
           * - ``kmattr``
   	     - ``true``
   	   * - ``kmplacemark``
    	     - ``false``
   	   * - ``kmscore``
   	     - ``50``
   	   * - ``styles``
   	     - [default style for the featuretype]

#. Utilisez l'URL suivante pour specifier un style particulier:

	http://localhost:8083/geoserver/wms/kml?layers=geosolutions:bbuildings&styles=polygon

   .. figure:: img/reflector2.png

      KML demande réflecteur avec un style particulier

   .. note:: 
      
     Le réflecteur KML peut fonctionner dans l'un des trois modes: **refresh**, **superoverlay**, et **download**. La mode est réglé en ajoutant le paramètre suivant à l'URL::

          mode=<mode>

      where ``<mode>`` est l'un des trois modes de réflecteur. Les détails pour chaque mode sont les suivantes:   
   
          .. list-table::
             :widths: 20 80

             * - **Mode**
               - **Description**
             * - ``Actualiser``
               - (*par défaut pour toutes les versions sauf 1.7.1 à travers 1.7.5*) Retourne KML dynamique qui peut être rafraîchi / mis à jour par le client Google Earth. Les données sont actualisées et de nouvelles données / images sont téléchargées lorsque le zoom / panoramique arrêts. Ce mode peut renvoyer soit vecteur ou raster (repère ou overlay)   La décision de renvoyer soit des données vectorielles ou raster est déterminé par la valeur ``kmscore``.
             * - ``superoverlay``
               - (*défaut pour les versions 1.7.1 à travers 1.7.5*) Retourne KML comme un super-overlay. Retourne KML comme un super-overlay.
             * - ``download``
               - Retours KML qui contient l'ensemble des données. Dans le cas d'une couche vecteur, il s'agira d'une série de repères KML. Avec des couches de trame, il s'agira d'une simple superposition de terre KML. C'est le seul mode qui ne demande pas dynamiquement de nouvelles données depuis le serveur, et est donc KML autonome.



