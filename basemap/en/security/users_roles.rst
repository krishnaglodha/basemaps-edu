.. module:: geoserver.users_roles

.. _geoserver.users_roles:


Managing Users and Roles
------------------------

Security in GeoServer is a role-based system. Roles are created to serve particular functions (examples: access WFS, administer UI, read certain layers), and users are linked to those roles.
Since GeoServer 2.2 the concept of **group** has been introduced; a group is basically a set of users.

Users and groups are managed by user/group services. A :guilabel:`default` service is available in standard GeoServer installation, that uses xml files in the GeoServer data dir to store data.

Roles are managed by role services. A :guilabel:`default` service is available in standard GeoServer installation, that uses xml files in the GeoServer data dir to store data.

Alternative services can be created if needed to:
 * store user data in a database or different source
 * bind different sets of users/groups to different :ref:`authentication <geoserver.authentication>` providers
 
To have more detail:

.. toctree::
   :maxdepth: 1 
   
   User/group services <usergroup_services>
   Roles services <roles_services>

We will now see how to add a new user to the default user service:
 
#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section.

      .. figure:: img/UsersGroupsRolesFullScreen.png
         :width: 550

      The Main page for managing Users, Group and Roles.

As you can see there are two sections: the first to handle Users and Groups and the second to handle Roles. By default there is only one service, called **default**. It is possible to add more clicking on the ``Add new`` button.  

#. Add more Users to **default** service (or any other) accessing to the service page clicking on the service name, then click on the second tab called **Users**.
    
      .. figure:: img/UsersGroupsRolesFullScreen.png
         :width: 550

      The Main page for managing Users and Group

#. From the users manager menu click ``Add new user`` and enter the following user configuration:

    - :guilabel:`user=wsuser`
    - :guilabel:`password=wsuser` 
    - Create a new role clicking ``Add a new role`` button, entering :guilabel:`ROLE_WS` in the ``Name`` field and clicking ``Save`` button.
    - Assign the user the new role selecting it in the ``Available`` list and moving it to the ``Selected`` list using the right arrow button.

      .. figure:: img/users2.png
         :width: 650

      The users list  

#. Click :guilabel:`Save` to create the new user.

.. note:: Linking users and roles is done via the file :guilabel:`users.properties` (located in $geoserver_data/security directory). By default, this file contains one line: :guilabel:`admin=geoserver,ROLE_ADMINISTRATOR` (user=admin and password=geoserver).
          The ROLE_ADMINISTRATOR is the predefined role and provides full access to all systems inside GeoServer. If you are using GeoServer in a production environment, the password (and possibly the user name as well) must be immediately changed.
          In the case of the Educational Training package, admin password was already changed to value Geos.
