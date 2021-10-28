.. module:: geoserver.roles_services

.. _geoserver.roles_services:


Managing Roles Services and Roles
---------------------------------

A role service provides the following information for roles:

 * List of roles
 * Calculation of role assignments for a given user
 * Mapping of a role to the system role ROLE_ADMINISTRATOR
 * Mapping of a role to the system role ROLE_GROUP_ADMIN
 
When a user/group service loads information about a user or a group, it delegates to the role service to determine which roles should be assigned to the user or group. 

A :guilabel:`default` service is available in standard GeoServer installation.

Additional services can be configured, if needed, but unlike User/group services, only one role service is active at any given time.

Two different type of service are available, one using xml files to store data about roles, another one using a database.

We will now add a role to the default service:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section.

      .. figure:: img/UsersGroupsRolesFullScreen.png
         :width: 550

      The Main page for managing Users, Group and Roles.

#. Add more Roles to **default** role service accessing to the roles page clicking on the role name, then click on the second tab called **Roles**.
	
#. Click on ``Add new`` button for add one more Role.

      .. figure:: img/role1.png
	  
	Insert ``testrole`` in the ``Name`` text field.

#. Click :guilabel:`Save` to create the new role.

The new role can be assigned to a user:

#. From the :guilabel:`Welcome` page click the :guilabel:`Users, Groups, Roles` link on the :guilabel:`Menu` Security section.

#. Click on the tab called **Users/Groups**.

#. Click on a username of your choice.

#. Select the :guilabel:`testrole` element in the :guilabel:`Available` list of the :guilabel:`Roles taken from active role service: default` menu

#. Click the :guilabel:`arrow right` button to add the element to the :guilabel:`Selected` list

      .. figure:: img/role2.png

#. Click :guilabel:`Save` to bind the new role to the user.

Now you can use the role to limit access to :ref:`Services <geoserver.service_level>` or :ref:`Layers <geoserver.layer_level>`.


