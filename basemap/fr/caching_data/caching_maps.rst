.. module:: geoserver.cahing_maps
   :synopsis: Apprendre comment mettre en mémoire les cartes avec GeoWebCache.

.. _geoserver.cahing_maps:

GeoWebCache Integration
=======================

La page GeoWebCache Settings dans le Serveur menu dans le :ref:`geoserver.wai` montre quelques options de configuration  pour GeoWebCache, un  tile server qui est inclus par défaut à l'intérieur de GeoServer.

#. Naviguez jusq'à GeoServer `Welcome Page <http://localhost:8083/geoserver/web/>`_.

#. Cliquez sur le lien :guilabel:`GeoWebCache` qui se trouve dans la séction Settings.

   .. figure:: img/gwcsettings1.png
      :align: center

#. Suivez dans la page :guilabel:`GeoWebCache Settings` .

   .. figure:: img/gwcsettings2.png
      :align: center

   .. figure:: img/gwcsettings21.png
      :align: center
   

Validez directement l'integration WMS 
--------------------------------------

GeoWebCache agit comme un mandataire (proxy) entre GeoServer et la carte client. GeoWebCache a par défaut une extremité separée du GeoServer WMS.

Cependant très souvent les clients sont décidés à faire des demandes carrelées au GeoServer WMS, et il arrive souvent que ces carreaux  correspondent une hiérarchie bien connue (la Google/OpenStreetMap par example).Abiliter l'intégration directe du WMS permet aux requetes de WMS qui correspondent une structure reconnue par GWC  d'étre mises en antémémoire où elles sons servies directement par GWC, de façon qu'elles soyent accélérés. 

Option pour attraper pad défaut 
----------------------------------

L'option pour attraper pad défaut influence
The default caching options influence comment GeoWebCache cache les couches configurées dans GeoServer.
Les séries appliées par GeoServer 2.1.x à toutes les couches, dans la série 2.2.x peuvent aussi etre personnalisées par couches. 

Le facteur méta carrelage influence le type de requete WMS est utilisée par GWC pour construire la cache des carreaux ( il s'agit par défaut  d'un blocque de 4x4 qui vient après tranché). La gouttière ajoute un numéro extra de colonnes de pixels dans la requete de Wms pour etre sur que le carreau ne soit coupé ou incomplet (dans GeoServer cela n'est pas nécessair, 
mais il peut dévenir critique si la mésure des symboles est commandée par les attributes, et cela ne peut pas etre determiné en vérifiant seulement le SLD). 

Les formats qui sont conservés dans la dans la mémoire-cache influencent ce que le GWC va effectivement mettre en mémoire sur le disque et le serveur.

La quota du disque
--------------------

Cette séction s'occupe de l'utilisation du disque pour les carreaux sauvés avec GeoWebCache. 

Par défaut l'utilisation du disque par GeoWebCache est illimité, sans aucun régard pour l'intégration avec le GeoServer WMS, donc chaque carreau servi par GeoWebCache serà mémorisé dans le repertoire de la cache (typiquement le répertoire :file:`gwc` dans le repertoire des donnés). Arranger une quota de disque permet de forcer l'utilisation du disque, cela pourrait etre critique quand on sert des grands secteurs résultant en plusieurs terabytes de cache de tuiles (tile cache) sur le disque.

.. list-table::
   :widths: 30 15 55
   :header-rows: 1

   * - Option
     - Default value
     - Description
   * - :guilabel:`Abiliter les limites de Quota du Disque`
     - Off
     - Démarre le quota du disque. Quand il est hors de sérvice, le repertoire de la cache croitra sens limites. Quand il est abilité le repertoire de la cache serà accordé aux options en dessous.
   * - :guilabel:`Usage de la cache de l'ordinateur basé sur un bloque du disque mesure de`
     - 4096 bytes
     - Ce domain doit etre mis de égal à la mésure du blocque du disque de mémoire où la cache est située.
   * - :guilabel:`Controle si le quota du disque est excedé chaque`
     - 10 secondes
     - L'interval de temps dans lequel la cache est interrogée. Des valeurs plus pétits (interrogations plus frèquentes) avec une petite augmentation de l'activité du disque, des valeurs plus grands (interrogations moins frèquentes) peuvent causer que le quota du disque soit temporairement excedée.
   * - :guilabel:`Disposer la taille maxime du cache`
     - 100 MiB (Mebibytes)
     - La taille maxime de la cache.  Quand cette valeur est excedé et on interroge la cache les toiles seront enlevés selon l'orientation choisie en dessous. Notez que les options de l'unité sont les **mebibytes** (approx. 1.05MB), **gibibytes** (approx. 1.07GB), et **tebibytes** (approx. 1.10TB).
   * - :guilabel:`En forçant les lymites de quota du disque enlevez avant tout`
     - les moins utilisés
     - Règle les pricipes pour la suppression des toiles quand le quota du disque est excédée. Les options sont ** Options are **Least Frequently Used** (removes tiles based on how often the tile was accessed) or **Least Recently Used** (removes tiles based on date of last access).

.. note:: It is not currently possible to set a disk quota for the entire GeoWebCache storage system. It is also not possible to mix LFU and LRU on a single layer. See the `GeoWebCache documentation <http://geowebcache.org/docs>`_ for more about disk quotas.

When finished making changes, click :guilabel:`Submit`.

This section also shows how much disk space is being used compared to the disk quota size, as well as the last time (if any) the quota was reached.

Links
-----

On top this page contains links to:

 * The embedded GWC homepage (containing runtime statistics and status updates).

   .. figure:: img/gwc1.jpg
      :align: center

 * The GWC demo page where you can view configured layers, reload the configuration (when changing settings or adding new layers), and seed/refresh the existing cache on a per-layer basis.

   .. figure:: img/gwc2.jpg
      :align: center
   
#. On top of :guilabel:`GeoWebCache Settings` page click on :guilabel:`Go to the GWC Demos Page` link.

   .. figure:: img/gwc3.png
      :align: center
    
#. In the :guilabel:`GeoWebCache Demo` page scroll down to visualize the :guilabel:`boulder` layer clicking on :guilabel:`png` link

   .. figure:: img/gwc4.png
      :align: center
    
#. :guilabel:`Zoom In` on the map and after :guilabel:`zoom out` in order to check the responsively of the layer rendering.

   .. figure:: img/gwc5.png
      :align: center

Viewing
-------

To view the GeoWebCache demo page, append ``/gwc/demo`` to the address of your GeoServer instance.  For example, if your GeoServer is at the following address::

   http://localhost:8083/geoserver
   
The GeoWebCache demo page is accessible here::

   http://localhost:8083/geoserver/gwc/demo

GeoWebCache endpoint URL
------------------------

When not using direct integration, you can point your client directly to GeoWebCache.

.. warning:: GeoWebCache is not a true WMS, and so the following is an oversimplification. 

To direct your client to GeoWebCache (and thus receive cached tiles) you need to change the WMS URL.

If your application requests WMS tiles from GeoServer at this URL::

   http://example.com/geoserver/wms

You can invoke the GeoWebCache WMS instead at this URL::

   http://example.com/geoserver/gwc/service/wms
   
In other words, add ``/gwc/service/wms`` in between the path to your GeoServer instance and the WMS call.

As soon as tiles are requested through GeoWebCache, GeoWebCache automatically starts saving them.  This means that initial requests for tiles will not be accelerated since GeoServer will still need to generate the tiles.  To automate this process of requesting tiles, you can **seed** the cache.

