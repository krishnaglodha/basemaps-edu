.. module:: geoserver.usergroup_services

.. _geoserver.usergroup_services:


Managing Users and Group Services
---------------------------------

Users and groups lists are managed by user/group services.

A :guilabel:`default` service is available in standard GeoServer installation.

Additional services can be configured, if needed.

Two different type of service are available, one using xml files to store data about users and groups, another one using a database.

.. toctree::
   :maxdepth: 1 
   
   XML User/group services <xmlusergroup_services>
   JDBC User/group services <jdbcusergroup_services>   
