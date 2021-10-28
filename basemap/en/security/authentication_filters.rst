.. module:: geoserver.authentication_filters

.. _geoserver.authentication_filters:

Authentication Filters
----------------------
Authentication filters perform a variety of tasks, including:
 * gathering user credentials from a request, for example from Basic and Digest Authentication headers
 * handling events such as ending the session (logging out), or setting the “Remember Me” browser cookie
 * performing session integration, detecting existing sessions and creating new sessions if necessary
 * invoking the authentication provider chain to perform actual authentication

Available filters are:
 * J2EE - Delegates to servlet container for authentication
 * Anonymous - Authenticates anonymously performing no actual authentication
 * Remember Me - Authenticates by recognizing authentication from a previous request
 * Form - Authenticates by processing username/password from a form submission
 * X.509 - Authenticates by extracting the common name (cn) of a X.509 certificate
 * HTTP Header - Authenticates by checking the existence of an HTTP request header
 * Basic - Authenticates using HTTP basic authentication
 * Digest - Authenticates using HTTP digest authentication

Some filter chains are available in order to configure which filters are to be used in a different context.

Available chains are:
 * web
 * webLogin
 * webLogout
 * rest
 * gwc
 * default

Many filters can be active for a particular chain at the same time, just add them to the filter chain.

Now we will modify the Default authentication filter to disable anonymous access for OWS services:

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section.

#. Click the :guilabel:`default` link from :guilabel:`Filter Chains` list.

#. Select the :guilabel:`anonymous` element in the :guilabel:`Selected` list of the :guilabel:`Chain filters` menu

   .. figure:: img/filter1.png

#. Click the :guilabel:`arrow left` button to add the element to the :guilabel:`Available` list

#. Click the :guilabel:`Close` button.

#. Click the :guilabel:`Save` button.

Now we are going to verify that the anonymous user is not allowed to launch OWS requests:

#. From the :guilabel:`Welcome` page click the :guilabel:`Demos` link on the :guilabel:`Menu`.

   .. note:: You have to be logged out from Geoserver admin console.

#. Click the :guilabel:`Demo requests` link

#. Select :guilabel:`WMS_getMap.url` from :guilabel:`Request` combo box.

#. Insert::

    http://localhost:8083/geoserver/wms?service=WMS&version=1.1.0&request=GetMap&layers=geosolutions:Counties&styles=&bbox=-109.060256,36.992424,-102.040878,41.003443999999995&width=577&height=330&srs=EPSG:4269&format=image/png

in the :guilabel:`URL` text field

#. Click the :guilabel:`Submit` button.

   .. figure:: img/filter2.png

You should get an error like: HTTP response: 401. Now let's try with an authenticated user:

#. Insert :guilabel:`admin` in the Username text field.

#. Insert :guilabel:`Geos` in the Password text field.

#. Click the :guilabel:`Submit` button.

You should get a map.
