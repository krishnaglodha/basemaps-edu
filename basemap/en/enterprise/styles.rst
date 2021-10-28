

.. _geoserver.jmeter_styles:


Style Optimizations
=================================================

The following section explains how GeoServer performances are improved when using optimized styles since styling is an important feature but requires some  attention in order to avoid slowing down the performances.
This tutorial aims at showing how GeoServer performances change by choosing a different style for the same data set using JMeter.

.. note:: This example requires to have already completed :ref:`Adding a Shapefile <geoserver.add_shp>`, :ref:`Adding a Style <geoserver.add_style>` and the first 9 steps of the :ref:`Creating a Base Map with a Layer Group <geoserver.base_map>` section.

Use scale dependencies
-----------------------
One of the key tricks with styling data is to delay the display of detailed layers at higher zoom levels by filtering data based on attributes to display only the **important** features first.

If you show too much data the map will not be readable, but rather a graphic blob. Rule of thumb: never show more than 1000 features (records) max in the display. A few additional hints:

 * Have labels show up only when zoomed in
 * Show details as you zoom in
 * Eagerly add MinScaleDenominator to your SLD rules
 * Add more expensive rendering when there are less features, this is key to get both a good looking and fast map

In the picture below we can see various applications of scale dependencies. Detailed layers, such as streets and buildings, show up only when fairly zoomed in. The DEM is hidden when we get very close to the city to avoid getting the representation too «busy», visually speaking. The streets change from simple line to cased line.

	.. figure:: img/styling1.png
		:align: center

		*Proper usage of scale dependencies*

Labeling
----------
Labels are one of the most important elements in creating good looking maps. Here below some recommendations:

 * Labeling conflict resolution is expensive, limit to the most inner zooms
 * Halo is important for readability, but adds significant overhead
 * Be careful with maxDisplacement, makes for various label location attempts


	.. figure:: img/styling2.png
		:align: center

		*Proper labels at work*

The Concept of FeatureTypeStyle
--------------------------------------
GeoServer uses SLD FeatureTypeStyle objects as Z layers for painting.  Each one allocates its own rendering surface (which can use a lot of memory), hence the recommendation here is to use as few of them as possible.

	.. figure:: img/styling3.png
		:align: center

		*FeatureTypeStyle at work*

Use translucency sparingly
---------------------------
Translucent display is expensive, use it sparingly (e.g. translucent fill ``<CssParameter name="fill-opacity">0.5</CssParameter>``).

Delay the display of detailed layers at higher zoom levels, filter based on attributes to display only the *important* features first.

Ok now let's dive into benchmarking various styling options.

Configuring GeoServer
-----------------------

#. On your Web browser, navigate to the GeoServer `Welcome Page <http://localhost:8083/geoserver/>`_.

#. Go to :guilabel:`Styles` and click on ``Add new Style``

#. On the bottom of the page, click on :guilabel:`Choose File` and select the SLD file called ``line_label`` in the ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` if you are on Windows )  directory

#. Click on :guilabel:`Upload` and then on :guilabel:`Submit`. Now we have a style which supports labeling but has no control on the label conflicts and overlapping

#. Return to the GeoServer `Welcome Page <http://localhost:8083/geoserver/>`_.

#. Go to :guilabel:`Layer Groups` and click on ``test``

#. Add a new Layer to the Layer Group called **bbuildings**

	.. figure:: img/jmeter36.png
		:align: center

		*Add a new Layer to the Layer Group*

#. Change the associated styles by clicking on each style and choosing another one on the list. Use the following styles:

	.. list-table::
		  :widths: 30 50

		  * - **Layer**
		    - **Style**
		  * - geosolutions:Mainrd
		    - line_label
		  * - geosolutions:BoulderCityLimits
		    - polygon
		  * - geosolutions:bplandmarks
		    - polygon
		  * - geosolutions:bbuildings
		    - polygon

	.. figure:: img/jmeter37.png
		:align: center

		*Styles configuration*

#. Click on :guilabel:`Save`. With this configuration we have a Layer Group composed by 4 Layers with 4 bad styles associated. This will result in a low throughput, if compared to that of the test with optimized styels.

Configuring JMeter
------------------

#. Go to ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) and copy the file ``template.jmx`` file and create a ``styles.jmx`` one

#. From the training root, on the command line, run ``jmeter.bat`` ( or ``jmeter.sh`` if you're on Linux) to start JMeter

#. On the top left go to :guilabel:`File --> Open` and search for the new *jmx* file copied

#. Disable **Thread Group** **8**, **16**, **32** and **64**

#. In the active ``CSV Data Set Config`` elements, modify the **path** of the CSV file by setting the path for the file ``style.csv`` in the ``$TRAINING_ROOT/data/jmeter_data``  ( or ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) directory

#. In the **HTTP Request Default** element modify the following parameters:

	.. list-table::
		  :widths: 30 50

		  * - **Name**
		    - **Value**
		  * - layers
		    - test
		  * - srs
		    - EPSG:2876

Test with unoptimized styles
----------------------------

#. Run the test. You should see something like this:

	.. figure:: img/jmeter38.png
		:align: center

		*View Results Tree panel with a bad styling*

	.. note:: Remember to run and stop the test a few times for having stable results

#. When the test is completed, Save the results in a text file.

#. Remove the result from JMeter by clicking on :guilabel:`Run --> Clear All` on the menu

Setting optimized styles
--------------------------

#. Go to :guilabel:`Layer Groups` and click on ``test``

#. Change the associated styles by clicking on each style and choosing another one on the list. Use the following styles:

	.. list-table::
		  :widths: 30 50

		  * - **Layer**
		    - **Style**
		  * - geosolutions:Mainrd
		    - mainrd
		  * - geosolutions:BoulderCityLimits
		    - citylimits
		  * - geosolutions:bplandmarks
		    - arealandmarks
		  * - geosolutions:bbuildings
		    - buildings

	.. figure:: img/jmeter39.png
		:align: center

		*Styles configuration*

#. Click on :guilabel:`Save`. The new styles contain scale dependencies and label optimization, which will result in a better throughput.

Test with optimized styles
--------------------------

#. Run again the test.

	.. figure:: img/jmeter40.png
		:align: center

		*View Results Tree panel with good styling*

	You may see that the throughput is greater than that of the first test. The use of scale dependencies reduces the layers to see at lower zoom levels while conflict resolution avoids to show multiple overlapping label at each zoom level.
