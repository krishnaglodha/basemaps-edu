.. module:: geoserver.jdbc_authentication

.. _geoserver.jdbc_authentication:


JDBC Authentication Provider
----------------------------
 
The JDBC Authentication Provider uses database users to authenticate. It basically tries to connect to a JDBC url, with the given credentials, authenticating the user if the connection can be estabilished.
A :ref:`user/group service <geoserver.users_roles>` is used to bind information to the user once it is authenticated.

To configure the provider you have to specify:
 * a name for the provider 
 * choose one of the available user/group service
 * the JDBC driver class name used to connect to the database (one of the drivers available in GeoServer installation)
 * the JDBC connection url for the database
   
We are going to configure a new JDBC Authentication provider, but first we need to create a new user/group service.
Look at :ref:`User/group service <geoserver.jdbcusergroup_services>` page to see how to create it.
When you have the service configured, go on and:
   
#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Click :guilabel:`Add new` in the :guilabel:`Authentication Providers` menu

#. Click :guilabel:`JDBC` in the :guilabel:`Authentication Providers` list and complete fields as follows:
 
	- Insert ``testdb`` in the ``Name`` text field.
	- Select ``jdbcservice`` from ``User/Group service`` combo box.	
	- Select ``org.postgresql.Driver`` from ``Driver class name`` combo box.	
	- Insert ``jdbc:postgresql://localhost:5434/postgis20`` in the ``Connection URL`` text field.

   .. figure:: img/jdbcprov1.png   

#. Click the :guilabel:`Save` button.

#. From the :guilabel:`Welcome` page click the :guilabel:`Authentication` link on the :guilabel:`Menu` Security section. 

#. Select the :guilabel:`testdb` element in the :guilabel:`Available` list of the :guilabel:`Provider Chain` menu

#. Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

#. Click the :guilabel:`Save` button.

Now, we have activated the new Authentication provider, having a new administrator user, named postgres. To verify it:

#. Click the :guilabel:`Logout` button on the top right part of the page.

#. Insert ``postgres`` in the ``Username`` and ``Password`` text fields on the top right part of the page.

#. Click the :guilabel:`Login` button on the top right part of the page.

You should be now logged in with the postgres user, with administrative rights.
