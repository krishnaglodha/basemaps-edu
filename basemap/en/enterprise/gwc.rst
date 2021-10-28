

.. _geoserver.jmeter_gwc:

Tile Caching with GeoWebCache
======================================
	.. hint:: You should read this section right after having read the :ref:`gwc.cahing_data` section.

In this section we are going to capture some additional recommendations on how to best tune GeoWebCache with special focus on when it's integrated with GeoServer.

First of all, using a tile oriented cache like GeoWebCache can greatly enhance throughput at runtime compared to pure WMS of 10 to 100 times, assuming tiles are already cached (whole layer pre-seeded), therefore when looking for performance (speed and throughput) tile caching is an obvious candidate for enhancement. It is, however, important to understand the limitations of tile oriented protocols:

 * Tile oriented maps means having clients working at fixed zoom levels and using a fixed grid as well as, potentially, a small set of coordinate reference systems and styles
 * Protocols supported are peculiar: WMTS, TMS, WMS-C, Google Maps/Earth, Virtual Earth

As usual the speed up you get is payed by the restrictions on the degrees of freedom that the protocols you expose offer the target clients.
As a rule of thumb tile caching, these hints are worth to keep in mind:

1. Tile caching is first class option for background layers.
2. Tile caching is always useful for static layers, i.e. layers that are not update often (once a week? once a month? once in a lifetime?)
3. Tile caching is always useful for simple layers, i.e. layers with no/few dynamic parameters (CQL filters, SLD params, SQL query params, time/elevation, format options)

Generally speaking, tile caching should be part of the plan for layers where:
 * Data doesn't change frequently and/or where we can introduce a slight delay between the changes in the data and the changes in the cached content (e.g. to group changes in batches)
 * The rendering options are limited. Layers with complex filtering, many styling, multiple dimensions might become a nightmare for the cache since a cache is as useful once if it gets reused a lot between different requests. If chances that two different requests would hit the cache are low then you should not be caching.

  .. note:: Caching is usually expensive so you should use it wisely. Caching involves writing to disk, keeping track of the age of the files as well as of the size of the cache and so on: it needs resources, therefore we need to make sure its impact is positive, everything considered.


Integrated GeoWebCache
-------------------------
GeoWebCache can be used either standalone or integrated with GeoServer. GeoServer ships by default with GeoWebCache deployed into it but the `direct integration <http://docs.geoserver.org/stable/en/user/geowebcache/using.html#direct-integration-with-geoserver-wms>`_  is not activated by default and needs to be activated as shown below from the *Caching Default* subsection under the *Tile Caching* section.

	.. figure:: img/gwc_embed.png
		:align: center

		*Create a new Gridset*

Once the direct integration is activated and a client sends a WMS request to GeoServer that is cacheable according to the GeoWebCache setup (i.e. request aligned to the gridset, proper style, proper params, etc..) GeoServer will pass this request to GeoWebCache for serving a cached tile **if and only if** the client has added the **TILED=TRUE** key-value-pair to the request.

Ok, but why using the integrated GeoWebCache is interesting? Well, it is simple, there is no double encoding when using meta-tiling which means faster cache creation times; this happens because, as shown below, the integrated GeoWebCache taps directly into the GeoServer rendering chain and there is less encodings before writing to disk. Encoding can be the most expensive step for most use cases.

	.. figure:: img/gwc1.png
		:align: center

		*Create a new Gridset*

Space considerations
---------------------
As mentioned above, caching comes with a cost, actually more than one. One cost factor to take into consideration when designing a tile cache is the space we need for caching the data we want to cache, or at least the maximum amount of space that the cache we are setting up might take.

We used such a distinction since most part of the time we won't seed entirely the cache for a layer upfront but we will simply cache the landing areas and zoom levels and we will leave the cache grow lazily at runtime (this is a cost factor which is often forgotten).
Anyway, if you need help in estimating how much space a cache would need we have set up a spreadsheet which you can use to perform space oocupation estimates, you can find it `here <https://docs.google.com/spreadsheets/d/1t9OJb-Z9vnHYKw6fWR82Wml4Dj9ePnNoOzqDw3v_PwU/edit#gid=0.>`_

The space required for the cache, especially on layers with many parameters combinations (CRS or gridset, Style, CQL_FILTER and so on) can grow exponential since for any combination of the available parameters we will have to build a different cache.
Assuming you have taken this into account and you are still willing to use a cache, you don't need to make sure you have the whole space available upfront but you can set up disk quota and have GeoServer make sure it will not use too much space (see the relative section in this material).


Client Side Caching
--------------------
When we talk about GeoWebCache we are usually considering only server side caching, i.e. render a tile, save it on disk and send it every time a client requests it. Well there is more and that is client-side caching of tiles.
Once the tile has been cached on the server and tranfered to the client there is no point for the client in asking that specific tile once again unless its content has changed.

	.. note:: Client-Side Caching does not work with browsers in private mode.

To send Caching-Headers to the requesting client one could set the expiration header value zoom dependent in the geowebcache.xml::

	<expireClientsList>
		<expirationRule minZoom="0" expiration="7200" />
		<expirationRule minZoom="10" expiration="600" />
	</expireClientsList>

and here is what you should expect to see if you inspect HTTP header for a request in a browser:

	.. figure:: img/gwc3.png
		:align: center

		*HTTP Headers with client side caching at work*

In case you were wondering you can set this option for the integrated GeoWebCache on a per layer basis using the tile caching tab as shown below.

	.. figure:: img/gwc4.png
		:align: center

		*Client side caching settings in the GeoServer GUI*

More Tweaks
-------------
It is important to use the right formats for the cache:

 * JPEG for background raster data (e.g. orthos) since you won't need transparency
 * PNG8 + precomputed palette for background vector data (e.g. osm)
 * PNG8 full for overlays with transparency
 * The format impacts also the disk space needed! (as well as the generation time) so evaluate it carefully

For a detailed list of things to look at for tweaking GeoWebCache (most of them applies to the standalone version but it is worth knowing them anyway), you can refer to `this <http://www.geo-solutions.it/blog/tips-tricks-geowebcache-tweaks-checklist/>`_ blog post.


Testing GeoWebCache fullWMS support with JMeter
------------------------------------------------

The following section compares GeoServer WMS with GeoWebCache ``fullWMS`` support. ``FullWMS`` is a new feature which allows GeoWebCache to act as a WMS endpoint, like GeoServer.
Using GeoWebCache, the server is able to cache the requested tiles in order to return them faster then GeoServer.

This example will show how to configure GeoWebCache with fullWMS support and how the performances are improved.


Configuring GeoServer/GeoWebCache
---------------------------------

#. On your Web browser, navigate to the GeoServer `Welcome Page <http://localhost:8083/geoserver/>`_.

#. Go to :guilabel:`Gridsets` and click on ``Create a new gridest``

#. Call it ``EPSG_2876`` and set ``EPSG:2876`` as :guilabel:`Coordinate Reference System`

#. Click on :guilabel:`Compute from maximum extent of CRS` and add 15 new **Zoom Levels** (from 0 to 14) by clicking on :guilabel:`Add zoom level`. It should look like this picture:

	.. figure:: img/jmeter41.png
		:align: center

		*Create a new Gridset*

#. Click on :guilabel:`Save`. Now this `GridSet` can be added to the Layer Group ``boulder`` for caching it with GeoWebCache

#. Go to :guilabel:`Layer Groups` and click on ``boulder``

#. On the :guilabel:`Available gridsets` panel add the gridset ``EPSG_2876`` from the list. Then click on :guilabel:`Save`.

	.. figure:: img/jmeter42.png
		:align: center

		*Add the new Gridset*

	.. note:: Remember to set :guilabel:`Published zoom levels` to ``Min`` and ``Max``

Configuring JMeter
------------------

#. Go to ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data``  if you are on Windows ) and copy the file ``template.jmx`` file into ``gwc.jmx``

#. From the training root, on the command line, run ``jmeter.bat`` (or ``jmeter.sh`` if you're on Linux) to start JMeter

#. On the top left go to :guilabel:`File --> Open` and search for the new *jmx* file copied

#. Disable all the **Thread Groups** except for **8**

#. Disable the **Content-Type Check**

#. In the ``CSV Data Set Config`` element, modify the **path** of the CSV file by setting the path for the file ``gwc.csv`` in the ``$TRAINING_ROOT/data/jmeter_data`` ( or ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) directory

#. In the **HTTP Request Default** element modify the following parameters:

	.. list-table::
		  :widths: 30 50

		  * - **Name**
		    - **Value**
		  * - layers
		    - boulder
		  * - srs
		    - EPSG:2876

Test GeoServer WMS
------------------

#. Run the test

	.. note:: Remember to run and stop the test a few times for having stable results

#. When the test is completed, Save the results in a text file.

#. Remove the result from JMeter by clicking on :guilabel:`Run --> Clear All` on the menu

#. Stop GeoServer

Test GeoWebCache fullWMS
------------------------

#. Go to ``$TRAINING_ROOT/geoserver_data/gwc/geowebcache.xml`` ( ``%TRAINING_ROOT%\geoserver_data\gwc\geowebcache.xml`` on Windows ) and add the following snippet:

	.. code-block:: xml

		<gwcConfiguration>

		  ...

		  <fullWMS>true</fullWMS>
		</gwcConfiguration>

	Setting ``fullWMS`` to `true` allows GeoWebCache to use fullWMS support

#. Restart GeoServer

#. On the JMeter **HTTP Request Default** panel, change the *Path* from ``geoserver/ows`` to ``geoserver/gwc/service/wms`` in order to execute WMS requests directly to GeoWebCache, without passing from GeoServer

#. Add a new parameter called **hints** which can have 3 values ``speed``, ``default`` and ``quality``. The first one should be used for having a faster response without concerning about image quality; the last one, instead, is slower but with a better quality; the second one is a good trade off between them. For the first test set **hints** to ``speed``.

#. Run the test

	.. note:: At the first run, the throughput should be lower than that of GeoServer, because GeoWebCache has spent much time on generating the cached tiles.

#. Remove the result from JMeter by clicking on :guilabel:`Run --> Clear All` on the menu

#. Run the same test again.

	Now the throughput should be improved, because GeoWebCache have already generated the tiles to cache and can reuse them. Image quality should be very poor because of the ``hints=speed`` configuration.

	.. figure:: img/jmeter43.png
		:align: center

		*Result from GeoWebCache fullWMS with hints=speed*

#. Run the same test with ``hints=default``

	.. figure:: img/jmeter44.png
		:align: center

		*Result from GeoWebCache fullWMS with hints=default*

#. Run the same test with ``hints=quality``

	.. figure:: img/jmeter45.png
		:align: center

		*Result from GeoWebCache fullWMS with hints=quality*

	It should be noted that changing the ``hints`` parameter changes the image quality, but the throughput is always greater than that of GeoServer WMS
