.. module:: geoserver.rest_security
   :synopsis: Learn how to use the GeoServer REST module.

GeoServer REST security configuration
-------------------------------------

.. note::
  
   RESTful security configuration is available in GeoServer versions greater than 2.0.1.

In addition to providing the ability to secure OWS style services GeoServer also allows for the securing of RESTful services.

As with layer and service security, RESTful security configuration is based on Users and roles. Mappings from request uri to role are defined in a file named rest.properties located in the security directory of the GeoServer data directory.

Syntax
^^^^^^

The following is the syntax for definiing access control rules for RESTful services (parameters in brackets [] are optional):

.. code-block:: xml

   uriPattern;method[,method,...]=role[,role,...]

where:

* **uriPattern** is the ant pattern that matches a set of request uri's.
* **method** is an HTTP request method, one of :guilabel:`GET`, :guilabel:`POST`, :guilabel:`PUT`, :guilabel:`POST`, :guilabel:`DELETE`, or :guilabel:`HEAD`
* **role** is the name of a predefined role. The wildcard '*' is used to indicate the permission is applied to all users, including anonymous users.

A few things to note:

* uri patterns should account for the first component of the rest path, usually :guilabel:`rest` or :guilabel:`api`
* method and role lists should **not** contain any spaces

Ant patterns
^^^^^^^^^^^^

Ant patterns are a commonly used syntax for pattern matching directory and file paths. The examples section contains some basic examples. The apache ant `user manual <http://ant.apache.org/manual/dirtasks.html>`_ contains more sophisticated cases.

Examples
^^^^^^^^

Most of the examples in this section are specific to the `rest configuration <http://docs.geoserver.org/stable/en/user/restconfig/index.html#rest-extension>`_ extension but any RESTful GeoServer service can be configured in the same manner.

Allowing only autenticated access to services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The most secure of configurations is one that forces any request to be authenticated. The following will lock down access to all requests:

.. code-block:: xml

   /**;GET,POST,PUT,DELETE=ROLE_ADMINISTRATOR

A slightly less restricting configuration locks down access to operations under the path :guilabel:`/rest`, but will allow anonymous access to requests that fall under other paths (for example :guilabel:`/api`):

.. code-block:: xml

   /rest/**;GET,POST,PUT,DELETE=ROLE_ADMINISTRATOR

The following configuration is like the previous except it grants access to a specific role rather than the administrator:

.. code-block:: xml

   /**;GET,POST,PUT,DELETE=ROLE_TRUSTED

Where :guilabel:`ROLE_TRUSTED` is a role defined in users.properties.

Providing anonymous read-only access
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following configuration allows anonymous access when the GET (read) method is used but forces authentication for a :guilabel:`POST`, :guilabel:`PUT`, or :guilabel:`DELETE` (write):

.. code-block:: xml

   /**;GET=IS_AUTHENTICATED_ANONYMOUSLY
   /**;POST,PUT,DELETE=TRUSTED_ROLE

Securing a specific resource
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following configuration forces authentication for access to a particular resource (in this case a feature type):

.. code-block:: xml

   /rest/**/states*;GET=TRUSTED_ROLE
   /rest/**;POST,PUT,DELETE=TRUSTED_ROLE

The following secures access to a set of resources (in this case all data stores):

.. code-block:: xml

   /rest/**/datastores/*;GET=TRUSTED_ROLE
   /rest/**/datastores/*.*;GET=TRUSTED_ROLE
   /rest/**;POST,PUT,DELETE=TRUSTED_ROLE

Exercise
^^^^^^^^

By default, on the workshop configuration, the REST methods have been protected to all.

Opening with a text editor the file:

.. code-block:: xml

   ${TRAINING_ROOT}/geoserver_data/security/rest.properties

you may notice how the methods are allowed only to the :guilabel:`ADMINISTRATOR`.

   .. figure:: img/rest_security1.png
      :width: 600
 
      Default Workshop REST security configuration

#. Open up a new browser window and open the GeoServer home page. Make sure you're logged out, so that the browser cannot recognize the administrator credentials:

   .. figure:: img/logout.png

      Logging out GeoServer

   .. figure:: img/loggedout.png

      Logged out of GeoServer


#. Browse to the following **URL**:

   .. code-block:: xml

     http://localhost:8083/geoserver/rest

   GeoServer will ask you for credentials.
   
   .. figure:: img/rest_security2.png
      :width: 600
   
   Providing the *administrator* credentials will allow you to access the *HTML* output of the REST interface.

#. Now we are going to modify the REST security in order to allow any user to access the layers configuration, but deny the access to the datastores configuration.
   In order to do this, edit the file

   .. code-block:: xml

     ${TRAINING_ROOT}/geoserver_data/security/rest.properties

   and modify the properties like below:

   .. code-block:: xml

     /rest/**/datastores/*;GET=ROLE_ADMINISTRATOR
     /rest/**/datastores/*.*;GET=ROLE_ADMINISTRATOR
     /rest/**/coveragestores/*;GET=ROLE_ADMINISTRATOR
     /rest/**/coveragestores/*.*;GET=ROLE_ADMINISTRATOR
     /**;POST,PUT,DELETE=ROLE_ADMINISTRATOR

   .. note:: You don't need to restart GeoServer. The security options will be applied at runtime.

#. Save, **open up a new browser window** and go to

   .. code-block:: xml

     http://localhost:8083/geoserver/rest

   GeoServer does not complain anymore for credentials, and will show you the main REST objects.

   .. figure:: img/rest_security3.png
      :width: 600

#. Navigate to :guilabel:`/rest/workspaces/geosolutions`. You should be able to see the list of all the available :guilabel:`datastores` and :guilabel:`coveragestores`.

   .. figure:: img/rest_security4.png
      :width: 400

#. If you now try to click on one of those, GeoServer will ask you again for credentials.
   Here below the **new** full :guilabel:`rest.properties` file.

   .. figure:: img/rest_security5.png
      :width: 600

   .. note::

     The rules must be written taking into account that the security sub-system works in **Allow All** mode, i.e. you need to write **only** the rules for the resources you want to protect.

#. Notice that with the previous configuration, anonymous users are still able to access the resources.
   If you navigate to :guilabel:`/rest/layers/Mainrd.xml` for instance, you are able to see the link to the layer resource. 

   .. figure:: img/rest_security6.png
      :width: 600

   with the current configuration you are still able to retrieve the resource without authentication.

   .. code-block:: xml

     localhost:8083/geoserver/rest/workspaces/geosolutions/datastores/Mainrd/featuretypes/Mainrd.xml

   GeoServer does not complain for credentials, and will show you the resource.

#. In order to protect the resources, edit the file

   .. code-block:: xml

     ${TRAINING_ROOT}/geoserver_data/security/rest.properties

   and **add** the properties like below:

   .. code-block:: xml

     /rest/**/featuretypes/*;GET=ROLE_ADMINISTRATOR
     /rest/**/featuretypes/*.*;GET=ROLE_ADMINISTRATOR
     /rest/**/coverages/*;GET=ROLE_ADMINISTRATOR
     /rest/**/coverages/*.*;GET=ROLE_ADMINISTRATOR

#. Save and try to access the resources again. You will now notice that GeoServer will ask for credentials.
   Here below the **new** full :guilabel:`rest.properties` file.

   .. figure:: img/rest_security7.png
      :width: 600
