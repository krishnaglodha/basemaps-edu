.. _geoserver.vector_data.get:

Récupération de données vectorielles et des métadonnées
-------------------------------------------------------

Dans cette section, nous allons apprendre comment faire face à des données vectorielles en utilisant WFS. Nous allons d'abord apprendre à composer avec les métadonnées et puis comment récupérer les caractéristiques. Nous allons utiliser la couche ``Counties`` dans le workshop namespace.

   .. note:: le Open Geospatial Consortium Web Feature Service Interface Standard (WFS) offre un interface qui permit de faire demande pour feature geographique en utilizent platform-independent appels. On peut penser à des caractéristiques géographiques comme le «code source» derrière une carte, considérant que l'interface WMS ou portails de cartographie en ligne comme Google Maps reviennent seulement une image, qui les utilisateurs finaux ne peuvent pas éditer ou analyser spatialement.
  
#. Accédez au GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Sur la page d'accueil localiser le lien :guilabel:`Layer Preview`  (pas besoin de vous connecter).

   .. figure:: img/get1.png

      couche Preview

#. Accédez à WFS GML output de la couche `Counties`.

   .. figure:: img/get2.png

      WFS GML output

  Selon le navigateur, la sortie (output) peut être formatée ou reconnu comme XML. Voici ce que Firefox 3 montre:
   http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=geosolutions:Counties&maxFeatures=50&outputFormat=GML2

   .. figure:: img/get3.png

      Aperçu de la couche WFS par défaut.

   .. note:: Nous vous recommandons le navigateur Web Mozilla Firefox pour naviguer documents de réponse WFS.


#. Maintenant que nous connaissons le moyen rapide et facile pour obtenir des donnèes WFS, revenons et nous le faisons de la façon dont un client WFS fonctionne. Tout d'abord, la seule chose connue devrait être l'URL du serveur WFS: http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0

  En Utilisant cette URL, nous pouvons émettre une demande ``GetCapabilities`` afin de savoir quelle couche il contient et quelles opérations sont prises en charge :
   http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0&request=GetCapabilities

   .. figure:: img/get4.png

      réponse GetCapabilities 

   Si nous faisons défiler ci-dessous, nous allons trouver le type de fonctionnalité décrite ``Counties``:

   .. figure:: img/get5.png

      GetCapabilities réponse (``Counties`` feature type)


#. Maintenant, nous allons demander plus d'information pour la couche ``Counties`` en utilisant une requête ``DescribeFeatureType``:
    http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0&request=DescribeFeatureType&typename=geosolutions:ccounties

   Ce qui nous donne des informations sur les noms des champs et les types ainsi que le type de géométrie, dans ce cas ``MultiPolygon``.

   .. figure:: img/get6.png

      DescribeFeatureType response for Counties feature type


#. Après cela, nous pouvons émettre une requête de base ``GetFeature``, qui ressemble à ceci::

    http://localhost:8083/geoserver/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=geosolutions:ccounties&featureId=Counties.1

   .. note:: Notez qu'il est presque le même que celui qui Geoserver généré, mais c'est requestin une seule caractéristique précisant son identifiant via ``featureId=Counties.1``

   Dans la section :ref:`next <geoserver.vector_data.filter>` nous allons voir comment filtrer la sortie du WFS en fonction de divers attributss.
