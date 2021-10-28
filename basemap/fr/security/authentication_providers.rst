.. module:: geoserver.authentication_providers

.. _geoserver.authentication_providers:
 
Authentication Providers
------------------------   
Authentication Providers are pluggable components that are able to check that a given user is trusted to login to the system, and which groups/roles are assigned to it.
Some providers are available in the standard GeoServer installation. Others can be developed as extensions.
The default one is Basic username/password authentication, that uses a user/group service for authentication.
   
   .. figure:: img/authproviders1.png
   
Providers can use a :ref:`user/group service <geoserver.users_roles>` to bind information to the authenticated users.
Many providers can be active at the same time, just add them to the unique provider chain.

.. toctree::
   :maxdepth: 1 
   
   Basic Authentication Provider <basic_authentication>
   JDBC Authentication Provider <jdbc_authentication>
   LDAP Authentication Provider <ldap_authentication>   
