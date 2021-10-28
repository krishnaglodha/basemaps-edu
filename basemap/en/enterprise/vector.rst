

.. _geoserver.jmeter_vector:


Vector Data Optimization
================================================

Vector data optimization checklikst
--------------------------------------
In order to have high performance and stable serving the list below captures what we do want from vector data:

 * Binary data, as usual storing data in a format which is not binary won't help (size, poor compression, no seek, etc. etc.)
 * No complex parsing of data structures: The more transformations we have to do during extraction (e.g. complex-feature) the slower serving will be.
 * Fast extraction of a geographic subset
 * Fast filtering on the most commonly used attributes, this is crucial for serving huge datasets with services like WMS and WFS where filtering is used extensively

Choosing a format
------------------
The first thing to evaluate when optimizing vector data is the native format used to store the data.
A quick and dirty classification of the formats is performed here below:

Slow formats
************
 * WFS
 * GML
 * DXF

Good formats, local and index-able
***********************************
 * Shapefile, directory of shapefiles; not good if you have heavy filtering on non spatial attributes
 * SDE
 * Spatial databases: PostGIS,  Oracle Spatial, DB2, SQL server, MySQL
 * SpatiaLite; although it cannot be shared safely in a clustered environment as multiple processes should not access the same SQLite file (especially on networked file systems).

DBMS Checklist
----------------
Well, it's pretty clear from the above lists that on average the way to go for storing vector data is using a spatial DBMS due to many factors but primarily its rich support for complex native filters. A quick checklist for properly configuring a spatial DBMS is presented below:

* Use connection pooling properly and consider using JNDI for sharing pools.
* Validate connections (with proper pooling), better doing that in background but never trust a connection coming from the pool.
* Table Clustering, you can usually cluster tables on most frequently used indexes
* Use Spatial Indexing
* Use Spatial Indexing
* Use Spatial Indexing
* Use Alphanumeric Indexing
* Use Alphanumeric Indexing
* Use Alphanumeric Indexing

	.. note:: Did we mention that indexes are important? Most part of the time performance problems are due to missing indexes.

Shapefile preparation
----------------------
Remove .qix file if present
If there are large DBF attributes that are not in use, get rid of them using ogr2ogr, e.g.::

	ogr2ogr -select FULLNAME,MTFCC arealm.shp tl_2010_08013_arealm.shp

If on Linux, enable memory mapping, faster, more scalable (beware that this option should not be enabled on Windows):

	.. figure:: img/shapefile.png
		:align: center

		*Shapefile DataStore options panel*

Shapefile filtering
--------------------
Using a Shapefile as data source is not a bad choice generally speaking. GeoServer has been optimized for serving large shapefile (GB size) with good speed (especially on Linux). The main limitation to take into account is that currently Shapefile (the Directory of Shapefiles DataStore in turn) do not offer alphanumeric indexing capabilities therefore if you have a schema to serve with many attributes and you want to be able to filter them finely for rendering or styling (i.e. being able to quickly extract small subset of rows depending purely on the non spatial attributes) then you need to move onto using a spatial DBMS. If you are stuck with shapefiles and have scale dependent rules like the following:

* Show highways first
* Show all streets when zoomed in

In order to avoid forcing GeoServer to load everything in memory to filter on alphanumeric attributes you can do the following:

1. Use ogr2ogr to build two shapefiles, one with just the highways, one with everything, and build two layers, e.g.::

	ogr2ogr -sql "SELECT * FROM tl_2010_08013_roads WHERE MTFCC in ('S1100', 'S1200')" primaryRoads.shp tl_2010_08013_roads.shp

2. Or hire us to develop non-spatial indexing for shapefile!

 .. note:: We highly recommend the second option.

PostgreSQL/PostGIS specific hints
-----------------------------------
PostgreSQL is out of the box configured for very small hardware as indicated `here <http://wiki.postgresql.org/wiki/Performance_Optimization>`_, therefore you might want to investigate on tweaking such configuration to get more speed out of your hardware, especially if you have a lot of memory available.

Here below you can find a list of other useful hints:

 * Make sure to run ANALYZE after data imports (updates optimizer stats)
 * As usual, avoid large joins in SQL views, consider materialized views
 * If the dataset is massive, CLUSTER on the spatial index as explained `here <http://postgis.refractions.net/documentation/manual-1.3/ch05.html>`_
 * Careful with prepared statements (bad performances)

About usage of prepared statements, which may seem counterintuitive, here below a longer explanation.

 * USE CASE: The layer’s style allows to display the whole layer in a single shot (no scale dependencies) -> prepared statements will slow down execution
 * EXPLANATION: PostGIS will choose to use the spatial index in all cases, this makes retrieving the full data set 2-4 times slower than when using a sequential scan
 * COUNTERMEASURE: Not using prepared statement allows PostGIS to figure out a suitable plan based on the request BBOX instead (assuming someone run "vacuum analyze" on the database to update the index statistics, and of course, provided there is a spatial index to start with)

Connection Pooling Tricks
----------------------------
As indicated above connection pools are a fundamental element in properly exploiting spatial DBMS from GeoServer.
Here below you can find some advices on how to best configure them:

 * Connection pool size should be proportional to the number of concurrent requests you want to serve (obvious, right?)
 * Activate connection validation, preferably in background although doing it in foreground is ok
 * Mind networking tools that might cut connections sitting idle (yes, your server is not always busy), they might cut the connection in “bad” ways (10 minutes timeout before the pool realizes the TCP connection attempt gives up)
 * Read more `here <http://geoserver.geo-solutions.it/edu/en/adv_gsconfig/db_pooling.html>`_ and `here <https://docs.google.com/document/d/1O02PeDRYKt2xLWG21k6BmcaRPzVRvXb4SQcrcZf4bHQ/edit>`_


Benchmarking Shapefile versus PostGIS
--------------------------------------
The following section compares vector data preparation using Shapefile and PostGIS. For this example a Shapefile containing primary or secondary roads is used.

The purpose is to test the throughput between the shapefile and an optimized database containing the same data. The result will demonstrate that database optimization can provide a better
throughput than the one of the shapefile


Configuring the database
---------------------------------

#. Open the terminal and change to training root directory: ``$TRAINING_ROOT`` on a linux machine resp. ``%TRAINING_ROOT%`` on a Windows machine

#. Load the shapefile ``tl_2014_01_prisecroads`` located in the corresponding user-data subfolder into PostGIS with the following commands:

	Linux (shapefile located in folder ``$TRAINING_ROOT/data/user_data``)::

		createdb -U postgres -T postgis20 shape2

		shp2pgsql -k -I "data/user_data/tl_2014_01_prisecroads/tl_2014_01_prisecroads.shp" public.pgroads | psql -U postgres -d shape2

	Windows (shapefile located in folder ``%TRAINING_ROOT%\data\user_data``)::

		setenv.bat

		createdb -U postgres -T postgis20 shape2

		shp2pgsql -k -I "data\user_data\tl_2014_01_prisecroads\tl_2014_01_prisecroads.shp" public.pgroads | psql -U postgres -d shape2

	.. note:: More information can be found at :ref:`Loading a Shapefile into PostGIS <geoserver.shp_postgis>`

#. Run PgAdmin3

    #. Linux: Run command **pgadmin3** in a terminal

    #. Windows machines: On the ``%TRAINING_ROOT%`` run command **pgAdmin.bat**

#. Go to the table ``pgroads`` inside database ``shape2`` and execute the following SQL script for creating an index on the *MTFCC* column:

	.. code-block:: sql

		CREATE INDEX mtfcc_idx ON pgroads ("MTFCC");

	.. figure:: img/jmeter46.png
		:align: center

		*Create a new index*

   The following index optimizes the access to the database when filtering on the *MTFCC* column.

Configuring GeoServer
---------------------------------

#. On your Web browser, navigate to the GeoServer `Welcome Page <http://localhost:8083/geoserver/>`_.

#. Following the instructions on :ref:`Adding a Postgis layer <geoserver.postgis_lay>`, configure the database ``shape2`` in GeoServer, publish the pgroads table and call it **pgroads**

	.. note:: Note that the database `Coordinate Reference System` is ``EPSG:4269``

#. Configure the shapefile ``tl_2014_01_prisecroads`` used before in GeoServer following the instructions in :ref:`Adding a Shapefile <geoserver.add_shp>`, publish a layer and call it **allroads**

    *  Linux: ``$TRAINING_ROOT/data/user_data/tl_2014_01_prisecroads/``
    *  Windows: ``%TRAINING_ROOT%\data\user_data\tl_2014_01_prisecroads\``

	.. note:: `Coordinate Reference System` of shapefile is ``EPSG:4269``

#. Go to :guilabel:`Styles` and click on ``Add new Style``

#. On the bottom of the page, click on :guilabel:`Choose File` and select the SLD file called ``shproads`` in the JMeter data directory:

    * Linux: ``$TRAINING_ROOT/data/jmeter_data``
    * Windows: ``%TRAINING_ROOT%\data\jmeter_data``

#. Click on :guilabel:`Upload` and then on :guilabel:`Submit`. This new style supports scale dependency which is used as filter on the roads to display.

    .. note:: Filter in SLD rule "Rule1" uses the attribute ``"MTFCC"`` for which an index has been created before.

Configuring JMeter
---------------------------------

#. Go to ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) and copy the file ``template.jmx`` file creating a ``vector.jmx`` file.

#. From the training root, on the command line, run ``jmeter.bat`` (or ``jmeter.sh`` if you're on Linux) to start JMeter

#. On the top left go to :guilabel:`File --> Open` and search for the new *jmx* file copied

#. In all the ``CSV Data Set Config`` elements, modify the **path** of the CSV file by setting the path for the file ``shp2pg.csv`` in the ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) directory

#. In the **HTTP Request Default** element modify the following parameters:

	.. list-table::
		  :widths: 30 50

		  * - **Name**
		    - **Value**
		  * - layers
		    - geosolutions:allroads
		  * - srs
		    - EPSG:4269
		  * - styles
		    - shproads

Test the Shapefile
------------------

#. Run the test. You should see something like this:

	.. figure:: img/jmeter47.png
		:align: center

		*Sample request on the Shapefile*

	.. note:: Remember to run and stop the test a few times for having stable results

#. When the test is completed, Save the results in a text file (e.g. with **Save Table Data** at the bottom of **Summary Report**)

#. Remove the results from JMeter by clicking on :guilabel:`Run --> Clear All` on the menu

Test the Database
------------------------

#. In the **HTTP Request Default** element modify the following parameter:

	.. list-table::
		  :widths: 30 50

		  * - **Name**
		    - **Value**
		  * - layers
		    - geosolutions:pgroads

#. Run the test again. It should be noted that **database throughput should be increased to those of the shapefile**, because the **created index provides a faster access on the database**, improving GeoServer performances.
