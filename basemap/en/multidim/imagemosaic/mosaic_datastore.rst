.. module:: geoserver.mosaic_datastore
   :synopsis: ImageMosaic DataStore Support.

.. _geoserver.mosaic_datastore:

Using a DBMS for the ImageMosaic index
==========================================

The ImageMosaic plugin for GeoServer allows users to place the index for the granules into a spatial DBMS as explained below. This is especially important when one or more of the following conditions apply:

* The granules depicts a phenomenon that depends on multiple dimensions (e.g. Time, Elevation and so on).
* The granules will be added/remove/update frequently at runtime, hence a full-blown transactional DBMS would be a better choice for the index.

Datastore.properties Keys Description
--------------------------------------

The *datastore.properties* is a properties file which is used by the ImageMosaic plugin in GeoServer in order to place the granule index in a spatial DBMS. Currently the supported ones are as follows:

* **Postgis** 
* **Oracle**
* **H2**
* **SQLServer**

Internally the ImageMosaic uses the key-value pairs contained in this property file to create a GeoTools DataStore (using the provided parameters) therefore you might notice a parallel between the names of the parameters we will discuss below and those you can configure when creating vector DataStore through the GeoServer user interface, since the underlying machinery is the same.

All the keys that can be configured are explained here below. Generally speaking, they depend on the DBMS you are connecting to hence we suggest to check the GeoServer documentation for the relative DataStore for more information:

* **dbtype** : database type.
* **host** : host name.
* **port** : port number.
* **database** : database name.
* **schema** : used schema.
* **user** : user name to login.
* **passwd** : password used to login.
* **namespace** : namespace prefix.
* **Data\\ Source** : data source used.
* **max\\ connections** : maximum number of open connections. (Default 10)
* **min\\ connections** : minimum number of pooled connection. (Default 1)
* **validate\\ connections** : boolean used for checking if the  connection is alive before using it. (Default Boolean.FALSE)
* **fetch\\ size** : number of records read with each iteraction with the dbms.
* **Connection\\ timeout** : number of **seconds** the connection pool will wait before timing out attempting to get a new connection. (Default 20 seconds)
* **Primary\ key\\ metadata\\ table** : optional table containing primary key structure and sequence associations. Can be expressed as 'schema.name' or just 'name'.
* **Max\\ open\\ prepared\\ statements** : maximum number of prepared statements kept open and cached for each connection in the pool. Set to 0 to have unbounded caching, to -1 to disable caching. (Default 50)
* **Expose\\ primary\\ keys** : expose primary key columns as attributes of the feature type. (Default Boolean.FALSE)
* **create\\ database\\ params=WITH\\ TEMPLATE\\** : optional parameter used for setting the proper PostGis template for Postgres version lower than 9.2.3.
* **SPI** : dataStoreFactory used. Note the ‘SPI’ key can substitute the ‘dbtype’.
* **Loose\\ bbox** : boolean indicating if the datastore performs only primary filter on bbox. (Default Boolean.TRUE)
* **Estimated\\ extends** : use the spatial index information to quickly get an estimate of the data bounds. (Default Boolean.TRUE)
* **preparedStatements** : use prepared statements. (Default Boolean.FALSE)
* **jndiReferenceName** : used for referencing a preconfigured JNDI connection pool, without having to set the database connection parameters. 

datastore.properties Examples
--------------------------------------

Below a few examples of *datastore.properties* file are shown for different database types. All the 4 examples create a database with name
**dbname**, username **usr** and password **psw**.

.. note:: For each type of database you want to use, you shall add to the `WEB-INF/lib` directory of your Geoserver installation the related jars, which can be found `here <https://build.geoserver.org/geoserver/2.18.x/ext-latest/>`_. This is especially true for Oracle support as ths DBMS is not supported by default in GeoServer.

Plain PostGIS Example
++++++++++++++++++++++++++++

.. code-block:: xml

   SPI=org.geotools.data.postgis.PostgisNGDataStoreFactory
   host=localhost
   port=5434
   database=dbname
   schema=public
   user=usr
   passwd=psw
   Loose\ bbox=true
   Estimated\ extends=false
   validate\ connections=true
   Connection\ timeout=10
   preparedStatements=true
      
This snippet contains the definition of the **host** and the **port** of a PostGIS instance on where the database will be stored.
*Loose\\ bbox=true*, *Estimated\\ extends=false* and *preparedStatements=true* are configured for achieving better performances.

.. note:: For a PostGIS datastore, it is not needed that the database is already present in the PostGIS instance; it will be created automatically by the ImageMosaic reader.  

Plain Oracle Example
++++++++++++++++++++++++

.. code-block:: xml

   SPI=org.geotools.data.oracle.OracleNGDataStoreFactory
   port=1521
   host=localhost
   database=dbname
   Loose\ bbox=true
   Estimated\ extends=false
   user=usr
   passwd=psw
   validate \connections=true
   Connection\ timeout=10
   
This snippet contains the definition of the **host** and the **port** of an Oracle instance on where the database will be stored.   

.. warning:: When using an Oracle database to host the index it is necessary to create the database before serving the mosaic because it is not automatically created by the underlying code. This is because database creation is not supported in a standard way through JDBC drivers, and we have a custom solution working for PostGIS only at the time being.
   
Plain H2 Example
++++++++++++++++++
   
.. code-block:: xml
   
   SPI=org.geotools.data.h2.H2DataStoreFactory
   type=javax.sql.DataSource
   driver=org.h2.Driver
   database=dbname
   user=usr
   password=psw

In this snippet **host** and **port** parameters are not defined because we are going to create and embedded database. This database can be accessed directly with an URL like this::

   jdbc:h2://path/to/the/db/directory

Plain SQLServer Example
++++++++++++++++++++++++

.. code-block:: xml

   SPI=org.geotools.data.sqlserver.SQLServerDataStoreFactory
   port=1433
   host=localhost
   database=dbname
   schema=dbo
   Loose\ bbox=true
   Estimated\ extends=false
   user=usr
   passwd=psw
   validate \connections=true
   Connection\ timeout=10
   Geometry\ metadata\ table= GEOMETRY_COLUMNS
   
This snippet contains the definition of the **host** and the **port** of a SQL Server instance on where the database will be stored. A ***Geometry metadata table property*** has also been defined, to allow the SQL Server datastore to determine the geometry type and the native SRID of the geometry column. For more information see  the `GeoServer documentation <https://docs.geoserver.org/stable/en/user/data/database/sqlserver.html#using-the-geometry-metadata-table>`_

.. warning:: As per the Oracle case, when using a SQLServer database to host the index it is necessary to create the database before serving the mosaic because it is not automatically created by the underlying code.


Using a JNDI Connection Pool 
--------------------------------------
`Java Naming and Directory Interface <http://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface>`_ (JNDI) is a Java API used 
for accessing resources via a single name mapped as a key. 

A common way of using JNDI is the configuration of a JDBC connection pool, 
which is a global container of database connections. This container is typically used for reducing the database resources 
usage by sharing the same connection pool between multiple objects accessing the same database. In fact, by maximizing the sharing database connections we minimize the need of creating new ones, 
which are resource-expensive objects, each time they are requested.

It is worth to point out that GeoServer when creating vector DataStores without JNDI implicitly creates an embedded pool reducing the possibility of sharing connections between multiple stores.

For configuring a JDBC connection pool with Tomcat, you have to edit the :file:`context.xml` file inside the 
:file:`$TOMCAT_HOME/conf` directory (:file:`$TOMCAT_HOME` is the directory where Tomcat is installed) defining the
parameters associated to the new database connections. 

These parameters are:

* **name** : The name of the JNDI object.
* **driverClassName** : the name of the JDBC driver used.
* **url** : URL for connecting to the JDBC database.
* **username** : username for accessing the database.
* **password** : password for accessing the database.
* **maxActive** : The number of maximum active connections to use.
* **maxIdle** : The number of maximum unused connections.
* **maxWait** : The maximum number of **milliseconds** that the pool will wait.

Optionally you can set also other parameters like:

* **poolPreparedStatements** : Enable the prepared statement pooling (very important for good performance).
* **maxOpenPreparedStatements** : The maximum number of prepared statements in pool.
* **validationQuery** : (default null) A validation query that double checks the connection is still alive before actually using it.
* **timeBetweenEvictionRunsMillis** : (default -1) The number of **milliseconds** to sleep between runs of the idle object evictor thread. When non-positive, no idle object evictor thread will be run.
* **numTestsPerEvictionRun** : (default 3) The number of objects to examine during each run of the idle object evictor thread (if any).
* **minEvictableIdleTimeMillis** : (default 1000 * 60 * 30) The minimum amount of time in **milliseconds** an object may sit idle in the pool before it is eligible for eviction by the idle object evictor (if any).
* **removeAbandoned** : (default false) Flag to remove abandoned connections if they exceed the removeAbandonedTimeout. If set to true a connection is considered abandoned and eligible for removal if it has been idle longer than the removeAbandonedTimeout. Setting this to true can recover db connections from poorly written applications which fail to close a connection.
* **removeAbandonedTimeout** : (default 300) Timeout in seconds before an abandoned connection can be removed.
* **logAbandoned** : (default false) Flag to log stack traces for application code which abandoned a Statement or Connection.
* **testWhileIdle** : (default false) Flag used to test connections when idle.
   
.. warning:: The previous settings should be modified only by experienced users. Using wrong low values for **removedAbandonedTimeout** and **minEvictableIdleTimeMillis** may result in connection failures; if so try it is important to set-up **logAbandoned** to *true* and check your *catalina.out* log file.

These 3 examples below configure a connection pool to a local database called **dbname**,  with username **usr** and password **psw**. Also these 3
configurations have the same connection parameters:

	* maxActive="20" 
	* maxIdle="10" 
	* maxWait="10000"
	* minEvictableIdleTimeMillis="300000"
	* timeBetweenEvictionRunsMillis="300000"
	* validationQuery=<check note below>
	
.. note:: validationQuery may change between the DataBases.

The  ``datastore.properties`` will then reference the connection pool by name using the **jndiReferenceName** parameter,
and also configure all the store parameters that do not deal with the pool creation itself, including:

* **dbtype** : database type.
* **SPI** : dataStoreFactory used. Note the ‘SPI’ key can substitute the ‘dbtype’.
* **schema** : used Schema.
* **namespace** : namespace prefix.
* **Expose\\ primary\\ keys** : expose primary key columns as attributes of the feature type. (Default Boolean.FALSE)
* **Loose\\ bbox** : boolean indicating if the datastore performs only primary filter on bbox. (Default Boolean.TRUE)
* **Estimated\\ extends** : use the spatial index information to quickly get an estimate of the data bounds. (Default Boolean.TRUE)
* **preparedStatements** : use prepared statements. (Default Boolean.FALSE)

The list might contain other parameters depending on the specific store being configured. 
It is advisable to check the documentation about it.

JNDI PostGIS Example
+++++++++++++++++++++

For setting up a PostgreSQL JNDI pool you have to remove the Postgres JDBC driver (it should be named :file:`postgresql-X.X-XXX.jar`) from the GeoServer `WEB-INF/lib` folder and put it into the `$TOMCAT_HOME/lib` folder.

A :file:`context.xml` example file for PostGIS could be:

.. code-block:: xml

     <Context>
      <Resource
       name="jdbc/postgres"
       auth="Container"
       type="javax.sql.DataSource"
       driverClassName="org.postgresql.Driver"
       url="jdbc:postgresql://localhost/dbname"
       username="usr"
       password="psw"
       maxActive="20" 
       maxIdle="10" 
       maxWait="10000"
       minEvictableIdleTimeMillis="300000"
       timeBetweenEvictionRunsMillis="300000"
       validationQuery="SELECT 1"/>
     </Context>

and here is the relative *datastore.properties* file.

.. code-block:: xml

	# JNDI specific #
	#dbtype=
	SPI=org.geotools.data.postgis.PostgisNGJNDIDataStoreFactory
	#String
	# JNDI data source
	# Default "java:comp/env/"+"jdbc/mydatabase"
	jndiReferenceName=postgres

	#Boolean
	# perform only primary filter on bbox
	# Default Boolean.TRUE
	Loose\ bbox=true

	#Boolean
	# use prepared statements
	#Default Boolean.FALSE
	preparedStatements=false
	 
JNDI Oracle Example
++++++++++++++++++++++++

For setting up an Oracle JNDI pool you have to remove the Oracle JDBC jar :file:`ojdbcX-XX.XX.jar` file from the :file:`WEB-INF/lib` folder and put it inside the :file:`$TOMCAT_HOME/lib` folder.

A :file:`context.xml` example file could be:

.. code-block:: xml

     <Context>
       <Resource
        name="jdbc/oralocal"
        auth="Container" 
        type="javax.sql.DataSource"
        url="jdbc:oracle:thin:@localhost:1521:dbname"
        driverClassName="oracle.jdbc.driver.OracleDriver"
        username="usr" 
        password="psw"
        maxActive="20" 
        maxIdle="10" 
        maxWait="10000"
        minEvictableIdleTimeMillis="300000"
        timeBetweenEvictionRunsMillis="300000"
        poolPreparedStatements="true"
        maxOpenPreparedStatements="100"
        validationQuery="SELECT SYSDATE FROM DUAL" />
     </Context>

JNDI H2 Example
++++++++++++++++++++++++

For setting up an H2 JNDI pool you have to remove the H2 JDBC jar file :file:`h2-XXXX.jar` from the :file:`WEB-INF/lib` folder and put it inside the :file:`$TOMCAT_HOME/lib` folder.

A :file:`context.xml` example file could be:

.. code-block:: xml

     <Context>
      <Resource 
         name="jdbc/h2" 
         auth="Container"
         type="javax.sql.DataSource"
         driverClassName="org.h2.Driver"
         url="jdbc:h2:mem:dbname"
         username="usr" 
         password="psw"
         maxActive="20" 
         maxIdle="10" 
         maxWait="10000"
         minEvictableIdleTimeMillis="300000"
         timeBetweenEvictionRunsMillis="300000"
         validationQuery="SELECT 1"/>
     </Context>

JNDI SQLServer Example
++++++++++++++++++++++++

For setting up a SQLServer JNDI pool you have to remove the SqlServer JDBC jar file :file:`mssql-jdbc-X.X.X.jar` from the :file:`WEB-INF/lib` folder and put it inside the :file:`$TOMCAT_HOME/lib` folder.

:file:`context.xml`:

.. code-block:: xml

     <Context>
      <Resource name="jdbc/sqlserver"
      auth="Container"
      type="javax.sql.DataSource"
      driverClassName="com.microsoft.sqlserver.jdbc.SQLServerDriver"
      url="jdbc:sqlserver://localhost:1433;databaseName=test;user=admin;password=admin;"
      username="admin" password="admin"
      maxActive="20"
      initialSize="0"
      minIdle="0"
      maxIdle="8"
      maxWait="10000"
      timeBetweenEvictionRunsMillis="30000"
      minEvictableIdleTimeMillis="60000"
      testWhileIdle="true"
      poolPreparedStatements="true"
      maxOpenPreparedStatements="100"
      validationQuery="SELECT 1"
      maxAge="600000"
      rollbackOnReturn="true"
      />
     </Context>
