.. module:: geoserver.jdbcusergroup_services

.. _geoserver.jdbcusergroup_services:


JDBC Users and Group Services
---------------------------------

We will now see how to add a new service using a database as a storage medium:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section. 

   .. note:: You have to be logged in as Administrator in order to activate this function.
   
   .. figure:: img/usergroup1.png

#. Click the :guilabel:`Add new` in the :guilabel:`User Group Services` menu   

#. Click the :guilabel:`JDBC` link on top of the page   

    - Insert ``jdbcservice`` in the ``Name`` text field.
    - Select ``Weak PBE`` from ``Password encryption`` combo box.
    - Select ``default`` from ``Password policy`` combo box.
    - Select ``org.postgresql.Driver`` from ``Driver class name`` combo box.
    - Insert ``jdbc:postgresql://localhost/storm_track_sql`` in the ``Connection URL`` text field.
    - Insert ``geosolutions`` in the ``Username`` text field.
    - Insert ``Geos`` in the ``Password`` text field.
    - Check the ``Create database tables`` checkbox

    .. figure:: img/usergroup6.png   
 
   
#. Click the :guilabel:`Save` button. 

Now we are going to add a user to the newly added user/group service:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section

#. Click on the :guilabel:`User/Groups` tab
  
#. Click on the :guilabel:`jdbcservice` link and the user/groups form will appear

   .. figure:: img/usergroup7.png

#. Click on the :guilabel:`edit` link to the right of the :guilabel:`jdbcservice` link

#. Click on the :guilabel:`Users` tab

#. Click on the :guilabel:`Add new user` button

	- Insert ``postgres`` in the ``User name``, ``Password`` and ``Confirm Password`` text fields.
	- Select the :guilabel:`ADMIN` element in the :guilabel:`Available` list of the :guilabel:`Roles taken from active role service: default` menu
	- Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

   .. figure:: img/usergroup8.png
   
#. Click the :guilabel:`Save` button. 

We will use this service in the :ref:`JDBC Authentication <geoserver.jdbc_authentication>` section to create a new Authentication Provider.