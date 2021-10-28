.. module:: geoserver.regionation

.. _geoserver.regionation:


KML Regionation
---------------

Super-overlays are a form of KML in which data is broken up into a **pyramid of regions** (tiles) and served according to the current scale. 

This allows Google Earth to refresh/request **only particular regions** of the map when the view area changes, and avoid Google Earth from slowing down or crashing due to data overload.

Super-overlays are used to efficiently publish large sets of data. GeoServer's KML output options allow to specify the criteria used to place data in the various pyramid zoom levels.

.. note::

   The most important aspect of regionation is to decide how to determine which features show up more prominently than others. This can be done either by geometry, or by attribute. One should choose the option that best exemplifies the relative "importance" of the feature. When choosing to regionate by geometry, only the larger lines and polygons will be displayed at higher zoom levels, with smaller ones being displayed when zooming in. When regionating by an attribute, the higher value of this attribute will make those features show up at higher zoom levels. 

#. GeoServer allows a set of **Regionation strategies** to determine which features should be shown at any given time or zoom level:

   .. list-table::
      :widths: 20 80
   
      * - **Strategy**
        - **Description**
      * - ``best_guess``
        - (*default*) The actual strategy is determined by the type of data being operated on. If the data consists of points, the ``random`` strategy is used. If the data consists of lines or polygons, the ``geometry`` strategy is used.
      * - ``external-sorting`` 
        - Creates a temporary auxiliary database within GeoServer.  It takes slightly extra time to build the index upon first request.
      * - ``native-sorting`` 
        - Uses the default sorting algorithm of the backend where the data is hosted. It is faster than external-sorting, but will only work with a store backed by DBMS (PostGIS, Oracle, ...) and requires the column in question to be indexed to get good performance.
      * - ``geometry``
        - Externally sorts by length (if lines) or area (if polygons).
      * - ``random``
        - Uses the existing order of the data, does not perform any sorting.
 
#. To set a different **Regionation startegy** go to the ``Welcome page`` and click the ``Layers`` link on the menu ``Data section``. 

#. Edit an existing layer clicking on its ``Layer Name`` link (for example ``states``).

   .. figure:: img/regionation1.png
   
      The Layers page

   .. figure:: img/regionation2.png
   
      The geosolutions:states layer editor

#. Click on ``Publishing`` tab and scroll to the bottom to ``KML Format Setting`` section. Now select your ``Attribute`` and ``Method`` from the ``Regionation`` combo box and click the :guilabel:`Save` button.
 
   .. figure:: img/regionation3.png
   
      The Regionation settings 

.. note:: By **default** the ``best_guess`` strategy in used.
