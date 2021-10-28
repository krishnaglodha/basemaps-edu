.. installation:

Installing the Monitoring Extension
===================================

Monitoring is an official extension, as such it can be found alongside any GeoServer release. The extension is split into two modules, "core" and "hibernate", where core provides the basic underpinnings of the module and allows to monitor "live" requests, while the hibernate extension provides database storage of the requests.

#. Stop Geoserver: 
    * In Linux ``sudo service geoserver stop``
    * In Windows, execute ``tomcat_stop_1.bat``

#. Get the monitoring zip files (already downloaded for you) from the training material ``data\plugins`` folder.

#. Extract the contents of the archives into the ``/opt/tomcat_geoserver/webapps/geoserver/WEB-INF/lib`` in Linux (``%TRAINING_ROOT%/tomcat/instances/instance1/webapps/geoserver/WEB-INF/lib`` in Windows) directory of the GeoServer installation.

Verifying the Installation
---------------------------

There are two ways to verify that the monitoring extension has been properly installed.

* Start GeoServer and open the `Web Administration interface <http://localhost:8083/geoserver>`_.  Log in using the administration account.  If successfully installed, there will be a :guilabel:`Monitor` section on the left column of the home page.

  .. figure:: img/monitorwebadmin.png
     :align: center

     *Monitoring section in the web admin interface*

* Start GeoServer and navigate to the current data directory.  If successfully installed, a new directory named ``monitoring`` will be created in the data directory.
