.. module:: geoserver.authentication

.. _geoserver.authentication:


Authentication
--------------

Authentication in GeoServer is composed of:
 * Authentication Filters: they configure how the user credentials are retrieved by GeoServer
 * Filter Chains: they configure which Authentication Filters are used
 * Authentication Provider: they configure how the user credentials are verified by GeoServer
 * Provider Chain: it configures which Authentication Providers are used

.. toctree::
   :maxdepth: 1  

   Authentication Filters <authentication_filters>
   Authentication Providers <authentication_providers>
  
