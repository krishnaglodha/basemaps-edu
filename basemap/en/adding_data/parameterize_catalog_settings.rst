.. module:: geoserver.gs_data_dir
   :synopsis: Learn how to managing GeoServer data directory.

.. _geoserver.gs_data_dir:

Parametere the Catalog Settings
=================================

This section explains how to parameterize some of the settings in the GeoServer's catalog.

What we mean with that is that you are able to use a templating mechanism to tailor GeoServer’s settings to the environment in which is run.

For example, you’d want to move the latest changes you’ve made from a GeoServer instance running on machine A to another instance running on machine B but there are some differences in the way the two environments are set up, let’s say the password used to connect to the database is different.

If you simply create a backup of the catalog from instance A and restore it on instance B, the stores configured on the database will not be accessible and the corresponding layers will not work properly.

Another example would be the max number of connections available in the connection pool to the database. You might want to have a different poll configuration in the two environments (maybe GeoServer on machine A is a test instance an GeoServer on machine B is used in production).

To overcome such limitation (to have the environment set up in the exact same way on the source machine and on the destination machine) the ENV parametrization allows you to customize the catalog configuration via the use of a templating system.

First of all to be able to use this feature, set the following flag via system variable to GeoServer’s environment:

   -DALLOW_ENV_PARAMETRIZATION=true

Then create a file called **geoserver-environment.properties** in the root of GeoServer’s datadir. This file will contain the definitions for the variables parameterized in the catalog configuration.   


**Example**

We want to parameterize the URL of a Shapefile store.

#. Add a definition for the variable **store_url** in **geoserver-environment.properties**

   .. code-block:: bash

      vim geoserver-environment.properties
   
   .. code-block:: bash   

      (Linux)
      user_data=$TRAINING_ROOT/data/user_data
      or
      (Windows)
      user_data=%TRAINING_ROOT%\\data\\user_data


#. Use a placeholder with syntax **${** <key> **}** to configure the store in parametric way:   

   .. code-block:: bash 
   
      vim $TRAINING_ROOT/workspaces/geosolutions/Mainrd/datastore.xml

   .. code-block:: XML

      ...
            <connectionParameters>
               <entry key="url">file://${user_data}\Mainrd.shp</entry>
            </connectionParameters>
         </coverageStore>
      </dataStore>

#. Restart GeoServer

#. Navigate to the store configuration

   .. figure:: img/param-cat-settings-0.png

#. Check if it works with the layer preview   

   .. figure:: img/param-cat-settings-1.png