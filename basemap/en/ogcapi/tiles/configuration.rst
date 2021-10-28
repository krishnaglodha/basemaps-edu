.. module:: geoserver.ogcapi.tiles.config
   :synopsis: Configuring tiled layers before getting involved in tiles service.

.. _geoserver.ogcapi.tiles.config:

Tiled Layers configuration
==========================

A couple of tiled layers need to be configured in GeoServer, before getting involved with the OGC-API Tiles. 

Raster layer
------------

Once logged on GeoServer GUI as an admin, click on ``Tile Layers`` link from the Tile Caching section on the left menu.

 .. figure:: img/tile-layers.png


Then, click on ``Add a new cached layer`` link 

 .. figure:: img/add-layers.png

From the list of selectable layers to be cached, check the box on the ``nurc:Img_Sample`` layer and then click on ``Configure selected layers with caching defaults`` link.

 .. figure:: img/add-cached-layer.png

Click ``Ok`` on the confirmation message and then go back to the ``Tile Layers`` link from the Tile Caching section on the left menu.

 .. figure:: img/tile-layers.png

Click on ``nurc:Img_Sample`` from the list of available tiled layers to configure it.

 .. figure:: img/tile-layer-config.png


From the layer configuration page, scroll at the very bottom of the page and add a grid subset, by selecting 
``WebMercatorQuad`` from the drop down list and click on the ``+`` icon when done, to add it.

 .. figure:: img/gridset-selection.png


Finally, click on ``Save`` and the cached layer will been configured.

Vector layer
------------

Repeat previous steps to configure a vector layer, this time selecting ``topp:states`` as tiled layer.

During the layer configuration, make sure to also select the ``application/json;type=geojson`` and ``application/vnd.mapbox-vector-tile`` from the ``Tile Image Formats`` list.

 .. figure:: img/tile-vector-format.png


Also make sure that ``ALL STYLES`` is checked in ``Alternate Styles`` area of the ``STYLES`` section.

 .. figure:: img/vector-styles.png

