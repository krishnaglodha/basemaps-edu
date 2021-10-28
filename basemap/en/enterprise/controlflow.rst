.. _geoserver.jmeter_controlflow:

Tuning Control Flow Settings
==================================================
	.. hint:: You should read this section right after having read the :ref:`geoserver.controlflow` section.

The control flow extension can be used to prevent GeoServer from executing too many requests in parallel, which could lead to bad performances, by reducing the number of concurrent operations to execute and appending the others to a queue.

The control flow extension, if properly configured can:
 * improve GeoServer scalability and stability under heavy load reducing OOM errors and resource contentions (e.g. for DBMS connections)
 * Enforce fairness between users
 * Improve performance

Control flow provides granular control on the level parallelism is allowed for requests to be served by GeoServer. Limits can be set per service (WMS, WCS, etc.), per request (GetMap, GetFeatureInfo, etc.) and per user. Control flow also allows you to set a global timeout for the maximum time requests are allowed to stay in the request queue before being dropped.

A common mistake is to confuse the rendering timeout (as described in the :ref:`previous section<geoserver.jmeter_wmslimits>`) with control flow timeout. Control flow timeout keep track of the amount of time spent by each request in the request queue before being pulled from the queue and processed by GeoServer, rendering timeout accounts for the time spent by GeoServer to actually process the request. Resource limits and control flow need to work jointly to achieve optimal performance under heavy load.

In production environments spatial infrastructures usually comprise components other than GeoServer itself (for example a spatial DBMS, a Web Server acting as a proxy, load balancer etc.). In this scenario incoming requests will go through a whole chain of steps before actually being served to the client and we need to keep that in mind when tuning control flow limits. For example if the proxy timeout on the web server is set to a low value (lower than the sum of control flow timeout and rendering timeout), under heavy load client connections will be dropped by the proxy server before having the chance to be served by GeoServer even in the case of a request observing the limits set in resource limits and control flow settings.

The effect of a properly configured control flow (as well as resource limits) for GeoServer is, that once the throughput has reached its peak and right before resource thrashing kicks in GeoServer starts queueing and eventually timing out overwhelming requests as shown here below. In an healthy system throughput should keep constant once the maximum resource utilization has been reached.

   .. figure:: img/controlflow2.png
      :align: center

      *Decreased throughput (Note the results may be different in other machines)*

Control Flow and the surroundings
----------------------------------
If you are wondering what mechanism is used by GeoServer to implement control flow, you can refer to the picture below. A sophisticated queue based throttling framework has been implemented in order to decide which requests can execute, which are delayed and which will time out.

   .. figure:: img/controlflow.png
      :align: center

      *Decreased throughput (Note the results may be different in other machines)*

To make sure that control flow can do its work we need to make sure that Apache Httpd and/or Apache Tomcat (or whatever Web Proxy and Application Server you are using) are not acting as bottlenecks.
Before a request can actually hit GeoServer it has to go through the (eventual) proxy (Apache Httpd, Nginx, etc..) and then it has to be accepted by the Application Server that is running GeoServer (Apache Tomcat, JBoss, etc.). Let us focus on Apache Httpd for the moment; Although we usually recommend using Workers rather than Prefork, you'll have to make sure that the maximum number of requests that Apache Httpd will let pass is not smaller than the settings you have configured in control flow, otherwise Apache Httpd will become your bottleneck and you'll never reach full utilization with GeoServer. If Apache Httpd is acting as a load balancer in front of a cluster of GeoServer instances this is even more pressing.
As far as Apache Tomcat is concerned, its thread pool ideally should be set to a value which is not going to be smaller than the control flow values otherwise request will be queued at the Application Server level, waiting for a free thread, before control flow can do its work. Generally speaking we tend to play with the default thread pools values in Apache Tomcat to set them according to control flow.

	.. important:: Web Server settings for incoming requests as well as Application Server setting for multithreading needs to be configured with attention. If they are too small compared to the settings you choose for control flow they will act as bottleneck, if they are too big this is usually ok but it might be worth setting them to a value close the the values in control flow to have extra protection. **Ideally, requests should be queued as far as possible from the core processing chain inside GeoServer**, so better discarding them at the HTTP Proxy level than instantiating a rendering buffer and being stuch on waiting on a DBMS connection from a pool.

	.. hint:: Problems happens frequently with Apache Httpd when not using workers but prefork or with default Apache Tomcat threadpool configuration when there is a fast Tiling Server behind, so we are in the order of thousands of requests per second.

Along the same line, when you do set control flow always think about what bottlenecks you are hitting when you process a request. If all of your layers are served from a DBMS and you cannot use more than X connections, you'll hardly be able to process more than X WMS requests at a time (this is a simplication, we are assuming we keep a connection open for the entire time we process a request but we know it is not like that. Anyway we are just trying to give you some hints) along the same line, if you have 4 cores you will hardly be able to processe more than 8 or 16 requests in parallel, depending on how much time you spend in I/O versus real processor work.
Long story short, stress tests are a crucial tool to understand your limits and set control flow accordingly but, as usual, we should not forget about using our brain and knowledge to validate the results.

Configure JMeter for the test
-------------------------------

.. note:: This example requires to have already completed :ref:`Adding a ShapeFile <geoserver.add_shp>` and :ref:`Adding a Style <geoserver.add_style>` sections.


#. Go to ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) and copy the file ``template.jmx`` file into ``controlflow.jmx``

#. From the training root, on the command line, run ``jmeter.bat`` (or ``jmeter.sh`` if you're on Linux) to start JMeter

#. On the top left go to :guilabel:`File --> Open` and search for the new *jmx* file copied

#. Disable **View Results Tree** section

#. In the ``CSV Data Set Config`` element, modify the **path** of the CSV file by setting the path for the file ``controlflow.csv`` in the ``$TRAINING_ROOT/data/jmeter_data`` ( ``%TRAINING_ROOT%\data\jmeter_data`` on Windows ) directory

#. In the **HTTP Request Default** element modify the following parameters:

	.. list-table::
		  :widths: 30 50

		  * - **Name**
		    - **Value**
		  * - layers
		    - geosolutions:Mainrd
		  * - srs
		    - EPSG:2876


Test without Control Flow
-------------------------

#. Run the test

	.. note:: Remember to run and stop the test a few times for having stable results

#. When the test is completed, Save the results in a text file.

	You should notice that the throughput initially increases and then starts to decrease. This is associated to a bad scalability of the input requests. Remember which number of threads provides better throughput (it should be *8*). This value indicates the maximum number of concurrent requests that the server can execute simultaneously.

   .. figure:: img/jmeter34.png
      :align: center

      *Decreased throughput (Note the results may be different in other machines)*

#. Remove the result from JMeter by clicking on :guilabel:`Run --> Clear All` on the menu

#. Stop GeoServer

Configure Control Flow
----------------------
#. Go to ``$TRAINING_ROOT/data/plugins/not_installed`` ( ``%TRAINING_ROOT%\data\plugins\not_installed`` on Windows ) and copy ``geoserver-2.13.2-control-flow-plugin.zip`` zip file inside ``/opt/tomcat_geoserver/webapps/geoserver/WEB-INF/lib`` (``%TRAINING_ROOT%\tomcat-7.0.72\instances\instance1\webapps\geoserver\WEB-INF\lib`` if you are on Windows ) 

#. Unzip the content of ``geoserver-2.13.2-control-flow-plugin.zip`` inside the same folder

#. Go to ``$TRAINING_ROOT/geoserver_data`` ( or ``%TRAINING_ROOT%\geoserver_data`` on Windows ) and create a new file called ``controlflow.properties`` and add the following snippet

		.. code-block:: xml

			# don't allow more than 8 WMS GetMap in parallel
			ows.wms.getmap=8

		This code snippet indicates that no more than 8 *GetMap* request can be executed simultaneously by the WMS service. Other informations about the configuration can be found in the next section

		.. note:: If during your test you have found another number for the maximum throughput, you should set that value instead of 8

Test with Control Flow
----------------------

#. Restart GeoServer

#. Run again the test.

	You may see that the throughput is no more reduced after the control-flow configuration, because the input requests are scheduled by the control-flow plugin, improving GeoServer scalability.

   .. figure:: img/jmeter35.png
      :align: center

      *Stable throughput (Note the results may be different in other machines)*
