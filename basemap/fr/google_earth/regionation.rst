.. module:: geoserver.regionation

.. _geoserver.regionation:


KML Regionation
---------------

Super-overlays sont une forme de KML dans laquelle les données sont divisés en une pyramide des régions (tuiles) et servi selon le barème actuel. Cela permet à Google Earth de actualiser / demander seulement certaines régions de la carte  lorsque la zone de vue change, et d'éviter GE de ralentir ou de s'écraser en raison de la surcharge de données.
Super-overlays sont utilisés pour publier efficacement de grands ensembles de données. Options de sortie KML de GeoServer permettent de spécifier les critères utilisés pour placer les données dans les différents niveaux de zoom de la pyramide.

.. note::

   L'aspect le plus important de regionation est de décider comment déterminer quelles caractéristiques présentent une place plus importante que d'autres. Cela peut se faire soit par la géométrie, ou par attribut. Il faut choisir l'option qui incarne le mieux le «importance» relative de l'entité. En choisissant de raisonner par la géométrie, seules les lignes et les polygones plus grands seront affichés au niveau de zoom élevé, au meme temps les petites seront affichés  lorsque le zoom. En raisonnant par un attribut, la valeur supérieure de cet attribut rendra ces caractéristiques présentent au plus haut niveaux de zoom.
#. GeoServer permette un ensemble d' **Regionation strategies** pour déterminer les caractéristiques doivent être affichés à un moment donné ou un niveau de zoom:

   .. list-table::
      :widths: 20 80
   
      * - **Strategy**
        - **Description**
      * - ``best_guess``
        - (*default*) La stratégie réelle est déterminée par le type de données étant opérés. Si les données sont constituées de points, la stratégie  ``random`` est utilisée. Si les données se compose de lignes ou des polygones, la stratégie ``geometry`` est utilisée
        - Crée une base de données auxiliaire temporaire au sein de GeoServer. Il faut du temps un peu plus pour construire l'index lors de la première demande.
      * - ``native-sorting`` 
        - algorithme de tri par défaut du backend où les données sont hébergées. Il est plus rapide que le tri externe, mais ne fonctionnera qu'avec un magasin soutenue par SGBD (PostGIS, Oracle, ...) et nécessite la colonne en question soit indexé à obtenir de bonnes performances.
      * - ``geometry``
        - Sortes externe par longueur (silignes) ou zone (si polygones).
      * - ``random``
        - Utilise l'ordre existant des données, ne pas effectuer de tri.
 
#. Pour définir un autre ``Regionation startegy`` aller à page :guilabel:`Welcome` et cliquez le lien :guilabel:`Layers` sur la section de données :guilabel:`Menu` Data section. 

#. Modifier une couche existante en cliquant sur le lien ``Layer Name`` (par example :guilabel:`states`).

   .. figure:: img/regionation1.png
      :width: 650
   
      La page des calques

   .. figure:: img/regionation2.png
   
      Les géosolutions: éditeur de couche - états

#. Cliquez sur le tab ``Publishing``  et faites défiler vers le bas pour la section ``KML Format Setting``. Maintenant, sélectionnez votre ``Attribute`` et``Method`` du combo box ``Regionation`` et cliquez le buton :guilabel:`Save`.

   .. figure:: img/regionation3.png
   
      Les paramètres de Regionation 

.. note:: par **défaut,** est utlisé la strategie``best_guess``.
