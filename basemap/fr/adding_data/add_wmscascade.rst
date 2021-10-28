.. module:: geoserver.add_wmscascade
   :synopsis: Découvrez comment ajouter un layer WMS Cascade.

.. _geoserver.add_wmscascade:

Ajout d'un WMS Cascade Layer
-----------------------------

Une des nouvelles fonctionnalités de la série GeoServer 2.1 est WMS cascade.
Pour ceux qui ne sont pas familier avec il, cascade permet d'exposer les layers provenant d'autres serveurs WMS comme s'ils étaient des layers locales. Celle-ci prévoit des avantages intéressants:

 * Les clients se connectent à votre SDI doivent se préoccuper moins de points d'origine, qui pourrait être important pour les réseaux de haute sécurité
 * Il est désormais possible de demander des cartes dans des formats non supportés par le serveur d'origine, ou à reproject les cartes de projections non pris en charge par le serveur d'origine (GeoServer soutient presque 5000 différents systèmes de coordonnées de référence)
 * Il est maintenant possible de mélanger les couches avec celles locales pour générer formats orientés d'impression tels que PDF
 * Il est maintenant possible de fournir plus d'informations sur le layer, comme une meilleure description, plus de mots-clés, ce qui profitera à tous les clients, en particulier les catalogues de récolte d'informations à partir de votre document capabilites

**Configuration**

La configuration de l'utilisation des layers en cascade suit GeoServer facilité d'utilisation traditionnelle.

#. Ouvrez le navigateur Web et accédez à la GeoServer `Welcome Page <http://localhost:8083/geoserver>`_.

#. sélectionner :guilabel:`Add stores` from the interface. 

   .. figure:: img/geotiff_addstores.png

#. sélectionner :guilabel:`WMS - Cascades a remote Web Map Service` from the set of available Other Data Sources. 

   .. figure:: img/wmscascade_sources.png
   

#. Spécifiez un nom (pour exemple, :file:`geoserver-enterprise`) dans le :guilabel:`Data Source Name` domaine de l'interface. 
#. Spécifiez :file:`http://demo1.geo-solutions.it/geoserver-enterprise/ows?service=wms&version=1.1.1&request=GetCapabilities` comme le URL des echantillons dee donnèes dans le domaine :guilabel:`Capabilities URL`. 

   .. figure:: img/wmscascade_store.png

#. Cliquez :guilabel:`Save`. 

#. Publier le layer en cliquant sur ​​le lien :guilabel:`publish` à côté de nome de layer `GeoSolutions:ne_shaded`. Notez que vous pouvez également ajouter des layers plus tard.

   .. figure:: img/wmscascading_publish.png

#. Vérifiez les systèmes de référence de coordonnées et les domaines Bounding Boxes fields sont correctement réglés et cliquez sur :guilabel:`Save`. 

   .. figure:: img/wmscascade_bbox.png

#. A ce stade, le nouveau WMS layer est publié avec GeoServer. Vous pouvez utiliser le layer preview pour vérifier les données-

   .. figure:: img/wmscascading_preview.png
