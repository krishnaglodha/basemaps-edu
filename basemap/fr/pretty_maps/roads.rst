.. _geoserver.roads:

Routes et étiquetter les routes
---------------------------------

#. De la `Welcome Page <http://localhost:8083/geoserver>`_ naviguez à :menuselection:`Styles --> mainrd` pour modifier le mainrd SLD.

   .. note:: Vous devez être connecté comme Administrator pour activer cette function.

#. Dans le :guilabel:`SLD Editor` retrouvez le :guilabel:`sld:TextSymbolizer` associé au :guilabel:`ogc:PropertyName` *LABEL_NAME*

   .. figure:: img/sld_create9.jpg
      :width: 600
 		  
      Road style

   .. note:: Le style definit un ``<Font>`` et un ``<Halo>`` pour rendre la valeur de la proprieté *LABEL_NAME* pour la couche. La partie la plus intéressantese se trouve au fond où plusieurs sont spécifiés ``<VendorOption>``. Ces options sont spécifiques de GeoServer et nous permettent d'avoir de meilleurs et plus agréables résultats tweacking le comportement de l'étiquette.

.. list-table::
   :widths: 10 80 10

   * - **Option**
     - **Description**
     - **Type**
   * - **followLine**
     - L'option suivante force une étiquette à suivre la curve de la ligne.
	 
       .. code-block:: xml 
	   
         <VendorOption name="followLine">true</VendorOption>

       Pour utiliser cette option placez ce qui suit dans votre ``<TextSymbolizer>``. Il est nécessaire de utiliser ``<LinePlacement>`` avec cette option pour faire en sorte que toutes étiquettes suivent correctement les lignes suivantes:
	   
       .. code-block:: xml 
	   
         <LabelPlacement>
           <LinePlacement/>
         </LabelPlacement>

     - boolean
   * - **repeat**
     - L'option repeat determine combien de fois GeoServer étiquette une ligne. Normalement GeoServer étiquetterait chaque ligne seulement une fois, indépendamment de leur longueur. Specifier une valeur positive pour le faire marquer une étiquette pour chaque pixel repeté.
	 
       .. code-block:: xml 
	   
         <VendorOption name="repeat">100</VendorOption>

     - number
   * - **group**
     - Parfois vous aurez un ensemble de fonctions liées pour lesquelles vous voulez une seule étiquette. Les options de groupement de toutes les fonctions ayant la meme étiquette de texte, trouve alors une géométrie representative pour le groupe.

       Le donné des routes en est un example - nous voulons une seule étiquette pour toutes les ``main street``, pas une étiquette pour chaque ``main street``.
	 
       .. figure:: img/group_not.gif 
	   
       Lorsque l'option de regroupement est éteint (défaut), le groupement n'est pas effectuée et chaque géométrie est étiquettée (si l'espace le permet).

       .. figure:: img/group_yes.gif 

       Avec l'option de regroupement fonctionnante, toutes les géométries avec la meme étiquette sont groupées ensemble et l'étiquette de la position est détérminée par TOUTES les géométries.

      

         
          *  **Point Set**
             le premier point à l'intérieur du rectangle de vue est utilisé.
          *  **Line Set**
             les lignes sont (a) reliés en réseau (b) attachées au rectangle de vue (c) le centre du chemin de réseau le plus long est utilisé.
          * **Polygon Set**
            les polygones sont (a) attachées au rectangle de vue (b) le barycentre du polygone le plus grand est utilisé.

       .. code-block:: xml 
	   
         <VendorOption name="group">yes</VendorOption>

       .. Attention:: Vous pouvez grouper ensemble deux ensembles de fonctionnalités par accident. Par example, vous pouvez créér un seul groupe pour ``Paris`` qui contient caractéristiques pour Paris (France) et Paris (Texas). 

     - enum{yes/no}
   * - **maxDisplacement**
     - L'option maxDisplacement controle le déplacement de l'étiquette le long d'une ligne. Normalement GeoServer n'étiquetterait une ligne que dans son point milieu, à condition que la place ne soit occupée par une autre étiquette, où elle ne soit autrement étiquettée. Une fois fixé, l'étiqueteuse chercerà une autre place dans les pixels maxDisplacement à partir du point d'étiquette précalculée.

       Lorsqu'il est utilisé en conjonction avec répétition, la valeur de maxDisplacement doit toujours etre plus baisse de la valeur de répétition.
	 
       .. code-block:: xml 
	   
         <VendorOption name="maxDisplacement">10</VendorOption>

     - number

Une autre chose importante à noter dans ce style est le **road casing**, qui est, le facte que chaque segment de route est peint par deux traits superposés de couleure et taille différentes.

Placer les courses dans les deux type de styles fonctionnel distinct est crucial:

  * avec les symboliseurs en deux elements FeatureTypeStyle distincts toutes les routes sont peintes avec un grand trait, et puis une autre fois avec un plus légèr.
  * si, au lieu de ça les deux symbolizeurs étaient mises in place dans le meme élément FeatureTypeStyle le resultat aurait été différent, et pas agréable à voire, car le rendu aurait du prendre la première route, la séquence painte avec les grands et les petits traits dedan, 
  puis bouger jusq'à à la suivante en rèpetant jusq'à la fin.

  .. figure:: img/nofts.png
	   
     Encadrement des routes avec un seul FeatureTypeStyle élément
